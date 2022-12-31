<template>
  <div>
    <p>count: {{ count.count }}</p>
    <p>n: {{ n }}</p>
    <button @click="increment">increment</button>
    <button @click="overwriteCount">overwrite</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

let count = reactive({ count: 0 });
let n = count.count;

function increment() {
  count.count++;
  n++;
}

function overwriteCount() {
  count = reactive({ numVar: 0 });
}
</script>
