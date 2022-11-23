<script setup>
import { ref } from "vue";

const isAwesome = ref(true);

function switchAwesome() {
  isAwesome.value = !isAwesome.value;
}
</script>

<template>
  <p v-show="isAwesome">Vue is awesome!</p>
  <p v-show="!isAwesome">Vue is not good...</p>
  <p v-if="isAwesome">React is also awesome!</p>
  <p v-else>React is not good too...</p>
  <button @click="switchAwesome">Click</button>
</template>
