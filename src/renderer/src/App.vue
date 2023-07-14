<template>
  <div class="container">
    <div class="row">
      <div class="col-3">
        <p class="text-center">
        {{ judge_1 }}
        </p>
      </div>
      <div class="col-3">
        <p class="text-center">
          {{ judge_2 }}
        </p>
      </div>
      <div class="col-3">
        <p class="text-center">
          {{ judge_3 }}
        </p>
      </div>
      <div class="col-3">
        <p class="text-center">
          {{ judge_4 }}
        </p>
      </div>
    </div><br>
    <div class="row">
      <p class="fs-1 text-center">
        {{ judge_total }}
      </p>
    </div><br>
  </div>
</template>


<script>
export default {
  data() {
    return {
      count: 0,
      judge_1: 0,
      judge_2: 0,
      judge_3: 0,
      judge_4: 0,
      judge_total: 0,
    }
  },

  // メソッドの中身は、状態を変化させ、更新をトリガーさせる関数です。
  // 各メソッドは、テンプレート内のイベントハンドラーにバインドすることができます。
  methods: {
    onKeyDown: function(e) {
      let key = e.key;
      if (key === "q" && this.judge_1 === 0) {
        this.judge_1 = 1;
      } else if (key === "w" && this.judge_1 === 0) {
        this.judge_1 = 2;
      } else if (key === "e" && this.judge_1 === 0) {
        this.judge_1 = 3;
      } else if (key === "a" && this.judge_2 === 0) {
        this.judge_2 = 1;
      } else if (key === "s" && this.judge_2 === 0) {
        this.judge_2 = 2;
      } else if (key === "d" && this.judge_2 === 0) {
        this.judge_2 = 3;
      } else if (key === "z" && this.judge_3 === 0) {
        this.judge_3 = 1;
      } else if (key === "x" && this.judge_3 === 0) {
        this.judge_3 = 2;
      } else if (key === "c" && this.judge_3 === 0) {
        this.judge_3 = 3;
      } else if (key === "r" && this.judge_4 === 0) {
        this.judge_4 = 1;
      } else if (key === "t" && this.judge_4 === 0) {
        this.judge_4 = 2;
      } else if (key === "y" && this.judge_4 === 0) {
        this.judge_4 = 3;
      } else if (key === "Enter") {
        this.judge_1 = 0;
        this.judge_2 = 0;
        this.judge_3 = 0;
        this.judge_4 = 0;
      }
      this.judge_total = this.judge_1 * this.judge_2 * this.judge_3 * this.judge_4;
    }
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
//
</style>
