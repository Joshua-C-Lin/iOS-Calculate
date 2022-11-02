<template>
  <div>
    <h1>{{ title }}</h1>
    <div class="calculator">
      <!-- <input v-model="field" type="tel" class="field-input" /> -->
      <div class="num-display-area">
        {{ calResults.toLocaleString() }}
      </div>

      <table class="table">
        <tr>
          <td>
            <div class="num-btn gray-btn" @click="acBtn">AC</div>
          </td>
          <td>
            <div class="num-btn gray-btn" @click="posNegSwitching">+/-</div>
          </td>
          <td>
            <div class="num-btn gray-btn">%</div>
          </td>
          <td>
            <div class="num-btn orange-btn">÷</div>
          </td>
        </tr>
        <tr>
          <td>
            <div class="num-btn">7</div>
          </td>
          <td>
            <div class="num-btn">8</div>
          </td>
          <td>
            <div class="num-btn">9</div>
          </td>
          <td>
            <div class="num-btn orange-btn">×</div>
          </td>
        </tr>
        <tr>
          <td>
            <div class="num-btn">4</div>
          </td>
          <td>
            <div class="num-btn">5</div>
          </td>
          <td>
            <div class="num-btn">6</div>
          </td>
          <td>
            <div class="num-btn orange-btn">-</div>
          </td>
        </tr>
        <tr>
          <td>
            <div class="num-btn">1</div>
          </td>
          <td>
            <div class="num-btn">2</div>
          </td>
          <td>
            <div class="num-btn">3</div>
          </td>
          <td>
            <div class="num-btn orange-btn">+</div>
          </td>
        </tr>
        <tr>
          <td class="td-left">
            <div class="num-btn zero-left">0</div>
          </td>
          <td class="td-right">
            <div class="num-btn zero-right"></div>
          </td>
          <td>
            <div class="num-btn">.</div>
          </td>
          <td>
            <div class="num-btn orange-btn">=</div>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
defineProps({
  title: String,
});

// 計算結果
const calResults = ref(10);

// 歸零
function acBtn() {
  calResults.value = 0;
}

// 正負切換
function posNegSwitching(){
  calResults.value = calResults.value * -1;
}
</script>

<style lang="sass" scoped>
.calculator
  border: 1px solid gray
  width: 100%
  max-width: 390px
  padding: 20px 0
  background-color: black
  // .field-input
  //   font-size: 2rem
  //   text-align: right
  //   box-sizing: border-box
  //   -webkit-box-sizing: border-box
  //   -moz-box-sizing: border-box
  //   width: 100%
  .num-display-area
    color: white
    font-size: 4rem
    text-align: right
    padding-right: 20px
  .table
    min-width: 324px
    tr
      border-color: black
      td
        width: 25%
        padding-top: 10px
        padding-bottom: 10px
        .num-btn
          background-color: #333333
          color: white
          font-size: 2rem
          width: 60px
          height: 60px
          line-height: 60px
          margin: 0 auto
          border-radius: 90px
          cursor: pointer
          padding: 0
          user-select: none
        .gray-btn
          background-color: #a5a5a5
          color: black
          line-height: 60px
          font-size: 1.5rem
        .orange-btn
          background-color: #fe9f0a
          color: white
          line-height: 52px
          font-size: 2.5rem
        .zero-left
          width: 70px
          border-radius: 90px 0 0 90px
          margin-left: 2.5px
          margin-right: 0
        .zero-right
          width: 70px
          border-radius: 0 90px 90px 0
          margin-left: 0
      .td-left
        padding-left: 0.5rem
        padding-right: 0
      .td-right
        padding-right: 0.5rem
        padding-left: 0
</style>
