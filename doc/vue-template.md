# テンプレート構文

Vue では、HTML ベースのテンプレート構文を使用する

テンプレート構文では、基盤となるコンポーネントインスタンスのデータ（`data()`のこと）とレンダリングされる DOM（`template()`のこと）を宣言的にバインドすることが可能

全ての Vue テンプレートは、構文的に正規の HTML である

_これ React と違うところなのかな_

内部では、Vue はテンプレートをコンパイルして JavaScript にする

リアクティビティ機構と組み合わされて、再レンダリングされるべき部分をすぐに察知することができる

オプションで JSX や VanillaJS の`render`関数を選択する事もできるが、コンパイルの最適化が上手くいかないかもしれない

---

## テキスト展開

データバインディング（おそらく HTML と JS の組み合わせのこと）で最も基本の形式はマスタッシュ構文（二重中括弧によるテキスト展開

マスタッシュ構文の中身は、対応するコンポーネントのインスタンスが持つ`msg`というプロパティの値に書き換えられる

```html
<span>Message: {{ msg }}</span>
```

---

## 生の HTML

マスタッシュ構文の中では、データが HTML ではなくプレーンテキストとして解釈される

本来の HTML を出力する場合は`v-html`ディレクティブを用いる必要がある

```js
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">
    <p>{{ msg }}</p> // プレーンテキストのまま表示される
    <p v-html="msg"></p> // テキストが赤く表示される
</div>

<script type="module">
    const { createApp } = Vue;

    createApp({
        data() {
            return {
                msg: '<span style="color: red">There is no message!</span>'
            }
        }
    }).mount('#app')
</script>
```

`v-html`という属性は、ディレクティブと呼ばれる

ディレクティブは`v-`という接頭辞をもち、Vue によって提供される特別な属性であることを示す（振る舞いや役割りを割り当てられる）

_HTML 書いているみたいに、挙動をコントロールできるってことかな_

---

## 属性バインディング

HTML 要素の内容をリアクティブにするにはマスタッシュ構文が有効だが、HTML 属性の中ではマスタッシュ構文が使えない

代わりに`v-bind`ディレクティブを使用する

```js
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">
    <p>{{ msg }}</p>
    <p v-bind:style="dynamicStyle">test</p> // p要素のstyle属性の値をdynamicStyleから取得している
    <p :style="dynamicStyle">test</p> // v-bindディレクティブは省略可能
    <p :style="nullData">test</p> // バインドされる値がnullまたは
    <p :style="undefinedData">test</p> // undefinedの場合は、レンダリングされない
</div>

<script type="module">
    const { createApp } = Vue

    createApp({
        data() {
            return {
                msg: "Hello!",
                dynamicStyle: "color: red"
            }
        }
    }).mount('#app')
</script>
```

---

## ブーリアン属性

属性値を持たない（もしくは、属性値が true もしくは false のみの）属性をブーリアン属性というが、v-bind 下ではブーリアン属性は特別な動作をする

バインドされるプロパティが真値（truthy value）の場合のみ属性が付与され、偽値（falsy value）の場合はレンダリングされない

```js
<button :disabled="isButtonDisabled">Click</button>

...

createApp({
    data() {
        return {
            isButtonDisabled: true,
        }
    }
}).mount('#app')
```

---

## 複数の属性を動的にバインドさせる

複数の属性をバインドするには、下記のように記述するだけでいい

```js
<div v-bind="objectOfAttrs"></div>
```

具体例としては以下の通り

```js
<div id="app">
    <button v-bind="redButton">Click Me</button>
    <button v-bind="blueButton">Click Me</button>
</div>

<script type="module">
    const { createApp } = Vue

    createApp({
        data() {
            return {
                redButton: {
                    style: 'color: red',
                    disabled: false,
                },
                blueButton: {
                    style: 'color: blue',
                    disabled: true,
                },
            }
        }
    }).mount('#app')
</script>
```

---

## JavaScript の式を用いる

Vue では、あらゆるデータバインディングにおいて、JavaScript 式を活用できる

現在のコンポーネントインスタンスのデータスコープ内で、JavaScript の式として評価される

Vue のテンプレートでは、以下の場所で JavaScript の式を使用できる

- テキスト展開の内部（マスタッシュ構文の内部）
- 任意の Vue ディレクティブ（`v-`で始まる属性）の属性値の中身

しかし、それぞれのバインディングは単一の式しか含めることはできない（return の後に含められる式しか対応していない）

---
