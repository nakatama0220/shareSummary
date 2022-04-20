# Event

## Eventとは
- Web上で起きる動作のこと。

## [Eventの登録: addEventListener](https://developer.mozilla.org/ja/docs/Web/API/EventTarget/addEventListener)

- 付与する要素.addEventListenerの引数は(付与するEvent, 付与する関数)
- reatはonclickイベントがあるためclickイベントを付与せずとも使用できるがpタグなどはclickイベントがないため必要なら付与するときに使用する。
- 他にもイベント系があるため付与してそのイベントが起きたとき関数が発火するイベントが付与できる(コピーやスクロールetc...)！！覚えるべき！

[イベントの一覧](https://developer.mozilla.org/ja/docs/Web/Events#%E3%82%A4%E3%83%99%E3%83%B3%E3%83%88%E3%81%AE%E4%B8%80%E8%A6%A7)

[Demo](https://stackblitz.com/edit/react-ts-1sbfcq?file=Hello.tsx)

### 実装例 (ライブラリなし)
```html
<button id="button">ボタン</button>
```
```javascript
// イベントを付与したいHTML要素を取得
const button = document.getElementById('button');
// イベントが発火したときに呼ばれる関数を定義(この関数はイベントハンドラと呼ばれる)
// ここではevent objectをlogに出力する関数を実装
const eventHandler = (e) => console.log(e);
// クリックイベントを登録する
// 
button.addEventListener('click', eventHandler);
```

### 実装例 (React)
```jsx
const Component: React.FC = () => {
  // イベントハンドラの定義
  const eventHandler = (e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => console.log(e);
  // ボタンにクリックイベントを登録
  return <button onClick={eventHandler}>ボタン</button>;
};
```

## [Eventの登録解除: removeEventListener](https://developer.mozilla.org/ja/docs/Web/API/EventTarget/removeEventListener)

## [Eventの要素取得: document.getElementById()](https://developer.mozilla.org/ja/docs/Web/API/Document/getElementById)

- idが一致した要素を表すElementオブジェクト返す
- 上記ではbuttonタグにid付与されたElementオブジェクトが取得。

## [Eventの要素取得: document.querySelector()](https://developer.mozilla.org/ja/docs/Web/API/Document/querySelector)

- 使用例が微妙なのでパソコン持ち帰ったら見てから行う(11/10)

## [Eventの要素取得: document.querySelectorAll()](https://developer.mozilla.org/ja/docs/Web/API/Document/querySelectorAll)

- 使用例が微妙なのでパソコン持ち帰ったら見てから行う(11/10)

## event.currentTarget
- イベントが登録された要素
- Element要素全体を取る(getElementByIdと同じ)

## event.target
- イベントが発火した要素
- 全体ではなく、クリックしたElement要素だけ取得する。

## <span style=" color: red;">基本的にe.targetを使用した方がいい。</span>
- e.target.valueなどを使用してinputタグの内容をstring型で取得できるためである。
- input type="text"
- input type="number"
- input type="password"
- input type="date"
- textarea
- selectbox
- 他

## event.target.checkedで値を取得
- input type="checkbox"
- input type="radio"

### 挙動確認コード

#### inside spanクリック
- target => HTMLSpanEement要素取得
- currentTarget => HtmlButtonElement

[Demo](https://stackblitz.com/edit/react-8snsqt?file=src/App.js)
```javascript
export default function App() {
  const handleClick = e => {
    console.log('target', e.target);
    console.log('currentTarget', e.currentTarget);
  };

  return (
    <div>
      <button onClick={handleClick}>
        <span>inside span</span>
      </button>
    </div>
  );
}
```

## 簡易的なDOM構造
<ul>
<li>Window</li>
<li>Document</li>
<li>html</li>
<li>main</li>
<li>divタグ等</li>
<li>aタグ等</li>
</ul>

## バブリングフェーズ
- 上記の簡易的なDOM構造を下から上に流れることをバブリングフェーズという(aタグ等からWindow)
- 子要素のイベントした後に、親要素のイベントが発火する。
- `addEventListener`の第３引数に`false`を渡すことでバブリングフェーズになる
  - defaultがfalseのため設定する必要がない。 

## キャプチャリングフェーズ
- 上記の簡易的なDOM構造を上から下に流れることをキャプチャリングフェーズという(Windowからaタグ等)
- 上から下にイベントが発火していく。
- 親要素のイベントが発生した後に、子要素のEvent発生
- `addEventListener`の第３引数に`true`を渡すことでキャプチャリングフェーズになる
```javascript
button.addEventListener('click', eventHandler, true);
```

### [preventDefault](https://developer.mozilla.org/ja/docs/Web/API/Event/preventDefault)
- その要素が持つイベントをキャンセル
- 自身の持つイベントは停止させるが、親要素のイベントは発火する。
- 親へのバブリングは行なう
- aタグ等が無効される。

### [stopPropagation](https://developer.mozilla.org/ja/docs/Web/API/Event/stopPropagation)
- 自身のイベントは発火するが、親要素のイベントを停止させる。
  - 子要素は無効にはならない
- 親へのバブリングを行なわない

### [stopImmediatePropagation](https://developer.mozilla.org/ja/docs/Web/API/Event/stopImmediatePropagation)
- 同じ要素に登録されたリスナーも実行しないようにする。
- 親要素の持つイベントも停止。

#### 挙動確認コード
[Demo](https://stackblitz.com/edit/web-platform-f1s4bx?file=script.js)
```html
<body>
  <form id="parent" class="parent">
    <button id="child" type="submit" class="child">
        <a
          id="grandchild"
          href="https://www.google.co.jp/"
          target="_blank"
          class="grandchild"
        >
          Link to Google
        </a>
      </button>
  </form>
</body>
```
```javascript
const parent = document.getElementById('parent');
const child = document.getElementById('child');
const grandchild = document.getElementById('grandchild');

function handleClickParent(e) {
  window.alert('parentが発火');
}
const handleClickChild = e => {
  window.alert('childが発火');
};
const handleClickGrandchild = e => {
  // e.preventDefault();
  // e.stopPropagation();
  // e.stopImmediatePropagation();
  window.alert('grandchildが発火');
};
const handleClickGrandchild2 = e => {
  window.alert('grandchild2が発火');
};

parent.addEventListener('click', e => handleClickParent(e));
child.addEventListener('click', handleClickChild);
grandchild.addEventListener('click', handleClickGrandchild);
grandchild.addEventListener('click', handleClickGrandchild2);
```
