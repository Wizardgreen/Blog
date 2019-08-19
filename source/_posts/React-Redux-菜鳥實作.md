---
title: React Redux 菜鳥實作
tags:
  - React
  - Redux
date: 2018-05-04 22:59:24
---

> 還搞不懂 Redux 要怎麼使用?

<!-- more -->

{% asset_img rr.png %}

- 文章時間：2018-04-26
- 適合對象：React 新手，剛熟悉用 React 製作一個元件、頁面
- 花費時間：一小時...半，Maybe？

## <center>React Redux 菜鳥實作</center> #

## <center>緣起</center> #
因為工作上的需求，我一頭栽入了 React 的生態圈，網路上中文資源很多，真的很友善，但不是資訊過時，就是 code 裡面寫了一些我自己看不懂的語句，搞得我花很多時間去讀一些不太相關的資料。說到底還是因為我自己是個真菜鳥，所以寫這篇記錄，除了強化自己對 React + Redux 的印象，也是希望屏除掉一堆術語跟一堆炫砲的 ES78~~910~~ 寫法，以真正新手的角度寫一篇新手看得懂的文章。

但 Redux 相對說來也是比較進階的套件，所以最起碼還是要具備以下幾樣知識：
- NPM
- Github
- ES6 \(沒有也可以，但有會比較好理解\)
- React \(製作完整頁面\)

那我們就先從原理開始，接著一步一步實作，直到完成一個最簡易的元件\(Component\).

## <center>React & Redux</center> #
Redux 跟 React 是兩個完全獨立的函式庫，React 的多層元件使得傳遞資料變的非常繁瑣，而 Redux 則能十分有效的透過資料流來解決這個問題，無論是在開發過程或是除錯都會因為 Redux 帶來的資料流動而有了大幅的改善，在 css trick 網站上的介紹文中，一張圖片直接了檔的表示有無 Redux 的差異。
{% asset_img diff.png %}
[圖片來源 - Leveling Up with React: Redux](https://css-tricks.com/learning-react-redux/)
<br />

Redux 的運作會從 Component 發出`action`開始，action 會夾帶著必要的資訊給`reducer`，reducer 經過判斷後對資料逕行處理，放到`store`的 state tree 當中，最後再傳新的資料給 Component，重新渲染到畫面上。
{% asset_img reduxWork.png %}
[圖片來源 - JR BLOG](https://julienrenaux.fr/)
<br />

沒有親自實作的話，這些抽象的概念很容易搞混新手，所以捲起袖子，我們來寫 Code 吧！

---
<br>

## <center>環境建置 - Create React App</center> #
我們使用 Facebook 自己推出的工具來建立 React 的開發環境，真心建議只要跟 Webpack 相關的東西，最好都用官方自己推出的比較好，不然遇到個小問題就要花一堆精神去查資料，只會帶來滿滿的挫折感而已。
[Create React App](https://github.com/facebook/create-react-app)

```sh
git clone git@github.com:facebook/create-react-app.git
npm install
```
從 Github 上把專案 Clone 下來進行安裝。

```sh
cd create-react-app
npx create-react-app redux-demo
cd redux-demo
npm start
```
建立專案，開啟來看看是否都正常，如果出現這個畫面就是沒問題囉。

{% asset_img readyToGo.png %}
<br />

接著清理根目錄底下的`src/`，只留下`index.js`，然後建立幾項資料夾，等等會依序用到。

```
// 整理成這樣
src
 ├─ actions
 ├─ reducer
 ├─ components
 ├─ container
 ├─ index.js
```

其實應該要留下`App.js`，但因為裡面要改的內容很多很雜，怕新手混亂所以乾脆砍掉，實作的途中我們會再把它建立回來。接著我們把`index.js`內不必要的東西也清理一下。
```js
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';     // <--拿掉
import App from './App'; // <--拿掉
import registerServiceWorker from './registerServiceWorker'; // <--拿掉

ReactDOM.render(<App />, document.getElementById('root'));
registerServiceWorker();  // <--通通拿掉
```

Okay! 感覺像是年末大掃除一樣，資料夾底下乾淨許多了。

---
<br>
## <center>安裝 Redux</center> #
現在來安裝這次的主角 Redux。

Redux 在獨立運作的情況下，要上手並不困難。一旦搭配了 React 就還會多出三四種新東西，要一新手一下子就吸收，其實並不容易。除此之外我們還要安裝一個套件：`react-redux`裡面提供了`<Provider/>`能夠更完善的把兩者結合在一起，還有`connect()`可以將資料傳遞給元件，後半段會實作一次這個過程。
```sh
npm install redux --save
npm install react-redux --save
```
好，到這裡我們的前置動作就完成了，是不是很簡單呢？ 真的很簡單。

## <center>實(ㄕˊ) 作(ㄗㄨㄛˋ)</center> #
我們現在要製作一個能夠加減的計數器，透過這樣簡易的功能，我們就能夠用最少的學習成本來理解 Redux，還記得前面說過的概念嗎？一切都是從發出`action`開始，我們也先從建立`action`來一步一步打造完整的內容吧。

---
### <center>Action</center> #
```js
{
  type: 'PLUS',
  val: 1
}
```
這樣就是一個`action`，跟 JS 隨處可見的 Object 幾乎沒什麼差異，每一個`action`必定會帶著一個`type`以及其他需要的資料，`type`是用來讓之後會出現的`Reducer`來判斷要做什麼事情，而其他資料則是`Reducer`在處理事情時，需要用到的資訊。以上面這個 Action 來說，他會告訴 Reducer 要進行“PLUS”這個行為(等等會在 Reducer .js 之中定義)，並且給了 val: 1 的值，也就組成我們的主要的+1行為。

當然這樣直接丟一個 action 是沒辦法使用的，還要像下面把它包裝成`action creater`。
```js
// Action creater
function add() {
  return {
    type: 'PLUS',
    val: 1
  }
}
```
好了`action`真的很簡單，現在我們把全部的功能\(也才兩個\)給補齊吧。

```js
// src/actions/action.js

export const PLUS = 'PLIS';
export const MINUS = 'MINUS';
// 看到這裡你一定會有點困惑，上面這兩行叫做 action type，
// 但是別管他了，你有聽過 export 嗎？ 這裡每一行開頭都有個 export，
// 每個開頭被賦予 export 的變數、函示，都能在別的檔案透過 import { name } from 'URL' 的方式引入，
// import、export、{還有這個叫解構賦值\(Destructuring Assignment\)}，三種都是 ES6 的寫法。
// 真的，很實用。

export function add(){
  return {
    type: PLUS,
    val: 1,
  }
};
export function sub(){
  return {
    type: MINUS,
    val: 1,
  }
};
// 好，我們現在可以回頭說說 action type，有看到 action creater 底下的 type 嗎？
// 因為 type 的字串與 action creater 都會需要在別的檔案中使用到，透過這樣綁在變數上的方式，
// 如果我們之後在別的地方打錯字的話，至少會在 console 上跑出紅紅的 not defined 來提醒我們，
// action type 可用可不用，但不論是管理、解讀或偵錯，使用 action type 只有滿滿的好處而已。
```

到這裡 action 的部分就完工了，
接下來我們要來製作`reducer`...接收`action creater`之後進行處理的運作中樞。

---
### <center>Reducer</center> #
reducer 會帶入兩個參數 - 1.現在的 state. 2.發出的 action，接著使用 switch\(\) 與`type`來判斷該做什麼事情，最後再回傳新的 state 就大功告成。但有一點非常重要，reducer 必須是一個*pure function*，這代表說絕對不要在 reducer 內作下列的事情：
- Call API、改變路由...等等會造成 side effect 的動作.
- 有`new Date()`、`Math.random()`等傳值不穩定的 function.
- 改變帶入的 state、action.

給 A 就回傳 B, 不會是 C 或 D，不會有意外，也沒有驚喜。

```js
// src/reducer/reducer.js

import { combineReducers } from 'redux';
import { PLUS, MINUS } from '../actions/action';
// import 對應我們之前寫的 export，能夠把內容匯入到這裡，再來 {} 是先前提過的解構賦值，
// 以 action.js 這段來說，寫法同等於 action.PLUS，也能用在取用物件內的資料。
// combineReducers 如其名，專案變得龐大以後，我們會需要多個 reducer 分別處理不同的資料
// 而 combineReducers 就是用於整合所有 reducer，輸出使用。

const initVal = {
  val: 0
} // 先建立預設值

function counter(state = initVal, action) {
  // 第一個參數的寫法也是 ES6 新玩意...Default parameters
  // 代表如果沒有帶參數，則 state 為 = 後的變數。
  switch(action.type) {
    default:
      return state; // 如果真的有錯誤，則回傳原本的 state
    case PLUS:
      return Object.assign({}, state, {val: state.val + action.val})
      // reducer 必須保持 pure，所以我們絕對不能改變帶入的參數
      // 我解釋一下 Object.assign() 這段如何運作：
      // 首先建立一個空物件，再以原本的 state 覆蓋空物件，再用改變後的直覆蓋前個物件，回傳。

    case MINUS:
      return { ...state, val: state.val - action.val }
      // 讚嘆 ES6！這“...”叫做展開運算子(Spread Operator)，這樣就能在物件中帶入 state 的資料
      // 接著一樣寫改變後的 state 覆蓋重複的部分，展開運算子同樣也可以用在陣列中。
  }
};

const counterApp = combineReducers({
  counter
}); // 用 combineReducers() 來打包，如果以後有多的 reducer，就可用物件的形式放進去。
export default counterApp;
// export default 一個檔案內只能寫一次，代表匯出整個檔案內容。
```
哇！恭喜～不屈不饒不放棄的你，終於抵達這裡了～
不過前面還很遠，就跟職涯一樣很漫長，你的肝也還很新鮮。
所以站起來動動身體，我們還要繼續後半段。

---
### <center>Components</center> #
加入 Redux 以後，React Component 就有了兩新概念：

1- Presentational Component
2- Container Component

為了方便起見，
我們就把 1 稱為元件(Component)、2 叫做容器(Container)，有沒有感覺親切許多？
元件負責接收 props 並渲染到使用者的畫面上，其實就是我們以往寫的 React 元件，會有這名稱上的變化是為了與容器做出差異，那麼容器是做什麼呢？你可以把它想像成一個蓋住元件的遮罩，只是這個遮罩不是為了阻擋東西，而是為了作為傳遞資料的橋樑。官網教學有製作出一個表個，好讓大家仔細比較一下差異：

{% asset_img table.png %}

---
<br>

#### <center>Presentational Component</center> #
Presentational Component 就是我們早已習慣的 React Component，不贅述直接看 Code。

```js
// src/components/counter.js
import React from 'react';

const Counter = ({value, addEvent, subEvent}) => {
  // 預設 prop 會傳來數值、與加減兩種事件，再用解構賦值的方式取出。
  return (
    <div>
      <button onClick={() => subEvent()}>-</button>
      <h1>{value}</h1>
      <button onClick={() => addEvent()}>+</button>
    </div>
  );
}

export default Counter;
```

#### <center>Container Component</center> #
Container Component 是完全的新東西，通常不會負責畫面上的表現。
負責傳遞 state 與 dispatch，擔當起 React Component 與 Redux 之間的橋樑。 
```js
// src/containers/counterContainer.js
import { connect } from 'react-redux';
import Counter from '../components/counter';
import { add, sub } from '../actions/action';

// 傳遞 state 至 props 的 function, 通常命名為 mapStateToProps
const mapStateToProps = (state) => {
  return {
    value: state.counter.val
    // counter 是 reducer 的名稱，也就是說 reducer 是 state tree 的分支點。
    // key 值的 value，就是接收的 props 的名稱
  }
}

// 與上面一樣，但這是傳遞 dispatch 的版本
const mapDispatchToProps = (dispatch) => {
  return {
    addEvent: () => {dispatch(add())},
    subEvent: () => {dispatch(sub())},
    // 同樣的，key 值就是接收的 props 的名稱
  }
}

// 連結的關鍵，使用 connect() 串起前兩個 function，最後帶入的參數則是要連結的元件。
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(Counter);
```

將元件分為這兩類，
能夠在專案龐大以後好維護管理，但不免還是會有些情況會沒辦法把兩者分離。
有時候也可以嘗試看看將兩者混用，就像下面官網的範例：
```js
// 官網範例，跟本文的實作無關

import React from 'react'
import { connect } from 'react-redux'
import { addTodo } from '../actions'

let AddTodo = ({ dispatch }) => {
  let input

  return (
    <div>
      <form onSubmit={e => {
        e.preventDefault()
        if (!input.value.trim()) {
          return
        }
        dispatch(addTodo(input.value))
        input.value = ''
      }}>
        <input ref={node => {
          input = node
        }} />
        <button type="submit">
          Add Todo
        </button>
      </form>
    </div>
  )
}
AddTodo = connect()(AddTodo)

export default AddTodo
```

#### <center>Root Component</center> #
好，主要的功能都完成了，
現在我們來補上 root component 吧～沒什麼特別的，就跟平常的 React 一樣。
```js
// src/app.js
import React from 'react';
import CounterContainer from './containers/counterContainer';

const App = () => (
  <div>
    <CounterContainer />
  </div>
);

export default App;
```
要特別注意一下，
因為我們已經把 component 包在 container 底下了，所以根元件這邊要匯入 container。
另外檔案目錄分類也需要提醒，App.js 這個根元件從檔案結構上看來是屬於 component，所以在官網範例中，App.js 被放到 components 資料夾底下，我自己是覺得這樣分類不方便，因此將他獨立出來，與 index.js 一起放在根目錄。

---
### <center>Store</center> #
好了，最複雜的部分都結束了。接下來很簡單，
就像魔戒裡面佛羅多把至尊戒丟到岩漿裡一樣簡單，沒有咕嚕在那邊亂的話，事情就這麼簡單。
你就是佛羅多，最後的步驟就項丟戒指到岩漿裡這麼easy。而我，就是咕嚕。   
<br>
<br>
.....開玩笑的。
才不會這麼戲劇化，最後這個步驟真的很簡單，簡單到我還想了上面這段來充字數。

回顧一下，我們現在有哪些東西：
- action.js : 建立不同的 action 讓 reducer 判斷要做什麼事情。
- reducer.js : 處理中樞，用 switch() 判斷 action.type 來對 state 進行處理。
- counter.js : 接收 props，唯一會出現在使用者畫面中的元件。
- counterContainer.js : 負責傳遞 state 與 dispatch 給指定的元件。
- app.js : 沒有反應，就是個根元件。
- index.js : 將一切 React 渲染至畫面的窗口。

六顆無限寶石都到手，只要裝到手套上我們就....我們就可以....完成單向資料流了...

```jsx
//src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import counterApp from './reducer/reducer';
import App from './app';


let store = createStore(counterApp);
// 依據我們撰寫的 reducer 建立出 store

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  // 然後全部把它裝上去！
  document.getElementById('root')
);
```

好，大功告成！
回到我們的終端機！
```sh
npm run start
```

然後享受成果～

{% asset_img done.gif %}
<center>woooooooooowowowowowoow~~!!</center>
<br>

---
<br>

## <center>總結</center> #
有沒有感覺明明只是一個計數器，卻感覺花了好大的功夫？的確，要應用 Redux 到一個極小的專案上面，是會有許多麻煩的地方。但是想一想，如果今天是一個正式的網站，使用者光是輸入資料就會穿透五層的元件來進行操作，不就要寫一堆 props、每層還要驗證跟傳遞，出了錯誤更要花時間慢慢找出問題點，光想到就覺得煩躁吧？只要花點心思先把 Redux 建立好，就能讓所有元件從唯一的 Store 存取資料，就算出問題我們也可以找出對應的 Reducer 來檢查！

最後用一張我自己畫超爛的概念圖，希望多少可以幫助大家理解啦ＱＱ
{% asset_img iThink.png %}

<br>
給自己休息一下，接著就來試著自己把 Redux 應用到自己的專案上吧！

---
<br>
## <center>相關文章</center> #
- [從0到100打造一個 React Native boilerplate](https://legacy.gitbook.com/book/noootown/deeperience-react-native-boilerplate/details)
- [Redux 教學](https://chentsulin.github.io/redux/index.html)
- [見證奇蹟的時刻 - 實作Redux](https://ithelp.ithome.com.tw/articles/10196346)

---