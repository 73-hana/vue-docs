<template>
  <div>
    <p>Using text interpolation: {{ rawHTML }}</p>
    <p>Using v-html directive: <span v-html="rawHTML"></span></p>
  </div>
</template>

<script setup>
const rawHTML = "<span style='color: red;'>text in span</span>";
</script>
