<template>
  <div>
    <h1 @click="testOne">{{ title }}</h1>
    <div class="calculator">
      <!-- 靈動島 -->
      <div class="dynamic-island"></div>
      <!-- 計算結果 -->
      <div class="num-display-area">
        <div v-if="parseFloat(calResults) < 1 && parseFloat(calResults) !== 0">
          {{ Number(parseFloat(calResults || "0").toPrecision(4)) }}
        </div>
        <div v-if="parseFloat(calResults) > 1">
          {{ maxNumber(parseFloat(calResults || "0")) }}
        </div>
        <div
          v-if="
            parseFloat(calResults) === 0 ||
            calResults === '' ||
            parseFloat(calResults) === 1
          "
        >
          {{ calResults || "0" }}
        </div>
      </div>

      <table class="table">
        <tr>
          <td>
            <div class="num-btn gray-btn" @click="clear">
              {{ acSwitching ? "AC" : "C" }}
            </div>
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
      <!-- 返回 -->
      <div class="home-bar"></div>
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
  console.log("b", previousValue.value);
  console.log("a", calResults.value);
}

// 計算結果
const calResults = ref("");

const operatorWay = ref("");

const acSwitching = ref(true);

// 限制最大數值
function maxNumber(num) {
  if (num > 999999999) {
    num = num.toExponential(2);
    return num;
  }
  return num.toLocaleString();
}

// 計算開始前紀錄目前數值
const previousValue = ref(null);

// 連續點擊的時候
const preNumber = ref("");

// false可以連續輸入 true不行
const canNotString = ref(false);

// 判斷是否要進行計算
const operatorClick = ref(false);

// 計算通式
const operator = ref(null);

// 歸零
function clear() {
  calResults.value = "";
  preNumber.value = "";
  previousValue.value = null;
  operatorWay.value = "";
  canNotString.value = false;
  acSwitching.value = true;
}

// 正負切換
function sign() {
  if (calResults.value === "") {
    return (calResults.value = "-0");
  }
  calResults.value =
    calResults.value[0] === "-"
      ? calResults.value.slice(1)
      : `-${calResults.value}`;
}

// 百分比
function percent() {
  if (calResults.value === "" || calResults.value === "0") {
    return;
  }
  calResults.value = `${parseFloat(calResults.value) / 100}`;
}

// 一般數字鍵
function getNumber(number) {
  acSwitching.value = false;
  if (operatorClick.value && !canNotString.value) {
    calResults.value = "";
    operatorClick.value = false;
    operatorWay.value = "";
  }

  if (canNotString.value) {
    calResults.value = "";
    operatorClick.value = false;
  }

  calResults.value = `${calResults.value}${number}`;
  preNumber.value = calResults.value;
  canNotString.value = false;
}

// 增加小數點
function getDot() {
  acSwitching.value = false;
  if (calResults.value === "" || calResults.value === "0") {
    calResults.value = "0.";
  }
  if (calResults.value.indexOf(".") === -1) {
    calResults.value = calResults.value + ".";
  }
}

// 計算分割
function setPrevious() {
  previousValue.value = calResults.value;
  operatorClick.value = true;
  canNotString.value = false;
}

// 除法 divide
function divide() {
  operatorWay.value = "除法";
  if (calResults.value === "" || calResults.value === "0") {
    return;
  }
  operator.value = (a, b) => b / a;
  setPrevious();
}
// 乘法 times
function times() {
  operatorWay.value = "乘法";
  if (calResults.value === "" || calResults.value === "0") {
    return;
  }
  operator.value = (a, b) => b * a;
  setPrevious();
}
// 減法 minus
function minus() {
  operatorWay.value = "減法";
  if (calResults.value === "" || calResults.value === "0") {
    return;
  }
  operator.value = (a, b) => b - a;
  setPrevious();
}
// 加法 add
function add() {
  operatorWay.value = "加法";
  if (calResults.value === "" || calResults.value === "0") {
    return;
  }
  operator.value = (a, b) => b + a;
  setPrevious();
}

// 計算結果
function equal() {
  if (calResults.value === "" || calResults.value === "0" || previousValue === null) {
    return;
  }
  if (calResults.value === "0." || calResults.value === "-0") {
    calResults.value = "";
    return;
  }
  if (operator.value !== null && !canNotString.value && previousValue.value) {
    calResults.value = `${operator.value(
      // a
      parseFloat(calResults.value),
      // b
      parseFloat(previousValue.value)
    )}`;

    canNotString.value = true;
  } else {
    calResults.value = `${operator.value(
      // a
      parseFloat(preNumber.value),
      // b
      parseFloat(calResults.value)
    )}`;

    canNotString.value = true;
  }
}
</script>

<style lang="sass" scoped>
h1
  cursor: pointer
  user-select: none
.calculator
  border: 1px solid gray
  border-radius: 27px
  width: 100%
  max-width: 390px
  padding: 70px 0 20px
  background-color: black
  position: relative
  .dynamic-island
    width: 30%
    height: 23px
    border: 1px solid gray
    border-radius: 30px
    position: absolute
    top: 20px
    left: 50%
    transform: translate(-50%, -50%)
    opacity: 0.5
    transition: opacity 0.2s linear

  .dynamic-island:hover
    opacity: 1
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
          font-weight: 600
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
  .home-bar
    height: 4px
    width: 132px
    border: 1px solid white
    background-color: white
    border-radius: 100px
    position: absolute
    bottom: 6px
    left: 50%
    transform: translate(-50%, -50%)
    opacity: 0.7
</style>
