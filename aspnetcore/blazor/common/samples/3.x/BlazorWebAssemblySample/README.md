# <a name="blazor-webassembly-sample-app"></a>布拉佐網路組裝範例應用程式

此示例說明瞭 Blazor 文件中描述的 Blazor 方案的使用。

## <a name="call-web-api-example"></a>呼叫 Web API 範例

Web API 範例需要基於「<a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">建立具有 ASP.NET 核心</a>主題的 Web API 的範例應用執行 Web API",預設情況下,該主題使用與 Blazor 範例應用相同的 HTTPS 埠 (5001)。 要同時在同一台電腦上使用這兩個應用,請使用 Web API 的連接埠(例如,使用連接埠 10000)。 範例應用在向的`https://localhost:10000/api/TodoItems`Web API 發出請求。 如果使用其他 Web API 位址`ServiceEndpoint`,請更新 Razor 元件`@code`區塊中的常量值。</p>

範例應用從`http://localhost:5000`Web API 或`https://localhost:5001`到 Web API 發出<a href="https://docs.microsoft.com/aspnet/core/security/cors">跨源資源分享 (CORS)</a>請求。 允許憑據(授權 Cookie/標頭)。 將以下 CORS 的元件設定到 Web`Startup.Configure`API 的方法:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

根據需要調整 Blazor`WithOrigins`應用的域和埠。

為 CORS 設定了 Web API 以允許授權 Cookie/ 標頭和來自用戶端代碼的請求,但本教程創建的 Web API 實際上並不授權請求。 有關實施指南<a href="https://docs.microsoft.com/aspnet/core/security/">,請參閱ASP.NET核心安全和標識文章</a>。
