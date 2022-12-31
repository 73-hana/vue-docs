<template>
  <div>
    <p>refNum: {{ refNum }}</p>
    <p>refObj.count: {{ refObj.count }}</p>
    <button @click="increment">increment</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const refNum = ref(0);
const refObj = ref({ count: 0 });

function increment() {
  refNum.value++;
  refObj.value.count++;
}
</script>
