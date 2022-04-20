# 三点リーダー

## [参考コード](https://codesandbox.io/s/eloquent-shamir-15hz2?file=/src/App.tsx)

## 一行
- ブロック要素にoverflow等の三点のCSSを付けることが重要。
  - spanにclassを当てても三点リーダーがでない。
- 三点リーダーを行う文章等はブロック要素では囲まない。
- display: flexを使用することでspanでも使用できるようになる。

---
### 普通の三点リーダーのjavascript
```javascript
import "./styles.css";

export const FirstLine = () => {
  return (
      {/* 普通の一行の三点リーダー */}
      <div className="normal">
        <span>この文章は三点リーダーを試すものです。</span>
      </div>
  );
};


```

### 普通の三点リーダーのcss(div、span構成の普通なタイプ)
``` css
/* ノーマルな三点リーダーCSS */
.normal {
  width: 100px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```

---

### flexを使用した三点リーダーのjavascript
```javascript
import "./styles.css";

export const FirstLine = () => {
  return (
      {/* flexを使用した一行の三点リーダー */}
      <div className="flex">
        <span className="flexSpan">この文章は三点リーダーを試すものです。</span>
      </div>
  );
};


```

###  flexを使用した三点リーダーのcss
``` css
/* flexの三点リーダーCSS */
.flex {
  display: flex;
  width: 100px;
  white-space: nowrap;
}
.flexSpan {
  overflow: hidden;
  text-overflow: ellipsis;
}

```

### min-widthの三点リーダーのjavascript
- flexBoxからコンテンツがはみ出してしまう時に使用する。
- 正直、なぜ`min-width: 0`で三点リーダーになるのは謎である。（わかる人いれば教えてほしいです。。）

```javascript
import "./styles.css";

export const MinWidth = () => {
  return (
    <div className="content">
      <div className="item">
        <span className="text">demoTest</span>
        <span>3</span>
      </div>
      <span>demo</span>
    </div>
  );
};


```

###  min-widthの三点リーダーのcss
``` css
.content {
  width: 100px;
  display: flex;
  gap: 5px;
}
.item {
  display: flex;
  gap: 5px;
  min-width: 0;
}
.text {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-size: 1.2rem;
  color: red;
}

```

---

### 先頭だけ三点リーダーしたい時のjavascript
```javascript
import "./styles.css";

export const FirstLine = () => {
  return (
      {/* 先頭だけ三点リーダーしたい時 */}
      <div className="flex">
        <span className="flexSpan">この文章は三点リーダーを試すものです。</span>
        <span>demo</span>
      </div>
  );
};

```

### 先頭だけ三点リーダーしたい時のcss
``` css

/* flexの三点リーダーCSS */
.flex {
  display: flex;
  width: 100px;
  white-space: nowrap;
}
/* 先頭だけ三点リーダーしたい時 */
.multiple {
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

```

---


## 複数行の三点リーダー

- 複数行の三点リーダー

--- 

### 先頭だけ三点リーダーしたい時のjavascript
```javascript
import "./styles.css";

export const MultipleLine = () => {
  return (
    <div className="flexMultiple">
      <span className="multiText">
         私の名前は、＊＊＊＊です。よろしくお願いいたします。
      </span>
    </div>
  );
};

```

### 先頭だけ三点リーダーしたい時のcss
``` css

/* 複数行の三点リーダーCSS */
.multiText {
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  /* 行数 */
  -webkit-line-clamp: 3;
}

```

---