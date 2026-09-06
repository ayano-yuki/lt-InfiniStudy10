<script setup lang="ts">
import * as constants from '@//constants';

defineProps<{
  icon: string
  label: string
  account: string
  href?: string
}>()
</script>

<template>
  <component
    :is="href ? 'a' : 'span'"
    class="social-link"
    :href="href"
    :aria-label="`${label}: ${account}`"
    target="_blank"
    rel="noopener noreferrer"
  >
    <img :src="icon" :alt="`${label} icon`" />
    <span aria-hidden="true">：</span>
    <code>{{ account }}</code>
  </component>
</template>

<style scoped>
.social-link {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  color: inherit;
  text-decoration: none;
}

.social-link img {
  width: 1.1em;
  height: 1.1em;
  vertical-align: middle;
}

.social-link code {
  font-size: v-bind("constants.FONT_SIZE_S");
}
</style>
