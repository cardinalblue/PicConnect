<script setup lang="ts">
defineProps<{
  src: string
  imgClass?: string
  alt?: string
}>()
</script>

<template>
  <div class=" overflow-hidden rounded-3xl border border-solid border-black">
    <img :src="src" :alt="alt || 'image'" :class="imgClass">
  </div>
</template>
