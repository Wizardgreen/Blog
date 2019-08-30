---
title: \[Golang\]Interface
tags: Golang
categories: Note
# date:
---

Golang 學習筆記 - Interface

<!-- more -->

先來一段 Code.

```go
type dog struct{}
type cat struct{}

func main() {
  alien := dog{}
  gentle := cat{}
  callYourDog(alien)
  callYourCat(gentle)
}

func callYourDog(d dog) {
  d.answer()
}
func callYourCat(c cat) {
  c.answer()
}

func (dog) answer() string {
  return "Woof! woof!"
}

func (cat) answer() string {
  return "Meow~"
}
```

先不管這段程式碼合不合理，如要達成讓寵物回應就得針對每個寵物的型別去寫一個對應的函式，而且現在才兩個動物，如果你還養了馬、老鷹、牛、甚至狐狸的話....

```go
func callYourDog(d dog) {
  d.answer()
}
func callYourCat(c cat) {
  c.answer()
}
func callYourDog(h horse) {
  d.answer()
}
func callYourCat(e eagle) {
  c.answer()
}
func callYourDog(f fox) {
  d.answer()
}
func callYourCat(b bull) {
  c.answer()
}
// ....以下各種對應的 receiver 就略過
```

(What dose the fox say? BTW)
一但傳入的型別不同，我們就得把邏輯幾乎相同的程式碼寫上好幾遍！

但透過 Interface 我們就能把上面的 Code 簡化這樣：

```go
type pet interface {
  answer() string
}
type dog struct{}
type cat struct{}

func main() {
  alien := dog{}
  gentle := cat{}
  // 宣告各種動物變數...

  callYourPet(alien)
  callYourPet(gentle)
  // callYourPet(各種動物)....
}

func callYourPet(p pet) {
  p.answer()
}
// ....以下各種對應的 receiver 就略過
```

這就是為什麼我們需要`Interface`，它可以幫助我們有更好品質且能重複使用的 Code，

1. Interface 就像是一個合約，申明了定義好的函式名/傳入參數/回傳參數，如果有個型別實作了定義好的函式，則被視為該 Interface
2. Interface 與一般具體(concrete)型別不同，不會帶有任何值，我們只能定義這個型別.
3. Interface 是隱式(implicit)的，這代表我們不需寫額外的 code 來宣告某個型別屬於某個 Interface，只要條件符合，就會自動歸類到該 Interface.
4. Interface 只在乎型別，不會驗證函式內部的計算過程.
