<script setup>
import { ref } from 'vue';
import Aquarium from './Aquarium.vue';
import FishModal from './FishModal.vue';
import FloatingAddButton from './FloatingAddButton.vue';

const peixes = ref([]);
const isModalOpen = ref(false);

const addFish = (fish) => {
  peixes.value.push(fish);
};

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};
</script>

<template>
  <div class="app h-screen w-screen relative">
    <!-- Aquarium ocupa toda a tela -->
    <Aquarium class="w-full h-full" :peixes="peixes" />
    
    <!-- Botão flutuante -->
    <FloatingAddButton @click="openModal" :shouldBlink="peixes.length === 0" />
    
    <!-- Modal para adicionar peixes -->
    <FishModal 
      :isOpen="isModalOpen" 
      @addFish="addFish" 
      @close="closeModal" 
    />
  </div>
</template>
