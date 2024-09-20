<script setup lang="ts">
import confetti from 'canvas-confetti'
import peopleMap from '@/public/peopleMap.json'

interface PeopleObject {
  name: string
  title: string
  fact?: string
}

// people info
const peopleRecord: Record<string, PeopleObject> = peopleMap as Record<string, any>
const nameArray = ref(Object.keys(peopleRecord).filter(name => peopleRecord[name].fact))
const allJobTitle = new Set(Object.values(peopleRecord).map(obj => obj.title))
const randomValue = ref(-1)
const selectedPerson = ref<PeopleObject>()

// options
const leftOption = ref('')
const rightOption = ref('')

// display checking
const showAllInfo = ref(false)
const hasNotSelectAnswer = ref(true)
const isHistoryModelOpen = ref(false)
const showSpecialEndingImage = ref(false)
const backgroundColor = ref<'default' | 'correct' | 'wrong'>('default')

// for easier use
const imageSrc = computed(() => `pd/${selectedPerson.value?.name.toLowerCase().replaceAll(' ', '') || 'not_selected' }/thumbnail.png`)
const toStandardName = (name: string) => name.toLowerCase().replaceAll(' ', '')

// record for seen members
const visitedMembers = reactive<string[]>([])
function selectNewPerson() {
  // don't do anything if there's only one guy left
  if (nameArray.value.length === 1) {
    showSpecialEndingImage.value = true
    return
  }

  if (randomValue.value !== -1) 
    nameArray.value = nameArray.value.filter((_, idx) => idx !== randomValue.value)
  
  randomValue.value = Math.floor(Math.random() * (nameArray.value.length - 1))
  if (randomValue.value < 0) 
    return
  backgroundColor.value = 'default'
  selectedPerson.value =  peopleRecord[nameArray.value[randomValue.value]]

  const selectedPersonName = nameArray.value[randomValue.value]

  const correctOption = peopleRecord[selectedPersonName].title

  const shuffledArray = Array.from(allJobTitle.values()).filter(title => title !== correctOption).sort(() => 0.5 - Math.random())
  const otherOption = shuffledArray[0]

  const options = [correctOption, otherOption].sort(() => 0.5 - Math.random())
  leftOption.value = options[0]
  rightOption.value = options[1]
  hasNotSelectAnswer.value = true
}

function onSelectOption(option: string) {
  visitedMembers.push(nameArray.value[randomValue.value])
  if (option === selectedPerson.value?.title)  {
    confetti()
    showAllInfo.value = false
    backgroundColor.value = 'correct'
  }
  else {
    backgroundColor.value = 'wrong'
    showAllInfo.value = true
  }

  hasNotSelectAnswer.value = false
  //selectNewPerson()
}

function viewMember(name: string) {
  selectedPerson.value = peopleRecord[name]
  showAllInfo.value = true
  isHistoryModelOpen.value = false
  backgroundColor.value = 'default'
}

onMounted(() => {
  selectNewPerson()
})
</script>

<template>
  <div
    class="w-[100dvw] h-[100dvh] px-4 overflow-hidden"
    :class="{
      'bg-light-purple-100': backgroundColor === 'default',
      'bg-light-green-100': backgroundColor === 'correct',
      'bg-kinda-pink-200': backgroundColor === 'wrong',
    }"
  >
    <div class="h-full w-full mx-auto md:w-[365px]">
      <GameHeader />
      <!--
    <template v-for="(people, nameKey) in peopleRecord" :key="people.name">
      <div>
        <img :src="`people_images/${nameKey}.png`" :width="200" :height="200">
        {{ people.name }}
        {{ people.title }}
      </div>
    </template>
    -->
      <div v-if="showSpecialEndingImage">
        <div class="caprasimo text-5xl">
          YOU DID IT!!!
        </div>
        <Image src="/ending.gif" img-class="w-96 h-auto" />
      </div>
      <div v-else-if="selectedPerson" class="w-full flex flex-col gap-4 items-center">
        <div 
          class="relative flex justify-center min-h-0 w-full h-[calc(100dvh-9rem)]"
        >
          <Transition name="rotate">
            <div v-if="hasNotSelectAnswer" class="relative w-full h-full">
              <CardContainer class="relative bg-white h-full py-8 z-20">
                <div class="h-full flex flex-col gap-2 justify-center items-center">
                  <Image :src="imageSrc" :alt="selectedPerson.name" class="flex-1 max-h-80" img-class="h-full max-h-80 w-full object-cover" />
                  <div class="text-3xl font-bold">
                    {{ selectedPerson.name }}
                  </div>
                  <div class="flex flex-col h-28 justify-between gap-6 max-w-full">
                    <UButton class="w-72 justify-center" color="green" truncate :label="leftOption" @click="() => onSelectOption(leftOption)" />
                    <UButton class="w-72 justify-center bg-purple-400 active:bg-purple-500 hover:bg-purple-500" truncate :label="rightOption" @click="() => onSelectOption(rightOption)" />
                  </div>
                </div>
              </CardContainer>
              <img src="/svgs/bottom_shadow.svg" class="absolute z-10 -bottom-8 w-full">
            </div>
            <div v-else class="absolute flex flex-col items-center justify-center w-full h-full">
              <Card
                :name="selectedPerson.name"
                :title="selectedPerson.title"
                :fact="selectedPerson.fact!"
                :image-src="imageSrc"
                :display-style="showAllInfo ? 'ALL' : 'FACT'"
              />
              <div v-if="!hasNotSelectAnswer" class="flex justify-around w-full pt-8">
                <UButton class="w-40 text-black justify-center bg-super-light-yellow-50 active:bg-super-light-yellow-200 hover:bg-super-light-yellow-200" label="History" @click="() => isHistoryModelOpen = true" />
                <UButton class="w-40 text-black justify-center bg-light-yellow-200 active:bg-light-yellow-300 hover:bg-light-yellow-300" label="Next" @click="() => selectNewPerson()" />
              </div>
            </div>
          </Transition>
        </div>
      </div>
    </div>
    <Modal v-model="isHistoryModelOpen">
      <CardContainer class="mx-auto w-96 max-w-screen h-[500px] relative bg-light-purple-100 overflow-hidden flex flex-col gap-6">
        <div class="pt-10 text-3xl font-bold text-black">
          Pokédex
        </div>
        <div class="grid grid-cols-3 gap-3 overflow-scroll pb-2">
          <template v-for="person in peopleRecord" :key="person.name">
            <div 
              v-if="visitedMembers.includes(toStandardName(person.name))"
              class="relative w-fit h-fit overflow-hidden cursor-pointer"
              @click="() => viewMember(toStandardName(person.name))"
            >
              <Image 
                :src="`pd/${toStandardName(person.name)}/thumbnail.png`"
                :alt="person.name" 
                class="border border-[#000] border-solid"
                img-class="w-24 h-24 object-cover"
              />
              <!-- mask -->
              <div class="absolute left-[1px] bottom-[1px] w-24 h-24 rounded-3xl bg-gradient-to-b from-transparent to-black" />
              <span class="absolute bottom-1 left-[1px] w-24 px-1 text-white text-sm"> {{ person.name }} </span>
            </div>
            <div v-else class="w-24 h-24 overflow-hidden bg-black rounded-3xl border border-[#000] border-solid" />
          </template>
        </div>
        <UButton icon="i-heroicons-x-mark-20-solid" class="bg-purple-400 active:bg-purple-500 hover:bg-purple-500 w-10 h-10 items-center justify-center absolute right-6 top-6" @click="isHistoryModelOpen = false" />
      </CardContainer>
    </Modal>
  </div>
</template>

<style scoped>
.rotate-enter-active {
  transition: all 0.3s linear;
  transition-delay: 0.3s;
}

.rotate-leave-active {
  transition: all 0.3s linear;
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
