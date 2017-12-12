---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: "了解 ASP.NET AJAX 驗證和設定檔的應用程式服務 |Microsoft 文件"
author: scottcate
description: "驗證服務可讓使用者提供認證以接收驗證 cookie，且閘道服務，讓自訂使用者..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 7e0ddc15fac9af40a0a20a99979a80517eb1b6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a><span data-ttu-id="f78e4-103">了解 ASP.NET AJAX 驗證和設定檔的應用程式服務</span><span class="sxs-lookup"><span data-stu-id="f78e4-103">Understanding ASP.NET AJAX Authentication and Profile Application Services</span></span>
====================
<span data-ttu-id="f78e4-104">由[Scott 類別](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="f78e4-104">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="f78e4-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="f78e4-105">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> <span data-ttu-id="f78e4-106">驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="f78e4-106">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="f78e4-107">使用 ASP.NET AJAX 驗證服務是使用標準 ASP.NET 表單驗證時，相容的因此目前使用表單驗證的應用程式 （例如與登入控制） 將不會中斷方法升級至 AJAX 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="f78e4-107">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>


## <a name="introduction"></a><span data-ttu-id="f78e4-108">簡介</span><span class="sxs-lookup"><span data-stu-id="f78e4-108">Introduction</span></span>

<span data-ttu-id="f78e4-109">.NET Framework 3.5 的一部分，Microsoft 會將可調整大小的環境升級;不只是新的開發環境，但新的 Language-Integrated Query (LINQ) 功能和其他語言增強功能即將推出。</span><span class="sxs-lookup"><span data-stu-id="f78e4-109">As part of the .NET Framework 3.5, Microsoft is delivering a sizable environment upgrade; not only is a new development environment available, but the new Language-Integrated Query (LINQ) features and other language enhancements are forthcoming.</span></span> <span data-ttu-id="f78e4-110">此外，某些熟悉的其他工具組，值得注意的是 ASP.NET AJAX Extensions 中所包含功能做為第一級的.NET Framework 基底類別庫的成員。</span><span class="sxs-lookup"><span data-stu-id="f78e4-110">In addition, some familiar features of other toolsets, notably the ASP.NET AJAX Extensions, are being included as first-class members of the .NET Framework Base Class Library.</span></span> <span data-ttu-id="f78e4-111">這些延伸可讓許多豐富型用戶端的新功能，包括局部網頁呈現，而不需要完整頁面重新整理，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API） 和廣泛的 API，用戶端存取 Web 服務的能力設計來鏡射許多 ASP.NET 伺服器端控制項集合中所見的控制項配置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-111">These extensions enable many new rich client features, including partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="f78e4-112">本白皮書會查看的實作和使用 ASP.NET 程式碼剖析和表單驗證服務由 Microsoft ASP.NET AJAX ExtensionsThe AJAX 擴充功能公開讓表單驗證非常容易支援，因為它 （以及程式碼剖析服務中） 會公開透過 Web 服務 proxy 指令碼。</span><span class="sxs-lookup"><span data-stu-id="f78e4-112">This whitepaper looks at the implementation and use of the ASP.NET Profiling and Forms Authentication services as they are exposed by the Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions make Forms authentication incredibly easy to support, as it (as well as the Profiling Service) is exposed through a Web Service proxy script.</span></span> <span data-ttu-id="f78e4-113">AJAX 擴充功能也支援透過 AuthenticationServiceManager 類別的自訂驗證。</span><span class="sxs-lookup"><span data-stu-id="f78e4-113">The AJAX Extensions also support custom authentication through the AuthenticationServiceManager class.</span></span>

<span data-ttu-id="f78e4-114">本白皮書根據在 Beta 2 版本的 Visual Studio 2008 和.NET Framework 3.5。</span><span class="sxs-lookup"><span data-stu-id="f78e4-114">This whitepaper is based on the Beta 2 release of the Visual Studio 2008 and the .NET Framework 3.5.</span></span> <span data-ttu-id="f78e4-115">本白皮書也會假設您將使用 Visual Studio 2008 Beta 2、 不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="f78e4-115">This whitepaper also assumes that you will be working with Visual Studio 2008 Beta 2, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="f78e4-116">某些程式碼範例可利用專案範本在 Visual Web Developer Express 中無法使用。</span><span class="sxs-lookup"><span data-stu-id="f78e4-116">Some code samples may utilize project templates unavailable in Visual Web Developer Express.</span></span>

## <a name="profiles-and-authentication"></a><span data-ttu-id="f78e4-117">*設定檔和驗證*</span><span class="sxs-lookup"><span data-stu-id="f78e4-117">*Profiles and Authentication*</span></span>

<span data-ttu-id="f78e4-118">Microsoft ASP.NET 設定檔和驗證服務所提供的 ASP.NET 表單驗證系統，標準的 ASP.NET 元件。</span><span class="sxs-lookup"><span data-stu-id="f78e4-118">The Microsoft ASP.NET Profiles and Authentication services are provided by the ASP.NET Forms Authentication system, and are standard components of ASP.NET.</span></span> <span data-ttu-id="f78e4-119">ASP.NET AJAX 擴充功能提供指令碼存取這些服務，透過指令碼 proxy，透過用戶端 AJAX 程式庫在 Sys.Services 命名空間之下相當簡單的模型。</span><span class="sxs-lookup"><span data-stu-id="f78e4-119">The ASP.NET AJAX Extensions provide script access to these services via script proxies, through a fairly straightforward model under the Sys.Services namespace of the client AJAX library.</span></span>

<span data-ttu-id="f78e4-120">驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="f78e4-120">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="f78e4-121">使用 ASP.NET AJAX 驗證服務是使用標準 ASP.NET 表單驗證時，相容的因此目前使用表單驗證的應用程式 （例如與登入控制） 將不會中斷方法升級至 AJAX 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="f78e4-121">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>

<span data-ttu-id="f78e4-122">根據驗證服務所提供的成員資格的使用者資料的儲存體與自動整合，可讓設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="f78e4-122">The Profile service allows the automatic integration and storage of user data based on membership as provided by the Authentication service.</span></span> <span data-ttu-id="f78e4-123">Web.config 檔案中，指定儲存的資料和各種不同的程式碼剖析服務提供者處理資料管理。</span><span class="sxs-lookup"><span data-stu-id="f78e4-123">The stored data is specified by the web.config file, and the various profiling service providers handle the data management.</span></span> <span data-ttu-id="f78e4-124">如同驗證服務，使目前納入 ASP.NET 設定檔服務的功能頁面應該不會因包括 AJAX 支援，是與標準 ASP.NET 設定檔服務相容 AJAX 設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="f78e4-124">As with the Authentication service, the AJAX Profile service is compatible with the standard ASP.NET profile service, so that pages currently incorporating features of the ASP.NET Profile service should not be broken by including AJAX support.</span></span>

<span data-ttu-id="f78e4-125">應用程式中併入 ASP.NET 驗證和分析服務本身已超出本白皮書的範圍。</span><span class="sxs-lookup"><span data-stu-id="f78e4-125">Incorporating the ASP.NET Authentication and Profiling services themselves into an application is outside of the scope of this whitepaper.</span></span> <span data-ttu-id="f78e4-126">如需有關主題的詳細資訊，請參閱 MSDN Library 參考發行項使用成員資格管理使用者在[https://msdn.microsoft.com/en-us/library/tw292whz.aspx](https://msdn.microsoft.com/en-us/library/tw292whz.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f78e4-126">For more information on the topic, see the MSDN Library reference article Managing Users by Using Membership at [https://msdn.microsoft.com/en-us/library/tw292whz.aspx](https://msdn.microsoft.com/en-us/library/tw292whz.aspx).</span></span> <span data-ttu-id="f78e4-127">ASP.NET 也會包含一個公用程式來自動設定 SQL Server，也就是 ASP.NET 成員資格的預設驗證服務提供者的成員資格。</span><span class="sxs-lookup"><span data-stu-id="f78e4-127">ASP.NET also includes a utility to automatically set up Membership with a SQL Server, which is the default authentication service provider for ASP.NET Membership.</span></span> <span data-ttu-id="f78e4-128">如需詳細資訊，請參閱文章 ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe) 在[https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="f78e4-128">For more information, see the article ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) at [https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx).</span></span>

## <a name="using-the-aspnet-ajax-authentication-service"></a><span data-ttu-id="f78e4-129">*使用 ASP.NET AJAX 驗證服務*</span><span class="sxs-lookup"><span data-stu-id="f78e4-129">*Using the ASP.NET AJAX Authentication Service*</span></span>

<span data-ttu-id="f78e4-130">必須啟用 ASP.NET AJAX 驗證服務的 web.config 檔案中：</span><span class="sxs-lookup"><span data-stu-id="f78e4-130">The ASP.NET AJAX Authentication service must be enabled in the web.config file:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

<span data-ttu-id="f78e4-131">驗證服務需要 ASP.NET 表單驗證以啟用，而且需要在用戶端瀏覽器 （指令碼無法啟用無 cookie 工作階段因為無 cookie 工作階段需要 URL 參數） 上啟用 cookie。</span><span class="sxs-lookup"><span data-stu-id="f78e4-131">The Authentication service requires ASP.NET Forms authentication to be enabled and requires cookies to be enabled on the client browser (a script cannot enable a cookieless session since cookieless sessions require URL parameters).</span></span>

<span data-ttu-id="f78e4-132">一旦啟用並設定 AJAX 驗證服務，用戶端指令碼可以立即利用 Sys.Services.AuthenticationService 物件。</span><span class="sxs-lookup"><span data-stu-id="f78e4-132">Once the AJAX authentication service is enabled and configured, client script can immediately take advantage of the Sys.Services.AuthenticationService object.</span></span> <span data-ttu-id="f78e4-133">主要是用戶端指令碼也會想要充分利用`login`方法和`isLoggedIn`屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-133">Primarily, client script will want to take advantage of the `login` method and `isLoggedIn` property.</span></span> <span data-ttu-id="f78e4-134">登入方法可接受大量的參數提供預設值，存在數個屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-134">Several properties exist to provide defaults for the login method, which can accept a large number of parameters.</span></span>

<span data-ttu-id="f78e4-135">*Sys.Services.AuthenticationService 成員*</span><span class="sxs-lookup"><span data-stu-id="f78e4-135">*Sys.Services.AuthenticationService members*</span></span>

<span data-ttu-id="f78e4-136">*登入方法：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-136">*login method:*</span></span>

<span data-ttu-id="f78e4-137">Login() 方法開始的要求來驗證使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="f78e4-137">The login() method begins a request to authenticate the user's credentials.</span></span> <span data-ttu-id="f78e4-138">這個方法是非同步，而且不會封鎖執行。</span><span class="sxs-lookup"><span data-stu-id="f78e4-138">This method is asynchronous and does not block execution.</span></span>

<span data-ttu-id="f78e4-139">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-139">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-140">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-140">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-141">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-141">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-142">userName</span><span class="sxs-lookup"><span data-stu-id="f78e4-142">userName</span></span> | <span data-ttu-id="f78e4-143">必要項。</span><span class="sxs-lookup"><span data-stu-id="f78e4-143">Required.</span></span> <span data-ttu-id="f78e4-144">要驗證的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-144">The username to authenticate.</span></span> |
| <span data-ttu-id="f78e4-145">密碼</span><span class="sxs-lookup"><span data-stu-id="f78e4-145">password</span></span> | <span data-ttu-id="f78e4-146">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-146">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-147">使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="f78e4-147">The user's password.</span></span> |
| <span data-ttu-id="f78e4-148">isPersistent</span><span class="sxs-lookup"><span data-stu-id="f78e4-148">isPersistent</span></span> | <span data-ttu-id="f78e4-149">選擇性 （預設為 false）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-149">Optional (defaults to false).</span></span> <span data-ttu-id="f78e4-150">是否應該保存使用者的驗證 cookie，跨工作階段。</span><span class="sxs-lookup"><span data-stu-id="f78e4-150">Whether the user's authentication cookie should persist across sessions.</span></span> <span data-ttu-id="f78e4-151">如果為 false，使用者將登出時關閉瀏覽器或工作階段到期。</span><span class="sxs-lookup"><span data-stu-id="f78e4-151">If false, the user will log out when the browser is closed or the session expires.</span></span> |
| <span data-ttu-id="f78e4-152">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="f78e4-152">redirectUrl</span></span> | <span data-ttu-id="f78e4-153">選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="f78e4-153">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="f78e4-154">如果這個參數是 null 或空字串，就會不發生任何重新導向。</span><span class="sxs-lookup"><span data-stu-id="f78e4-154">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="f78e4-155">customInfo</span><span class="sxs-lookup"><span data-stu-id="f78e4-155">customInfo</span></span> | <span data-ttu-id="f78e4-156">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-156">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-157">這個參數是目前未使用，並且會保留供未來使用。</span><span class="sxs-lookup"><span data-stu-id="f78e4-157">This parameter is currently unused and is reserved for future use.</span></span> |
| <span data-ttu-id="f78e4-158">loginCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-158">loginCompletedCallback</span></span> | <span data-ttu-id="f78e4-159">選擇性 （預設為 null）。登入已順利完成時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-159">Optional (defaults to null).The function to call when the login has successfully completed.</span></span> <span data-ttu-id="f78e4-160">如果指定，此參數會覆寫 defaultLoginCompleted 屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-160">If specified, this parameter overrides the defaultLoginCompleted property.</span></span> |
| <span data-ttu-id="f78e4-161">failedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-161">failedCallback</span></span> | <span data-ttu-id="f78e4-162">選擇性 （預設為 null）。登入失敗時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-162">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="f78e4-163">如果指定，此參數會覆寫 defaultFailedCallback 屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-163">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="f78e4-164">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-164">userContext</span></span> | <span data-ttu-id="f78e4-165">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-165">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-166">自訂的使用者內容應該傳遞至回呼函式的資料。</span><span class="sxs-lookup"><span data-stu-id="f78e4-166">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="f78e4-167">*傳回值：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-167">*Return Value:*</span></span>

<span data-ttu-id="f78e4-168">此函式不包含傳回的值。</span><span class="sxs-lookup"><span data-stu-id="f78e4-168">This function does not include a return value.</span></span> <span data-ttu-id="f78e4-169">不過，一些行為會包含在呼叫這個函式完成：</span><span class="sxs-lookup"><span data-stu-id="f78e4-169">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="f78e4-170">目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數為 null 或空字串都不。</span><span class="sxs-lookup"><span data-stu-id="f78e4-170">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="f78e4-171">不過，如果參數是 null 或空字串，`loginCompletedCallback`參數，或`defaultLoginCompletedCallback`呼叫屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-171">However, if the parameter was null or an empty string, the `loginCompletedCallback` parameter, or `defaultLoginCompletedCallback` property is called.</span></span>
- <span data-ttu-id="f78e4-172">如果 web 服務呼叫失敗，`failedCallback`參數`defaultFailedCallback`呼叫屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-172">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="f78e4-173">*登出方法：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-173">*logout method:*</span></span>

<span data-ttu-id="f78e4-174">Logout() 方法移除認證 cookie，並從 web 應用程式目前的使用者登出。</span><span class="sxs-lookup"><span data-stu-id="f78e4-174">The logout() method removes the credentials cookie and logs out the current user from the web application.</span></span>

<span data-ttu-id="f78e4-175">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-175">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-176">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-176">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-177">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-177">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-178">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="f78e4-178">redirectUrl</span></span> | <span data-ttu-id="f78e4-179">選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="f78e4-179">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="f78e4-180">如果這個參數是 null 或空字串，就會不發生任何重新導向。</span><span class="sxs-lookup"><span data-stu-id="f78e4-180">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="f78e4-181">logoutCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-181">logoutCompletedCallback</span></span> | <span data-ttu-id="f78e4-182">選擇性 （預設為 null）。登出已順利完成時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-182">Optional (defaults to null).The function to call when the logout has successfully completed.</span></span> <span data-ttu-id="f78e4-183">如果指定，此參數會覆寫 defaultLogoutCompleted 屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-183">If specified, this parameter overrides the defaultLogoutCompleted property.</span></span> |
| <span data-ttu-id="f78e4-184">failedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-184">failedCallback</span></span> | <span data-ttu-id="f78e4-185">選擇性 （預設為 null）。登入失敗時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-185">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="f78e4-186">如果指定，此參數會覆寫 defaultFailedCallback 屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-186">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="f78e4-187">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-187">userContext</span></span> | <span data-ttu-id="f78e4-188">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-188">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-189">自訂的使用者內容應該傳遞至回呼函式的資料。</span><span class="sxs-lookup"><span data-stu-id="f78e4-189">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="f78e4-190">*傳回值：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-190">*Return Value:*</span></span>

<span data-ttu-id="f78e4-191">此函式不包含傳回的值。</span><span class="sxs-lookup"><span data-stu-id="f78e4-191">This function does not include a return value.</span></span> <span data-ttu-id="f78e4-192">不過，一些行為會包含在呼叫這個函式完成：</span><span class="sxs-lookup"><span data-stu-id="f78e4-192">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="f78e4-193">目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數為 null 或空字串都不。</span><span class="sxs-lookup"><span data-stu-id="f78e4-193">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="f78e4-194">不過，如果參數是 null 或空字串，`logoutCompletedCallback`參數，或`defaultLogoutCompletedCallback`呼叫屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-194">However, if the parameter was null or an empty string, the `logoutCompletedCallback` parameter, or `defaultLogoutCompletedCallback` property is called.</span></span>
- <span data-ttu-id="f78e4-195">如果 web 服務呼叫失敗，`failedCallback`參數`defaultFailedCallback`呼叫屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-195">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="f78e4-196">*defaultFailedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-196">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="f78e4-197">此屬性會指定應該在與 web 服務通訊失敗發生時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-197">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="f78e4-198">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-198">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-199">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-199">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

<span data-ttu-id="f78e4-200">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-200">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-201">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-201">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-202">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-202">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-203">個錯誤</span><span class="sxs-lookup"><span data-stu-id="f78e4-203">error</span></span> | <span data-ttu-id="f78e4-204">指定資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f78e4-204">Specifies the error information.</span></span> |
| <span data-ttu-id="f78e4-205">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-205">userContext</span></span> | <span data-ttu-id="f78e4-206">指定的登入或登出函式呼叫時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="f78e4-206">Specifies the user context information provided when the login or logout function was called.</span></span> |
| <span data-ttu-id="f78e4-207">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-207">methodName</span></span> | <span data-ttu-id="f78e4-208">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-208">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-209">*defaultLoginCompletedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-209">*defaultLoginCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="f78e4-210">此屬性會指定應該在完成登入 web 服務呼叫時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-210">This property specifies a function that should be called when the login web service call has completed.</span></span> <span data-ttu-id="f78e4-211">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-211">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-212">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-212">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

<span data-ttu-id="f78e4-213">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-213">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-214">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-214">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-215">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-215">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-216">validCredentials</span><span class="sxs-lookup"><span data-stu-id="f78e4-216">validCredentials</span></span> | <span data-ttu-id="f78e4-217">指定使用者提供有效的認證。</span><span class="sxs-lookup"><span data-stu-id="f78e4-217">Specifies whether the user provided valid credentials.</span></span> <span data-ttu-id="f78e4-218">`true`如果使用者成功登入。否則`false`。</span><span class="sxs-lookup"><span data-stu-id="f78e4-218">`true` if the user successfully logged in; otherwise `false`.</span></span> |
| <span data-ttu-id="f78e4-219">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-219">userContext</span></span> | <span data-ttu-id="f78e4-220">指定登入函式呼叫時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="f78e4-220">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="f78e4-221">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-221">methodName</span></span> | <span data-ttu-id="f78e4-222">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-222">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-223">*defaultLogoutCompletedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-223">*defaultLogoutCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="f78e4-224">此屬性會指定應在登出 web 服務呼叫完成時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-224">This property specifies a function that should be called when the logout web service call has completed.</span></span> <span data-ttu-id="f78e4-225">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-225">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-226">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-226">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

<span data-ttu-id="f78e4-227">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-227">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-228">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-228">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-229">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-229">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-230">結果</span><span class="sxs-lookup"><span data-stu-id="f78e4-230">result</span></span> | <span data-ttu-id="f78e4-231">這個參數一定會`null`; 它保留供未來使用。</span><span class="sxs-lookup"><span data-stu-id="f78e4-231">This parameter will always be `null`; it is reserved for future use.</span></span> |
| <span data-ttu-id="f78e4-232">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-232">userContext</span></span> | <span data-ttu-id="f78e4-233">指定登入函式呼叫時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="f78e4-233">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="f78e4-234">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-234">methodName</span></span> | <span data-ttu-id="f78e4-235">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-235">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-236">*isLoggedIn 屬性 (get):*</span><span class="sxs-lookup"><span data-stu-id="f78e4-236">*isLoggedIn property (get):*</span></span>

<span data-ttu-id="f78e4-237">這個屬性會取得目前驗證狀態的使用者資訊。ScriptManager 物件設定在網頁要求期間。</span><span class="sxs-lookup"><span data-stu-id="f78e4-237">This property gets the current authentication state of the user; it is set by the ScriptManager object during a page request.</span></span>

<span data-ttu-id="f78e4-238">這個屬性會傳回`true`如果使用者目前登入，否則它會傳回`false`。</span><span class="sxs-lookup"><span data-stu-id="f78e4-238">This property returns `true` if the user is currently logged in; otherwise, it returns `false`.</span></span>

<span data-ttu-id="f78e4-239">*path 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-239">*path property (get, set):*</span></span>

<span data-ttu-id="f78e4-240">這個屬性會以程式設計方式判斷驗證 web 服務的位置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-240">This property programmatically determines the location of the authentication web service.</span></span> <span data-ttu-id="f78e4-241">它可以用來覆寫預設的驗證提供者，以及一個 ScriptManager 控制項 AuthenticationService 子節點的路徑屬性中以宣告方式設定 (如需詳細資訊，請參閱 < 使用自訂驗證服務提供者下列主題）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-241">It can be used to override the default authentication provider, as well as one set declaratively in the Path property of the ScriptManager control's AuthenticationService child node (for more information, see the Using a Custom Authentication Service Provider topic below).</span></span>

<span data-ttu-id="f78e4-242">請注意，預設驗證服務的位置不會變更。</span><span class="sxs-lookup"><span data-stu-id="f78e4-242">Note that the location of the default authentication service does not change.</span></span> <span data-ttu-id="f78e4-243">不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面做為 ASP.NET AJAX 驗證服務 proxy 的位置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-243">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="f78e4-244">請注意這個屬性不應設為值，將指令碼要求導向移出目前的站台。</span><span class="sxs-lookup"><span data-stu-id="f78e4-244">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="f78e4-245">目前應用程式不會收到的驗證認證，因為它會是無用的訊息;此外，技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f78e4-245">Because the current application would not receive the authentication credentials, it would be useless; also, the technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="f78e4-246">這個屬性是`String`物件，表示驗證 web 服務的路徑。</span><span class="sxs-lookup"><span data-stu-id="f78e4-246">This property is a `String` object representing the path to the authentication web service.</span></span>

<span data-ttu-id="f78e4-247">*（get、 set），就會逾時屬性：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-247">*timeout property (get, set):*</span></span>

<span data-ttu-id="f78e4-248">此屬性會判斷失敗的驗證服務前假設登入要求等待的時間長度。</span><span class="sxs-lookup"><span data-stu-id="f78e4-248">This property determines the length of time to wait for the authentication service before assuming the login request has failed.</span></span> <span data-ttu-id="f78e4-249">如果超過逾時等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。</span><span class="sxs-lookup"><span data-stu-id="f78e4-249">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="f78e4-250">這個屬性是`Number`物件，代表要等候的驗證服務來搜尋結果的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="f78e4-250">This property is a `Number` object representing the number of milliseconds to wait for results from the authentication service.</span></span>

<span data-ttu-id="f78e4-251">*登入驗證服務的程式碼範例：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-251">*Code Sample: Logging into the Authentication Service*</span></span>

<span data-ttu-id="f78e4-252">下列標記是與登入和登出 AuthenticationService 類別方法的簡單的指令碼呼叫 ASP.NET 頁面範例。</span><span class="sxs-lookup"><span data-stu-id="f78e4-252">The following markup is an example ASP.NET page with a simple script call to the login and logout methods of the AuthenticationService class.</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a><span data-ttu-id="f78e4-253">存取程式碼剖析資料透過 AJAX 的 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f78e4-253">Accessing ASP.NET Profiling Data via AJAX</span></span>

<span data-ttu-id="f78e4-254">ASP.NET 程式碼剖析服務也會公開透過 ASP.NET AJAX 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="f78e4-254">The ASP.NET profiling service is also exposed through the ASP.NET AJAX Extensions.</span></span> <span data-ttu-id="f78e4-255">由於 ASP.NET 程式碼剖析服務提供豐富且更細微的 API，用來儲存和擷取使用者資料，這可以是絕佳的產能工具。</span><span class="sxs-lookup"><span data-stu-id="f78e4-255">Since the ASP.NET profiling service provides a rich, granular API by which to store and retrieve user data, this can be an excellent productivity tool.</span></span>

<span data-ttu-id="f78e4-256">必須啟用此設定檔服務，在 web.config 中;不是預設值。</span><span class="sxs-lookup"><span data-stu-id="f78e4-256">The profile service must be enabled in web.config; it is not by default.</span></span> <span data-ttu-id="f78e4-257">若要這樣做，請確認`profileService`子項目已啟用 = true 中所指定的 web.config 和指定的屬性可以讀取或寫入，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f78e4-257">To do so, ensure that the `profileService` child element has enabled= true specified in web.config, and that you have specified which properties can be read or written as follows:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

<span data-ttu-id="f78e4-258">也必須設定設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="f78e4-258">The profile service must also be configured.</span></span> <span data-ttu-id="f78e4-259">雖然本白皮書的範圍內的程式碼剖析服務的組態，值得注意設定檔組態設定中所定義的群組將可存取子屬性的群組名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-259">Although configuration of the profiling service is outside of the scope of this whitepaper, it is worthwhile to note that groups as defined in profile configuration settings will be accessible as sub-properties of the group name.</span></span> <span data-ttu-id="f78e4-260">例如，用於指定下列設定檔區段：</span><span class="sxs-lookup"><span data-stu-id="f78e4-260">For example, with the following profile section specified:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

<span data-ttu-id="f78e4-261">用戶端指令碼就可以存取名稱、 Address.Line1、 Address.Line2、 Address.City、 Address.State、 Address.Zip 和 BackgroundColor 為 ProfileService 類別的 [屬性] 欄位的屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-261">Client script would be able to access Name, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip, and BackgroundColor as properties of the properties field of the ProfileService class.</span></span>

<span data-ttu-id="f78e4-262">一旦設定 AJAX 程式碼剖析服務，它就可以立即在頁面;不過，它必須載入一次才能使用。</span><span class="sxs-lookup"><span data-stu-id="f78e4-262">Once the AJAX Profiling Service is configured, it will be immediately available in pages; however, it will have to be loaded once before use.</span></span>

<span data-ttu-id="f78e4-263">*Sys.Services.ProfileService 成員*</span><span class="sxs-lookup"><span data-stu-id="f78e4-263">*Sys.Services.ProfileService members*</span></span>

<span data-ttu-id="f78e4-264">*屬性欄位：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-264">*properties field:*</span></span>

<span data-ttu-id="f78e4-265">[屬性] 欄位會公開為.運算子名稱慣例，可參考的子屬性的所有設定的設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="f78e4-265">The properties field exposes all configured profile data as child properties that can be referenced by the dot-operator-name convention.</span></span> <span data-ttu-id="f78e4-266">屬性群組的子系的屬性會參照 GroupName.PropertyName。</span><span class="sxs-lookup"><span data-stu-id="f78e4-266">Properties that are children of property groups are referred to as GroupName.PropertyName.</span></span> <span data-ttu-id="f78e4-267">在上述範例設定檔設定中，以取得狀態的使用者，您可以使用下列識別碼：</span><span class="sxs-lookup"><span data-stu-id="f78e4-267">In the example profile configuration presented above, to get the state of the user, you could use the following identifier:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

<span data-ttu-id="f78e4-268">*負載平衡方法：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-268">*load method:*</span></span>

<span data-ttu-id="f78e4-269">從伺服器載入選取的清單或所有屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-269">Loads a selected list or all properties from the server.</span></span>

<span data-ttu-id="f78e4-270">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-270">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-271">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-271">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-272">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-272">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-273">propertyNames</span><span class="sxs-lookup"><span data-stu-id="f78e4-273">propertyNames</span></span> | <span data-ttu-id="f78e4-274">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-274">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-275">要從伺服器載入的屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-275">The properties to be loaded from the server.</span></span> |
| <span data-ttu-id="f78e4-276">loadCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-276">loadCompletedCallback</span></span> | <span data-ttu-id="f78e4-277">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-277">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-278">已完成載入時要呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-278">The function to call when loading has completed.</span></span> |
| <span data-ttu-id="f78e4-279">failedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-279">failedCallback</span></span> | <span data-ttu-id="f78e4-280">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-280">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-281">發生錯誤時所要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-281">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="f78e4-282">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-282">userContext</span></span> | <span data-ttu-id="f78e4-283">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-283">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-284">要傳遞給回呼函式的內容資訊。</span><span class="sxs-lookup"><span data-stu-id="f78e4-284">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="f78e4-285">Load 函式沒有傳回值。</span><span class="sxs-lookup"><span data-stu-id="f78e4-285">The load function does not have a return value.</span></span> <span data-ttu-id="f78e4-286">如果呼叫已順利完成，它會呼叫`loadCompletedCallback`參數或`defaultLoadCompletedCallback`屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-286">If the call completed successfully, it will call either the `loadCompletedCallback` parameter or `defaultLoadCompletedCallback` property.</span></span> <span data-ttu-id="f78e4-287">如果呼叫失敗，或逾時，可能是`failedCallback`參數或`defaultFailedCallback`屬性將會呼叫。</span><span class="sxs-lookup"><span data-stu-id="f78e4-287">If the call failed, or the timeout expired, either the `failedCallback` parameter or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="f78e4-288">如果`propertyNames`沒有提供參數，從伺服器擷取所有的讀取設定屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-288">If the `propertyNames` parameter is not supplied, all read-configured properties are retrieved from the server.</span></span>

<span data-ttu-id="f78e4-289">*save 方法：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-289">*save method:*</span></span>

<span data-ttu-id="f78e4-290">Save （） 方法會將使用者的 ASP.NET 設定檔指定的屬性清單 （或所有屬性）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-290">The save() method saves the specified property list (or all properties) to the user's ASP.NET profile.</span></span>

<span data-ttu-id="f78e4-291">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-291">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-292">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-292">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-293">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-293">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-294">propertyNames</span><span class="sxs-lookup"><span data-stu-id="f78e4-294">propertyNames</span></span> | <span data-ttu-id="f78e4-295">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-295">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-296">要儲存到伺服器的屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-296">The properties to be saved to the server.</span></span> |
| <span data-ttu-id="f78e4-297">saveCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-297">saveCompletedCallback</span></span> | <span data-ttu-id="f78e4-298">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-298">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-299">儲存時要呼叫的函式已完成。</span><span class="sxs-lookup"><span data-stu-id="f78e4-299">The function to call when saving has completed.</span></span> |
| <span data-ttu-id="f78e4-300">failedCallback</span><span class="sxs-lookup"><span data-stu-id="f78e4-300">failedCallback</span></span> | <span data-ttu-id="f78e4-301">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-301">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-302">發生錯誤時所要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-302">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="f78e4-303">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-303">userContext</span></span> | <span data-ttu-id="f78e4-304">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-304">Optional (defaults to null).</span></span> <span data-ttu-id="f78e4-305">要傳遞給回呼函式的內容資訊。</span><span class="sxs-lookup"><span data-stu-id="f78e4-305">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="f78e4-306">儲存函式沒有傳回值。</span><span class="sxs-lookup"><span data-stu-id="f78e4-306">The save function does not have a return value.</span></span> <span data-ttu-id="f78e4-307">如果呼叫成功完成，它會呼叫`saveCompletedCallback`參數或`defaultSaveCompletedCallback`屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-307">If the call completes successfully, it will call either the `saveCompletedCallback` parameter or `defaultSaveCompletedCallback` property.</span></span> <span data-ttu-id="f78e4-308">如果呼叫失敗，或逾時，可能是`failedCallback`或`defaultFailedCallback`屬性將會呼叫。</span><span class="sxs-lookup"><span data-stu-id="f78e4-308">If the call failed, or the timeout expired, either the `failedCallback` or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="f78e4-309">如果`propertyNames`參數為 null，所有設定檔屬性將會傳送到伺服器，伺服器會決定可以儲存的內容，以及哪些不可。</span><span class="sxs-lookup"><span data-stu-id="f78e4-309">If the `propertyNames` parameter is null, all profile properties will be sent to the server, and the server will decide which properties can be saved and which cannot.</span></span>

<span data-ttu-id="f78e4-310">*defaultFailedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-310">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="f78e4-311">此屬性會指定應該在與 web 服務通訊失敗發生時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-311">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="f78e4-312">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-312">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-313">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-313">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

<span data-ttu-id="f78e4-314">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-314">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-315">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-315">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-316">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-316">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-317">錯誤</span><span class="sxs-lookup"><span data-stu-id="f78e4-317">Error</span></span> | <span data-ttu-id="f78e4-318">指定資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f78e4-318">Specifies the error information.</span></span> |
| <span data-ttu-id="f78e4-319">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-319">userContext</span></span> | <span data-ttu-id="f78e4-320">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="f78e4-320">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="f78e4-321">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-321">methodName</span></span> | <span data-ttu-id="f78e4-322">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-322">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-323">*defaultSaveCompleted 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-323">*defaultSaveCompleted property (get, set):*</span></span>

<span data-ttu-id="f78e4-324">此屬性會指定應儲存使用者設定檔資料完成時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-324">This property specifies a function that should be called upon the completion of saving the user's profile data.</span></span> <span data-ttu-id="f78e4-325">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-325">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-326">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-326">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

<span data-ttu-id="f78e4-327">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-327">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-328">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-328">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-329">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-329">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-330">numPropsSaved</span><span class="sxs-lookup"><span data-stu-id="f78e4-330">numPropsSaved</span></span> | <span data-ttu-id="f78e4-331">指定已儲存的屬性的數目。</span><span class="sxs-lookup"><span data-stu-id="f78e4-331">Specifies the number of properties that were saved.</span></span> |
| <span data-ttu-id="f78e4-332">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-332">userContext</span></span> | <span data-ttu-id="f78e4-333">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="f78e4-333">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="f78e4-334">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-334">methodName</span></span> | <span data-ttu-id="f78e4-335">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-335">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-336">*defaultLoadCompleted 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-336">*defaultLoadCompleted property (get, set):*</span></span>

<span data-ttu-id="f78e4-337">此屬性會指定應完成載入使用者設定檔資料時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-337">This property specifies a function that should be called upon the completion of loading of the user's profile data.</span></span> <span data-ttu-id="f78e4-338">它應該會收到委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="f78e4-338">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="f78e4-339">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="f78e4-339">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

<span data-ttu-id="f78e4-340">*參數：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-340">*Parameters:*</span></span>

| <span data-ttu-id="f78e4-341">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="f78e4-341">**Parameter Name**</span></span> | <span data-ttu-id="f78e4-342">**意義**</span><span class="sxs-lookup"><span data-stu-id="f78e4-342">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="f78e4-343">numPropsLoaded</span><span class="sxs-lookup"><span data-stu-id="f78e4-343">numPropsLoaded</span></span> | <span data-ttu-id="f78e4-344">指定載入的屬性數目。</span><span class="sxs-lookup"><span data-stu-id="f78e4-344">Specifies the number of properties loaded.</span></span> |
| <span data-ttu-id="f78e4-345">userContext</span><span class="sxs-lookup"><span data-stu-id="f78e4-345">userContext</span></span> | <span data-ttu-id="f78e4-346">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="f78e4-346">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="f78e4-347">方法名稱</span><span class="sxs-lookup"><span data-stu-id="f78e4-347">methodName</span></span> | <span data-ttu-id="f78e4-348">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="f78e4-348">The name of the calling method.</span></span> |

<span data-ttu-id="f78e4-349">*path 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-349">*path property (get, set):*</span></span>

<span data-ttu-id="f78e4-350">此屬性以程式設計方式決定的設定檔的 web 服務的位置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-350">This property programmatically determines the location of the profile web service.</span></span> <span data-ttu-id="f78e4-351">它可以用來覆寫預設設定檔服務提供者，以及一個 ScriptManager 控制項的 ProfileService 子節點的路徑屬性中以宣告方式設定。</span><span class="sxs-lookup"><span data-stu-id="f78e4-351">It can be used to override the default profile service provider, as well as one set declaratively in the Path property of the ScriptManager control's ProfileService child node.</span></span>

<span data-ttu-id="f78e4-352">請注意，不會變更預設設定檔服務的位置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-352">Note that the location of the default profile service does not change.</span></span> <span data-ttu-id="f78e4-353">不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面做為 ASP.NET AJAX 驗證服務 proxy 的位置。</span><span class="sxs-lookup"><span data-stu-id="f78e4-353">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="f78e4-354">請注意這個屬性不應設為值，將指令碼要求導向移出目前的站台。</span><span class="sxs-lookup"><span data-stu-id="f78e4-354">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="f78e4-355">技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f78e4-355">The technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="f78e4-356">這個屬性是`String`物件代表 web 服務設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="f78e4-356">This property is a `String` object representing the path to the profile web service.</span></span>

<span data-ttu-id="f78e4-357">*（get、 set），就會逾時屬性：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-357">*timeout property (get, set):*</span></span>

<span data-ttu-id="f78e4-358">此屬性會判斷失敗的設定檔服務等候假設載入或儲存要求的時間長度。</span><span class="sxs-lookup"><span data-stu-id="f78e4-358">This property determines the length of time to wait for the profile service before assuming the load or save request has failed.</span></span> <span data-ttu-id="f78e4-359">如果超過逾時等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。</span><span class="sxs-lookup"><span data-stu-id="f78e4-359">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="f78e4-360">這個屬性是`Number`物件，代表要等候結果，從設定檔服務的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="f78e4-360">This property is a `Number` object representing the number of milliseconds to wait for results from the profile service.</span></span>

<span data-ttu-id="f78e4-361">*程式碼範例： 載入的頁面載入的設定檔資料*</span><span class="sxs-lookup"><span data-stu-id="f78e4-361">*Code sample: Loading profile data at page load*</span></span>

<span data-ttu-id="f78e4-362">下列程式碼會檢查是否已驗證使用者，並且若是如此，載入使用者的慣用的背景色彩的頁面。</span><span class="sxs-lookup"><span data-stu-id="f78e4-362">The following code will check to see whether a user is authenticated, and if so, will load the user's preferred background color as the page's.</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a><span data-ttu-id="f78e4-363">*使用自訂驗證提供者*</span><span class="sxs-lookup"><span data-stu-id="f78e4-363">*Using a Custom Authentication Service Provider*</span></span>

<span data-ttu-id="f78e4-364">ASP.NET AJAX 擴充功能可讓您建立自訂指令碼驗證服務提供者公開您透過自訂的 web 服務的功能。</span><span class="sxs-lookup"><span data-stu-id="f78e4-364">The ASP.NET AJAX Extensions allow you to create a custom script authentication service provider by exposing your functionality through a custom web service.</span></span> <span data-ttu-id="f78e4-365">才能加以使用，您的 web 服務必須公開兩個方法：`Login`和`Logout`; 這些方法必須指定為預設的 ASP.NET AJAX 驗證 web 服務相同的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="f78e4-365">In order to be used, your web service must expose two methods, `Login` and `Logout`; and these methods must be specified with the same method signatures as the default ASP.NET AJAX Authentication web service.</span></span>

<span data-ttu-id="f78e4-366">當您建立自訂的 web 服務之後時，您必須指定它，不論是以宣告方式在頁面上，路徑的程式碼，以程式設計方式或透過用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="f78e4-366">Once you have created the custom web service, you will need to specify the path to it, either declaratively on your page, programmatically in code, or via the client script.</span></span>

<span data-ttu-id="f78e4-367">*若要以宣告方式設定路徑：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-367">*To set the path declaratively:*</span></span>

<span data-ttu-id="f78e4-368">若要以宣告方式設定的路徑，包含 AuthenticationService 物件的子節點 ScriptManager ASP.NET 網頁上：</span><span class="sxs-lookup"><span data-stu-id="f78e4-368">To set the path declaratively, include the AuthenticationService child of the ScriptManager object on your ASP.NET page:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

<span data-ttu-id="f78e4-369">*若要設定程式碼中的路徑：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-369">*To set the path in code:*</span></span>

<span data-ttu-id="f78e4-370">若要以程式設計方式設定的路徑，指定透過指令碼管理員的執行個體的路徑：</span><span class="sxs-lookup"><span data-stu-id="f78e4-370">To set the path programmatically, specify the path via the instance of your script manager:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

<span data-ttu-id="f78e4-371">*若要設定的指令碼中的路徑：*</span><span class="sxs-lookup"><span data-stu-id="f78e4-371">*To set the path in script:*</span></span>

<span data-ttu-id="f78e4-372">若要以程式設計方式設定的路徑，指令碼中，利用`path`AuthenticationService 類別的屬性：</span><span class="sxs-lookup"><span data-stu-id="f78e4-372">To set the path programmatically in script, utilize the `path` property of the AuthenticationService class:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

<span data-ttu-id="f78e4-373">*自訂驗證的範例 Web 服務*</span><span class="sxs-lookup"><span data-stu-id="f78e4-373">*Sample Web Service for Custom Authentication*</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a><span data-ttu-id="f78e4-374">總結</span><span class="sxs-lookup"><span data-stu-id="f78e4-374">Summary</span></span>

<span data-ttu-id="f78e4-375">在用戶端瀏覽器，ASP.NET 服務-特別的程式碼剖析、 成員資格，以及驗證服務-輕鬆地會公開給 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="f78e4-375">ASP.NET services - specifically the profiling, membership, and authentication services - are easily exposed to JavaScript on the client browser.</span></span> <span data-ttu-id="f78e4-376">這可讓開發人員不必依靠控制項，例如執行困難的 updatepanel 就緊密，整合其用戶端程式碼使用的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="f78e4-376">This allows developers to integrate their client-side code with the authentication mechanism seamlessly, without depending on controls such as UpdatePanels to do the heavy lifting.</span></span> <span data-ttu-id="f78e4-377">藉由使用 web 組態設定; 用戶端，也可以保護設定檔資料沒有資料可根據預設，同時開發人員必須在選擇設定檔屬性。</span><span class="sxs-lookup"><span data-stu-id="f78e4-377">Profile data can be protected from the client as well, by utilizing web configuration settings; no data is available by default, and developers must opt-in to profile properties.</span></span>

<span data-ttu-id="f78e4-378">此外，藉由使用等的方法簽章建立簡化的 web 服務實作，開發人員可以建立這些內建 ASP.NET 服務的自訂指令碼提供者。</span><span class="sxs-lookup"><span data-stu-id="f78e4-378">Furthermore, by creating simplified web service implementations with equivalent method signatures, developers can create custom script providers for these intrinsic ASP.NET services.</span></span> <span data-ttu-id="f78e4-379">如需這些技術的支援可簡化豐富型用戶端應用程式，同時開發人員提供各種不同的彈性，以符合特定需求的開發。</span><span class="sxs-lookup"><span data-stu-id="f78e4-379">Support for these techniques simplifies the development of rich client applications, while providing developers with a wide range of flexibility to meet specific needs.</span></span>

## <a name="bio"></a><span data-ttu-id="f78e4-380">*簡歷*</span><span class="sxs-lookup"><span data-stu-id="f78e4-380">*Bio*</span></span>

<span data-ttu-id="f78e4-381">Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f78e4-381">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="f78e4-382">透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="f78e4-382">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f78e4-383">[上一頁](understanding-asp-net-ajax-updatepanel-triggers.md)
[下一頁](understanding-asp-net-ajax-localization.md)</span><span class="sxs-lookup"><span data-stu-id="f78e4-383">[Previous](understanding-asp-net-ajax-updatepanel-triggers.md)
[Next](understanding-asp-net-ajax-localization.md)</span></span>
