# Websocket介紹

## 介紹
WebSocket 是一種在 Web 應用程式中實現雙向通訊的協議。

傳統的 HTTP 協議是一種無狀態的協議，每次請求都需要重新建立連線，並且由伺服器主動回應。

允許伺服器和客戶端之間**建立持久的連線**，以實現即時的、雙向的通訊。

* Client 不用一直發 request
* Server 可以主動傳給 Client

## 創建一個新的WebSocket物件
```js
var socket = new WebSocket('ws://example.com/websocket');
```

* ws://example.com 

* wss://example.com (有SSL加密的websocket)

## WebSocket打開連接後的處理
```js
socket.onopen = function(event) {
  console.log('連接已打開');

  // 可以發送訊息給伺服器
  socket.send('你好，伺服器！');
};
```
## 接收來自伺服器的訊息
```js
socket.onmessage = function(event) {
  console.log('收到訊息：' + event.data);
  // 處理接收到的訊息
};
```
## 處理錯誤
```js
socket.onerror = function(error) {
  console.log('WebSocket發生錯誤：' + error.message);
};
```
## WebSocket關閉連接後的處理
```js
socket.onclose = function(event) {
  console.log('連接已關閉');
};
```
## 發送訊息到伺服器
```js
function sendMessage(message) {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(message);
  } else {
    console.log('WebSocket還沒有準備好發送資料');
  }
}
```

## Websocket readyState Value
|  Value   | State  | Description |
|  :----:  | :----:  | :----: |
|0|CONNECTING|Socket has been created. The connection is not yet open.|
|1|OPEN|The connection is open and ready to communicate.|
|2|CLOSING|The connection is in the process of closing.|
|3|CLOSED|The connection is closed or couldn't be opened.|

## 關閉WebSocket連接
```js
function closeConnection() {
  socket.close();
}
```



## reference
1. [https://forum.cocos.org/t/topic/84649](https://forum.cocos.org/t/topic/84649)
2. [https://developer.mozilla.org/zh-TW/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications](https://developer.mozilla.org/zh-TW/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications)
3. [https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/readyState](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket/readyState)
4. [https://www.letswrite.tw/websocket/](https://www.letswrite.tw/websocket/)