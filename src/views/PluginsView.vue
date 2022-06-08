<template>
  <div>
    <v-list nav>
      <v-text-field label="搜索"></v-text-field>

      <v-subheader> 已安装插件 </v-subheader>
      <v-list-item v-for="plugins in installed" :key="plugins.name" target="_blank">
        <v-list-item-icon>
          <v-badge dot color="red" overlap>
            <v-icon large>mdi-puzzle</v-icon>
          </v-badge>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>
            {{ plugins.name }}

            <v-chip label small outlined style="margin: 0px 2px" dark color="green">
                            {{ "v0.0.0" }}->{{'v1.1.1'}}

            </v-chip>
            <v-chip
              style="margin: 0px 2px"
              color="primary"
              small
              label
              v-for="tag in plugins.tags"
              :key="tag"
            >
              {{ tag }}
            </v-chip>
          </v-list-item-title>
          <v-list-item-subtitle
            >{{ plugins.description }}

            <v-btn x-small text :href="link(plugins.github)" target="_blank">
              <v-icon small> mdi-open-in-new </v-icon>
              查看详情
            </v-btn>
          </v-list-item-subtitle>
        </v-list-item-content>
        <v-btn text color="green"> <v-icon>mdi-arrow-up</v-icon> 升级 </v-btn>
        <div style="width: 10px"></div>
        <v-btn text color="red"> <v-icon>mdi-delete</v-icon> 卸载 </v-btn>
        <div style="width: 10px"></div>
        <v-switch> </v-switch>
      </v-list-item>
    </v-list>
    <v-divider></v-divider>
    <v-list two-line subheader>
      <v-subheader>未安装插件</v-subheader>

      <v-list-item v-for="plugins in installed" :key="plugins.name" target="_blank">
        <v-list-item-icon>
          <!-- <v-badge dot color="red" overlap> -->
            <v-icon large>mdi-puzzle</v-icon>
          <!-- </v-badge> -->
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>
            {{ plugins.name }}

            <v-chip label small outlined style="margin: 0px 2px" dark color="green">
              {{ "v0.0.0" }}
            </v-chip>
            <v-chip
              style="margin: 0px 2px"
              color="primary"
              small
              label
              v-for="tag in plugins.tags"
              :key="tag"
            >
              {{ tag }}
            </v-chip>
          </v-list-item-title>
          <v-list-item-subtitle
            >{{ plugins.description }}

            <v-btn x-small text :href="link(plugins.github)" target="_blank">
              <v-icon small> mdi-open-in-new </v-icon>
              查看详情
            </v-btn>
          </v-list-item-subtitle>
        </v-list-item-content>
        <v-btn text color="primary"> <v-icon>mdi-plus</v-icon> 安装 </v-btn>
        <!-- <div style="width: 10px"></div> -->
      </v-list-item>
    </v-list>
  </div>
</template>

<script>
export default {
  data() {
    return {
      plugins: {},
    };
  },
  mounted() {
    this.loadMarket();
  },

  methods: {
    loadMarket() {
      this.$axios
        .get("data/market.json")
        .then((res) => {
          console.log(res.data);
          this.plugins = res.data;
        })
        .catch();
    },
    link(old) {
      return "https://github.com/" + old;
    },
  },

  computed: {
    installed() {
      return this.plugins;
    },
    uninstalled() {
      return {};
    },
    isUpdate() {
      return false;
    },
  },
};
</script>

<style></style>
