<script setup>
import { ref } from "vue";

const activeClass = ref("active");
const errorClass = ref("text-danger");
</script>

<template>
  <p v-bind:class="[activeClass, errorClass]">sample</p>
</template>
