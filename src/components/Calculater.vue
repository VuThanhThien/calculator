<script setup lang="ts">
import { ref } from "vue";
const formula = ref<string>("");
const result = ref<number>(0);
const operators = ["+", "-", "x", "÷", "%"];

const operate = (element: string) => {
  // Not allow input symbol before number
  if (!formula.value && operators.includes(element)) {
    return;
  }
  if (operators.includes(formula.value.slice(-1)) && operators.includes(element)) {
    formula.value = formula.value.slice(0, -1) + element;
    return
  }
  formula.value += element;
};
const validate = (str: string) => {
  const multiPatern = /x/gi;
  const devidePatern = /÷/gi;
  return str.replace(multiPatern, "*").replace(devidePatern, "/");
};
const equal = () => {
  const tempResult = validate(formula.value);
  result.value = eval(tempResult);
};
const clear = () => {
  formula.value = "";
  result.value = 0;
};
const percent = () => {
  const tempResult = validate(formula.value) || 0;
  result.value = eval(`0.01 * ${tempResult}`);
};
const toggle = () => {

  if (!formula.value) {
    return
  }
  else if (operators.includes(formula.value.slice(-1))) {
    formula.value = '-0';
  }
  else if (formula.value.startsWith("-")) {
    formula.value = Math.abs(eval(validate(formula.value))).toString();
  }
  else {
    formula.value = `-${Math.abs(eval(validate(formula.value))).toString()}`;
  }
  equal();
};
</script>

<template>
  <div class="calculator">
    <div class="screen">
      <span class="fomula" v-cloak>{{ formula }}</span>
      <span class="result" v-cloak>{{ result }}</span>
    </div>
    <div class="buttons">
      <button @click="clear()">AC</button>
      <button @click="toggle()">±</button>
      <button @click="percent()">%</button>
      <button @click="operate('÷')">÷</button>

      <button @click="operate('7')">7</button>
      <button @click="operate('8')">8</button>
      <button @click="operate('9')">9</button>
      <button @click="operate('x')">x</button>

      <button @click="operate('4')">4</button>
      <button @click="operate('5')">5</button>
      <button @click="operate('6')">6</button>
      <button @click="operate('-')">-</button>

      <button @click="operate('1')">1</button>
      <button @click="operate('2')">2</button>
      <button @click="operate('3')">3</button>
      <button @click="operate('+')">+</button>

      <button @click="operate('0')">0</button>
      <button @click="operate('.')">.</button>
      <button @click="equal()">=</button>
    </div>
  </div>
</template>
<style scoped>
[v-cloak] {
  display: none;
}

.calculator {
  background-color: var(--color-background-body);
  margin: 0;
  width: 350px;
  overflow: hidden;
  justify-content: center;
  align-items: center;
  background-image: linear-gradient(140deg, #3b3e4c, #1e222f);
  padding: 20px;
  border-radius: 40px;
  box-shadow: 50px 150px 150px #2b2e36;
}

.calculator .screen {
  height: 200px;
  color: #bbbbbb;
  font-size: 50px;
  text-align: right;
  display: flex;
  flex-direction: column;
}

.calculator .screen .result {
  color: #fff;
  font-size: 80px;
  overflow: hidden;
}

.calculator .buttons {
  display: grid;
  grid-template-columns: repeat(4, 70px);
  grid-template-rows: repeat(5, 70px);
  gap: 10px;
}

.calculator .buttons button {
  border-radius: 20px;
  border: none;
  font-size: x-large;
  color: #efefef;
  background-image: linear-gradient(140deg, #262b39, #323847);
  box-shadow: inset 5px 5px 5px #323847, inset -5px -5px 20px #323847,
    5px 5px 5px #1d1f25;
}

.calculator .buttons button:nth-child(17) {
  grid-column-start: 1;
  grid-column-end: 3;
}

.calculator .buttons button:nth-child(-n + 3) {
  background-image: linear-gradient(140deg, #3b424d, #5d697a);
  box-shadow: inset 5px 5px 5px #5d697a, inset -5px -5px 20px #5d697a,
    5px 5px 5px #1d1f25;
}

.calculator .buttons button:nth-child(4n),
.calculator .buttons button:nth-child(19) {
  background-image: linear-gradient(140deg, #bc5a17, #e09121);
  box-shadow: inset 5px 5px 5px #e09121, inset -5px -5px 20px #e99725,
    5px 5px 5px #1d1f25;
}

.calculator .buttons button:active {
  box-shadow: none;
}
</style>
