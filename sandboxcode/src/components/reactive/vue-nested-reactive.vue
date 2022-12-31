<template>
  <div>
    <button @click="changeNestedValue">{{ nestedObj.arr }}</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const nestedObj = reactive({
  obj: { count: 0 },
  arr: ["foo", "bar"],
});

function changeNestedValue() {
  nestedObj.obj.count++;
  nestedObj.arr.push("baz");
}
</script>
