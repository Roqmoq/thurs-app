<template>
  <div v-if="main_flg" class="container w-100">
    <div class="row subscore-row d-flex align-items-center odai-field">
      <p v-if="odai_vis" id="odai" style="color: white">{{ odai }}</p>
    </div>
    <div
      v-if="score_open"
      class="d-flex align-items-center row d-flex total_score align-items-center"
    >
      <div class="total">
        <a class="text-center circle" :style="{ color: [is_plus ? 'red' : 'white'] }">
          {{ judge_total }}
        </a>
      </div>
    </div>
  </div>
  <div v-if="config_flg && !odaiset_flg" class="container w-100" style="background: gray">
    <h1 style="color: white">設定ページ</h1>
    <h3 style="color: skyblue; margin-top: 10px">各審査員の得点</h3>
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
    </div>
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
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>1</th>
        <th><input v-model="point[1]" type="number" style="width: 50px" name="point[]" /></th>
        <th><input v-model="effect[1]" type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>2</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>3</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>4</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>5</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>6</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>7</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>8</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>9</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
      <tr>
        <th>10</th>
        <th><input  type="number" style="width: 50px" name="point[]" /></th>
        <th><input  type="checkbox" name="effect[]" /></th>
      </tr>
    </table>
    <input id="bonus" type="button" name="bonus" value="お題セット" @click="odaiset()" />
  </div>
  <div v-if="config_flg && odaiset_flg" class="container w-100">
    <h1 style="color: white">設定ページ > お題入力ページ</h1>
    <input id="bonus" v-model="odai" type="text" name="bonus" />
    <input id="bonus" type="button" name="bonus" value="登録する" @click="register()" />
    <h3 style="color: skyblue; margin-top: 5px">現状のお題一覧</h3>
    <li v-for="(item, index) in odais">
      <a style="color: #9feaf9; font-size: 12px">{{ item }}</a
      ><input :key="index" type="button" value="このお題を消す" @click="deleteit(index)" />
    </li>
    <h3 style="color: skyblue; margin-top: 20px"></h3>
    <input
      id="bonus"
      type="button"
      name="bonus"
      style="color: red"
      value="お題を全て消す"
      @click="resetAll()"
    />
    <input id="bonus" type="button" name="bonus" value="通常設定に戻る" @click="odaiset()" />
  </div>
  <div v-if="shutsudai_flg" class="container w-100 pre">
    <div class="hount">
      <h2 v-if="odai_vis" style="color: white">{{ odai }}</h2>
      <input
        id="bonus"
        style="margin-top: 10px"
        type="button"
        name="bonus"
        value="出題"
        @click="shutsudai()"
      />
    </div>
  </div>
</template>

<script>
import decision1 from './assets/click.mp3'
import decision2 from './assets/bonus.mp3'
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
      effect: [false, false, false, false, false, false, false, false, false, false, false],
      bonus: 10,
      button_visible: 'visible',
      decision1: new Audio(decision1),
      decision2: new Audio(decision2),
      odaiset_flg: false,
      odais: [],
      odai: '',
      odai_vis: false,
      odai_count: -1
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
        this.is_plus = false
      } else if (key === 'Escape') {
        this.main_flg = !this.main_flg
        this.config_flg = !this.config_flg
      } else if (key === 'Shift') {
        this.main_flg = !this.main_flg
        this.shutsudai_flg = !this.shutsudai_flg
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
        this.sum = this.judge_1 + this.judge_2 + this.judge_3 + this.judge_4 + this.judge_5
        const pointArr = Object.values(this.point).map((value) => {
          return value
        })
        const effectArr = this.effect.map((item) => {
          return item.valueOf()
        })
        this.judge_total = pointArr[this.sum]
        if (effectArr[this.sum]) {
          this.is_plus = true
          this.decision2.currentTime = 0
          this.decision2.play()
        } else {
          this.decision1.currentTime = 0
          this.decision1.play()
        }
        this.score_open = true
      }
    },
    odaiset: function () {
      this.odaiset_flg = !this.odaiset_flg
    },
    deleteit(index) {
      this.odais.splice(index, 1)
    },
    resetAll: function () {
      this.odais = []
    },
    register: function () {
      if (this.odai.length > 0) {
        this.odais.push(this.odai)
      }
    },
    shutsudai: function () {
      this.odai_vis = true
      this.odai_count++
      if (this.odai_count > this.odais.length) {
        this.odai_count = 0
      }
      this.odai = this.odais[this.odai_count]
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
