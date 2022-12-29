<template>
  <div>
    <p v-bind="objAttr">sample text</p>
  </div>
</template>

<script setup>
const objAttr = {
  id: "text",
  style: "color: blue;",
};
</script>
