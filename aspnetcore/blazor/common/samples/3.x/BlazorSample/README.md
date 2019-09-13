# <a name="blazor-webassembly-sample-app"></a>Blazor WebAssembly 範例應用程式

此範例說明如何使用 Blazor 檔中所述的 Blazor 案例。

## <a name="call-web-api-example"></a>呼叫 Web API 範例

Web API 範例需要以<a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">教學課程的範例應用程式為基礎的執行 Web API：使用 ASP.NET Core MVC</a>主題建立 Web API。 範例應用程式會對 Web API `https://localhost:10000/api/todo`提出要求。 如果使用不同的 Web API 位址，請更新 Razor `ServiceEndpoint` `@functions`元件區塊中的常數值。</p>

範例應用程式會將<a href="https://docs.microsoft.com/aspnet/core/security/cors">跨原始來源資源分享（CORS）</a>要求從`http://localhost:5000`或`https://localhost:5001` Web API。 允許認證（授權 cookie/標頭）。 在呼叫`Startup.Configure` `UseMvc`之前，將下列 CORS 中介軟體設定新增至 Web API 的方法：</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

`WithOrigins`視需要調整 Blazor 應用程式的網域和埠。

Web API 已針對 CORS 設定，以允許來自用戶端程式代碼的授權 cookie/標頭和要求，但本教學課程所建立的 Web API 實際上不會授權要求。 如需執行指引，請參閱<a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core 安全性和身分識別文章</a>。
