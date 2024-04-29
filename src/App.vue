<template>
  <div class="container vertical-center">
    <div class="block mb-6">
      <div class="title is-size-1">{{ label }}是 {{ result }}</div>
      <div class="title is-size-2">当前十进制是 {{ current }}</div>
    </div>
    <div class="columns mb-6">
      <div class="column">
        <label class="label is-large">十进制数字</label>
        <div class="buttons">
          <input class="input is-large" type="number" min="0" v-model="num" />
          <button class="button is-large" @click="add">+</button>
          <button class="button is-large" @click="minus">-</button>
        </div>
      </div>
      <div class="column">
        <label class="label is-large">选择进制</label>
        <div class="select is-large">
          <select name="system" v-model="system">
            <option value="2">2</option>
            <option value="8">8</option>
            <option value="16">16</option>
          </select>
        </div>
      </div>
    </div>
    <div class="buttons">
      <button class="button is-primary is-large" @click="start">转换</button>
      <button class="button is-large" @click="clear">清空</button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { computed, ref, watch } from "vue"

const num = ref(0)
const result = ref("0")
const current = ref(0)
const timer = ref(0)
const system = ref("2")

watch(system, () => {
  if (timer.value) clearInterval(timer.value)
  current.value = 0
  result.value = "0"
})

const label = computed(() => {
  if (system.value === "2") return "二进制"
  else if (system.value === "8") return "八进制"
  else return "十六进制"
})

function start() {
  if (timer.value) clearInterval(timer.value)
  timer.value = setInterval(() => {
    if (current.value >= num.value) {
      clearInterval(timer.value)
      return
    }
    current.value += 1
    result.value = Number(current.value)
      .toString(parseInt(system.value))
      .toUpperCase()
  }, 1000)
}

function clear() {
  if (timer.value) clearInterval(timer.value)
  num.value = 0
  current.value = 0
  result.value = "0"
}

function add() {
  num.value += 1
}

function minus() {
  if (num.value > 0 && num.value > current.value) {
    num.value -= 1
  }
}
</script>
<style scoped>
@font-face {
  font-family: "MyFont";
  src: url(/font.ttf);
}

* {
  font-family: "MyFont";
  box-sizing: border-box;
}

.container {
  max-width: 80%;
}

.vertical-center {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
