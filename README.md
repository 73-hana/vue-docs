# Vue.js Doc まとめ

## アプリケーションのインスタンス

全ての vue アプリケーションは、`main.js`内で`createApp()`関数を用いてアプリケーションのインスタンスを新しく生成する。`createApp()`の引数にはルートコンポーネントを渡す。

アプリケーションのインスタンスは`.mount()`メソッドが呼ばれて初めてレンダリングされる。`.mount()`メソッドの引数にはレンダリング先のコンテナ要素（DOM 要素）を指定する。

```js:main.js
import { createApp } from "vue";
import App from "./App.vue"; // ルートコンポーネント

const app = createApp(App);
app.mount("#app");
```

```html:index.html
<div id="app"></div>
```

---

## テンプレート構文

### 文字列のバインディング

データバインディングの基本形はマスタッシュ構文。

```vue
<template>
  <span>Message: {{ msg }}</span>
</template>

<script setup>
const msg = "sample text";
</script>
```

### HTML のバインディング

HTML 要素を出力するには`v-html`ディレクティブを用いる。

```vue
<template>
  <span v-html="rawHTML"><span>
</template>

<script setup>
const rawHTML = "<span>text in span</span>";
</script>
```

### 属性のバインディング

HTML 要素の属性に文字列をバインディングするには`v-bind`ディレクティブを用いる。`v-bind`ディレクティブは省略できる。

`v-bind`が論理属性を扱う場合、バインドされるプロパティが真値の場合のみ属性がレンダリングされる。

複数の属性を 1 回でバインドさせたい場合は、`v-bind`に引数を指定せず、値にオブジェクトを指定する。

```vue
<template>
  <!-- 属性のバインディング -->
  <p v-bind:id="dynamicId">sample p element</p>
  <p :id="dynamicId">sample p element</p>
  <!-- 論理属性のバインディング -->
  <p :disabled="isButtonDisabled">sample p element</p>
  <!-- 複数の属性を一括バインディング -->
  <p v-bind="attrObj">sample p element</p>
  <p></p>
</template>

<script setup>
const dynamicId = "red";
const isButtonDisabled = "ok";
const attrObj = {
  id: "text",
  style: "color: blue;",
};
</script>

<style scoped>
#red {
  color: red;
}
</style>
```

---

## リアクティブ
