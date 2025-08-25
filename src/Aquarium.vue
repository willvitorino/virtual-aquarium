<script setup lang="ts">
import { PropType, ref, onMounted, onUnmounted, watch } from 'vue';
import FishBase from './FishBase.vue';

const props = defineProps({
  peixes: {
    type: Array as PropType<{
      type: string;
      name: string;
    }[]>,
    required: true,
  },
});

interface FishPosition {
  x: number;
  y: number;
  directionX: number;
  directionY: number;
  speed: number;
}

interface DragState {
  isDragging: boolean;
  dragIndex: number | null;
  offsetX: number;
  offsetY: number;
}

const fishPositions = ref<FishPosition[]>([]);
const aquariumRef = ref<HTMLElement>();
const dragState = ref<DragState>({
  isDragging: false,
  dragIndex: null,
  offsetX: 0,
  offsetY: 0,
});
let animationId: number;

// Função para inicializar posições dos peixes
const initializeFishPositions = () => {
  fishPositions.value = props.peixes.map(() => ({
    x: Math.random() * 80, // Posição em porcentagem (0-80% para deixar margem)
    y: Math.random() * 80,
    directionX: (Math.random() - 0.5) * 2, // Direção aleatória (-1 a 1)
    directionY: (Math.random() - 0.5) * 2,
    speed: 0.3 + Math.random() * 0.7, // Velocidade entre 0.3 e 1
  }));
};

// Função para mover os peixes
const moveFish = () => {
  fishPositions.value.forEach((fish, index) => {
    // Não move o peixe que está sendo arrastado
    if (dragState.value.isDragging && dragState.value.dragIndex === index) {
      return;
    }

    // Atualiza posição
    fish.x += fish.directionX * fish.speed;
    fish.y += fish.directionY * fish.speed;

    // Verifica limites e inverte direção se necessário
    if (fish.x <= 0 || fish.x >= 85) {
      fish.directionX *= -1;
      fish.x = Math.max(0, Math.min(85, fish.x));
    }
    if (fish.y <= 0 || fish.y >= 85) {
      fish.directionY *= -1;
      fish.y = Math.max(0, Math.min(85, fish.y));
    }

    // Adiciona variação aleatória na direção ocasionalmente
    if (Math.random() < 0.01) {
      fish.directionX += (Math.random() - 0.5) * 0.3;
      fish.directionY += (Math.random() - 0.5) * 0.3;
      
      // Normaliza direção para manter velocidade constante
      const magnitude = Math.sqrt(fish.directionX ** 2 + fish.directionY ** 2);
      if (magnitude > 0) {
        fish.directionX = (fish.directionX / magnitude) * 0.8;
        fish.directionY = (fish.directionY / magnitude) * 0.8;
      }
    }
  });
};

// Loop de animação
const animate = () => {
  moveFish();
  animationId = requestAnimationFrame(animate);
};

// Reinicializa posições quando a lista de peixes muda
const updateFishPositions = () => {
  const currentCount = fishPositions.value.length;
  const newCount = props.peixes.length;
  
  if (newCount > currentCount) {
    // Adiciona novas posições para novos peixes
    for (let i = currentCount; i < newCount; i++) {
      fishPositions.value.push({
        x: Math.random() * 80,
        y: Math.random() * 80,
        directionX: (Math.random() - 0.5) * 2,
        directionY: (Math.random() - 0.5) * 2,
        speed: 0.3 + Math.random() * 0.7,
      });
    }
  } else if (newCount < currentCount) {
    // Remove posições de peixes removidos
    fishPositions.value = fishPositions.value.slice(0, newCount);
  }
};

onMounted(() => {
  initializeFishPositions();
  animate();
});

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
  
  // Limpa event listeners caso o componente seja desmontado durante drag
  document.removeEventListener('mousemove', handleDrag);
  document.removeEventListener('mouseup', endDrag);
});

// Funções de drag and drop
const startDrag = (event: MouseEvent, index: number) => {
  event.preventDefault();
  
  if (!aquariumRef.value) return;
  
  const aquariumRect = aquariumRef.value.getBoundingClientRect();
  const fishElement = event.currentTarget as HTMLElement;
  const fishRect = fishElement.getBoundingClientRect();
  
  dragState.value = {
    isDragging: true,
    dragIndex: index,
    offsetX: event.clientX - fishRect.left,
    offsetY: event.clientY - fishRect.top,
  };
  
  // Adiciona eventos globais
  document.addEventListener('mousemove', handleDrag);
  document.addEventListener('mouseup', endDrag);
};

const handleDrag = (event: MouseEvent) => {
  if (!dragState.value.isDragging || dragState.value.dragIndex === null || !aquariumRef.value) return;
  
  const aquariumRect = aquariumRef.value.getBoundingClientRect();
  
  // Calcula nova posição em porcentagem
  const newX = ((event.clientX - aquariumRect.left - dragState.value.offsetX) / aquariumRect.width) * 100;
  const newY = ((event.clientY - aquariumRect.top - dragState.value.offsetY) / aquariumRect.height) * 100;
  
  // Limita dentro dos bounds do aquário
  const boundedX = Math.max(0, Math.min(85, newX));
  const boundedY = Math.max(0, Math.min(85, newY));
  
  // Atualiza posição do peixe
  if (fishPositions.value[dragState.value.dragIndex]) {
    fishPositions.value[dragState.value.dragIndex].x = boundedX;
    fishPositions.value[dragState.value.dragIndex].y = boundedY;
  }
};

const endDrag = () => {
  dragState.value = {
    isDragging: false,
    dragIndex: null,
    offsetX: 0,
    offsetY: 0,
  };
  
  // Remove eventos globais
  document.removeEventListener('mousemove', handleDrag);
  document.removeEventListener('mouseup', endDrag);
};

// Observa mudanças na lista de peixes
watch(() => props.peixes.length, updateFishPositions);
</script>

<template>
  <div class="aquarium w-full h-full relative" ref="aquariumRef">
    <div
      class="fish-item absolute cursor-grab active:cursor-grabbing"
      :class="{
        'transition-all duration-1000 ease-in-out': !dragState.isDragging || dragState.dragIndex !== peixesIndex,
        'z-50': dragState.isDragging && dragState.dragIndex === peixesIndex
      }"
      v-for="(peixe, peixesIndex) in peixes"
      :key="peixesIndex"
      :style="{
        left: fishPositions[peixesIndex]?.x + '%',
        top: fishPositions[peixesIndex]?.y + '%',
      }"
      @mousedown="startDrag($event, peixesIndex)"
    >
      <FishBase
        :type="peixe.type"
        :selected="false"
        class="transition-all duration-1000 ease-in-out"
        :style="{
          transform: `scaleX(${fishPositions[peixesIndex]?.directionX > 0 ? 1 : -1})`
        }"
      />
      <span class="fish-name text-white text-center block mt-1">{{ peixe.name }}</span>
    </div>
  </div>
</template>

<style scoped>
.aquarium {
  background-image: url('fundo-aquario.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  overflow: hidden;
}

.fish-item {
  z-index: 10;
  pointer-events: auto;
}

.fish-name {
  font-size: 12px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.8);
  white-space: nowrap;
}

/* Animação suave para mudanças de direção */
.fish-item img {
  transition: transform 0.3s ease-in-out;
}
</style>
