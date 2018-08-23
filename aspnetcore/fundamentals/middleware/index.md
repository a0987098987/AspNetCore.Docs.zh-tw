---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870943"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core 中介軟體

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

中介軟體為組成應用程式管線的軟體，用以處理要求與回應。 每個元件：

* 可選擇是否要將要求傳送到管線中的下一個元件。
* 可以在叫用管線中下一個元件的前後執行工作。

要求委派用於建置要求管線， 其會處理每個 HTTP 要求。

要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。 您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。 這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。 要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。

<xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>使用 IApplicationBuilder 建立中介軟體管線

ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。 下圖說明此概念。 執行緒遵循黑色箭號執行。

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。 每個中介軟體會執行自己的邏輯，並於 next() 陳述式中將要求遞交給下一個中介軟體。 在第三個中介軟體處理要求後，要求會反向傳回經前兩個中介軟體，以在其 next() 陳述式後，至作為回應離開應用程式傳到用戶端前的期間內，進行額外處理。](index/_static/request-delegate-pipeline.png)

每一個委派皆能在下個委派的前後執行作業。 委派也可決定不將要求傳遞到下個委派，這就是所謂*對要求管線執行最少運算*。 因為最少運算可避免不必要的工作，所以經常使用。 例如，靜態檔案中介軟體可傳回靜態檔案的要求，並對剩餘管線執行最少運算。 處理例外狀況的委派會提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。

最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。 此情況不包含實際要求管線。 反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。

將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。 `next` 參數代表管線中的下個委派。 您可以「不」呼叫「下一個」參數來對管線執行最少運算。 您通常可以在下個委派的前後執行動作，如下列範例所示：

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> 請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。 回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。 例如，變更設定標頭、狀態碼等都會擲回例外狀況。 若在呼叫 `next` 後寫入回應本文：
>
> * 可能導致違反通訊協定。 例如，寫入超過指定 `Content-Length` 的內容。
> * 可能損毀本文格式。 例如，將 HTML 頁尾寫入 CSS 檔。
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。

## <a name="order"></a>訂單

`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。 對安全性、效能與功能性而言，此順序相當重要。

下列 `Configure` 方法會新增下列中介軟體元件：

1. 例外狀況/錯誤處理
2. 靜態檔案伺服器
3. 驗證
4. MVC

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

在上述程式碼中，<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增至管線的中介軟體元件。 因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。

系統會提早在管線中呼叫靜態檔案中介軟體，以便該軟體可處理要求並執行最少運算，而無需一一處理剩餘的元件。 靜態檔案中介軟體**不**提供授權檢查。 其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開使用。 如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。

::: moniker range=">= aspnetcore-2.0"

若要求未經靜態檔案中介軟體處理，則會將其傳遞至驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)，以執行驗證。 驗證不會對未經驗證的要求執行最少運算。 雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要求未經靜態檔案中介軟體處理，則會將其傳遞至身分識別中介軟體 (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>)，以執行驗證。 身分識別不會對未經驗證的要求執行最少運算。 雖然身分識別會驗證要求，但只有在 MVC 選取特定控制器及動作後，才會進行驗證 (與拒絕)。

::: moniker-end

下列範例會示範中介軟體的順序，其中靜態檔案中介軟體會比回應壓縮中介軟體先處理靜態檔案的要求。 靜態檔案並不會以此中介軟體順序壓縮。 來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Use、Run 與 Map

設定 HTTP 管線的方法是使用 `Use`、`Run` 及 `Map`。 `Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。 `Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。 `Map*` 會依據指定要求路徑的相符項目將要求管線分支。 如果要求路徑以指定路徑為開頭，則會執行分支。

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。

| 要求             | 回應                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) 會依據指定述詞的結果將要求管線分支。 `Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。 下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。

| 要求                       | 回應                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Hello from non-Map delegate. |
| localhost:1234/?branch=master | Branch used = master         |

`Map` 支援巢狀項目，例如：

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` 也可以一次比對多個線段：

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>內建的中介軟體

ASP.NET Core 隨附下列中介軟體元件。 「順序」欄位說明中介軟體在要求管線中的位置，以及中介軟體可終止要求並防止其他中介軟體處理要求的情況。

| 中介軟體 | 描述 | 訂單 |
| ---------- | ----------- | ----- |
| [驗證](xref:security/authentication/identity) | 提供驗證支援。 | 在需要 `HttpContext.User` 之前。 OAuth 回呼的終端機。 |
| [CORS](xref:security/cors) | 設定跨原始來源資源共用。 | 在使用 CORS 的元件之前。 |
| [診斷](xref:fundamentals/error-handling) | 設定診斷。 | 在產生錯誤的元件之前。 |
| [轉送標頭](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | 將設為 Proxy 的標頭轉送到目前要求。 | 在使用更新欄位的元件之前 (例如：配置、主機、用戶端 IP、方法)。 |
| [HTTP 方法覆寫](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | 允許傳入的 POST 要求覆寫方法。 | 在使用更新方法的元件之前。 |
| [HTTPS 重新導向](xref:security/enforcing-ssl#require-https) | 將所有 HTTP 要求重新都導向至 HTTPS (ASP.NET Core 2.1 或更新版本)。 | 在使用 URL 的元件之前。 |
| [HTTP 嚴格的傳輸安全性 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | 增強安全性的中介軟體，可新增特殊的回應標頭 (ASP.NET Core 2.1 或更新版本)。 | 在傳送回應前和修改要求的元件後 (例如，轉送標頭、URL 重寫)。 |
| [MVC](xref:mvc/overview) | 使用 MVC/Razor 頁面處理要求 (ASP.NET Core 2.0 或更新版本)。 | 若要求符合路由則終止。 |
| [OWIN](xref:fundamentals/owin) | 以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。 | 若 OWIN 中介軟體完全處理要求則終止。 |
| [回應快取](xref:performance/caching/middleware) | 提供快取回應的支援。 | 在需要快取的元件之前。 |
| [回應壓縮](xref:performance/response-compression) | 提供壓縮回應的支援。 | 在需要壓縮的元件之前。 |
| [要求當地語系化](xref:fundamentals/localization) | 提供當地語系化支援。 | 在偵測當地語系化的元件之前。 |
| [路由傳送](xref:fundamentals/routing) | 定義並限制要求路由。 | 比對路由的終端機。 |
| [工作階段](xref:fundamentals/app-state) | 提供管理使用者工作階段的支援。 | 在需要工作階段的元件之前。 |
| [靜態檔案](xref:fundamentals/static-files) | 支援靜態檔案的提供和目錄瀏覽。 | 若要求符合檔案則終止。 |
| [URL 重寫](xref:fundamentals/url-rewriting) | 提供重寫 URL 及重新導向要求的支援。 | 在使用 URL 的元件之前。 |
| [WebSockets](xref:fundamentals/websockets) | 啟用 WebSockets 通訊協定。 | 在接受 WebSocket 要求的必要元件之前。 |

## <a name="write-middleware"></a>撰寫中介軟體

中介軟體通常封裝在類別中，並以擴充方法公開。 請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

上述範例程式碼用於示範中介軟體元件的建立。 如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。

您可藉由傳遞文化特性來測試中介軟體，例如 `http://localhost:7997/?culture=no`。

下列程式碼會將中介軟體委派移至類別：

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

中介軟體 `Task` 方法的名稱必須是 `Invoke`。 在 ASP.NET Core 2.0 或更新版本中，此名稱可以是 `Invoke` 或 `InvokeAsync`。

::: moniker-end

下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

下列程式碼會從 `Startup.Configure` 呼叫中介軟體：

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。 中介軟體會在每次「應用程式存留期」就建構一次。 若您需要在要求內與中介軟體共用服務，請參閱[依要求的相依性](#per-request-dependencies)一節。

中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。 [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。

### <a name="per-request-dependencies"></a>依要求的相依性

因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」存留期服務不會與其它插入相依性的類型共用。 如果您必須在中介軟體和其他類型間共用「已限定範圍」的服務，請將這些服務新增至 `Invoke` 方法的簽章。 `Invoke` 方法可以接受 DI 所填入的其他參數：

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>其他資源

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
