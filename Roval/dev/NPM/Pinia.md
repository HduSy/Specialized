Created Date：2024-01-24 14:44:22  
Last Modified：2024-01-24 14:44:22

# Tags

# Content

## 具有响应式的使用 store

- 第一种方式直接 `return`

```vue
<script setup>
import { useNaubStore } from '../stores/index';

const naubStore = useNaubStore()
</script>

<template>
  <div>{{ message }}</div>
</template>
```

- 第二种方式 `computed API`

```vue
<script setup>
import { useNaubStore } from '../stores/index';
const naubStore = useNaubStore()
const message = computed(() => naubStore.message)
</script>

<template>
  <div>{{ message }}</div>
</template>

```

- 第三种方式 `storeToRefs Pinia API`

```vue
<script>
import { useNaubStore } from '../stores/index';
import { storeToRefs } from 'pinia';

const naubStore = useNaubStore()
const { message } = storeToRefs(naubStore)
</script>

<template>
  <div>{{ message }}</div>
</template>
```

- 第四种方式 `toRefs Vue3 API`

```vue
<script>
import { useNaubStore } from '../stores/index';
import { toRefs } from 'vue';

const naubStore = useNaubStore()
const { message } = toRefs(naubStore)
</script>

<template>
  <div>{{ message }}</div>
</template>
```

- 第五种方式 `toRef Vue3 API`

```vue
<script>
import { useNaubStore } from '../stores/index';
import { toRef } from 'vue';

const naubStore = useNaubStore()
const message = toRef(naubStore, 'message')
</script>

<template>
  <div>{{ message }}</div>
</template>
```

## `getters`

 相当于计算 `computed` 数据

```ts
export const useStore = defineStore('main', {
  state: () => ({
    message: 'Hello World',
  }),
  getters: {
    fullMessage: (state) => `The message is "${state.message}".`,
    // 这个 getter 返回了另外一个 getter 的结果
    emojiMessage(): string {
      return `🎉🎉🎉 ${this.fullMessage}`
    },
  },
})
```

## `actions`

相比 [[Vuex]]

- 不再区分同步 (`mutations`) or 异步 (`actions`) 操作
- 像普通函数调用一样使用，不用 `commit` or `dispatch` 操作

```ts
import { defineStore } from 'pinia';
export const useNaubStore = defineStore('Naub', {
  state: () => ({
    message: 'Hello Pinia'
  }),
  actions: {
    updateMessageSync(newMessage: string): string {
      this.message = newMessage
      return `Sync done`
    },
    async updateMessageASync(newMessage: string): Promise<string> {
      return new Promise((resolve) => {
        setTimeout(() => {
          this.message = newMessage
          resolve('Async done')
        }, 1000)
      })
    }
  },
});

// x.vue
import { useNaubStore } from '../stores/index';
const naubStore = useNaubStore()
naubStore.updateMessageSync('New message by sync.')
naubStore.updateMessageASync('New message by async.')
```

# Reference

[全局状态管理 | Vue3 入门指南与实战案例](https://vue3.chengpeiquan.com/pinia.html)
