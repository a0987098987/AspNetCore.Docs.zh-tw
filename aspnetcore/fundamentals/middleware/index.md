---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/6/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/middleware/index
ms.openlocfilehash: 69c253171c51e08802b82415245a66921168ec80
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404258"
---
# <a name="aspnet-core-middleware"></a>ASP.NET Core 中介軟體

::: moniker range=">= aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

中介軟體為組成應用程式管線的軟體，用以處理要求與回應。 每個元件：

* 可選擇是否要將要求傳送到管線中的下一個元件。
* 可以下一個元件的前後執行工作。

要求委派用於建置要求管線， 其會處理每個 HTTP 要求。

要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。 您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。 這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。 要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。 當中介軟體短路時，稱為「終端中介軟體」**，因為它會防止接下來的中介軟體處理要求。

<xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>使用 IApplicationBuilder 建立中介軟體管線

ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。 下圖說明此概念。 執行緒遵循黑色箭號執行。

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。 每個中介軟體會執行其邏輯，並於 next() 陳述式中將要求遞交至下個中介軟體。 在第三個中介軟體處理要求後，要求會反向傳回經前兩個中介軟體，以在其 next() 陳述式後，至作為回應離開應用程式傳到用戶端前的期間內，進行額外處理。](index/_static/request-delegate-pipeline.png)

每一個委派皆能在下個委派的前後執行作業。 處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。

最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。 此情況不包含實際要求管線。 反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。 `next` 參數代表管線中的下個委派。 您可以「不」** 呼叫「下一個」** 參數來對管線執行最少運算。 您通常可以在下個委派的前後執行動作，如下列範例所示：

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。 因為最少運算可避免不必要的工作，所以經常使用。 例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。 在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。 不過，查看下列有關嘗試寫入已傳送之回應的警告。

> [!WARNING]
> 請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。 回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。 例如，變更設定標頭、狀態碼等都會擲回例外狀況。 若在呼叫 `next` 後寫入回應本文：
>
> * 可能導致違反通訊協定。 例如，寫入超過指定 `Content-Length` 的內容。
> * 可能損毀本文格式。 例如，將 HTML 頁尾寫入 CSS 檔。
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。

<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>委派不會收到 `next` 參數。 第一個 `Run` 委派一律是 terminal，並終止管線。 `Run`是慣例。 某些中介軟體元件可能會公開在 `Run[Middleware]` 管線結束時執行的方法：

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

在上述範例中， `Run` 委派會寫入 `"Hello from 2nd delegate."` 至回應，然後終止管線。 如果 `Use` `Run` 在委派之後加入另一個或委派 `Run` ，則不會呼叫它。

<a name="order"></a>

## <a name="middleware-order"></a>中介軟體順序

下圖顯示 ASP.NET Core MVC 和頁面應用程式的完整要求處理管線 Razor 。 您可以查看在一般應用程式中，如何排序現有的中介軟體，以及新增自訂中介軟體的位置。 您可以完整控制如何重新排序現有的中介軟體，或在您的案例中插入新的自訂中介軟體。

![ASP.NET Core 中介軟體管線](index/_static/middleware-pipeline.svg)

上圖中的**端點**中介軟體會執行對應應用程式類型 &mdash; MVC 或頁面的篩選準則管線 Razor 。

![ASP.NET Core 篩選器管線](index/_static/mvc-endpoint.svg)

`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。 順序對於安全性、效能和功能非常**重要**。

下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

在上述程式碼中：

* 使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。
* 並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。 例如：
  * `UseCors`、 `UseAuthentication` 和 `UseAuthorization` 必須以顯示的循序執行。
  * `UseCors`目前必須在 `UseResponseCaching` [此 bug](https://github.com/dotnet/aspnetcore/issues/23218)之前進行。

下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：

1. 例外狀況/錯誤處理
   * 當應用程式在開發環境中執行時：
     * 開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。
     * 資料庫錯誤頁面中介軟體會報告資料庫執行階段錯誤。
   * 當應用程式在生產環境中執行時：
     * 例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。
     * HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。
1. HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。
1. 靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。
1. Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。
1. 路由中介軟體（ `UseRouting` ）以路由傳送要求。
1. 驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。
1. 授權中介軟體（ `UseAuthorization` ）會授權使用者存取安全的資源。
1. 工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。 若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。
1. 端點路由中介軟體（ `UseEndpoints` 含 `MapRazorPages` ），以將 Razor 頁面端點新增至要求管線。

<!--

FUTURE UPDATE

On the next topic overhaul/release update, add API crosslink to "Database Error Page Middleware" in Item 1 of the list ...

Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*

... when available via the API docs.

-->

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseRouting();
    app.UseAuthentication();
    app.UseAuthorization();
    app.UseSession();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。 因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。

靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。 靜態檔案中介軟體**不**提供授權檢查。 靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。 如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。

若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。 驗證不會對未經驗證的要求執行最少運算。 雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器和動作之後，才會進行授權（和拒絕）。

下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。 靜態檔案並不會以此中介軟體順序壓縮。 Razor頁面回應可以壓縮。

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

對於單一頁面應用程式（Spa），SPA 中介軟體 <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> 通常是在中介軟體管線中的最後一個。 SPA 中介軟體最後一步：

* 以允許所有其他中介軟體先回應相符的要求。
* 允許 Spa 與用戶端路由針對伺服器應用程式無法辨識的所有路由執行。

如需 Spa 的詳細資訊，請參閱[回應](xref:spa/react)和[角度](xref:spa/angular)專案範本的指南。

### <a name="forwarded-headers-middleware-order"></a>轉送的標頭中介軟體順序

[!INCLUDE[](~/includes/ForwardedHeaders.md)]

## <a name="branch-the-middleware-pipeline"></a>分支中介軟體管線

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。 `Map` 會依據指定要求路徑的相符項目將要求管線分支。 如果要求路徑以指定路徑為開頭，則會執行分支。

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。

| 要求             | 回應                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。

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

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。 `Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。 下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：

| 要求                       | 回應                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Hello from non-Map delegate. |
| localhost:1234/?branch=master | Branch used = master         |

<xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*>也會根據給定述詞的結果來分支要求管線。 與不同的 `MapWhen` 是，這個分支會重新加入主要管線（如果它不是短路或包含終端機中介軟體）：

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=25-26)]

在上述範例中，回應為「來自主要管線的 Hello」。 會針對所有要求寫入。 如果要求包含查詢字串變數，則 `branch` 會在重新加入主要管線之前記錄其值。

## <a name="built-in-middleware"></a>內建的中介軟體

ASP.NET Core 隨附下列中介軟體元件。 「順序」** 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。 當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」**。 如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。

| 中介軟體 | 說明 | 單 |
| ---------- | ----------- | ----- |
| [驗證](xref:security/authentication/identity) | 提供驗證支援。 | 在需要 `HttpContext.User` 之前。 OAuth 回呼的終端機。 |
| [授權](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | 提供授權支援。 | 緊接在驗證中介軟體之後。 |
| [Cookie 原則](xref:security/gdpr) | 追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。 | 在發出 Cookie 的中介軟體之前。 範例：驗證、工作階段、MVC (TempData)。 |
| [CORS](xref:security/cors) | 設定跨原始來源資源共用。 | 在使用 CORS 的元件之前。 `UseCors`目前必須在 `UseResponseCaching` [此 bug](https://github.com/dotnet/aspnetcore/issues/23218)之前進行。|
| [診斷](xref:fundamentals/error-handling) | 提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。 | 在產生錯誤的元件之前。 終端機的例外狀況，或為新的應用程式提供預設的網頁。 |
| [轉送的標頭](xref:host-and-deploy/proxy-load-balancer) | 將設為 Proxy 的標頭轉送到目前要求。 | 在使用更新方法的欄位之前。 範例：配置、主機，用戶端 IP、方法。 |
| [健康情況檢查](xref:host-and-deploy/health-checks) | 檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。 | 若某項要求與健康狀態檢查端點相符，則會是終端機。 |
| [標頭傳播](xref:fundamentals/http-requests#header-propagation-middleware) | 將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。 |
| [HTTP 方法覆寫](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | 允許傳入的 POST 要求覆寫方法。 | 在使用更新方法的元件之前。 |
| [HTTPS 重新導向](xref:security/enforcing-ssl#require-https) | 將所有 HTTP 要求都重新導向至 HTTPS。 | 在使用 URL 的元件之前。 |
| [HTTP 嚴格的傳輸安全性 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | 增強安全性的中介軟體，可新增特殊的回應標頭。 | 在傳送回應前和修改要求的元件後。 範例：轉送的標頭、URL 重寫。 |
| [MVC](xref:mvc/overview) | 處理 MVC/Pages 的要求 Razor 。 | 若要求符合路由則終止。 |
| [OWIN](xref:fundamentals/owin) | 以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。 | 若 OWIN 中介軟體完全處理要求則終止。 |
| [回應快取](xref:performance/caching/middleware) | 提供快取回應的支援。 | 在需要快取的元件之前。 `UseCORS`必須早于 `UseResponseCaching` 。|
| [回應壓縮](xref:performance/response-compression) | 提供壓縮回應的支援。 | 在需要壓縮的元件之前。 |
| [要求當地語系化](xref:fundamentals/localization) | 提供當地語系化支援。 | 在偵測當地語系化的元件之前。 |
| [端點路由](xref:fundamentals/routing) | 定義並限制要求路由。 | 比對路由的終端機。 |
| [SPA](xref:Microsoft.AspNetCore.Builder.SpaApplicationBuilderExtensions.UseSpa*) | 藉由傳回單一頁面應用程式（SPA）的預設頁面，處理來自中介軟體鏈中此點的所有要求 | 在鏈晚期，讓提供靜態檔案、MVC 動作等的其他中介軟體優先。|
| [本次](xref:fundamentals/app-state) | 提供管理使用者工作階段的支援。 | 在需要工作階段的元件之前。 | 
| [靜態檔案](xref:fundamentals/static-files) | 支援靜態檔案的提供和目錄瀏覽。 | 若要求符合檔案則終止。 |
| [URL 重寫](xref:fundamentals/url-rewriting) | 提供重寫 URL 及重新導向要求的支援。 | 在使用 URL 的元件之前。 |
| [WebSocket](xref:fundamentals/websockets) | 啟用 WebSockets 通訊協定。 | 在接受 WebSocket 要求的必要元件之前。 |

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/write>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫

中介軟體為組成應用程式管線的軟體，用以處理要求與回應。 每個元件：

* 可選擇是否要將要求傳送到管線中的下一個元件。
* 可以下一個元件的前後執行工作。

要求委派用於建置要求管線， 其會處理每個 HTTP 要求。

要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。 您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。 這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。 要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。 當中介軟體短路時，稱為「終端中介軟體」**，因為它會防止接下來的中介軟體處理要求。

<xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>使用 IApplicationBuilder 建立中介軟體管線

ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。 下圖說明此概念。 執行緒遵循黑色箭號執行。

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。 每個中介軟體會執行其邏輯，並於 next() 陳述式中將要求遞交至下個中介軟體。 在第三個中介軟體處理要求後，要求會反向傳回經前兩個中介軟體，以在其 next() 陳述式後，至作為回應離開應用程式傳到用戶端前的期間內，進行額外處理。](index/_static/request-delegate-pipeline.png)

每一個委派皆能在下個委派的前後執行作業。 處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。

最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。 此情況不包含實際要求管線。 反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。

將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。 `next` 參數代表管線中的下個委派。 您可以「不」** 呼叫「下一個」** 參數來對管線執行最少運算。 您通常可以在下個委派的前後執行動作，如下列範例所示：

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。 因為最少運算可避免不必要的工作，所以經常使用。 例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。 在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。 不過，查看下列有關嘗試寫入已傳送之回應的警告。

> [!WARNING]
> 請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。 回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。 例如，變更設定標頭、狀態碼等都會擲回例外狀況。 若在呼叫 `next` 後寫入回應本文：
>
> * 可能導致違反通訊協定。 例如，寫入超過指定 `Content-Length` 的內容。
> * 可能損毀本文格式。 例如，將 HTML 頁尾寫入 CSS 檔。
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。

<a name="order"></a>

## <a name="middleware-order"></a>中介軟體順序

`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。 順序對於安全性、效能和功能非常**重要**。

下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

在上述程式碼中：

* 使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。
* 並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。 例如， `UseCors` 和 `UseAuthentication` 必須以顯示的循序執行。

下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：

1. 例外狀況/錯誤處理
   * 當應用程式在開發環境中執行時：
     * 開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。
     * 資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。
   * 當應用程式在生產環境中執行時：
     * 例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。
     * HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。
1. HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。
1. 靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。
1. Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。
1. 驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。
1. 工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。 若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。
1. MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();
    app.UseMvc();
}
```

在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。 因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。

靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。 靜態檔案中介軟體**不**提供授權檢查。 靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。 如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。

若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。 驗證不會對未經驗證的要求執行最少運算。 雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器和動作之後，才會進行授權（和拒絕）。

下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。 靜態檔案並不會以此中介軟體順序壓縮。 來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a>Use、Run 與 Map

設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。 `Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。 `Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。

<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。 `Map` 會依據指定要求路徑的相符項目將要求管線分支。 如果要求路徑以指定路徑為開頭，則會執行分支。

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。

| 要求             | 回應                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Hello from non-Map delegate. |
| localhost:1234/map1 | Map Test 1                   |
| localhost:1234/map2 | Map Test 2                   |
| localhost:1234/map3 | Hello from non-Map delegate. |

使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。

<xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。 `Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。 下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

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

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a>內建的中介軟體

ASP.NET Core 隨附下列中介軟體元件。 「順序」** 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。 當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」**。 如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。

| 中介軟體 | 說明 | 單 |
| ---------- | ----------- | ----- |
| [驗證](xref:security/authentication/identity) | 提供驗證支援。 | 在需要 `HttpContext.User` 之前。 OAuth 回呼的終端機。 |
| [Cookie 原則](xref:security/gdpr) | 追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。 | 在發出 Cookie 的中介軟體之前。 範例：驗證、工作階段、MVC (TempData)。 |
| [CORS](xref:security/cors) | 設定跨原始來源資源共用。 | 在使用 CORS 的元件之前。 |
| [診斷](xref:fundamentals/error-handling) | 提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。 | 在產生錯誤的元件之前。 終端機的例外狀況，或為新的應用程式提供預設的網頁。 |
| [轉送的標頭](xref:host-and-deploy/proxy-load-balancer) | 將設為 Proxy 的標頭轉送到目前要求。 | 在使用更新方法的欄位之前。 範例：配置、主機，用戶端 IP、方法。 |
| [健康情況檢查](xref:host-and-deploy/health-checks) | 檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。 | 若某項要求與健康狀態檢查端點相符，則會是終端機。 |
| [HTTP 方法覆寫](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | 允許傳入的 POST 要求覆寫方法。 | 在使用更新方法的元件之前。 |
| [HTTPS 重新導向](xref:security/enforcing-ssl#require-https) | 將所有 HTTP 要求都重新導向至 HTTPS。 | 在使用 URL 的元件之前。 |
| [HTTP 嚴格的傳輸安全性 (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | 增強安全性的中介軟體，可新增特殊的回應標頭。 | 在傳送回應前和修改要求的元件後。 範例：轉送的標頭、URL 重寫。 |
| [MVC](xref:mvc/overview) | 處理 MVC/Pages 的要求 Razor 。 | 若要求符合路由則終止。 |
| [OWIN](xref:fundamentals/owin) | 以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。 | 若 OWIN 中介軟體完全處理要求則終止。 |
| [回應快取](xref:performance/caching/middleware) | 提供快取回應的支援。 | 在需要快取的元件之前。 |
| [回應壓縮](xref:performance/response-compression) | 提供壓縮回應的支援。 | 在需要壓縮的元件之前。 |
| [要求當地語系化](xref:fundamentals/localization) | 提供當地語系化支援。 | 在偵測當地語系化的元件之前。 |
| [端點路由](xref:fundamentals/routing) | 定義並限制要求路由。 | 比對路由的終端機。 |
| [本次](xref:fundamentals/app-state) | 提供管理使用者工作階段的支援。 | 在需要工作階段的元件之前。 |
| [靜態檔案](xref:fundamentals/static-files) | 支援靜態檔案的提供和目錄瀏覽。 | 若要求符合檔案則終止。 |
| [URL 重寫](xref:fundamentals/url-rewriting) | 提供重寫 URL 及重新導向要求的支援。 | 在使用 URL 的元件之前。 |
| [WebSocket](xref:fundamentals/websockets) | 啟用 WebSockets 通訊協定。 | 在接受 WebSocket 要求的必要元件之前。 |

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/middleware/write>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
