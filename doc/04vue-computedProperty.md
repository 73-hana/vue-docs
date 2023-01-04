# 算出プロパティ

## 基本的な例

マスタッシュ構文や属性の引用符の中で JS の式を書くことはできるが、非常に簡単な操作しかできない。多くの式を詰め込んでしまうと、可読性が下がり修正しにくくなる。

もしリアクティブなデータを含む複雑なロジックを記述したい場合は computed プロパティを使う。

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

`computed()`関数の返り値は`ref`であるので、`publishedBooks`と`publishedBooks.value`は同じ値を返す。また、マスタッシュ構文内では`.value`は要らない。

`computed()`関数を用いず、通常の関数でロジックを作る場合、返り値がリアクティブにならない。そのため、返り値もリアクティブにしたい場合は`computed()`を使う必要がある。

また、`computed()`関数で作られた ref は自動的にリアクティブにするべき依存関係を追跡する。例えば、`publishedBooks`は`author.books`に依存することを把握しているため、`author.books`が変わると`publishedBooks`が使われているバインディングを全て更新する。

---

## computed プロパティとメソッド

`computed()`関数を使わなくても、コンポーネント内で関数（メソッド）を使うことで同じ結果を得られる。

`computed()`プロパティはリアクティブな依存関係を把握しているため、リアクティブな依存関係に基づきキャッシュを保持することができる。上記の例の場合は、`author.books`が変化しない限りは`publishedBooks`に何度アクセスしてもキャッシュされた値を返すだけである。

下記の例では、リアクティブな`name`に依存する`introduction`と、リアクティブな依存先がない`now`がある。この場合`now`はキャッシュされた値を返すだけになってしまう。

```vue
<template>
  <div>
    <p>This text depends on ref: {{ introduction }}</p>
    <p>This text depends on primitive: {{ now }}</p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const name = ref("john doe");
const introduction = computed(() => {
  return `My name is ${name.value}`;
});

const now = computed(() => Date.now());
</script>
```

対してコンポーネント内の関数の場合は呼びだされる度に常に実行される。

よって、常に実行させたい処理（Date メソッドなど）は関数で処理を行い、巨大な配列や多くの計算を処理する場合は`computed()`プロパティを用いるのが良い。

---

## 書き込み可能な`computed`関数

算出プロパティはデフォルトでは`getter`関数のみを保持している。書き込み可能な`computed()`プロパティを作る場合は明示的に`getter`関数と`setter`関数を定義する必要がある。

## ベストプラクティス

### getter 関数は副作用のないものでなければならない

`computed()`プロパティでは値の計算や処理だけを行うのがベターである。非同期リクエストや DOM 操作等を行うのはやめる。役割りをしっかり分担させる。

### 算出した値の変更を避ける

`computed()`プロパティはキャッシュを保持する機構のため、戻り値は一時的なスナップショットとして扱うのが良い。依存先が変わる度にスナップショットが更新されるため、わざわざ戻り値を手動で変更する必要がない。
