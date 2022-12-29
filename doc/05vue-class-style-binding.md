# クラスとスタイルのバインディング

データバインディングを用いることで、HTML 要素（テンプレート）に適用する CSS クラスのリストや、インラインのスタイルを自由に操作することができる

`class`も`style`もどちらも属性であるため、他の属性と同じように`v-bind`（ディレクティブ）を使用することで動的に文字列の値を割り当てることができる

しかし、文字列の連携を使ってクラス属性やスタイル属性を生成しようとすると、手間がかかり間違いが起きやすくなる

そこで、`class`や`style`に対してディレクティブを用いるとき、特別な拡張が了できるようになっている

## HTML クラスのバインディング

### オブジェクトへのバインディング

`:click`では、オブジェクトを渡して CSS クラスを動的に切り替えることができる

また、オブジェクトのプロパティを増やすように CSS クラスを増やせば、複数のクラスをトグルすることができる

また、`const isActive = ref(true)`のように`ref()`でオブジェクトを作成して`v-bind:class={ active: isActive }`のように中括弧で囲ってもいいし、`const classObj = reactive({ active: true, danger: false })`のように`reactive()`のようにオブジェクトを生成してそれをそのまま`v-bind`に渡しても良い

computed プロパティを用いて、各プロパティで計算をおこなってもいい

```vue
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
```

---

### 配列へのバインディング

`v-bind:class`の属性値にオブジェクトをバインドするところを、配列をバインドすることも可能

```vue
<script setup>
import { ref } from "vue";

const activeClass = ref("active");
const errorClass = ref("text-danger");
</script>

<template>
  <p v-bind:class="[activeClass, errorClass]">sample</p>
</template>
```

---

### コンポーネントでの利用

一度飛ばす

---

## インラインスタイルのバインディング

### オブジェクトへのバインディング

`v-bind:style`では、JavaScript のオブジェクト値へのバインディングがサポートされている

css プロパティのキーにはキャメルケースとケバブケース両方サポートされているが、ケバブケースの場合はシングルクオーテーションで囲む必要がある

また、テンプレートの見た目をスッキリさせるために、多くの場合はスタイルオブジェクトを定義して、それを直接バインドすることが多い

また、スタイルへのオブジェクトのバインディングにおいても、オブジェクトを返す computed プロパティと組み合わせることも可能

---

### 配列へのバインディング

`:style`は、ref 等のプロキシオブジェクトを含む配列をバインドすることもできる

---

### 自動プレフィックス

`:style`でベンダープレフィックスを必要とするプロパティを指定したい場合も心配いらない（Vue が適切なプレフィックスを自動で追加するから）

---

### 複数の値

また、`:style`には複数の値を配列で追加することができる（プレフィックスなどを手動で追加したい場合に有効）

この設定を行うと、配列に含まれる値の内、ブラウザでサポートされている最後の値だけがレンダリングに使われることになる
