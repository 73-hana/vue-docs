<script setup>
import { reactive } from "vue";

const obj = reactive({
  count: 0,
});

function increment() {
  obj.count++;
}
</script>

<template>
  <button @click="increment">{{ obj.count }}</button>
</template>
