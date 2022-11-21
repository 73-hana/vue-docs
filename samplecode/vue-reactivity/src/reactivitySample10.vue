<script setup>
import { ref } from "vue";

const obj = {
  foo: ref(0),
};

const { foo } = obj;
</script>

<template>
  <div>
    <p>refの挙動</p>
    <p>const obj = { foo: ref(0) };</p>
    <p>obj.foo + 1 = {{ obj.foo + 1 }}</p>
    <p>obj.foo.value + 1 = {{ obj.foo.value + 1 }}</p>
    <hr />
    <p>const { foo } = obj;</p>
    <p>foo + 1 = {{ foo + 1 }}</p>
  </div>
</template>
