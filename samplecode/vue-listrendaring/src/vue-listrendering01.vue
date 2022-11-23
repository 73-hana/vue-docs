<script setup>
import { ref } from "vue";

const nameofList = ref("Title:");
const books = ref([
  { title: "book1 - JavaScript" },
  { title: "book2 - React" },
  { title: "book3 - Vue.js" },
]);
</script>

<template>
  <p>My favorite books</p>
  <ul>
    <li v-for="(book, index) in books">
      {{ index }} {{ nameofList }} {{ book.title }}
    </li>
  </ul>
</template>
