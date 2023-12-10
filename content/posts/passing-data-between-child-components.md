---
title: "Làm thế nào để truyền data giữa các components con trong Vue?"
date: 2023-12-10
draft: false
ShowToc: true
TocOpen: true
tags: ["vue"]
description: "Có cách nào để các components con (thuộc cùng một component cha) giao tiếp với nhau trong Vue?"
weight: 1
---

## I. Trong Vue, có 3 loại giao tiếp giữa các components mà ta cần chú ý:

1. Component Cha giao tiếp với Component Con (Parent =\> Child)

2. Component Con giao tiếp với Component Cha (Child =\> Parent)

3. Component Con giao tiếp với Component Con (Child =\> Child)

Trong 3 loại, việc giao tiếp giữa các component con (của cùng 1 component cha) có lẽ là thứ gây khó dễ nhiều nhất cho các lập trình viên dùng Vue.

Vậy có một phương pháp chuẩn nào để truyền data giữa các component con không?

Câu trả lời ngắn gọn là: chúng ta **KHÔNG** truyền data từ một component con này sang một component con khác.

## II. Nguyên tắc của việc truyền data giữa các Vue Components

1. **Nguyên tắc 1**: Data được truyền (từ component cha) xuống (component con), không truyền (từ component con) lên (component cha)
   **("Data flows down, never up.")**

- Để nhận data từ component cha, chúng ta khai báo và sử dụng "props" trong component con.

- Chính vì có nguyên tắc này mà chúng ta sẽ tránh tuyệt đối việc tác động thay đổi props được truyền xuống từ component cha. Khi component con tác động thay đổi props, data từ component con được đẩy ngược lên component cha

=\> Chúng ta đã vi phạm nguyên tắc 1.

2. **Nguyên tắc 2**: Event được emit (từ component con) lên (component cha) **(Events are emitted up.)**

- Đây là nguyên tắc giao tiếp giữa component con =\> component cha

Nguyên tắc 1 và Nguyên tắc 2 là "kim chỉ nam", là giải pháp chúng ta sẽ nghĩ tới ngay khi cần giao tiếp Component Cha =\> Component Con (nguyên tắc 1) hoặc giao tiếp Component Con =\> Component Cha (nguyên tắc 2)

Vậy có nguyên tắc thứ 3 nào chúng ta sử dụng khi cần giao tiếp Component Con =\> Component Con không?

**Câu trả lời là: KHÔNG.**

## III. Cách truyền data giữa các component con

Ví dụ chúng ta có component A là component cha của 2 component con là component B và component C:

**A (cha) - B và C (con)**

- Để giao tiếp component B và component C, trước tiên, ta giao tiếp component B với Component A (component cha):

**B (con) ---(emit)---\> A (cha)**

- Sau đó component A (cha) sẽ giao tiếp với component C:

**B (con) ---(emit)---\> A (cha)**

**A (cha) ---(props)--\> C (con)**

## IV. Ví dụ

Trong đoạn code sau:

- Toggle Component: nếu "open" props là true =\> Hiện nội dung (và ngược lại)

- Button component: Button component cần giao tiếp với Toggle Component. Khi click =\> ẩn/ hiện nội dung của Toggle Component.

Toggle và Button là 2 component con cần giao tiếp với nhau.

```
// Parent
<template>
  <div>
    <Toggle :open="true">
      Show or hide this content!
    </Toggle>

    <Button @click="">
      Toggle the content
    </Button>
  </div>
</template>
```

Để giao tiếp giữa Toggle - Button, chúng ta cần:

- Đẩy data từ Button lên component cha, sử dụng emit (nguyên tắc 1)

- Truyền data từ component cha xuống Toggle, sử dụng prop (nguyên tắc 2)

```
// Parent
<template>
  <div>
    <Toggle :open="isOpen">
      Show or hide this content!
    </Toggle>

    <Button @click="isOpen = !isOpen">
      Toggle the content
    </Button>
  </div>
</template>

```

Bằng cách thêm biến isOpen ở component cha, hai component con là Toggle - Button đã có thể giao tiếp với nhau :D

Khi click Button component, component này sẽ emit 1 event để update giá trị mới nhất cho biến isOpen. Sau đó biến isOpen sẽ được truyền xuống là prop trong Toggle component.

---

_Dịch từ bài viết của Michael Thiessen (https://michaelnthiessen.com/passing-data-between-child-components)_
