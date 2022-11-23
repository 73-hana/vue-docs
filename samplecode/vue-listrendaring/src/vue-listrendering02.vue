<script setup>
import { reactive } from "vue";

const myObject = reactive({
  title: "How to do lists in Vue",
  author: "John Doe",
  publishedAt: "2016-04-10",
});
</script>

<template>
  <p>My favorite book</p>
  <ul>
    <li v-for="(value, key, index) in myObject">
      {{ index }} {{ key }}: {{ value }}
    </li>
  </ul>
</template>
