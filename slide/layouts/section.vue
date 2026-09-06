<script setup>
import { COLOR_PRIMARY, COLOR_WHITE } from './constants';

defineProps({
  message: String,
});
</script>

<template>
  <div class="section">
    <h1 v-if="message">{{ message }}</h1>
  </div>
</template>

<style scoped>
.section {
  position: relative;
  width: 100%;
  height: 100%;
  background: v-bind("COLOR_PRIMARY") !important;
  color: v-bind("COLOR_WHITE");
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 3rem;
  text-align: center;
}
</style>
