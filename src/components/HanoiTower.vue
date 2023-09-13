<template>
  <div class="center-container">
    <div>
      <h1>汉诺塔问题</h1>
      <div class="tower">
        <div
            v-for="(peg, index) in towers"
            :key="index"
            class="peg"
        >
          <div
              v-for="(disk, dIndex) in peg"
              :key="dIndex"
              class="disk"
              :style="{ width: disk * 20 + 'px' }"
          >
            {{ disk }}
          </div>
        </div>
      </div>
      <el-row>操作次数: {{step}}</el-row>
      <button @click="solveHanoi">解决汉诺塔</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      step: 0,
      towers: [[3, 2, 1], [], []], // 三个柱子，初始状态
    };
  },
  methods: {
    async solveHanoi() {
      const numberOfDisks = this.towers[0].length;
      await this.moveDisks(numberOfDisks, 0, 2, 1);
    },
    async moveDisks(num, source, target, auxiliary) {
      if (num === 1) {
        await this.delay(1000); // 增加适当的延迟，例如1秒
        const disk = this.towers[source].pop();
        this.step++
        this.towers[target].push(disk);
      } else {
        await this.moveDisks(num - 1, source, auxiliary, target);
        await this.delay(1000); // 增加适当的延迟，例如1秒
        const disk = this.towers[source].pop();
        this.step++
        this.towers[target].push(disk);
        await this.moveDisks(num - 1, auxiliary, target, source);
      }
    },
    delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    }
  },
};
</script>

<style scoped>
  .center-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 80vh;
    text-align: center;
  }

  .tower {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
  }

  .peg {
    width: 20px;
    height: 200px;
    background-color: #333;
    display: flex;
    flex-direction: column-reverse;
    align-items: center;
    justify-content: flex-end;
    border: 1px solid #666;
    margin: 0 100px; /* 添加水平间隔 */
  }

  .disk {
    background-color: #3498db;
    margin: 2px;
    border-radius: 4px;
    text-align: center;
    color: white;
    font-weight: bold;
    transition: all 0.3s ease-in-out;
  }

  /* Add some hover effect for disks */
  .disk:hover {
    background-color: #2980b9;
    transform: scale(1.05);
    cursor: pointer;
  }

  button {
    margin-top: 20px;
    background-color: #27ae60;
    color: white;
    border: none;
    border-radius: 4px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
  }

  button:hover {
    background-color: #219952;
  }
</style>