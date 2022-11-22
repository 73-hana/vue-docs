<script setup>
import {reactive, ref, computed } from "vue";

const isActive01 = ref(true);
const hasError01 = ref(true);

const classObj01 = reactive({
  active: true,
  error: false,
});

const justBool = ref(true);
const classObj02 = computed(() => ({
  active: isActive01.value && justBool.value,
  error: hasError01.value || justBool.value,
}));

const isActive02 = ref(true);
const hasError02 = ref(true);

</script>

<template>
  <p v-bind:class="{ active: isActive01, error: hasError01 }">クラス内でオブジェクトを定義し、オブジェクトの各プロパティの値にref()オブジェクトを挿入</p>
  <p v-bind:class="classObj01">reactive()オブジェクトを定義し、そのプロパティにブーリアンを代入</p>
  <p v-bind:class="classObj02">computed()オブジェクトを定義し、その中で計算を行い結果に基づいてクラスを付与</p>
  <p v-bind:class="[isActive02, hasError02]">クラス内で配列を定義し、各要素にref()オブジェクトを挿入</p>
</template>