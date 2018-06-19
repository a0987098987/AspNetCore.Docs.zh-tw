---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: 了解 ASP.NET MVC 執行程序 |Microsoft 文件
author: microsoft
description: 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500727"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="44c72-103">了解 ASP.NET MVC 執行程序</span><span class="sxs-lookup"><span data-stu-id="44c72-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="44c72-104">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="44c72-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="44c72-105">了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="44c72-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="44c72-106">對 ASP.NET MVC 為基礎的 Web 應用程式要求先通過**UrlRoutingModule**物件，它是 HTTP 模組。</span><span class="sxs-lookup"><span data-stu-id="44c72-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="44c72-107">此模組會剖析該要求，並執行路由選取。</span><span class="sxs-lookup"><span data-stu-id="44c72-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="44c72-108">**UrlRoutingModule**物件會選取符合目前要求的第一個路由物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="44c72-109">(路由物件是實作的類別**RouteBase**，且通常是的執行個體**路由**類別。)如果沒有路由相符， **UrlRoutingModule**物件不做任何動作，並讓要求回到標準 ASP.NET 或 IIS 要求處理。</span><span class="sxs-lookup"><span data-stu-id="44c72-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="44c72-110">從所選取**路由**物件**UrlRoutingModule**物件取得**IRouteHandler**與其相關聯物件**路由**物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="44c72-111">一般來說，在 MVC 應用程式，這會是的執行個體**MvcRouteHandler**。</span><span class="sxs-lookup"><span data-stu-id="44c72-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="44c72-112">**IRouteHandler**執行個體建立**IHttpHandler**物件，並將它傳遞至**IHttpContext**物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="44c72-113">根據預設， **IHttpHandler** MVC 是執行個體**MvcHandler**物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="44c72-114">**MvcHandler**物件接著會選取最後會處理要求的控制站。</span><span class="sxs-lookup"><span data-stu-id="44c72-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="44c72-115">ASP.NET MVC Web 應用程式執行時在 IIS 7.0 中，沒有名稱的副檔名是必要的 MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="44c72-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="44c72-116">不過，在 IIS 6.0 中，處理常式需要您將.mvc 副檔名對應至 ASP.NET ISAPI DLL。</span><span class="sxs-lookup"><span data-stu-id="44c72-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="44c72-117">模組與處理常式是 ASP.NET MVC 架構的進入點。</span><span class="sxs-lookup"><span data-stu-id="44c72-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="44c72-118">使用者執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="44c72-118">They perform the following actions:</span></span>

- <span data-ttu-id="44c72-119">在 MVC Web 應用程式中選取適當的控制器。</span><span class="sxs-lookup"><span data-stu-id="44c72-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="44c72-120">取得特定控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="44c72-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="44c72-121">呼叫控制器的**Execute**方法。</span><span class="sxs-lookup"><span data-stu-id="44c72-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="44c72-122">以下列出 MVC Web 專案的執行階段：</span><span class="sxs-lookup"><span data-stu-id="44c72-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="44c72-123">接收應用程式的第一個要求</span><span class="sxs-lookup"><span data-stu-id="44c72-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="44c72-124">在 Global.asax 檔案中，**路由**物件加入至**RouteTable**物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="44c72-125">執行路由</span><span class="sxs-lookup"><span data-stu-id="44c72-125">Perform routing</span></span> 

    - <span data-ttu-id="44c72-126">**UrlRoutingModule**模組會使用第一個符合**路由**物件存放至**RouteTable**集合，以建立**RouteData**物件，然後用來建立**RequestContext** (**IHttpContext**) 物件。</span><span class="sxs-lookup"><span data-stu-id="44c72-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="44c72-127">建立 MVC 要求處理常式</span><span class="sxs-lookup"><span data-stu-id="44c72-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="44c72-128">**MvcRouteHandler**物件建立的執行個體**MvcHandler**類別，並將它傳遞至**RequestContext**執行個體。</span><span class="sxs-lookup"><span data-stu-id="44c72-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="44c72-129">建立控制站</span><span class="sxs-lookup"><span data-stu-id="44c72-129">Create controller</span></span> 

    - <span data-ttu-id="44c72-130">**MvcHandler**物件會使用**RequestContext**執行個體來識別**IControllerFactory**物件 (通常的執行個體**DefaultControllerFactory**類別) 來建立控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="44c72-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="44c72-131">執行控制器- **MvcHandler**執行個體會呼叫控制器 s **Execute**方法。</span><span class="sxs-lookup"><span data-stu-id="44c72-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="44c72-132">叫用動作</span><span class="sxs-lookup"><span data-stu-id="44c72-132">Invoke action</span></span> 

    - <span data-ttu-id="44c72-133">大部分的控制站繼承自**控制器**基底類別。</span><span class="sxs-lookup"><span data-stu-id="44c72-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="44c72-134">這樣做，請的控制站的**ControllerActionInvoker**與控制器相關聯的物件決定的動作方法的呼叫，控制器類別，然後呼叫該方法。</span><span class="sxs-lookup"><span data-stu-id="44c72-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="44c72-135">執行結果</span><span class="sxs-lookup"><span data-stu-id="44c72-135">Execute result</span></span> 

    - <span data-ttu-id="44c72-136">典型的動作方法可能會收到使用者輸入、 準備適當的回應資料，並藉由傳回的結果型別執行結果。</span><span class="sxs-lookup"><span data-stu-id="44c72-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="44c72-137">可以執行內建的結果類型包括下列： **ViewResult** （其中呈現檢視，是最常用的結果型別）， **RedirectToRouteResult**， **RedirectResult**， **ContentResult**， **JsonResult**，和**EmptyResult**。</span><span class="sxs-lookup"><span data-stu-id="44c72-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
