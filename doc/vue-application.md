# Vue アプリケーションの作成

## アプリケーションのインスタンス

全ての Vue アプリケーションは`createApp`関数を使ってアプリケーションのインスタンスを新しく作成することから始まる

```javascript
import { createApp } from "vue"; // vueからcreateAppクラスをインポートする

const app = createApp({
  // ルートコンポーネントのオプション
  // ルートコンポーネントについては次の節で学ぶ
});
```

---

## ルートコンポーネント

`createApp`に渡しているオブジェクトは、実際にはコンポーネントである

全てのアプリには、他のコンポーネントを子要素として保持できる親要素（ルートコンポーネント）が必要である

もし SFC を使用している場合は、他のファイルからルートコンポーネントをインポートする

```javascript
import { createApp } from "vue";
import App from "./App.vue"; // ルートコンポーネントをSFCからインポートしている

const app = createApp(App); // createAppの引数としてルートコンポーネントを渡している
```

---

## アプリのマウント

アプリケーションのインスタンス（上記の例でいう`const app = createApp({...})`である）は、`.mount()`メソッドが呼ばれるまで何もレンダリングしない

インスタンス（`app`）にはコンテナ引数（`app.mount('#app')`）が必要

アプリのルートコンポーネント（`createApp()`の引数）のコンテンツはコンテナ要素（`mount()`の引数として指定した要素）の中でレンダリングされる

`.mount()`メソッドは、全てのアプリの設定やアセットの登録が完了した後、常に呼ばれる必要がある

また、アセット登録するメソッドとは異なり、`.mount()`の戻り値はルートコンポーネントインスタンスである

---

## DOM 内のルートコンポーネントテンプレート

ビルドなしで Vue を扱う場合、ルートコンポーネントのテンプレート（HTML のこと）をマウントコンテナ内に直接書くこともできる

_これは凄く JSX みがある_

また、ルートコンポーネントに`template`オプションが無い場合、Vue はコンテナ（今回は`<div id="app"></div>`）内の`innerHTML`をテンプレートとして使用する

```html
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
```

```js
import { createApp } from "vue";

const app = createApp({
  // ロジックを直書きしている
  data() {
    return {
      count: 0,
    };
  },
});

app.mount("#app");
```

## アプリの設定

アプリケーションのインスタンス（`app`）は`.config`オブジェクトを持っている

`.config`オブジェクトを用いて、アプリケーションレベルの設定を行うことができる

```js
app.config.errorHandler = (err) => {
  // 子孫コンポーネントから発生したエラーをcatchするエラーハンドラの定義
  // エラーの制御を記述
};
```

また、アプリケーションのインスタンスは、アプリ用のアセットを登録するいくつかのメソッドを用意している

```js
app.component("TodoDeleteButton", TodoDeleteButton);
```

アプリがマウントされる前に、アプリの設定を確認しよう！

---
