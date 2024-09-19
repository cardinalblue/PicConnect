<script setup lang="ts">
import confetti from 'canvas-confetti'
import peopleMap from '@/public/peopleMap.json'

interface PeopleObject {
  name: string
  title: string
  fact?: string
}

const peopleRecord: Record<string, PeopleObject> = peopleMap as Record<string, any>
const nameArray = ref(Object.keys(peopleRecord).filter(name => peopleRecord[name].fact))
const allJobTitle = new Set(Object.values(peopleRecord).map(obj => obj.title))
const randomValue = ref(-1)

const selectedPerson = computed(() => peopleRecord[nameArray.value[randomValue.value]])
const leftOption = ref('')
const rightOption = ref('')

// swiping
const showPic = ref(true)

function selectNewPerson() {
  if (randomValue.value !== -1) 
    nameArray.value = nameArray.value.filter((_, idx) => idx !== randomValue.value)
  
  randomValue.value = Math.floor(Math.random() * (nameArray.value.length - 1))
  if (randomValue.value < 0) 
    return
  
  const selectedPersonName = nameArray.value[randomValue.value]

  const correctOption = peopleRecord[selectedPersonName].title

  const shuffledArray = Array.from(allJobTitle.values()).filter(title => title !== correctOption).sort(() => 0.5 - Math.random())
  const otherOption = shuffledArray[0]

  const options = [correctOption, otherOption].sort(() => 0.5 - Math.random())
  leftOption.value = options[0]
  rightOption.value = options[1]
  showPic.value = true
}

function onSelectOption(option: string) {
  if (option === selectedPerson.value.title) 
    confetti()

  showPic.value = false
  //selectNewPerson()
}

onMounted(() => {
  selectNewPerson()
})
</script>

<template>
  <div class="w-screen h-screen">
    <h1>Pic Connect </h1>
    <!--
    <template v-for="(people, nameKey) in peopleRecord" :key="people.name">
      <div>
        <img :src="`people_images/${nameKey}.png`" :width="200" :height="200">
        {{ people.name }}
        {{ people.title }}
      </div>
    </template>
    -->
    <div v-if="nameArray.length === 0" class="text-3xl p-4">
      You have seen all the members! yay
    </div>
    <div v-else-if="randomValue >= 0" class="w-full flex flex-col gap-4 items-center overflow-hidden">
      <div class="relative px-4 flex justify-center h-96 w-full">
        <Transition name="rotate">
          <div v-if="showPic" class="w-full h-96 flex justify-center items-center">
            <img :src="`pd/${nameArray[randomValue]}/thumbnail.png`" class="md:hidden object-contain h-96 w-full" :alt="selectedPerson.name" height="100%">
            <img :src="`pd/${nameArray[randomValue]}/thumbnail.png`" class="hidden md:block object-contain h-96 w-full" :alt="selectedPerson.name">
          </div>
          <div v-else class="absolute flex items-center justify-center px-4 w-full h-full">
            <div class="card bg-white w-full md:w-96 h-full md:h-96 text-black p-4">
              <div class="text-2xl font-bold">
                {{ selectedPerson.name }}
              </div>
              <div>
                {{ selectedPerson.title }}
              </div>
            </div>
          </div>
        </Transition>
      </div>
      <div class="flex flex-col md:flex-row justify-between gap-2 md:gap-8 max-w-full">
        <UButton color="red" class="w-80" :onclick="() => onSelectOption(leftOption)">
          {{ leftOption }} 
        </UButton>
        <UButton color="blue" class="w-80" :onclick="() => onSelectOption(rightOption)">
          {{ rightOption }}
        </UButton>
      </div>
      <UButton v-if="!showPic" color="primary" class="w-40" :onclick="() => selectNewPerson()">
        Next
      </UButton>
    </div>
  </div>
</template>

<style scoped>
.rotate-enter-active {
  transition: all 0.5s linear;
  transition-delay: 0.5s;
}

.rotate-leave-active {
  transition: all 0.5s linear;
}

.rotate-enter-from,
.rotate-leave-to {
  transform: rotateY(-90deg);
}
.rotate-enter-to,
.rotate-leave-from {
  transform: rotateY(0deg);
}
</style>
