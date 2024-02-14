# Autoplay Policy

## 前言
現在使用 Facebook 網頁版來觀看影片，我們可以發現**自動播放的影片都是靜音的**，這是因為現在瀏覽器為了改善使用者體驗而有了**限制autoplay的條件**。

因為如果今天在滑網頁時，突然有一個鬼片然後播出了尖叫聲。那不知情的使用者的使用體驗一定很差。
所以要使用 Autoplay 可以，但瀏覽器規定必須要靜音，等到使用者與網頁有互動(點擊事件等等)以後才可以播放聲音。

## Chrome的自動播放規則
* 始終允許靜音影片的自動播放
* 允許自動播放有音效的情況如下：
    * 使用者與網域互動 (點擊、輕觸等)。
    * 使用者的[媒體參與度指數](https://developer.chrome.com/blog/autoplay?hl=zh-tw#media_engagement_index)門檻已門檻，代表使用者先前播放過有聲的影片。
    * 使用者已在行動裝置上將網站新增至主畫面，或是在電腦上安裝 [PWA](https://web.dev/explore/progressive-web-apps?hl=zh-tw)。
* 使用者可以針對個別網站添加例外
* 上層可以將[自動播放權限傳遞](https://developer.chrome.com/blog/autoplay?hl=zh-tw#iframe_delegation)給他們的 Iframe

## Media Engagement Index (MEI)
![](../../assets/autoplayPolicy/autoplayPolicy-1.png)
這是 Chrome 獨有的媒體參與指數，Chrome 會計算使用者對各網域的媒體互動程度進行評分。

但算分有以下的限制：

* 媒體 (音訊/影片) 的使用時間必須大於 7 秒。
* 必須顯示音訊並取消靜音。
* 顯示影片的分頁為有效狀態。
* 影片大小 (以像素為單位) 必須大於 200x140。

簡易來說**看的影片越久，網站分數就越高。而當分數超過一定的數值時，就會啟動自動撥放。**

Chrome的使用者可以訪問 **[chrome://media-engagement/](chrome://media-engagement/)** 查看自己的 MEI 