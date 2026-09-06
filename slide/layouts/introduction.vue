<script setup>
import * as constants from '@//constants';

defineProps({
  name: String,
  image: String,
});
</script>

<template>
  <div class="introduction">
    <h1 class="introduction-title">自己紹介</h1>

    <div class="introduction-content">
      <div class="introduction-left">
        <img :src="image" :alt="name" class="introduction-image" />
      </div>

      <div class="introduction-right">
        <div class="introduction-name">
          {{ name }}
        </div>

        <div class="introduction-slot">
          <slot />
        </div>
      </div>
    </div>
  </div>
</template>

<style>
/* Page structure */
.introduction {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding: 1rem 2rem !important;
  background: v-bind("constants.COLOR_BACKGROUND") !important;
}

.introduction-content {
  display: flex;
  flex: 1;
  gap: 2rem;
  min-height: 0;
}

/* Profile image */
.introduction-left {
  display: flex;
  flex: 0 0 30%;
  align-items: center;
  justify-content: center;
  min-width: 0;
}

.introduction-image {
  display: block;
  width: 100%;
  max-width: 100%;
  height: auto;
  max-height: 100%;
  margin-top: -2rem;
  border-radius: 8px;
  object-fit: contain;
}

/* Profile details */
.introduction-right {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: flex-start;
  min-width: 0;
  margin-left: 1rem;
}

.introduction-slot {
  flex: 1;
  min-height: 0;
}

/* Heading */
.introduction-title {
  position: relative;
  flex-shrink: 0;
  margin: 0 0 1rem;
  padding-bottom: 5px;
  color: v-bind("constants.COLOR_PRIMARY");
  font-size: v-bind("constants.FONT_SIZE_M") !important;
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
}

.introduction-title::after {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-color: v-bind("constants.COLOR_PRIMARY");
  content: "";
}

.introduction-name {
  margin: 0 0 2rem;
  color: v-bind("constants.COLOR_TEXT");
  font-size: v-bind("constants.FONT_SIZE_L") !important;
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
}

/* Slot content */
.introduction-slot p {
  margin: 0 0 0.5rem;
  color: v-bind("constants.COLOR_TEXT");
  font-size: v-bind("constants.FONT_SIZE_S") !important;
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
  line-height: v-bind("constants.FONT_LINE_HEIGHT");
}

.introduction-slot ul,
.introduction-slot ol {
  display: block !important;
  margin: 0;
  color: v-bind("constants.COLOR_TEXT");
  font-size: v-bind("constants.FONT_SIZE_S") !important;
  font-weight: v-bind("constants.FONT_WEIGHT_BOLD");
  line-height: v-bind("constants.FONT_LINE_HEIGHT");
  list-style-position: outside !important;
}

.introduction-slot ul {
  padding-left: 1.5rem !important;
  list-style-type: disc !important;
}

.introduction-slot ol {
  padding-left: 1.75rem !important;
  list-style-type: decimal !important;
}

/* Nested list markers */
.introduction-slot ul ul {
  margin-top: 0.25rem;
  padding-left: 1.5rem !important;
  list-style-type: circle !important;
}

.introduction-slot ul ul ul {
  list-style-type: square !important;
}

.introduction-slot ul ul ul ul {
  list-style-type: disc !important;
}

.introduction-slot ol ol {
  margin-top: 0.25rem;
  padding-left: 1.75rem !important;
  list-style-type: lower-alpha !important;
}

.introduction-slot ol ol ol {
  list-style-type: lower-roman !important;
}

.introduction-slot li {
  display: list-item !important;
  margin: 0 0 0.5rem;
  padding-left: 0.25rem;
}

.introduction-slot li > ul,
.introduction-slot li > ol {
  margin-top: 0.25rem;
  margin-bottom: 0.25rem;
}

.introduction-slot > :first-child {
  margin-top: 0;
}

.introduction-slot > :last-child {
  margin-bottom: 0;
}
</style>
