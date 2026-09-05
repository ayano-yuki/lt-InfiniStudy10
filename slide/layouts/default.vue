<script setup>
import { computed } from 'vue';
import { useSlideContext } from '@slidev/client';
import { COLOR_BLACK, COLOR_PRIMARY, COLOR_WHITE } from './constants';

const { $frontmatter } = useSlideContext();

const pageTitle = computed(() => {
  return $frontmatter.title || $frontmatter.pageTitle || $frontmatter.header || '';
});
</script>

<template>
  <div class="default">
    <h1 v-if="pageTitle" class="default-title">{{ pageTitle }}</h1>
    <div class="default-slot">
      <slot />
    </div>
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

.default-title {
  position: relative;
  margin: 0 0 1rem;
  padding-bottom: 5px;
  color: v-bind("COLOR_PRIMARY");
  font-size: 1.5rem;
  font-weight: 900;
}

.default-title::after {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background-color: v-bind("COLOR_PRIMARY");
  content: "";
}

.default-slot {
  width: 100%;
  height: 100%;
}

/* Body */
.default-slot p {
  margin: 0 0 0.5rem;
  color: v-bind("COLOR_BLACK");
  font-size: 1.25rem;
  font-weight: 600;
  line-height: 1.5;
}

.default-slot ul,
.default-slot ol {
  display: block !important;
  margin: 0 0 0.5rem;
  color: v-bind("COLOR_BLACK");
  font-size: 1.25rem !important;
  font-weight: 600;
  line-height: 1.5;
  list-style-position: outside !important;
}

.default-slot ul {
  padding-left: 1.5rem !important;
  list-style-type: disc !important;
}

.default-slot ol {
  padding-left: 1.75rem !important;
  list-style-type: decimal !important;
}

.default-slot ul ul {
  margin-top: 0.25rem;
  padding-left: 1.5rem !important;
  list-style-type: circle !important;
}

.default-slot ul ul ul {
  list-style-type: square !important;
}

.default-slot ul ul ul ul {
  list-style-type: disc !important;
}

.default-slot ol ol {
  margin-top: 0.25rem;
  padding-left: 1.75rem !important;
  list-style-type: lower-alpha !important;
}

.default-slot ol ol ol {
  list-style-type: lower-roman !important;
}

.default-slot li {
  display: list-item !important;
  margin: 0 0 0.5rem;
  padding-left: 0.25rem;
}

.default-slot li > ul,
.default-slot li > ol {
  margin-top: 0.25rem;
  margin-bottom: 0.25rem;
}

.default-slot > :first-child {
  margin-top: 0;
}

.default-slot > :last-child {
  margin-bottom: 0;
}
</style>
