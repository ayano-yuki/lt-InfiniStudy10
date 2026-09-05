<script setup>
import { COLOR_PRIMARY, COLOR_WHITE } from './constants';
</script>

<template>
  <div class="cover">
    <slot />
  </div>
</template>

<style>
.cover {
  position: relative;
  width: 100%;
  height: 100%;
  background: v-bind("COLOR_PRIMARY") !important;
  color: v-bind("COLOR_WHITE");
  font-weight: 700;
  text-align: center;
}

/* Title */
.cover h1 {
  margin: 0;
  font-size: 3rem !important;
}

.cover h2 {
  margin: 0;
  font-size: 1.5rem !important;
}

/* Center image */
.cover img {
  position: absolute;
  top: 55%;
  left: 50%;
  max-width: 20%;
  object-fit: contain;
  transform: translate(-50%, -50%);
}
</style>
