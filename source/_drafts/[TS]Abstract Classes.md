---
title: \[TypeScript\] Abstract Classes
tags: TypeScript
categories: Note
# date:
---

1. 無法直接拿來創造物件.
2. 只能被當作父類(parent class)使用.
3. 能包含實作好的方法(methods).
4. 能指向不實際存在的方法，但需要先提供方法名稱與函式簽名(function signature).
5. 能夠確保繼承的子類必須實作出睇四點中不存在的方法.

1) 跟`interface`一樣能用來約數多個不同的 Class，但沒辦法用於物件.
2) 與`interface`不同，耦合度非常高(需搭配繼承關鍵字不然毫無用處、父類可指向子類方法，反之亦然).

總結：在 TypeScript 中我們還是 Interfaces 為主，方便、低耦合、容易重複使用，除非某種使用場景需要關係如此緊密的物件，才會去考慮 Abstract Classes.
