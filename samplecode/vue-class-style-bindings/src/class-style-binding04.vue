<script setup>
import { reactive, ref } from "vue";

const activeColor = ref("red");
const fontSize = ref(30);

const styleObj = reactive({
  color: "red",
  fontSize: "30px",
});
</script>

<template>
  <!-- camelCased -->
  <p v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }">
    camelCased
  </p>
  <!-- short for v-bind:style, and kebab-cased -->
  <p :style="{ color: activeColor, 'font-size': fontSize + 'px' }">
    kebab-cased
  </p>
  <hr />
  <p v-bind:style="styleObj">binding object</p>
</template>