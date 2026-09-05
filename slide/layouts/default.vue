<script setup>
import { COLOR_BLACK, COLOR_PRIMARY, COLOR_WHITE } from './constants';
</script>

<template>
  <div class="default">
    <slot />
  </div>
</template>

<style>
.default {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding: 1rem 2rem !important;
  background: v-bind("COLOR_WHITE") !important;
}

/* Heading */
.default h1 {
  position: relative;
  margin: 0 0 1rem;
  padding-bottom: 5px;
  color: v-bind("COLOR_PRIMARY");
  font-size: 1.5rem;
  font-weight: 900;
}

.default h1::after {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-color: v-bind("COLOR_PRIMARY");
  content: "";
}

/* Body */
.default p {
  margin: 0;
  color: v-bind("COLOR_BLACK");
  font-size: 1.25rem;
  font-weight: 600;
}
</style>
