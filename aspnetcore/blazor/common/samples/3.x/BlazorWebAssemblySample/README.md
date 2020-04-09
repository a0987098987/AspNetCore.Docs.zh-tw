# <a name="blazor-webassembly-sample-app"></a><span data-ttu-id="a0329-101">布拉佐網路組裝範例應用程式</span><span class="sxs-lookup"><span data-stu-id="a0329-101">Blazor WebAssembly Sample App</span></span>

<span data-ttu-id="a0329-102">此示例說明瞭 Blazor 文件中描述的 Blazor 方案的使用。</span><span class="sxs-lookup"><span data-stu-id="a0329-102">This sample illustrates the use of Blazor scenarios described in the Blazor documentation.</span></span>

## <a name="call-web-api-example"></a><span data-ttu-id="a0329-103">呼叫 Web API 範例</span><span class="sxs-lookup"><span data-stu-id="a0329-103">Call web API example</span></span>

<span data-ttu-id="a0329-104">Web API 範例需要基於「<a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">建立具有 ASP.NET 核心</a>主題的 Web API 的範例應用執行 Web API",預設情況下,該主題使用與 Blazor 範例應用相同的 HTTPS 埠 (5001)。</span><span class="sxs-lookup"><span data-stu-id="a0329-104">The web API example requires a running web API based on the sample app for the <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">Create a web API with ASP.NET Core</a> topic, which by default uses the same HTTPS port (5001) as the Blazor sample app.</span></span> <span data-ttu-id="a0329-105">要同時在同一台電腦上使用這兩個應用,請使用 Web API 的連接埠(例如,使用連接埠 10000)。</span><span class="sxs-lookup"><span data-stu-id="a0329-105">To use both apps on the same machine at the same time, change the port of the web API (for example, use port 10000).</span></span> <span data-ttu-id="a0329-106">範例應用在向的`https://localhost:10000/api/TodoItems`Web API 發出請求。</span><span class="sxs-lookup"><span data-stu-id="a0329-106">The sample app makes requests to the web API at `https://localhost:10000/api/TodoItems`.</span></span> <span data-ttu-id="a0329-107">如果使用其他 Web API 位址`ServiceEndpoint`,請更新 Razor 元件`@code`區塊中的常量值。</span><span class="sxs-lookup"><span data-stu-id="a0329-107">If a different web API address is used, update the `ServiceEndpoint` constant value in the Razor component's `@code` block.</span></span></p>

<span data-ttu-id="a0329-108">範例應用從`http://localhost:5000`Web API 或`https://localhost:5001`到 Web API 發出<a href="https://docs.microsoft.com/aspnet/core/security/cors">跨源資源分享 (CORS)</a>請求。</span><span class="sxs-lookup"><span data-stu-id="a0329-108">The sample app makes a <a href="https://docs.microsoft.com/aspnet/core/security/cors">cross-origin resource sharing (CORS)</a> request from `http://localhost:5000` or `https://localhost:5001` to the web API.</span></span> <span data-ttu-id="a0329-109">允許憑據(授權 Cookie/標頭)。</span><span class="sxs-lookup"><span data-stu-id="a0329-109">Credentials (authorization cookies/headers) are permitted.</span></span> <span data-ttu-id="a0329-110">將以下 CORS 的元件設定到 Web`Startup.Configure`API 的方法:</span><span class="sxs-lookup"><span data-stu-id="a0329-110">Add the following CORS middleware configuration to the web API's `Startup.Configure` method:</span></span></p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

<span data-ttu-id="a0329-111">根據需要調整 Blazor`WithOrigins`應用的域和埠。</span><span class="sxs-lookup"><span data-stu-id="a0329-111">Adjust the domains and ports of `WithOrigins` as needed for the Blazor app.</span></span>

<span data-ttu-id="a0329-112">為 CORS 設定了 Web API 以允許授權 Cookie/ 標頭和來自用戶端代碼的請求,但本教程創建的 Web API 實際上並不授權請求。</span><span class="sxs-lookup"><span data-stu-id="a0329-112">The web API is configured for CORS to permit authorization cookies/headers and requests from client code, but the web API as created by the tutorial doesn't actually authorize requests.</span></span> <span data-ttu-id="a0329-113">有關實施指南<a href="https://docs.microsoft.com/aspnet/core/security/">,請參閱ASP.NET核心安全和標識文章</a>。</span><span class="sxs-lookup"><span data-stu-id="a0329-113">See the <a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core Security and Identity articles</a> for implementation guidance.</span></span>
