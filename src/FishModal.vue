<script setup lang="ts">
import { ref } from 'vue';
import FishBase from './FishBase.vue';

const emit = defineEmits(['addFish', 'close']);

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
});

const fishOptions = [
  'golden-purple-fish',
  'gold-fish',
  'guppie-fish',
  'tropical-fish',
  'tuna-fish'
];

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
};

const handleAddFish = () => {
  if (fishName.value && selectedFishType.value) {
    emit('addFish', {
      name: fishName.value,
      type: selectedFishType.value
    });

    // Reset form
    selectedFishType.value = '';
    fishName.value = '';
    
    // Close modal
    emit('close');
  }
};

const closeModal = () => {
  // Reset form when closing
  selectedFishType.value = '';
  fishName.value = '';
  emit('close');
};

const handleBackdropClick = (event: MouseEvent) => {
  if (event.target === event.currentTarget) {
    closeModal();
  }
};
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div 
        v-if="isOpen"
        class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50 p-4"
        @click="handleBackdropClick"
      >
        <div class="bg-blue-300 rounded-lg p-6 w-full max-w-md max-h-[90vh] overflow-y-auto relative">
          <!-- Close button -->
          <button
            @click="closeModal"
            class="absolute top-4 right-4 text-white hover:text-gray-200 text-2xl font-bold"
          >
            ×
          </button>

          <!-- Modal header -->
          <h2 class="text-2xl font-bold text-white text-center mb-6">Adicionar Peixe</h2>

          <!-- Fish selection -->
          <div class="fish-types-container mb-6">
            <label class="text-lg text-center font-bold text-white w-full flex justify-center mb-2">Peixes:</label>
            <label class="text-sm text-center mb-4 text-white w-full flex justify-center">(Escolha um peixe)</label>

            <div class="fish-types-list grid grid-cols-3 gap-3 justify-items-center">
              <FishBase
                v-for="fishType in fishOptions"
                :key="fishType"
                :type="fishType"
                :selected="selectedFishType === fishType"
                @click="handleSelectFishType(fishType)"
                class="cursor-pointer hover:scale-110 transition-transform"
              />
            </div>
          </div>

          <!-- Fish name input -->
          <div class="fish-name-container">
            <h3 class="text-lg text-center mb-4 font-bold text-white">Nome do peixe:</h3>

            <input
              type="text"
              v-model="fishName"
              class="w-full p-3 rounded-md mb-4 text-lg"
              placeholder="Digite o nome do peixe"
              :readonly="!selectedFishType"
              :disabled="!selectedFishType"
              @keyup.enter="handleAddFish"
            />

            <div class="flex gap-3">
              <button
                class="bg-gray-500 text-white p-3 rounded-md flex-1 hover:bg-gray-600 transition-colors"
                @click="closeModal"
              >
                Cancelar
              </button>
              <button
                class="bg-blue-500 text-white p-3 rounded-md flex-1 disabled:opacity-50 disabled:cursor-not-allowed hover:bg-blue-600 transition-colors"
                :disabled="!fishName || !selectedFishType"
                @click="handleAddFish"
              >
                Adicionar
              </button>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-enter-active, .modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from, .modal-leave-to {
  opacity: 0;
}

.modal-enter-active .bg-blue-300,
.modal-leave-active .bg-blue-300 {
  transition: transform 0.3s ease;
}

.modal-enter-from .bg-blue-300,
.modal-leave-to .bg-blue-300 {
  transform: scale(0.9);
}
</style>
