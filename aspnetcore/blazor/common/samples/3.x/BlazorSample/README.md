# <a name="blazor-client-side-sample-app"></a><span data-ttu-id="5ae64-101">Blazor (用戶端) 範例應用程式</span><span class="sxs-lookup"><span data-stu-id="5ae64-101">Blazor (client-side) Sample App</span></span>

<span data-ttu-id="5ae64-102">此範例說明如何使用 Blazor 檔中所述的 Blazor 案例。</span><span class="sxs-lookup"><span data-stu-id="5ae64-102">This sample illustrates the use of Blazor scenarios described in the Blazor documentation.</span></span>

## <a name="call-web-api-example"></a><span data-ttu-id="5ae64-103">呼叫 Web API 範例</span><span class="sxs-lookup"><span data-stu-id="5ae64-103">Call web API example</span></span>

<span data-ttu-id="5ae64-104">Web API 範例需要以<a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">教學課程的範例應用程式為基礎的執行 Web API:使用 ASP.NET Core MVC</a>主題建立 Web API。</span><span class="sxs-lookup"><span data-stu-id="5ae64-104">The web API example requires a running web API based on the sample app for the <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">Tutorial: Create a web API with ASP.NET Core MVC</a> topic.</span></span> <span data-ttu-id="5ae64-105">範例應用程式會對 Web API `https://localhost:10000/api/todo`提出要求。</span><span class="sxs-lookup"><span data-stu-id="5ae64-105">The sample app makes requests to the web API at `https://localhost:10000/api/todo`.</span></span> <span data-ttu-id="5ae64-106">如果使用不同的 Web API 位址, 請更新 Razor `ServiceEndpoint` `@functions`元件區塊中的常數值。</span><span class="sxs-lookup"><span data-stu-id="5ae64-106">If a different web API address is used, update the `ServiceEndpoint` constant value in the Razor component's `@functions` block.</span></span></p>

<span data-ttu-id="5ae64-107">範例應用程式會將<a href="https://docs.microsoft.com/aspnet/core/security/cors">跨原始來源資源分享 (CORS)</a>要求從`http://localhost:5000`或`https://localhost:5001` Web API。</span><span class="sxs-lookup"><span data-stu-id="5ae64-107">The sample app makes a <a href="https://docs.microsoft.com/aspnet/core/security/cors">cross-origin resource sharing (CORS)</a> request from `http://localhost:5000` or `https://localhost:5001` to the web API.</span></span> <span data-ttu-id="5ae64-108">允許認證 (授權 cookie/標頭)。</span><span class="sxs-lookup"><span data-stu-id="5ae64-108">Credentials (authorization cookies/headers) are permitted.</span></span> <span data-ttu-id="5ae64-109">在呼叫`Startup.Configure` `UseMvc`之前, 將下列 CORS 中介軟體設定新增至 Web API 的方法:</span><span class="sxs-lookup"><span data-stu-id="5ae64-109">Add the following CORS middleware configuration to the web API's `Startup.Configure` method before it calls `UseMvc`:</span></span></p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

<span data-ttu-id="5ae64-110">`WithOrigins`視需要調整 Blazor 應用程式的網域和埠。</span><span class="sxs-lookup"><span data-stu-id="5ae64-110">Adjust the domains and ports of `WithOrigins` as needed for the Blazor app.</span></span>

<span data-ttu-id="5ae64-111">Web API 已針對 CORS 設定, 以允許來自用戶端程式代碼的授權 cookie/標頭和要求, 但本教學課程所建立的 Web API 實際上不會授權要求。</span><span class="sxs-lookup"><span data-stu-id="5ae64-111">The web API is configured for CORS to permit authorization cookies/headers and requests from client code, but the web API as created by the tutorial doesn't actually authorize requests.</span></span> <span data-ttu-id="5ae64-112">如需執行指引, 請參閱<a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core 安全性和身分識別文章</a>。</span><span class="sxs-lookup"><span data-stu-id="5ae64-112">See the <a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core Security and Identity articles</a> for implementation guidance.</span></span>
