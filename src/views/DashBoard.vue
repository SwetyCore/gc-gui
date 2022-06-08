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
                  服务器版本:<v-chip style="margin: 0px 5px" small color="primary"
                    >2.7.0</v-chip
                  >
                  <v-chip style="margin: 0px 5px" small color="primary"
                    >1.1.2-dev[ddce927f]</v-chip
                  >
                  <v-chip style="margin: 0px 5px" small color="green" dark
                    >有新版本可用</v-chip
                  >
                </v-list-item-title>
              </v-list-item>
              <v-list-item>
                <v-list-item-title style="font-size: 20px">
                  服务器状态:<v-chip style="margin: 0px 5px" small color="red" dark
                    >已关闭</v-chip
                  >
                </v-list-item-title> </v-list-item
              ><v-list-item>
                <v-list-item-title style="font-size: 20px">
                  在线人数:<v-chip style="margin: 0px 5px" small color="primary"
                    >0</v-chip
                  >
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
        <v-btn color="primary" text>开启服务器</v-btn>
        <v-btn color="primary" text>切换服务器版本</v-btn>
      </v-card-actions>
    </v-card>
    <v-card flat>
      <v-card-title><v-icon>mdi-github</v-icon> 最新提交</v-card-title>
      <v-card-text style="height:50vh;overflow: auto;">
        <template>
          <v-row justify="center" style="margin: 10px">
            <v-expansion-panels accordion flat>
              <v-expansion-panel v-for="(commit, i) in commits" :key="i">
                <v-expansion-panel-header>
                  <div>
                    <v-chip small label :href="commit.html_url" text target="_blank">
                      {{ commit.sha.substring(0, 6) }}
                    </v-chip>
                    <v-chip small label color="transparent" disabled>
                      {{ commit.commit.committer.date }}
                    </v-chip>
                  </div>
                </v-expansion-panel-header>
                <v-expansion-panel-content>
                  <v-avatar color="primary" size="32">
                    <img :src="commit.author.avatar_url"/>
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
    };
  },
  mounted() {
    this.LoadCommits();
  },
  methods: {
    LoadCommits() {
      this.$axios
        .get("data/commits.json")
        .then((res) => {
          console.log(res.data);
          this.commits = res.data;
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
