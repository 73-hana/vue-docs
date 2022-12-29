<template>
  <div>
    <p v-bind:id="dynamicId">This element has "{{ dynamicId }}" as id.</p>
    <p :id="dynamicId">This element has "{{ dynamicId }}" as id and written with abbreviated notation.</p>
  </div>
</template>

<script setup>
const dynamicId = "originalId";
</script>
