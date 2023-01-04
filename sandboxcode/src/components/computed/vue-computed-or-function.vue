<template>
  <div>
    <p>This text depends on ref: {{ introduction }}</p>
    <p>This text depends on primitive: {{ now }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const name = ref("john doe");
const introduction = computed(() => {
  return `My name is ${name.value}`;
});

const now = computed(() => Date.now());
</script>
