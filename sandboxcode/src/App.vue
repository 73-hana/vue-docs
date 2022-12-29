<template>
  <h1>Sample codes</h1>
  <h2>vue binding</h2>
  <VueBinding />
  <VueVBind />
  <VueVBindBool />
  <VueVBindAtOnce />
  <VueJsExpressions />
  <VueDirective />
  <VueDynamicDirective />
  <VueModifier />
</template>

<script setup>
import VueBinding from "./components/template/vue-binding.vue";
import VueDirective from "./components/template/vue-directive.vue";
import VueDynamicDirective from "./components/template/vue-dynamic-directive.vue";
import VueJsExpressions from "./components/template/vue-js-expressions.vue";
import VueModifier from "./components/template/vue-modifier.vue";
import VueVBindAtOnce from "./components/template/vue-v-bind-at-once.vue";
import VueVBindBool from "./components/template/vue-v-bind-bool.vue";
import VueVBind from "./components/template/vue-v-bind.vue";
</script>

<style scoped>
div {
  border: 1px solid black;
  margin: 5px;
  padding: 5px;
}
</style>
