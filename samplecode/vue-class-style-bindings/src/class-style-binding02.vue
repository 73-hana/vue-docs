<script setup>
import { reactive, ref, computed } from "vue";

const isActive = ref(true);
const hasError = ref(false);

const classObj = reactive({
  active: true,
  "text-danger": false,
});

const error = ref(null);

const computedObj = computed(() => ({
  active: isActive.value && !error.value,
  "text-danger": error.value && error.value.type === "fatal",
}));
</script>

<template>
  <button
    class="static"
    v-bind:class="{ active: isActive, 'text-tanger': hasError }"
  >
    click me
  </button>
  <button :class="classObj">Click Me</button>
  <button :class="computedObj">CLick</button>
</template>
