#リアクティビティの基礎

リアクティブなオブジェクトや配列を作るには、`reactive()`関数を使用する

Vue はリアクティブなオブジェクトのプロパティへ、アクセスでき変更を追跡できる

```js
<script>
import { reactive } from "vue";

export default {
  setup() {
    const state = reactive({
      count: 0,
    });
    return {
      state,
    };
  },
};
</script>

<template>
  <div>
    {{ state.count }}
  </div>
</template>
```

リアクティブな状態を変化させる関数を同じスコープで宣言することもできる（状態と同じように公開されるため、メソッドとしてアクセス可能）

```js
<script>
import { reactive } from "vue";

export default {
  setup() {
    const state = reactive({
      count: 0,
    });

    function increment() {
      state.count++;
    }

    return {
      state,
      increment,
    };
  },
};
</script>

<template>
  <div>
    <button @click="increment">{{ state.count }}</button>
  </div>
</template>
```

---

## <script setup>

`setup()`関数を使って手動で状態やメソッドを取り扱うと冗長化する恐れがある

SFC を利用する場合は`<script setup>`を使用する事で大幅に簡略化することができる

```
<script setup>
import { reactive } from "vue";

const state = reactive({
  count: 0,
});

function increment() {
  state.count++;
}
</script>

<template>
  <button @click="increment">
    {{ state.count }}
  </button>
</template>
```

トップレベルのインポートと、`<script setup>`で宣言された変数は、同じコンポーネントのテンプレートで自動的に利用できるようになる

---
