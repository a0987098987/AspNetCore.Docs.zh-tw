---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: 不執行在 ASP.NET 中，以及該做什麼 |Microsoft Docs
author: Rick-Anderson
description: 本主題會描述人員進行 ASP.NET web 專案中的數個常見的錯誤。 它提供您該如何避免這些通用的建議...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 512d2e2b39467635390fa175546f79d8c9f89f4a
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667709"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="6f322-104">在 ASP.NET 中不該做什麼以及該做什麼</span><span class="sxs-lookup"><span data-stu-id="6f322-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="6f322-105">本主題會描述人員進行 ASP.NET web 專案中的數個常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6f322-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="6f322-106">它提供建議您應該如何避免這些常見的錯誤。</span><span class="sxs-lookup"><span data-stu-id="6f322-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="6f322-107">它根據[簡報](http://vimeo.com/68390507)依**Damian Edwards**在 Norwegian Developers Conference。</span><span class="sxs-lookup"><span data-stu-id="6f322-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="6f322-108">免責聲明</span><span class="sxs-lookup"><span data-stu-id="6f322-108">Disclaimer</span></span>

<span data-ttu-id="6f322-109">本主題不是做為完整的指南以確保您的應用程式既安全又有效率。</span><span class="sxs-lookup"><span data-stu-id="6f322-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="6f322-110">您仍然要遵循的安全性和效能不會在本主題中所述的最佳作法。</span><span class="sxs-lookup"><span data-stu-id="6f322-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="6f322-111">它只會建議如何避免常見的錯誤與.NET 類別和程序。</span><span class="sxs-lookup"><span data-stu-id="6f322-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="6f322-112">總覽</span><span class="sxs-lookup"><span data-stu-id="6f322-112">Overview</span></span>

<span data-ttu-id="6f322-113">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="6f322-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="6f322-114">標準相容性</span><span class="sxs-lookup"><span data-stu-id="6f322-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="6f322-115">控制項配接器</span><span class="sxs-lookup"><span data-stu-id="6f322-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="6f322-116">控制項的樣式屬性</span><span class="sxs-lookup"><span data-stu-id="6f322-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="6f322-117">頁面和控制項的回呼</span><span class="sxs-lookup"><span data-stu-id="6f322-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="6f322-118">偵測瀏覽器功能</span><span class="sxs-lookup"><span data-stu-id="6f322-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="6f322-119">安全性</span><span class="sxs-lookup"><span data-stu-id="6f322-119">Security</span></span>](#security)

    - [<span data-ttu-id="6f322-120">要求驗證</span><span class="sxs-lookup"><span data-stu-id="6f322-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="6f322-121">Cookieless 表單驗證和工作階段</span><span class="sxs-lookup"><span data-stu-id="6f322-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="6f322-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="6f322-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="6f322-123">中度信任</span><span class="sxs-lookup"><span data-stu-id="6f322-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="6f322-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="6f322-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="6f322-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="6f322-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="6f322-126">可靠性和效能</span><span class="sxs-lookup"><span data-stu-id="6f322-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="6f322-127">PreSendRequestHeaders 和 PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="6f322-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="6f322-128">Web form 的非同步頁面事件</span><span class="sxs-lookup"><span data-stu-id="6f322-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="6f322-129">Fire-and-Forget Work</span><span class="sxs-lookup"><span data-stu-id="6f322-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="6f322-130">要求實體內容</span><span class="sxs-lookup"><span data-stu-id="6f322-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="6f322-131">Response.Redirect 和 Response.End</span><span class="sxs-lookup"><span data-stu-id="6f322-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="6f322-132">EnableViewState 和 ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="6f322-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="6f322-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="6f322-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="6f322-134">長時間執行的要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="6f322-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="6f322-135">標準相容性</span><span class="sxs-lookup"><span data-stu-id="6f322-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="6f322-136">控制項配接器</span><span class="sxs-lookup"><span data-stu-id="6f322-136">Control adapters</span></span>

<span data-ttu-id="6f322-137">建議：停止使用調適性的轉譯控制項配接器，並改為使用 CSS 媒體查詢和符合標準的 HTML。</span><span class="sxs-lookup"><span data-stu-id="6f322-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="6f322-138">要呈現已自訂為不同的裝置和環境的展示程式碼的.NET 2.0 中引進的控制項配接器。</span><span class="sxs-lookup"><span data-stu-id="6f322-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="6f322-139">現在，此調整呈現即可透過 CSS 與 HTML。</span><span class="sxs-lookup"><span data-stu-id="6f322-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="6f322-140">您應該停止使用控制項配接器，並將任何現有的配接器轉換為 CSS 與 HTML。</span><span class="sxs-lookup"><span data-stu-id="6f322-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="6f322-141">如需詳細資訊，請參閱 <<c0> [ 媒體查詢](http://www.w3.org/TR/css3-mediaqueries/)和[How To:將行動網頁加入 ASP.NET Web Form / MVC 應用程式](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="6f322-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="6f322-142">控制項的樣式屬性</span><span class="sxs-lookup"><span data-stu-id="6f322-142">Style properties on controls</span></span>

<span data-ttu-id="6f322-143">建議：停止在控制項標記中，設定樣式值，並改為設定 CSS 樣式表中的 格式化的值。</span><span class="sxs-lookup"><span data-stu-id="6f322-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="6f322-144">Web 伺服器控制項包含數十個可用設定的內嵌樣式屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="6f322-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="6f322-145">比方說，ForeColor 屬性設定控制項的文字的色彩。</span><span class="sxs-lookup"><span data-stu-id="6f322-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="6f322-146">您可以達成此相同的效果更有效率地透過 CSS 樣式表。</span><span class="sxs-lookup"><span data-stu-id="6f322-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="6f322-147">樣式表可讓您集中管理樣式值，並避免設定這些值在您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f322-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="6f322-148">下列範例顯示 CSS 類別集文字設為紅色。</span><span class="sxs-lookup"><span data-stu-id="6f322-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="6f322-149">下一個範例示範如何以動態方式套用的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="6f322-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="6f322-150">頁面和控制項的回呼</span><span class="sxs-lookup"><span data-stu-id="6f322-150">Page and control callbacks</span></span>

<span data-ttu-id="6f322-151">建議：停止使用頁面和控制項的回呼，並改為使用下列其中一項：AJAX、 UpdatePanel、 MVC 動作方法、 Web API 或 SignalR。</span><span class="sxs-lookup"><span data-stu-id="6f322-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="6f322-152">在舊版 ASP.NET 中，頁面和控制項的回呼方法會啟用您更新網頁的一部分，而不重新整理整個頁面。</span><span class="sxs-lookup"><span data-stu-id="6f322-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="6f322-153">您現在可以完成部分頁面更新，透過[AJAX](../../../ajax/index.md)， [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx)， [MVC](../../../mvc/index.md)， [Web API](../../../web-api/index.md)或[SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="6f322-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="6f322-154">您應該停止使用回呼方法，因為它們會導致問題，使用易記的 Url 和路由。</span><span class="sxs-lookup"><span data-stu-id="6f322-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="6f322-155">根據預設，控制項不會啟用回呼方法，但如果您啟用這項功能在控制項中的，您應該停用它。</span><span class="sxs-lookup"><span data-stu-id="6f322-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="6f322-156">偵測瀏覽器功能</span><span class="sxs-lookup"><span data-stu-id="6f322-156">Browser capability detection</span></span>

<span data-ttu-id="6f322-157">建議：停止使用靜態的瀏覽器功能偵測，並改為使用動態功能偵測。</span><span class="sxs-lookup"><span data-stu-id="6f322-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="6f322-158">在舊版 ASP.NET 中，每個瀏覽器支援的功能已儲存在 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="6f322-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="6f322-159">透過靜態查閱支援的偵測功能不是最好的方法。</span><span class="sxs-lookup"><span data-stu-id="6f322-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="6f322-160">現在，您可以動態地偵測瀏覽器支援的功能使用的功能偵測架構，例如[Modernizr](http://modernizr.com/)。</span><span class="sxs-lookup"><span data-stu-id="6f322-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="6f322-161">功能偵測來嘗試使用的方法或屬性，然後檢查以查看是否瀏覽器就會產生所要的結果判斷支援。</span><span class="sxs-lookup"><span data-stu-id="6f322-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="6f322-162">根據預設，Modernizr 包含在 Web 應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="6f322-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="6f322-163">安全性</span><span class="sxs-lookup"><span data-stu-id="6f322-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="6f322-164">要求驗證</span><span class="sxs-lookup"><span data-stu-id="6f322-164">Request validation</span></span>

<span data-ttu-id="6f322-165">建議：驗證使用者輸入，並將來自使用者的輸出編碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="6f322-166">要求驗證是一項功能的 ASP.NET 會檢查每個要求，並停止要求，如果找到察覺到的威脅。</span><span class="sxs-lookup"><span data-stu-id="6f322-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="6f322-167">不相依於要求驗證來保護您的應用程式防止跨網站指令碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="6f322-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="6f322-168">相反地，驗證來自使用者的所有輸入和輸出編碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="6f322-169">在某些限制的情況下，您可以使用規則運算式來驗證輸入，但在更複雜的情況下，您應該驗證使用者輸入，使用.NET 類別，以決定如果值符合允許的值。</span><span class="sxs-lookup"><span data-stu-id="6f322-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="6f322-170">下列範例示範如何使用 Uri 類別中的靜態方法，以判斷使用者所提供的 Uri 是否有效。</span><span class="sxs-lookup"><span data-stu-id="6f322-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="6f322-171">不過，若要充分驗證 Uri，您也應該檢查以確定它會指定`http`或`https`。</span><span class="sxs-lookup"><span data-stu-id="6f322-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="6f322-172">下列範例會使用執行個體方法，驗證 Uri 有效。</span><span class="sxs-lookup"><span data-stu-id="6f322-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="6f322-173">轉譯為 HTML 的使用者輸入，或在 SQL 查詢中包括使用者輸入，再將編碼的值，以確保惡意程式碼就不會包含。</span><span class="sxs-lookup"><span data-stu-id="6f322-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="6f322-174">您可以 HTML 編碼標記中的值&lt;%: %&gt;語法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="6f322-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="6f322-175">或者，您可以在 Razor 語法中，可以 HTML 編碼與 @，如下所示。</span><span class="sxs-lookup"><span data-stu-id="6f322-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="6f322-176">下一個範例顯示如何為 HTML 編碼程式碼後置中的值。</span><span class="sxs-lookup"><span data-stu-id="6f322-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="6f322-177">若要安全地將編碼的 SQL 命令的值，請使用命令參數如下[SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="6f322-178">Cookieless 表單驗證和工作階段</span><span class="sxs-lookup"><span data-stu-id="6f322-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="6f322-179">建議：要求 cookie。</span><span class="sxs-lookup"><span data-stu-id="6f322-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="6f322-180">查詢字串中傳遞驗證資訊並不安全。</span><span class="sxs-lookup"><span data-stu-id="6f322-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="6f322-181">因此，當您的應用程式包含驗證時，才需要 cookie。</span><span class="sxs-lookup"><span data-stu-id="6f322-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="6f322-182">如果您的 cookie 儲存機密資訊，請考慮 cookie 需要 SSL。</span><span class="sxs-lookup"><span data-stu-id="6f322-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="6f322-183">下列範例示範如何在 Web.config 檔案中指定表單驗證，需要透過 SSL 傳送的 cookie。</span><span class="sxs-lookup"><span data-stu-id="6f322-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="6f322-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="6f322-184">EnableViewStateMac</span></span>

<span data-ttu-id="6f322-185">建議：永遠不會設定為 false。</span><span class="sxs-lookup"><span data-stu-id="6f322-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="6f322-186">根據預設，設定 EnbableViewStateMac 設為 true。</span><span class="sxs-lookup"><span data-stu-id="6f322-186">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="6f322-187">即使您的應用程式不使用檢視狀態，不將 EnableViewStateMac 設定為 false。</span><span class="sxs-lookup"><span data-stu-id="6f322-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="6f322-188">將此值設定為 false 會讓應用程式容易遭受跨網站指令碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="6f322-189">從 ASP.NET 4.5.2 開始，執行階段會強制**EnableViewStateMac = true**。</span><span class="sxs-lookup"><span data-stu-id="6f322-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="6f322-190">即使您將它設定為 false，執行階段就會忽略此值，並會繼續進行設定的值，設為 true。</span><span class="sxs-lookup"><span data-stu-id="6f322-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="6f322-191">如需詳細資訊，請參閱 < [ASP.NET 4.5.2 和 EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="6f322-192">下列範例示範如何將 EnableViewStateMac 設定為 true。</span><span class="sxs-lookup"><span data-stu-id="6f322-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="6f322-193">您不需要實際此值設為 true，因為它是預設值為 true。</span><span class="sxs-lookup"><span data-stu-id="6f322-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="6f322-194">不過，如果您已將它設定為 false 任何頁面上您的應用程式中，您必須立即更正此值。</span><span class="sxs-lookup"><span data-stu-id="6f322-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="6f322-195">中度信任</span><span class="sxs-lookup"><span data-stu-id="6f322-195">Medium trust</span></span>

<span data-ttu-id="6f322-196">建議：不相依於中度信任 （或任何其他的信任層級） 做為安全性界限。</span><span class="sxs-lookup"><span data-stu-id="6f322-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="6f322-197">在部分信任不會適當地保護您的應用程式，和不應使用。</span><span class="sxs-lookup"><span data-stu-id="6f322-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="6f322-198">相反地，使用完全信任 」，並隔離個別的應用程式集區中不受信任的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f322-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="6f322-199">此外，執行下的唯一識別每個應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="6f322-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="6f322-200">如需詳細資訊，請參閱 < [ASP.NET 部分信任的資訊並不保證應用程式隔離](https://support.microsoft.com/kb/2698981)。</span><span class="sxs-lookup"><span data-stu-id="6f322-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="6f322-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="6f322-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="6f322-202">建議：請勿停用中的安全性設定&lt;appSettings&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="6f322-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="6f322-203">AppSettings 項目包含許多值所需的安全性更新。</span><span class="sxs-lookup"><span data-stu-id="6f322-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="6f322-204">您不應該變更或停用這些值。</span><span class="sxs-lookup"><span data-stu-id="6f322-204">You should not change or disable these values.</span></span> <span data-ttu-id="6f322-205">如果部署更新時，您必須停用這些值，請立即重新啟用完成部署之後。</span><span class="sxs-lookup"><span data-stu-id="6f322-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="6f322-206">如需詳細資訊，請參閱 < [ASP.NET appSettings 項目](https://msdn.microsoft.com/library/hh975440.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="6f322-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="6f322-207">UrlPathEncode</span></span>

<span data-ttu-id="6f322-208">建議：使用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)改。</span><span class="sxs-lookup"><span data-stu-id="6f322-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="6f322-209">UrlPathEncode 方法已新增至.NET Framework，才能解決非常特定的瀏覽器相容性問題。</span><span class="sxs-lookup"><span data-stu-id="6f322-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="6f322-210">它不會適當地編碼 URL，並不會保護您的應用程式從跨網站指令碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="6f322-211">您應該永遠不會使用您的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="6f322-211">You should never use it in your application.</span></span> <span data-ttu-id="6f322-212">請改用[UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="6f322-213">下列範例示範如何為超連結控制項的查詢字串參數傳遞編碼的 URL。</span><span class="sxs-lookup"><span data-stu-id="6f322-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="6f322-214">可靠性和效能</span><span class="sxs-lookup"><span data-stu-id="6f322-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="6f322-215">PreSendRequestHeaders 和 PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="6f322-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="6f322-216">建議：請勿使用這些事件與受管理模組。</span><span class="sxs-lookup"><span data-stu-id="6f322-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="6f322-217">相反地，撰寫原生 IIS 模組來執行必要的工作。</span><span class="sxs-lookup"><span data-stu-id="6f322-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="6f322-218">請參閱[建立原生程式碼 HTTP 模組](https://msdn.microsoft.com/library/ms693629.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="6f322-219">您可以使用[PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx)並[PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx)使用原生 IIS 模組的事件。</span><span class="sxs-lookup"><span data-stu-id="6f322-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="6f322-220">請勿使用`PreSendRequestHeaders`並`PreSendRequestContent`實作的受管理模組使用`IHttpModule`。</span><span class="sxs-lookup"><span data-stu-id="6f322-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="6f322-221">設定這些屬性會導致發生問題的非同步要求。</span><span class="sxs-lookup"><span data-stu-id="6f322-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="6f322-222">應用程式要求路由 (ARR) 和 websockets 的組合可能會導致存取違規例外狀況會導致損毀的 w3wp。</span><span class="sxs-lookup"><span data-stu-id="6f322-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="6f322-223">比方說，iiscore ！W3_CONTEXT_BASE::GetIsLastNotification + 68 iiscore.dll 中的造成存取違規例外 (0xC0000005)。</span><span class="sxs-lookup"><span data-stu-id="6f322-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="6f322-224">Web form 的非同步頁面事件</span><span class="sxs-lookup"><span data-stu-id="6f322-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="6f322-225">建議：避免在 Web Form 中的 撰寫非同步 void 的方法，適用於網頁生命週期事件，並改用[Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx)非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="6f322-226">當您將標記的頁面事件**非同步**並**void**，您無法決定完成非同步的程式碼時。</span><span class="sxs-lookup"><span data-stu-id="6f322-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="6f322-227">請改用 Page.RegisterAsyncTask 執行非同步程式碼可讓您追蹤其完成方式。</span><span class="sxs-lookup"><span data-stu-id="6f322-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="6f322-228">下列範例顯示的按鈕中，按一下 處理常式，其中包含非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="6f322-229">此範例包含以非同步方式讀取的字串值所提供只做為非同步工作的簡化範例，而不是建議的作法。</span><span class="sxs-lookup"><span data-stu-id="6f322-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="6f322-230">如果您使用的非同步工作，將 Http 執行階段目標架構設為 4.5 （或更新版本） 在 Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="6f322-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="6f322-231">.NET 4.5 中已新增設定的目標 framework 4.5 的開啟新的同步處理內容上的。</span><span class="sxs-lookup"><span data-stu-id="6f322-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="6f322-232">此值在 Visual Studio 中的新專案中的預設設定，但如果不是設定您正在使用現有的專案。</span><span class="sxs-lookup"><span data-stu-id="6f322-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="6f322-233">火災和忘記工作</span><span class="sxs-lookup"><span data-stu-id="6f322-233">Fire-and-forget work</span></span>

<span data-ttu-id="6f322-234">建議：當處理 asp.net 要求，避免啟動火災和忘記工作 （這類呼叫 ThreadPool.QueueUserWorkItem 方法或建立一個計時器，重複呼叫委派）。</span><span class="sxs-lookup"><span data-stu-id="6f322-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="6f322-235">如果您的應用程式有在 ASP.NET 中執行的單一事件-fire-and-forget 工作，您的應用程式可以取得不同步。在任何時間，應用程式定義域可以終結這表示您持續不斷的過程可能不再符合應用程式的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="6f322-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="6f322-236">您應該移動這種類型的工作，在 ASP.NET 之外。</span><span class="sxs-lookup"><span data-stu-id="6f322-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="6f322-237">您可以在 Azure 中使用 Web 工作、 Windows 服務或背景工作角色來執行進行中的工作，並從另一個處理序執行該程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="6f322-238">如果您必須執行這項工作在 ASP.NET 中的，您可以新增 Nuget 套件，稱之為[WebBackgrounder](http://www.nuget.org/packages/webbackgrounder)執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="6f322-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="6f322-239">要求實體內容</span><span class="sxs-lookup"><span data-stu-id="6f322-239">Request entity body</span></span>

<span data-ttu-id="6f322-240">建議：避免讀取 Request.Form 或 Request.InputStream 之前的處理常式執行事件。</span><span class="sxs-lookup"><span data-stu-id="6f322-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="6f322-241">您應該讀取 Request.Form 或 Request.InputStream 最舊期間的處理常式執行事件。</span><span class="sxs-lookup"><span data-stu-id="6f322-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="6f322-242">在 MVC 中，控制器是處理常式的執行事件時，則在動作方法執行。</span><span class="sxs-lookup"><span data-stu-id="6f322-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="6f322-243">在 Web Form 頁面是處理常式，Page.Init 事件引發時執行事件。</span><span class="sxs-lookup"><span data-stu-id="6f322-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="6f322-244">如果您的執行事件之前讀取要求實體內容，您會干擾處理要求。</span><span class="sxs-lookup"><span data-stu-id="6f322-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="6f322-245">如果您要讀取要求實體內容的執行事件之前，請使用[Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx)或是[Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="6f322-246">當您使用 GetBufferlessInputStream 時，您會從要求取得未經處理的資料流，並負責提供處理整個要求。</span><span class="sxs-lookup"><span data-stu-id="6f322-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="6f322-247">在呼叫之後 GetBufferlessInputStream，Request.Form 和 Request.InputStream 無法使用。 因為它們未填入 asp.net</span><span class="sxs-lookup"><span data-stu-id="6f322-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="6f322-248">當您使用 GetBufferedInputStream 時，您會從要求取得一份資料流。</span><span class="sxs-lookup"><span data-stu-id="6f322-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="6f322-249">Request.Form Request.InputStream 等要求的更新版本中仍然可用因為 ASP.NET 會填入其他複本。</span><span class="sxs-lookup"><span data-stu-id="6f322-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="6f322-250">Response.Redirect 和 Response.End</span><span class="sxs-lookup"><span data-stu-id="6f322-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="6f322-251">建議：注意執行緒之後呼叫的處理方式的差異[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="6f322-252">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx)方法會呼叫 Response.End 方法。</span><span class="sxs-lookup"><span data-stu-id="6f322-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="6f322-253">在同步處理中，呼叫 Request.Redirect，將會導致目前的執行緒立即中止。</span><span class="sxs-lookup"><span data-stu-id="6f322-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="6f322-254">不過，在非同步處理序，呼叫 Response.Redirect 不會中止目前的執行緒，因此執行程式碼會繼續要求。</span><span class="sxs-lookup"><span data-stu-id="6f322-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="6f322-255">在非同步處理序，您必須從要停止執行程式碼的方法傳回工作。</span><span class="sxs-lookup"><span data-stu-id="6f322-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="6f322-256">在 MVC 專案中，您不應該呼叫 Response.Redirect。</span><span class="sxs-lookup"><span data-stu-id="6f322-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="6f322-257">相反地，傳回 RedirectResult。</span><span class="sxs-lookup"><span data-stu-id="6f322-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="6f322-258">EnableViewState 和 ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="6f322-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="6f322-259">建議：使用 ViewStateMode，而非 EnableViewState，來提供更精確地控制哪些控制項使用檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="6f322-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="6f322-260">當您設定為 false，Page 指示詞中的 EnableViewState 時，檢視狀態已停用頁面內的所有控制項，且無法啟用。</span><span class="sxs-lookup"><span data-stu-id="6f322-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="6f322-261">如果您想要啟用在網頁中某些控制項的檢視狀態，設定為停用 ViewStateMode 頁面。</span><span class="sxs-lookup"><span data-stu-id="6f322-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="6f322-262">然後，設定為 已啟用 ViewStateMode 實際上需要檢視狀態的控制項上。</span><span class="sxs-lookup"><span data-stu-id="6f322-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="6f322-263">藉由啟用需要它的控制項檢視狀態，您可以讓網頁的壓縮檢視狀態的大小。</span><span class="sxs-lookup"><span data-stu-id="6f322-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="6f322-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="6f322-264">SqlMembershipProvider</span></span>

<span data-ttu-id="6f322-265">建議：使用通用的提供者。</span><span class="sxs-lookup"><span data-stu-id="6f322-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="6f322-266">已被取代目前的專案範本，SqlMembershipProvider [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers)，這是以 NuGet 套件形式提供。</span><span class="sxs-lookup"><span data-stu-id="6f322-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="6f322-267">如果您使用 SqlMembershipProvider 使用較早版本的範本建置的專案中，您應該切換至 Universal Providers。</span><span class="sxs-lookup"><span data-stu-id="6f322-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="6f322-268">通用的提供者使用 Entity Framework 所支援的所有資料庫。</span><span class="sxs-lookup"><span data-stu-id="6f322-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="6f322-269">如需詳細資訊，請參閱 < [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx)。</span><span class="sxs-lookup"><span data-stu-id="6f322-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="6f322-270">長時間執行的要求 (> 110 秒)</span><span class="sxs-lookup"><span data-stu-id="6f322-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="6f322-271">建議：使用  [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx)或是[SignalR](../../../signalr/index.md)連線的用戶端和使用非同步 I/O 作業。</span><span class="sxs-lookup"><span data-stu-id="6f322-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="6f322-272">長時間執行的要求可能會在您的 web 應用程式中造成無法預期的結果和效能不佳。</span><span class="sxs-lookup"><span data-stu-id="6f322-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="6f322-273">要求的預設逾時設定為 110 秒。</span><span class="sxs-lookup"><span data-stu-id="6f322-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="6f322-274">如果您使用工作階段狀態與長時間執行要求，ASP.NET 會 110 秒後發行的工作階段物件上的鎖定。</span><span class="sxs-lookup"><span data-stu-id="6f322-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="6f322-275">不過，您的應用程式可能正在工作階段物件上的作業時釋放鎖定，以及作業可能無法順利完成。</span><span class="sxs-lookup"><span data-stu-id="6f322-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="6f322-276">如果第一個要求正在執行時，會封鎖來自使用者的第二個要求，第二個要求可能需要存取工作階段物件不一致的狀態。</span><span class="sxs-lookup"><span data-stu-id="6f322-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="6f322-277">如果您的應用程式包含封鎖 （或非同步） I/O 作業，應用程式將會沒有回應。</span><span class="sxs-lookup"><span data-stu-id="6f322-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="6f322-278">為了改善效能，請在 .NET Framework 中使用非同步 I/O 作業。</span><span class="sxs-lookup"><span data-stu-id="6f322-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="6f322-279">此外，使用 WebSockets 或 SignalR 來連接到伺服器的用戶端。</span><span class="sxs-lookup"><span data-stu-id="6f322-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="6f322-280">這些功能被設計來有效率地處理長時間執行要求。</span><span class="sxs-lookup"><span data-stu-id="6f322-280">These features are designed to efficiently handle long-running requests.</span></span>
