<template>
  <div>
    <v-card>
      <v-toolbar flat color="primary" dark>
        <v-toolbar-title>GrassCutter 配置</v-toolbar-title>
      </v-toolbar>

      <v-tabs vertical>
        <v-tab>
          <v-icon left> mdi-cog</v-icon>
          环境设定
        </v-tab>
        <v-tab>
          <v-icon left> mdi-code-json</v-icon>
          config.json
        </v-tab>
        <v-tab>
          <v-icon left> mdi-code-json</v-icon>
          卡池编辑
        </v-tab>
        <v-tab>
          <v-icon left> mdi-code-json </v-icon>
          Other
        </v-tab>
        <v-tab-item>
          <v-card flat>
            <v-list-item>
              <v-list-item-content>
                <v-text-field
                  label="GC 运行目录"
                  outlined
                  dense
                  v-model="gcCFG.tmpPath"
                  :readonly="true"
                ></v-text-field>
              </v-list-item-content>
            </v-list-item>
            <v-list-item>
              <v-list-item-content>
                <v-text-field
                  label="GC 核心"
                  outlined
                  dense
                  :readonly="true"
                  v-model="gcCFG.gcFileName"
                ></v-text-field>
              </v-list-item-content>
            </v-list-item>

            <v-card-actions>
              <v-btn text dark color="primary" @click="setEnv">
                更改环境信息
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-expansion-panels flat>
              <v-expansion-panel v-for="(item, i) in 5" :key="i">
                <v-expansion-panel-header> Item </v-expansion-panel-header>
                <v-expansion-panel-content>
                  Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
                  tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim
                  veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea
                  commodo consequat.
                </v-expansion-panel-content>
              </v-expansion-panel>
            </v-expansion-panels>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <p>
                Morbi nec metus. Suspendisse faucibus, nunc et pellentesque egestas, lacus
                ante convallis tellus, vitae iaculis lacus elit id tortor. Sed mollis,
                eros et ultrices tempus, mauris ipsum aliquam libero, non adipiscing dolor
                urna a orci. Curabitur ligula sapien, tincidunt non, euismod vitae,
                posuere imperdiet, leo. Nunc sed turpis.
              </p>

              <p>
                Suspendisse feugiat. Suspendisse faucibus, nunc et pellentesque egestas,
                lacus ante convallis tellus, vitae iaculis lacus elit id tortor. Proin
                viverra, ligula sit amet ultrices semper, ligula arcu tristique sapien, a
                accumsan nisi mauris ac eros. In hac habitasse platea dictumst. Fusce ac
                felis sit amet ligula pharetra condimentum.
              </p>

              <p>
                Sed consequat, leo eget bibendum sodales, augue velit cursus nunc, quis
                gravida magna mi a libero. Nam commodo suscipit quam. In consectetuer
                turpis ut velit. Sed cursus turpis vitae tortor. Aliquam eu nunc.
              </p>

              <p>
                Etiam ut purus mattis mauris sodales aliquam. Ut varius tincidunt libero.
                Aenean viverra rhoncus pede. Duis leo. Fusce fermentum odio nec arcu.
              </p>

              <p class="mb-0">
                Donec venenatis vulputate lorem. Aenean viverra rhoncus pede. In dui
                magna, posuere eget, vestibulum et, tempor auctor, justo. Fusce commodo
                aliquam arcu. Suspendisse enim turpis, dictum sed, iaculis a, condimentum
                nec, nisi.
              </p>
            </v-card-text>
          </v-card>
        </v-tab-item>
        <v-tab-item>
          <v-card flat>
            <v-card-text>
              <p>
                Fusce a quam. Phasellus nec sem in justo pellentesque facilisis. Nam eget
                dui. Proin viverra, ligula sit amet ultrices semper, ligula arcu tristique
                sapien, a accumsan nisi mauris ac eros. In dui magna, posuere eget,
                vestibulum et, tempor auctor, justo.
              </p>

              <p class="mb-0">
                Cras sagittis. Phasellus nec sem in justo pellentesque facilisis. Proin
                sapien ipsum, porta a, auctor quis, euismod ut, mi. Donec quam felis,
                ultricies nec, pellentesque eu, pretium quis, sem. Nam at tortor in tellus
                interdum sagittis.
              </p>
            </v-card-text>
          </v-card>
        </v-tab-item>
      </v-tabs>
      <v-divider></v-divider>
      <v-card-actions>
        <v-btn text @click="tree = []"> 从磁盘加载配置 </v-btn>

        <v-spacer></v-spacer>

        <v-btn class="white--text" color="green darken-1" depressed>
          保存配置
          <v-icon right> mdi-content-save </v-icon>
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      gcCFG: {},
    };
  },
  mounted() {
    this.getGcCFG();
  },
  methods: {
    getGcCFG() {
      this.$axios.get("/loadEnv").then((res) => {
        this.gcCFG = res.data.data;
      });
    },
    setEnv() {
      this.$bus.$emit("setEnv");
    },
  },
};
</script>

<style></style>
