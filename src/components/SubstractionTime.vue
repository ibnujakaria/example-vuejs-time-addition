<template>
  <div>
    <h1>Substraction Components</h1>

    <table>
      <thead>
        <tr>
          <th v-if="hourColumn">Hour</th>
          <th v-if="minuteColumn">Minute</th>
          <th v-if="secondColumn">Second</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td v-if="hourColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.top.hour"
              :disabled="!controls.supportTopHour"
            >
          </td>
          <td v-if="minuteColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.top.minute"
              :disabled="!controls.supportTopMinute"
            >
          </td>
          <td v-if="secondColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.top.second"
              :disabled="!controls.supportTopSecond"
            >
          </td>
        </tr>
        <tr>
          <td v-if="hourColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.bottom.hour"
              :disabled="!controls.supportBottomHour"
            >
          </td>
          <td v-if="minuteColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.bottom.minute"
              :disabled="!controls.supportBottomMinute"
            >
          </td>
          <td v-if="secondColumn">
            <input
              type="number"
              class="w-12"
              v-model="substraction.bottom.second"
              :disabled="!controls.supportBottomSecond"
            >
          </td>
        </tr>
        <tr>
          <td colspan="3" class="py-3 row-tabel relative">
            <hr>
          </td>
        </tr>
        <tr>
          <td v-if="hourColumn" class="align-top">
            <input
              type="number"
              class="w-12"
              :class="{
                'border-green-500 border-2 text-green-500': substraction.answer.hour === correctHour.result,
              }"
              v-model="substraction.answer.hour"
            >

            <div
              v-if="substraction.answer.hour !== null && substraction.answer.hour !== correctHour.result"
              class="text-red-500"
            >
              {{ correctHour.result }}
            </div>
          </td>
          <td v-if="minuteColumn" class="align-top">
            <input
              type="number"
              class="w-12"
              :class="{
                'border-green-500 border-2 text-green-500': substraction.answer.minute === correctMinute.result,
              }"
              v-model="substraction.answer.minute"
            >

            <div
              v-if="substraction.answer.minute !== null && substraction.answer.minute !== correctMinute.result"
              class="text-red-500"
            >
              {{ correctMinute.result }}
            </div>
          </td>
          <td v-if="secondColumn" class="align-top">
            <input
              type="number"
              class="w-12"
              :class="{
                'border-green-500 border-2 text-green-500': substraction.answer.second === correctSecond.result,
              }"
              v-model="substraction.answer.second"
            >
            
            <div
              v-if="substraction.answer.second !== null && substraction.answer.second !== correctSecond.result"
              class="text-red-500"
            >
              {{ correctSecond.result }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- <pre>{{ substraction }}</pre>
    <pre>{{ ({
      correctSecond,
      correctMinute,
      correctHour,
    }) }}</pre> -->
  </div>
</template>

<script>
import { computed, reactive } from '@vue/reactivity'
import { controls } from '../state/controls'

export default {
  setup () {
    const hourColumn = computed(() => controls.supportTopHour || controls.supportBottomHour)
    const minuteColumn = computed(() => controls.supportTopMinute || controls.supportBottomMinute)
    const secondColumn = computed(() => controls.supportTopSecond || controls.supportBottomSesupportTopSecond)

    const substraction = reactive({
      top: {
        hour: 10,
        minute: 20,
        second: 30,
      },
      bottom: {
        hour: 10,
        minute: 10,
        second: 5,
      },
      answer: {
        hour: null,
        minute: null,
        second: null,
      }
    })

    const correctSecond = computed(() => {
      const result = substraction.top.second + substraction.bottom.second

      return {
        result: result > 60 ? result - 60 : result,
        addition: result > 60 ? Math.floor(result / 60) : 0,
      }
    })

    const correctMinute = computed(() => {
      const result = substraction.top.minute + substraction.bottom.minute + (correctSecond.value.addition || 0)

      return {
        result: result > 60 ? result - 60 : result,
        addition: result > 60 ? Math.floor(result / 60) : 0,
      }
    })

    const correctHour = computed(() => {
      const result = substraction.top.hour + substraction.bottom.hour + (correctMinute.value.addition || 0)

      return {
        result: result > 60 ? result - 60 : result,
        addition: result > 60 ? Math.floor(result / 60) : 0,
      }
    })

    return {
      controls,
      hourColumn,
      minuteColumn,
      secondColumn,
      substraction,
      correctSecond,
      correctMinute,
      correctHour,
    }
  }
}
</script>

<style scoped>
.row-tabel::after {
  content: "+";
  position: absolute;
  right: -15px;
  top: 0px;
}
</style>