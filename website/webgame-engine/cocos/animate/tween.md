# Tween

## **主要的用處**
* 直接用腳本完全控制動畫內容
* 針對指定的屬性變化提供緩動效果
* 適合應用於較為不固定、需計算數值的動畫
* 不適合處理複雜的動畫需求

## **Tween使用方式**

* 透過tween(target)構造一個實例
    
    * target可以指定為任一物件
    
    - 包含節點或組件
    ``` ts 
    // 指定為節點 可以控制節點的屬性
    tween(this.node)
    // 指定為UIOpacity元件 可以控制透明度
    tween(this.getComponent(UIOpacity))
    ```

* 以Method chaining的方式組合動畫流程
    ``` ts
    tween(this.getComponent(UIOpacity))
        .to(1, { opacity: 0 }) // 指定透明度在1秒間下降至0(淡出)
        .start() // 開始執行緩動
    ```

## **Tween常用組件接口**

* start() 開始播放緩動
    * 通常會在Method chaining的最後被呼叫

* to(duration, {property})、 by(duration, {property})
    * to：將屬性值降至指定的數值 (絕對值)
    * by：將屬性值降低指定的數值 (基於原本數值的相對值)

* to、by差異實際案例
    ``` ts
    this.node.getComponent(UIOpacity).opacity = 255
    tween(this.getComponent(UIOpacity))
        .by(1, { opacity: -100 }) // 透明度在1秒降低-100 => 最終為155
        .to(1, { opacity: 0 })  // 透明度一秒降到0 => 降低了155
        .start() // 開始執行緩動
    ```

* delay(duration: number) 延遲幾秒執行
* repeat(times: number, embedTween: Tween)
    * 若有傳入embedTween，則為表演time次embedTween
    * 若未傳入，則是上一個表演再重複time次
    ``` ts
    tween(this.node)
        .to(0, { angle: 0 }) // 指定角度為0
        .by(1, { angle: 30 }) // 指定1秒角度提升30度
        .repeat(4) // 重複4次 (4秒間提高120度，每一秒提高30度)
        .start() // 開始執行緩動
    ```
* hide()、show()
    * 隱藏/顯示節點
    * **target為Node才有效果**
    ``` ts
    tween(this.node)
        .hide() // 隱藏
        .delay(1) // 延遲1秒
        .show() // 恢復顯示
        .start() // 開始執行緩動
    ```
* call(function)
    * 觸發一段callback
    * 不會影響到在這之後的tween參數
    ``` ts
    tween(this.node)
        .set({ position: v3() }) // 設定座標為(0,0,0)
        .by(1, { position: v3(0, 100, 0) }) // 使Y軸1秒內+100
        .call(() => {
            pos = v3(100, 0, 0) // 重新設置pos
        })
        .by(1, { position: v3(100, 0, 0) })  // 使Y軸1秒內+100，而不是X軸
        // 在這邊pos寫入position的時間早於call回掉的時機，因此call改變pos並不會影響上行
        .start() // 開始執行緩動
    ```
## **Tween－靜態接口**
* Tween.stopAll()
    * 停止所有緩動
* Tween.stopAllByTag(tag: number)
    * 停止所有指定tag的緩動
    * 每個tween可使用.tag(i) 設定各自的Tag
* Tween.stopAllByTarget(node: Node)
    * 停止節點上的所有緩動

## **Easing**
* 以較為平滑的方式表演動畫

<center>
![alter name](../../assets/tween/tween-1.gif)
![alter name](../../assets/tween/tween-2.gif)
![alter name](../../assets/tween/tween-3.gif)
</center>

* 目前Cocos支援的緩動類型如下
![alter name](../../assets/tween/tween-4.png)

* Easing 緩動效果
![alter name](../../assets/tween/tween-5.png)
    [線上範例](https://easings.net/)

* Tween指定Easing效果
    * 於to()、by()帶入第三個參數指定
    ``` ts
    tween(this.node)
        .to(1, { position: v3(0, -100) }, { easing: 'bounceOut' }) // 指定bounceOut緩動
        .start()
    ```
    ![alter name](../../assets/tween/tween-6.gif)

## **Cocos CurveRange Property**
應用在目標值的曲線範圍，可以用裝飾器的方式使用
### **事前作業**
需開cocos引擎中的粒子系統

![](../../assets/tween/tween-7.png)

![](../../assets/tween/tween-8.png)

### **裝飾器介紹**

``` ts
@property(CurveRange)
private curveRange: CurveRange = new CurveRange();
```

我們可以看到它有四種模式

![](../../assets/tween/tween-9.png)

* Constant ：

![](../../assets/tween/tween-10.png)

* Curve：

![](../../assets/tween/tween-11.png)

* TwoConstants：

![](../../assets/tween/tween-12.png)

* TwoCurves：

![](../../assets/tween/tween-13.png)

### **實際操作**

我們選擇Curve接著我們可以自定義使用的取線

![](../../assets/tween/tween-14.png)

![](../../assets/tween/tween-15.png)

我們可以讓tween使用自己設計的曲線
```ts
tween(this.node)
    .to(1, { position: v3(0, -100) }, {
        easing: (k) => {
            return this.curveRange.evaluate(k, 1)
        }
    })
    .start()
```
![](../../assets/tween/tween-16.gif)
