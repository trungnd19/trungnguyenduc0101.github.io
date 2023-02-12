---
title: "Tạo clickOutside Directive trong Angular"
date: 2022-02-12
draft: false
ShowToc: true
TocOpen: false
tags: ["angular"]
description: "Tạo directive xử lý trường hợp người dùng click ra ngoài dropdow/ dialog."
weight: 1
---

Khi viết code làm một dropdown menu hay modal/ dialog, trường hợp click ra bên ngoài dropdown hay modal, người dùng thường mong muốn dropdown/ modal đó sẽ được đóng lại. Vậy nếu làm Front-end bằng Angular, chúng ta sẽ giải quyết bài toán đó như thế nào?

Câu trả lời: chúng ta sẽ tạo một custom directive để handle việc đóng dropdown/ modal khi người dùng click bên ngoài dropdown/ modal đó.

## I. clickOutside Directive Code Sample

```
import {
  Directive,
  Input,
  Output,
  EventEmitter,
  ElementRef,
  HostListener,
} from '@angular/core';

@Directive({
  selector: '[clickOutside]',
})
export class ClickOutsideDirective {
  @Output() clickOutside = new EventEmitter<void>();

  constructor(private elementRef: ElementRef) {}

  @HostListener('document:click', ['$event.target'])
  public onClick(target: EventTarget) {
    const clickedInside = this.elementRef.nativeElement.contains(target);
    if (!clickedInside) {
      this.clickOutside.emit();
    }
  }
}
```

## II. Giải thích
### 1. @HostListener
Đầu tiên, chúng ta sẽ sử dụng @HostListener decorator để lắng nghe sự kiện "document:click", là sự kiện sẽ xảy ra khi người dùng click vào bất kì element nào trong DOM.

**@HostListener** decorator có 2 tham số:
-	**eventName?**: Tham số thứ nhất là tên sự kiện cần lắng nghe. Ở đây ta lắng nghe sự kiện **“document:click”**.
-	**args?**: Tham số thứ hai là một array chứa các arguments mà ta sẽ truyền cho hàm xử lý sự kiện được gọi tới mỗi khi sự kiện cần lắng nghe (đã nêu trong tham số thứ nhất) xảy ra. Ở đây, ta truyền **“$event.target”** cho hàm onClick(target: EventTarget). Parameter “target” của hàm onClick(target: EventTarget) ở đây chính là **“$event.target”** được decorator truyền sang.

```
  @HostListener('document:click', ['$event.target'])
  public onClick(target: EventTarget) {
    // ...
  }
```
### 2. elementRef
Dùng elementRef để trỏ tới element được gán directive 
```
constructor(private elementRef: ElementRef) {}
```

### 3. Node.contains() method
Tiếp theo, ta sẽ dùng Node.contains() method, dùng để check xem một node có là child node của một node nào đó trong DOM Tree hay không. Method này trả về true/ false.

- Nếu người dùng click bên trong element được gắn directive, Node.contains() method sẽ trả về true.

- Ngược lại, nếu người dùng click bên ngoài element được gắn directive, Node.contains() method sẽ trả về false, khi đó ta sẽ emit @Output() clickOutside.

```
const clickedInside = this.elementRef.nativeElement.contains(target);
if (!clickedInside) {
    this.clickOutside.emit();
}
```

### 4. Sử dụng directive vừa tạo
- Import vào module có component cần sử dụng directive:
```
import { ClickOutsideDirective } from './click-outside.directive';

@NgModule({
  imports:      [ BrowserModule, FormsModule ],
  declarations: [ AppComponent,  ClickOutsideDirective ],
  bootstrap:    [ AppComponent ]
})
```
- Trong template của component, có thể import như sau:
Cách 1: Nêu rõ tên directive và event binding hàm xử lý khi người dùng click outside dropdown/ dialog
```
<div clickOutside (clickOutside)="someHandler()"></div>
```

Cách 2: ở đây, do tên directive và event binding trùng nhau, cộng với việc directive chỉ có duy nhất một event binding => không cần nêu tên directive
```
<div (clickOutside)="someHandler()"></div>
```

### 5. Stackblitz link:
Các bạn có thể tham khảo demo trên stackblitz:
https://stackblitz.com/edit/angular-j7jqq2

## Reference
1.  https://www.willtaylor.blog/click-outside-directive/ (link bài gốc)
2.	https://angular.io/api/core/HostListener
3.	https://developer.mozilla.org/en-US/docs/Web/API/Node/contains
 