<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
    </div>
  </header>

  <main>
    <button @click="isSecond = !isSecond">Switch</button>
    <FirstChart v-if="!isSecond"/>
    <SecondChart v-else/>
  </main>
</template>
<script setup>
import FirstChart from './components/FirstChart.vue'
import SecondChart from './components/SecondChart.vue';
import { ref } from 'vue'
const isSecond = ref(false)
</script>
<style scoped>
</style>
