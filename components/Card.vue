<script setup lang="ts">
defineProps<{
  name: string
  title: string
  fact: string
  imageSrc: string
  displayStyle: 'ALL' | 'FACT'
}>()
</script>

<template>
  <CardContainer class="bg-white pt-10 pb-4 h-full shadow-[10px_10px]">
    <div v-if="displayStyle === 'ALL'" class="text-2xl">
      <div class="text-lg pb-2">
        {{ name }} is a
      </div>
      <div class="text-3xl roboto-black font-extrabold">
        {{ title }}
      </div>
    </div>
    <div>
      <div
        class="text-lg pb-2"
      >
        And {{ name }} also like to
      </div>
      <div class="text-3xl roboto-black font-extrabold">
        {{ fact }}
      </div>
    </div>
    <div class="w-full flex items-center gap-2">
      <Image :src="imageSrc" alt="image" img-class="w-24 h-24" />
      <div class="flex-1 flex flex-col items-center justify-between">
        <span class="text-xl font-bold"> {{ name }} </span>
        <span> {{ displayStyle === 'ALL' ? 'Go Say Hi! 🤩👋' : title }} </span>
      </div>
    </div>
  </CardContainer>
</template>
