<template>
  <div class="container">
    <div class="row">
      <div class="col-2">
        <p v-if="judge_1 === 99" class="text-center sub-score judge-score">--</p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_1 }}
        </p>
      </div>
      <div class="col-2">
        <p v-if="judge_2 === 99" class="text-center sub-score judge-score">--</p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_2 }}
        </p>
      </div>
      <div class="col-2">
        <p v-if="judge_3 === 99" class="text-center sub-score judge-score">--</p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_3 }}
        </p>
      </div>
      <div class="col-2">
        <p v-if="judge_4 === 99" class="text-center sub-score judge-score">--</p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_4 }}
        </p>
      </div>
      <div class="col-2">
        <p v-if="judge_5 === 99" class="text-center sub-score judge-score">--</p>
        <p v-else class="text-center sub-score judge-score">
          {{ judge_5 }}
        </p>
      </div>
    </div>
    <br />
    <div v-if="score_open" class="row total_score align-items-center">
      <div class="parent">
        <a class="text-center circle">
          {{ judge_total }}
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  data() {
    return {
      count: 0,
      judge_1: 99,
      judge_2: 99,
      judge_3: 99,
      judge_4: 99,
      judge_5: 99,
      judge_total: 0,
      sum: 0,
      score_open: false,
      result_flg: false
    }
  },
  // メソッドの中身は、状態を変化させ、更新をトリガーさせる関数です。
  // 各メソッドは、テンプレート内のイベントハンドラーにバインドすることができます。
  methods: {
    onKeyDown: function (e) {
      const key = e.key
      if (key === '1' && this.judge_1 === 99) {
        this.judge_1 = 1
      } else if (key === '2' && this.judge_1 === 99) {
        this.judge_1 = 2
      } else if (key === '0' && this.judge_1 === 99) {
        this.judge_1 = 0
      } else if (key === '3' && this.judge_2 === 99) {
        this.judge_2 = 0
      } else if (key === '4' && this.judge_2 === 99) {
        this.judge_2 = 1
      } else if (key === '5' && this.judge_2 === 99) {
        this.judge_2 = 2
      } else if (key === '6' && this.judge_3 === 99) {
        this.judge_3 = 0
      } else if (key === '7' && this.judge_3 === 99) {
        this.judge_3 = 1
      } else if (key === '8' && this.judge_3 === 99) {
        this.judge_3 = 2
      } else if (key === '9' && this.judge_4 === 99) {
        this.judge_4 = 0
      } else if (key === 'q' && this.judge_4 === 99) {
        this.judge_4 = 1
      } else if (key === 'w' && this.judge_4 === 99) {
        this.judge_4 = 2
      } else if (key === 'e' && this.judge_5 === 99) {
        this.judge_5 = 0
      } else if (key === 'r' && this.judge_5 === 99) {
        this.judge_5 = 1
      } else if (key === 't' && this.judge_5 === 99) {
        this.judge_5 = 2
      } else if (key === 'Enter') {
        this.judge_1 = 99
        this.judge_2 = 99
        this.judge_3 = 99
        this.judge_4 = 99
        this.judge_5 = 99
        this.judge_total = 0
        this.sum = 0
        this.score_open = false
        this.result_flg = false
      }
      if (
        this.judge_total === 0 &&
        this.judge_1 !== 99 &&
        this.judge_2 !== 99 &&
        this.judge_3 !== 99 &&
        this.judge_4 !== 99 &&
        this.judge_5 !== 99
      ) {
        this.sum = this.judge_1 + this.judge_2 + this.judge_3 + this.judge_4 + this.judge_5
        if (this.sum > 4) {
          this.judge_total = this.sum * 10
        } else {
          this.judge_total = this.sum
        }
        this.score_open = true
      }
    }
  }
}
</script>

<style lang="less">
.total_score {
  height: 300px;
}

.sub-score {
  font-size: 50px;
}

.judge-score {
  font-style: italic;
}

.parent {
  text-align: center;
}

.circle {
  display: inline-block;
  border-radius: 50%;
  background: yellow;
  color: #ff5900;
  text-align: center;
  font-style: italic;
  font-weight: 700;
  text-decoration: none;
}
</style>
