<template>
  <div>
    <h1 @click="testOne">{{ title }}</h1>
    <div class="calculator">
      <!-- 計算結果 -->
      <div class="num-display-area">
        <div v-if="calResults < 1">
          {{ calResults || "0" }}
        </div>
        <div v-else>
          {{ parseFloat(calResults || "0" ).toLocaleString() }}
        </div>
      </div>

      <table class="table">
        <tr>
          <td>
            <div class="num-btn gray-btn" @click="clear">AC</div>
          </td>
          <td>
            <div class="num-btn gray-btn" @click="sign">+/-</div>
          </td>
          <td>
            <div class="num-btn gray-btn" @click="percent">%</div>
          </td>
          <td>
            <div
              @click="divide"
              tabindex="0"
              hidefocus="true"
              class="num-btn orange-btn"
            >
              ÷
            </div>
          </td>
        </tr>
        <tr>
          <!-- 7 到 9 -->
          <td v-for="(item, index) in numRowSeven" :key="index">
            <div @click="getNumber(item)" class="num-btn">{{ item }}</div>
          </td>
          <td>
            <div
              @click="times"
              tabindex="0"
              hidefocus="true"
              class="num-btn orange-btn"
            >
              ×
            </div>
          </td>
        </tr>
        <tr>
          <!-- 4 到 6 -->
          <td v-for="(item, index) in numRowFour" :key="index">
            <div @click="getNumber(item)" class="num-btn">{{ item }}</div>
          </td>
          <td>
            <div
              @click="minus"
              tabindex="0"
              hidefocus="true"
              class="num-btn orange-btn"
            >
              -
            </div>
          </td>
        </tr>
        <tr>
          <!-- 1 到 3 -->
          <td v-for="(item, index) in numRowOne" :key="index">
            <div @click="getNumber(item)" class="num-btn">{{ item }}</div>
          </td>
          <td>
            <div
              @click="add"
              tabindex="0"
              hidefocus="true"
              class="num-btn orange-btn"
            >
              +
            </div>
          </td>
        </tr>
        <tr>
          <td colspan="2">
            <div @click="getNumber('0')" class="num-btn zero-btn">0</div>
          </td>
          <td>
            <div @click="getDot" class="num-btn">.</div>
          </td>
          <td>
            <div @click="equal" class="num-btn equal-sign">=</div>
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

const numRowSeven = ref(["7", "8", "9"]);
const numRowFour = ref(["4", "5", "6"]);
const numRowOne = ref(["1", "2", "3"]);

function testOne() {
  console.log(calResults.value);
  console.log(previousValue.value);
}

// 計算結果
const calResults = ref("");

// 計算開始前紀錄目前數值
const previousValue = ref(null);

// 連續點擊的時候
const preNumber = ref("");
const continuousCal = ref(false);

// 判斷是否要進行計算
const operatorClick = ref(false);

// 計算通式
const operator = ref(null);

// 歸零
function clear() {
  calResults.value = "";
  preNumber.value = "";
  previousValue.value = null;
}

// 正負切換
function sign() {
  calResults.value =
    calResults.value[0] === "-"
      ? calResults.value.slice(1)
      : `-${calResults.value}`;
}

// 百分比
function percent() {
  calResults.value = `${parseFloat(calResults.value) / 100}`;
}

// 一般數字鍵
function getNumber(number) {
  if (operatorClick.value && !continuousCal.value) {
    calResults.value = "";
    operatorClick.value = false;
  }
  calResults.value = `${calResults.value}${number}`;
}

// 增加小數點
function getDot() {
  if (calResults.value.indexOf(".") === -1) {
    calResults.value = calResults.value + ".";
  }
}

// 計算分割
function setPrevious() {
  previousValue.value = calResults.value;
  operatorClick.value = true;
  continuousCal.value = false;
}

// 除法 divide
function divide() {
  operator.value = (a, b) => b / a;
  setPrevious();
}
// 乘法 times
function times() {
  operator.value = (a, b) => a * b;
  setPrevious();
}
// 減法 minus
function minus() {
  operator.value = (a, b) => (b - a);
  setPrevious();
}
// 加法 add
function add() {
  operator.value = (a, b) => a + b;
  setPrevious();
}

// 計算結果
function equal() {
  if (operator.value !== null && !continuousCal.value) {
    calResults.value = `${operator.value(
      parseFloat(calResults.value),
      parseFloat(previousValue.value)
    )}`;
  
    previousValue.value = calResults.value;
    continuousCal.value = true;
  }
}
</script>

<style lang="sass" scoped>
h1
  cursor: pointer
  user-select: none
.calculator
  border: 1px solid gray
  width: 100%
  max-width: 390px
  padding: 20px 0
  background-color: black
  .num-display-area
    color: white
    font-size: 3rem
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
        .num-btn:active
          background-color: #6b6b6b
        .gray-btn
          background-color: #a5a5a5
          color: black
          line-height: 60px
          font-size: 1.5rem
        .gray-btn:active
          background-color: #d9d9d9
        .orange-btn
          background-color: #fe9f0a
          color: white
          line-height: 52px
          font-size: 2.5rem
        .orange-btn:focus
          background-color: white
          color: #fe9f0a
        .zero-btn
          width: 139px
          margin: 0 auto
          text-align: left
          padding-left: 20px
        .equal-sign
          background-color: #fe9f0a
          color: white
          line-height: 52px
          font-size: 2.5rem
        .equal-sign:active
          background-color: #fbc88e
          color: white
</style>
