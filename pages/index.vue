<template>
  <div class="w-screen h-screen">
    <Transition appear>
      <div class="h-full flex flex-col justify-center items-center gap-28">
        <div class="flex flex-col justify-center items-center gap-4">
          <div class="text-4xl font-bold">
            PicConnect
          </div>
          <div>
            How well do you know your members 😎
          </div>
        </div>
        <UButton color="white" variant="outline" size="xl" class="w-52 justify-center rounded-full" to="/game">
          Start
        </UButton>
      </div>
    </Transition>
  </div>
</template>
