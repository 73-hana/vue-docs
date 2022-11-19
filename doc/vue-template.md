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
