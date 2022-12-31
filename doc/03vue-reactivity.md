# リアクティビティの基礎

## リアクティブな状態を宣言する

リアクティブなオブジェクトや配列を作るには、`reactive()`関数を使用する。Vue はリアクティブなオブジェクトのプロパティへアクセスでき、変更を追跡できる。

`<script setup>`を持たないコンポーネントのテンプレート内でリアクティブな状態を扱うには、コンポーネントの`setup()`関数で`const xxx = reactive({...})`のように宣言し、それを返す方法がある。

注意：下のコードは SFC と`<script setup>`を使わない書き方です。

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

注意：下のコードは SFC と`<script setup>`を使わない書き方です。

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

`setup()`関数を使って状態やメソッドを取り扱うと、`return`を記述する必要があるため冗長化する恐れがある。

SFC を利用する場合は`<script setup>`を使用する事で大幅に簡略化することができる。トップレベルのインポートと`<script setup>`で宣言された関数は、同じコンポーネントのテンプレート内で利用可能。

```vue
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

---

## DOM 更新のタイミング

リアクティブな状態を変化させると、DOM は自動的に更新されるが、その更新は同期的ではない。

Vue は更新サイクルの next tick まで更新をバッファリングし、どれだけ状態を変化させても 1 度だけ更新されることを保証してくれる。

状態が変化されたあとの DOM 更新を待つため、`nextTick()`というグローバル API を利用できる。

## ディープなリアクティビティ―

値に配列やオブジェクトを持つ配列やオブジェクトもリアクティブになっている。入れ子になっている配列やオブジェクトを変化させた場合も DOM 変更が起きる。

```vue
<template>
  <div>
    <button @click="changeNestedValue">{{ nestedObj.arr }}</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const nestedObj = reactive({
  obj: { count: 0 },
  arr: ["foo", "bar"],
});

function changeNestedValue() {
  nestedObj.obj.count++;
  nestedObj.arr.push("baz");
}
</script>
```

---

## リアクティブプロキシと独自

`reactive()`関数を使用する上で注意すべきなのは、`reactive()`関数の戻り値が元のオブジェクトのプロキシである（元のオブジェクトと等しくない）ということである。つまり、元のオブジェクトがリアクティブになるのではなく、元のオブジェクトのプロキシがリアクティブになるということである。

Vue のリアクティブシステムを利用する際のベストプラクティスは、プロキシされた状態の配列・オブジェクト（`reactive()`で生成された配列・オブジェクト）のみを利用する事である。

```vue
<template>
  <div>
    <p>raw: {{ raw }}</p>
    <p>rawProxy: {{ rawProxy }}</p>
    <button @click="pushHoge">add hoge</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";
const raw = {
  key1: "foo",
  key2: "bar",
};
const rawProxy = reactive(raw);

function pushHoge() {
  rawProxy.key3 = "hoge";
}
</script>
```

また、元のオブジェクトのプロキシに対して再び`reactive()`関数を呼び出すと、元のプロキシと等しいプロキシが返される。

```vue
<template>
  <div>
    <p>Are rawProxy and raw same?:{{ rawProxy === raw ? "yes" : "no" }}</p>
    <p>
      Are rawProxy and rawProxyAgain same?:
      {{ rawProxy === rawProxyAgain ? "yes" : "no" }}
    </p>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const raw = {
  foo: "foo",
  bar: "bar",
};
const rawProxy = reactive(raw);
const rawProxyAgain = reactive(rawProxy);
</script>
```

---

## `reactive()`の制限

`reactive()`API には２つの制限がある:

- オブジェクト型（オブジェクト、配列、map()や set()などのコレクション型）を引数に持つが、プリミティブ型は引数に持てない。

- リアクティビティ追跡はプロパティへのアクセスで行われるため、引数に持ったオブジェクト型のアドレスを一定に保つ必要がある（例えば再代入したり、プロパティの値を他の変数に代入したりするとリアクティブなつながりがなくなる）。

```vue
<template>
  <div>
    <p>count: {{ count.count }}</p>
    <p>n: {{ n }}</p>
    <button @click="increment">increment</button>
    <button @click="overwriteCount">overwrite</button>
  </div>
</template>

<script setup>
import { reactive } from "vue";

let count = reactive({ count: 0 });
let n = count.count;

function increment() {
  count.count++;
  n++;
}

function overwriteCount() {
  count = reactive({ numVar: 0 });
}
</script>
```

---

## `ref()`でのリアクティブなプリミティブ

Vue は`reactive()`の制限に対処するため、`ref()`という関数を用意している。`ref()`を用いることで、プリミティブをリアクティブにできる。

`ref()`は引数を（プリミティブ型も可能！）受け取り、それを`.value`プロパティの値にしてオブジェクトを作る。`reactive()`で作成したオブジェクトのプロパティと同様に、`ref()`の`.value`プロパティもリアクティブになる。

```vue
<template>
  <div>
    <p>refNum: {{ refNum }}</p>
    <p>refObj.count: {{ refObj.count }}</p>
    <button @click="increment">increment</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const refNum = ref(0);
const refObj = ref({ count: 0 });

function increment() {
  refNum.value++;
  refObj.value.count++;
}
</script>
```

また、`ref()`で生成されたオブジェクト（ref()の返り値）を他の関数に渡したり、オブジェクトから分解したりしても、リアクティビティは保持されたままである。

```vue
<template>
  <div>
    <p>refObj.foo: {{ refObj.foo }}</p>
    <p>detachedNum: {{ detachedNum }}</p>
    <button @click="increment">increment</button>
    <button @click="overwriteRefObj">overwriteRefObj</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

let refObj = ref({
  foo: 1,
  bar: 2,
});

let detachedNum = refObj.value.foo;

function increment() {
  refObj.value.foo++;
  refObj.value.bar++;
  detachedNum++;
}

function overwriteRefObj() {
  refObj.value = {
    foo: 100,
    bar: 100,
  };
}
</script>
```

## テンプレートでの Ref の挙動

ref が`<template>`内で呼び出されたとき、その ref が他のオブジェクトでネストされていない場合は自動的にアンラップされる。なので`.value`を使用する必要はない。

```vue
<template>
  <div>
    <p>count: {{ count }}</p>
    <p>nestedCount: {{ nestedCount.count.value }}</p>
    <button @click="increment">increment</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const count = ref(0);
const nestedCount = {
  count: ref(0),
};

function increment() {
  count.value++;
  nestedCount.count.value++;
}
</script>
```

---
