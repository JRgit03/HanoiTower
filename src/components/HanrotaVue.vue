<template>
  <div>
    <el-form :inline="true" class="demo-form-inline" style="margin-top: 20px">
      <el-form-item label="输入汉罗塔层数：">
        <el-input v-model.number="n"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="success" @click="startProcedure" icon="el-icon-open">启动</el-button>
      </el-form-item>
      <el-row class="Tips">Tips：格子的数字代表HanoiTower盘大小, 完整日志信息在控制台, n值不建议太大</el-row>
    </el-form>

    <el-divider></el-divider>
    <div>
      <el-tag>动画展示</el-tag>
      <el-divider></el-divider>
      <el-row>
        <el-tag size="medium">A 塔：</el-tag>
        <el-tag size="small" class="marginLeft30 elTagClass" v-for="(x,index) in ArrA" :key="index"> {{ x }} </el-tag>
      </el-row>
      <el-divider></el-divider>
      <el-row>
        <el-tag size="medium">B 塔：</el-tag>
        <el-tag size="small" class="marginLeft30 elTagClass" v-for="(x,index) in ArrB" :key="index"> {{ x }} </el-tag>
      </el-row>
      <el-divider></el-divider>
      <el-row>
        <el-tag size="medium">C 塔：</el-tag>
        <el-tag size="small" class="marginLeft30 elTagClass" v-for="(x,index) in ArrC" :key="index"> {{ x }} </el-tag>
      </el-row>
      <el-divider></el-divider>
      <el-row>
        <el-tag size="medium" type="success">
          操作计数器：{{ step }} / {{ stepSum }}
          {{ step === stepSum ? "执行完毕 " : " " }}
          <i class="el-icon-loading" v-show="step !== stepSum && step !== 0"></i>
        </el-tag>
        <el-progress :show-text="false" :stroke-width="24" :percentage="step * 100.0 / stepSum"
                     status="success"></el-progress>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: "HanrotaVue",
  data() {
    return {
      n: 1, //汉罗塔层数
      ArrA: [], //A塔
      ArrB: [], //B塔
      ArrC: [], //C塔
      step: 0,  //当前的操作步数
      stepSum: 1, //目标操作步数
    }
  },
  methods: {
    // move(n,a,b,c) 将n个盘从a移动到c借助b
    async move(n, A, B, C) {
      if (n === 1) {
        await this.delay(1000)
        this.step++; let x = 0
        if (A === 'A') {
          x = this.ArrA.pop()
        } else if (A === 'B') {
          x = this.ArrB.pop()
        } else {
          x = this.ArrC.pop()
        } //判断操作塔
        if (C === 'A') {
          this.ArrA.push(x)
        } else if (C === 'B') {
          this.ArrB.push(x)
        } else {
          this.ArrC.push(x)
        } //判断目标他
        await this.$notify({ //通知日志
          title: '操作日志：',
          message: "move " + A + " to " + C,
          offset: 100,
          duration: 3000, //持续时间
        })
        console.log("step" + this.step + "：move " + A + " to " + C)
      } else {
        await this.move(n - 1, A, C, B);
        await this.delay(1000); let x = 0
        if (A === 'A') {
          x = this.ArrA.pop()
        } else if (A === 'B') {
          x = this.ArrB.pop()
        } else {
          x = this.ArrC.pop()
        } //判断操作塔
        if (C === 'A') {
          this.ArrA.push(x)
        } else if (C === 'B') {
          this.ArrB.push(x)
        } else {
          this.ArrC.push(x)
        } //判断目标塔
        this.step++; //将第n个盘移动到C
        await this.$notify({
          title: '操作日志：',
          message: "move " + A + " to " + C,
          offset: 100,
          duration: 3000, //持续时间
        })
        console.log("step" + this.step + "：move " + A + " to " + C)
        await this.move(n - 1, B, A, C)
      }
    },
    async delay(ms) { //设置延时
      return new Promise(resolve => setTimeout(resolve, ms));
    },
    async startProcedure() { //汉罗塔可视化初始化操作
      this.$message.success('启动成功,可视化汉罗塔问题...')
      this.step = 0
      this.stepSum = (1 << this.n) - 1;
      this.ArrA = []
      this.ArrB = []
      this.ArrC = []
      for (let i = this.n; i; i--) {
        this.ArrA.push(i)
      } //初始化A塔
      console.log('---------start---------') //console 日志输出
      await this.move(this.n, 'A', 'B', 'C')
      console.log('----------end----------')
    }
  }
}
</script>

<style scoped>
  .marginLeft30 {
    margin-left: 30px
  }

  .Tips {
    font-size: 8px;
    color: gray;
  }

  .elTagClass{
    width: 100px;
    text-align: center;
  }
</style>