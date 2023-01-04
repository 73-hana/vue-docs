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
  <h2>reactive</h2>
  <VueReactive />
  <VueNestedReactive />
  <VueProxyAndOriginal />
  <VueReactiveTwice />
  <VueTwoRestriction />
  <VueRefMethod />
  <VueRefKeepingReactivity />
  <VueAutoUnwrap />
  <h2>Computed</h2>
  <VueComputedProperty />
  <VueComputedOrFunction />
</template>

<script setup>
import VueComputedOrFunction from "./components/computed/vue-computed-or-function.vue";
import VueComputedProperty from "./components/computed/vue-computed-property.vue";
import VueAutoUnwrap from "./components/reactive/vue-auto-unwrap.vue";
import VueNestedReactive from "./components/reactive/vue-nested-reactive.vue";
import VueProxyAndOriginal from "./components/reactive/vue-proxy-and-original.vue";
import VueReactiveTwice from "./components/reactive/vue-reactive-twice.vue";
import VueReactive from "./components/reactive/vue-reactive.vue";
import VueRefKeepingReactivity from "./components/reactive/vue-ref-keeping-reactivity.vue";
import VueRefMethod from "./components/reactive/vue-ref-method.vue";
import VueTwoRestriction from "./components/reactive/vue-two-restriction.vue";
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
