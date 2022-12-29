<template>
  <div>
    <a :[attrName]="url">sample anchor</a>
  </div>
</template>

<script setup>
const attrName = "href";
const url = "#";
</script>
