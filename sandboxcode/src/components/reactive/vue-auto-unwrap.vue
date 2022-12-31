<template>
  <div>
    <p>count: {{ count }}</p>
    <p>nestedCount: {{ nestedCount.count.value }}</p>
    <button @click="increment">increment</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const count = ref(0);
const nestedCount = {
  count: ref(0),
};

function increment() {
  count.value++;
  nestedCount.count.value++;
}
</script>
