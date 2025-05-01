<template>
  <UIcon
    :name="'i-heroicons:heart-solid'"
    :class="[
      'text-xl cursor-pointer transition-colors duration-300',
      isFilled ? 'text-purple-500 animate-pop' : 'text-gray-400'
    ]"
    @click="toggleHeart"
    @animationend="isAnimating = false"
  />
</template>

<script setup>
import { ref } from 'vue';

const isFilled = ref(false);
const isAnimating = ref(false);

function toggleHeart() {
  isFilled.value = !isFilled.value;
  if (isFilled.value) {
    isAnimating.value = true;
  }
}
</script>

<style scoped>
@keyframes pop {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.4);
  }
  100% {
    transform: scale(1);
  }
}

.animate-pop {
  animation: pop 0.3s ease;
}
</style>
