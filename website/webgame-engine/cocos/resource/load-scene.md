# **Load scene**
## **場景預加載 ( director.preloadScene )**

用於**預**加載指定場景。[^note1]

| 輸入參數 | 描述 |
| -- | -- |
| sceneName | 場景名稱。 |
| onProgress | 加載進度的 callback。 |
| onLoaded | 加載完成的 callback。 |

```TypeScript
    director.preloadScene(sceneName, onProgress, onLoaded);
```

### **onProgress**

加載進度的 callback。[^note2]

| 輸入參數 | 描述 |
| -- | -- |
| completedCount | 加載完畢的資源數量。 |
| totalCount | 資源總量。 |

> totalCount 可能會提升。

### **onLoaded**

加載完成的 callback。[^note3]

| 輸入參數 | 描述 |
| -- | -- |
| error | 例外資訊，正常載入時為null。 |
| sceneAsset | 載入的場景資源。 |

## **場景切換 ( director.loadScene )**

用於加載指定場景。[^note4]

| 輸入參數 | 描述 |
| -- | -- |
| sceneName | 場景名稱。 |

```TypeScript
    director.loadScene(sceneName);
```

## **使用範例**
![使用範例](/webgame-engine/assets/load-scene/example.png)


[^note1]: [Cocos Creator 3.6 API - Director - preloadScene](https://docs.cocos.com/creator/3.6/api/en/class/Director?id=preloadScene)
[^note2]: [Cocos Creator 3.6 API - Director - OnLoadSceneProgress](https://docs.cocos.com/creator/3.6/api/en/namespace/Director?id=OnLoadSceneProgress)
[^note3]: [Cocos Creator 3.6 API - Director - OnSceneLoaded](https://docs.cocos.com/creator/3.6/api/zh/namespace/Director?id=OnSceneLoaded)
[^note4]: [Cocos Creator 3.6 API - Director - loadScene](https://docs.cocos.com/creator/3.6/api/en/class/Director?id=loadScene)