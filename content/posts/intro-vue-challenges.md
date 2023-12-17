---
title: "Kiểm tra kiến thức Vue của bản thân với vue-challenges!"
date: 2023-12-15
draft: false
ShowToc: false
TocOpen: false
tags: ["vue"]
description: "Thử thách bản thân với hơn 30 bài tập code Vue"
summary: "Thử thách bản thân với hơn 30 bài tập code Vue"
weight: 2
---

## Về vue-challenges
Để nắm vững một front end framework, bên cạnh việc đọc Documentation để nắm vững lý thuyết thì việc thực hành code, sử dụng chính những tính năng của framework đó để giải quyết bài toán thực tế là một điều vô cùng quan trọng.

Trong quá trình học Vue, mình có tình cờ bắt gặp một github repo rất thú vị là [vue-challenges]( https://github.com/webfansplz/vuejs-challenges). Repo này là tổng hợp hơn 30 bài tập code yêu cầu chúng ta phải áp dụng những hiểu biết của mình về Vue để giải quyết vấn đề.

Tác giả chính của repo là [webfansplz]( https://github.com/webfansplz), một core team member của vue và vueuse. (đọc profile của tác giả là biết đây sẽ là một repo chất lượng rồi phải không? :D)

Để bắt đầu thử sức mình với vue-challenges, chúng ta chỉ cần:
1. Chọn câu hỏi muốn làm.
2. Click "Take the challenge" để bắt đầu code trực tiếp trên Vue SFC Playground. Tuy nhiên, nếu muốn execute file unit test được viết sẵn để đảm bảo code đã thỏa mãn tất cả testcases, chúng ta sẽ phải download project về và code trên máy tính của mình.
3. Nếu chưa nghĩ ra đáp án, hoặc muốn tham khảo cách người khác làm, click "Check out Solutions".
Trong post này, mình sẽ thử làm 4 câu hỏi là "Hello World", "Global CSS", "prevent event propagation" và "Dependency Injection".

## Hello World
- URL: https://github.com/webfansplz/vuejs-challenges/tree/main/questions/1-hello-word
- Thử thách đầu tiên nên rất đơn giản :D Để hiển thị giá trị một ref ra template, ta chỉ cần dùng syntax text interpolation
- Solution:
```
<script setup>
import { ref } from "vue"
const msg = ref("Hello World")
</script>

<template>
  <div>
    <h1>{{msg}}</h1>
  </div>
</template>
```

## Global CSS
- URL: https://github.com/webfansplz/vuejs-challenges/blob/main/questions/27-global-css/README.md
- Làm thế nào để set global CSS và scoped CSS trong cùng 1 component? Với câu hỏi này, ta có 2 cách giải quyết như bên dưới.
- Solution 1: Mixing Local and Global Styles (dùng 1 cặp thẻ <style> cho global và 1 cặp thẻ <style> cho scoped css)

```
<template>
  <p>Hello Vue.js</p>
</template>

<style scoped>

p {
  font-size:20px;
  color:red;
  text-align: center;
  line-height: 50px;
}
</style>

<style>
/* Make it work */
body {
  width: 100vw;
  height: 100vh;
  background-color: burlywood;
}
</style>
```

- Solution 2: Dùng global selector
```
<template>
  <p>Hello Vue.js</p>
</template>

<style scoped>

p {
  font-size:20px;
  color:red;
  text-align: center;
  line-height: 50px;
}

/* Make it work */
:global(body) {
  width: 100vw;
  height: 100vh;
  background-color: burlywood;
}

</style>
```

## 3. Dependency Injection
- URL: https://github.com/webfansplz/vuejs-challenges/blob/main/questions/9-dependency-injection/README.md
- Làm thế nào để truyền một giá trị từ component cha xuống component con của component con của component con của component con? :D Cách nhanh nhất đó là dùng provide/ inject, một feature của Vue để tránh tình trạng Prop Drilling (truyền props qua một loạt component để tới component con cần dùng)
Khi cần inject value nào => phải dùng đúng key đã được provide. (Về provide/ inject, mình cũng từng dịch [một bài](https://trungnd19.github.io/posts/typing-provide-inject/) của Anthony Fu về cách dùng Typescript với provide/ inject.)
- Solution:
```
<script setup lang="ts">
import { inject } from "vue"
// Add a piece of code to make the `count` value get injected into the child component.
const count = inject("count", 0)
</script>

<template>
  {{ count }}
</template>
```

## Prevent event propagation
- URL: https://github.com/webfansplz/vuejs-challenges/blob/main/questions/243-prevent-event-propagation/README.md
- Bạn có biết khái niệm **Event Modifiers** trong Vue? Đây là feature của Vue giúp chúng ta có thể tránh được việc phải tự mình gọi hàm stopProgation hay preventDefault trong các hàm xử lý sự kiện của mình.
Cách dùng event modifiers cũng rất đơn giản, ta chỉ cần dùng syntax tên sự kiện + . + event modifiers. **VD: @click.prevent/ @click.stop**
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
Để giải quyết 4 challenges, chúng ta đã dùng các feature của Vue là Global css selector, provide/ inject và event modifiers.
Mình sẽ để link Vue Documentation về những feature này dưới đây để chúng ta cùng đọc lại nhé.
- https://vuejs.org/api/sfc-css-features.html#global-selectors
- https://vuejs.org/guide/components/provide-inject
- https://vuejs.org/guide/essentials/event-handling#event-modifiers
