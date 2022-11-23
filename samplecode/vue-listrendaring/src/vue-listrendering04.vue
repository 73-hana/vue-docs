<script setup>
import { ref } from "vue";

const books = ref([
  { title: "JavaScript", price: "$15", isAvailable: true },
  { title: "React", price: "$20", isAvailable: false },
  { title: "Vue", price: "$20", isAvailable: true },
]);
</script>

<template>
  <p>my favorite books</p>
  <ul>
    <template v-for="book in books">
      <li v-if="book.isAvailable">
        title: {{ book.title }} ({{ book.price }})
      </li>
    </template>
  </ul>
</template>
