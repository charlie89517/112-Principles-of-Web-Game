# Websocket 介紹

## 介紹

WebSocket 是一種在 Web 應用程式中實現雙向通訊的協議。

傳統的 HTTP 協議是一種**無狀態**的協議，每次請求都需要重新建立連線，並且由伺服器主動回應。

WebSocket 允許伺服器和客戶端之間**建立持久的連線**，以實現即時的、雙向的通訊。

- Client 不用一直發 request
- Server 可以主動傳給 Client

適用需要持續連線的場景: 聊天室、網路遊戲

## 創建一個新的 WebSocket 物件

```js
let socket = new WebSocket("ws://example.com/WebSocket");
```

- ws://example.com

- wss://example.com (有 SSL 加密的 WebSocket)

- 若網站用了 http 則不能使用 wss(安全性層級不同)

## WebSocket 打開連接後的處理

```js
socket.onopen = function (event) {
  console.log("連接已打開");

  // 可以發送訊息給伺服器
  socket.send("你好，伺服器！");
};
```

## 接收來自伺服器的訊息

```js
socket.onmessage = function (event) {
  console.log("收到訊息：" + event.data);
  // 處理接收到的訊息
};
```

## 處理錯誤

```js
socket.onerror = function (error) {
  console.log("WebSocket發生錯誤：" + error.message);
};
```

## WebSocket 關閉連接後的處理

```js
socket.onclose = function (event) {
  console.log("連接已關閉");
};
```

## 發送訊息到伺服器

```js
function sendMessage(message) {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(message);
  } else {
    console.log("WebSocket還沒有準備好發送資料");
  }
}
```

## WebSocket Enum Value

| Enum Value |   State    |          Description          |
| :--------: | :--------: | :---------------------------: |
|     0      | CONNECTING | Socket 已創建，連線尚未開啟。   |
|     1      |    OPEN    | 連線已開啟並準備好進行通訊。    |
|     2      |  CLOSING   |    連線正在關閉的過程中。       |
|     3      |   CLOSED   |    連線已關閉或無法開啟。       |

## 關閉 WebSocket 連接

```js
function closeConnection() {
  socket.close();
}
```

## reference

1. [https://developer.mozilla.org/zh-TW/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications](https://developer.mozilla.org/zh-TW/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications)
2. [https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/readyState](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/readyState)
