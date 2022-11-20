<script setup>
import { reactive } from "vue";

const rawObject = {
  count: 0,
};
const proxyObject = reactive(rawObject);
</script>

<template>
  <div>{{ proxyObject === rawObject ? "same" : "not same" }}</div>
</template>
