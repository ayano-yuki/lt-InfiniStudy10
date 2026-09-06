<script setup>
import * as constants from '@//constants';

defineProps({
  mainTitle: String,
  subTitle: String,
  image: String,
});
</script>

<template>
  <div class="cover">
    <div class="cover-heading">
      <h1>{{ mainTitle }}</h1>
      <h2 v-if="subTitle">{{ subTitle }}</h2>
    </div>

    <img v-if="image" :src="image" :alt="mainTitle" class="cover-image" />

    <div class="cover-footer">
      <slot />
    </div>
  </div>
</template>

<style>
.cover {
  position: relative;
  width: 100%;
  height: 100%;
  background: v-bind("constants.COLOR_PRIMARY") !important;
  color: v-bind("constants.COLOR_COVER");
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
  text-align: center;
}

.cover-heading {
  position: absolute;
  inset: 0 0 auto;
  padding: 4rem;
}

/* Title */
.cover h1 {
  margin: 0;
  font-size: v-bind("constants.FONT_SIZE_XL") !important;
}

.cover h2 {
  margin: 0;
  font-size: v-bind("constants.FONT_SIZE_M") !important;
}

/* Center image */
.cover-image {
  position: absolute;
  top: 58%;
  left: 50%;
  max-width: 20%;
  object-fit: contain;
  transform: translate(-50%, -50%);
}

.cover-footer {
  position: absolute;
  inset: auto 0 0;
  padding: 2rem;
}
</style>
