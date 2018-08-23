---
title: 將 HTTP 處理常式和模組移轉至 ASP.NET Core 中介軟體
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 9dd28b86966912cce87166feb37e65adf3dd6dcb
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902667"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a>將 HTTP 處理常式和模組移轉至 ASP.NET Core 中介軟體

藉由[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)

本文說明如何移轉現有的 ASP.NET [HTTP 模組和處理常式 system.webserver](/iis/configuration/system.webserver/)至 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)。

## <a name="modules-and-handlers-revisited"></a>模組和第二次接觸的處理常式

ASP.NET Core 中介軟體之前，讓我們先複習一下 HTTP 模組和處理常式的運作方式：

![模組處理常式](http-modules/_static/moduleshandlers.png)

**處理常式包括：**

   * 類別實作[IHttpHandler](/dotnet/api/system.web.ihttphandler)

   * 用來處理要求，以指定的檔案名稱或副檔名，例如 *.report*

   * [設定](/iis/configuration/system.webserver/handlers/)在*Web.config*

**模組有︰**

   * 類別實作[IHttpModule](/dotnet/api/system.web.ihttpmodule)

   * 叫用每個要求

   * 能夠以最少運算 （停止進一步處理的要求）

   * 無法新增至 HTTP 回應，或自行建立

   * [設定](/iis/configuration/system.webserver/modules/)在*Web.config*

**模組中處理連入要求的順序取決於：**

   1. [應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的系列事件： [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)， [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一或多個事件的處理常式。

   2. 對於相同事件，也就是在已設定的順序*Web.config*。

除了模組，您可以新增至生命週期事件的處理常式您*Global.asax.cs*檔案。 在 設定模組中的處理常式之後，執行這些處理常式。

## <a name="from-handlers-and-modules-to-middleware"></a>從處理常式和模組到中介軟體

**中介軟體是 HTTP 模組和處理常式比簡單的：**

   * 模組、 處理常式*Global.asax.cs*， *Web.config* （除了 IIS 組態） 和應用程式生命週期都不見了

   * 模組和處理常式的角色有接手的中介軟體

   * 中介軟體會設定為使用程式碼，而非在*Web.config*

   * [管線分支](xref:fundamentals/middleware/index#use-run-and-map)可讓您將要求傳送至特定中介軟體，根據不僅同時也在要求標頭、 查詢字串等的 URL。

**中介軟體是非常類似於模組：**

   * 在每個要求的主體中叫用

   * 能夠藉由將要求中，最少運算[不將要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)

   * 能夠建立自己的 HTTP 回應

**中介軟體和模組會處理不同的順序：**

   * 以這它們要插入至要求管線，而模組的順序主要是根據順序為基礎的中介軟體順序[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件

   * 回應的中介軟體順序是反向程序，從要求，而模組的順序是相同的要求和回應

   * 請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)

![中介軟體](http-modules/_static/middleware.png)

請注意如何在上圖中，驗證中介軟體縮短，則要求。

## <a name="migrating-module-code-to-middleware"></a>將模組程式碼移轉至中介軟體

現有的 HTTP 模組會看起來像這樣：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

中所示[中介軟體](xref:fundamentals/middleware/index)頁面上，ASP.NET Core 中介軟體是公開的類別`Invoke`方法採用`HttpContext`，並傳回`Task`。 您新的中介軟體會看起來像這樣：

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

上一節中建立上述的中介軟體範本[寫入中介軟體](xref:fundamentals/middleware/index#write-middleware)。

*MyMiddlewareExtensions*協助程式類別可讓您更輕鬆地設定您的中介軟體，在您`Startup`類別。 `UseMyMiddleware`方法會將您的中介軟體類別加入至要求管線。 中介軟體所需的服務取得插入中介軟體的建構函式中。

<a name="http-modules-shortcircuiting-middleware"></a>

您的模組可能會終止要求，例如，如果使用者未獲授權：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

中介軟體會處理此不呼叫`Invoke`管線中的下一個中介軟體上。 記住這不會完全終止要求，因為前一個中介軟體將仍會叫用時的回應會回到管線。

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

當您將您的模組功能移轉到新中介軟體時，您可能會發現不編譯您的程式碼，因為`HttpContext`類別已大幅改變 ASP.NET Core 中。 [稍後在](#migrating-to-the-new-httpcontext)，您將了解如何移轉至新的 ASP.NET Core HttpContext。

## <a name="migrating-module-insertion-into-the-request-pipeline"></a>移轉至要求管線的模組插入

HTTP 模組通常會新增至要求管線，使用*Web.config*:

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

轉換所[新增您新的中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)至要求管線中您`Startup`類別：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

您可以在該處插入新中介軟體管線中的確切取決於它處理為模組的事件 (`BeginRequest`，`EndRequest`等) 和其在清單中的模組中的順序*Web.config*。

如先前所述，有是 ASP.NET Core 中的任何應用程式生命週期和模組所使用的順序由中介軟體處理回應的順序不同。 這可能會使您訂購的決策更具挑戰性。

如果順序會變成問題，您可以將您的模組分割成多個可以獨立地排序的中介軟體元件。

## <a name="migrating-handler-code-to-middleware"></a>將處理常式程式碼移轉至中介軟體

HTTP 處理常式看起來像這樣：

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

在 ASP.NET Core 專案中，您會將它轉譯成中介軟體如下所示：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

此中介軟體是非常類似於對應模組到中介軟體。 唯一的真正差異在於，以下是不需要呼叫`_next.Invoke(context)`。 這很合理，因為處理常式結尾的要求管線，因此沒有下一個中介軟體來叫用。

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a>移轉的處理常式插入至要求管線

設定 HTTP 處理常式完成*Web.config* ，看起來像這樣：

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

您可以將此轉換加入至要求管線中的新處理常式中介軟體程式`Startup`類別，類似於從模組轉換的中介軟體。 該方法的問題是，它會將所有要求都傳送至新處理常式中介軟體。 不過，您只想具有特定副檔名的要求進入您的中介軟體。 就能得到相同的功能，您必須與您的 HTTP 處理常式。

其中一個解決方案是分支的要求指定的擴充功能中，管線使用`MapWhen`擴充方法。 這麼做在同一個`Configure`您可在其中新增其他中介軟體的方法：

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

`MapWhen` 採用這些參數：

1. 採用 lambda `HttpContext` ，並傳回`true`如果要求應該關閉分支。 這表示您可以分支不只是根據其延伸模組，而是根據在要求標頭、 查詢字串參數等的要求。

2. 採用 lambda `IApplicationBuilder` ，並將新分支的所有中介軟體。 這表示您可以加入其他中介軟體至分支之前處理常式中介軟體。

之前的分支將會叫用的所有要求; 新增至管線的中介軟體分支不會影響在其上。

## <a name="loading-middleware-options-using-the-options-pattern"></a>正在載入使用 「 選項 」 模式的中介軟體選項

部分模組和處理常式已儲存在組態選項*Web.config*。不過，在 ASP.NET Core 中新的組態模型用來代替*Web.config*。

新[組態系統](xref:fundamentals/configuration/index)提供您一個選項來解決這個問題：

* 直接插入到中介軟體選項中所示[下一節](#loading-middleware-options-through-direct-injection)。

* 使用[選項模式](xref:fundamentals/configuration/options):

1. 建立類別以包裝您的中介軟體選項，例如：

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. 儲存選項值

   組態系統，可讓您儲存選項任何地方您希望的值。 不過，在網站使用最*appsettings.json*，因此我們將該方法：

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   *MyMiddlewareOptionsSection*以下是區段名稱。 它不一定要與您的選項類別的名稱相同。

3. 選項類別相關聯的選項值

    「 選項 」 模式使用 ASP.NET Core 的相依性插入架構為建立關聯的選項類型 (例如`MyMiddlewareOptions`) 與`MyMiddlewareOptions`具有實際選項物件。

    更新您`Startup`類別：

   1. 如果您使用*appsettings.json*，將它新增至組態產生器中`Startup`建構函式：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. 設定選項的服務：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. 您的選項類別相關聯的選項：

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. 插入至中介軟體建構函式的選項。 這是類似於插入到控制器的選項。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   [UseMiddleware](#http-modules-usemiddleware)擴充方法，將它新增至中的介軟體`IApplicationBuilder`相依性插入會負責。

   這並不限於`IOptions`物件。 如此一來，可插入中介軟體所需要的任何其他物件。

## <a name="loading-middleware-options-through-direct-injection"></a>正在載入透過直接插入中介軟體選項

「 選項 」 模式的優點是，它會建立鬆散耦合的選項值和其取用者之間。 一旦您已建立的選項類別關聯的實際選項值，任何其他類別可以取得存取權透過相依性插入架構的選項。 就不需要以傳遞選項的值。

這會細分不過如果您想要使用不同選項兩次，使用相同的中介軟體。 例如授權中介軟體可讓不同角色的不同分支中使用。 您無法將兩個不同的選項物件關聯的一個選項類別。

解決方法是取得使用中的實際選項值的選項物件您`Startup`類別，並將它們直接加入中介軟體的每個執行個體。

1. 新增第二個碼*appsettings.json*

   若要新增第二組選項，以*appsettings.json*檔案，請使用新的金鑰來唯一識別它：

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. 擷取選項值，並將它們傳遞給中介軟體。 `Use...` （其會新增至管線的中介軟體） 的擴充方法是傳入的選項值的邏輯位置： 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. 啟用中介軟體来採取的 options 參數。 提供的多載`Use...`擴充方法 (採用的 options 參數，並將它傳遞給`UseMiddleware`)。 當`UseMiddleware`稱為使用參數時，它會將參數傳遞至您的中介軟體建構函式中介軟體物件具現化時。

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   請注意這會在將選項物件的包裝`OptionsWrapper`物件。 這會實作`IOptions`，如預期般的中介軟體建構函式。

## <a name="migrating-to-the-new-httpcontext"></a>移轉至新的 HttpContext

您稍早所見，`Invoke`方法中介軟體中的使用的型別參數`HttpContext`:

```csharp
public async Task Invoke(HttpContext context)
```

`HttpContext` 已大幅改變 ASP.NET Core 中。 本節說明如何將最常使用的屬性轉譯[System.Web.HttpContext](/dotnet/api/system.web.httpcontext)新`Microsoft.AspNetCore.Http.HttpContext`。

### <a name="httpcontext"></a>HttpContext

**HttpContext.Items**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

**唯一的要求識別碼 （沒有 System.Web.HttpContext 對應項目）**

針對每個要求，提供您的唯一識別碼。 要包含在您的記錄檔中非常有用。

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a>HttpContext.Request

**HttpContext.Request.HttpMethod**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

**HttpContext.Request.QueryString**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

**HttpContext.Request.Url**並**HttpContext.Request.RawUrl**轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

**HttpContext.Request.IsSecureConnection**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

**HttpContext.Request.UserHostAddress**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

**HttpContext.Request.Cookies**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

**HttpContext.Request.RequestContext.RouteData**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

**HttpContext.Request.Headers**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

**HttpContext.Request.UserAgent**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

**HttpContext.Request.UrlReferrer**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

**HttpContext.Request.ContentType**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

**HttpContext.Request.Form**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> 讀取表單值的內容的子型別才*x-www-表單-urlencoded*或是*表單資料*。

**HttpContext.Request.InputStream**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> 此程式碼只能用在處理常式型別中介軟體，在管線結尾處。
>
>每個要求一次如上所示，您可以閱讀原始的主體。 嘗試讀取主體，在第一次讀取之後的中介軟體會讀取空白主體。
>
>這不適用於讀取表單，如先前所示因為完成從緩衝區。

### <a name="httpcontextresponse"></a>HttpContext.Response

**HttpContext.Response.Status**並**HttpContext.Response.StatusDescription**轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

**HttpContext.Response.ContentEncoding**並**HttpContext.Response.ContentType**轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

**HttpContext.Response.ContentType**上自己也會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

**HttpContext.Response.Output**會轉譯成：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

**HttpContext.Response.TransmitFile**

提供的檔案會討論[此處](../fundamentals/request-features.md#middleware-and-request-features)。

**HttpContext.Response.Headers**

傳送回應標頭複雜，原因，如果您設定它們的任何項目寫入至回應主體之後，它們將不會傳送。

解決方法是將設定寫入回應啟動之前將會直接呼叫的回呼方法。 最好的做法是在開頭`Invoke`中介軟體中的方法。 它是這個回呼方法，以設定您的回應標頭。

下列程式碼設定呼叫的回呼方法`SetHeaders`:

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetHeaders`回呼方法會看起來像這樣：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

**HttpContext.Response.Cookies**

在瀏覽器的 cookie 移動*Set-cookie*回應標頭。 傳送回應標頭，將傳送 cookie 需要如此一來，所用相同的回呼：

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

`SetCookies`回呼方法看起來如下所示：

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a>其他資源

* [HTTP 處理常式和 HTTP 模組概觀](/iis/configuration/system.webserver/)
* [組態](xref:fundamentals/configuration/index)
* [應用程式啟動](xref:fundamentals/startup)
* [中介軟體](xref:fundamentals/middleware/index)
