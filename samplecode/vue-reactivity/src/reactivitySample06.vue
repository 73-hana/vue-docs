<script setup>
import { reactive } from "vue";

const rawObj = {
  count: 0,
};
const proxyObj = reactive(rawObj);
</script>

<template>
  <div>
    proxyObj and rawObj are
    {{ proxyObj === rawObj ? "same" : "not same" }}
  </div>
  <div>
    reactive(proxyObj) and proxyObj are
    {{ reactive(proxyObj) === proxyObj ? "same" : "not same" }}
  </div>
</template>
