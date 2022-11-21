<script setup>
import { ref } from "vue";

const objRef = ref({
  count: 0,
});

function increment() {
  objRef.value.count++;
}

function overwriteObjRef() {
  objRef.value = {
    count: 0,
  };
}
</script>

<template>
  <div>
    <p>refの返り値であるオブジェクトの.valueを書き換える</p>
    <span>Current count: {{ objRef }}</span>
    <button @click="increment">Add one</button>
    <button @click="overwriteObjRef">Caution! Overwrite!</button>
  </div>
</template>
