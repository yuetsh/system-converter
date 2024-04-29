<template>
  <div>
    <div>{{ label }}是 {{ result }}</div>
    <div>当前是 {{ current }}</div>
  </div>
  <div>
    <input type="number" v-model="num" />
    <select name="system" v-model="system">
      <option value="2">2</option>
      <option value="8">8</option>
      <option value="16">16</option>
    </select>
    <button @click="start">开始转换</button>
    <button @click="clear">清空</button>
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
</script>
