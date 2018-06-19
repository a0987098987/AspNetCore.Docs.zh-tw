---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: 在 IIS 中的 OWIN 中介軟體整合管線 |Microsoft 文件
author: Praburaj
description: 本文將說明如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，如何設定管線事件 OMC 上並執行。 您應該...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 5df70c80084a32c5f61ac9288c8cdbfaaa47f124
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871489"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a><span data-ttu-id="c7267-104">IIS integrated 管線中的 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="c7267-104">OWIN Middleware in the IIS integrated pipeline</span></span>
====================
<span data-ttu-id="c7267-105">由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c7267-105">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="c7267-106">本文將說明如何執行 OWIN 中介軟體元件 (OMCs) 在 IIS 整合式管線中，如何設定管線事件 OMC 上並執行。</span><span class="sxs-lookup"><span data-stu-id="c7267-106">This article shows how to run OWIN middleware Components (OMCs) in the IIS integrated pipeline, and how to set the pipeline event an OMC runs on.</span></span> <span data-ttu-id="c7267-107">您應該檢閱[的專案 Katana 概觀](an-overview-of-project-katana.md)和[OWIN 啟動類別偵測](owin-startup-class-detection.md)閱讀本教學課程之前。</span><span class="sxs-lookup"><span data-stu-id="c7267-107">You should review [An Overview of Project Katana](an-overview-of-project-katana.md) and [OWIN Startup Class Detection](owin-startup-class-detection.md) before reading this tutorial.</span></span> <span data-ttu-id="c7267-108">本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Chris Ross、 Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。</span><span class="sxs-lookup"><span data-stu-id="c7267-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>


<span data-ttu-id="c7267-109">雖然[OWIN](an-overview-of-project-katana.md)中介軟體元件 (OMCs) 主要用於在伺服器無從驗證管線中執行，就可以在 IIS integrated 管線以及中執行 OMC (**傳統模式是*不*支援**)。</span><span class="sxs-lookup"><span data-stu-id="c7267-109">Although [OWIN](an-overview-of-project-katana.md) middleware components (OMCs) are primarily designed to run in a server-agnostic pipeline, it is possible to run an OMC in the IIS integrated pipeline as well (**classic mode is *not* supported**).</span></span> <span data-ttu-id="c7267-110">OMC 對 IIS integrated 管線搭配使用封裝管理員主控台 (PMC) 從安裝下列套件：</span><span class="sxs-lookup"><span data-stu-id="c7267-110">An OMC can be made to work in the IIS integrated pipeline by installing the following package from the Package Manager Console (PMC):</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

<span data-ttu-id="c7267-111">這表示，所有應用程式架構，即使尚無法執行 IIS 和 System.Web，外部可受益於現有的 OWIN 中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="c7267-111">This means that all application frameworks, even those that are not yet able to run outside of IIS and System.Web, can benefit from existing OWIN middleware components.</span></span> 

> [!NOTE]
> <span data-ttu-id="c7267-112">所有`Microsoft.Owin.Security.*`封裝隨附於 Visual Studio 2013 中新的身分識別系統 (例如： Cookie、 Microsoft 帳戶、 Google、 Facebook、 Twitter [Bearer Token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html)，OAuth 授權伺服器，JWT，Azure Active目錄中，與 Active directory 同盟服務） 為 OMCs，編寫，並可在自我裝載和 IIS 裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="c7267-112">All of the `Microsoft.Owin.Security.*` packages shipping with the new Identity System in Visual Studio 2013 (for example: Cookies, Microsoft Account, Google, Facebook, Twitter, [Bearer Token](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, Authorization server, JWT, Azure Active directory, and Active directory federation services) are authored as OMCs, and can be used in both self-hosted and IIS-hosted scenarios.</span></span>

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a><span data-ttu-id="c7267-113">OWIN 中介軟體會在 IIS Integrated 管線中執行的方式</span><span class="sxs-lookup"><span data-stu-id="c7267-113">How OWIN Middleware Executes in the IIS Integrated Pipeline</span></span>

<span data-ttu-id="c7267-114">OWIN 主控台應用程式，應用程式管線使用建置[啟動設定](owin-startup-class-detection.md)設定元件會使用加入的順序`IAppBuilder.Use`方法。</span><span class="sxs-lookup"><span data-stu-id="c7267-114">For OWIN console applications, the application pipeline built using the [startup configuration](owin-startup-class-detection.md) is set by the order the components are added using the `IAppBuilder.Use` method.</span></span> <span data-ttu-id="c7267-115">也就是說，OWIN 管線中的[Katana](an-overview-of-project-katana.md)執行階段將其註冊使用的順序處理 OMCs `IAppBuilder.Use`。</span><span class="sxs-lookup"><span data-stu-id="c7267-115">That is, the OWIN pipeline in the [Katana](an-overview-of-project-katana.md) runtime will process OMCs in the order they were registered using `IAppBuilder.Use`.</span></span> <span data-ttu-id="c7267-116">在 IIS integrated 管線要求管線組成[HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx)例如訂閱一組預先定義的管線事件[BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx)， [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)， [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx)等等。</span><span class="sxs-lookup"><span data-stu-id="c7267-116">In the IIS integrated pipeline the request pipeline consists of [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) subscribed to a pre-defined set of the pipeline events such as [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.</span></span>

<span data-ttu-id="c7267-117">如果我們比較的 OMC [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) ASP.NET 世界 OMC 必須註冊到正確的預先定義的管線的事件。</span><span class="sxs-lookup"><span data-stu-id="c7267-117">If we compare an OMC to that of an [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) in the ASP.NET world, an OMC must be registered to the correct pre-defined pipeline event.</span></span> <span data-ttu-id="c7267-118">例如，HttpModule`MyModule`會叫用時要求送入[AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)管線階段中：</span><span class="sxs-lookup"><span data-stu-id="c7267-118">For example, the HttpModule `MyModule` will get invoked when a request comes to the [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) stage in the pipeline:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

<span data-ttu-id="c7267-119">為了讓 OMC 參與此相同且以事件為基礎的執行順序[Katana](an-overview-of-project-katana.md)透過執行階段程式碼掃描[啟動設定](owin-startup-class-detection.md)中, 介軟體元件的每個訂閱整合的管線的事件。</span><span class="sxs-lookup"><span data-stu-id="c7267-119">In order for an OMC to participate in this same, event-based execution ordering, the [Katana](an-overview-of-project-katana.md) runtime code scans through the [startup configuration](owin-startup-class-detection.md) and subscribes each of the middleware components to an integrated pipeline event.</span></span> <span data-ttu-id="c7267-120">例如，下列 OMC 及註冊程式碼可讓您看到的中介軟體元件的預設事件註冊。</span><span class="sxs-lookup"><span data-stu-id="c7267-120">For example, the following OMC and registration code enables you to see the default event registration of middleware components.</span></span> <span data-ttu-id="c7267-121">(如需詳細指示，建立 OWIN 啟動類別時，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。)</span><span class="sxs-lookup"><span data-stu-id="c7267-121">(For more detailed instructions on creating an OWIN startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).)</span></span>

1. <span data-ttu-id="c7267-122">建立空 web 應用程式專案並將其命名**owin2**。</span><span class="sxs-lookup"><span data-stu-id="c7267-122">Create an empty web application project and name it **owin2**.</span></span>
2. <span data-ttu-id="c7267-123">從封裝管理員主控台 (PMC)，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c7267-123">From the Package Manager Console (PMC), run the following command:</span></span> 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. <span data-ttu-id="c7267-124">新增`OWIN Startup Class`並將其命名`Startup`。</span><span class="sxs-lookup"><span data-stu-id="c7267-124">Add an `OWIN Startup Class` and name it `Startup`.</span></span> <span data-ttu-id="c7267-125">產生的程式碼替換成的下列 （所做的變更會反白顯示）：</span><span class="sxs-lookup"><span data-stu-id="c7267-125">Replace the generated code with the following (the changes are highlighted):</span></span>  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. <span data-ttu-id="c7267-126">按 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7267-126">Hit F5 to run the app.</span></span>

<span data-ttu-id="c7267-127">啟動組態來設定具有三個中介軟體元件，前兩個顯示的診斷資訊及回應事件的最後一個 （和也顯示診斷資訊） 的管線。</span><span class="sxs-lookup"><span data-stu-id="c7267-127">The Startup configuration sets up a pipeline with three middleware components, the first two displaying diagnostic information and the last one responding to events (and also displaying diagnostic information).</span></span> <span data-ttu-id="c7267-128">`PrintCurrentIntegratedPipelineStage`方法會顯示在叫用此中介軟體的整合式的管線事件和訊息。</span><span class="sxs-lookup"><span data-stu-id="c7267-128">The `PrintCurrentIntegratedPipelineStage` method displays the integrated pipeline event this middleware is invoked on and a message.</span></span> <span data-ttu-id="c7267-129">[輸出] 視窗會顯示下列資訊：</span><span class="sxs-lookup"><span data-stu-id="c7267-129">The output windows displays the following:</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

<span data-ttu-id="c7267-130">Katana 執行階段對應每個 OWIN 中介軟體元件[PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)根據預設，它會對應到 IIS 管線事件[PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx)。</span><span class="sxs-lookup"><span data-stu-id="c7267-130">The Katana runtime mapped each of the OWIN middleware components to [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) by default, which corresponds to the IIS pipeline event [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).</span></span>

## <a name="stage-markers"></a><span data-ttu-id="c7267-131">階段標記</span><span class="sxs-lookup"><span data-stu-id="c7267-131">Stage Markers</span></span>

<span data-ttu-id="c7267-132">您可以利用執行管線的特定階段 OMCs`IAppBuilder UseStageMarker()`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="c7267-132">You can mark OMCs to execute at specific stages of the pipeline by using the `IAppBuilder UseStageMarker()` extension method.</span></span> <span data-ttu-id="c7267-133">若要在特定階段期間執行一組的中介軟體元件，請插入之後的最後一個元件的階段標誌是在註冊期間的集合。</span><span class="sxs-lookup"><span data-stu-id="c7267-133">To run a set of middleware components during a particular stage, insert a stage marker right after the last component is the set during registration.</span></span> <span data-ttu-id="c7267-134">在管線的哪個階段中，您可以執行中介軟體的規則，而且必須在執行順序元件 （稍後在本教學課程中的規則會說明）。</span><span class="sxs-lookup"><span data-stu-id="c7267-134">There are rules on which stage of the pipeline you can execute middleware and the order components must run (The rules are explained later in the tutorial).</span></span> <span data-ttu-id="c7267-135">新增`UseStageMarker`方法`Configuration`程式碼如下所示：</span><span class="sxs-lookup"><span data-stu-id="c7267-135">Add the `UseStageMarker` method to the `Configuration` code as shown below:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

<span data-ttu-id="c7267-136">`app.UseStageMarker(PipelineStage.Authenticate)`呼叫會設定 （在此情況下，我們有兩個診斷元件） 的所有先前註冊的中介軟體元件在管線的驗證階段上執行。</span><span class="sxs-lookup"><span data-stu-id="c7267-136">The `app.UseStageMarker(PipelineStage.Authenticate)` call configures all the previously registered middleware components (in this case, our two diagnostic components) to run on the authentication stage of the pipeline.</span></span> <span data-ttu-id="c7267-137">在上執行的最後一個中介軟體元件 （這會顯示診斷和回應的要求）`ResolveCache`階段 ( [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx)事件)。</span><span class="sxs-lookup"><span data-stu-id="c7267-137">The last middleware component (which displays diagnostics and responds to requests) will run on the `ResolveCache` stage (the [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) event).</span></span>

<span data-ttu-id="c7267-138">按 F5 執行應用程式。[輸出] 視窗顯示下列資訊：</span><span class="sxs-lookup"><span data-stu-id="c7267-138">Hit F5 to run the app.The output window shows the following:</span></span>

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a><span data-ttu-id="c7267-139">階段標記規則</span><span class="sxs-lookup"><span data-stu-id="c7267-139">Stage Marker Rules</span></span>

<span data-ttu-id="c7267-140">Owin 中介軟體元件 (OMC) 可以設定為在下列的 OWIN 管線階段事件時執行：</span><span class="sxs-lookup"><span data-stu-id="c7267-140">Owin middleware components (OMC) can be configured to run at the following OWIN pipeline stage events:</span></span>

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. <span data-ttu-id="c7267-141">根據預設，OMCs 執行在最後一個事件 (`PreHandlerExecute`)。</span><span class="sxs-lookup"><span data-stu-id="c7267-141">By default, OMCs run at the last event (`PreHandlerExecute`).</span></span> <span data-ttu-id="c7267-142">這就是為什麼我們第一個範例程式碼會顯示 「 PreExecuteRequestHandler"。</span><span class="sxs-lookup"><span data-stu-id="c7267-142">That's why our first example code displayed "PreExecuteRequestHandler".</span></span>
2. <span data-ttu-id="c7267-143">您可以使用`app.UseStageMarker`中所列的方法，以註冊 OWIN 管線的任何階段之前，執行 OMC`PipelineStage`列舉。</span><span class="sxs-lookup"><span data-stu-id="c7267-143">You can use the a `app.UseStageMarker` method to register a OMC to run earlier, at any stage of the OWIN pipeline listed in the `PipelineStage` enum.</span></span>
3. <span data-ttu-id="c7267-144">OWIN 管線的管線與 IIS 管線經過排序，因此呼叫`app.UseStageMarker`必須按照順序。</span><span class="sxs-lookup"><span data-stu-id="c7267-144">The OWIN pipeline and the IIS pipeline is ordered, therefore calls to `app.UseStageMarker` must be in order.</span></span> <span data-ttu-id="c7267-145">您無法將事件處理常式設定事件之前的最後一個事件以向`app.UseStageMarker`。</span><span class="sxs-lookup"><span data-stu-id="c7267-145">You cannot set the event handler to an event that precedes the last event registered with to `app.UseStageMarker`.</span></span> <span data-ttu-id="c7267-146">例如，*之後*呼叫：</span><span class="sxs-lookup"><span data-stu-id="c7267-146">For example, *after* calling:</span></span>

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   <span data-ttu-id="c7267-147">呼叫`app.UseStageMarker`傳遞`Authenticate`或`PostAuthenticate`不會接受，而且將會擲回任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c7267-147">calls to `app.UseStageMarker` passing `Authenticate` or `PostAuthenticate` will not be honored, and no exception will be thrown.</span></span> <span data-ttu-id="c7267-148">在最新的階段，其預設值是執行 OMCs `PreHandlerExecute`。</span><span class="sxs-lookup"><span data-stu-id="c7267-148">OMCs run at the latest stage, which by default is `PreHandlerExecute`.</span></span> <span data-ttu-id="c7267-149">階段標記用來使它們執行早。</span><span class="sxs-lookup"><span data-stu-id="c7267-149">The stage markers are used to make them to run earlier.</span></span> <span data-ttu-id="c7267-150">如果您指定順序階段標記，我們會四捨五入至較早的標記。</span><span class="sxs-lookup"><span data-stu-id="c7267-150">If you specify stage markers out of order, we round to the earlier marker.</span></span> <span data-ttu-id="c7267-151">換句話說，新增的階段標誌會說 「 不晚於階段 X 執行 」。</span><span class="sxs-lookup"><span data-stu-id="c7267-151">In other words, adding a stage marker says "Run no later than stage X".</span></span> <span data-ttu-id="c7267-152">在 OWIN 管線中加入後它們最早的階段標誌 OMC 的執行。</span><span class="sxs-lookup"><span data-stu-id="c7267-152">OMC's run at the earliest stage marker added after them in the OWIN pipeline.</span></span>
4. <span data-ttu-id="c7267-153">若要呼叫的最早階段`app.UseStageMarker`wins。</span><span class="sxs-lookup"><span data-stu-id="c7267-153">The earliest stage of calls to `app.UseStageMarker` wins.</span></span> <span data-ttu-id="c7267-154">例如，如果您切換的順序`app.UseStageMarker`呼叫我們先前的範例：</span><span class="sxs-lookup"><span data-stu-id="c7267-154">For example, if you switch the order of `app.UseStageMarker` calls from our previous example:</span></span>

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   <span data-ttu-id="c7267-155">[輸出] 視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="c7267-155">The output window will display:</span></span> 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   <span data-ttu-id="c7267-156">所有執行中 OMCs`AuthenticateRequest`階段，因為最後一個 OMC 向`Authenticate`事件，而`Authenticate`事件位於所有其他事件。</span><span class="sxs-lookup"><span data-stu-id="c7267-156">The OMCs all run in the `AuthenticateRequest` stage, because the last OMC registered with the `Authenticate` event, and the `Authenticate` event precedes all other events.</span></span>
