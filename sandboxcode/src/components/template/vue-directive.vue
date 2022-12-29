<template>
  <div>
    <p v-bind:style="sampleStyle">sample</p>
    <p :style="sampleStyle">sample</p>
    <button v-on:click="handleAlert">v-on:click</button>
    <button @click="handleAlert">@click</button>
  </div>
</template>

<script setup>
const sampleStyle = {
  color: "yellow",
  fontWeight: "bold",
};
function handleAlert() {
  alert("hello");
}
</script>
