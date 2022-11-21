<script setup>
import { reactive, ref } from "vue";

const count = ref(0);
const state = reactive({
  count,
});

console.log(count.value);

function increment() {
  state.count++;
}

function overwriteRef() {
  const otherCount = ref(5);
  state.count = otherCount;
}
</script>

<template>
  <p>state.count is {{ state.count }}</p>
  <p>
    execute
    <code>state.count++</code>
    <button @click="increment">Click</button>
  </p>
  <p>now, count.value (count) is {{ count }}</p>
  <p>
    execute <code>state.count = otherCount</code
    ><button @click="overwriteRef">Click</button>
  </p>
  <p>now, count.value is {{ count }}</p>
</template>
