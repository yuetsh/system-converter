<template>
  <div class="container vertical-center">
    <div class="block mb-6">
      <div class="title is-size-1">
        <span class="pr-4">转换成{{ systemLabel }}是</span>
        <span class="has-text-danger">{{ result }}</span>
      </div>
      <div class="title is-size-2">
        <span class="pr-4">（当前的十进制是</span>
        <span class="has-text-warning">{{ current }}</span>
        <span>）</span>
      </div>
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
    <div class="buttons are-large">
      <button class="button is-primary" @click="start">
        {{ buttonLabel }}
      </button>
      <button class="button" @click="clear">清空</button>
    </div>
  </div>
</template>
<script setup lang="ts">
import { computed, ref, watch } from "vue"

enum State {
  PREPARE,
  UNDERDOING,
  PENDING,
  DONE,
}

const num = ref(0)
const result = ref("0")
const current = ref(0)
const system = ref("2")
const state = ref(State.PREPARE)

let timer = 0

function convert() {
  result.value = Number(current.value)
    .toString(parseInt(system.value))
    .toUpperCase()
}

watch(system, () => {
  state.value = State.PREPARE
  if (timer) clearInterval(timer)
  convert()
})

const buttonLabel = computed(() => {
  if (state.value === State.DONE) return "转换"
  if (state.value === State.PENDING) return "继续"
  if (state.value === State.PREPARE) return "转换"
  if (state.value === State.UNDERDOING) return "暂停"
})

const systemLabel = computed(() => {
  if (system.value === "2") return "二进制"
  else if (system.value === "8") return "八进制"
  else return "十六进制"
})

function start() {
  if (state.value === State.UNDERDOING) {
    clearInterval(timer)
    state.value = State.PENDING
    return
  }
  state.value = State.UNDERDOING
  timer = setInterval(() => {
    if (current.value >= num.value) {
      clearInterval(timer)
      state.value = State.DONE
      return
    }
    current.value += 1
    convert()
  }, 1000)
}

function clear() {
  if (timer) clearInterval(timer)
  num.value = 0
  current.value = 0
  result.value = "0"
  state.value = State.PREPARE
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
