<script setup lang="ts">
defineProps<{
  totalQuestion?: number
  correct?: number
}>()
</script>

<template>
  <div class="w-full h-24 px-6 flex items-center justify-between">
    <div class="font-bold text-2xl caprasimo">
      PicConnect
    </div>
    <div v-if="typeof totalQuestion === 'number' && typeof correct === 'number'" class="text-xl caprasimo text-purple-600">
      {{ correct }} / {{ totalQuestion }}
    </div>
    <UButton class="rounded-full font-bold bg-purple-400 active:bg-purple-500 hover:bg-purple-500 w-14 justify-center" icon="material-symbols:home-outline" size="lg" to="/" />
  </div>
</template>
