<template>
  <div>
    <section class="mt-3">
      <h2 class="text-gray-500">Top</h2>

      <div class="flex gap-4">
        <section>
          <input id="topHour" type="checkbox" v-model="controls.supportTopHour">
          <label for="topHour" class="ml-1">Hour</label>
        </section>
          
        <section>
          <input id="topMinute" type="checkbox" v-model="controls.supportTopMinute">
          <label for="topMinute" class="ml-1">Minute</label>
        </section>
          
        <section>
          <input id="topSecond" type="checkbox" v-model="controls.supportTopSecond">
          <label for="topSecond" class="ml-1">Second</label>
        </section>
      </div>
    </section>

    <section class="mt-3">
      <h2 class="text-gray-500">Bottom</h2>

      <div class="flex gap-4">
        <section>        
          <input id="bottomHour" type="checkbox" v-model="controls.supportBottomHour">
          <label for="bottomHour" class="ml-1">Hour</label>
        </section>

        <section>
          <input id="bottomMinute" type="checkbox" v-model="controls.supportBottomMinute">
          <label for="bottomMinute" class="ml-1">Minute</label>
        </section>

        <section>
          <input id="bottomSecond" type="checkbox" v-model="controls.supportBottomSecond">
          <label for="bottomSecond" class="ml-1">Second</label>
        </section>
      </div>
    </section>
  </div>
</template>

<script>
import { controls } from '../state/controls'

export default {
  setup() {
    return {
      controls
    }
  },
}
</script>