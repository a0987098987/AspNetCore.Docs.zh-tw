# <a name="blazor-webassembly-sample-app"></a>Blazor WebAssembly 範例應用程式

此範例說明如何使用 Blazor 檔中所述的 Blazor 案例。

## <a name="call-web-api-example"></a>呼叫 Web API 範例

Web API 範例需要根據使用<a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">ASP.NET Core 建立 Web API</a>主題的範例應用程式執行 Web API，其預設會使用與 Blazor 範例應用程式相同的 HTTPS 埠（5001）。 若要同時在同一部電腦上使用這兩個應用程式，請變更 Web API 的埠（例如，使用埠10000）。 範例應用程式會對 Web API 提出 `https://localhost:10000/api/TodoItems` 的要求。 如果使用不同的 Web API 位址，請更新 Razor 元件 `@code` 區塊中的 `ServiceEndpoint` 常數值。</p>

範例應用程式會將<a href="https://docs.microsoft.com/aspnet/core/security/cors">跨原始來源資源分享（CORS）</a>要求從 `http://localhost:5000` 或 `https://localhost:5001` 至 Web API。 允許認證（授權 cookie/標頭）。 將下列 CORS 中介軟體設定新增至 Web API 的 `Startup.Configure` 方法：</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

視需要調整 Blazor 應用程式的網域和埠 `WithOrigins`。

Web API 已針對 CORS 設定，以允許來自用戶端程式代碼的授權 cookie/標頭和要求，但本教學課程所建立的 Web API 實際上不會授權要求。 如需執行指引，請參閱<a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core 安全性和身分識別文章</a>。
