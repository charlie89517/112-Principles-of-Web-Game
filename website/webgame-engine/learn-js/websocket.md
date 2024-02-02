# Websocket介紹

## 介紹
WebSocket 是一種在 Web 應用程式中實現雙向通訊的協議。

傳統的 HTTP 協議是一種無狀態的協議，每次請求都需要重新建立連線，並且由伺服器主動回應。
然而，WebSocket 協議允許伺服器和客戶端之間**建立持久的連線**，以實現即時的、雙向的通訊。

## 範例
```js
// 創建一個新的WebSocket物件
var socket = new WebSocket('ws://example.com/websocket');

// WebSocket打開連接後的處理
socket.onopen = function(event) {
  console.log('連接已打開');

  // 可以發送訊息給伺服器
  socket.send('你好，伺服器！');
};

// 接收來自伺服器的訊息
socket.onmessage = function(event) {
  console.log('收到訊息：' + event.data);
  // 處理接收到的訊息
};

// 處理錯誤
socket.onerror = function(error) {
  console.log('WebSocket發生錯誤：' + error.message);
};

// WebSocket關閉連接後的處理
socket.onclose = function(event) {
  console.log('連接已關閉');
};

// 發送訊息到伺服器
function sendMessage(message) {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(message);
  } else {
    console.log('WebSocket還沒有準備好發送資料');
  }
}

// 關閉WebSocket連接
function closeConnection() {
  socket.close();
}

```
## reference
1. https://forum.cocos.org/t/topic/84649

2. https://developer.mozilla.org/zh-TW/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications