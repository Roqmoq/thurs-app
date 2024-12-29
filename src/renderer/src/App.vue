<template>
  <div v-if="main_flg" class="container w-100">
    <!-- <div class="row subscore-row d-flex align-items-center odai-field">
      <p v-if="odai_vis" id="odai" style="color: white">{{ odai }}</p>
    </div> -->
    <div
      v-if="score_open"
      class="d-flex align-items-center row d-flex total_score align-items-center"
    >
      <div class="total">
        <a class="text-center circle" :class="scoreClass">
          {{ judge_total }}
        </a>
      </div>
    </div>
  </div>
  <div v-if="config_flg && !odaiset_flg" class="container w-100" style="background: gray">
    <h1 style="color: white">設定ページ</h1>
    <!-- <h3 style="color: skyblue; margin-top: 10px">各審査員の得点</h3>
    <div>
      <input
        id="visible"
        v-model="button_visible"
        type="radio"
        name="button_color"
        value="visible"
        checked
      />
      <label for="visible" style="color: white">見せる</label>
    </div>
    <div>
      <input id="hide" v-model="button_visible" type="radio" name="button_color" value="hide" />
      <label for="hide" style="color: white">隠す</label>
    </div> -->
    <!--    <h3 style="color: skyblue; margin-top: 10px">ボーナス基準となる点数(デフォルト5)</h3>-->
    <!--    <input type="number" id="baseScore" name="baseScore" v-model="baseScore"/>-->
    <!--    <h3 style="color: skyblue; margin-top: 10px">ボーナス時の倍率(デフォルト10)</h3>-->
    <!--    <input type="number" id="bonus" name="bonus" v-model="bonus"/>-->
    <!--    <h3 style="color: skyblue; margin-top: 100px"></h3>-->
    <table style="text-align: center">
      <tr>
        <th>合計</th>
        <th>ポイント</th>
        <th>効果</th>
      </tr>
      <tr>
        <th>0</th>
        <th><input v-model="point[0]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[0]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>1</th>
        <th><input v-model="point[1]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[1]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>2</th>
        <th><input v-model="point[2]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[2]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>3</th>
        <th><input v-model="point[3]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[3]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>4</th>
        <th><input v-model="point[4]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[4]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>5</th>
        <th><input v-model="point[5]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[5]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>6</th>
        <th><input v-model="point[6]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[6]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>7</th>
        <th><input v-model="point[7]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[7]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>8</th>
        <th><input v-model="point[8]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[8]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>9</th>
        <th><input v-model="point[9]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[9]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
      <tr>
        <th>10</th>
        <th><input v-model="point[10]" type="number" style="width: 100px" name="point[]" /></th>
        <th>
          <select v-model="effect[10]">
            <option>small</option>
            <option>medium</option>
            <option>large</option>
          </select>
        </th>
      </tr>
    </table>
    <div style="margin-top: 10px">
      <span>通常モード</span>
      <input v-model="mode" type="radio" name="normal" value="normal" />
      <br />
      <span>MinMax削除モード</span>
      <input v-model="mode" type="radio" name="strict" value="strict" />
    </div>
  </div>
</template>

<script>
import smallSe from './assets/small_sound.mp3'
import mediumSe from './assets/medium_sound.mp3'
import largeSe from './assets/large_sound.mp3'
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
      result_flg: false,
      is_plus: false,
      is_blind: false,
      main_flg: true,
      config_flg: false,
      shutsudai_flg: false,
      baseScore: 5,
      point: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      effect: [
        'small',
        'small',
        'small',
        'small',
        'small',
        'small',
        'small',
        'small',
        'small',
        'small',
        'small'
      ],
      bonus: 10,
      button_visible: 'visible',
      smallSe: new Audio(smallSe),
      mediumSe: new Audio(mediumSe),
      largeSe: new Audio(largeSe),
      odaiset_flg: false,
      odais: [],
      odai: '',
      odai_vis: false,
      odai_count: -1,
      scoreClass: 'small',
      mode: 'normal'
    }
  },
  // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  mounted() {
    // イベントリスナーの登録
    document.addEventListener('keydown', this.onKeyDown)
  },
  // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
  beforeUnmount() {
    // イベントリスナーの削除
    document.removeEventListener('keydown', this.onKeyDown)
  },
  // メソッドの中身は、状態を変化させ、更新をトリガーさせる関数です。
  // 各メソッドは、テンプレート内のイベントハンドラーにバインドすることができます。
  methods: {
    // eslint-disable-next-line @typescript-eslint/explicit-function-return-type
    onKeyDown: function (e) {
      const key = e.key
      if (key === '2' && this.judge_1 === 99) {
        this.judge_1 = 1
      } else if (key === '3' && this.judge_1 === 99) {
        this.judge_1 = 2
      } else if (key === '1' && this.judge_1 === 99) {
        this.judge_1 = 0
      } else if (key === '4' && this.judge_2 === 99) {
        this.judge_2 = 0
      } else if (key === '5' && this.judge_2 === 99) {
        this.judge_2 = 1
      } else if (key === '6' && this.judge_2 === 99) {
        this.judge_2 = 2
      } else if (key === '7' && this.judge_3 === 99) {
        this.judge_3 = 0
      } else if (key === '8' && this.judge_3 === 99) {
        this.judge_3 = 1
      } else if (key === '9' && this.judge_3 === 99) {
        this.judge_3 = 2
      } else if (key === 'q' && this.judge_4 === 99) {
        this.judge_4 = 0
      } else if (key === 'w' && this.judge_4 === 99) {
        this.judge_4 = 1
      } else if (key === 'e' && this.judge_4 === 99) {
        this.judge_4 = 2
      } else if (key === 'r' && this.judge_5 === 99) {
        this.judge_5 = 0
      } else if (key === 't' && this.judge_5 === 99) {
        this.judge_5 = 1
      } else if (key === 'y' && this.judge_5 === 99) {
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
        this.is_plus = false
      } else if (key === 'Escape') {
        this.main_flg = !this.main_flg
        this.config_flg = !this.config_flg
      } else if (key === 'ArrowDown') {
        this.shutsudai()
      }
      if (
        this.judge_total === 0 &&
        this.judge_1 !== 99 &&
        this.judge_2 !== 99 &&
        this.judge_3 !== 99 &&
        this.judge_4 !== 99 &&
        this.judge_5 !== 99
      ) {
        if (this.mode == "normal"){
          this.sum = this.judge_1 + this.judge_2 + this.judge_3 + this.judge_4 + this.judge_5
        } else {
          const sumArray = [this.judge_1, this.judge_2, this.judge_3, this.judge_4, this.judge_5]
          sumArray.sort((a, b) => {
            return a - b
          })
          sumArray.pop()
          sumArray.shift()
          this.sum = sumArray.reduce(function (sum, element) {
            return sum + element
          }, 0)
        }
        const pointArr = Object.values(this.point).map((value) => {
          return value
        })
        const effectArr = this.effect.map((item) => {
          return item.valueOf()
        })
        console.log(this.judge_1)
        console.log(this.judge_2)
        console.log(this.judge_3)
        console.log(this.judge_4)
        console.log(this.judge_5)
        console.log(this.sum)
        this.judge_total = pointArr[this.sum]
        if (effectArr[this.sum] === 'small') {
          this.scoreClass = 'smallScore'
          this.smallSe.currentTime = 0
          this.smallSe.play()
        } else if (effectArr[this.sum] === 'medium') {
          this.scoreClass = 'mediumScore'
          this.mediumSe.currentTime = 0
          this.mediumSe.play()
        } else {
          this.scoreClass = 'largeScore'
          this.largeSe.currentTime = 0
          this.largeSe.play()
        }
        this.score_open = true
      }
    }
  }
}
</script>

<style lang="less">
.sub-score {
  font-size: 50px;
}

.total {
  text-align: center;
}

.circle {
  //color: #ff0000;
  text-align: center;
  font-weight: 700;
  text-decoration: none;
  font-size: 200px;
}

#app {
  height: 100vh;
}

.container {
  height: 100vh;
}

.test {
  display: flex;
  justify-content: center;
  align-items: center;
}

body {
  height: 100%;
}

.container {
  background-color: #000000;
}

.subscore-row {
  height: 200px;
}

.plus {
  color: #ffffff;
}

.visible {
  color: white;
}

.hidden {
  color: black;
}

.hunt {
  width: 50%;
  margin: 0 auto;
  text-align: center;
}

.pre {
  display: flex;
  align-items: center;
}

.odai-field {
  height: 300px;
  text-align: center;
  vertical-align: center;
}

#odai {
  font-size: 50px;
  white-space: pre-wrap;
  font-family: 'Menlo', 'Lucida Console', monospace;
  font: bold;
}

.smallScore {
  color: white;
}

.mediumScore {
  color: yellow;
}

.largeScore {
  color: rgb(0, 221, 255);
}

.total_score {
  height: 100%;
}

@media (min-width: 576px) {
  .container-sm,
  .container {
    max-width: 100% !important;
  }
}
@media (min-width: 768px) {
  .container-md,
  .container-sm,
  .container {
    max-width: 100% !important;
  }
}
@media (min-width: 992px) {
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 100% !important;
  }
}
@media (min-width: 1200px) {
  .container-xl,
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 100% !important;
  }
}
@media (min-width: 1400px) {
  .container-xxl,
  .container-xl,
  .container-lg,
  .container-md,
  .container-sm,
  .container {
    max-width: 100% !important;
  }
}
</style>
