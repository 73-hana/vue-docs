<template>
  <div>
    <p>Is "ok" true? :{{ ok ? "Yes" : "No" }}</p>
    <p :id="`list-${id}`">This p element has v-bind:id with JS expression.</p>
  </div>
</template>

<script setup>
const ok = true;
const id = "1234";
</script>
