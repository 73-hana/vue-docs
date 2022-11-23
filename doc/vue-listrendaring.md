# リストレンダリング

## `v-for`

配列に基づいて項目のリストをレンダリングする場合、`v-for`ディレクティブを使用する

`v-for`には、`item in items`というような形式（`items`は元のデータ配列（複数形なので配列）を指し、`item`は反復処理の対象となっている配列の各要素を示すエイリアスである）の特別な構文を用いる（区切り文字には代わりに`of`を使い、JavaScript のイテレータ構文に近づけることも可能）

テンプレートの`v-for`スコープ内では、その親スコープである`<script>`の全てのプロパティにアクセスできる（ので、マスタッシュ構文を使える）

さらに、`v-for`では現在の項目のインデックスを指すエイリアスも使える

また、`v-for`をネストしたとしても、スコープの挙動はネストされた関数と同じである

```vue
<script setup>
import { ref } from "vue";

const nameofList = ref("Title:");
const books = ref([
  { title: "book1 - JavaScript" },
  { title: "book2 - React" },
  { title: "book3 - Vue.js" },
]);
</script>

<template>
  <p>My favorite books</p>
  <ul>
    <li v-for="(book, index) in books">
      {{ index }} {{ nameofList }} {{ book.title }}
    </li>
  </ul>
</template>
```

---

## `v-for`をオブジェクトに適用する

`v-for`はオブジェクトの各プロパティを反復処理するのにも利用できる

配列と同様に、2 つめのエイリアスを指定するとプロパティのキーを取り出すことができる

また、配列とは異なり、３つ目のエイリアスを指定するとインデックスを取り出すことができる

```vue
<script setup>
import { reactive } from "vue";

const myObject = reactive({
  title: "How to do lists in Vue",
  author: "John Doe",
  publishedAt: "2016-04-10",
});
</script>

<template>
  <p>My favorite book</p>
  <ul>
    <li v-for="(value, key, index) in myObject">
      {{ index }} {{ key }}: {{ value }}
    </li>
  </ul>
</template>
```

---

## `v-for`で範囲指定を設定する

`v-for`で連続した整数を作成することもできる（その場合は、`v-for`が適用された要素が、その回数だけ繰り返しレンダリングされる）

注意すべき点として、数値は 1 から繰り返される事に注意する

```vue
<template>
  <p v-for="n in 10">{{ n }}</p>
</template>
```

---

## `<template>`に`v-for`を適用する

テンプレートに`v-if`を適用する場合と同様に、`<template>`タグに`v-for`を適用することができる

---

## `v-for`と`v-if`を組み合わせる場合

同じノードに両方が存在する場合、`v-for`よりも`v-if`のほうが優先順位が高くなる（よって、`v-for`のスコープにある変数は、同じノードにあったとしても`v-if`で使うことは出来ない）

この問題を解決するためには、ラップ用の`<template>`タグを設けて、そこで`v-for`ディレクティブを指定することで解決できる

```vue
<script setup>
import { ref } from "vue";

const books = ref([
  { title: "JavaScript", price: "$15", isAvailable: true },
  { title: "React", price: "$20", isAvailable: false },
  { title: "Vue", price: "$20", isAvailable: true },
]);
</script>

<template>
  <p>my favorite books</p>
  <ul>
    <template v-for="book in books">
      <li v-if="book.isAvailable">
        title: {{ book.title }} ({{ book.price }})
      </li>
    </template>
  </ul>
</template>
```
