<template>

  <div class="max-w-4xl">

    <div class="flex flex-col md:flex-row gap-4 mt-8">

      <textarea
        class="w-full p-4 rounded-md border border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-600 resize-none"
        placeholder="Enter the text to be encoded or decoded..."
        rows="10"
        v-model="text"
      />

      <textarea
        class="w-full p-4 rounded-md border border-blue-500 bg-gray-900 text-white focus:outline-none resize-none"
        placeholder="Result..."
        rows="10"
        :value="result"
        readonly
        disabled
      />
      
    </div>

    <div class="flex flex-wrap justify-center items-center gap-2 mt-8">

      <button @click="clear" class="btn-gradient p-2 rounded-md w-20 md:w-24 text-sm md:text-base"> Clear </button>
      <button @click="encode" :class="['p-2 rounded-md w-20 md:w-24 text-sm md:text-base', text ? 'btn-gradient' : 'btn-disabled']" :disabled="!text" > Encode </button>
      <button @click="decode" :class="['p-2 rounded-md w-20 md:w-24 text-sm md:text-base', text ? 'btn-gradient' : 'btn-disabled']" :disabled="!text"> Decode </button>
      <button @click="copy" :class="['p-2 rounded-md w-20 md:w-24 text-sm md:text-base', result ? 'btn-gradient' : 'btn-disabled']" :disabled="!result"> Copy </button>

    </div>

    <transition name="fade">
      
      <div v-if="copyFeedback" class="flex justify-center mt-4">
        <span class="text-[#2563eb] font-semibold">Copied!</span>
      </div>

    </transition>

  </div>

  <Footer />

</template>

<style scoped>

@keyframes gradient-move {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 200% 50%;
  }
}

.btn-gradient {
  background: linear-gradient(90deg, #2563eb, #1e3a8a, #2563eb);
  background-size: 200% 200%;
  animation: gradient-move 2s linear infinite;
  color: #fff;
  border: none;
  transition: box-shadow 0.2s;
}
.btn-gradient:hover {
  box-shadow: 0 4px 20px 0 rgba(37,99,235,0.3);
}

.btn-disabled {
  background: #4b5563;
  color: #9ca3af;
  cursor: default;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>

<script setup>

import { ref } from 'vue';
import Footer from './Footer.vue';

const text          = ref('');
const result        = ref('');
const copyFeedback  = ref(false);

function encode() {
  try {
    result.value = btoa(encodeURIComponent(text.value));
  } catch (e) {
    result.value = 'Erro ao codificar: texto inválido.';
  }
}

function decode() {
  try {
    result.value = decodeURIComponent(atob(text.value));
  } catch (e) {
    result.value = 'Erro ao decodificar: texto inválido.';
  }
}

function copy() {
  navigator.clipboard.writeText(result.value);
  copyFeedback.value = true;
  setTimeout(() => copyFeedback.value = false, 2000);
}

function clear() {
  text.value    = '';
  result.value  = '';
}

</script>