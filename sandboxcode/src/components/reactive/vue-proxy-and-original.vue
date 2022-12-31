<template>
  <div>
    <p>raw: {{ raw }}</p>
    <p>rawProxy: {{ rawProxy }}</p>
    <p>is raw and rawProxy same?: {{ raw === rawProxy ? "yes" : "no" }}</p>
    <button @click="pushHoge">add hoge</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";
const raw = {
  key1: "foo",
  key2: "bar",
}
const rawProxy = reactive(raw);

function pushHoge() {
  rawProxy.key3 = "hoge";
}
</script>
