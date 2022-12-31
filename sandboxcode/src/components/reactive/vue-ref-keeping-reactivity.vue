<template>
  <div>
    <p>refObj.foo: {{ refObj.foo }}</p>
    <p>detachedNum: {{ detachedNum }}</p>
    <button @click="increment">increment</button>
    <button @click="overwriteRefObj">overwriteRefObj</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

let refObj = ref({
  foo: 1,
  bar: 2,
});

let detachedNum = refObj.value.foo;

function increment() {
  refObj.value.foo++;
  refObj.value.bar++;
  detachedNum++;
}

function overwriteRefObj() {
  refObj.value = {
    foo: 100,
    bar: 100,
  };
}
</script>
