<script setup>
import * as constants from '@//constants';

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
  background: v-bind("constants.COLOR_PRIMARY") !important;
  color: v-bind("constants.COLOR_WHITE");
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
  font-size: v-bind("constants.FONT_SIZE_XL");
  text-align: center;
}
</style>
