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

單例模式唯一性的優點除了節省資源外，有時是必需要保持該元件的唯一性，例如輸入控制器、音效控制器等如果有多個同時實例存在，會造成許多意外錯誤。而使用時較容易導致測試困難的部分，下面舉一個正反例，假設遊戲中控制音效的模組被稱作`AudioManager`，所有與音效相關的功能都由該 Class 負責時：

- 使用單例模式：每個要播音效的模組都要 Import `AudioManager`。

```ts
class MainShip {
  // 發射子彈
  public shoot() {
    // ...發射子彈的各種處理
    AudioManager.Instance.playEffect("ShootBullet"); // 播放發射子彈的音效
  }
}
```

- 使用服務定位器模式：透過一個全域的 Class 取用音效模組，無須認定是`AudioManager`還是其他的 Class。

```ts
class MainShip {
  // 發射子彈
  public shoot() {
    // ...發射子彈的各種處理
    Game.audio.playEffect("ShootBullet"); // 播放發射子彈的音效
    // 無須強制指定，而是透過Game進行存取
    // 所以只要保持介面一致，就算Game.audio的實例不是AudioManager也可以使用
    // 當AudioManager被替換時，可以省掉到處修改Import的時間
    // 並且如此一來，不管有多少個模組需要共用，都只會有`Game`一個全域單例。
  }
}
```

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
