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
  <div class="bg-white w-full h-full rounded-3xl overflow-hidden border border-solid border-black py-10 px-5 flex flex-col justify-between text-center">
    <div v-if="displayStyle === 'ALL'" class="text-2xl font-bold">
      <div class="text-lg pb-2">
        {{ name }} is a
      </div>
      <div class="text-3xl font-bold">
        {{ title }}
      </div>
    </div>
    <div>
      <div
        class="text-lg"
        :class="{
          'pb-2': displayStyle === 'ALL',
          'pb-24': displayStyle === 'FACT',
        }"
      >
        And {{ name }} also like to
      </div>
      <div class="text-3xl font-bold">
        {{ fact }}
      </div>
    </div>
    <div class="w-full flex items-center">
      <Image :src="imageSrc" alt="image" img-class="w-24 h-24" />
      <div class="flex-1 flex flex-col items-center justify-between">
        <span class="text-xl font-bold"> {{ name }} </span>
        <span> {{ displayStyle === 'ALL' ? 'Go Say Hi! 🤩👋' : title }} </span>
      </div>
    </div>
  </div>
</template>
