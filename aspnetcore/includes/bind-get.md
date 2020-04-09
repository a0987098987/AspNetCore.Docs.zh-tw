> [!WARNING]
> 基於安全性考量，您必須選擇將 `GET` 要求資料繫結到頁面模型屬性。 請先驗證使用者輸入再將其對應至屬性。 在處理依賴於`GET`查詢字串或路由值的方案時,選擇綁定非常有用。
>
> 要對要求`GET`結合屬性,請將[`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute)屬性的`SupportsGet`屬性`true`設定為 :
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> 有關詳細資訊,請參閱[ASP.NET核心社區站立:綁定 GET 討論 (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s)。
