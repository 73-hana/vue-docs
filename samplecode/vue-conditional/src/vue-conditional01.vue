<script setup>
import { ref } from "vue";

const isAwesome = ref("awesome");

function swichAwesome() {
  isAwesome.value = "awesome";
}

function switchSoSo() {
  isAwesome.value = "so-so";
}

function switchBad() {
  isAwesome.value = "bad";
}
</script>

<template>
  <p v-if="isAwesome === 'awesome'">Vue is awesome!</p>
  <p v-else-if="isAwesome === 'so-so'">Vue is so-so.</p>
  <p v-else>Vue is not good...</p>
  <button @click="swichAwesome">Awesome</button>
  <button @click="switchSoSo">SoSo</button>
  <button @click="switchBad">Bad</button>
</template>
