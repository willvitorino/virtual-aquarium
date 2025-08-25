<script setup lang="ts">
const emit = defineEmits(['click']);

const props = defineProps({
  shouldBlink: {
    type: Boolean,
    default: false
  }
});

const handleClick = () => {
  emit('click');
};
</script>

<template>
  <button
    @click="handleClick"
    class="fixed bottom-6 right-6 w-16 h-16 bg-blue-500 hover:bg-blue-600 text-white rounded-full shadow-lg hover:shadow-xl transition-all duration-300 flex items-center justify-center text-3xl font-bold z-40 group"
    :class="{ 'blink-animation': shouldBlink }"
  >
    <span class="group-hover:scale-110 transition-transform duration-200 !font-mono">+</span>
  </button>
</template>

<style scoped>
button {
  box-shadow: 0 4px 20px rgba(59, 130, 246, 0.4);
}

button:hover {
  box-shadow: 0 8px 30px rgba(59, 130, 246, 0.6);
  transform: translateY(-2px);
}

/* Animação de piscar */
.blink-animation {
  animation: blink 2s infinite;
}

@keyframes blink {
  0%, 50% {
    opacity: 1;
    transform: scale(1);
    box-shadow: 0 4px 20px rgba(59, 130, 246, 0.4);
  }
  25% {
    opacity: 0.7;
    transform: scale(1.1);
    box-shadow: 0 8px 30px rgba(59, 130, 246, 0.8);
  }
  75% {
    opacity: 0.8;
    transform: scale(1.05);
    box-shadow: 0 6px 25px rgba(59, 130, 246, 0.6);
  }
}

/* Pausa a animação quando hover para não interferir com o efeito hover */
.blink-animation:hover {
  animation-play-state: paused;
}
</style>
