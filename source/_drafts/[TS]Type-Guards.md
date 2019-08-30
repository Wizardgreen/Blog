---
title: \[TypeScript\] Type Guards
tags: TypeScript
categories: Note
# date:
---

在 TypeScript 中有兩種 Type Guards 可使用

```ts
const ary: number[] = [1, 5, 10];
ary instanceof Array
// 用在 object, array 與 interface 這類”集合的型別“

typeof ary === ???
// 作用於 string, number, boolean.. 等等屬於“值的型別”
```
