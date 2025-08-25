<script setup lang="ts">
import { ref } from 'vue';
import FishBase from './FishBase.vue';

const emit = defineEmits(['addFish']);

const fishOptions = [
  'golden-purple-fish',
  'gold-fish',
  'guppie-fish',
  'tropical-fish',
  'tuna-fish'
]

const selectedFishType = ref<string>('');
const fishName = ref<string>('');

const handleSelectFishType = (fishType: string) => {
  if (selectedFishType.value === fishType) {
    selectedFishType.value = '';
    fishName.value = '';
  } else {
    selectedFishType.value = fishType;
    fishName.value = '';
  }
}

const handleAddFish = () => {
  emit('addFish', {
    name: fishName.value,
    type: selectedFishType.value
  });

  selectedFishType.value = '';
  fishName.value = '';
}
</script>

<template>
  <div class="fish-form w-[100%] h-full bg-blue-300">
    <div class="fish-types-container">
      <label class="text-lg text-center font-bold text-white w-full flex justify-center">Peixes:</label>
      <label class="text-sm text-center mb-4 text-white w-full flex justify-center">(Escolha um peixe)</label>

      <div class="fish-types-list flex flex-row flex-wrap gap-2 justify-around items-center">
        <FishBase
          v-for="fishType in fishOptions"
          :key="fishType"
          :type="fishType"
          :selected="selectedFishType === fishType"
          @click="handleSelectFishType(fishType)"
        />
      </div>
    </div>

    <div
      v-if="selectedFishType"
      class="fish-name-container flex flex-col items-center justify-center w-full p-4"
    >
      <h1 class="text-lg text-center mb-4 font-bold text-white">Nome do peixe:</h1>
      <input type="text" v-model="fishName" class="w-full p-2 rounded-md mb-2" placeholder="Digite o nome do peixe" />
      <button
        class="bg-blue-500 text-white p-2 rounded-md w-full disabled:opacity-50 disabled:cursor-not-allowed"
        :disabled="!fishName"
        @click="handleAddFish"
      >
        Adicionar
      </button>
    </div>
  </div>
</template>

<style scoped>

</style>
