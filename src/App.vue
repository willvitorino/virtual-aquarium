<script setup>
import { ref } from 'vue';
import Aquarium from './Aquarium.vue';
import FishForm from './FishForm.vue';

const peixes = ref([]);

const addFish = (fish) => {
  peixes.value.push(fish);
}
</script>

<template>
  <div class="app flex flex-col md:flex-row items-center justify-center h-screen w-screen">
    <!-- No mobile, o Aquarium vem primeiro (80% altura) -->
    <Aquarium class="aquarium-container w-full md:w-[80%] h-full md:h-full order-1 md:order-2 flex-1" :peixes="peixes" />
    
    <!-- No mobile, o FishForm vai para o final (20% altura) -->
    <FishForm class="fish-form-container w-full md:w-[20%] h-auto md:h-full order-2 md:order-1" @addFish="addFish" />
  </div>
</template>
