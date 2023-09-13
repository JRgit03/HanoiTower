<template>
  <el-main>
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="HanoiTower可视化" name="first">
        <HanrotaVue></HanrotaVue>
      </el-tab-pane>
      <el-tab-pane label="HanoiTower可视化Chatgpt版本" name="second">
        <HanoiTower></HanoiTower>
      </el-tab-pane>
    </el-tabs>
  </el-main>
</template>

<script>
import HanrotaVue from "@/components/HanrotaVue";
import HanoiTower from "@/components/HanoiTower";

export default {
  components: {HanrotaVue,HanoiTower},
  data(){
   return {
     activeName: 'first'
   }
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    }
  }
}
</script>

<style>

</style>

