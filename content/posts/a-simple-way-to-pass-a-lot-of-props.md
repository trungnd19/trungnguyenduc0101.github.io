---
title: "Quá nhiều Props phải truyền xuống? Làm thế nào để code gọn?"
date: 2023-12-21
draft: false
ShowToc: false
TocOpen: false
tags: ["vue"]
description: "Cách để code gọn gàng hơn khi phải truyền rất nhiều props cho component con"
summary: "Cách để code gọn gàng hơn khi phải truyền rất nhiều props cho component con"
weight: 2
---

## Quá nhiều props phải truyền xuống?

Khi làm việc với Vue, chắc hẳn bạn đã gặp trường hợp phải sử dụng 1 component **nhận vào rất nhiều props.**

```
<template>
  <User
    :name="user.name"
    :profile="user.profile"
    :twitter="user.twitter"
    :location="user.location"
    :framework="user.framework === 'Vue' ? 'The best' : undefined"
  />
</template>
```

Thay vì truyền xuống từng props riêng lẻ, để code gọn gàng hơn, ta có thể **nhóm tất cả props cần truyền xuống cho component con vào một object** rồi kết hợp với **v-bind syntax**:

```
<template>
  <User v-bind="user"/>
</template>
```

```
export default {
  setup() {
    return {
      user: {
        name: 'Anakin',
        profile: 'ani-profile.jpg',
        twitter: '@TatooineJedi',
        location: 'Undisclosed',
        framework: 'Vue',
      },
    };
  },
};
```

## Quá nhiều event handlers phải "hứng/ bắt"?
Tương tự, trong trường hợp cần sử dụng nhiều event handlers với component con, để code gọn gàng hơn, ta có thể dùng **object chứa các method** kết hợp với **v-on syntax**:

```
<template>
  <User v-on="userEventHandlers"/>
</template>
```

```
export default {
  setup() {
    return {
      userEventHandlers: {
        updateName(newName) {
          // ...
        },
        deleteUser() {
          // ...
        },
        addFriend(friend) {
          // ...
        }
      },
    };
  },
};
```

_Dịch từ bài viết của Michael Thiessen (https://michaelnthiessen.com/simpler-way-to-pass-lots-of-props)_