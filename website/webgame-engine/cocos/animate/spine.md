# Spine

## 介紹

- 針對遊戲開發的 2D 骨骼動畫工具
- 將圖片綁定到骨骼上、透過控制骨骼實現動畫
- 更少的美術素材、更流暢的表演
- **收費軟體**
- [官方網站](http://zh.esotericsoftware.com/)

![](/webgame-engine/assets/spine/spine-1.gif)

## Spine 範例素材

!!! Note

    官方提供的素材無法直接在 Cocos Creator 上使用，我們有重新輸出一份可以使用的版本於下方`下載連結`。

- 官方提供的 [SpineBoy](http://zh.esotericsoftware.com/spine-examples-spineboy#Spineboy) 範例

  ![](/webgame-engine/assets/spine/spine-2.gif)

  [Spineboy素材下載連結](/webgame-engine/assets/spine/SpineBoy.zip)

- 官方提供的 [Mix-And-Match](http://zh.esotericsoftware.com/spine-examples-mix-and-match) 範例

  ![](/webgame-engine/assets/spine/spine-3.gif)

  [Mix-And-Match素材下載連結](/webgame-engine/assets/spine/mix-and-match.zip)

## 腳本控制案例

- 利用腳本控制動畫、取得相關資訊
- 範例遊戲專案
    * 場景 Tutorial-Week6
    * 組件 SampleSpine
    ![](/webgame-engine/assets/spine/spine-4.png)

## sp.Skeleton - 常用組件接口

- setAnimation(trackIndex, name, loop)
    - 設定當前動畫
    - 安排至隊列中的所有動畫都會清除
    - trackIndex – 通常傳入 1
    - name – 動畫名稱
    - loop – 是否循環播放

- addAnimation(trackIndex, name, loop, delay)
    - 指定一個動畫至隊列中
    - trackIndex – 通常傳入 1
    - name – 動畫名稱
    - loop – 是否循環播放
    - delay - 可選，指定延遲幾秒後播放

- findAnimation(name)
    - 以動畫名稱取得相關資訊
    - name – 動畫名稱

    - 可使用此方法取得動畫時長
    - spine.findAnimation(‘run’).duration

- setCompleteListener(callback)
    - 動畫播放完成時的事件監聽
    - callback – 提供一參數（sp.spine.TrackEntry）
    ```ts
    spine.setCompleteListener((entry: sp.spine.TrackEntry) => {
      console.log("Spine Animation End:", entry.animation.name, entry);
    });
    ```
- pause
    - 指定動畫播放/暫停
    - spine.pause = false; // 暫停
- setSkin(skinName)

    - 指定 Spine 皮膚
    - skinName – 皮膚名稱

    - 相同的骨骼，貼上不同圖片

- Skin 換裝的範例使用 Mix-And-Match 範例可測試
  ![](/webgame-engine/assets/spine/spine-5.gif)

## sp.Skeleton - 其他屬性介紹

- Animation Cache Mode－動畫的快取方式

    - **REALTIME**
        - 實時運算，但支援所有功能
    - **SHARE_CACHE**
        - 將骨骼資訊跟貼圖緩存，與其他 sp.Skeleton 共享
        - 不支援動作融合、疊加、換裝及一部份事件
        - 有大量重複 Spine 使用時，能大幅降低緩存使用及高性能
    - **PRIVATE_CACHE**
        - 進行緩存，但不與其他 sp.Skeleton 共用
        - 適合有換裝的情況，但希望提高性能

- Premultiplied Alpha－啟用貼圖預乘

    - 需要看該 Spine 專案輸出時是否有啟用，否則會發生異常

- TimeScale – 所有動畫的時間倍率

- Sockets：Spine 掛點

    - 將指定的節點掛到 Spine 動畫上
    - 掛上的節點會跟隨 Spine 表演
      ![](/webgame-engine/assets/spine/spine-6.gif)

    - 範例：將圖片掛到右手上
      ![](/webgame-engine/assets/spine/spine-7.png)
      ![](/webgame-engine/assets/spine/spine-8.png)
