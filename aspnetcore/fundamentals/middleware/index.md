---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core 中介軟體

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

中介軟體為組成應用程式管線的軟體，用以處理要求與回應。 每個元件：

* 可選擇是否要將要求傳送到管線中的下一個元件。
* 可以下一個元件的前後執行工作。

要求委派用於建置要求管線， 其會處理每個 HTTP 要求。

要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。 您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。 這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。 要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。 當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。

<xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>使用 IApplicationBuilder 建立中介軟體管線

ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。 下圖說明此概念。 執行緒遵循黑色箭號執行。

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。 每個中介軟體會執行自己的邏輯，並於 next() 陳述式中將要求遞交給下一個中介軟體。 在第三個中介軟體處理要求後，要求會反向傳回經前兩個中介軟體，以在其 next() 陳述式後，至作為回應離開應用程式傳到用戶端前的期間內，進行額外處理。](index/_static/request-delegate-pipeline.png)

每一個委派皆能在下個委派的前後執行作業。 處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。

最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。 此情況不包含實際要求管線。 反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。

將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。 `next` 參數代表管線中的下個委派。 您可以「不」呼叫「下一個」參數來對管線執行最少運算。 您通常可以在下個委派的前後執行動作，如下列範例所示：

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。 因為最少運算可避免不必要的工作，所以經常使用。 例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。 在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。 不過，查看下列有關嘗試寫入已傳送之回應的警告。

> [!WARNING]
> 請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。 回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。 例如，變更設定標頭、狀態碼等都會擲回例外狀況。 若在呼叫 `next` 後寫入回應本文：
>
> * 可能導致違反通訊協定。 例如，寫入超過指定 `Content-Length` 的內容。
> * 可能損毀本文格式。 例如，將 HTML 頁尾寫入 CSS 檔。
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。

## <a name="order"></a>順序

`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。 對安全性、效能與功能性而言，此順序相當重要。

下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：

::: moniker range=">= aspnetcore-2.0"

1. 例外狀況/錯誤處理
1. HTTP 嚴格的傳輸安全性通訊協定
1. HTTPS 重新導向
1. 靜態檔案伺服器
1. Cookie 強制原則
1. 驗證
1. 工作階段
1. MVC

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. 例外狀況/錯誤處理
1. 靜態檔案
1. 驗證
1. 工作階段
1. MVC

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。 因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。

靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。 靜態檔案中介軟體**不會**執行授權檢查。 其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開使用。 如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。

::: moniker range=">= aspnetcore-2.0"

若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。 驗證不會對未經驗證的要求執行最少運算。 雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的身分識別中介軟體 (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>)。 身分識別不會對未經驗證的要求執行最少運算。 雖然身分識別會驗證要求，但只有在 MVC 選取特定控制器及動作後，才會進行驗證 (與拒絕)。

::: moniker-end

下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。 靜態檔案並不會以此中介軟體順序壓縮。 來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
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

ASP.NET Core 隨附下列中介軟體元件。 「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。 當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。 如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。

| 中介軟體 | 描述 | 順序 |
| ---------- | ----------- | ----- |
| [驗證](xref:security/authentication/identity) | 提供驗證支援。 | 在需要 `HttpContext.User` 之前。 OAuth 回呼的終端機。 |
| [Cookie 原則](xref:security/gdpr) | 追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。 | 在發出 Cookie 的中介軟體之前。 例如：驗證、工作階段、MVC (TempData)。 |
| [CORS](xref:security/cors) | 設定跨原始來源資源共用。 | 在使用 CORS 的元件之前。 |
| [診斷](xref:fundamentals/error-handling) | 設定診斷。 | 在產生錯誤的元件之前。 |
| [轉送標頭](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | 將設為 Proxy 的標頭轉送到目前要求。 | 在使用更新方法的欄位之前。 範例：配置、主機，用戶端 IP、方法。 |
| [健康狀態檢查](xref:host-and-deploy/health-checks) | 檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。 | 若某項要求與健康狀態檢查端點相符，則會是終端機。 |
| [HTTP 方法覆寫](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | 允許傳入的 POST 要求覆寫方法。 | 在使用更新方法的元件之前。 |
| [HTTPS 重新導向](xref:security/enforcing-ssl#require-https) | 將所有 HTTP 要求重新都導向至 HTTPS (ASP.NET Core 2.1 或更新版本)。 | 在使用 URL 的元件之前。 |
| [HTTP 嚴格的傳輸安全性 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | 增強安全性的中介軟體，可新增特殊的回應標頭 (ASP.NET Core 2.1 或更新版本)。 | 在傳送回應前和修改要求的元件後。 例如：轉送的標頭、URL 重寫。 |
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

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
