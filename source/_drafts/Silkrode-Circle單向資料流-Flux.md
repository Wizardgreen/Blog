---
title: Vuex原始碼系列 - 單向資料流 Flux
tags:
  - flux
  - vuex
  - redux
categories: Study-Circle, Silkrode-Web, Vuex原始碼系列
---

Flux 是一個由 Facebook 提出的設計概念，相比傳統的雙向資料流，Flux 的用意是透過定義清楚的職責來簡化資料流向，進而達成容易維護的結構。

# 結構與資料流

{% asset_img idea.png %}

資料流向在 Flux 的應用中必須遵照單一方向，並透過四個主要的角色來運作 Dispatcher、Store、View、Action。

|名稱          |作用                                      |
|---          |---                                      |
|View         |即為畫面元素，例如 Vue＆Recact 的元件         |
|Store        |商業邏輯、儲存資料，並開放 API 使 View 讀取    |
|Action       |定義了所有相關的行為，透過 View 打給 Dispatcher|
|Dispatcher   |作為 API 將 Action 傳入 Store              |

# Redux 中的結構

*Action & Dispatcher*: 能夠被拆成三個部分，Creator、Type、Payload(不一定有)，Creator 是一 function，回傳結果會是一個帶有 Type 與 Payload 的單純物件，Type 是一個純粹的字串，用來給開發人員與 Dispatcher 識別行為，而 payload 就行為可能會需要用到的資料，例如改變值、會員 id 之類的。

```js
// action.js
const createNewMember = () => {  // <--- Creator
  return {
    type: 'CREATE_NEW_MEMBER',
    payload: {
      // ...會員資料
    }
  }
}
```

dispatcher API 在 React Redux 中有兩種方式(不包括 hook )可以用，一種是直接透過 store.dispatch, 另一種則是透過 HOC 把 dispatch 傳給元件
```js
import action from './action';
const createNewMember = () => {
  dispatch(action.createNewMember());
}
```
```jsx
<button onClick={createNewMember}>送出會員資料</button>
```

*Store*: 在 redux 中會被分離出一個部分稱為 Reducer ，用來管理不同的 Action Type 要做什麼樣的邏輯處理
```js
// 第一參數接受 store.state, 第二參數則是 action creator 回傳出的物件
// 回傳修改後的 state
export default function MemberReducer (state, action) {
  switch (action.type) {
    case 'CREATE_NEW_MEMBER':
      return {
        ...state,
        members: [
          ...state.members
          action.payload
        ]
      }
    case 'DELETE_MEMBER':
      // do something with state and return it
      return;
    default:
      return state; // 若 type 不合，回傳原本的 state
  }
}
```

最後再把 Reducer 包裝起來植入到 React 的 Root Component

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import MemberReducer from './reducer/reducer';
import App from './app';

const store = createStore(MemberReducer);
// 依據我們撰寫的 reducer 建立出 store

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

# Vuex 中的結構

```js
// 在 Vuex 的結構相較簡單許多
const store = new Vuex.Store({
  state: () => ({
    count: 0,
  }),
  getters: { // computed 版本的 state
    getCountPlusFive(state) {
      return state.count + 5;
    }
  }, 
  mutations: {
    // key 即為 action type
    // 在 view 中透過 this.$store.commit('increment') 發送 dispatch
    increment (state, payload) {
      state.count += payload
    }
  },
})
```

# 缺陷
Flux 