<script setup lang="ts">
import UiNetworkCalculator from '../components/UiNetworkCalculator.vue';
</script>

<template>
  <header class="header">
    <h1>Калькулятор подсетей</h1>
  </header>
  <div>
    <UiNetworkCalculator></UiNetworkCalculator>
  </div>
</template>

<style scoped>
.header {
  height:40px;
  background-color:var(--color-back-header);
  text-align: center;
  align-items:center;
  margin-bottom: 20px;
  color: var(--color-text);
  font-family: 'base', sans-serif;
}
</style>