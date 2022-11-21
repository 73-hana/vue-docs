<script setup>
import { reactive } from "vue";

// 上書きされる恐れがあるので、constを使う
let state = reactive({
  count: 0,
});

function increment() {
  state.count++;
}

function overwriteReactive() {
  state = reactive({
    count: 1,
  });
}

let n = state.count;
function incrementN(n) {
  return n++;
}

function addone(number) {
  return number++;
}
</script>

<template>
  <div>
    <p>リアクティブなオブジェクトを置き換えたとき</p>
    <button @click="increment">Click count: {{ state.count }}</button>
    <button @click="overwriteReactive">Overwrite</button>
  </div>
  <div>
    <p>リアクティブなオブジェクトのプロパティの扱い</p>
    <p>Current state.count: {{ state.count }}</p>
    <button @click="incrementN(n)">increment n</button>
    <button @click="addone(state.count)">function argment</button>
  </div>
</template>
