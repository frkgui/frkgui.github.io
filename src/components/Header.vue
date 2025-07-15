<template>

  <header class="header-animated w-full shadow-lg py-4 px-0 fixed top-0 left-0 z-10 border-b border-blue-800/40">
    <div class="flex flex-col items-center">
      <h1 class="text-3xl md:text-4xl font-extrabold text-white tracking-wide text-center drop-shadow-lg">{{ title }}</h1>
    </div>
  </header>

</template>

<style scoped>

@keyframes gradient-move { 
  0% {background-position: 0% 50%;}
  100% {background-position: 200% 50%;}
}

.header-animated {
  background: linear-gradient(90deg, #1d4ed8, #1e3a8a, #1d4ed8);
  background-size: 200% 200%;
  animation: gradient-move 3s linear infinite;
}

</style>

<script setup>

defineProps({ title: { type: String, required: true } })

</script>