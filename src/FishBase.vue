<script setup lang="ts">
defineOptions({
  name: 'FishBase'
});

defineProps<{
  type: string;
  selected: boolean;
}>();

defineEmits(['click']);
</script>

<template>
  <div
    class="fish-container w-[80px] cursor-pointer"
    @click="$emit('click')"
  >
    <img
      class="fish-image"
      :src="`${type}.png`"
      alt="Fish"
      :class="{ 'selected': selected }"
    />
  </div>
</template>

<style scoped>
.fish-container .selected {
  filter: drop-shadow(0 0 4px #3b82f6) drop-shadow(0 0 8px #3b82f6);
  transform: scale(1.05);
  transition: all 0.2s ease-in-out;
}

.fish-container .fish-image {
  transition: all 0.2s ease-in-out;
}
</style>
