<script setup>
import { ref } from "vue";

const count = ref(0);

function increment() {
  count.value++;
  console.log(count.value);
}
</script>

<template>
  <button @click="increment">Click me</button>
</template>
