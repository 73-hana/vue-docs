<script setup>
import { reactive } from "vue";

const obj = reactive({
  nested: {
    count: 0,
  },
  arr: ["foo", "bar"],
});

function mutateDeeply() {
  obj.nested.count++;
  obj.arr.push("baz");
}
</script>

<template>
  <button @click="mutateDeeply">{{ obj.arr }}</button>
</template>
