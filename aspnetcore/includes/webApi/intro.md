## <a name="overview"></a>概觀

本教學課程會建立以下 API：

|API | 說明 | 要求本文 | 回應本文 |
|--- | ---- | ---- | ---- |
|GET /api/todo | 取得所有待辦事項 | 無 | 待辦事項的陣列|
|GET /api/todo/{id} | 依識別碼取得項目 | 無 | 待辦事項|
|POST /api/todo | 新增記錄 | 待辦事項 | 待辦事項 |
|PUT /api/todo/{id} | 更新現有的項目 &nbsp; | 待辦事項 | 無 |
|DELETE /api/todo/{id} &nbsp; &nbsp; | 刪除項目 &nbsp; &nbsp; | 無 | 無|

<br>

下圖顯示應用程式的基本設計。

![左側方塊所代表的用戶端會送出要求並接收來自應用程式 (右側繪製的方塊) 的回應。 在應用程式方塊中，三個方塊代表控制器、模型以及資料存取層。 要求進入應用程式的控制器，而在控制器與資料存取層之間進行讀取/寫入作業。 模型會序列化並在回應中傳回至用戶端。](../../tutorials/first-web-api/_static/architecture.png)

* 會使用 Web API 者即為用戶端 (行動裝置應用程式、瀏覽器等)。 本教學課程不會建立用戶端。 將使用 [Postman](https://www.getpostman.com/) \(英文\) 或 [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html) \(英文\) 做為測試應用程式的用戶端。

* 「模型」是代表應用程式中資料的物件。 在此情況下，唯一的模型是待辦事項。 模型以 C# 類別表示，也稱純舊 C# 物件 (**P**lain **O**ld **C**# **O**bject, POCO)。

* 「控制器」是用來處理 HTTP 要求並建立 HTTP 回應的物件。 此應用程式具有單一控制器。

* 為了讓教學課程保持簡單，應用程式不會使用持續性資料庫。 範例應用程式會在記憶體內部資料庫中儲存待辦事項。
