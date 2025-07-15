<template>

  <!-- Interface Mobile -->
  <div class="mobile-content">
    <div class="mobile-container">
      
      <!-- Header Mobile -->
      <div class="mobile-header">
        <h1 class="mobile-title">🔐 Base64 Tools</h1>
      </div>

      <!-- Área de entrada -->
      <div class="mobile-input-section">
        <label class="mobile-label">Texto de entrada:</label>
        <textarea
          class="mobile-textarea mobile-input"
          placeholder="Digite o texto para codificar ou decodificar..."
          rows="6"
          v-model="text"
        />
      </div>

      <!-- Botões de ação -->
      <div class="mobile-buttons">
        <button @click="clear" class="mobile-btn mobile-btn-clear">
          <span class="mobile-btn-icon">🗑️</span>
          Limpar
        </button>
        <button @click="encode" :class="['mobile-btn', text ? 'mobile-btn-primary' : 'mobile-btn-disabled']" :disabled="!text">
          <span class="mobile-btn-icon">🔒</span>
          Codificar
        </button>
        <button @click="decode" :class="['mobile-btn', text ? 'mobile-btn-primary' : 'mobile-btn-disabled']" :disabled="!text">
          <span class="mobile-btn-icon">🔓</span>
          Decodificar
        </button>
      </div>

      <!-- Área de resultado -->
      <div class="mobile-result-section">
        <label class="mobile-label">Resultado:</label>
        <textarea
          class="mobile-textarea mobile-result"
          placeholder="Resultado aparecerá aqui..."
          rows="6"
          :value="result"
          readonly
          disabled
        />
        
        <!-- Botão copiar -->
        <button @click="copy" :class="['mobile-btn mobile-btn-copy', result ? 'mobile-btn-success' : 'mobile-btn-disabled']" :disabled="!result">
          <span class="mobile-btn-icon">📋</span>
          Copiar
        </button>
      </div>

      <!-- Feedback de cópia -->
      <transition name="mobile-fade">
        <div v-if="copyFeedback" class="mobile-feedback">
          <span class="mobile-feedback-text">✅ Copiado para a área de transferência!</span>
        </div>
      </transition>

    </div>
  </div>

  <!-- Conteúdo original (visível apenas em desktop) -->
  <div class="desktop-content">

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

      <div class="flex justify-center items-center gap-2 mt-8">

        <button @click="clear" class="btn-gradient p-2 rounded-md w-24"> Clear </button>
        <button @click="encode" :class="['p-2 rounded-md w-24', text ? 'btn-gradient' : 'btn-disabled']" :disabled="!text" > Encode </button>
        <button @click="decode" :class="['p-2 rounded-md w-24', text ? 'btn-gradient' : 'btn-disabled']" :disabled="!text"> Decode </button>
        <button @click="copy" :class="['p-2 rounded-md w-24', result ? 'btn-gradient' : 'btn-disabled']" :disabled="!result"> Copy </button>

      </div>

      <transition name="fade">
        
        <div v-if="copyFeedback" class="flex justify-center mt-4">
          <span class="text-[#2563eb] font-semibold">Copied!</span>
        </div>

      </transition>

    </div>

    <Footer />

  </div>

</template>

<style scoped>

/* Interface Mobile */
.mobile-content {
  display: none;
  min-height: 100vh;
  padding: 1rem;
}

.mobile-container {
  max-width: 100%;
  margin: 0 auto;
}

.mobile-header {
  text-align: center;
  margin-bottom: 2rem;
  padding: 1rem;
}

.mobile-title {
  font-size: 2rem;
  font-weight: bold;
  color: white;
  margin-bottom: 0.5rem;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
}

.mobile-subtitle {
  color: rgba(255,255,255,0.9);
  font-size: 1rem;
}

.mobile-input-section,
.mobile-result-section {
  margin-bottom: 1.5rem;
}

.mobile-label {
  display: block;
  color: white;
  font-weight: 600;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

.mobile-textarea {
  width: 100%;
  border: none;
  border-radius: 12px;
  font-size: 1rem;
  font-family: inherit;
  resize: none;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.mobile-input {
  background: white;
  color: #333;
  padding: 1rem;
}

.mobile-input:focus {
  outline: none;
  box-shadow: 0 4px 20px rgba(37, 99, 235, 0.4);
}

.mobile-result {
  background: #1f2937;
  color: white;
  padding: 1rem;
  font-family: 'Courier New', monospace;
}

.mobile-buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
}

.mobile-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 1rem;
  border: none;
  border-radius: 12px;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.mobile-btn-clear {
  grid-column: 1 / -1;
  background: linear-gradient(135deg, #ff6b6b, #ee5a52);
  color: white;
}

.mobile-btn-clear:active {
  transform: translateY(2px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
}

.mobile-btn-primary {
  background: linear-gradient(90deg, #2563eb, #1e3a8a, #2563eb);
  background-size: 200% 200%;
  animation: gradient-move 2s linear infinite;
  color: white;
}

.mobile-btn-primary:active {
  transform: translateY(2px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
}

.mobile-btn-success {
  background: linear-gradient(90deg, #2563eb, #1e3a8a, #2563eb);
  background-size: 200% 200%;
  animation: gradient-move 2s linear infinite;
  color: white;
}

.mobile-btn-success:active {
  transform: translateY(2px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
}

.mobile-btn-disabled {
  background: #4b5563;
  color: #9ca3af;
  cursor: not-allowed;
}

.mobile-btn-icon {
  font-size: 1.1rem;
}

.mobile-btn-copy {
  width: 100%;
  margin-top: 0.5rem;
}

.mobile-feedback {
  text-align: center;
  padding: 1rem;
  margin-top: 1rem;
}

.mobile-feedback-text {
  color: white;
  font-weight: 600;
  background: rgba(37, 99, 235, 0.9);
  padding: 0.75rem 1.5rem;
  border-radius: 25px;
  display: inline-block;
}

.mobile-fade-enter-active, .mobile-fade-leave-active {
  transition: all 0.3s ease;
}

.mobile-fade-enter-from, .mobile-fade-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

/* Mostrar interface mobile apenas em mobile */
@media (max-width: 768px) {
  .mobile-content {
    display: block;
  }
  
  .desktop-content {
    display: none;
  }
}

/* Esconder interface mobile em desktop */
@media (min-width: 769px) {
  .mobile-content {
    display: none;
  }
  
  .desktop-content {
    display: block;
  }
}

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