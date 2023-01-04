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

### reactive()

リアクティブなオブジェクトや配列を作るには`reactive()`関数を利用する。オブジェクトや配列のリアクティビティは、入れ子になったオブジェクトにも適用される。

値をオーバーライドしたり、プロパティの値を他の変数・関数に渡すとリアクティビティが破壊される。

```vue
<template>
  <div>
    <button @click="increment">{{ nestedObj.obj.count }}</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const nestedObj = reactive({
  obj: { count: 0 },
});

function increment() {
  nestedObj.obj.count++;
}
</script>
```

### ref()

リアクティブなプリミティブ型やオブジェクト、配列を作るには`ref()`関数を用いる。`reactive()`関数とは異なり、`ref()`で生成されたオブジェクトを他の変数・関数に渡したり、新しい値でオーバーライドしてもリアクティビティは保持されたままになる。

```vue
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
```

## computed プロパティ

マスタッシュ構文や属性の引用符の中では 1 つの JS 式しか書けないため、複雑なロジックを記述する場合には`computed()`プロパティを用いる。

`computed()`プロパティはロジックの結果をキャッシュすることができる。一時的なスナップショットを取る機構である。よって、複雑なロジックを実行するのも最小回数で済む。

```vue
<template>
  <div>
    <p>Has published books: {{ publishedBooksMessage }}</p>
  </div>
</template>

<script setup>
import { reactive, computed } from "vue";

const author = reactive({
  name: "John Doe",
  books: [
    "Vue 2 - Advanced Guide",
    "Vue 3 - Basic Guide",
    "Vue 4 - The Mystery",
  ],
});

const publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? "Yes" : "No";
});
</script>
```

`computed()`プロパティの場合は、戻り値もリアクティブになる。`computed()`プロパティはリアクティブな依存関係を把握しているため、依存先の変更を感知してキャッシュを更新することができる。

対してテンプレート内の関数の場合は、関数を呼び出すたびに関数が実行されるため、複雑なロジックを都度実行してしまう。

よって、常に実行させたい処理（Date メソッドなど）は関数で処理を行い、巨大な配列や多くの計算を処理する場合は`computed()`プロパティを用いるのが良い。

### `computed()`プロパティのベストプラクティス

- `computed()`プロパティでは、値の計算や処理だけを行い、非同期リクエストや DOM 操作など副作用が発生する処理は行わない。
- `computed()`プロパティの戻り値をわざわざ変更するのは避ける。変更したい場合は依存先を変更する方が良い。
