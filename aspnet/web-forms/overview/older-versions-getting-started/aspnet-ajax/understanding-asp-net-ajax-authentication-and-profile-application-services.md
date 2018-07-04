---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: 了解 ASP.NET AJAX 驗證和設定檔應用程式服務 |Microsoft Docs
author: scottcate
description: 驗證服務，可讓使用者提供認證以接收驗證 cookie，且閘道服務，讓自訂使用者...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 323fec56f18281b5b5a3d312a2e4c4c7133e3f03
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393057"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a><span data-ttu-id="4ecf7-103">了解 ASP.NET AJAX 驗證和設定檔應用程式服務</span><span class="sxs-lookup"><span data-stu-id="4ecf7-103">Understanding ASP.NET AJAX Authentication and Profile Application Services</span></span>
====================
<span data-ttu-id="4ecf7-104">藉由[Scott Cate](https://github.com/scottcate)</span><span class="sxs-lookup"><span data-stu-id="4ecf7-104">by [Scott Cate](https://github.com/scottcate)</span></span>

[<span data-ttu-id="4ecf7-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="4ecf7-105">Download PDF</span></span>](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> <span data-ttu-id="4ecf7-106">驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-106">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="4ecf7-107">ASP.NET AJAX 驗證服務，使用適用於標準的 ASP.NET 表單驗證，因此目前正在使用表單驗證的應用程式 （例如與登入控制） 將不會中斷藉由升級至 AJAX 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-107">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>


## <a name="introduction"></a><span data-ttu-id="4ecf7-108">簡介</span><span class="sxs-lookup"><span data-stu-id="4ecf7-108">Introduction</span></span>

<span data-ttu-id="4ecf7-109">.NET Framework 3.5 的一部分，Microsoft 正逐步實現的可調整大小的環境升級;不只是新的開發環境，但新的 Language-Integrated Query (LINQ) 功能，以及其他語言增強功能即將推出。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-109">As part of the .NET Framework 3.5, Microsoft is delivering a sizable environment upgrade; not only is a new development environment available, but the new Language-Integrated Query (LINQ) features and other language enhancements are forthcoming.</span></span> <span data-ttu-id="4ecf7-110">此外，有些熟悉的功能的其他工具組，值得注意的是 ASP.NET AJAX Extensions 中所包含當做最上級成員的.NET Framework 基底類別庫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-110">In addition, some familiar features of other toolsets, notably the ASP.NET AJAX Extensions, are being included as first-class members of the .NET Framework Base Class Library.</span></span> <span data-ttu-id="4ecf7-111">這些擴充功能可讓許多豐富的用戶端的新功能，包括部分呈現的頁面，而不需要重新整理整個頁面，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），以及廣泛的用戶端 API 存取 Web 服務的能力設計用來鏡像處理許多 ASP.NET 伺服器端控制項集合中的控制項配置。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-111">These extensions enable many new rich client features, including partial rendering of pages without requiring a full page refresh, the ability to access Web Services via client script (including the ASP.NET profiling API), and an extensive client-side API designed to mirror many of the control schemes seen in the ASP.NET server-side control set.</span></span>

<span data-ttu-id="4ecf7-112">本白皮書探討如何實作和使用 ASP.NET 程式碼剖析和表單驗證服務公開的 Microsoft ASP.NET AJAX ExtensionsThe AJAX 擴充功能，讓表單驗證非常輕鬆地支援，因為它 （以及「 分析 」 服務） 會公開透過 Web 服務 proxy 指令碼。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-112">This whitepaper looks at the implementation and use of the ASP.NET Profiling and Forms Authentication services as they are exposed by the Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions make Forms authentication incredibly easy to support, as it (as well as the Profiling Service) is exposed through a Web Service proxy script.</span></span> <span data-ttu-id="4ecf7-113">AJAX 擴充功能也支援透過 AuthenticationServiceManager 類別的自訂驗證。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-113">The AJAX Extensions also support custom authentication through the AuthenticationServiceManager class.</span></span>

<span data-ttu-id="4ecf7-114">本白皮書根據 Visual Studio 2008 Beta 2 版和.NET Framework 3.5。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-114">This whitepaper is based on the Beta 2 release of the Visual Studio 2008 and the .NET Framework 3.5.</span></span> <span data-ttu-id="4ecf7-115">本白皮書也會假設您將使用 Visual Studio 2008 Beta 2，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-115">This whitepaper also assumes that you will be working with Visual Studio 2008 Beta 2, not Visual Web Developer Express, and will provide walkthroughs according to the user interface of Visual Studio.</span></span> <span data-ttu-id="4ecf7-116">某些程式碼範例可利用無法在 Visual Web Developer Express 的專案範本。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-116">Some code samples may utilize project templates unavailable in Visual Web Developer Express.</span></span>

## <a name="profiles-and-authentication"></a><span data-ttu-id="4ecf7-117">*設定檔和驗證*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-117">*Profiles and Authentication*</span></span>

<span data-ttu-id="4ecf7-118">Microsoft ASP.NET 設定檔和驗證服務所提供的 ASP.NET 表單驗證系統，ASP.NET 的標準元件。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-118">The Microsoft ASP.NET Profiles and Authentication services are provided by the ASP.NET Forms Authentication system, and are standard components of ASP.NET.</span></span> <span data-ttu-id="4ecf7-119">ASP.NET AJAX Extensions 提供指令碼存取這些服務透過指令碼的 proxy，透過非常簡單的模型 Sys.Services 命名空間下的用戶端 AJAX 程式庫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-119">The ASP.NET AJAX Extensions provide script access to these services via script proxies, through a fairly straightforward model under the Sys.Services namespace of the client AJAX library.</span></span>

<span data-ttu-id="4ecf7-120">驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-120">The Authentication service allows users to provide credentials in order to receive an authentication cookie, and is the gateway service to allow custom user profiles provided by ASP.NET.</span></span> <span data-ttu-id="4ecf7-121">ASP.NET AJAX 驗證服務，使用適用於標準的 ASP.NET 表單驗證，因此目前正在使用表單驗證的應用程式 （例如與登入控制） 將不會中斷藉由升級至 AJAX 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-121">Use of the ASP.NET AJAX authentication service is compatible with standard ASP.NET Forms authentication, so applications currently using Forms authentication (such as with the Login control) will not be broken by upgrading to the AJAX authentication service.</span></span>

<span data-ttu-id="4ecf7-122">根據驗證服務所提供的成員資格的使用者資料的儲存體與自動整合，可讓設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-122">The Profile service allows the automatic integration and storage of user data based on membership as provided by the Authentication service.</span></span> <span data-ttu-id="4ecf7-123">Web.config 檔案中，指定儲存的資料和各種程式碼剖析的服務提供者處理資料管理。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-123">The stored data is specified by the web.config file, and the various profiling service providers handle the data management.</span></span> <span data-ttu-id="4ecf7-124">如同驗證服務，使頁面目前併入 ASP.NET 設定檔服務的功能應該不會中斷包含 AJAX 支援，是相容於標準的 ASP.NET 設定檔服務，AJAX 設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-124">As with the Authentication service, the AJAX Profile service is compatible with the standard ASP.NET profile service, so that pages currently incorporating features of the ASP.NET Profile service should not be broken by including AJAX support.</span></span>

<span data-ttu-id="4ecf7-125">應用程式中併入 ASP.NET 驗證和分析服務本身是在本白皮書的範圍之外。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-125">Incorporating the ASP.NET Authentication and Profiling services themselves into an application is outside of the scope of this whitepaper.</span></span> <span data-ttu-id="4ecf7-126">如需有關本主題的詳細資訊，請參閱 MSDN Library 參考文章使用成員資格管理使用者在[ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-126">For more information on the topic, see the MSDN Library reference article Managing Users by Using Membership at [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx).</span></span> <span data-ttu-id="4ecf7-127">ASP.NET 也會包含一個公用程式來自動設定與 SQL Server，也就是 ASP.NET 成員資格的預設驗證服務提供者的成員資格。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-127">ASP.NET also includes a utility to automatically set up Membership with a SQL Server, which is the default authentication service provider for ASP.NET Membership.</span></span> <span data-ttu-id="4ecf7-128">如需詳細資訊，請參閱文章 ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe) 在[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-128">For more information, see the article ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe) at [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).</span></span>

## <a name="using-the-aspnet-ajax-authentication-service"></a><span data-ttu-id="4ecf7-129">*使用 ASP.NET AJAX 驗證服務*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-129">*Using the ASP.NET AJAX Authentication Service*</span></span>

<span data-ttu-id="4ecf7-130">必須啟用 ASP.NET AJAX 驗證服務的 web.config 檔案中：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-130">The ASP.NET AJAX Authentication service must be enabled in the web.config file:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

<span data-ttu-id="4ecf7-131">驗證服務需要啟用 ASP.NET 表單驗證，而且需要在用戶端瀏覽器 （因為無 cookie 工作階段需要 URL 參數，指令碼無法啟用 cookie 的工作階段） 上啟用 cookie。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-131">The Authentication service requires ASP.NET Forms authentication to be enabled and requires cookies to be enabled on the client browser (a script cannot enable a cookieless session since cookieless sessions require URL parameters).</span></span>

<span data-ttu-id="4ecf7-132">一旦啟用 AJAX 驗證服務，並設定，用戶端指令碼可以立即利用 Sys.Services.AuthenticationService 物件。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-132">Once the AJAX authentication service is enabled and configured, client script can immediately take advantage of the Sys.Services.AuthenticationService object.</span></span> <span data-ttu-id="4ecf7-133">主要用戶端指令碼也會想要善用`login`方法和`isLoggedIn`屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-133">Primarily, client script will want to take advantage of the `login` method and `isLoggedIn` property.</span></span> <span data-ttu-id="4ecf7-134">若要登入方法，可接受的參數提供預設值的數個屬性存在。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-134">Several properties exist to provide defaults for the login method, which can accept a large number of parameters.</span></span>

<span data-ttu-id="4ecf7-135">*Sys.Services.AuthenticationService 成員*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-135">*Sys.Services.AuthenticationService members*</span></span>

<span data-ttu-id="4ecf7-136">*登入方法：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-136">*login method:*</span></span>

<span data-ttu-id="4ecf7-137">Login （） 方法開始的要求，以驗證使用者的認證。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-137">The login() method begins a request to authenticate the user's credentials.</span></span> <span data-ttu-id="4ecf7-138">這個方法是非同步的而且不會封鎖執行。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-138">This method is asynchronous and does not block execution.</span></span>

<span data-ttu-id="4ecf7-139">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-139">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-140">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-140">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-141">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-141">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-142">userName</span><span class="sxs-lookup"><span data-stu-id="4ecf7-142">userName</span></span> | <span data-ttu-id="4ecf7-143">必要。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-143">Required.</span></span> <span data-ttu-id="4ecf7-144">要驗證的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-144">The username to authenticate.</span></span> |
| <span data-ttu-id="4ecf7-145">密碼</span><span class="sxs-lookup"><span data-stu-id="4ecf7-145">password</span></span> | <span data-ttu-id="4ecf7-146">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-146">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-147">使用者的密碼。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-147">The user's password.</span></span> |
| <span data-ttu-id="4ecf7-148">isPersistent</span><span class="sxs-lookup"><span data-stu-id="4ecf7-148">isPersistent</span></span> | <span data-ttu-id="4ecf7-149">選擇性 （預設為 false）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-149">Optional (defaults to false).</span></span> <span data-ttu-id="4ecf7-150">不論使用者的驗證 cookie 應該跨工作階段保存。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-150">Whether the user's authentication cookie should persist across sessions.</span></span> <span data-ttu-id="4ecf7-151">如果為 false，使用者將登出時就會關閉瀏覽器或工作階段到期。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-151">If false, the user will log out when the browser is closed or the session expires.</span></span> |
| <span data-ttu-id="4ecf7-152">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="4ecf7-152">redirectUrl</span></span> | <span data-ttu-id="4ecf7-153">選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-153">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="4ecf7-154">如果此參數為 null 或空字串，就會不發生任何重新導向。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-154">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="4ecf7-155">customInfo</span><span class="sxs-lookup"><span data-stu-id="4ecf7-155">customInfo</span></span> | <span data-ttu-id="4ecf7-156">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-156">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-157">這個參數是目前未使用，並且會保留供未來使用。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-157">This parameter is currently unused and is reserved for future use.</span></span> |
| <span data-ttu-id="4ecf7-158">loginCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-158">loginCompletedCallback</span></span> | <span data-ttu-id="4ecf7-159">選擇性 （預設為 null）。登入已順利完成時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-159">Optional (defaults to null).The function to call when the login has successfully completed.</span></span> <span data-ttu-id="4ecf7-160">如果指定，此參數會覆寫 defaultLoginCompleted 屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-160">If specified, this parameter overrides the defaultLoginCompleted property.</span></span> |
| <span data-ttu-id="4ecf7-161">failedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-161">failedCallback</span></span> | <span data-ttu-id="4ecf7-162">選擇性 （預設為 null）。登入失敗時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-162">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="4ecf7-163">如果指定，此參數會覆寫 defaultFailedCallback 屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-163">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="4ecf7-164">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-164">userContext</span></span> | <span data-ttu-id="4ecf7-165">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-165">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-166">應該傳遞至回呼函式的自訂使用者內容資料。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-166">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="4ecf7-167">*傳回值：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-167">*Return Value:*</span></span>

<span data-ttu-id="4ecf7-168">此函式不包含傳回的值。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-168">This function does not include a return value.</span></span> <span data-ttu-id="4ecf7-169">不過，一些行為是呼叫此函式完成時包含：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-169">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="4ecf7-170">目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數不是 null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-170">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="4ecf7-171">不過，如果參數為 null 或空字串，`loginCompletedCallback`參數，或`defaultLoginCompletedCallback`稱為屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-171">However, if the parameter was null or an empty string, the `loginCompletedCallback` parameter, or `defaultLoginCompletedCallback` property is called.</span></span>
- <span data-ttu-id="4ecf7-172">如果 web 服務呼叫失敗，`failedCallback`參數的`defaultFailedCallback`稱為屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-172">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="4ecf7-173">*登出方法：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-173">*logout method:*</span></span>

<span data-ttu-id="4ecf7-174">Logout() 方法移除認證 cookie，並將目前的使用者從 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-174">The logout() method removes the credentials cookie and logs out the current user from the web application.</span></span>

<span data-ttu-id="4ecf7-175">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-175">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-176">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-176">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-177">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-177">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-178">redirectUrl</span><span class="sxs-lookup"><span data-stu-id="4ecf7-178">redirectUrl</span></span> | <span data-ttu-id="4ecf7-179">選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-179">Optional (defaults to null).The URL to redirect the browser to upon successful authentication.</span></span> <span data-ttu-id="4ecf7-180">如果此參數為 null 或空字串，就會不發生任何重新導向。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-180">If this parameter is null or an empty string, no redirection occurs.</span></span> |
| <span data-ttu-id="4ecf7-181">logoutCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-181">logoutCompletedCallback</span></span> | <span data-ttu-id="4ecf7-182">選擇性 （預設為 null）。當登出已順利完成時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-182">Optional (defaults to null).The function to call when the logout has successfully completed.</span></span> <span data-ttu-id="4ecf7-183">如果指定，此參數會覆寫 defaultLogoutCompleted 屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-183">If specified, this parameter overrides the defaultLogoutCompleted property.</span></span> |
| <span data-ttu-id="4ecf7-184">failedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-184">failedCallback</span></span> | <span data-ttu-id="4ecf7-185">選擇性 （預設為 null）。登入失敗時要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-185">Optional (defaults to null).The function to call when the login has failed.</span></span> <span data-ttu-id="4ecf7-186">如果指定，此參數會覆寫 defaultFailedCallback 屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-186">If specified, this parameter overrides the defaultFailedCallback property.</span></span> |
| <span data-ttu-id="4ecf7-187">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-187">userContext</span></span> | <span data-ttu-id="4ecf7-188">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-188">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-189">應該傳遞至回呼函式的自訂使用者內容資料。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-189">Custom user context data that should be passed to the callback functions.</span></span> |

<span data-ttu-id="4ecf7-190">*傳回值：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-190">*Return Value:*</span></span>

<span data-ttu-id="4ecf7-191">此函式不包含傳回的值。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-191">This function does not include a return value.</span></span> <span data-ttu-id="4ecf7-192">不過，一些行為是呼叫此函式完成時包含：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-192">However, a number of behaviors are included upon completion of a call to this function:</span></span>

- <span data-ttu-id="4ecf7-193">目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數不是 null 或空字串。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-193">The current page will either be refreshed or be changed if the `redirectUrl` parameter was neither null nor an empty string.</span></span>
- <span data-ttu-id="4ecf7-194">不過，如果參數為 null 或空字串，`logoutCompletedCallback`參數，或`defaultLogoutCompletedCallback`稱為屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-194">However, if the parameter was null or an empty string, the `logoutCompletedCallback` parameter, or `defaultLogoutCompletedCallback` property is called.</span></span>
- <span data-ttu-id="4ecf7-195">如果 web 服務呼叫失敗，`failedCallback`參數的`defaultFailedCallback`稱為屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-195">If the call to the web service fails, the `failedCallback` parameter of `defaultFailedCallback` property is called.</span></span>

<span data-ttu-id="4ecf7-196">*defaultFailedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-196">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="4ecf7-197">此屬性會指定應該在發生與 web 服務通訊失敗時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-197">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="4ecf7-198">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-198">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-199">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-199">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

<span data-ttu-id="4ecf7-200">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-200">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-201">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-201">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-202">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-202">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-203">個錯誤</span><span class="sxs-lookup"><span data-stu-id="4ecf7-203">error</span></span> | <span data-ttu-id="4ecf7-204">指定資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-204">Specifies the error information.</span></span> |
| <span data-ttu-id="4ecf7-205">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-205">userContext</span></span> | <span data-ttu-id="4ecf7-206">指定的登入或登出函式呼叫時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-206">Specifies the user context information provided when the login or logout function was called.</span></span> |
| <span data-ttu-id="4ecf7-207">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-207">methodName</span></span> | <span data-ttu-id="4ecf7-208">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-208">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-209">*defaultLoginCompletedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-209">*defaultLoginCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="4ecf7-210">此屬性會指定應該在登入 web 服務呼叫完成時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-210">This property specifies a function that should be called when the login web service call has completed.</span></span> <span data-ttu-id="4ecf7-211">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-211">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-212">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-212">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

<span data-ttu-id="4ecf7-213">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-213">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-214">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-214">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-215">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-215">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-216">validCredentials</span><span class="sxs-lookup"><span data-stu-id="4ecf7-216">validCredentials</span></span> | <span data-ttu-id="4ecf7-217">指定使用者是否提供有效的認證。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-217">Specifies whether the user provided valid credentials.</span></span> <span data-ttu-id="4ecf7-218">`true` 如果使用者成功登入;否則`false`。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-218">`true` if the user successfully logged in; otherwise `false`.</span></span> |
| <span data-ttu-id="4ecf7-219">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-219">userContext</span></span> | <span data-ttu-id="4ecf7-220">指定呼叫 login 函式時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-220">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="4ecf7-221">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-221">methodName</span></span> | <span data-ttu-id="4ecf7-222">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-222">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-223">*defaultLogoutCompletedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-223">*defaultLogoutCompletedCallback property (get, set):*</span></span>

<span data-ttu-id="4ecf7-224">此屬性指定應在登出的 web 服務呼叫完成時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-224">This property specifies a function that should be called when the logout web service call has completed.</span></span> <span data-ttu-id="4ecf7-225">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-225">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-226">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-226">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

<span data-ttu-id="4ecf7-227">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-227">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-228">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-228">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-229">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-229">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-230">結果</span><span class="sxs-lookup"><span data-stu-id="4ecf7-230">result</span></span> | <span data-ttu-id="4ecf7-231">這個參數會一律是`null`; 它保留供日後使用。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-231">This parameter will always be `null`; it is reserved for future use.</span></span> |
| <span data-ttu-id="4ecf7-232">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-232">userContext</span></span> | <span data-ttu-id="4ecf7-233">指定呼叫 login 函式時所提供的使用者內容資訊。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-233">Specifies the user context information provided when the login function was called.</span></span> |
| <span data-ttu-id="4ecf7-234">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-234">methodName</span></span> | <span data-ttu-id="4ecf7-235">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-235">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-236">*isLoggedIn 屬性 (get):*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-236">*isLoggedIn property (get):*</span></span>

<span data-ttu-id="4ecf7-237">這個屬性會取得目前驗證狀態的使用者資訊。它是由 ScriptManager 物件在設定頁面要求。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-237">This property gets the current authentication state of the user; it is set by the ScriptManager object during a page request.</span></span>

<span data-ttu-id="4ecf7-238">這個屬性會傳回`true`如果使用者目前記錄中; 否則它會傳回`false`。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-238">This property returns `true` if the user is currently logged in; otherwise, it returns `false`.</span></span>

<span data-ttu-id="4ecf7-239">*（get、 set） 的路徑屬性：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-239">*path property (get, set):*</span></span>

<span data-ttu-id="4ecf7-240">此屬性以程式設計方式決定驗證 web 服務的位置。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-240">This property programmatically determines the location of the authentication web service.</span></span> <span data-ttu-id="4ecf7-241">它可以用來覆寫預設的驗證提供者，以及一個 ScriptManager 控制項的 AuthenticationService 子節點的路徑屬性中以宣告方式設定 (如需詳細資訊，請參閱使用自訂驗證服務提供者下列主題）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-241">It can be used to override the default authentication provider, as well as one set declaratively in the Path property of the ScriptManager control's AuthenticationService child node (for more information, see the Using a Custom Authentication Service Provider topic below).</span></span>

<span data-ttu-id="4ecf7-242">請注意，預設驗證服務的位置不會變更。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-242">Note that the location of the default authentication service does not change.</span></span> <span data-ttu-id="4ecf7-243">不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面，做為 ASP.NET AJAX 驗證服務 proxy 的位置。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-243">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="4ecf7-244">也請注意這個屬性不應設為值，將指令碼要求導向從目前的站台。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-244">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="4ecf7-245">目前的應用程式不會收到的驗證認證，因為它會是毫無用處;此外，技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-245">Because the current application would not receive the authentication credentials, it would be useless; also, the technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="4ecf7-246">這個屬性是`String`物件，表示驗證 web 服務的路徑。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-246">This property is a `String` object representing the path to the authentication web service.</span></span>

<span data-ttu-id="4ecf7-247">*（get、 set），就會逾時屬性：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-247">*timeout property (get, set):*</span></span>

<span data-ttu-id="4ecf7-248">此屬性會判斷失敗的驗證服務前假設登入要求等待的時間長度。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-248">This property determines the length of time to wait for the authentication service before assuming the login request has failed.</span></span> <span data-ttu-id="4ecf7-249">如果在逾時到期等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-249">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="4ecf7-250">這個屬性是`Number`物件，表示要等候從驗證服務的搜尋結果的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-250">This property is a `Number` object representing the number of milliseconds to wait for results from the authentication service.</span></span>

<span data-ttu-id="4ecf7-251">*登入驗證服務的程式碼範例：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-251">*Code Sample: Logging into the Authentication Service*</span></span>

<span data-ttu-id="4ecf7-252">下列標記是簡單的指令碼呼叫的登入和登出方法 AuthenticationService 類別的範例 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-252">The following markup is an example ASP.NET page with a simple script call to the login and logout methods of the AuthenticationService class.</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a><span data-ttu-id="4ecf7-253">存取程式碼剖析資料透過 AJAX 的 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4ecf7-253">Accessing ASP.NET Profiling Data via AJAX</span></span>

<span data-ttu-id="4ecf7-254">ASP.NET 程式碼剖析服務也會公開透過 ASP.NET AJAX Extensions 中。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-254">The ASP.NET profiling service is also exposed through the ASP.NET AJAX Extensions.</span></span> <span data-ttu-id="4ecf7-255">由於 ASP.NET 程式碼剖析服務提供的豐富、 細微的 API，用來儲存和擷取使用者資料，這可以是一個絕佳的生產力工具。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-255">Since the ASP.NET profiling service provides a rich, granular API by which to store and retrieve user data, this can be an excellent productivity tool.</span></span>

<span data-ttu-id="4ecf7-256">設定檔服務必須能夠在 web.config 中;它不是預設。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-256">The profile service must be enabled in web.config; it is not by default.</span></span> <span data-ttu-id="4ecf7-257">若要這樣做，請確定`profileService`子項目已啟用 = true 中所指定的 web.config，和指定的屬性可以讀取或寫入，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-257">To do so, ensure that the `profileService` child element has enabled= true specified in web.config, and that you have specified which properties can be read or written as follows:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

<span data-ttu-id="4ecf7-258">也必須設定設定檔服務。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-258">The profile service must also be configured.</span></span> <span data-ttu-id="4ecf7-259">雖然本白皮書的範圍外的程式碼剖析的服務組態，或許值得注意會有可存取子屬性群組名稱為設定檔組態設定中所定義的群組。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-259">Although configuration of the profiling service is outside of the scope of this whitepaper, it is worthwhile to note that groups as defined in profile configuration settings will be accessible as sub-properties of the group name.</span></span> <span data-ttu-id="4ecf7-260">例如，使用指定的下列設定檔區段：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-260">For example, with the following profile section specified:</span></span>

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

<span data-ttu-id="4ecf7-261">用戶端指令碼能夠存取名稱、 Address.Line1、 Address.Line2、 Address.City、 Address.State、 Address.Zip 及 BackgroundColor ProfileService 類別的 [屬性] 欄位的屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-261">Client script would be able to access Name, Address.Line1, Address.Line2, Address.City, Address.State, Address.Zip, and BackgroundColor as properties of the properties field of the ProfileService class.</span></span>

<span data-ttu-id="4ecf7-262">一旦設定了 AJAX 程式碼剖析服務，它就可以立即在頁面;不過，它必須載入一次使用之前。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-262">Once the AJAX Profiling Service is configured, it will be immediately available in pages; however, it will have to be loaded once before use.</span></span>

<span data-ttu-id="4ecf7-263">*Sys.Services.ProfileService 成員*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-263">*Sys.Services.ProfileService members*</span></span>

<span data-ttu-id="4ecf7-264">*屬性欄位：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-264">*properties field:*</span></span>

<span data-ttu-id="4ecf7-265">[屬性] 欄位會公開為.運算子名稱慣例可以參考的子屬性的所有設定的設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-265">The properties field exposes all configured profile data as child properties that can be referenced by the dot-operator-name convention.</span></span> <span data-ttu-id="4ecf7-266">屬性群組的子系的屬性稱為 GroupName.PropertyName。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-266">Properties that are children of property groups are referred to as GroupName.PropertyName.</span></span> <span data-ttu-id="4ecf7-267">在上述範例設定檔的組態，以取得狀態的使用者，您可以使用下列識別碼：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-267">In the example profile configuration presented above, to get the state of the user, you could use the following identifier:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

<span data-ttu-id="4ecf7-268">*load 方法：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-268">*load method:*</span></span>

<span data-ttu-id="4ecf7-269">從伺服器載入精選的清單或所有屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-269">Loads a selected list or all properties from the server.</span></span>

<span data-ttu-id="4ecf7-270">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-270">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-271">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-271">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-272">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-272">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-273">之 propertyNames</span><span class="sxs-lookup"><span data-stu-id="4ecf7-273">propertyNames</span></span> | <span data-ttu-id="4ecf7-274">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-274">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-275">要從伺服器載入的屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-275">The properties to be loaded from the server.</span></span> |
| <span data-ttu-id="4ecf7-276">loadCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-276">loadCompletedCallback</span></span> | <span data-ttu-id="4ecf7-277">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-277">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-278">已完成載入時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-278">The function to call when loading has completed.</span></span> |
| <span data-ttu-id="4ecf7-279">failedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-279">failedCallback</span></span> | <span data-ttu-id="4ecf7-280">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-280">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-281">發生錯誤時所要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-281">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="4ecf7-282">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-282">userContext</span></span> | <span data-ttu-id="4ecf7-283">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-283">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-284">要傳遞至回呼函式的內容資訊。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-284">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="4ecf7-285">Load 函式沒有傳回值。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-285">The load function does not have a return value.</span></span> <span data-ttu-id="4ecf7-286">如果在呼叫已順利完成時，它會呼叫`loadCompletedCallback`參數或`defaultLoadCompletedCallback`屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-286">If the call completed successfully, it will call either the `loadCompletedCallback` parameter or `defaultLoadCompletedCallback` property.</span></span> <span data-ttu-id="4ecf7-287">如果呼叫失敗，或逾時過期，請`failedCallback`參數或`defaultFailedCallback`屬性將會呼叫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-287">If the call failed, or the timeout expired, either the `failedCallback` parameter or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="4ecf7-288">如果`propertyNames`未提供參數，從伺服器擷取所有的讀取設定屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-288">If the `propertyNames` parameter is not supplied, all read-configured properties are retrieved from the server.</span></span>

<span data-ttu-id="4ecf7-289">*save 方法：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-289">*save method:*</span></span>

<span data-ttu-id="4ecf7-290">Save （） 方法會將使用者的 ASP.NET 設定檔指定的屬性清單 （或所有屬性）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-290">The save() method saves the specified property list (or all properties) to the user's ASP.NET profile.</span></span>

<span data-ttu-id="4ecf7-291">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-291">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-292">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-292">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-293">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-293">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-294">之 propertyNames</span><span class="sxs-lookup"><span data-stu-id="4ecf7-294">propertyNames</span></span> | <span data-ttu-id="4ecf7-295">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-295">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-296">要儲存到伺服器的屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-296">The properties to be saved to the server.</span></span> |
| <span data-ttu-id="4ecf7-297">saveCompletedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-297">saveCompletedCallback</span></span> | <span data-ttu-id="4ecf7-298">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-298">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-299">儲存時所呼叫的函式已完成。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-299">The function to call when saving has completed.</span></span> |
| <span data-ttu-id="4ecf7-300">failedCallback</span><span class="sxs-lookup"><span data-stu-id="4ecf7-300">failedCallback</span></span> | <span data-ttu-id="4ecf7-301">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-301">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-302">發生錯誤時所要呼叫函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-302">The function to call if an error occurs.</span></span> |
| <span data-ttu-id="4ecf7-303">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-303">userContext</span></span> | <span data-ttu-id="4ecf7-304">選擇性 （預設為 null）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-304">Optional (defaults to null).</span></span> <span data-ttu-id="4ecf7-305">要傳遞至回呼函式的內容資訊。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-305">Context information to be passed to the callback function.</span></span> |

<span data-ttu-id="4ecf7-306">儲存函式沒有傳回值。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-306">The save function does not have a return value.</span></span> <span data-ttu-id="4ecf7-307">如果呼叫成功完成，它會呼叫`saveCompletedCallback`參數或`defaultSaveCompletedCallback`屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-307">If the call completes successfully, it will call either the `saveCompletedCallback` parameter or `defaultSaveCompletedCallback` property.</span></span> <span data-ttu-id="4ecf7-308">如果呼叫失敗，或逾時過期，請`failedCallback`或`defaultFailedCallback`屬性將會呼叫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-308">If the call failed, or the timeout expired, either the `failedCallback` or `defaultFailedCallback` property will be called.</span></span>

<span data-ttu-id="4ecf7-309">如果`propertyNames`參數為 null，所有的設定檔內容將會傳送到伺服器，伺服器會決定哪些屬性可以儲存、 哪些不可安裝。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-309">If the `propertyNames` parameter is null, all profile properties will be sent to the server, and the server will decide which properties can be saved and which cannot.</span></span>

<span data-ttu-id="4ecf7-310">*defaultFailedCallback 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-310">*defaultFailedCallback property (get, set):*</span></span>

<span data-ttu-id="4ecf7-311">此屬性會指定應該在發生與 web 服務通訊失敗時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-311">This property specifies a function that should be called if a failure to communicate with the web service occurs.</span></span> <span data-ttu-id="4ecf7-312">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-312">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-313">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-313">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

<span data-ttu-id="4ecf7-314">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-314">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-315">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-315">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-316">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-316">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-317">錯誤</span><span class="sxs-lookup"><span data-stu-id="4ecf7-317">Error</span></span> | <span data-ttu-id="4ecf7-318">指定資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-318">Specifies the error information.</span></span> |
| <span data-ttu-id="4ecf7-319">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-319">userContext</span></span> | <span data-ttu-id="4ecf7-320">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-320">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="4ecf7-321">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-321">methodName</span></span> | <span data-ttu-id="4ecf7-322">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-322">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-323">*defaultSaveCompleted 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-323">*defaultSaveCompleted property (get, set):*</span></span>

<span data-ttu-id="4ecf7-324">此屬性指定應儲存使用者設定檔資料，完成時呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-324">This property specifies a function that should be called upon the completion of saving the user's profile data.</span></span> <span data-ttu-id="4ecf7-325">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-325">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-326">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-326">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

<span data-ttu-id="4ecf7-327">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-327">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-328">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-328">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-329">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-329">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-330">numPropsSaved</span><span class="sxs-lookup"><span data-stu-id="4ecf7-330">numPropsSaved</span></span> | <span data-ttu-id="4ecf7-331">指定已儲存的屬性的數目。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-331">Specifies the number of properties that were saved.</span></span> |
| <span data-ttu-id="4ecf7-332">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-332">userContext</span></span> | <span data-ttu-id="4ecf7-333">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-333">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="4ecf7-334">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-334">methodName</span></span> | <span data-ttu-id="4ecf7-335">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-335">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-336">*defaultLoadCompleted 屬性 （get、 set）：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-336">*defaultLoadCompleted property (get, set):*</span></span>

<span data-ttu-id="4ecf7-337">此屬性會指定使用者的設定檔資料載入完成時，應該呼叫的函式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-337">This property specifies a function that should be called upon the completion of loading of the user's profile data.</span></span> <span data-ttu-id="4ecf7-338">它應該會收到的委派 （或函式參考）。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-338">It should receive a delegate (or function reference).</span></span>

<span data-ttu-id="4ecf7-339">這個屬性所指定的函式參考應該具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-339">The function reference specified by this property should have the following signature:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

<span data-ttu-id="4ecf7-340">*參數：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-340">*Parameters:*</span></span>

| <span data-ttu-id="4ecf7-341">**參數名稱**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-341">**Parameter Name**</span></span> | <span data-ttu-id="4ecf7-342">**意義**</span><span class="sxs-lookup"><span data-stu-id="4ecf7-342">**Meaning**</span></span> |
| --- | --- |
| <span data-ttu-id="4ecf7-343">numPropsLoaded</span><span class="sxs-lookup"><span data-stu-id="4ecf7-343">numPropsLoaded</span></span> | <span data-ttu-id="4ecf7-344">指定載入的屬性數目。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-344">Specifies the number of properties loaded.</span></span> |
| <span data-ttu-id="4ecf7-345">userContext</span><span class="sxs-lookup"><span data-stu-id="4ecf7-345">userContext</span></span> | <span data-ttu-id="4ecf7-346">指定當提供的使用者內容資訊的載入或儲存函式呼叫。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-346">Specifies the user context information provided when the load or save function was called.</span></span> |
| <span data-ttu-id="4ecf7-347">方法名稱</span><span class="sxs-lookup"><span data-stu-id="4ecf7-347">methodName</span></span> | <span data-ttu-id="4ecf7-348">呼叫方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-348">The name of the calling method.</span></span> |

<span data-ttu-id="4ecf7-349">*（get、 set） 的路徑屬性：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-349">*path property (get, set):*</span></span>

<span data-ttu-id="4ecf7-350">此屬性以程式設計方式決定設定檔 web 服務的位置。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-350">This property programmatically determines the location of the profile web service.</span></span> <span data-ttu-id="4ecf7-351">它可以用來覆寫預設設定檔服務提供者，以及一個 ScriptManager 控制項的 ProfileService 子節點的路徑屬性中以宣告方式設定。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-351">It can be used to override the default profile service provider, as well as one set declaratively in the Path property of the ScriptManager control's ProfileService child node.</span></span>

<span data-ttu-id="4ecf7-352">請注意，預設設定檔服務的位置不會變更。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-352">Note that the location of the default profile service does not change.</span></span> <span data-ttu-id="4ecf7-353">不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面，做為 ASP.NET AJAX 驗證服務 proxy 的位置。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-353">However, ASP.NET AJAX allows you to specify the location of a web service that provides the same class interface as the ASP.NET AJAX authentication service proxy.</span></span>

<span data-ttu-id="4ecf7-354">也請注意這個屬性不應設為值，將指令碼要求導向從目前的站台。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-354">Note also that this property should not be set to a value that directs the script request off of the current site.</span></span> <span data-ttu-id="4ecf7-355">技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-355">The technology underlying AJAX should not post cross-site requests, and may generate a security exception in the client browser.</span></span>

<span data-ttu-id="4ecf7-356">這個屬性是`String`物件，表示設定檔 web 服務的路徑。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-356">This property is a `String` object representing the path to the profile web service.</span></span>

<span data-ttu-id="4ecf7-357">*（get、 set），就會逾時屬性：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-357">*timeout property (get, set):*</span></span>

<span data-ttu-id="4ecf7-358">此屬性會判斷失敗的等候設定檔服務，然後再假設載入或儲存要求的時間長度。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-358">This property determines the length of time to wait for the profile service before assuming the load or save request has failed.</span></span> <span data-ttu-id="4ecf7-359">如果在逾時到期等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-359">If the timeout expires while waiting for a call to complete, the request-failed callback will be called, and the call will not complete.</span></span>

<span data-ttu-id="4ecf7-360">這個屬性是`Number`物件，表示要等候從設定檔服務的搜尋結果的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-360">This property is a `Number` object representing the number of milliseconds to wait for results from the profile service.</span></span>

<span data-ttu-id="4ecf7-361">*程式碼範例： 在頁面載入的設定檔資料載入*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-361">*Code sample: Loading profile data at page load*</span></span>

<span data-ttu-id="4ecf7-362">下列程式碼會檢查以確認是否已驗證使用者，以及如果是的話，會當做網頁的載入使用者的慣用的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-362">The following code will check to see whether a user is authenticated, and if so, will load the user's preferred background color as the page's.</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a><span data-ttu-id="4ecf7-363">*使用自訂驗證提供者*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-363">*Using a Custom Authentication Service Provider*</span></span>

<span data-ttu-id="4ecf7-364">ASP.NET AJAX 擴充功能可讓您建立自訂指令碼驗證服務提供者公開您透過自訂的 web 服務的功能。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-364">The ASP.NET AJAX Extensions allow you to create a custom script authentication service provider by exposing your functionality through a custom web service.</span></span> <span data-ttu-id="4ecf7-365">若要使用，您的 web 服務必須公開兩種方法，`Login`和`Logout`; 這些方法必須指定為預設的 ASP.NET AJAX 驗證 web 服務相同的方法簽章。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-365">In order to be used, your web service must expose two methods, `Login` and `Logout`; and these methods must be specified with the same method signatures as the default ASP.NET AJAX Authentication web service.</span></span>

<span data-ttu-id="4ecf7-366">當您建立自訂的 web 服務之後時，您必須指定它，不論是以宣告方式在您的頁面的路徑，以程式設計方式在程式碼，或透過用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-366">Once you have created the custom web service, you will need to specify the path to it, either declaratively on your page, programmatically in code, or via the client script.</span></span>

<span data-ttu-id="4ecf7-367">*若要以宣告方式設定路徑：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-367">*To set the path declaratively:*</span></span>

<span data-ttu-id="4ecf7-368">若要以宣告方式設定路徑，包括 AuthenticationService 物件的子系 ScriptManager 在 ASP.NET 頁面上：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-368">To set the path declaratively, include the AuthenticationService child of the ScriptManager object on your ASP.NET page:</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

<span data-ttu-id="4ecf7-369">*若要在程式碼中設定的路徑：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-369">*To set the path in code:*</span></span>

<span data-ttu-id="4ecf7-370">若要以程式設計方式設定路徑，指定透過指令碼管理員的執行個體的路徑：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-370">To set the path programmatically, specify the path via the instance of your script manager:</span></span>

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

<span data-ttu-id="4ecf7-371">*若要設定指令碼中的路徑：*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-371">*To set the path in script:*</span></span>

<span data-ttu-id="4ecf7-372">若要以程式設計方式設定路徑的指令碼中，利用`path`AuthenticationService 類別的屬性：</span><span class="sxs-lookup"><span data-stu-id="4ecf7-372">To set the path programmatically in script, utilize the `path` property of the AuthenticationService class:</span></span>

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

<span data-ttu-id="4ecf7-373">*自訂驗證的範例 Web 服務*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-373">*Sample Web Service for Custom Authentication*</span></span>

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a><span data-ttu-id="4ecf7-374">總結</span><span class="sxs-lookup"><span data-stu-id="4ecf7-374">Summary</span></span>

<span data-ttu-id="4ecf7-375">ASP.NET 服務-特別是程式碼剖析、 成員資格，以及驗證服務-輕鬆地在用戶端瀏覽器上公開 JavaScript 中。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-375">ASP.NET services - specifically the profiling, membership, and authentication services - are easily exposed to JavaScript on the client browser.</span></span> <span data-ttu-id="4ecf7-376">這可讓開發人員不必依靠等執行困難的 Updatepanel 控制項順暢地整合其用戶端程式碼使用的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-376">This allows developers to integrate their client-side code with the authentication mechanism seamlessly, without depending on controls such as UpdatePanels to do the heavy lifting.</span></span> <span data-ttu-id="4ecf7-377">設定檔資料可以保護用戶端，以防止藉由使用 web 組態設定;沒有資料可供使用，根據預設，和開發人員必須 opt-in 以設定檔屬性。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-377">Profile data can be protected from the client as well, by utilizing web configuration settings; no data is available by default, and developers must opt-in to profile properties.</span></span>

<span data-ttu-id="4ecf7-378">此外，藉由使用對等的方法簽章建立簡化的 web 服務實作，開發人員可以建立這些內建的 ASP.NET 服務的自訂指令碼提供者。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-378">Furthermore, by creating simplified web service implementations with equivalent method signatures, developers can create custom script providers for these intrinsic ASP.NET services.</span></span> <span data-ttu-id="4ecf7-379">如需這些技術的支援可簡化開發豐富型用戶端應用程式，同時開發人員提供各種不同的彈性，來滿足特定需求。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-379">Support for these techniques simplifies the development of rich client applications, while providing developers with a wide range of flexibility to meet specific needs.</span></span>

## <a name="bio"></a><span data-ttu-id="4ecf7-380">*簡歷*</span><span class="sxs-lookup"><span data-stu-id="4ecf7-380">*Bio*</span></span>

<span data-ttu-id="4ecf7-381">Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。</span><span class="sxs-lookup"><span data-stu-id="4ecf7-381">Scott Cate has been working with Microsoft Web technologies since 1997 and is the President of myKB.com ([www.myKB.com](http://www.myKB.com)) where he specializes in writing ASP.NET based applications focused on Knowledge Base Software solutions.</span></span> <span data-ttu-id="4ecf7-382">可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)</span><span class="sxs-lookup"><span data-stu-id="4ecf7-382">Scott can be contacted via email at [scott.cate@myKB.com](mailto:scott.cate@myKB.com) or his blog at [ScottCate.com](http://ScottCate.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ecf7-383">[上一頁](understanding-asp-net-ajax-updatepanel-triggers.md)
> [下一頁](understanding-asp-net-ajax-localization.md)</span><span class="sxs-lookup"><span data-stu-id="4ecf7-383">[Previous](understanding-asp-net-ajax-updatepanel-triggers.md)
[Next](understanding-asp-net-ajax-localization.md)</span></span>
