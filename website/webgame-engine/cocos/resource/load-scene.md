# 場景加載與切換

## 場景預加載（director.preloadScene）

用於**預先**載入指定的場景。[^note1]

| 輸入參數   | 描述                           |
| ---------- | ------------------------------ |
| sceneName  | 場景名稱。                     |
| onProgress | 加載進度的 callback function。 |
| onLoaded   | 加載完成的 callback function。 |

```TypeScript
    director.preloadScene(sceneName, onProgress, onLoaded);
```

### onProgress

加載進度的 callback。[^note2]

| 輸入參數       | 描述                 |
| -------------- | -------------------- |
| completedCount | 加載完畢的資源數量。 |
| totalCount     | 資源總量。           |

!!! Warning

    `totalCount` 可能會提高，若將此數值用於計算載入進度時需注意。

### onLoaded

加載完成的 callback。[^note3]

| 輸入參數   | 描述                          |
| ---------- | ----------------------------- |
| error      | 例外資訊，正常載入時為 null。 |
| sceneAsset | 載入的場景資源。              |

## 場景切換（director.loadScene）[^note4]\*\*

用於切換至指定場景，若場景尚未加載則會先載入再加載，因此不一定要先呼叫 `director.preloadScene`。

| 輸入參數  | 描述       |
| --------- | ---------- |
| sceneName | 場景名稱。 |

```TypeScript
    director.loadScene(sceneName);
```

## 使用範例

```TypeScript
export class Loading extends Component {
    @property(ProgressBar)
    private readonly progressBar: ProgressBar = null;

    @property(Label)
    private readonly progressLabel: Label = null;

    protected onLoad(): void {
        director.preloadScene('Game', this.onProgress.bind(this), this.onLoaded.bind(this));
    }

    private onProgress(completeCount: number, totalCount: number): void {
        const newPercent = completeCount / totalCount;
        // 避免 totalCount 提高而導致計算出來的 progress 反而變低
        if (newPercent > this.progressBar.progress) {
            this.progressBar.progress = newPercent;
            this.progressLabel.string = `${Math.trunc(newPercent * 100)}%`;
        }
    }

    private onLoaded(): void {
        // 預載完畢後，進入主遊戲場景
        director.loadScene('Game');
    }
}

```

[^note1]: [Cocos Creator 3.6 API - Director - preloadScene](https://docs.cocos.com/creator/3.6/api/en/class/Director?id=preloadScene)
[^note2]: [Cocos Creator 3.6 API - Director - OnLoadSceneProgress](https://docs.cocos.com/creator/3.6/api/en/namespace/Director?id=OnLoadSceneProgress)
[^note3]: [Cocos Creator 3.6 API - Director - OnSceneLoaded](https://docs.cocos.com/creator/3.6/api/zh/namespace/Director?id=OnSceneLoaded)
[^note4]: [Cocos Creator 3.6 API - Director - loadScene](https://docs.cocos.com/creator/3.6/api/en/class/Director?id=loadScene)
