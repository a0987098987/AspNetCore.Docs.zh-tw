---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: "ASP.NET Web API 2 中的追蹤 |Microsoft 文件"
author: MikeWasson
description: "示範如何在 ASP.NET Web API 中啟用追蹤。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="f31d6-103">ASP.NET Web API 2 中的追蹤</span><span class="sxs-lookup"><span data-stu-id="f31d6-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f31d6-104">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f31d6-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="f31d6-105">當您嘗試偵錯 web 應用程式時，是沒有替代的一組良好的追蹤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f31d6-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="f31d6-106">本教學課程會示範如何在 ASP.NET Web API 中啟用追蹤。</span><span class="sxs-lookup"><span data-stu-id="f31d6-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="f31d6-107">您可以使用這項功能，來追蹤 Web API framework 功能之前和之後叫用您的控制器。</span><span class="sxs-lookup"><span data-stu-id="f31d6-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="f31d6-108">您也可以使用它來追蹤您自己的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f31d6-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f31d6-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="f31d6-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f31d6-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) （也可用於 Visual Studio 2015）</span><span class="sxs-lookup"><span data-stu-id="f31d6-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="f31d6-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="f31d6-111">Web API 2</span></span>
> - [<span data-ttu-id="f31d6-112">用於 Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="f31d6-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="f31d6-113">啟用 System.Diagnostics 中 Web API 追蹤</span><span class="sxs-lookup"><span data-stu-id="f31d6-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="f31d6-114">首先，我們將建立新的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="f31d6-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="f31d6-115">在 Visual Studio 中，從**檔案**功能表上，選取**新增**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="f31d6-116">在下**範本**， **Web**，選取**ASP.NET Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f31d6-117">選擇 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="f31d6-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="f31d6-118">從**工具**功能表上，選取**程式庫套件管理員**，然後**套件管理主控台**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="f31d6-119">在 [封裝管理員主控台] 視窗中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="f31d6-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="f31d6-120">第一個命令會安裝最新的 Web API 追蹤封裝。</span><span class="sxs-lookup"><span data-stu-id="f31d6-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="f31d6-121">它也會更新核心 Web 應用程式開發介面封裝的封裝。</span><span class="sxs-lookup"><span data-stu-id="f31d6-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="f31d6-122">第二個命令將 WebApi.WebHost 套件更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="f31d6-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="f31d6-123">如果您想要以特定版本的 Web API 為目標，使用當您安裝追蹤封裝-版本旗標。</span><span class="sxs-lookup"><span data-stu-id="f31d6-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="f31d6-124">在應用程式中開啟檔案 WebApiConfig.cs\_開始資料夾。</span><span class="sxs-lookup"><span data-stu-id="f31d6-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="f31d6-125">將下列程式碼加入**註冊**方法。</span><span class="sxs-lookup"><span data-stu-id="f31d6-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="f31d6-126">這個程式碼加入[SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Web API 管線的類別。</span><span class="sxs-lookup"><span data-stu-id="f31d6-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="f31d6-127">**SystemDiagnosticsTraceWriter**類別會將寫入追蹤[System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace)。</span><span class="sxs-lookup"><span data-stu-id="f31d6-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="f31d6-128">若要查看追蹤，請在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f31d6-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="f31d6-129">在瀏覽器中，瀏覽至`/api/values`。</span><span class="sxs-lookup"><span data-stu-id="f31d6-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="f31d6-130">在 Visual Studio 中的 [輸出] 視窗會寫入追蹤陳述式。</span><span class="sxs-lookup"><span data-stu-id="f31d6-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="f31d6-131">(從**檢視**功能表上，選取**輸出**)。</span><span class="sxs-lookup"><span data-stu-id="f31d6-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="f31d6-132">因為**SystemDiagnosticsTraceWriter**寫入追蹤**System.Diagnostics.Trace**，您可以註冊其他追蹤接聽項; 例如，要寫入追蹤記錄檔。</span><span class="sxs-lookup"><span data-stu-id="f31d6-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="f31d6-133">如需有關追蹤寫入器的詳細資訊，請參閱[追蹤接聽項](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx)MSDN 上的主題。</span><span class="sxs-lookup"><span data-stu-id="f31d6-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="f31d6-134">設定的 SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="f31d6-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="f31d6-135">下列程式碼會示範如何設定追蹤寫入器。</span><span class="sxs-lookup"><span data-stu-id="f31d6-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="f31d6-136">有兩個您可以控制的設定：</span><span class="sxs-lookup"><span data-stu-id="f31d6-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="f31d6-137">IsVerbose： 如果為 false，每個追蹤中包含最少資訊。</span><span class="sxs-lookup"><span data-stu-id="f31d6-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="f31d6-138">如果為 true，則追蹤會包含更多資訊。</span><span class="sxs-lookup"><span data-stu-id="f31d6-138">If true, traces include more information.</span></span>
- <span data-ttu-id="f31d6-139">MinimumLevel： 設定最低追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="f31d6-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="f31d6-140">追蹤層級，依照順序，是偵錯、 資訊、 警告、 錯誤和嚴重錯誤。</span><span class="sxs-lookup"><span data-stu-id="f31d6-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="f31d6-141">將追蹤加入至您的 Web API 應用程式</span><span class="sxs-lookup"><span data-stu-id="f31d6-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="f31d6-142">將追蹤寫入器可讓您立即存取 Web API 管線所建立的追蹤。</span><span class="sxs-lookup"><span data-stu-id="f31d6-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="f31d6-143">您也可以使用追蹤寫入器來追蹤您自己的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f31d6-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="f31d6-144">若要取得追蹤寫入器，呼叫**HttpConfiguration.Services.GetTraceWriter**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="f31d6-145">在控制站，這個方法是透過可存取**ApiController.Configuration**屬性。</span><span class="sxs-lookup"><span data-stu-id="f31d6-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="f31d6-146">若要撰寫追蹤，您可以呼叫**ITraceWriter.Trace**方法直接，但[ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx)類別定義會更好記某些擴充方法。</span><span class="sxs-lookup"><span data-stu-id="f31d6-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="f31d6-147">例如，**資訊**如上所示的方法建立的追蹤使用追蹤層級**資訊**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="f31d6-148">Web API 追蹤基礎結構</span><span class="sxs-lookup"><span data-stu-id="f31d6-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="f31d6-149">本節說明如何撰寫 Web API 的自訂追蹤寫入器。</span><span class="sxs-lookup"><span data-stu-id="f31d6-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="f31d6-150">用於 Microsoft.AspNet.WebApi.Tracing 套件的建置 Web 應用程式開發介面中的多個一般追蹤基礎結構的頂端。</span><span class="sxs-lookup"><span data-stu-id="f31d6-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="f31d6-151">不使用用於 Microsoft.AspNet.WebApi.Tracing，您可以也插入其他除掉，然後再追蹤/程式庫，例如[NLog](http://nlog-project.org/)或[log4net](http://logging.apache.org/log4net/)。</span><span class="sxs-lookup"><span data-stu-id="f31d6-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="f31d6-152">若要收集追蹤，實作**ITraceWriter**介面。</span><span class="sxs-lookup"><span data-stu-id="f31d6-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="f31d6-153">以下是簡單的範例：</span><span class="sxs-lookup"><span data-stu-id="f31d6-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="f31d6-154">**ITraceWriter.Trace**方法會建立追蹤。</span><span class="sxs-lookup"><span data-stu-id="f31d6-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="f31d6-155">呼叫端指定類別目錄 」 和 「 追蹤層級。</span><span class="sxs-lookup"><span data-stu-id="f31d6-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="f31d6-156">類別可以是任何使用者定義的字串。</span><span class="sxs-lookup"><span data-stu-id="f31d6-156">The category can be any user-defined string.</span></span> <span data-ttu-id="f31d6-157">實作**追蹤**應該執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="f31d6-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="f31d6-158">建立新**TraceRecord**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="f31d6-159">使用初始化要求、 類別和追蹤層級，如下所示。</span><span class="sxs-lookup"><span data-stu-id="f31d6-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="f31d6-160">呼叫端會提供這些值。</span><span class="sxs-lookup"><span data-stu-id="f31d6-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="f31d6-161">叫用*traceAction*委派。</span><span class="sxs-lookup"><span data-stu-id="f31d6-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="f31d6-162">在此委派，呼叫端必須填滿的其餘部分**TraceRecord**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="f31d6-163">寫入**TraceRecord**，使用任何您喜歡的記錄方法。</span><span class="sxs-lookup"><span data-stu-id="f31d6-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="f31d6-164">此處顯示的範例只會呼叫至**System.Diagnostics.Trace**。</span><span class="sxs-lookup"><span data-stu-id="f31d6-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="f31d6-165">設定追蹤寫入器</span><span class="sxs-lookup"><span data-stu-id="f31d6-165">Setting the Trace Writer</span></span>

<span data-ttu-id="f31d6-166">若要啟用追蹤，您必須設定 Web API 來使用您**ITraceWriter**實作。</span><span class="sxs-lookup"><span data-stu-id="f31d6-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="f31d6-167">您透過**HttpConfiguration**物件，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="f31d6-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="f31d6-168">只有一個追蹤寫入器可以使用。</span><span class="sxs-lookup"><span data-stu-id="f31d6-168">Only one trace writer can be active.</span></span> <span data-ttu-id="f31d6-169">根據預設，Web API 設定&quot;無作業&quot;不做任何動作的追蹤。</span><span class="sxs-lookup"><span data-stu-id="f31d6-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="f31d6-170">(&quot;無作業&quot;追蹤存在，因此不需要檢查追蹤寫入器是否追蹤程式碼**null**之前寫入追蹤。)</span><span class="sxs-lookup"><span data-stu-id="f31d6-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="f31d6-171">Web API 追蹤的運作</span><span class="sxs-lookup"><span data-stu-id="f31d6-171">How Web API Tracing Works</span></span>

<span data-ttu-id="f31d6-172">追蹤中的 Web API 所使用的 Web 應用程式開發介面使用*外觀*模式： 當啟用追蹤時，Web 應用程式開發介面包裝要求管線包含執行追蹤呼叫類別的各種組件。</span><span class="sxs-lookup"><span data-stu-id="f31d6-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="f31d6-173">例如，當選取控制站，管線會使用**IHttpControllerSelector**介面。</span><span class="sxs-lookup"><span data-stu-id="f31d6-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="f31d6-174">利用啟用的追蹤，pipleline 插入實作的類別**IHttpControllerSelector**但呼叫傳遞給實際的實作：</span><span class="sxs-lookup"><span data-stu-id="f31d6-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Web API 追蹤會使用外觀樣式。](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="f31d6-176">這種設計的優點包括：</span><span class="sxs-lookup"><span data-stu-id="f31d6-176">The benefits of this design include:</span></span>

- <span data-ttu-id="f31d6-177">如果您不需要新增追蹤寫入器，追蹤元件未具現化，並不造成任何效能影響。</span><span class="sxs-lookup"><span data-stu-id="f31d6-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="f31d6-178">如果您取代預設的服務，例如**IHttpControllerSelector**您自己自訂的實作，追蹤受到影響，因為追蹤必須由包裝函式物件完成。</span><span class="sxs-lookup"><span data-stu-id="f31d6-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="f31d6-179">您也可以取代您自己自訂的架構，整個 Web API 追蹤架構，來取代預設**ITraceManager**服務：</span><span class="sxs-lookup"><span data-stu-id="f31d6-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="f31d6-180">實作**ITraceManager.Initialize**初始化追蹤系統。</span><span class="sxs-lookup"><span data-stu-id="f31d6-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="f31d6-181">請注意這會取代*整個*追蹤架構，包括所有內建 Web API 的追蹤程式碼。</span><span class="sxs-lookup"><span data-stu-id="f31d6-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
