# Particle System

## 介紹

粒子系統是指計算機圖學中模擬特定現象的技術，時常用來模擬自然現象(火、煙、水等等自然現象)

!!! Warning

    Cocos Creator 3.X 有提供兩種粒子系統：ParticleSystem 、ParticleSystem2D，分別對應3D及2D的粒子系統。其中2D的粒子系統 `ParticleSystem2D` 於 Cocos Creator 2.X 的版本時命名為 `ParticleSystem`，初學者使用時可能會產生混淆，需多加注意。
    本文所提到的粒子系統為2D的 `ParticleSystem2D`，其屬性設定相對較少，比較適合入門上手。

## 優勢

!!! Info

    要使用粒子系統，請先確認`項目設置`中，`功能裁減`分頁是否有將對應的粒子系統啟用。
    <center>![](/webgame-engine/assets/tween/tween-7.png)</center>

- 模擬爆炸、煙火、水流比較有真實性
- 相比其他特效更節省空間
- 修改播放時間比 Spine 更加便捷

## 應用

- 先建立粒子元件

![](/webgame-engine/assets/particleSystem/particleSystem-1.png)

- 就可以看到粒子系統的樣子

![](/webgame-engine/assets/particleSystem/particleSystem-2.gif)

- 打勾**Custom**就可以自訂粒子特效樣貌

![](/webgame-engine/assets/particleSystem/particleSystem-3.png)

## Particle System - 常用接口

- **play()**： 播放粒子特效
- **stop()**： 停止粒子特效
- **clear()**： 清除現有的粒子
- **pause()**： 暫停粒子特效
