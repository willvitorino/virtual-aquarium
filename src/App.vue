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
  <div class="app flex flex-row items-center justify-center h-screen w-screen">
    <FishForm class="fish-form-container !w-[20%] !h-full" @addFish="addFish" />

    <Aquarium class="aquarium-container w-[80%] h-full" :peixes="peixes" />
  </div>
</template>
