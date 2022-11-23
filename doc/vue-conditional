# 条件つきレンダリング

## `v-if`と`v-else`、`v-else-if`

`v-if`ディレクティブは、ブロックを条件に応じてレンダリングしたい場合に使用される

ブロックは、ディレクティブの式が truthy の場合のみレンダリングされる

`v-if`に対応する"else block"を示すために、`v-else`ディレクティブを使用できる

また、"else if block"を示したい場合は`v-else-if`ディレクティブを使用できる

`v-else-if`は`v-if`と`v-else`の間に何度も連結することができる

```vue
<script setup>
import { ref } from "vue";

const isAwesome = ref("awesome");

function swichAwesome() {
  isAwesome.value = "awesome";
}

function switchSoSo() {
  isAwesome.value = "so-so";
}

function switchBad() {
  isAwesome.value = "bad";
}
</script>

<template>
  <p v-if="isAwesome === 'awesome'">Vue is awesome!</p>
  <p v-else-if="isAwesome === 'so-so'">Vue is so-so.</p>
  <p v-else>Vue is not good...</p>
  <button @click="swichAwesome">Awesome</button>
  <button @click="switchSoSo">SoSo</button>
  <button @click="switchBad">Bad</button>
</template>
```

---

## `<template>`での`v-if`

`v-if`はディレクティブなので、単一の要素に付加する必要があるが、多くの要素をまとめてしまって、それごと表示非表示を切り替えてしまいたいこともある

その場合は、`<template>`要素で`v-if`を使い、`<template>`をラッパーとして使用することができる

---

## `v-show`

`v-if`と同様に要素の表示非表示をつかさどる方法として、`v-show`が用意されている

使用方法はほとんど同じだが、`v-show`で表示非表示を切り替える要素は常にレンダリングされ、`v-if`で表示非表示を切り替える要素は結果次第でレンダリングされるかどうかが決まる

`v-show`要素の挙動はシンプルで、要素の CSS プロパティのうち`display`を切り替えるだけである

```vue
<script setup>
import { ref } from "vue";

const isAwesome = ref(true);

function switchAwesome() {
  isAwesome.value = !isAwesome.value;
}
</script>

<template>
  <p v-show="isAwesome">Vue is awesome!</p>
  <p v-show="!isAwesome">Vue is not good...</p>
  <p v-if="isAwesome">React is also awesome!</p>
  <p v-else>React is not good too...</p>
  <button @click="switchAwesome">Click</button>
</template>
```

---

## `v-if`と`v-show`

`v-if`はイベントリスナと子コンポーネント内部の条件ブロックが適切に破棄され、また再生成されるため、リアルな条件レンダリングである

`v-if`は遅延レンダリングでもある（つまり、条件が true になるまでレンダリングされない）

一方で、`v-show`は要素は条件に関わらず常にレンダリングされ、シンプルな CSS の切り替えを提供する

一般的に`v-if`は高い切り替えコストを持っているのに対して、`v-show`はより高い初期レンダリングコストを持っている

そのため、とても頻繁に切り替える必要がある場合は`v-show`を用いて、条件が実行時にほとんど変わらない場合は`v-if`を選ぶ

---

## `v-if`と`v-for`

暗黙的な優先順のせいで、`v-if`と`v-for`を同じ要素で利用することは推奨されていない

`v-if`と`v-for`が同じ要素に両方使う場合は、`v-if`が先に評価される

---
