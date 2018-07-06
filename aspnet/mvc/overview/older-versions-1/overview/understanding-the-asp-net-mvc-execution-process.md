---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: 了解 ASP.NET MVC 執行程序 |Microsoft Docs
author: microsoft
description: 了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 34699c314eb862e91ecba8831c5092af9d47dc70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823731"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="f9f1a-103">了解 ASP.NET MVC 執行程序</span><span class="sxs-lookup"><span data-stu-id="f9f1a-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="f9f1a-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f9f1a-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f9f1a-105">了解 ASP.NET MVC framework 如何處理逐步瀏覽器要求。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="f9f1a-106">ASP.NET MVC 為基礎的 Web 應用程式的要求先通過**UrlRoutingModule**物件，也就是 HTTP 模組。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="f9f1a-107">此模組會剖析該要求，並執行路由選取。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="f9f1a-108">**UrlRoutingModule**物件會選取符合目前要求的第一個路由物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="f9f1a-109">(路由物件是類別可實作**RouteBase**，且通常是的執行個體**路由**類別。)如果沒有路由相符， **UrlRoutingModule**物件不執行任何動作，並讓要求回到標準 ASP.NET 或 IIS 要求處理。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="f9f1a-110">從所選**路由**物件， **UrlRoutingModule**物件取得**IRouteHandler**相關聯的物件**路由**物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="f9f1a-111">一般而言，在 MVC 應用程式，這會是的執行個體**MvcRouteHandler**。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="f9f1a-112">**IRouteHandler**執行個體會建立**IHttpHandler**物件，並將它傳遞**IHttpContext**物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="f9f1a-113">根據預設， **IHttpHandler**執行個體為 MVC **MvcHandler**物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="f9f1a-114">**MvcHandler**物件接著會選取最後會處理要求的控制器。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="f9f1a-115">在 IIS 7.0 中，執行的 ASP.NET MVC Web 應用程式，無副檔名時需要的 MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="f9f1a-116">不過，在 IIS 6.0 中，處理常式需要您將.mvc 副檔名對應至 ASP.NET ISAPI DLL。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="f9f1a-117">模組與處理常式是 ASP.NET MVC 架構的進入點。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="f9f1a-118">執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="f9f1a-118">They perform the following actions:</span></span>

- <span data-ttu-id="f9f1a-119">MVC Web 應用程式中選取適當的控制器。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="f9f1a-120">取得特定控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="f9f1a-121">呼叫控制器的**Execute**方法。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="f9f1a-122">以下列出 MVC Web 專案執行階段：</span><span class="sxs-lookup"><span data-stu-id="f9f1a-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="f9f1a-123">接收應用程式的第一個要求</span><span class="sxs-lookup"><span data-stu-id="f9f1a-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="f9f1a-124">在 Global.asax 檔案中，**路由**物件加入至**RouteTable**物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="f9f1a-125">執行路由</span><span class="sxs-lookup"><span data-stu-id="f9f1a-125">Perform routing</span></span> 

    - <span data-ttu-id="f9f1a-126">**UrlRoutingModule**模組使用的第一個符合**路由**物件**RouteTable**來建立集合**RouteData**物件，然後使用建立該**RequestContext** (**IHttpContext**) 物件。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="f9f1a-127">建立 MVC 要求處理常式</span><span class="sxs-lookup"><span data-stu-id="f9f1a-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="f9f1a-128">**MvcRouteHandler**物件建立的執行個體**MvcHandler**類別，並將它傳遞**RequestContext**執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="f9f1a-129">建立控制器</span><span class="sxs-lookup"><span data-stu-id="f9f1a-129">Create controller</span></span> 

    - <span data-ttu-id="f9f1a-130">**MvcHandler**物件會使用**RequestContext**執行個體，以識別**IControllerFactory**物件 (通常是的執行個體**DefaultControllerFactory**類別) 來建立控制器執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="f9f1a-131">執行控制器- **MvcHandler**執行個體會呼叫控制器 s **Execute**方法。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="f9f1a-132">叫用動作</span><span class="sxs-lookup"><span data-stu-id="f9f1a-132">Invoke action</span></span> 

    - <span data-ttu-id="f9f1a-133">大部分的控制站會繼承**控制器**基底類別。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="f9f1a-134">控制站，這樣做，請**ControllerActionInvoker**與控制器相關聯的物件決定的動作方法的呼叫，控制器類別，然後呼叫該方法。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="f9f1a-135">執行結果</span><span class="sxs-lookup"><span data-stu-id="f9f1a-135">Execute result</span></span> 

    - <span data-ttu-id="f9f1a-136">典型的動作方法可能會接收使用者輸入、 準備適當的回應資料，並藉由傳回的結果型別執行結果。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="f9f1a-137">可以執行的內建結果型別包括下列： **ViewResult** （其中將檢視呈現並是最常用的結果型別）， **RedirectToRouteResult**， **RedirectResult**， **ContentResult**， **JsonResult**，和**EmptyResult**。</span><span class="sxs-lookup"><span data-stu-id="f9f1a-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
