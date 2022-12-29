<template>
  <div>
    <p>This button has v-bind:disabled with truthy value (ok).</p>
    <button :disabled="isButtonDisabled">sample button</button>
  </div>
</template>

<script setup>
const isButtonDisabled = "ok";
</script>
