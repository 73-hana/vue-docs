# 算出プロパティ

## 基本的な例

テンプレート内に式を書けるのは便利だが、非常に簡単な操作しかできない

テンプレート内に多くのロジックを詰め込んでしまうと、コードが長くなり、後々読みにくく修正しにくくなってしまう

リアクティブなデータを含む複雑なロジックには算出プロパティを利用する

```vue
<script setup>
import { reactive, computed } from "vue";

const author = reactive({
  name: "John Doe",
  books: ["Book1 - for beginner", "Book2 - Basic", "Book3 - Advanced"],
});

const publishedBooks = computed(() => {
  return author.books.length > 0 ? "yes" : "no";
});
</script>

<template>
  <p>
    Favorite Books?
    <span>{{ publishedBooks }}</span>
  </p>
</template>
```

`computed()`関数は getter 関数が渡される事を想定しており、返り値は算出された ref となる（つまり`publishedBooks`の値と`publishedBooks.value`はおなじ）

また、ref が返り値のため、マスタッシュ構文内では`.value`入らない

`computed()`で算出された ref プロパティは、自動的にリアクティブにする範囲を追跡する（たとえば、`publishedBooks`は`author.books`に依存すること察知し、`author.books`の変化も感知できるようになる）

---

## computed プロパティとメソッド

computed プロパティを使わなくても、コンポーネント内で関数（性格にはメソッド）を持つことで同じ結果を得られる

```vue
const computedWay = computed(() => { return obj.innerObj.value ? "yes" : "no";
}) function functionalWay() { return obj.innerObj.value ? "yes" : "no"; }
```

最終的には、computed プロパティとメソッドの 2 つの方法を使っても、結果は同じである

しかし、computed プロパティはリアクティブな依存関係を参照してキャッシュを残してくれるという違いがある（つまり、computed プロパティにアクセスしても、依存関係先が変更されていなかったら、キャッシュを即座に返すということ）

対照的に、メソッド呼び出しは再描画が起きると常に関数が実行されるようになっている

```vue
<script setup>
import { reactive, computed } from "vue";

const today = reactive({ date: Date.now() });

const computedDate = computed(() => today.date);
function functionalDate() {
  return today.date;
}
</script>

<template>
  <p>{{ computedDate }}</p>
  <p>{{ functionalDate() }}</p>
</template>
```

---
