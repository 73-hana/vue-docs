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

## DOM 更新のタイミング

リアクティブな状態を変化させると、DOM は自動的に更新されるが、その更新は同期的ではない

Vue は更新サイクルの next tick まで更新をバッファリングし、どれだけ状態を変化させても 1 度だけ更新されることを保証してくれる

状態が変化されたあとの DOM 更新を待つため、`nextTick()`というグローバル API を利用できる

## ディープなリアクティビティ―

Vue では、状態はデフォルトでリアクティビティである

入れ子になっている配列やオブジェクトが変化された場合も変更が検出される

```js
<script setup>
import { reactive } from "vue";

const obj = reactive({
  nested: {
    count: 0,
  },
  arr: ["foo", "bar"],
});

function mutateDeeply() {
  obj.nested.count++;
  obj.arr.push("baz");
}
</script>

<template>
  <button @click="mutateDeeply">{{ obj.arr }}</button>
</template>

```

---

## リアクティブプロキシと独自

`reactive()`関数を使用する上で注意すべきなのは、`reactive()`関数の戻り値が元のオブジェクトのプロキシである（つまり、元のオブジェクトと等しくない）ということである

元のオブジェクトがリアクティブになるのではなく、元のオブジェクトのプロキシがリアクティブになるということなので、元のオブジェクトを変更しても DOM の更新は行われない

```vue
<script setup>
import { reactive } from "vue";

const rawObject = {
  count: 0,
};
const proxyObject = reactive(rawObject);
</script>

<template>
  <div>{{ proxyObject === rawObject ? "same" : "not same" }}</div>
</template>
```

また、元のオブジェクトのプロキシに対して改めて`reactive()`関数を呼び出すと、元のプロキシと等しいプロキシが返される

```vue
<script setup>
import { reactive } from "vue";

const rawObj = {
  count: 0,
};
const proxyObj = reactive(rawObj);
</script>

<template>
  <div>
    proxyObj and rawObj are
    {{ proxyObj === rawObj ? "same" : "not same" }}
  </div>
  <div>
    reactive(proxyObj) and proxyObj are
    {{ reactive(proxyObj) === proxyObj ? "same" : "not same" }}
  </div>
</template>
```

上記のルールはネストされたオブジェクトにも適用されるものである

---

## `reactive()`の制限

`reactive()`API には２つの制限がある

- オブジェクト型（オブジェクト、配列、map()や set()などのコレクション型）を引数に持つが、プリミティブ型は引数に持てない

- リアクティビティ追跡はプロパティへのアクセスで行われるため、引数に持ったオブジェクト型のアドレスを一定に保つ必要がある（例えば再代入したり、プロパティの値を他の変数に代入したりするとリアクティブなつながりがなくなる）

```vue
<script setup>
import { reactive } from "vue";

// 上書きされる恐れがあるので、constを使う
let state = reactive({
  count: 0,
});

function increment() {
  state.count++;
}

function overwriteReactive() {
  state = reactive({
    count: 1,
  });
}

let n = state.count;
function incrementN(n) {
  return n++;
}

function addone(number) {
  return number++;
}
</script>

<template>
  <div>
    <p>リアクティブなオブジェクトを置き換えたとき</p>
    <button @click="increment">Click count: {{ state.count }}</button>
    <button @click="overwriteReactive">Overwrite</button>
  </div>
  <div>
    <p>リアクティブなオブジェクトのプロパティの扱い</p>
    <p>Current state.count: {{ state.count }}</p>
    <button @click="incrementN(n)">increment n</button>
    <button @click="addone(state.count)">function argment</button>
  </div>
</template>
```

---
