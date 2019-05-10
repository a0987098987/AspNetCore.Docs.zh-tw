# <a name="aspnet-core-middleware-extensibility-sample"></a>ASP.NET Core 中介軟體擴充性範例

這個範例會示範 [ASP.NET Core 的 Factory 中介軟體啟用](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility)中所述案例。

範例應用程式會示範如何依據下列條件來啟用中介軟體：

* 慣例。 如需傳統中介軟體啟用的詳細資訊，請參閱[中介軟體](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/)主題。
* [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) 實作。 預設的 [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) 類別會啟動中介軟體。

中介軟體實作運作方式都相同，並會記錄查詢字串參數 (`key`) 所提供的值。 中介軟體會使用插入的資料庫內容 (範圍服務)，以記錄記憶體中資料庫的查詢字串值。
