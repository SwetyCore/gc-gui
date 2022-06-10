<template>
  <div>
    <v-card flat outlined>
      <v-card-title>状态</v-card-title>
      <v-card-text>
        <v-row>
          <v-col>
            <v-list nav dense>
              <v-list-item>
                <v-list-item-title style="font-size: 20px">
                  服务器版本:<v-chip style="margin: 0px 5px" small color="primary">{{
                    serverStats.status.version
                  }}</v-chip>
                  <v-chip style="margin: 0px 5px" small color="primary"
                    >{{ gcData.tag }}[{{ gcData.hash }}]</v-chip
                  >
                  <v-chip style="margin: 0px 5px" small color="green" dark
                    >有新版本可用</v-chip
                  >
                </v-list-item-title>
              </v-list-item>
              <v-list-item>
                <v-list-item-title style="font-size: 20px">
                  服务器状态:<v-chip
                    style="margin: 0px 5px"
                    small
                    dark
                    :color="serverStats.retcode == 0 ? 'green' : 'red'"
                    >{{ serverStats.retcode == 0 ? "正在运行" : "已停止" }}</v-chip
                  >
                </v-list-item-title> </v-list-item
              ><v-list-item>
                <v-list-item-title style="font-size: 20px">
                  在线人数:<v-chip style="margin: 0px 5px" small color="primary">{{
                    serverStats.status.playerCount
                  }}</v-chip>
                </v-list-item-title>
              </v-list-item>
            </v-list>
          </v-col>
          <v-divider vertical></v-divider>

          <v-col>
            <v-progress-circular
              style="margin: 5px"
              :rotate="-90"
              :size="140"
              :width="20"
              :value="cpuLoad"
              color="primary"
            >
              CPU {{ cpuLoad }} %
            </v-progress-circular>
            <v-progress-circular
              style="margin: 5px"
              :rotate="-90"
              :size="140"
              :width="20"
              :value="cpuLoad"
              color="primary"
            >
              RAM {{ cpuLoad }} %
            </v-progress-circular>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-btn color="primary" text @click="startServer">{{
          serverStats.retcode == 0 ? "关闭服务器" : "开启服务器"
        }}</v-btn>
        <v-btn color="primary" text>切换服务器版本</v-btn>
      </v-card-actions>
    </v-card>
    <v-card flat>
      <v-card-title><v-icon>mdi-github</v-icon> 最新提交</v-card-title>
      <v-card-text style="height: 50vh; overflow: auto">
        <template>
          <v-row justify="center" style="margin: 10px">
            <v-expansion-panels accordion flat>
              <v-expansion-panel v-for="(commit, i) in commits" :key="i">
                <v-expansion-panel-header>
                  <div>
                    <v-chip small label :href="commit.html_url" text target="_blank">
                      {{ commit.sha.substring(0, 8) }}
                    </v-chip>
                    <v-tooltip bottom>
                      <template v-slot:activator="{ on, attrs }">
                        <v-chip small label color="transparent" v-bind="attrs" v-on="on">
                          <span
                            class="d-inline-block text-truncate"
                            style="max-width: 50vw"
                          >
                            {{ commit.commit.message }}
                          </span>
                        </v-chip>
                      </template>

                      {{ commit.commit.committer.date }}
                      <span></span>
                    </v-tooltip>

                    <!-- <v-chip small label color="transparent" disabled>
                      {{ commit.commit.committer.date }}
                    </v-chip> -->
                  </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                  <v-avatar color="primary" size="32">
                    <img :src="commit.author.avatar_url" />
                  </v-avatar>
                  {{ commit.commit.message }}
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>
          </v-row>
        </template>
      </v-card-text>
      <v-card-actions> </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "DashBoard",
  data() {
    return {
      cpuLoad: 20,
      commits: {},
      gcData: {
        tag: "loading",
        hash: "loading",
      },
      serverStats: {
        retcode: -1,
        status: { playerCount: 0, maxPlayer: -1, version: "null" },
      },
      statusWatcher: null,
    };
  },
  mounted() {
    this.LoadCommits();
    this.getGcData();
    var _this = this;
    this.statusWatcher = setInterval(function () {
      _this.getServerStats();
    }, 5000);
  },
  methods: {
    LoadCommits() {
      this.$axios
        .get("getCommits")
        .then((res) => {
          console.log(res.data);
          this.commits = res.data.data;
        })
        .catch();
    },
    getGcData() {
      this.$axios
        .get("getGcData")
        .then((res) => {
          console.log(res.data);
          this.gcData = res.data.data;
        })
        .catch();
    },
    startServer() {
      this.openTerminal();

      if (this.serverStats.retcode == 0) {
        this.stopServer();
      } else {
        this.$axios
          .get("startServer")
          .then((res) => {
            console.log(res.data);
          })
          .catch();
      }

      this.getServerStats();
    },
    stopServer() {
      this.$axios
        .get("stopServer")
        .then((res) => {
          console.log(res.data);
        })
        .catch();
    },
    openTerminal() {
      this.$bus.$emit("showTerminal");
    },

    getServerStats() {
      this.$axios
        .get("serverStats")
        .then((res) => {
          if (res.data.code != 200) {
            this.serverStats.retcode = -1;

            return;
          }

          console.log(res.data);
          this.serverStats = res.data.data;
        })
        .catch();
    },
  },
};
</script>

<style>
.v-card {
  margin: 0px 0px;
}
</style>
