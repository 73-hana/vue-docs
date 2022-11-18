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
