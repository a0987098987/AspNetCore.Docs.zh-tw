---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: 啟用 ASP.NET Web API 2 中的跨原始來源要求 |Microsoft Docs
author: MikeWasson
description: 示範如何在 ASP.NET Web API 中支援跨原始資源共用 (CORS)。
ms.author: aspnetcontent
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7ac6158c2365aa324cefe97db044f568a1a43795
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805408"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="66518-103">啟用 ASP.NET Web API 2 中的跨原始來源要求</span><span class="sxs-lookup"><span data-stu-id="66518-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="66518-104">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="66518-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="66518-105">瀏覽器安全性可防止網頁對另一個網域提出 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="66518-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="66518-106">這項限制稱為*同源原則*，可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="66518-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="66518-107">不過，有時候您可能想要讓呼叫您的 web API 的其他站台。</span><span class="sxs-lookup"><span data-stu-id="66518-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="66518-108">[跨原始資源共用](http://www.w3.org/TR/cors/)(CORS) 是 W3C 標準，可讓伺服器放寬同源原則。</span><span class="sxs-lookup"><span data-stu-id="66518-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="66518-109">使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。</span><span class="sxs-lookup"><span data-stu-id="66518-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="66518-110">CORS 可較為安全且更有彈性，比早期技術這類[JSONP](http://en.wikipedia.org/wiki/JSONP)。</span><span class="sxs-lookup"><span data-stu-id="66518-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="66518-111">本教學課程會示範如何在您的 Web API 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="66518-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="66518-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="66518-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="66518-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="66518-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="66518-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="66518-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="66518-115">簡介</span><span class="sxs-lookup"><span data-stu-id="66518-115">Introduction</span></span>

<span data-ttu-id="66518-116">本教學課程會示範在 ASP.NET Web API 的 CORS 支援。</span><span class="sxs-lookup"><span data-stu-id="66518-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="66518-117">我們先建立兩個 ASP.NET 專案 – 一個稱為 「 web 服務 」，裝載的 Web API 控制器和其他呼叫 「 WebClient 」，它會呼叫 web 服務。</span><span class="sxs-lookup"><span data-stu-id="66518-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="66518-118">因為兩個應用程式裝載在不同網域時，web 服務的 AJAX 要求從 WebClient 會是跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="66518-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="66518-119">什麼是 「 相同原始 」？</span><span class="sxs-lookup"><span data-stu-id="66518-119">What is "Same Origin"?</span></span>

<span data-ttu-id="66518-120">如果它們有相同的配置、 主機和連接埠，兩個 Url 會有相同的原點。</span><span class="sxs-lookup"><span data-stu-id="66518-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="66518-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="66518-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="66518-122">這些兩個 Url 有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="66518-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="66518-123">這些 Url 會有兩個不同的來源，比上一個：</span><span class="sxs-lookup"><span data-stu-id="66518-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="66518-124">`http://example.net` -不同的網域</span><span class="sxs-lookup"><span data-stu-id="66518-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="66518-125">`http://example.com:9000/foo.html` -不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="66518-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="66518-126">`https://example.com/foo.html` -不同的配置</span><span class="sxs-lookup"><span data-stu-id="66518-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="66518-127">`http://www.example.com/foo.html` -不同的子網域</span><span class="sxs-lookup"><span data-stu-id="66518-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="66518-128">Internet Explorer 在比較來源時，不會考慮該連接埠。</span><span class="sxs-lookup"><span data-stu-id="66518-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="66518-129">建立 web 服務專案</span><span class="sxs-lookup"><span data-stu-id="66518-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="66518-130">本節假設您已經知道如何建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="66518-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="66518-131">如果沒有，請參閱[Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="66518-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="66518-132">啟動 Visual Studio 並建立新**ASP.NET Web 應用程式**專案。</span><span class="sxs-lookup"><span data-stu-id="66518-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="66518-133">選取 **空**專案範本。</span><span class="sxs-lookup"><span data-stu-id="66518-133">Select the **Empty** project template.</span></span> <span data-ttu-id="66518-134">在"新增資料夾和核心參考 」，選取**Web API**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="66518-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="66518-135">（選擇性） 選取 Mircosoft azure 中部署應用程式的 「 主應用程式在雲端 」 選項。</span><span class="sxs-lookup"><span data-stu-id="66518-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="66518-136">Microsoft 提供免費的 web 裝載最多 10 個網站中[免費 Azure 試用帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="66518-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="66518-137">新增名為 Web API 控制器`TestController`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="66518-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="66518-138">您可以在本機執行應用程式，或部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="66518-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="66518-139">（在本教學課程的螢幕擷取畫面，如我部署至 Azure App Service Web Apps。）若要確認 web API 會正常運作，巡覽至`http://hostname/api/test/`，其中*hostname*是您用來部署應用程式的網域。</span><span class="sxs-lookup"><span data-stu-id="66518-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="66518-140">回應文字，您應該會看到&quot;取得： 測試訊息&quot;。</span><span class="sxs-lookup"><span data-stu-id="66518-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="66518-141">建立 WebClient 專案</span><span class="sxs-lookup"><span data-stu-id="66518-141">Create the WebClient Project</span></span>

<span data-ttu-id="66518-142">建立 ASP.NET Web 應用程式的另一個專案，然後選取**MVC**專案範本。</span><span class="sxs-lookup"><span data-stu-id="66518-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="66518-143">（選擇性） 選取**變更驗證** > **不需要驗證**。</span><span class="sxs-lookup"><span data-stu-id="66518-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="66518-144">本教學課程中，您不需要驗證。</span><span class="sxs-lookup"><span data-stu-id="66518-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="66518-145">在方案總管 中，開啟檔案 Views/Home/Index.cshtml。</span><span class="sxs-lookup"><span data-stu-id="66518-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="66518-146">您可以將此檔案中的程式碼取代為下列：</span><span class="sxs-lookup"><span data-stu-id="66518-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="66518-147">針對*serviceUrl*變數中，使用 web 服務應用程式的 URI。</span><span class="sxs-lookup"><span data-stu-id="66518-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="66518-148">現在在本機執行 WebClient 應用程式，或將它發佈至另一個網站。</span><span class="sxs-lookup"><span data-stu-id="66518-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="66518-149">按一下 試試看 」 按鈕 AJAX 會將要求提交至 web 服務應用程式，使用中列出的 HTTP 方法 （GET、 POST 或 PUT） 的下拉式清單方塊。</span><span class="sxs-lookup"><span data-stu-id="66518-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="66518-150">這可讓我們檢查不同的跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="66518-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="66518-151">現在，web 服務應用程式不支援 CORS，因此如果您按一下按鈕時，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="66518-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="66518-152">如果您觀察在工具中的 HTTP 流量要[Fiddler](http://www.telerik.com/fiddler)，您會看到瀏覽器會傳送 GET 要求，並要求成功，但 AJAX 呼叫會傳回錯誤。</span><span class="sxs-lookup"><span data-stu-id="66518-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="66518-153">請務必了解同源原則不會防止從瀏覽器*傳送*要求。</span><span class="sxs-lookup"><span data-stu-id="66518-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="66518-154">相反地，它會防止應用程式看到*回應*。</span><span class="sxs-lookup"><span data-stu-id="66518-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="66518-155">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="66518-155">Enable CORS</span></span>

<span data-ttu-id="66518-156">現在讓我們的 web 服務應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="66518-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="66518-157">首先，新增 CORS NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="66518-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="66518-158">在 Visual Studio 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="66518-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="66518-159">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="66518-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="66518-160">此命令會安裝最新的封裝，並更新所有相依性，包括 core Web API 程式庫。</span><span class="sxs-lookup"><span data-stu-id="66518-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="66518-161">使用者-特定版本為目標的版本旗標。</span><span class="sxs-lookup"><span data-stu-id="66518-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="66518-162">CORS 封裝則需要 Web API 2.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="66518-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="66518-163">開啟檔案的應用程式\_Start/WebApiConfig.cs。</span><span class="sxs-lookup"><span data-stu-id="66518-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="66518-164">將下列程式碼加入**WebApiConfig.Register**方法。</span><span class="sxs-lookup"><span data-stu-id="66518-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="66518-165">接下來，新增 **[EnableCors]** 屬性設定為`TestController`類別：</span><span class="sxs-lookup"><span data-stu-id="66518-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="66518-166">針對*origins*參數，使用 WebClient 應用程式的部署位置的 URI。</span><span class="sxs-lookup"><span data-stu-id="66518-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="66518-167">這讓跨源要求 WebClient，同時仍然不允許所有其他的跨網域要求。</span><span class="sxs-lookup"><span data-stu-id="66518-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="66518-168">稍後，我將說明的參數 **[EnableCors]** 中更多詳細資料。</span><span class="sxs-lookup"><span data-stu-id="66518-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="66518-169">不包含結尾斜線*origins* URL。</span><span class="sxs-lookup"><span data-stu-id="66518-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="66518-170">重新部署更新的 web 服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="66518-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="66518-171">您不需要更新 WebClient。</span><span class="sxs-lookup"><span data-stu-id="66518-171">You don't need to update WebClient.</span></span> <span data-ttu-id="66518-172">現在應該會成功從 WebClient 的 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="66518-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="66518-173">所有允許的 GET、 PUT 和 POST 方法。</span><span class="sxs-lookup"><span data-stu-id="66518-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="66518-174">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="66518-174">How CORS Works</span></span>

<span data-ttu-id="66518-175">本章節描述 CORS 要求，在 HTTP 訊息的層級中發生的動作。</span><span class="sxs-lookup"><span data-stu-id="66518-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="66518-176">請務必了解 CORS 的運作方式，以便您可以設定 **[EnableCors]** 屬性是否正確，以及疑難排解如果無法運作如預期般。</span><span class="sxs-lookup"><span data-stu-id="66518-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="66518-177">CORS 規格引進了數個新的 HTTP 標頭啟用跨源要求。</span><span class="sxs-lookup"><span data-stu-id="66518-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="66518-178">如果瀏覽器支援 CORS，它會設定自動跨原始來源要求，這些標頭您不需要執行任何 JavaScript 程式碼中的特殊動作。</span><span class="sxs-lookup"><span data-stu-id="66518-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="66518-179">以下是跨原始要求的範例。</span><span class="sxs-lookup"><span data-stu-id="66518-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="66518-180">「 來源 」 標頭會提供提出要求的站台的網域。</span><span class="sxs-lookup"><span data-stu-id="66518-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="66518-181">如果伺服器允許的要求，它會設定存取控制-允許-原始標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="66518-182">此標頭的值符合 Origin 標頭，或者是萬用字元值"\*"，表示允許任何來源。</span><span class="sxs-lookup"><span data-stu-id="66518-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="66518-183">如果回應不包含存取控制-允許-原始標頭，AJAX 要求將會失敗。</span><span class="sxs-lookup"><span data-stu-id="66518-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="66518-184">具體而言，瀏覽器不允許要求。</span><span class="sxs-lookup"><span data-stu-id="66518-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="66518-185">即使伺服器會傳回成功的回應，瀏覽器不會回應提供用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="66518-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="66518-186">**預檢要求**</span><span class="sxs-lookup"><span data-stu-id="66518-186">**Preflight Requests**</span></span>

<span data-ttu-id="66518-187">對於某些 CORS 要求，瀏覽器會傳送其他要求，稱為 「 預檢要求，」 後才會傳送實際要求的資源。</span><span class="sxs-lookup"><span data-stu-id="66518-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="66518-188">如果下列條件成立，則瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="66518-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="66518-189">要求方法是 GET、 HEAD 或 POST*和*</span><span class="sxs-lookup"><span data-stu-id="66518-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="66518-190">應用程式不會設定 Accept、 Accept-language、 內容語言、 以外任何要求標頭內容類型或最後一個事件識別碼、*和*</span><span class="sxs-lookup"><span data-stu-id="66518-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="66518-191">Content-type 標頭 (如果設定) 是下列其中之一：</span><span class="sxs-lookup"><span data-stu-id="66518-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="66518-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="66518-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="66518-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="66518-193">multipart/form-data</span></span>
    - <span data-ttu-id="66518-194">text/plain</span><span class="sxs-lookup"><span data-stu-id="66518-194">text/plain</span></span>

<span data-ttu-id="66518-195">要求標頭的相關規則有套用到應用程式設定所呼叫的標頭**setRequestHeader**上**XMLHttpRequest**物件。</span><span class="sxs-lookup"><span data-stu-id="66518-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="66518-196">（CORS 規格會呼叫這些 「 作者要求標頭 」）。規則不適用於標頭*瀏覽器*設定，例如使用者代理程式、 主機或內容長度。</span><span class="sxs-lookup"><span data-stu-id="66518-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="66518-197">預檢要求的範例如下：</span><span class="sxs-lookup"><span data-stu-id="66518-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="66518-198">事前要求使用 HTTP OPTIONS; 方法。</span><span class="sxs-lookup"><span data-stu-id="66518-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="66518-199">它包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="66518-199">It includes two special headers:</span></span>

- <span data-ttu-id="66518-200">存取控制-要求的方法： 將實際的要求使用 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="66518-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="66518-201">存取控制-要求的標頭： 要求標頭的清單，*應用程式*實際要求上設定。</span><span class="sxs-lookup"><span data-stu-id="66518-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="66518-202">（同樣地，這不包括瀏覽器設定的標頭。）</span><span class="sxs-lookup"><span data-stu-id="66518-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="66518-203">以下是範例回應，假設伺服器允許的要求：</span><span class="sxs-lookup"><span data-stu-id="66518-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="66518-204">此回應包含存取控制-允許-方法標頭，其中列出允許的方法，並選擇性地存取控制-允許-標頭的標頭，它會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="66518-205">如果預檢要求成功，瀏覽器會傳送實際要求，如先前所述。</span><span class="sxs-lookup"><span data-stu-id="66518-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="66518-206">[EnableCors] 的範圍規則</span><span class="sxs-lookup"><span data-stu-id="66518-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="66518-207">您可以在應用程式中，每個動作，每個控制站，或適用於所有的 Web API 控制器啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="66518-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="66518-208">**每個動作**</span><span class="sxs-lookup"><span data-stu-id="66518-208">**Per Action**</span></span>

<span data-ttu-id="66518-209">若要啟用 CORS 的單一動作時，將 **[EnableCors]** 動作方法上的屬性。</span><span class="sxs-lookup"><span data-stu-id="66518-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="66518-210">下列範例會啟用 CORS`GetItem`僅方法。</span><span class="sxs-lookup"><span data-stu-id="66518-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="66518-211">**每個控制站**</span><span class="sxs-lookup"><span data-stu-id="66518-211">**Per Controller**</span></span>

<span data-ttu-id="66518-212">如果您設定 **[EnableCors]** 在控制器類別，它適用於控制站上的所有動作。</span><span class="sxs-lookup"><span data-stu-id="66518-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="66518-213">若要停用動作的 CORS，請新增 **[DisableCors]** 屬性的動作。</span><span class="sxs-lookup"><span data-stu-id="66518-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="66518-214">下列範例會針對每個方法，除了啟用 CORS `PutItem`。</span><span class="sxs-lookup"><span data-stu-id="66518-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="66518-215">**全域**</span><span class="sxs-lookup"><span data-stu-id="66518-215">**Globally**</span></span>

<span data-ttu-id="66518-216">若要啟用 CORS 的應用程式中的所有 Web API 控制器時，傳遞**EnableCorsAttribute**執行個體**EnableCors**方法：</span><span class="sxs-lookup"><span data-stu-id="66518-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="66518-217">如果您在一個以上的範圍設定的屬性，優先順序就會是：</span><span class="sxs-lookup"><span data-stu-id="66518-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="66518-218">動作</span><span class="sxs-lookup"><span data-stu-id="66518-218">Action</span></span>
2. <span data-ttu-id="66518-219">控制器</span><span class="sxs-lookup"><span data-stu-id="66518-219">Controller</span></span>
3. <span data-ttu-id="66518-220">Global</span><span class="sxs-lookup"><span data-stu-id="66518-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="66518-221">設定允許的來源</span><span class="sxs-lookup"><span data-stu-id="66518-221">Set the Allowed Origins</span></span>

<span data-ttu-id="66518-222">*Origins*的參數 **[EnableCors]** 屬性會指定哪些原始來源允許存取資源。</span><span class="sxs-lookup"><span data-stu-id="66518-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="66518-223">值是允許的來源的逗號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="66518-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="66518-224">您也可以使用萬用字元值"\*」 以允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="66518-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="66518-225">請仔細考慮，才能允許來自任何來源的要求。</span><span class="sxs-lookup"><span data-stu-id="66518-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="66518-226">這表示幾乎任何網站可以讓您的 web API 的 AJAX 呼叫。</span><span class="sxs-lookup"><span data-stu-id="66518-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="66518-227">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="66518-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="66518-228">*方法*的參數 **[EnableCors]** 屬性會指定哪些 HTTP 方法才能存取資源。</span><span class="sxs-lookup"><span data-stu-id="66518-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="66518-229">若要允許所有方法，使用萬用字元值"\*」。</span><span class="sxs-lookup"><span data-stu-id="66518-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="66518-230">下列範例允許只有 GET 和 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="66518-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="66518-231">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="66518-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="66518-232">稍早所述方式預檢要求可能會包含存取控制-access-control-request-headers 標標頭，列出應用程式設定的 HTTP 標頭 (所謂 「 撰寫要求標頭 」)。</span><span class="sxs-lookup"><span data-stu-id="66518-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="66518-233">*標頭*的參數 **[EnableCors]** 屬性可讓您指定允許哪些作者要求標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="66518-234">若要允許任何標頭，設定*標頭*到 「\*"。</span><span class="sxs-lookup"><span data-stu-id="66518-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="66518-235">允許清單特定的標頭，設定*標頭*允許的標頭以逗號分隔清單：</span><span class="sxs-lookup"><span data-stu-id="66518-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="66518-236">不過，瀏覽器並不會在它們如何設定存取控制-access-control-request-headers 標完全一致。</span><span class="sxs-lookup"><span data-stu-id="66518-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="66518-237">例如，Chrome 目前包含"origin";雖然 FireFox 不包含標準標頭，例如"Accept"，即使應用程式會設定它們的指令碼中。</span><span class="sxs-lookup"><span data-stu-id="66518-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="66518-238">如果您設定*標頭*到以外的任何項目"\*」，您應該至少包含在 「 accept 」，"content-type"、"origin"，以及您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="66518-239">設定允許的回應標頭</span><span class="sxs-lookup"><span data-stu-id="66518-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="66518-240">根據預設，瀏覽器不會公開所有應用程式的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="66518-241">是預設可用的回應標頭如下：</span><span class="sxs-lookup"><span data-stu-id="66518-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="66518-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="66518-242">Cache-Control</span></span>
- <span data-ttu-id="66518-243">內容語言</span><span class="sxs-lookup"><span data-stu-id="66518-243">Content-Language</span></span>
- <span data-ttu-id="66518-244">Content-Type</span><span class="sxs-lookup"><span data-stu-id="66518-244">Content-Type</span></span>
- <span data-ttu-id="66518-245">到期</span><span class="sxs-lookup"><span data-stu-id="66518-245">Expires</span></span>
- <span data-ttu-id="66518-246">上次修改</span><span class="sxs-lookup"><span data-stu-id="66518-246">Last-Modified</span></span>
- <span data-ttu-id="66518-247">Pragma</span><span class="sxs-lookup"><span data-stu-id="66518-247">Pragma</span></span>

<span data-ttu-id="66518-248">CORS 規格會呼叫這些[簡單的回應標頭](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="66518-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="66518-249">若要讓其他標頭可供應用程式，設定*exposedHeaders*的參數 **[EnableCors]**。</span><span class="sxs-lookup"><span data-stu-id="66518-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="66518-250">在下列範例中，控制器的`Get`方法會設定名為 'X 自訂-標頭' 的自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="66518-251">根據預設，瀏覽器不會公開此標頭中的跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="66518-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="66518-252">要使用的標頭，請納入 ' X-自訂-Header *exposedHeaders*。</span><span class="sxs-lookup"><span data-stu-id="66518-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="66518-253">跨原始來源要求中傳遞認證</span><span class="sxs-lookup"><span data-stu-id="66518-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="66518-254">認證需要在 CORS 要求的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="66518-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="66518-255">根據預設，瀏覽器不會傳送任何與跨原始要求的認證。</span><span class="sxs-lookup"><span data-stu-id="66518-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="66518-256">認證包含 cookie，以及 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="66518-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="66518-257">若要傳送與跨原始要求的認證，用戶端必須將**XMLHttpRequest.withCredentials**設為 true。</span><span class="sxs-lookup"><span data-stu-id="66518-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="66518-258">使用**XMLHttpRequest**直接：</span><span class="sxs-lookup"><span data-stu-id="66518-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="66518-259">在 jQuery 中：</span><span class="sxs-lookup"><span data-stu-id="66518-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="66518-260">此外，伺服器必須允許的認證。</span><span class="sxs-lookup"><span data-stu-id="66518-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="66518-261">若要在 Web API 中，允許跨原始來源的認證，請設定**SupportsCredentials**屬性設為 true，在 **[EnableCors]** 屬性：</span><span class="sxs-lookup"><span data-stu-id="66518-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="66518-262">如果這個屬性為 true，則 HTTP 回應將包含存取控制-允許-認證標頭。</span><span class="sxs-lookup"><span data-stu-id="66518-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="66518-263">此標頭會告知瀏覽器伺服器允許跨原始來源要求認證。</span><span class="sxs-lookup"><span data-stu-id="66518-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="66518-264">如果瀏覽器傳送認證，但回應不包含有效的存取控制-允許-認證標頭，瀏覽器不會公開應用程式、 回應和 AJAX 要求失敗。</span><span class="sxs-lookup"><span data-stu-id="66518-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="66518-265">請務必小心設定的相關**SupportsCredentials**為 true，因為這表示在另一個定義域的網站可以傳送給您的 Web API，代替使用者的登入的使用者的認證不會察覺使用者。</span><span class="sxs-lookup"><span data-stu-id="66518-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="66518-266">CORS 規格也會指出該設定*origins*要&quot; \* &quot;無效如果**SupportsCredentials**為 true。</span><span class="sxs-lookup"><span data-stu-id="66518-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="66518-267">自訂的 CORS 原則提供者</span><span class="sxs-lookup"><span data-stu-id="66518-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="66518-268">**[EnableCors]** 屬性會實作**ICorsPolicyProvider**介面。</span><span class="sxs-lookup"><span data-stu-id="66518-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="66518-269">您可以提供您自己的實作方式是建立衍生自類別**屬性**，並會實作**ICorsProlicyProvider**。</span><span class="sxs-lookup"><span data-stu-id="66518-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="66518-270">現在您可以套用的屬性的任何放置，您會放置 **[EnableCors]**。</span><span class="sxs-lookup"><span data-stu-id="66518-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="66518-271">例如，自訂的 CORS 原則提供者無法從組態檔讀取設定。</span><span class="sxs-lookup"><span data-stu-id="66518-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="66518-272">您可以使用屬性的替代方案，以註冊**ICorsPolicyProviderFactory**建立的物件**ICorsPolicyProvider**物件。</span><span class="sxs-lookup"><span data-stu-id="66518-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="66518-273">若要設定**ICorsPolicyProviderFactory**，呼叫**SetCorsPolicyProviderFactory**擴充方法，在啟動時，如下所示：</span><span class="sxs-lookup"><span data-stu-id="66518-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="66518-274">瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="66518-274">Browser Support</span></span>

<span data-ttu-id="66518-275">Web API CORS 套件是一種伺服器端技術。</span><span class="sxs-lookup"><span data-stu-id="66518-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="66518-276">使用者的瀏覽器也必須支援 CORS。</span><span class="sxs-lookup"><span data-stu-id="66518-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="66518-277">幸運的是，所有主要瀏覽器的目前版本包括[cors 支援](http://caniuse.com/cors)。</span><span class="sxs-lookup"><span data-stu-id="66518-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="66518-278">Internet Explorer 8 和 Internet Explorer 9 已部分 CORS 支援，而不 XMLHttpRequest 使用舊版 XDomainRequest 物件。</span><span class="sxs-lookup"><span data-stu-id="66518-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="66518-279">如需詳細資訊，請參閱 < [XDomainRequest-限制、 限制和因應措施](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx)。</span><span class="sxs-lookup"><span data-stu-id="66518-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
