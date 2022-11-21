<script setup>
import { reactive, computed } from "vue";

const author = reactive({
  name: "John Doe",
  books: ["Book1 - for beginner", "Book2 - Basic", "Book3 - Advanced"],
});

const publishedBooks = computed(() => {
  return author.books.length > 0 ? "yes" : "no";
});
</script>

<template>
  <p>
    Favorite Books?
    <span>{{ publishedBooks }}</span>
  </p>
</template>
