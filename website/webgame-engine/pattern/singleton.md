# 單例模式

一個 Class 只會有一個 Instance 被創建，整隻程式都只使用這個 Instance，就叫做單例模式。

我們通常會這麼樣去使用：

```ts
class SingletonClass {
  private static instance: SingletonClass = new SingletonClass();

  constructor() {}

  public static get Instance() {
    return this.instance;
  }
}
```

!!! note

    TypeScript static method 中使用的 `this` 可用於取用同一Class的其他靜態方法或屬性
    因此這邊的 `this.instance` 實際上等同於 `SingletonClass.instance`。

以上的範例會在 Class 載入時建立 instance，被稱作`積極單例`；
與之對應的是`延遲建立實例`(lazy initialization)，如以下範例：

```ts
class SingletonClass {
  private static instance: SingletonClass = null;

  constructor() {}

  public static get Instance() {
    if (!SingletonClass.instance) {
      this.instance = new SingletonClass();
    }
    return this.instance;
  }
}
```

用這種做法會在第一次呼叫 Instance 時才會實例出來

## 使用 Singleton 的優缺點

### 優點

- **簡化存取**： 單例模式能夠使物件的存取變得相對簡單，特別適合那些需要在應用程式中頻繁使用的共用實例。

- **節省資源**： 單例模式在程式執行過程中只創建一個實例，避免了重複創建的成本，有助於節省資源。

### 缺點

- **高度耦合**： 過度使用單例模式可能導致各個類別之間高度耦合，使得程式結構難以理解和維護。

- **測試困難**： 單例模式可能導致測試變得複雜，因為它引入了全域狀態，而測試通常需要隔離和控制單個對象的狀態。

### 使用建議

在使用單例模式時，應慎選適用的情境。單例模式適合於需要在程式中共用單一實例並提供全域訪問點的情況，應避免在所有類別中過度使用，以免導致高度耦合的情況。另外，應該適度使用單例模式，避免對其進行過度的依賴，並在一些情境中考慮其他模式如依賴注入，以更靈活地解決相關問題。

## 服務定位器

服務定位器模式（Service Locator Pattern）與單例模式相似，它主要提供一個中央化的服務註冊表，允許各元件根據名稱存取這些服務或資源，而不需直接涉及實作細節。這種方式有效地降低了元件之間的相依性。以下是一個簡單的例子：

```ts
// Service Locator Game.ts
class Game {
  private static instance: Game = new Game();

  constructor() {}

  private _eventTarget: EventTarget = new EventTarget(); // 全域事件收發
  private _i18n: I18n = new I18n(); // 多語系模組
  private _network: Network = new Network(); // 連線模組
  private _audio: AudioManager = new AudioManager(); // 音效模組

  public static get Instance() {
    return this.instance;
  }

  public static get event() {
    return this.instance._eventTarget;
  }

  public static get i18n() {
    return this.instance._i18n;
  }

  public static get network() {
    return this.instance._network;
  }

  public static get audio() {
    return this.instance._audio;
  }
}
```

```ts
// Component CustomButton.ts
class CustomButton extends Button {
  protected onLoad() {
    this.node.on(Button.EventType.CLICK, this.onClick, this); // 監聽按鈕點擊事件
  }
  private onClick() {
    Game.audio.playEffect("button"); // 播放按鈕音效
    Game.event.emit("onClick"); // 發送一個事件
  }
}
```

以上的程式碼讓 `Game` 充當服務定位器，負責提供全域性的服務，而 `CustomButton` 透過 `Game` 來取得需要的服務，降低了元件 `CustomButton` 對具體服務的直接相依性，提高了程式碼的靈活性和可測試性。
