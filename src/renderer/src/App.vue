<template>
  <div class="container">
    <div class="row">
      <div class="col-3">
        <p v-if="judge_1 === 0" class="text-center sub-score judge-score">
          --
        </p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_1 }}
        </p>
      </div>
      <div class="col-3">
        <p v-if="judge_2 === 0" class="text-center sub-score judge-score">
          --
        </p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_2 }}
        </p>
      </div>
      <div class="col-3">
        <p v-if="judge_3 === 0" class="text-center sub-score judge-score">
          --
        </p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_3 }}
        </p>
      </div>
      <div class="col-3">
        <p v-if="judge_4 === 0" class="text-center sub-score judge-score">
          --
        </p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_4 }}
        </p>
      </div>
    </div><br>
    <div v-if="score_open" class="row total_score align-items-center">
      <div class="parent">
        <a class="text-center circle" v-bind:style="{
          height: result_flg ? judge_total * 4 + 350 + 'px' : judge_total * 4 + 300 + 'px',
          width: result_flg ? judge_total * 4 + 350 + 'px': judge_total * 4 + 300 + 'px',
          lineHeight: result_flg ? judge_total * 4 + 350 + 'px' : judge_total * 4 + 300 + 'px',
          fontSize: result_flg ? judge_total * 3 + 250 + 'px' : judge_total * 3 + 200 + 'px',
          color:result_flg ? 'red' : '#FF5900FF'}">
          {{ judge_total }}
        </a>
      </div>
    </div>
  </div>
</template>


<script>
import up from './assets/up.mp3';
import decision1 from './assets/dicision1.mp3';
import decision2 from './assets/dicision2.mp3';
export default {
  data() {
    return {
      count: 0,
      judge_1: 0,
      judge_2: 0,
      judge_3: 0,
      judge_4: 0,
      judge_total: 0,
      sum: 0,
      score_open: false,
      result_flg: false,
      up: new Audio(up),
      decision1: new Audio(decision1),
      decision2: new Audio(decision2),
    }
  },
  // メソッドの中身は、状態を変化させ、更新をトリガーさせる関数です。
  // 各メソッドは、テンプレート内のイベントハンドラーにバインドすることができます。
  methods: {
    onKeyDown: function(e) {
      let key = e.key;
      if (key === "1" && this.judge_1 === 0) {
        this.judge_1 = 1;
      } else if (key === "2" && this.judge_1 === 0) {
        this.judge_1 = 2;
      } else if (key === "3" && this.judge_1 === 0) {
        this.judge_1 = 3;
      } else if (key === "4" && this.judge_2 === 0) {
        this.judge_2 = 1;
      } else if (key === "5" && this.judge_2 === 0) {
        this.judge_2 = 2;
      } else if (key === "6" && this.judge_2 === 0) {
        this.judge_2 = 3;
      } else if (key === "7" && this.judge_3 === 0) {
        this.judge_3 = 1;
      } else if (key === "8" && this.judge_3 === 0) {
        this.judge_3 = 2;
      } else if (key === "9" && this.judge_3 === 0) {
        this.judge_3 = 3;
      } else if (key === "q" && this.judge_4 === 0) {
        this.judge_4 = 1;
      } else if (key === "w" && this.judge_4 === 0) {
        this.judge_4 = 2;
      } else if (key === "e" && this.judge_4 === 0) {
        this.judge_4 = 3;
      } else if (key === "Enter") {
        this.judge_1 = 0;
        this.judge_2 = 0;
        this.judge_3 = 0;
        this.judge_4 = 0;
        this.judge_total = 0;
        this.sum = 0;
        this.score_open = false
        this.result_flg = false
      }
      if (this.judge_total === 0 && this.judge_1 * this.judge_2 * this.judge_3 * this.judge_4 > 0) {
        this.sum = this.judge_1 * this.judge_2 * this.judge_3 * this.judge_4
        this.stepUpTotal(this.sum);
      }
    },
    stepUpTotal: async function(num) {
      const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));
      const interval = async (ms, maxCount) => {
        while (this.judge_total < maxCount) {
          await new Promise(resolve => setTimeout(resolve, ms));
          if (this.judge_total !== maxCount) {
            this.judge_total++
            this.up.currentTime = 0
            await this.up.play()
          }
        }
      }
      this.score_open = true;
      await sleep(500);
      let intervalTime = num > 36 ? 1400 / num : 700 / num;
      await interval(intervalTime, num)
      await sleep(700);
      if (num === 81) {
        this.decision2.currentTime = 0
        await this.decision2.play()
      } else {
        this.decision1.currentTime = 0
        await this.decision1.play()
      }
      this.result_flg = true
    },
  },
  // ライフサイクルフックは、コンポーネントのライフサイクルの
  // 特定のステージで呼び出されます。
  // 以下の関数は、コンポーネントが「マウント」されたときに呼び出されます。
  mounted() {
    document.addEventListener('keydown', this.onKeyDown)
  },
  beforeDestroy() {
    document.removeEventListener('keydown', this.onKeyDown)
  },
}
</script>

<style lang="less">
.total_score {
  height: 800px;
}

.sub-score{
  font-size: 50px;
}

.judge-score {
  font-style: italic;
}

.parent {
  text-align: center;
}

.circle{
  display: inline-block;
  border-radius: 50%;
  background: yellow;
  color: #ff5900;
  text-align:center;
  font-style: italic;
  font-weight: 700;
  text-decoration:none;
}
</style>
