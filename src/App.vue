<template>
  <v-app>
    <v-navigation-drawer app>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> GrassCutter GUI </v-list-item-title>
          <v-list-item-subtitle> gc 助手 </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link :to="item.to">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app dark color="primary">
      <v-toolbar dark flat color="primary">
        <v-toolbar-title>{{ title }}</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
    </v-app-bar>

    <!-- 根据应用组件来调整你的内容 -->
    <v-main>
      <!-- 给应用提供合适的间距 -->
      <v-container fluid>
        <!-- 如果使用 vue-router -->
        <keep-alive>
          <router-view></router-view>
        </keep-alive>
      </v-container>
    </v-main>

    <v-footer height="28px" app color="blue">
      <!-- -->
      <div class="text-center">
        <v-bottom-sheet v-model="sheet" ref="termianl">
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" x-small elevation="0" text dark>
              <v-icon small> mdi-console </v-icon>打开终端
            </v-btn>
          </template>
          <v-sheet class="text-left" height="420px" style="padding: 10px">
            <div>终端</div>

            <terminal-view />
          </v-sheet>
        </v-bottom-sheet>
      </div>
    </v-footer>
    <!-- 对话框 -->
    <v-row justify="center">
      <v-dialog v-model="dialog" persistent max-width="70%">
        <v-card>
          <v-card-title class="text-h5"> GC 环境设置 </v-card-title>
          <v-card-text>
            <v-stepper flat v-model="e1">
              <v-stepper-header>
                <v-stepper-step :complete="e1 > 1" step="1">
                  1. 设置GC运行目录
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="e1 > 2" step="2">
                  2. 选择 GC jar文件
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step step="3"> 3. 运行环境检查 </v-stepper-step>
              </v-stepper-header>

              <v-stepper-items>
                <v-stepper-content step="1">
                  <v-container>
                    <v-text-field
                      label="GC 运行目录"
                      outlined
                      v-model="cfgSetter.targetPath"
                    ></v-text-field>
                  </v-container>
                  <v-btn color="primary" @click="listFiles"> 下一步 </v-btn>
                </v-stepper-content>

                <v-stepper-content step="2">
                  <v-container>
                    <!-- <span style="margin-bottom:20px">{{cfgSetter.targetPath}}</span> -->

                    <v-select
                      :items="cfgSetter.files"
                      label="GC 核心"
                      v-model="cfgSetter.selected"
                      outlined
                    ></v-select>
                  </v-container>

                  <v-btn color="primary" @click="e1 = 3"> 下一步 </v-btn>
                  <v-btn text @click="e1 = 1"> 上一步 </v-btn>
                </v-stepper-content>

                <v-stepper-content step="3">
                  <v-list-item>
                    <v-list-item-content>
                        <v-text-field
                          label="GC 运行目录"
                          outlined
                          dense
                          v-model="cfgSetter.targetPath"
                          readonly="true"
                        ></v-text-field>
                    </v-list-item-content>
                  </v-list-item>
                  <v-list-item>
                    <v-list-item-content>
                        <v-text-field
                          label="GC 核心"
                          outlined
                          dense
                          readonly="true"
                          v-model="cfgSetter.selected"
                        ></v-text-field>
                    </v-list-item-content>
                  </v-list-item>

                  <v-btn color="primary" @click="setGcPath"> 完成 </v-btn>
                  <v-btn text @click="e1 = 2"> 上一步 </v-btn>
                </v-stepper-content>
              </v-stepper-items>
            </v-stepper>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-row>
  </v-app>
</template>

<script>
import TerminalView from "./views/TerminalView.vue";
export default {
  components: { TerminalView },
  name: "App",
  data: () => ({
    //
    title: "标题",
    sheet: false,
    dialog: false,
    items: [
      { title: "仪表盘", icon: "mdi-view-dashboard", to: "/" },
      { title: "插件管理", icon: "mdi-puzzle", to: "/plugins" },
      { title: "配置编辑", icon: "mdi-cog", to: "/config" },
      { title: "文件管理", icon: "mdi-folder", to: "/files" },
      { title: "玩家管理", icon: "mdi-account-multiple", to: "/users" },
      { title: "关于", icon: "mdi-information", to: "/about" },
    ],
    e1: 1,
    cfgSetter: {
      targetPath: "D:\\Server\\Server-2.7-dev\\Grasscutter",
      files: [],
      selected: "",
    },
    right: null,
  }),
  created() {
    this.checkEnv();
  },
  mounted() {
    this.$bus.$on("showTerminal", () => {
      this.showTerminal();
    });

    this.$bus.$on("setEnv", () => {
      this.dialog=true;
    });
  },
  methods: {
    showTerminal() {
      this.sheet = true;
    },
    checkEnv() {
      this.$axios
        .get("checkEnv")
        .then((res) => {
          if (res.data.data) {
            this.dialog = false;
          } else {
            this.dialog = true;
          }
        })
        .catch(() => {
          this.dialog = true;
        });
    },
    listFiles() {
      if (this.cfgSetter.targetPath == "") {
        return;
      }
      this.$axios({
        method: "get",
        params: { path: this.cfgSetter.targetPath }, //发送get请求，使用params关键字接收请求参数
        url: "listFiles",
      }).then((res) => {
        this.cfgSetter.files = res.data.data;
        this.e1 = 2;
      });
      // this.$axios
      //   .get("listFiles", {
      //     params: { path: this.cfgSetter.targetPath },
      //   })
      //   .then((res) => {
      //     this.el = 2;
      //     this.cfgSetter.files = res.data;
      //   })
      //   .catch();
    },

    setGcPath() {
      this.$axios({
        method: "post",
        data: this.cfgSetter,
        url: "setGcPath",
      }).then(() => {
        // this.checkEnv();
        location.reload();
      });
    },
  },
};
</script>
<style>
footer {
  padding: 0px !important;
}
</style>
