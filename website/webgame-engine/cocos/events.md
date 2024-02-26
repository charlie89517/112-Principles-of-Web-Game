# 事件系統

在開始本章之前，建議先對 [觀察者模式](/webgame-engine/pattern/observer.md) 有一些基礎的了解。

## EventTarget

Cocos 的事件系統大多是透過 EventTarget 實現，Node 本身即為最常見的 EventTarget，EventTarget 具有以下幾種常見的方法：

- **on**：監聽指定事件
- **once**：監聽一次指定事件
- **off**：移除指定事件的監聽
- **emit**：透過事件名稱發送事件

### on & once

這兩個方法的差別只在於，同一事件發生多次時，`on`每次都會被觸發到，`once`在第一次觸發後就會被回收，因此後面無論又觸發了幾次都不會動作。它們具有以下參數

- `type: string` 事件名稱
- `callback: Function` 事件觸發時，所要執行的 callback 函式，當事件觸發時有附帶額外參數的話，也會一併傳入作為 `callback` 的參數。
- `target?: Object` 執行 callback 的目標物件，為可選參數

實際應用案例如下：

```ts
protected onLoad(): void {
  // 監聽事件，當事件觸發時執行 onClick 方法
  this.node.on(Button.EventType.CLICK, this.onClick, this)
}

private onClick(): void {
  console.log('點了按鈕一下!')
}
```

!!! Note

    簡而言之在 on 時有提供 `target` 參數，就會幫你綁定 `target` 為 `this` 物件。<br>
    關於 `this` 值詳細的介紹，可以參考 [This 與 閉包](/webgame-engine/learn-js/this-and-closure.md)

### off

與 `on` 相反，解除指定對象的事件監聽

```ts
protected onLoad(): void {
  // 監聽事件，當事件觸發時執行 onClick 方法
  this.node.on(Button.EventType.CLICK, this.onClick, this)
  // 取消監聽
  this.node.off(Button.EventType.CLICK, this.onClick, this)
}
```

### emit

當要觸發一些自定義的事件時，可以使用此方法發送事件。

```ts
// in MainGame component
private startGame(): void {
   // 發送 `GameEvent.onNewGameStart` 事件
    this.node.emit(GameEvent.onNewGameStart);
}

// in other component
protected onLoad(): void {
  // 監聽 `GameEvent.onNewGameStart` 事件
  GameManager.Instance.node.on(GameEvent.onNewGameStart, this.onNewGameStart, this);
}
```

## Cocos 事件

### Node 常用事件

- **Node.EventType.TOUCH_START**：手指、滑鼠按下時
- **Node.EventType.TOUCH_MOVE**：手指滑動、滑鼠移動時
- **Node.EventType.TOUCH_END**：在節點區域內手指離開螢幕/滑鼠放開時
- **Node.EventType.TOUCH_CANCEL**：當在節點區域外手指離開螢幕/滑鼠放開時

Touch 系列的事件會提供 EventTouch 物件作為參數傳入

```ts
private onTouchStart(event: EventTouch) {
    const { x, y } = event.touch.getLocation(); // 點下去的位置的'世界'坐標
}
```

!!! Note

    Cocos有提供滑鼠的監聽事件系列，如`MOUSE_DOWN`等，但無法在手機上觸發，所以通常使用Touch系列的事件進行監聽。

### 常用全域事件

- **Input.EventType.KEY_DOWN**：按下鍵盤時
- **Input.EventType.KEY_UP**：放開鍵盤時

```ts
private onKeyboardDown(event: EventKeyboard) {
  // 判斷按下去的按鍵是哪一個
  switch (event.keyCode) {
    case KeyCode.KEY_W:
      break;
    case KeyCode.KEY_A:
      break;
    case KeyCode.KEY_S:
      break;
    case KeyCode.KEY_D:
      break;
    case KeyCode.SPACE: // 空白鍵
      break;
    default:
      break;
  }
}
```

## 事件捕捉

事件系統的處理流程大致上可分為`事件捕捉`、`事件冒泡` 2 個階段。
以點擊事件為例，當使用者按下滑鼠點擊畫面中的某個元素時，事件系統會以最上層的根節點作為起點，經過一層一層的檢測後找出實際上使用者點擊到的元素，這個流程被稱作`捕捉階段`。

## 事件冒泡[^1]

當事件捕捉完成後，事件會傳遞到使用者按下的實際元素上，此時會開始`冒泡階段`。在這個階段事件會從目標元素上開始被觸發，目標元素觸發完之後會往上使父節點觸發事件直至根節點。

以下圖場景為例，當滑鼠或手指在 c 節點區域內按下時，事件將首先在 c 節點觸發，c 節點接收到事件。 接著 c 節點會將事件傳遞到其父節點， b 節點會將事件再傳遞給父節點 a 。 這就是最基本的事件冒泡過程。 需要強調的是，在觸摸事件冒泡的過程中不會有觸摸檢測，這意味著即使觸點不在 a b 節點區域內，a b 節點也會透過觸摸事件冒泡的機制接收到這個事件。
![事件冒泡](https://i.imgur.com/6KBmLTe.png)
要關閉事件冒泡，可以將 `event.propagationStopped` 設為 `true`
![關閉事件冒泡](https://i.imgur.com/2VTqJNy.png)

![w3c eventflow](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)

<center>▲ 事件捕捉與事件冒泡 [^2]</center>

!!! Warning

    由於事件冒泡是從目標節點往根節點層層傳遞，因此就算畫面視覺上兩個元素可能是重疊的，也不會冒泡到同一層級的節點，在設計場景結構時需多加注意。

## 事件應用的正反例

### 不使用事件系統時

- 控制遊戲流程的核心：需要呼叫底下的所有子元件進行處理，高度耦合。
  ![事件系統-反例](https://i.imgur.com/Fdio6nw.png)

### 使用事件系統時

- 控制遊戲流程的核心：只需要發送事件。
  ![事件系統-正例核心](https://i.imgur.com/kFhXcCh.png)
- 各個子元件：只需要監聽事件發生。
  ![事件系統-正例子物件](https://i.imgur.com/WRCs1kb.png)

[^1]: Cocos Creator 節點事件系統 https://docs.cocos.com/creator/3.6/manual/zh/engine/event/event-builtin.html
[^2]: W3C - Event Flow https://www.w3.org/TR/DOM-Level-3-Events/#event-flow
