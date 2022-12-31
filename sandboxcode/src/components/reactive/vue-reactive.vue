<template>
  <div>
    <button @click="increment">{{ state.count }}</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const state = reactive({
  count: 0,
});

function increment() {
  state.count++;
}
</script>
