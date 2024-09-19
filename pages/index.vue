<template>
  <div class="relative w-screen h-screen bg-light-purple-100">
    <Transition appear>
      <div class="relative z-20 h-full flex flex-col justify-center items-center gap-28">
        <div class="flex flex-col justify-center items-center gap-4">
          <div class="text-7xl font-semi bold text-black caprasimo">
            Pic <br> Connect
          </div>
          <div class="roboto-mono">
            How well do you know your members 😎
          </div>
        </div>
        <UButton size="xl" class="w-14 justify-center bg-purple-400 active:bg-purple-500 hover:bg-purple-500" to="/game" icon="heroicons-solid:arrow-right" />
      </div>
    </Transition>
    <img src="/svgs/bg.svg" class="z-10 absolute bottom-0 w-screen h-screen object-cover">
  </div>
</template>
