---
title: Vue3笔记 --- 响应性基础API
date: 2021-06-03 10:09:55
tags:
- Vue3
categories:
- 前端

---
记录整理Vue3的响应性API...
<!--more-->
#### 响应性基础 API

Vue3 提供了两种方式构建响应式数据：ref 和 reactive
ref 用于构建简单值的响应式数据，基于 Object.defineProperty 监听 value 值
reactive 用于构建复杂的响应式数据，基于 Proxy 对数据进行深度监听


##### ref

接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象具有指向内部值的单个 property `.value`。

有时我们可能需要为 ref 的内部值指定复杂类型。我们可以通过在调用 `ref` 来覆盖默认推断时传递一个泛型参数来简洁地做到这一点：

```ts
const foo = ref<string | number>('foo') // foo's type: Ref<string | number>

foo.value = 123 // ok!
```

如果泛型的类型未知，建议将 `ref` 转换为 `Ref<T>`：

```js
function useState<State extends string>(initial: State) {
  const state = ref(initial) as Ref<State> // state.value -> State extends string
  return state
}
```



##### toRef

可以用来为源响应式对象上的 property 性创建一个 [`ref`](https://vue3js.cn/docs/zh/api/refs-api.html#ref)。然后可以将 ref 传递出去，从而保持对其源 property 的响应式连接。接收两个参数：源响应式对象和属性名，返回一个ref数据。例如使用父组件传递的props数据时，要引用props的某个属性且要保持响应式连接时就很有用。`详细示例在文章结尾的代码块`

- 获取数据值的时候需要加.value
- `toRef后的ref数据不是原始数据的拷贝，而是引用，改变结果数据的值也会同时改变原始数据`

```js
import { defineComponent, toRef } from 'vue'

export default defineComponent({
  props: [title],
  
  setup (props) {
    // 创建变量myTitle
    const myTitle = toRef(props, 'title')
    console.log(myTitle.value)
  }
})
```



##### toRefs

将响应式对象转换为普通对象，其中结果对象的每个 property 都是指向原始对象相应 property 的[`ref`](https://vue3js.cn/docs/zh/api/refs-api.html#ref)。

```vuejs
<template>
  <div>
    <h1>解构响应式对象数据</h1>
    <p>Username: {{username}}</p>
    <p>Age: {{age}}</p>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";

export default {
  name: "DestructReactiveObject",
  setup() {
    const user = reactive({
      username: "haihong",
      age: 10000,
    });

    return { ...toRefs(user) };
  },
};
</script>
```

当想要从一个组合逻辑函数中返回响应式对象时，用 toRefs 是很有效的，该 API 让消费组件可以 解构 / 扩展（使用 ... 操作符）返回的对象，并不会丢失响应性：

```js
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2,
  })

  // 对 state 的逻辑操作
  // ....

  // 返回时将属性都转为 ref
  return toRefs(state)
}

export default {
  setup() {
    // 可以解构，不会丢失响应性
    const { foo, bar } = useFeatureX()

    return {
      foo,
      bar,
    }
  },
}
```



##### readonly -- 深层”的只读**代理

传入一个对象（响应式或普通）或 ref，返回一个原始对象的只读代理。一个只读的代理是“深层的”，对象内部任何嵌套的属性也都是只读的。

```vuejs
<template>
  <div>
    <h1>readonly - “深层”的只读代理</h1>
    <p>original.count: {{original.count}}</p>
    <p>copy.count: {{copy.count}}</p>
  </div>
</template>

<script>
import { reactive, readonly } from "vue";

export default {
  name: "Readonly",
  setup() {
    const original = reactive({ count: 0 });
    const copy = readonly(original);

    setInterval(() => {
      original.count++;
      copy.count++; // 报警告，Set operation on key "count" failed: target is readonly. Proxy {count: 1}
    }, 1000);


    return { original, copy };
  },
};
</script>
```



##### reactive

返回对象的响应式副本,完整示例如下

```vuejs
<template>
  <div class="about">
    <button @click.stop="obj.increase">increase</button>

    <button @click.stop="reverse">reverse</button>

    <h1>{{ obj.count }}</h1>

    <h1>{{ obj.double }}</h1>

    <h1>{{ obj.title }}</h1>

    --------------------

    <h1>{{ count }}</h1>

    <h1>{{ double }}</h1>

    <h1>{{ title }}</h1>

    <componentA :count="obj.count"/>
  </div>
</template>


<script lang="ts">

  import {reactive, ref, computed, toRefs } from "vue";

  import componentA from "./components/componentA.vue";

  interface ObjProps {
    title: string;
    count: number;
    double: number;
    reverse: () => void;
    increase: () => void;
  }

  export default {
    components: {componentA},
    setup(props: any) {
      // ref返回普通数据类型的响应式副本
      const count = ref(0);
      // reactive返回对象的响应式副本
      const obj: ObjProps = reactive({
        title: "hello world",
        count: 0,
        double: computed(() => obj.count * 2),
        reverse: () => {
          obj.title = obj.title.split("").reverse().join("");
        },
        increase: () => {
          obj.count++;
        },
      });
	  const count = toRef(obj, 'count')
      const objToRefs = toRefs(obj)
      return {
        obj,
        title: objToRefs.title,
        count: objToRefs.count,
        double: objToRefs.double,
        reverse: objToRefs.reverse,
        increase: objToRefs.increase
      };
    },
  };
</script>
```



##### watchEffect

立即执行传入的一个函数，并响应式追踪其依赖，并在其依赖变更时重新运行该函数。

```vuejs
<template>
  <div>
    <h1>watchEffect - 侦听器</h1>
    <p>{{data.count}}</p>
    <button @click="stop">手动关闭侦听器</button>
  </div>
</template>

<script>
import { reactive, watchEffect } from "vue";
export default {
  name: "WatchEffect",
  setup() {
    const data = reactive({ count: 1 });
    const stop = watchEffect(() => console.log(`侦听器：${data.count}`));
    setInterval(() => {
      data.count++;
    }, 1000);
    return { data, stop };
  },
};
</script>
```

![image-20210603154819179](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210603154819179.png)



##### watch

对比`watchEffect`，`watch`允许我们：

- 懒执行副作用，也就是说仅在侦听的源变更时才执行回调；
- `更明确哪些状态的改变会触发侦听器重新运行副作用；`
- 访问侦听状态变化前后的值。

```vuejs
<template>
  <div>
    <h1>watch - 侦听器</h1>
    <p>count1: {{data.count1}}</p>
    <p>count2: {{data.count2}}</p>
    <button @click="stopAll">Stop All</button>
  </div>
</template>

<script>
import { reactive, watch } from "vue";
export default {
  name: "Watch",
  setup() {
    const data = reactive({ count1: 0, count2: 0 });
    // 侦听单个数据源
    const stop1 = watch(data, () =>
      console.log("watch1", data.count1, data.count2)
    );
    // 侦听多个数据源
    const stop2 = watch([data], () => {
      console.log("watch2", data.count1, data.count2);
    });
    setInterval(() => {
      data.count1++;
    }, 1000);
    return {
      data,
      stopAll: () => {
        stop1();
        stop2();
      },
    };
  },
};
</script>
```



##### computed

返回一个默认不可手动修改的 ref 对象。

- ###### 只传 getter

```vuejs
<template>
  <div>
    <h1>computed - 计算属性</h1>
    <p>{{username}}</p>
  </div>
</template>

<script>
import { reactive, computed } from "vue";
export default {
  name: "Computed",
  setup() {
    const user = reactive({ firstname: "chen", lastname: "haihong" });
    const username = computed(() => user.firstname + " " + user.lastname);
    username.value = "hello world"; // 报警告，computed value is readonly
    return { username };
  },
};
</script>
```

![image-20210603141847906](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210603141847906.png)

如果是这种使用方式，则会报错 ❌，而不是警告 ⚠️，

```javascript
 setup() {
    const user = reactive({ firstname: "chen", lastname: "haihong" });
    const username = computed({
      get: () => user.firstname + " " + user.lastname,
    });
    username.value = "hello world"; // ❌ computed value is readonly
    return { username };
  },
```



![image-20210603142037622](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210603142037622.png)



- ###### 同时传 getter、setter

  创建一个可手动修改的计算状态。

```vuejs
<template>
  <div>
    <h1>computed - 计算属性</h1>
    <p>firstname: <input v-model="user.firstname" /></p>
    <p>lastname: <input v-model="user.lastname" /></p>
    <p>username: <input v-model="username" /></p>
  </div>
</template>

<script>
import { reactive, computed } from "vue";
export default {
  name: "Computed2",
  setup() {
    const user = reactive({ firstname: "Chen", lastname: "Haihong" });
    const username = computed({
      get: () => user.firstname + " " + user.lastname,
      set: (value) => {
        const [firstname, lastname] = value.trim().split(" ");
        user.firstname = firstname;
        user.lastname = lastname;
      },
    });
    return { user, username };
  },
};
</script>
```

![image-20210603142503777](https://cdn.jsdelivr.net/gh/houpai/hp-cdn@latest/picGo/image-20210603142503777.png)
