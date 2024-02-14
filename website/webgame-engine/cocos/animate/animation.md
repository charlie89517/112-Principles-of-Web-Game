# Animation

## **介紹**

- 透過指定關鍵幀實現
- 官方提供動畫編輯器
- 適合應用於較為固定表演的動畫
  ![](../../assets/animation/animation-1.png)

## **建立 Animation**

- 在動畫的節點上新增 Animation 組件
  ![](../../assets/animation/animation-2.png)

- 新增 Animation Clip 檔案 並拉入 Clips
  ![](../../assets/animation/animation-3.png)
  ![](../../assets/animation/animation-4.png)

- 進入動畫編輯模式
  ![](../../assets/animation/animation-5.png)
  ![](../../assets/animation/animation-6.png)

## **Animation 使用方式**

1. 自動播放

   - 透過打勾”Play On Load”設定
   - 會播放 Default Clip 動畫

2. 透過腳本控制

## **Animation - 常用組件接口**

- 取得組件

```ts
const animation = this.node.getComponent(Animation);
```

- 播放 Default Clip

```ts
animation.play();
```

- 播放指定的 Clip

```ts
animation.play("Clip Name");
```

- 暫停/繼續動畫表演

```ts
animation.pause();
animation.resume();
```

- 取得指定的 Clip 狀態

```ts
const state: AnimationState = animation.getState("Clip Name");
state.duration;
state.current;
state.isPlaying;
```

## **Animation－事件**

- PLAY：開始播放時觸發
- STOP ：停止時觸發
- PAUSE：暫停時觸發
- RESUME ：恢復播放時觸發
- LASTFRAME：若動畫為循環播放，播放到最後一幀時觸發
- FINISHED：動畫播放完畢時觸發

## **Animation－事件使用方式**

- _注意：與其他組件不同，是直接在組件監聽_

```ts
const animation: Animation = this.node.getComponent(Animation);
animation.on(
  Animation.EventType.PLAY,
  (type: Animation.EventType, state: AnimationState) => {
    console.log("動畫已開始播放", state);
  }
);
```
