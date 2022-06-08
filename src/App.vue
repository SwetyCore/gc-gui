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
      <v-toolbar
          dark
          flat
          color="primary"
        >
          <v-toolbar-title>{{title}}</v-toolbar-title>
          <v-spacer></v-spacer>

      </v-toolbar>
    </v-app-bar>

    <!-- 根据应用组件来调整你的内容 -->
    <v-main>
      <!-- 给应用提供合适的间距 -->
      <v-container fluid>
        <!-- 如果使用 vue-router -->
        <router-view></router-view>
      </v-container>
    </v-main>

    <v-footer height="28px" app color="blue">
      <!-- -->
      <div class="text-center">
        <v-bottom-sheet v-model="sheet">
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" x-small elevation="0" text dark>
              <v-icon small> mdi-console </v-icon>打开终端
            </v-btn>
          </template>
          <v-sheet class="text-left" height="420px" style="padding:10px">
              <div>
                终端
              </div>

              <terminal-view />
          </v-sheet>
        </v-bottom-sheet>

        
      </div>
    </v-footer>
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
    items: [
      { title: "仪表盘", icon: "mdi-view-dashboard", to: "/" },
      { title: "插件管理", icon: "mdi-puzzle", to: "/plugins" },
      { title: "配置编辑", icon: "mdi-cog", to: "/config" },
      { title: "文件管理", icon: "mdi-folder", to: "/files" },
      { title: "玩家管理", icon: "mdi-account-multiple", to: "/users" },
      { title: "关于", icon: "mdi-information", to: "/about" },
    ],
    right: null,
  }),
};
</script>
<style>
footer {
  padding: 0px !important;
}
</style>
