# リアクティビティの基礎

## リアクティブな状態を宣言する

リアクティブなオブジェクトや配列を作るには、`reactive()`関数を使用する。Vue はリアクティブなオブジェクトのプロパティへ、アクセスでき変更を追跡できる。

コンポーネントのテンプレート内でリアクティブな状態を扱うには、コンポーネントの`setup()`関数で`const xxx = reactive({...})`のように宣言し、それを返す。

注意：下のコードは SFC（script setup）を使わない書き方です。

```vue
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

`reactive()`で設定した変数の状態を変化させる関数も、同じスコープで宣言することができる。この関数も返す（公開する）必要がある。

注意：下のコードは SFC（script setup）を使わない書き方です。

```vue
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

## `ref()`と共に使うリアクティブな変数

Vue は`reactive()`の制限に対処するため、`ref()`という関数を用意している

`ref()`を用いることで、任意の値の型を保持できるリアクティブな refs を作成できる

`ref()`は引数を（プリミティブ型も可能！）受け取り、それを`.value`プロパティを持つ ref オブジェクトにラップして返す

リアクティブなオブジェクトのプロパティと同様に、`ref()`の`.value`プロパティはリアクティブになる

つまり、`ref()`を使うと、任意の値への参照を作り、リアクティビティを失わずにポインタを受け渡すことができる

```vue
<script setup>
import { ref } from "vue";

const count = ref(0);

function increment() {
  count.value++;
  console.log(count.value);
}
</script>

<template>
  <button @click="increment">Click me</button>
</template>
```

また、引数としてオブジェクト型の値を代入する場合も、オブジェクト全体をリアクティブにすることができる

また、`ref()`で生成されたオブジェクト（ref()の返り値）を他の関数に渡したり、オブジェクトから分解したりしても、リアクティビティは保持されたままである

_ここが`reactive()`との大きな違い_

```vue
<script setup>
import { ref } from "vue";

const objRef = ref({
  count: 0,
});

function increment() {
  objRef.value.count++;
}

function overwriteObjRef() {
  objRef.value = {
    count: 0,
  };
}
</script>

<template>
  <div>
    <p>refの返り値であるオブジェクトの.valueを書き換える</p>
    <span>Current count: {{ objRef }}</span>
    <button @click="increment">Add one</button>
    <button @click="overwriteObjRef">Caution! Overwrite!</button>
  </div>
</template>
```

## テンプレートでの Ref の挙動

ref がテンプレートのトップレベルプロパティとしてアクセスされた場合、それらは自動的にアンラップされる

そのため、`.value`を使用する必要はない

アンラップは、ref がテンプレートに描画されるコンテキスト上のトップレベルのプロパティ（ネストされていない状態）である場合のみ適用される

```vue
<script setup>
import { ref } from "vue";

const obj = {
  foo: ref(0),
};

const { foo } = obj;
</script>

<template>
  <div>
    <p>refの挙動</p>
    <p>const obj = { foo: ref(0) };</p>
    <p>obj.foo + 1 = {{ obj.foo + 1 }}</p>
    <p>obj.foo.value + 1 = {{ obj.foo.value + 1 }}</p>
    <hr />
    <p>const { foo } = obj;</p>
    <p>foo + 1 = {{ foo + 1 }}</p>
  </div>
</template>
```

---

## リアクティブなオブジェクトにおける Ref のアンラッピング

`reactive()`関数で生成されたオブジェクトのプロパティとして`ref()`でさらにオブジェクトを生成する

そのオブジェクトにアクセスしたり変化させたりすると、自動的にアンラップされる

また、ref から生成された既存のプロパティに、新しい ref が割り当てられた場合、上書きされる

```vue
<script setup>
import { reactive, ref } from "vue";

const count = ref(0);
const state = reactive({
  count,
});

console.log(count.value);

function increment() {
  state.count++;
}

function overwriteRef() {
  const otherCount = ref(5);
  state.count = otherCount;
}
</script>

<template>
  <p>state.count is {{ state.count }}</p>
  <p>
    execute
    <code>state.count++</code>
    <button @click="increment">Click</button>
  </p>
  <p>now, count.value (count) is {{ count }}</p>
  <p>
    execute <code>state.count = otherCount</code
    ><button @click="overwriteRef">Click</button>
  </p>
  <p>now, count.value is {{ count }}</p>
</template>
```

---
