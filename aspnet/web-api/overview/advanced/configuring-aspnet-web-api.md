---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: 設定 ASP.NET Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8fd08098b5a425f2cbd7939f5f90550b98c34071
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387073"
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="bcc5d-102">設定 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bcc5d-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bcc5d-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bcc5d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bcc5d-104">本主題描述如何設定 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="bcc5d-105">組態設定</span><span class="sxs-lookup"><span data-stu-id="bcc5d-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="bcc5d-106">設定與 ASP.NET 裝載的 Web API</span><span class="sxs-lookup"><span data-stu-id="bcc5d-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="bcc5d-107">設定 Web API 與 OWIN 自我裝載</span><span class="sxs-lookup"><span data-stu-id="bcc5d-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="bcc5d-108">全域的 Web API 服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="bcc5d-109">每個控制站設定</span><span class="sxs-lookup"><span data-stu-id="bcc5d-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="bcc5d-110">組態設定</span><span class="sxs-lookup"><span data-stu-id="bcc5d-110">Configuration Settings</span></span>

<span data-ttu-id="bcc5d-111">Web API 組態設定中定義[HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="bcc5d-112">成員</span><span class="sxs-lookup"><span data-stu-id="bcc5d-112">Member</span></span> | <span data-ttu-id="bcc5d-113">描述</span><span class="sxs-lookup"><span data-stu-id="bcc5d-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bcc5d-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-114">**DependencyResolver**</span></span> | <span data-ttu-id="bcc5d-115">可讓控制站的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="bcc5d-116">請參閱[使用 Web API 的相依性解析程式](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="bcc5d-117">**篩選**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-117">**Filters**</span></span> | <span data-ttu-id="bcc5d-118">動作篩選條件。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-118">Action filters.</span></span> |
| <span data-ttu-id="bcc5d-119">**格式器**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-119">**Formatters**</span></span> | <span data-ttu-id="bcc5d-120">[媒體類型格式器](../formats-and-model-binding/media-formatters.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="bcc5d-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="bcc5d-122">指定伺服器是否應該在 HTTP 回應訊息中包含錯誤詳細資料，例如例外狀況訊息和堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="bcc5d-123">請參閱[IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108))。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="bcc5d-124">**初始設定式**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-124">**Initializer**</span></span> | <span data-ttu-id="bcc5d-125">執行最後的初始化函式**HttpConfiguration**。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="bcc5d-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-126">**MessageHandlers**</span></span> | <span data-ttu-id="bcc5d-127">[HTTP 訊息處理常式](http-message-handlers.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="bcc5d-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="bcc5d-129">用於控制器動作上的繫結參數的規則集合。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="bcc5d-130">**屬性**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-130">**Properties**</span></span> | <span data-ttu-id="bcc5d-131">泛型屬性包。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-131">A generic property bag.</span></span> |
| <span data-ttu-id="bcc5d-132">**路由**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-132">**Routes**</span></span> | <span data-ttu-id="bcc5d-133">路由的集合。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-133">The collection of routes.</span></span> <span data-ttu-id="bcc5d-134">請參閱[ASP.NET Web API 中的路由](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="bcc5d-135">**服務**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-135">**Services**</span></span> | <span data-ttu-id="bcc5d-136">服務集合。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-136">The collection of services.</span></span> <span data-ttu-id="bcc5d-137">請參閱[Services](#services)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="bcc5d-138">必要條件</span><span class="sxs-lookup"><span data-stu-id="bcc5d-138">Prerequisites</span></span>

<span data-ttu-id="bcc5d-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、 Professional 或 Enterprise Edition。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="bcc5d-140">設定與 ASP.NET 裝載的 Web API</span><span class="sxs-lookup"><span data-stu-id="bcc5d-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="bcc5d-141">在 ASP.NET 應用程式，請藉由呼叫中設定 Web API [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx)中**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="bcc5d-142">**設定**方法會接受具有單一參數型別的委派**HttpConfiguration**。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="bcc5d-143">執行所有您在委派內的組態。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="bcc5d-144">以下是使用匿名委派的範例：</span><span class="sxs-lookup"><span data-stu-id="bcc5d-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="bcc5d-145">在 Visual Studio 2017 中，「 ASP.NET Web 應用程式 」 專案範本會自動設定的組態程式碼中，如果您選取 Web API 」 中**新的 ASP.NET 專案**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="bcc5d-146">專案範本會建立名為 「 應用程式的 WebApiConfig.cs 檔案\_開始資料夾。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="bcc5d-147">這個程式碼檔會定義應放置您的 Web API 組態程式碼的委派。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="bcc5d-148">專案範本也會將加入程式碼呼叫的委派**應用程式\_啟動**。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="bcc5d-149">設定 Web API 與 OWIN 自我裝載</span><span class="sxs-lookup"><span data-stu-id="bcc5d-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="bcc5d-150">如果您是使用 OWIN 自我裝載，建立新**HttpConfiguration**執行個體。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="bcc5d-151">在此情況下，執行任何設定，然後將傳遞至執行個體**Owin.UseWebApi**擴充方法。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="bcc5d-152">本教學課程[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)顯示完成的步驟。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="bcc5d-153">全域的 Web API 服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-153">Global Web API Services</span></span>

<span data-ttu-id="bcc5d-154">**HttpConfiguration.Services**集合包含一組 Web API 用來執行各種工作，例如控制器選取項目和內容交涉的全域服務。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="bcc5d-155">**Services**集合不是服務探索或相依性插入的一般用途機制。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="bcc5d-156">它只會儲存 Web API 架構已知的服務類型。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="bcc5d-157">**Services**集合已初始化與一組預設的服務，而且您可以提供您自己的自訂實作。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="bcc5d-158">某些服務會支援多個執行個體，而有些則可以有只有一個執行個體。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="bcc5d-159">(不過，您也可以提供在控制器層級的服務，請參閱[每個控制站設定](#percontrollerconfig)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="bcc5d-160">單一執行個體的服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-160">Single-Instance Services</span></span>


| <span data-ttu-id="bcc5d-161">服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-161">Service</span></span> | <span data-ttu-id="bcc5d-162">描述</span><span class="sxs-lookup"><span data-stu-id="bcc5d-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bcc5d-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-163">**IActionValueBinder**</span></span> | <span data-ttu-id="bcc5d-164">取得參數繫結。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="bcc5d-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-165">**IApiExplorer**</span></span> | <span data-ttu-id="bcc5d-166">取得描述元的應用程式所公開的 Api。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="bcc5d-167">請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="bcc5d-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="bcc5d-169">取得應用程式的組件清單。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="bcc5d-170">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="bcc5d-172">驗證的模型，會從要求主體讀取媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="bcc5d-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-173">**IContentNegotiator**</span></span> | <span data-ttu-id="bcc5d-174">執行內容交涉。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="bcc5d-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="bcc5d-176">提供 Api 的文件。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-176">Provides documentation for APIs.</span></span> <span data-ttu-id="bcc5d-177">預設值是**null**。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-177">The default is **null**.</span></span> <span data-ttu-id="bcc5d-178">請參閱[建立 Web API 說明頁面](../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="bcc5d-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="bcc5d-180">表示主機是否應緩衝 HTTP 訊息實體內文。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="bcc5d-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="bcc5d-182">叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-182">Invokes a controller action.</span></span> <span data-ttu-id="bcc5d-183">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="bcc5d-185">選取控制器動作。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-185">Selects a controller action.</span></span> <span data-ttu-id="bcc5d-186">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="bcc5d-188">啟動控制站。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-188">Activates a controller.</span></span> <span data-ttu-id="bcc5d-189">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="bcc5d-191">選取控制器。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-191">Selects a controller.</span></span> <span data-ttu-id="bcc5d-192">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="bcc5d-194">提供一份應用程式中的 Web API 控制器類型。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="bcc5d-195">請參閱[路由和動作選取](../web-api-routing-and-actions/routing-and-action-selection.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="bcc5d-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-196">**ITraceManager**</span></span> | <span data-ttu-id="bcc5d-197">初始化追蹤架構。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-197">Initializes the tracing framework.</span></span> <span data-ttu-id="bcc5d-198">請參閱[ASP.NET Web API 中追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="bcc5d-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-199">**ITraceWriter**</span></span> | <span data-ttu-id="bcc5d-200">提供的追蹤寫入器。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-200">Provides a trace writer.</span></span> <span data-ttu-id="bcc5d-201">預設為 「 無操作 」 追蹤寫入器。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="bcc5d-202">請參閱[ASP.NET Web API 中追蹤](../testing-and-debugging/tracing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="bcc5d-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="bcc5d-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="bcc5d-204">提供模型驗證程式的快取。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="bcc5d-205">多個執行個體的服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-205">Multiple-Instance Services</span></span>


|                 <span data-ttu-id="bcc5d-206">服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-206">Service</span></span>                 |                                                                                                              <span data-ttu-id="bcc5d-207">描述</span><span class="sxs-lookup"><span data-stu-id="bcc5d-207">Description</span></span>                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <span data-ttu-id="bcc5d-208"><strong>IFilterProvider</strong></span><span class="sxs-lookup"><span data-stu-id="bcc5d-208"><strong>IFilterProvider</strong></span></span>     |                                                                                           <span data-ttu-id="bcc5d-209">傳回控制器動作篩選條件的清單。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-209">Returns a list of filters for a controller action.</span></span>                                                                                           |
|  <span data-ttu-id="bcc5d-210"><strong>ModelBinderProvider</strong></span><span class="sxs-lookup"><span data-stu-id="bcc5d-210"><strong>ModelBinderProvider</strong></span></span>   |                                                                                                <span data-ttu-id="bcc5d-211">傳回指定型別的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-211">Returns a model binder for a given type.</span></span>                                                                                                |
| <span data-ttu-id="bcc5d-212"><strong>ModelMetadataProvider</strong></span><span class="sxs-lookup"><span data-stu-id="bcc5d-212"><strong>ModelMetadataProvider</strong></span></span>  |                                                                                                     <span data-ttu-id="bcc5d-213">提供模型中繼資料。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-213">Provides metadata for a model.</span></span>                                                                                                     |
| <span data-ttu-id="bcc5d-214"><strong>ModelValidatorProvider</strong></span><span class="sxs-lookup"><span data-stu-id="bcc5d-214"><strong>ModelValidatorProvider</strong></span></span> |                                                                                                   <span data-ttu-id="bcc5d-215">提供模型驗證程式。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-215">Provides a validator for a model.</span></span>                                                                                                    |
|  <span data-ttu-id="bcc5d-216"><strong>ValueProviderFactory</strong></span><span class="sxs-lookup"><span data-stu-id="bcc5d-216"><strong>ValueProviderFactory</strong></span></span>  | <span data-ttu-id="bcc5d-217">建立值提供者。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-217">Creates a value provider.</span></span> <span data-ttu-id="bcc5d-218">如需詳細資訊，請參閱 Mike Stall 的部落格文章[如何在 WebAPI 中建立的自訂值提供者](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="bcc5d-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |

<span data-ttu-id="bcc5d-219">若要加入多個執行個體服務的自訂實作，請呼叫**新增**或是**插入**上**Services**集合：</span><span class="sxs-lookup"><span data-stu-id="bcc5d-219">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="bcc5d-220">若要使用的自訂實作取代單一執行個體服務，請呼叫**取代**上**服務**集合：</span><span class="sxs-lookup"><span data-stu-id="bcc5d-220">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="bcc5d-221">每個控制站設定</span><span class="sxs-lookup"><span data-stu-id="bcc5d-221">Per-Controller Configuration</span></span>

<span data-ttu-id="bcc5d-222">您可以覆寫每個控制器為基礎的下列設定：</span><span class="sxs-lookup"><span data-stu-id="bcc5d-222">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="bcc5d-223">媒體類型格式器</span><span class="sxs-lookup"><span data-stu-id="bcc5d-223">Media-type formatters</span></span>
- <span data-ttu-id="bcc5d-224">參數繫結規則</span><span class="sxs-lookup"><span data-stu-id="bcc5d-224">Parameter binding rules</span></span>
- <span data-ttu-id="bcc5d-225">服務</span><span class="sxs-lookup"><span data-stu-id="bcc5d-225">Services</span></span>

<span data-ttu-id="bcc5d-226">若要這樣做，請定義自訂屬性，以實作**IControllerConfiguration**介面。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-226">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="bcc5d-227">然後將屬性套用至控制器中。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-227">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="bcc5d-228">下列範例會以自訂格式器取代預設媒體類型格式器。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-228">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="bcc5d-229">**IControllerConfiguration.Initialize**方法採用兩個參數：</span><span class="sxs-lookup"><span data-stu-id="bcc5d-229">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="bcc5d-230">**HttpControllerSettings**物件</span><span class="sxs-lookup"><span data-stu-id="bcc5d-230">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="bcc5d-231">**HttpControllerDescriptor**物件</span><span class="sxs-lookup"><span data-stu-id="bcc5d-231">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="bcc5d-232">**HttpControllerDescriptor**包含控制器，您可以檢查僅供參考之用 （例如，若要區別兩個控制站） 的描述。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-232">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="bcc5d-233">使用**HttpControllerSettings**設定控制站的物件。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-233">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="bcc5d-234">此物件包含您可以覆寫每個控制器為基礎的組態參數的子集。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-234">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="bcc5d-235">您不會變更任何設定預設為全域**HttpConfiguration**物件。</span><span class="sxs-lookup"><span data-stu-id="bcc5d-235">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
