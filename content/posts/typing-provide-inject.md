---
title: "Sử dụng Typescript với provide/ inject trong Vue"
date: 2023-12-11
draft: false
ShowToc: false
TocOpen: false
tags: ["vue"]
description: "Cách typing provide/ inject trong Vue 3"
summary: "Cách typing provide/ inject trong Vue 3"
weight: 2
---

Trong bài blog ngắn của mình, **Anthony Fu** - core member của Vue, tác giả thư viện VueUse đã viết:

_"Trước khi xem bài nói của Thorsten Lunborg tại sự kiện Vue Amsterdam, tôi đã không biết rằng ta có thể typing provide/ inject một cách dễ dàng như thế."_

Vậy cách typing provide/ inject của Lunborg (một core team member của Vue) như thế nào mà khiến Anthony Fu - tác giả của thư viện VueUse tuyệt vời ấn tượng đến thế?

Với Vue, ta có thể dùng dạng string hoặc Symbol để dùng làm key khi provide/ inject một value. Ý tưởng cơ bản ở đây là Vue cung cấp cho ta **InjectionKey** - một type utility giúp ta có thể typing cho giá trị cần provide ứng với symbol key đó. Khi sử dụng provide/ inject với key dạng Symbol, Vue sẽ tự động infer type của value được provide/ inject cho chúng ta.

```
// context.ts
import type { InjectionKey } from 'vue'

export interface UserInfo {
  id: number
  name: string
}

export const InjectKeyUser: InjectionKey<UserInfo> = Symbol()
```

```
// parent.vue
import { provide } from 'vue'
import { InjectKeyUser } from './context'

export default {
  setup() {
    provide(InjectKeyUser, {
      id: '117', // type error: should be number
      name: 'Anthony',
    })
  },
}
```

```
// child.vue
import { inject } from 'vue'
import { InjectKeyUser } from './context'

export default {
  setup() {
    const user = inject(InjectKeyUser) // UserInfo | undefined

    if (user)
      console.log(user.name) // Anthony
  },
}
```

---

_Dịch từ bài viết của Anthony Fu (https://antfu.me/posts/typed-provide-and-inject-in-vue)_
