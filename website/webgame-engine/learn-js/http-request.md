# HTTP request

## 介紹

HTTP 全名為 Hyper Text Transfer Protocol 超文字傳輸協定。在 OSI 模型裡，它屬於應用層的協定，可以透過 TCP 或 TLS 來發送或接收資訊。

## 流程

- Client 向 Server 請求

```mermaid
graph LR
  A[Client] -->|request| B[Server];
```

- 請求格式
  [^1]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview

 ![請求格式](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_request.png)

- Server 向 Client 回應

```mermaid
graph LR
  A[Client];B[Server];
  B -->|response| A

```

* 回應格式[^1]

![回傳格式](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http_response.png)

## URL 格式

```
  abc://username:password@example.com:123/path/data?key=value&key2=value2#fragid1
  └┬┘   └───────┬───────┘ └────┬────┘ └┬┘           └─────────┬─────────┘ └──┬──┘
scheme  user information     host     port                  query         fragment
```

在該例子中：

- abc 是協定名稱
- username 是用戶
- password 是密碼
- example.com 是網域
- 123 是連線的 port
- /path/data 是路徑
- ?key=value&key2=vale2 是查詢字串
- fragid1 是 fragment

## Http Code 狀態碼

- HTTP Status Code 1xx 訊息

這一類型的狀態碼，代表請求已被接受，需要繼續處理。這類回應是臨時回應，只包含狀態行和某些可選的回應頭資訊，並以空行結束。

- HTTP Status Code 2xx 成功

請求已成功被伺服器接收、理解、並接受。

`200 OK`：成功

`204 No Content`：成功，但沒有回傳的內容

- HTTP Status Code 3xx 重新導向

通常這些狀態碼用來重新導向。

`301 Moved Permanently`：資源**永久**移到其他位置

`302 Found (Moved Temporarily)`：資源**暫時**移到其他位置

`304 Not Modified`：從快取拿東西

- HTTP Status Code 4xx 客戶端錯誤

`400 Bad Request`：請求語法錯誤…等等

`401 Unauthorized`：未認證

`403 Forbidden`：禁止存取，可能是沒有權限

`404 Not Found`：找不到資源

- HTTP Status Code 5xx 伺服器錯誤

`500 Internal Server Error`：伺服器出錯

`502 Bad Gateway`：服務沒有正確執行

## RESTful API

Restful API（Representational State Transfer）是一種基於 HTTP 協議的**設計風格**。

使用標準的 HTTP 動詞（GET、POST、PUT、DELETE 等）來操作。
例如，使用 GET 來獲取資料，使用 POST 來新增，使用 PUT 來更新，使用 DELETE 來刪除，這些對應於 CURD 操作。

- GET (取得)
- POST (新增)
- PUT (修改)
- DELETE (刪除)

[其他HTTP動詞 https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Methods](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Methods)

!!! note
    不建議在 GET 中使用登入帳號密碼，
    因為在 GET 請求中，URL 參數是以明文形式傳輸的，在未加密的情況下帳號密碼可以在 URL 被看見。
    
    例如: /login?account="admin"&password="123"

    
## 與一般的 API 的差異

|            |      一般的 API      |   RESTful API    |
| :--------: | :------------------: | :--------------: |
| 創建使用者 | `POST` `/createUser` |  `POST` `/user`  |
| 更新使用者 | `POST` `/updateUser` |  `PUT` `/user/{uid}`   |
| 取得使用者 |  `POST` `/getUser`   |  `GET` `/user/{uid}`   |
| 刪除使用者 | `POST` `/deleteUser` | `DELETE` `/user/{uid}` |

## RESTful API 有哪些優勢？

[^3]: https://aws.amazon.com/tw/what-is/restful-api/

- 可擴展性[^3]

伺服器不必保留過去的用戶端請求資訊，透過無狀態和有效的快取機制來減少伺服器負擔，提升系統的擴展能力且減少效能瓶頸。

- 靈活性

RESTful Web 服務允許用戶端和伺服器分離，各部件可以獨立演進，使得平台或技術變更不會影響到對方，並使應用程式功能更加分層和靈活。
例如，開發人員可以在不重寫應用程式邏輯的情況下，對資料庫層進行變更。

- 獨立性

您可以使用各種程式設計語言來編寫用戶端和伺服器應用程式，而不會影響 API 設計。

[^2]: https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API/Using_Fetch

## POST Request

```js
postData("http://example.com/answer", { answer: 42 })
  .then((data) => console.log(data))
  .catch((error) => console.error(error));

function postData(url, data) {
  // 預設值標記為*
  return fetch(url, {
    body: JSON.stringify(data), // must match 'Content-Type' header
    cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
    credentials: "same-origin", // include, same-origin, *omit
    headers: {
      "user-agent": "Mozilla/4.0 MDN Example",
      "content-type": "application/json",
    },
    method: "POST", // *GET, POST, PUT, DELETE, etc.
    mode: "cors", // no-cors, cors, *same-origin
    redirect: "follow", // manual, *follow, error
    referrer: "no-referrer", // *client, no-referrer
  }).then((response) => response.json());
}
```

範例程式碼[^2]中有幾個比較重要的參數

1. url 是 API 的網址
2. data 把OBJ轉成JSON字串作為Body提供給Server
3. headers 可以放入附加訊息，通常放入 User-Agent Content-Type 等... [Header參數](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)
4. method 放入HTTP動詞

## 氣象資料開放平台 API 實作
1. 到[氣象資料開放平台](https://opendata.cwa.gov.tw/index)註冊帳號
![](/webgame-engine/assets/HTTP-request/openWeatherData.png)

2. 註冊一個帳號 [https://pweb.cwa.gov.tw/emember/register](https://pweb.cwa.gov.tw/emember/register)
![](/webgame-engine/assets/HTTP-request/openWeatherDataRegister.png)

3. 到電子信箱啟用會員後再次[登入](https://opendata.cwa.gov.tw/userLogin)
![](/webgame-engine/assets/HTTP-request/emailConfirm.png)

4. 登入後取得授權碼並將授權碼複製下來
![](/webgame-engine/assets/HTTP-request/getToken.png)

5. 前往開發指南查看[api文件](https://opendata.cwa.gov.tw/dist/opendata-swagger.html)
![](/webgame-engine/assets/HTTP-request/apiDoc.png)

6. 可以在swagger文檔中使用API request
![](/webgame-engine/assets/HTTP-request/trySwagger1.png)

7. 填入需要的參數後點擊Execute執行
![](/webgame-engine/assets/HTTP-request/trySwagger2.png)

8. 執行後的回傳結果
![](/webgame-engine/assets/HTTP-request/result.png)


## 執行程式碼
* BaseUrl
![](/webgame-engine/assets/HTTP-request/baseUrl.png)

```
  https://opendata.cwa.gov.tw/api/v1/rest/datastore/F-D0047-063?Authorization=XXX-XXX-XXX
   └┬┘   └────────┬────────┘ └┬┘    └────────────┬───────────┘  └──────────┬───────────┘
Scheme       API domain    BaseUrl             Path                     Query Argument
```

* BaseUrl
![](/webgame-engine/assets/HTTP-request/baseUrl.png)

* API Path
![](/webgame-engine/assets/HTTP-request/apiPath.png)


```js
const baseUrl = "https://opendata.cwa.gov.tw/api/v1/"
const path = "rest/datastore/F-D0047-063"
const Authorization = "yourAuthorization"
fetch( `${baseUrl}${path}?Authorization=${Authorization}`)
  .then(function (response) {
    return response.json();
  })
  .then(function (myJson) {
    console.log(myJson);
    // 輸出json資料
  });
```

* 結果輸出
![](/webgame-engine/assets/HTTP-request/consoleTest.png)

!!! info 
    如果你的網站使用 HTTPS (加密) 則不能請求 HTTP (未加密)的網站，為了確保整個網站的安全性，瀏覽器禁止在使用HTTPS的網站中請求HTTP。
    > 可以在HTTPS存取HTTP圖片、影片資源

## CORS (Cross-Origin Resource Sharing)

CORS是一種瀏覽器**安全機制**，當在網頁中使用 JavaScript 發起跨域請求時，瀏覽器會執行同源策略，阻止該請求，以保護使用者的安全。

### 同源策略 (Same-Origin Policy)
同源是指**相同域名(domain)、協議(protocol)跟端口(port)**，
在同源策略下，僅允許網頁從與它自己相同的原始來源來載入資源。
在某些情況下，網頁需要與其他來源的資源，例如使用不同網域的 API。

在伺服器端可以設定 CORS 的限制，確保僅允許來自特定網域的請求或限制允許的 HTTP 方法和標頭。
這有助於防止跨站腳本攻擊(XSS)和跨站點請求偽造(CSRF)等安全風險。

* 出現CORS的錯誤[^4]
[^4]:https://miro.medium.com/v2/resize:fit:1400/format:webp/0*bI2yxKryqJzyUkud

![出現CORS的錯誤](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*bI2yxKryqJzyUkud)