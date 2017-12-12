---
title: "移轉的 HTTP 處理常式和 ASP.NET Core 中介軟體的模組"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: f217e5264742826f285444dcbaea4b28b97c4d7e
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a>移轉的 HTTP 處理常式和 ASP.NET Core 中介軟體的模組 

由[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

本文將說明如何移轉現有的 ASP.NET [HTTP 模組和處理常式 system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/)到 ASP.NET Core[中介軟體](../fundamentals/middleware.md)。

## <a name="modules-and-handlers-revisited"></a>模組和處理常式進行重新瀏覽

之前 ASP.NET Core 中介軟體，讓我們先複習一下 HTTP 模組和處理常式的運作方式：

![模組處理常式](http-modules/_static/moduleshandlers.png)

**處理常式包括：**

   * 類別可實作[IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)

   * 用來處理要求，以指定的檔案名稱或副檔名，例如*.report*

   * [設定](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/)中*Web.config*

**模組為：**

   * 類別可實作[IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)

   * 叫用每個要求

   * 能夠最少運算 （停止進一步處理的要求）

   * 無法加入至 HTTP 回應，或建立自己

   * [設定](https://docs.microsoft.com//iis/configuration/system.webserver/modules/)中*Web.config*

**模組處理內送要求的順序取決於：**

   1. [應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的系列事件： [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest)， [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一或多個事件的處理常式。

   2. 相同事件中設定的順序*Web.config*。

除了模組，您可以加入處理常式的生命週期事件，以您*Global.asax.cs*檔案。 在 設定模組中的處理常式之後，執行這些處理常式。

## <a name="from-handlers-and-modules-to-middleware"></a>從處理常式和模組中介軟體

**中介軟體會簡單比 HTTP 模組和處理常式：**

   * 模組、 處理常式、 *Global.asax.cs*， *Web.config* （除了 IIS 組態） 和應用程式生命週期已消失

   * 模組和處理常式的角色有接手中介軟體

   * 使用程式碼設定中介軟體，而非在*Web.config*

   * [管線分支](../fundamentals/middleware.md#middleware-run-map-use)可讓您將要求傳送至特定中介軟體，根據不僅還在要求標頭、 查詢字串和其他內容的 URL。

**中介軟體是非常類似於模組：**

   * 在每個要求主體中叫用

   * 能夠藉由將要求，最少運算[不將要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)

   * 可以建立自己的 HTTP 回應

**中介軟體和模組會處理不同的順序：**

   * 中介軟體的順序根據在其中插入至要求管線時之模組的順序主要是根據的順序[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件

   * 順序中介軟體的回應時，針對要求，從該反向模組的順序是相同的要求和回應

   * 請參閱[使用 IApplicationBuilder 建立中介軟體管線](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)

![中介軟體](http-modules/_static/middleware.png)

請注意如何在上圖中，驗證中介軟體 short-circuited 要求。

## <a name="migrating-module-code-to-middleware"></a>中介軟體的移轉模組程式碼

現有的 HTTP 模組會看起來像這樣：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

中所示[中介軟體](../fundamentals/middleware.md) 頁面上，ASP.NET Core 中介軟體是公開的類別`Invoke`方法擷取`HttpContext`並傳回`Task`。 您新的中介軟體看起來像這樣：

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

上述的中介軟體範本上建立從區段[寫入中介軟體](../fundamentals/middleware.md#middleware-writing-middleware)。

*MyMiddlewareExtensions* helper 類別可讓您更輕鬆地設定中的介軟體在您`Startup`類別。 `UseMyMiddleware`方法將中介軟體類別加入至要求管線。 中介軟體所需的服務取得插入的中介軟體的建構函式中。

<a name="http-modules-shortcircuiting-middleware"></a>

您可以在模組可能會終止要求，例如，如果使用者未獲授權：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

中介軟體所不會呼叫處理`Invoke`在管線中的下一個中介軟體。 請記住，這不會完全終止要求，因為前一個 middlewares 將仍會叫用時回應回通過管線。

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

當您將模組的功能移轉到新的中介軟體時，您可能會發現不會編譯程式碼，因為`HttpContext`類別已經大幅變更 ASP.NET Core 中。 [稍後](#migrating-to-the-new-httpcontext)，您會看到如何將移轉至新的 ASP.NET Core HttpContext。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>移轉模組插入要求管線

HTTP 模組通常會加入至要求管線使用*Web.config*:

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

轉換所[新增您新的中介軟體](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)要求管線中您`Startup`類別：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

您可以在該處插入新的中介軟體管線中的確切取決於它處理為模組的事件 (`BeginRequest`，`EndRequest`等等) 和其清單中的模組中的順序*Web.config*。

如先前所述，沒有應用程式生命週期中有 ASP.NET Core 並且由中介軟體處理回應的順序不同模組所使用的順序。 這可以決定排序更具挑戰性。

如果順序會變成問題，您可以在模組無法分成多個可以獨立地排序的中介軟體元件。

## <a name="migrating-handler-code-to-middleware"></a>移轉的處理常式程式碼中介軟體

HTTP 處理常式看起來像這樣：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

在 ASP.NET Core 專案中，會轉換這個中介軟體如下所示：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

此中介軟體是非常類似於對應到模組的中介軟體。 唯一的差異在於，以下是不需要呼叫`_next.Invoke(context)`。 這很合理，因為處理常式的要求管線結尾應該會有沒有下一個中介軟體要叫用。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>移轉處理常式插入要求管線

設定 HTTP 處理常式是在*Web.config* ，看起來像這樣：

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

您可以將此轉換將新處理常式中介軟體新增至要求管線中您`Startup`類別細明體，類似於轉換從模組的中介軟體。 該方法的問題是，它會將所有的要求傳送至新處理常式中介軟體。 不過，您只想連線到中介軟體具有特定副檔名的要求。 這樣會使您與您的 HTTP 處理常式有相同的功能。

其中一個解決方案是分支，具有給定延伸模組，要求的管線使用`MapWhen`擴充方法。 這麼做在同一個`Configure`方法讓您加入其他中介軟體：

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen`採用這些參數：

1. 採用 lambda`HttpContext`並傳回`true`如果要求應該向分支。 這表示您可以建立分支不只是根據它們的副檔名，而要求標頭、 查詢字串參數和其他內容的要求。

2. 採用 lambda `IApplicationBuilder` ，並將分支的所有中介軟體。 這表示您可以加入其他中介軟體至分支前面處理常式中介軟體。

中介軟體新增至管線之前分支將會叫用的所有要求。分支將會有任何影響，在其上。

## <a name="loading-middleware-options-using-the-options-pattern"></a>載入使用選項模式的中介軟體選項

部分模組和處理常式有儲存在組態選項*Web.config*。不過，在 ASP.NET Core 新的組態模型會使用取代*Web.config*。

新[組態系統](xref:fundamentals/configuration/index)提供您下列選項可用來解決這個問題：

* 直接插入到中介軟體選項中所示[下一節](#loading-middleware-options-through-direct-injection)。

* 使用[選項模式](xref:fundamentals/configuration/options):

1.  建立類別以包裝您的中介軟體選項，例如：

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  儲存選項值

    組態系統，可讓您儲存選項任何地方需要的值。 不過，最站台使用*appsettings.json*，因此我們將方法：

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    *MyMiddlewareOptionsSection*以下是區段名稱。 它不一定要與您的選項類別名稱相同。

3. 選項值關聯的選項類別

    選項模式使用 ASP.NET Core 相依性插入架構相關聯的選項類型 (例如`MyMiddlewareOptions`) 與`MyMiddlewareOptions`具有實際的選項物件。

    更新您`Startup`類別：

    1.  如果您使用*appsettings.json*，將它加入至組態產生器中`Startup`建構函式：

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  設定選項服務：

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  將您的選項與選項類別產生關聯：

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  將插入至中介軟體建構函式的選項。 這是類似於插入到控制器的選項。

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  [UseMiddleware](#http-modules-usemiddleware)擴充方法，將它新增至中的介軟體`IApplicationBuilder`，負責相依性插入。

  這並不限於`IOptions`物件。 這種方式可插入您的中介軟體所需要的任何其他物件。

## <a name="loading-middleware-options-through-direct-injection"></a>載入經由直接資料隱碼的中介軟體選項

選項模式的優點，它會建立鬆散選項值和其取用者之間的連結。 一旦您已建立的選項類別關聯的實際選項值，任何其他類別可以取得存取透過相依性插入架構選項。 沒有需要通過選項值。

這會細分不過如果您想要使用不同的選項兩次，使用相同的中介軟體。 例如授權中介軟體可讓不同角色的不同分支中使用。 您無法將兩個不同的選項物件與一個選項類別。

解決方法是取得選項物件中的實際選項值與您`Startup`類別，並傳遞至每個執行個體的中介軟體直接。

1.  新增第二個機碼*appsettings.json*

    若要加入第二組選項以*appsettings.json*檔案中，以唯一識別它使用新的金鑰：

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  擷取選項值，並將其傳遞至中介軟體。 `Use...` （這會加入到管線的中介軟體） 的擴充方法是將選項值在邏輯上： 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  可讓中介軟體選項參數。 提供的多載`Use...`擴充方法 (採用 options 參數，並將其傳遞給`UseMiddleware`)。 當`UseMiddleware`稱為參數，它將參數傳遞到中介軟體建構函式時，它會具現化中介軟體物件。

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    請注意這會在將選項物件的包裝`OptionsWrapper`物件。 這會實作`IOptions`，如預期般的中介軟體建構函式。

## <a name="migrating-to-the-new-httpcontext"></a>移轉至新的 HttpContext

您稍早所見，`Invoke`中介軟體中的方法參數的型別`HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext`已經大幅變更 ASP.NET Core 中。 本節說明如何將最常用的屬性轉譯[System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext)新`Microsoft.AspNetCore.Http.HttpContext`。

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**唯一的要求識別碼 （沒有 System.Web.HttpContext 對應項目）**

針對每個要求，讓您的唯一識別碼。 要包含在記錄中非常有用。

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url**和**HttpContext.Request.RawUrl**轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> 讀取表單值，只有內容的子類型為*x-www-表單-urlencoded*或*表單資料*。

**HttpContext.Request.InputStream**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> 此程式碼只能用在處理常式型別中介軟體，在管線結尾處。
>
>每個要求只能出現一次如上所示，您可以閱讀原始的主體。 嘗試讀取內容之後第一次讀取會讀取空本文, 的中介軟體。
>
>這不適用於讀取因為緩衝區完成表單為稍早所示。

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status**和**HttpContext.Response.StatusDescription**轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding**和**HttpContext.Response.ContentType**轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType**上自己也會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output**會轉譯成：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

檔案服務會討論[這裡](../fundamentals/request-features.md#middleware-and-request-features)。

**HttpContext.Response.Headers**

傳送回應標頭複雜，是因為，如果您將任何項目寫入至回應主體之後，它們將不會傳送。

方案是將設定寫入回應啟動之前將權限呼叫的回呼方法。 最好的做法是在開始`Invoke`中介軟體中的方法。 它是設定您的回應標頭這個回呼方法。

下列程式碼設定呼叫的回呼方法`SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders`回呼方法會看起來像這樣：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

周遊到中的瀏覽器的 cookie *Set-cookie*回應標頭。 如此一來，傳送 cookie 需要所用相同的回呼傳送回應標頭：

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies`回呼方法會如下所示：

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>其他資源

* [HTTP 處理常式和 HTTP 模組概觀](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [組態](xref:fundamentals/configuration/index)

* [應用程式啟動](../fundamentals/startup.md)

* [中介軟體](../fundamentals/middleware.md)
