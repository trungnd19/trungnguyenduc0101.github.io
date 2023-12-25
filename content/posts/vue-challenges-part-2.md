---
title: "vue-challenges phần 2: Dynamic CSS, v-once, shallowRef và Key Modifiers"
date: 2023-12-25
draft: false
ShowToc: false
TocOpen: false
tags: ["vue"]
description: "Thử thách bản thân với hơn 4 câu hỏi vue-challenges"
summary: "Thử thách bản thân với hơn 4 câu hỏi vue-challenges"
weight: 2
---

## 1. Dynamic css values

- URL: https://github.com/webfansplz/vuejs-challenges/blob/main/questions/14-dynamic-css-values/README.md
- Đây là challenge thứ hai về các tính năng liên quan đến CSS của Vue (challenge đầu tiên về CSS là Global CSS Selector).
  Điểm nổi bật của Vue là tính chất Reactivity, khi giá trị của một ref thay đổi thì template/ computed... có sử dụng ref đó cũng sẽ được update. **Nhưng liệu có cách nào để giá trị CSS cũng dynamic theo giá trị của ref không?**

- Solution:

```
<script setup>
import { ref } from "vue"
const theme = ref("red")
const colors = ["blue", "yellow", "red", "green"]
setInterval(() => {
  theme.value = colors[Math.floor(Math.random() * 4)]
}, 1000)
</script>

<template>
  <p>hello</p>
</template>

<style scoped>
/* Modify the code to bind the dynamic color */
p {
  color: v-bind(theme)
}
</style>
```

## 2. Optimize performance directive

- URL: https://github.com/webfansplz/vuejs-challenges/blob/main/questions/12-optimize-perf-directive/README.md
- Vue cung cấp một built-in directive (directive có sẵn) cho phép chúng ta render element/ component chỉ một lần duy nhất, sau đó dù ref gắn liền với element/ component thay đổi thì element/ component đó vẫn không được render lại.
  Bạn có biết đó là built-in directive nào không?
- Solution:

```
<script setup>
import { ref } from "vue"

const count = ref(0)

setInterval(() => {
  count.value++
}, 1000)
</script>

<template>
  <span v-once>Make it never change: {{ count }}</span>
</template>
```

## 3. shallow ref

- URL: https://vuejs-challenges.netlify.app/questions/6-shallow-ref/README.html
- Để cải thiện hiệu năng khi dùng Vue, trong một vài trường hợp, ta có thể xét đến việc dùng shallowRef thay vì ref. Khác với ref, những thay đổi liên quan đến shallowRef (như trên UI hoặc trong watcher) sẽ **chỉ được trigger khi giá trị của shallowRef.value được gán giá trị mới**. Nói cách khác, khi dùng shallowRef, chỉ .value access là reactive.
- VD trong Vue Doc:

```
const state = shallowRef({ count: 1 })
// does NOT trigger change - Không dẫn tới thay đổi
state.value.count = 2
// does trigger change - Dẫn tới thay đổi
state.value = { count: 2 }
```

- VD khác, khi dùng shallowRef với array:

```
const state = shallowRef([1])

// Nếu dùng hàm updateArr thì watcher không được trigger. Chỉ trigger nếu dùng hàm updateArrThatWorks
watch(state, () => {
  console.log("State.count Updated")
}, { deep: true })

function updateArr() {
  state.value.push(Math.floor(Math.random() * 10))
  console.log(state.value) // state có giá trị mới nhất, nhưng UI và watcher không được trigger
}

function updateArrThatWorks() {
 state.value = [...state.value, Math.floor(Math.random() * 10)]
}

</script>

<template>
  <div>
    <p v-for="item in state" :key="item">
      {{ item }}
    </p>
    <button @click="updateArr">Add item</button>
  </div>
</template>
```

- Quay trở lại với solution:

```
<script setup lang="ts">
import { shallowRef, watch } from "vue"

const state = shallowRef({ count: 1 })

// Does NOT trigger
watch(state, () => {
  console.log("State.count Updated")
}, { deep: true })

/**
 * Modify the code so that we can make the watch callback trigger.
*/
state.value = { count: 2}

</script>

<template>
  <div>
    <p>
      {{ state.count }}
    </p>
  </div>
</template>
```

## 4. Key Modifiers

- URL: https://vuejs-challenges.netlify.app/questions/232-key-modifiers/README.html
- Ví dụ ta có bài toán: một button, hàm xử lý sự kiện của của button chỉ được gọi khi ta ấn CTRL và click chuột. Nếu không ấn CTRL + click thì hàm xử lý sự kiện không được gọi. Nếu dùng Vue, ta sẽ xử lý như thế nào? Câu trả lời ở đây là dùng **.exact key modifier**.
  **VD: <button @click.ctrl.exact="onCtrlClick">A</button>**
- Solution:

```
<script setup lang="ts">
const click1 = () => {
  console.log('click1')
}
const click2 = () => {
  console.log('click2')
}
</script>

<template>
  <div @click="click1()">
   <div @click.stop="click2()">
     click me
   </div>
  </div>
</template>
```

## Summary

Để giải quyết 4 challenges, chúng ta đã dùng các feature của Vue là V-bind in CSS, v-once, shallowRef và Exact modifier. Mình sẽ để link Vue Documentation về những feature này dưới đây để chúng ta cùng đọc lại nhé.

- https://vuejs.org/api/sfc-css-features.html#v-bind-in-css
- https://vuejs.org/api/built-in-directives.html#v-once
- https://vuejs.org/api/reactivity-advanced.html#shallowref
- https://vuejs.org/guide/best-practices/performance#reduce-reactivity-overhead-for-large-immutable-structures
- https://vuejs.org/guide/essentials/event-handling#exact-modifier
