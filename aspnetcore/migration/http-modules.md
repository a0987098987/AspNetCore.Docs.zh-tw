---
title: 將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體
author: rick-anderson
description: ''
ms.author: riande
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: bdf27ccb742d4bc05bac71e6c96d71c38dcb4b62
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659681"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體

本文說明如何將 ASP.NET[的現有 HTTP 模組和處理常式，從 system.webserver](/iis/configuration/system.webserver/)遷移至 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)。

## <a name="modules-and-handlers-revisited"></a>已再次進行模組和處理常式

在繼續 ASP.NET Core 中介軟體之前，讓我們先回顧一下 HTTP 模組和處理常式的操作方式：

![模組處理常式](http-modules/_static/moduleshandlers.png)

**處理常式包括：**

* 執行[IHttpHandler](/dotnet/api/system.web.ihttphandler)的類別

* 用來處理指定的檔案名或副檔名的要求，例如 *. report*

* *在 web.config*中[設定](/iis/configuration/system.webserver/handlers/)

**模組包括：**

* 執行[IHttpModule](/dotnet/api/system.web.ihttpmodule)的類別

* 針對每個要求叫用

* 能夠短路（停止進一步處理要求）

* 能夠新增至 HTTP 回應，或自行建立

* *在 web.config*中[設定](/iis/configuration/system.webserver/modules/)

**模組處理傳入要求的順序取決於：**

1. [應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的數列事件： [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一個或多個事件的處理常式。

2. 針對相同的事件，這是在*web.config*中設定的順序。

除了模組之外，您還可以將生命週期事件的處理常式新增至*Global.asax.cs*檔案。 這些處理常式會在已設定之模組中的處理常式之後執行。

## <a name="from-handlers-and-modules-to-middleware"></a>從處理常式和模組到中介軟體

**中介軟體比 HTTP 模組和處理常式簡單：**

* 模組、處理常式、 *Global.asax.cs*、 *web.config （IIS*設定除外）和應用程式生命週期都已消失

* 中介軟體已接管模組和處理常式的角色

* 中介軟體是使用程式碼（而不*是 web.config）* 來設定

* [管線分支](xref:fundamentals/middleware/index#use-run-and-map)可讓您將要求傳送至特定中介軟體，而不只是 URL，也會根據要求標頭、查詢字串等。

**中介軟體非常類似模組：**

* 在每個要求的主體中叫用

* 無法將[要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)，藉此縮短要求的路線

* 能夠建立自己的 HTTP 回應

**中介軟體和模組會以不同的順序進行處理：**

* 中介軟體的順序是根據它們插入要求管線的順序，而模組的順序則主要是根據[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件

* 回應的中介軟體順序與要求相反，而模組的順序則與要求和回應相同

* 請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![中介軟體](http-modules/_static/middleware.png)

請注意，在上圖中，驗證中介軟體會簡短縮短要求。

## <a name="migrating-module-code-to-middleware"></a>將模組程式碼遷移至中介軟體

現有的 HTTP 模組看起來會像這樣：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

如[中介軟體](xref:fundamentals/middleware/index)頁面所示，ASP.NET Core 中介軟體是一個類別，它會公開取得 `HttpContext` 並傳回 `Task`的 `Invoke` 方法。 您的新中介軟體看起來會像這樣：

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

先前的中介軟體範本取自[撰寫中介軟體](xref:fundamentals/middleware/write)的一節。

*MyMiddlewareExtensions* helper 類別可讓您更輕鬆地在 `Startup` 類別中設定中介軟體。 `UseMyMiddleware` 方法會將中介軟體類別新增至要求管線。 中介軟體所需的服務會插入中介軟體的函式中。

<a name="http-modules-shortcircuiting-middleware"></a>

您的模組可能會終止要求，例如，如果使用者未獲授權：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

中介軟體會藉由不在管線中的下一個中介軟體上呼叫 `Invoke` 來處理這種情況。 請記住，這並不會完全終止要求，因為當回應透過管線回傳時，仍然會叫用先前的中介軟體。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

當您將模組的功能遷移至新的中介軟體時，可能會發現您的程式碼未編譯，因為 ASP.NET Core 中的 `HttpContext` 類別已大幅變更。 [稍後](#migrating-to-the-new-httpcontext)，您將瞭解如何遷移至新的 ASP.NET Core HttpCoNtext。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>將模組插入遷移至要求管線

HTTP 模組通常*會使用 web.config*新增至要求管線：

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

將[新的中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)新增至 `Startup` 類別中的要求管線，以轉換此項：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

管線中您插入新中介軟體的確切位置，取決於它處理為模組的事件（`BeginRequest`、`EndRequest`等）及其*在 web.config 中*的模組清單中的順序。

如先前所述，ASP.NET Core 中沒有應用程式生命週期，中介軟體處理回應的順序與模組使用的順序不同。 這可能會讓您的訂購決策更具挑戰性。

如果順序變成問題，您可以將模組分割成可獨立排序的多個中介軟體元件。

## <a name="migrating-handler-code-to-middleware"></a>將處理常式程式碼遷移至中介軟體

HTTP 處理常式看起來像這樣：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

在您的 ASP.NET Core 專案中，您會將其轉譯為中介軟體，如下所示：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

這個中介軟體非常類似于模組的對應中介軟體。 唯一的真正差異在於，這裡沒有 `_next.Invoke(context)`的呼叫。 這是合理的，因為處理常式是在要求管線的結尾，因此不會叫用下一個中介軟體。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>將處理常式插入遷移至要求管線

設定 HTTP 處理常式*是在 web.config*中完成，看起來像這樣：

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

您可以將新的處理常式中介軟體新增至 `Startup` 類別中的要求管線，以進行轉換，類似于從模組轉換的中介軟體。 該方法的問題在於，它會將所有要求傳送至新的處理常式中介軟體。 不過，您只想要要求具有指定的延伸模組，才能到達您的中介軟體。 這會提供您與 HTTP 處理常式相同的功能。

其中一個解決方案是使用 `MapWhen` 擴充方法，將管線分支給具有給定副檔名的要求。 您可以在新增其他中介軟體的相同 `Configure` 方法中執行此動作：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` 會採用下列參數：

1. Lambda，會採用 `HttpContext` 並傳回 `true` （如果要求應該在分支下）。 這表示您不僅可以根據要求的延伸模組，也會根據要求標頭、查詢字串參數等來分支要求。

2. 會採用 `IApplicationBuilder` 並新增分支之所有中介軟體的 lambda。 這表示您可以將其他中介軟體新增至處理常式中介軟體前方的分支。

在所有要求上叫用分支之前，已將中介軟體新增至管線;分支不會對它們產生任何影響。

## <a name="loading-middleware-options-using-the-options-pattern"></a>使用選項模式載入中介軟體選項

有些模組和處理常式都有*儲存在 web.config*中的設定選項。不過，在 ASP.NET Core 會使用新的設定模型來取代*web.config*。

新的設定[系統](xref:fundamentals/configuration/index)會提供您下列選項來解決此問題：

* 將選項直接插入中介軟體，如下一[節](#loading-middleware-options-through-direct-injection)所示。

* 使用[選項模式](xref:fundamentals/configuration/options)：

1. 建立類別來存放中介軟體選項，例如：

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. 儲存選項值

   設定系統可讓您將選項值儲存在您想要的任何位置。 不過，大部分的網站都使用*appsettings*，所以我們會採用這種方法：

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   這裡的*MyMiddlewareOptionsSection*是區段名稱。 它不一定要與選項類別的名稱相同。

3. 將選項值與選項類別產生關聯

    選項模式會使用 ASP.NET Core 的相依性插入架構，將選項類型（例如 `MyMiddlewareOptions`）與具有實際選項的 `MyMiddlewareOptions` 物件產生關聯。

    更新您的 `Startup` 類別：

   1. 如果您使用的是*appsettings*，請將它新增至 `Startup` 的函式中的 configuration builder：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. 設定選項服務：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. 將您的選項與選項類別產生關聯：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. 將選項插入中介軟體的函式。 這類似于將選項插入控制器中。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   將中介軟體新增至 `IApplicationBuilder` 的[UseMiddleware](#http-modules-usemiddleware)擴充方法會負責相依性插入。

   這不限於 `IOptions` 物件。 中介軟體所需的任何其他物件都可以用這種方式插入。

## <a name="loading-middleware-options-through-direct-injection"></a>透過直接插入來載入中介軟體選項

[選項] 模式的優點是它會在選項值及其取用者之間建立鬆散結合。 當您將 options 類別與實際的選項值建立關聯之後，任何其他類別都可以透過相依性插入架構取得選項的存取權。 不需要傳遞選項值。

不過，如果您想要使用相同的中介軟體兩次，但有不同的選項，則會中斷。 例如，在允許不同角色的不同分支中使用的授權中介軟體。 您無法將兩個不同的選項物件與一個選項類別產生關聯。

解決方法是使用 `Startup` 類別中的實際選項值來取得 options 物件，並將它們直接傳遞至中介軟體的每個實例。

1. 將第二個金鑰新增至*appsettings*

   若要將第二組選項新增至*appsettings* ，請使用新的金鑰來唯一識別它：

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. 取出選項值，並將它們傳遞至中介軟體。 `Use...` 擴充方法（會將中介軟體新增至管線）是傳遞選項值的邏輯位置： 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. 啟用中介軟體以接受選項參數。 提供 `Use...` 擴充方法的多載（會採用 options 參數，並將它傳遞給 `UseMiddleware`）。 當以參數呼叫 `UseMiddleware` 時，它會在具現化中介軟體物件時，將參數傳遞至中介軟體的函式。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   請注意，這會在 `OptionsWrapper` 物件中包裝 options 物件。 這會依照中介軟體的所需來執行 `IOptions`。

## <a name="migrating-to-the-new-httpcontext"></a>遷移至新的 HttpCoNtext

您稍早看到，中介軟體中的 `Invoke` 方法會接受 `HttpContext`類型的參數：

```csharp
public async Task Invoke(HttpContext context)
```

ASP.NET Core 中的 `HttpContext` 已大幅變更。 本節說明如何將[system.web](/dotnet/api/system.web.httpcontext)的最常使用屬性轉譯為新的 `Microsoft.AspNetCore.Http.HttpContext`。

### <a name="httpcontext"></a>HttpCoNtext

**HttpCoNtext 的專案**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**唯一要求識別碼（不含 System.web. HttpCoNtext 對應）**

為每個要求提供唯一的識別碼。 在記錄中包含非常有用的。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpCoNtext 要求

**HttpMethod**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpCoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**RawUrl**會轉譯為：（& i）

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**IsSecureConnection**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**UserHostAddress**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpCoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**RequestCoNtext. RouteData**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpcoNtext.current**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**UserAgent**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**UrlReferrer**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpCoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpCoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> 只有在 content 子類型為*x-www-表單 urlencoded*或*表單資料*時，才讀取表單值。

**InputStream**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> 請只在管線結尾的處理常式類型中介軟體中使用此程式碼。
>
>您可以讀取每個要求只顯示一次的原始本文。 在第一次讀取之後嘗試讀取本文的中介軟體將會讀取空白主體。
>
>這並不適用于讀取如先前所示的表單，因為這是從緩衝區完成的。

### <a name="httpcontextresponse"></a>HttpCoNtext 回應

**StatusDescription**會轉譯為下列內容：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**ContentEncoding**和**HTTPcoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpCoNtext**本身的 ContentType 也會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpCoNtext**會轉譯為：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpCoNtext. Response TransmitFile**

[這裡](../fundamentals/request-features.md#middleware-and-request-features)會討論如何提供檔案。

**HttpCoNtext. 標頭**

傳送回應標頭很複雜，因為如果您在任何專案都已寫入回應主體之後設定它們，則不會將它們送出。

解決方案是設定回呼方法，在寫入回應開始之前，會先呼叫它。 這最好是在中介軟體的 `Invoke` 方法開始時執行。 這是設定回應標頭的回呼方法。

下列程式碼會設定名為 `SetHeaders`的回呼方法：

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders` 回呼方法看起來像這樣：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpCoNtext. 回應 Cookie**

Cookie 會以*設定-Cookie*回應標頭傳送至瀏覽器。 因此，傳送 cookie 需要與用來傳送回應標頭相同的回呼：

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies` 回呼方法如下所示：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>其他資源

* [HTTP 處理常式和 HTTP 模組總覽](/iis/configuration/system.webserver/)
* [組態](xref:fundamentals/configuration/index)
* [應用程式啟動](xref:fundamentals/startup)
* [中介軟體](xref:fundamentals/middleware/index)
