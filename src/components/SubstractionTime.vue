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
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 0
              }"
              v-model="substraction.top.hour"
              :disabled="!controls.supportTopHour"
            >
          </td>
          <td v-if="minuteColumn">
            <input
              type="number"
              class="w-12"
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 1
              }"
              v-model="substraction.top.minute"
              :disabled="!controls.supportTopMinute"
            >
          </td>
          <td v-if="secondColumn">
            <input
              type="number"
              class="w-12"
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 2
              }"
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
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 0
              }"
              v-model="substraction.bottom.hour"
              :disabled="!controls.supportBottomHour"
            >
          </td>
          <td v-if="minuteColumn">
            <input
              type="number"
              class="w-12"
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 1
              }"
              v-model="substraction.bottom.minute"
              :disabled="!controls.supportBottomMinute"
            >
          </td>
          <td v-if="secondColumn">
            <input
              type="number"
              class="w-12"
              :class="{
                'bg-yellow-200 border-dotted border-2 border-black': iColumnActive === 2
              }"
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
              :disabled="iColumnActive < 0"
            >

            <div
              v-if="iColumnActive < 0 && substraction.answer.hour !== correctHour.result"
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
              :disabled="iColumnActive < 0"
            >

            <div
              v-if="iColumnActive < 1 && substraction.answer.minute !== correctMinute.result"
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
              :disabled="iColumnActive < 0"
            >
            
            <div
              v-if="iColumnActive < 2 && substraction.answer.second !== correctSecond.result"
              class="text-red-500"
            >
              {{ correctSecond.result }}
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="flex flex-col">
      <button
        v-if="iColumnActive > -1"
        class="px-4 py-1 bg-blue-500 text-white rounded-md shadow-sm mt-4"
        @click="checkAnswer()"
      >
        Check Answer
      </button>

      <button
        class="px-4 py-1 bg-red-500 text-white rounded-md shadow-sm mt-4"
        @click="reset()"
      >
        Reset
      </button>
    </div>


    <!-- <pre>{{ substraction }}</pre>
    <pre>{{ ({
      correctSecond,
      correctMinute,
      correctHour,
    }) }}</pre> -->
  </div>
</template>

<script>
import { computed, reactive, ref } from '@vue/reactivity'
import { controls } from '../state/controls'

export default {
  setup () {
    const hourColumn = computed(() => controls.supportTopHour || controls.supportBottomHour)
    const minuteColumn = computed(() => controls.supportTopMinute || controls.supportBottomMinute)
    const secondColumn = computed(() => controls.supportTopSecond || controls.supportBottomSesupportTopSecond)

    const iColumnActive = ref(2);

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

    const checkAnswer = () => {
      iColumnActive.value = iColumnActive.value - 1
    }

    const reset = () => {
      substraction.answer = {
        hour: null,
        minute: null,
        second: null,
      }

      iColumnActive.value = 2
    }

    return {
      controls,
      hourColumn,
      minuteColumn,
      secondColumn,
      substraction,
      correctSecond,
      correctMinute,
      correctHour,
      iColumnActive,
      checkAnswer,
      reset,
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