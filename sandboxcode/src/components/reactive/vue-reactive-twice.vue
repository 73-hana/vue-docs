<template>
  <div>
    <p>Are rawProxy and raw same?:{{ rawProxy === raw ? "yes" : "no" }}</p>
    <p>Are rawProxy and rawProxyAgain same?: {{ rawProxy === rawProxyAgain ? "yes" : "no" }}</p>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const raw = {
  foo: "foo",
  bar: "bar",
};
const rawProxy = reactive(raw);
const rawProxyAgain = reactive(rawProxy);
</script>
