<script setup lang="ts">
const isOpen = defineModel<boolean>()
</script>

<template>
  <div>
    <UModal v-model="isOpen" class="overflow-hidden items-center justify-center" :ui="{ container: 'justify-center items-center', background: 'bg-transparent', shadow: 'shadow-none' }">
      <slot />
    </UModal>
  </div>
</template>


