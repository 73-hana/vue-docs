# テンプレート構文

Vue では、HTML ベースのテンプレート構文を使用する。

テンプレート構文では、基盤となるコンポーネントインスタンスのデータ（`data()`のこと）とレンダリングされる DOM（`template()`のこと）を宣言的にバインドすることが可能。

内部では、Vue はテンプレートをコンパイルして JavaScript にする。

リアクティビティ機構と組み合わされて、再レンダリングされるべき部分をすぐに察知することができる。

オプションで JSX や VanillaJS の`render`関数を選択する事もできるが、コンパイルの最適化が上手くいかないかもしれない。

---

## 文字列のバインディング（テキスト展開）

データバインディングで最も基本の形式はマスタッシュ構文（二重中括弧によるテキスト展開。

マスタッシュ構文の中身は、対応するコンポーネントのインスタンスが持つ`msg`というプロパティの値に書き換えられる。このプロパティは`<script setup>`にて定義される変数も含む。

```html
<span>Message: {{ msg }}</span>
```

---

## HTML のバインディング（生の HTML）

マスタッシュ構文の中では、データが HTML ではなくプレーンテキストとして解釈される

本来の HTML を出力する場合は`v-html`ディレクティブを用いる必要がある

```vue
<template>
  <div>
    <p>Using text interpolation: {{ rawHTML }}</p>
    <p>Using v-html directive: <span v-html="rawHTML"></span></p>
  </div>
</template>

<script setup>
const rawHTML = "<span style='color: red;'>text in span</span>";
</script>
```

`v-html`という属性は、ディレクティブと呼ばれる特別な属性である。

ディレクティブは、レンダリングされる DOM に対して特別な振る舞いを割り当てる役割りを持つ。

---

## 属性バインディング

HTML 要素の内容には（テキストバインディングとして）マスタッシュ構文が有効だが、HTML 要素の属性にはマスタッシュ構文が使えない。代わりに`v-bind`ディレクティブを使用する。

`v-bind`ディレクティブは使用頻度が高いため、省略記法が利用できる。

```vue
<p v-bind:xxx="">sample</p>
<!-- xxxに属性名を指定する -->
<p :xxx="">sample</p>
<!-- 省略記法 -->
```

```vue
<template>
  <div>
    <p v-bind:id="dynamicId">This element has "{{ dynamicId }}" as id.</p>
    <p :id="dynamicId">
      This element has "{{ dynamicId }}" as id and written with abbreviated
      notation.
    </p>
  </div>
</template>

<script setup>
const dynamicId = "originalId";
</script>
```

---

## ブーリアン属性

`v-bind`が論理属性を扱う場合、通常の属性を扱う場合とは異なり、バインドされるプロパティが真値（truthy value）の場合のみ属性が付与される。偽値（falsy value）の場合はレンダリングされない。

```js
<template>
  <div>
    <p>This button has v-bind:disabled with truthy value (ok).</p>
    <button :disabled="isButtonDisabled">sample button</button>
  </div>
</template>

<script setup>
const isButtonDisabled = "ok";
</script>
```

---

## 複数の属性を動的にバインドさせる

複数の属性を 1 回でバインドさせたいときには、`v-bind:xxx`のように属性名を指定せず、値に属性名と値を持つオブジェクトを指定する。

```vue
<template>
  <div>
    <p v-bind="objAttr">sample text</p>
  </div>
</template>

<script setup>
const objAttr = {
  id: "text",
  style: "color: blue;",
};
</script>
```

---

## JavaScript の式を用いる

Vue では、あらゆるデータバインディングにおいて、JavaScript 式を活用できる。

データバインディングが記述されているコンポーネントのスコープ内での式として評価される。

Vue のテンプレートでは、以下の場所で JavaScript の式を使用できるが、それぞれのバインディングは単一の式しか含めることはできない（return の後に含められる式しか対応していない）。

- テキスト展開の内部（マスタッシュ構文の内部）
- 任意の Vue ディレクティブ（`v-`で始まる属性）の属性値の中身

```vue
<template>
  <div>
    <p>Is "ok" true? :{{ ok ? "Yes" : "No" }}</p>
    <p :id="`list-${id}`">This p element has v-bind:id with JS expression.</p>
  </div>
</template>

<script setup>
const ok = true;
const id = "1234";
</script>
```

---

## 関数の呼び出し

コンポーネントから公開されているメソッドであれば、バインディングの式の内部で呼び出すことができる。

```js
<span :title="toTitleDate(date)">{{ formatDate(date) }}</span>
```

バインディングの式の内部で呼び出される関数はコンポーネントが更新されるたびに呼び出される関数になるので、データの変更や非同期処理を行う関数を持たせてはいけない。

---

## グローバルへのアクセスの制限

テンプレートで用いる式からは、限定的なグローバルのリスト（`Math`や`Date`など）にのみアクセスできる。ユーザが独自に`window`に付与したプロパティなどにはアクセスできない。
アクセスできるようにするためには、`app.config.globalProperties`にプロパティを追加することでアクセスできるようになる。

## ディレクティブ

ディレクティブは、`vi`という接頭辞を持つ特別な属性である。

ディレクティブの属性値は JavaScript の単一の式であることが期待されている（`v-for`、`v-on`、`v-slot`は例外）。

ディレクティブの役割は、値の変化（変数の再代入や式の評価）があったときにリアクティブに DOM を更新することである。

```vue
<p v-if="seen">Now you see me</p>
```

---

## 引数

一部のディレクティブは引数を取ることができる。個々での引数は属性値とは異なる。

引数は、ディレクティブ名の後にコロンで示す。

```vue
<!-- hrefが引数 -->
<a v-bind:href="url">...</a>
<!-- v-bind:を省略して:にしている -->
<a :href="url">...</a>
```

DOM のイベントをリッスンするディレクティブである`v-on`ディレクティブも引数を持つ。省略記法は`v-bild`の時と異なる。

```vue
<!-- clickが引数（clickイベントを指している） -->
<a v-on:click="doSomething">...</a>
<!-- v-on:clickを省略して@にしている -->
<a @click="doSomething">...</a>
```

```vue
<template>
  <div>
    <p v-bind:style="sampleStyle">sample</p>
    <p :style="sampleStyle">sample</p>
    <button v-on:click="handleAlert">v-on:click</button>
    <button @click="handleAlert">@click</button>
  </div>
</template>

<script setup>
const sampleStyle = {
  color: "yellow",
  fontWeight: "bold",
};
function handleAlert() {
  alert("hello");
}
</script>
```

---

## 動的引数

ディレクティブの引数には、リテラルだけではなく変数も使える。

動的引数は、評価結果が`null`または文字列のいずれかになる事が期待される（`null`はバインディングを明示的に削除する役割りを持つ特別な値）。

```vue
<template>
  <div>
    <a :[attrName]="url">sample anchor</a>
  </div>
</template>

<script setup>
const attrName = "href";
const url = "#";
</script>
```

---

## 修飾子

ディレクティブを何らかの特別な方法でバインドすることを明示的に表す際に使用する記法を修飾子という。これはドットを用いて記される。

```vue
<template>
  <div>
    <form>
      <p>form without preventDefault()</p>
      <input type="submit" />
    </form>
    <form @submit.prevent="onSubmit">
      <p>form with preventDefault()</p>
      <input type="submit" />
    </form>
  </div>
</template>
```

---
