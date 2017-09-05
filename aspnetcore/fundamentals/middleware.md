---
title: "ASP.NET Core 中介軟體"
author: rick-anderson
description: "說明中介軟體，以及要求管線。"
keywords: "ASP.NET Core 中, 介軟體管線、 委派"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 84c357ebbf28dffc4382f6c648921210e72ac854
ms.sourcegitcommit: 26166785ad181a8519cb008800d71d96499b0499
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/01/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a>ASP.NET Core 中介軟體的基本概念

<a name=fundamentals-middleware></a>

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Steve Smith](http://ardalis.com)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)

## <a name="what-is-middleware"></a>什麼是中介軟體

中介軟體是組裝至應用程式管線來處理要求和回應的軟體。 每個元件：

* 您可以選擇是否要將要求傳遞至管線中的下一個元件。
* 之前和之後叫用管線中的下一個元件時，就可以執行工作。 

要求的委派可以用來建立要求管線。 將要求委派會處理每個 HTTP 要求。

要求使用設定委派[執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)，[對應](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)，和[使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)擴充方法。 個別要求委派可以指定內建為匿名方法 （稱為內建的中介軟體），或是它可以定義可重複使用的類別中。 這些可重複使用的類別和內嵌匿名方法*中介軟體*，或*中介軟體元件*。 要求管線中的每個中介軟體元件負責叫用下一個元件在管線中，或者如果適當的話，最少運算鏈結。

[中介軟體的移轉 HTTP 模組](../migration/http-modules.md)說明要求管線中 ASP.NET Core 版本和舊版之間的差異，並提供更多的中介軟體範例。

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>建立與 IApplicationBuilder 的中介軟體管線

ASP.NET Core 要求管線包含一連串要求委派，呼叫其中一個之後，這個圖表可顯示 （執行緒的執行如下所示的黑色箭號）：

![顯示要求抵達，透過三個 middlewares，並讓應用程式保持回應處理的要求處理模式。 每個中介軟體執行其邏輯，並傳遞要在 next （） 陳述式的下一個中介軟體的要求。 第三個中介軟體在處理要求之後, 是左手透過先前的兩個 middlewares 額外處理 next （） 陳述式之後，每個依次離開做為回應至用戶端應用程式之前傳回。](middleware/_static/request-delegate-pipeline.png)

每個委派之前和之後的下一個委派，可以執行作業。 委派，也可以決定要將要求傳遞至下一步的委派，會呼叫最少運算要求管線。 最少運算通常會因為它可讓以避免不必要的工作。 比方說，靜態檔案中介軟體可以傳回靜態檔案的要求，並最少運算管線的其餘部分。 例外狀況處理委派需要呼叫及早在管線中，讓它們可以攔截管線的後續階段中發生例外狀況。

最簡單的可能 ASP.NET Core 應用程式設定單一要求委派會處理所有要求。 此案例不包含實際的要求管線。 相反地，單一的匿名函式會呼叫每個 HTTP 要求的回應。

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

第一個[應用程式。執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)委派終止管線。

您可以鏈結在一起的多個要求委派[應用程式。使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)。 `next`參數代表管線中的下一個委派。 (請記住，您就可以縮短由管線*不*呼叫*下一步*參數。)您可以通常執行動作之前和之後的下一個委派，如這個範例所示：

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> 請勿呼叫`next.Invoke`回應傳送至用戶端之後。 若要變更`HttpResponse`開始回應後將會擲回例外狀況。 例如，變更，例如設定標頭、 狀態碼和其他內容，將會擲回例外狀況。 寫入回應主體之後呼叫`next`:
> - 可能會造成通訊協定違規。 例如，寫入多個述`content-length`。
> - 可能會損毀的本文格式。 例如，HTML 頁尾寫入 CSS 檔案。
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)是有用的提示，指出如果在傳送標頭和/或本文寫入。

## <a name="ordering"></a>排序

順序中新增中介軟體元件`Configure`方法定義在要求中，會呼叫的順序和回應的相反順序。 此順序相當重要的安全性、 效能和功能。

Configure 方法 （如下所示） 會加入下列的中介軟體元件：

1. 例外狀況/錯誤處理
2. 靜態檔案伺服器
3. 驗證
4. MVC

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

在上述程式碼中`UseExceptionHandler`是第一個新增至管線的中介軟體元件，因此，它會攔截稍後呼叫中發生任何例外狀況。

靜態檔案中介軟體稱為及早在管線中，讓它可以處理要求，並最少運算無須經過剩餘的元件。 靜態檔案中介軟體提供**沒有**授權檢查。 任何檔案由提供服務，包括下*wwwroot*，都可以公開使用。 請參閱[靜態檔案處理](xref:fundamentals/static-files)如需安全靜態檔案的方法。

如果要求不會處理靜態檔案中介軟體，它會傳遞給身分識別的中介軟體 (`app.UseIdentity`)，它會執行驗證。 身分識別不最少運算未經驗證的要求。 雖然身分識別進行驗證的要求時，授權 （及拒絕） 後才會 MVC 選取特定的控制器和動作。

下列範例會示範中介軟體訂購靜態檔案的要求處理前回應壓縮中介軟體的靜態檔案中介軟體的位置。 具有此中介軟體的順序，不會壓縮靜態檔案。 MVC 回應[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)可以壓縮。

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a>使用、 執行和對應

您可以設定 HTTP 管線使用`Use`， `Run`，和`Map`。 `Use`方法可以縮短管線 (亦即，如果它不會呼叫`next`要求委派)。 `Run`是一項慣例，以及一些中介軟體元件暴露`Run[Middleware]`在管線結尾處執行的方法。

`Map*`分支管線，依照慣例可用擴充功能。 [地圖](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)分支要求管線會根據給定的要求路徑的相符項目。 如果要求路徑的開頭指定的路徑，則會執行分支。

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：

| 要求 | 回應 |
| --- | --- |
| localhost:1234 | 從非對應委派的 hello。  |
| localhost:1234 / map1 | 測試 1 對應 |
| localhost:1234 / map2 | 測試 2 對應 |
| localhost:1234 / map3 | 從非對應委派的 hello。  |

當`Map`是使用，相符的路徑區段已從`HttpRequest.Path`和附加至`HttpRequest.PathBase`針對每個要求。

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)分支要求管線會根據給定的述詞的結果。 型別的任何述詞`Func<HttpContext, bool>`可用來將要求對應至新分支的管線。 在下列範例中，使用述詞來偵測是否存在的查詢字串變數`branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：

| 要求 | 回應 |
| --- | --- |
| localhost:1234 | 從非對應委派的 hello。  |
| localhost:1234 /？ 分支 = master | 使用分支 = master|

`Map`支援巢狀結構，例如：

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map`也可以符合多個區段同時發生，例如：

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>內建的中介軟體

ASP.NET Core 隨附下列的中介軟體元件：

| 中介軟體 | 說明 |
| ----- | ------- |
| [驗證](xref:security/authentication/identity) | 提供的驗證支援。 |
| [CORS](xref:security/cors) | 設定跨原始資源共用。 |
| [回應快取](xref:performance/caching/middleware) | 提供支援快取回應。 |
| [回應的壓縮](xref:performance/response-compression) | 提供支援壓縮回應。 |
| [路由傳送](xref:fundamentals/routing) | 定義，並限制要求的路由。 |
| [工作階段](xref:fundamentals/app-state) | 提供管理使用者工作階段的支援。 |
| [靜態檔案](xref:fundamentals/static-files) | 提供靜態檔案和目錄瀏覽提供服務的支援。 |
| [URL 重寫中介軟體](xref:fundamentals/url-rewriting) | 提供重寫 Url，並將要求重新導向的支援。 |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a>寫入中介軟體

中介軟體通常是封裝在類別和使用擴充方法公開。 請考慮下列的中介軟體會從查詢字串設定為目前要求的文化特性：

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

注意： 上述範例程式碼會示範建立中介軟體元件。 請參閱[全球化和當地語系化](xref:fundamentals/localization)ASP.NET Core 內建的當地語系化支援。

您可以藉由傳遞文化特性，例如測試中介軟體`http://localhost:7997/?culture=no`。

下列程式碼會將中介軟體委派移至類別：

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

下列擴充方法會公開透過中的介軟體[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

下列程式碼呼叫的中介軟體從`Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

中介軟體應該遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)藉由公開在其建構函式及其相依性。 中介軟體建構一次，每個*應用程式存留期*。 請參閱*每個要求的相依性*下方如果您需要與中介軟體要求內的共用服務。

中介軟體元件可以解決其相依性的相依性插入透過建構函式的參數。 [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)也可以接受其他參數直接。

### <a name="per-request-dependencies"></a>每個要求的相依性

中介軟體建構應用程式啟動時，無法個別要求，因為*範圍*中介軟體建構函式所使用的存留時間服務不會與其他相依性插入類型共用期間每個要求。 如果您必須共用*範圍*服務中介軟體和其他類型之間，加入這些服務可`Invoke`方法的簽章。 `Invoke`方法可以接受其他參數會填入相依性插入。 例如: 

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>資源

* [此文件中使用的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [移轉的 HTTP 模組中, 介軟體](../migration/http-modules.md)
* [應用程式啟動](startup.md)
* [要求的功能](request-features.md)
