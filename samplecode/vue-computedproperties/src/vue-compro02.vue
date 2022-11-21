<script setup>
import { reactive, computed } from "vue";

const today = reactive({ date: Date.now() });

const computedDate = computed(() => today.date);
function functionalDate() {
  return today.date;
}
</script>

<template>
  <p>{{ computedDate }}</p>
  <p>{{ functionalDate() }}</p>
</template>
