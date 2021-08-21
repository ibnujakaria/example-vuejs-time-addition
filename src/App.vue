<template>
  <div class="container mt-8">
    <h1 class="text-4xl text-center">Example</h1>

    <main class="flex gap-8 justify-center mt-8">
      <controls />
      <substraction-time />
    </main>
  </div>
</template>

<script>
import Controls from './components/Controls.vue'
import SubstractionTime from './components/SubstractionTime.vue'

export default {
  components: {
    Controls,
    SubstractionTime
  }
}
</script>

<style>
@import './App.css';
</style>