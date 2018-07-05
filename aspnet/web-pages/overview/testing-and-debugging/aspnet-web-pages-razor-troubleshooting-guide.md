---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) 疑難排解指南 |Microsoft Docs
author: tfitzmac
description: 本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時，您可能遇到的問題。 ASP.NET Web 頁面的軟體版本...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: 48c0380af32038a1d916d1b99f7de0f918d1a74e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373535"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="dc342-104">ASP.NET Web Pages (Razor) 疑難排解指南</span><span class="sxs-lookup"><span data-stu-id="dc342-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="dc342-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dc342-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dc342-106">本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時，您可能遇到的問題。</span><span class="sxs-lookup"><span data-stu-id="dc342-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="dc342-107">軟體版本</span><span class="sxs-lookup"><span data-stu-id="dc342-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="dc342-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dc342-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="dc342-109">本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。</span><span class="sxs-lookup"><span data-stu-id="dc342-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="dc342-110">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="dc342-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="dc342-111">執行網頁的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="dc342-112">Razor 程式碼的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="dc342-113">安全性與成員資格的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="dc342-114">傳送電子郵件的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="dc342-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="dc342-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="dc342-116">一般問題，請參閱[ASP.NET Web Pages (Razor) 常見問題集](https://go.microsoft.com/fwlink/?LinkId=253000)。</span><span class="sxs-lookup"><span data-stu-id="dc342-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="dc342-117">執行網頁的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-117">Issues with Running Pages</span></span>

<span data-ttu-id="dc342-118">各種問題妨礙 *.cshtml*並 *.vbhtml*頁面無法正常執行。</span><span class="sxs-lookup"><span data-stu-id="dc342-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="dc342-119">本節列出常見的錯誤訊息，並有可能的原因。</span><span class="sxs-lookup"><span data-stu-id="dc342-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="dc342-120">HTTP 錯誤 403-禁止： 拒絕存取</span><span class="sxs-lookup"><span data-stu-id="dc342-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="dc342-121">*您無權檢視此目錄或使用您提供的認證 頁面。*</span><span class="sxs-lookup"><span data-stu-id="dc342-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="dc342-122">如果伺服器未執行正確的.NET framework 版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="dc342-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="dc342-123">請確定正在執行伺服器 （本機或遠端） 電腦至少已安裝.NET Framework 4。</span><span class="sxs-lookup"><span data-stu-id="dc342-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="dc342-124">也請確定應用程式本身已設定為執行正確的版本。</span><span class="sxs-lookup"><span data-stu-id="dc342-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="dc342-125">如果您在本機在 WebMatrix 中工作時看到此問題，請按一下**站台**工作區中，然後在樹狀檢視中按一下 **設定**。</span><span class="sxs-lookup"><span data-stu-id="dc342-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="dc342-126">在 **選取 .NET Framework 版本**清單中，選取 **.NET 4 （整合模式）**。</span><span class="sxs-lookup"><span data-stu-id="dc342-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="dc342-127">如果此版本已設定，請以系統管理員身分執行 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="dc342-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="dc342-128">請確定您網站的根目錄有至少一個 *.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="dc342-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="dc342-129">如果遠端伺服器上的 web 伺服器時，您會看到此錯誤，請連絡伺服器系統管理員。</span><span class="sxs-lookup"><span data-stu-id="dc342-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="dc342-130">請確定伺服器具有.NET Framework 4 或更新的版本。</span><span class="sxs-lookup"><span data-stu-id="dc342-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="dc342-131">也請確定已設定為使用該版本的.net Framework 應用程式集區中正在執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="dc342-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="dc342-132">如果您有伺服器的控制權，請確定它正在執行正確的.NET framework 版本。</span><span class="sxs-lookup"><span data-stu-id="dc342-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="dc342-133">您也可以嘗試執行修復安裝`aspnet_regiis -iru`命令。</span><span class="sxs-lookup"><span data-stu-id="dc342-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="dc342-134">（例如，如果您安裝.NET Framework 之後，您就會安裝 IIS，IIS 會不正確設定來執行 ASP.NET 網頁。）如需詳細資訊，請參閱 < [ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="dc342-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="dc342-135">HTTP 錯誤 403.14-禁止</span><span class="sxs-lookup"><span data-stu-id="dc342-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="dc342-136">*Web 伺服器被設定為不列出此目錄的內容。*</span><span class="sxs-lookup"><span data-stu-id="dc342-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="dc342-137">如果您要求的受保護的資源，會發生此錯誤 (像是*Web.config*檔案) 或位於受保護的資料夾 (例如*應用程式\_資料*或*應用程式\_程式碼*)。</span><span class="sxs-lookup"><span data-stu-id="dc342-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="dc342-138">找不到的 HTTP 錯誤 404.17-</span><span class="sxs-lookup"><span data-stu-id="dc342-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="dc342-139">*要求的內容似乎是指令碼，並不會處理靜態檔案處理常式。*</span><span class="sxs-lookup"><span data-stu-id="dc342-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="dc342-140">如果伺服器已不正確設定成使用.NET Framework 4 或更新版本中，因此無法辨識中的程式碼，可能會發生此錯誤`@{ }`區塊。</span><span class="sxs-lookup"><span data-stu-id="dc342-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="dc342-141">請參閱稍早的說明*HTTP 錯誤 403-禁止： 拒絕存取*。</span><span class="sxs-lookup"><span data-stu-id="dc342-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="dc342-142">找不到的 HTTP 錯誤 404.7-</span><span class="sxs-lookup"><span data-stu-id="dc342-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="dc342-143">*要求篩選模組已拒絕的副檔名*</span><span class="sxs-lookup"><span data-stu-id="dc342-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="dc342-144">如果此錯誤可能會發生 *.cshtml*或是 *.vbhtml*擴充功能已被明確封鎖在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="dc342-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="dc342-145">此問題的徵兆時，該 Url 工作並不包含延伸模組，但包含的 Url *.cshtml*或是 *.vbhtml*無法運作。</span><span class="sxs-lookup"><span data-stu-id="dc342-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="dc342-146">可能的解決方案是要重新啟用在站台的延伸模組*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="dc342-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="dc342-147">下列範例示範如何啟用 *.cshtml*延伸模組。</span><span class="sxs-lookup"><span data-stu-id="dc342-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="dc342-148">找不到的 HTTP 錯誤 404.8-</span><span class="sxs-lookup"><span data-stu-id="dc342-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="dc342-149">*要求篩選模組會設定為拒絕的 URL，其中包含 hiddenSegment 區段中的路徑。*</span><span class="sxs-lookup"><span data-stu-id="dc342-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="dc342-150">如果您要求的受保護的資源，會發生此錯誤 (像是*Web.config*檔案) 或位於受保護的資料夾 (例如*應用程式\_資料*或*應用程式\_程式碼*)。</span><span class="sxs-lookup"><span data-stu-id="dc342-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="dc342-151">這種類型的頁面不會提供服務 （伺服器錯誤 '/' 應用程式中）</span><span class="sxs-lookup"><span data-stu-id="dc342-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="dc342-152">請參閱 HTTP 錯誤 404.17 稍早的描述。</span><span class="sxs-lookup"><span data-stu-id="dc342-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="dc342-153">Razor 程式碼的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="dc342-154">名稱 '*類別*' 不存在於目前的內容</span><span class="sxs-lookup"><span data-stu-id="dc342-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="dc342-155">通常，您會看到此錯誤的原因在於`class`協助程式，但協助程式未安裝的參考。</span><span class="sxs-lookup"><span data-stu-id="dc342-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="dc342-156">例如，如果您嘗試使用協助程式，但您尚未從 NuGet 安裝套件，您會看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="dc342-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="dc342-157">使用 WebMatrix 中的資源庫尋找並安裝的協助程式。</span><span class="sxs-lookup"><span data-stu-id="dc342-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="dc342-158">如果已安裝的協助程式，但頁面仍無法辨識它，再嘗試新增新增`using`陳述式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="dc342-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="dc342-159">在 `using`陳述式，參考包含協助程式的命名空間。</span><span class="sxs-lookup"><span data-stu-id="dc342-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="dc342-160">例如，ASP.NET Web Helpers 封裝中的基本 helper 會位於`System.Web.Helpers`命名空間。</span><span class="sxs-lookup"><span data-stu-id="dc342-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="dc342-161">在 [您要使用協助程式] 頁面頂端，加入這一行：</span><span class="sxs-lookup"><span data-stu-id="dc342-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="dc342-162">安全性與成員資格的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-162">Issues with Security and Membership</span></span>

<span data-ttu-id="dc342-163">如果您使用內建的安全性 （成員資格） 系統中 ASP.NET Web Pages (Razor)，您可能會遇到下列問題。</span><span class="sxs-lookup"><span data-stu-id="dc342-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="dc342-164">若要呼叫這個方法，"Membership.Provider"屬性必須是"ExtendedMembershipProvider"的執行個體</span><span class="sxs-lookup"><span data-stu-id="dc342-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="dc342-165">此錯誤可能表示沒有`AspNetSqlMembershipProvider`類別設定。</span><span class="sxs-lookup"><span data-stu-id="dc342-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="dc342-166">（徵兆是站台在本機正常運作，但當您發行至裝載提供者的伺服器時，會擲回這個錯誤）。一個解決此問題是要明確地將下列內容新增至站台的啟用簡單成員資格*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="dc342-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="dc342-167">傳送電子郵件的問題</span><span class="sxs-lookup"><span data-stu-id="dc342-167">Issues with Sending Email</span></span>

<span data-ttu-id="dc342-168">傳送電子郵件的問題可能較難偵錯。</span><span class="sxs-lookup"><span data-stu-id="dc342-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="dc342-169">可以，您無法連接到 SMTP 伺服器的初始的問題。</span><span class="sxs-lookup"><span data-stu-id="dc342-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="dc342-170">如果連接成功時，ASP.NET 遞交訊息到 SMTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="dc342-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="dc342-171">不過，可能會造成問題的訊息本身，可防止 SMTP 伺服器傳送。</span><span class="sxs-lookup"><span data-stu-id="dc342-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="dc342-172">如果您的應用程式並不會成功傳送電子郵件，請嘗試下列方法：</span><span class="sxs-lookup"><span data-stu-id="dc342-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="dc342-173">SMTP 伺服器名稱通常會像下面`smtp.provider.com`或`smtp.provider.net`。</span><span class="sxs-lookup"><span data-stu-id="dc342-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="dc342-174">不過，如果您將您的站台發佈到主機服務提供者時，SMTP 伺服器名稱此時可能`localhost`。</span><span class="sxs-lookup"><span data-stu-id="dc342-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="dc342-175">因為您已發行，並提供者的伺服器上執行您的網站之後，SMTP 伺服器可能已本機從您的應用程式的觀點來看，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="dc342-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="dc342-176">伺服器名稱中的這項變更可能表示您必須變更 SMTP 伺服器名稱做為您的發行程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="dc342-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="dc342-177">連接埠號碼通常為 25。</span><span class="sxs-lookup"><span data-stu-id="dc342-177">The port number is usually 25.</span></span> <span data-ttu-id="dc342-178">不過，某些提供者需要您使用的連接埠 587 或某些其他連接埠。</span><span class="sxs-lookup"><span data-stu-id="dc342-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="dc342-179">檢查 SMTP 伺服器的擁有者在您使用預期哪個連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="dc342-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="dc342-180">請確定您使用正確的認證。</span><span class="sxs-lookup"><span data-stu-id="dc342-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="dc342-181">如果您已發行您的站台至主控提供者，使用提供者特別指出適用於電子郵件的認證。</span><span class="sxs-lookup"><span data-stu-id="dc342-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="dc342-182">這些認證可能與您用來發行的認證不同。</span><span class="sxs-lookup"><span data-stu-id="dc342-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="dc342-183">有時候您不需要認證。</span><span class="sxs-lookup"><span data-stu-id="dc342-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="dc342-184">如果您要使用您個人的 ISP，傳送電子郵件，您的電子郵件提供者可能已經知道您的認證。</span><span class="sxs-lookup"><span data-stu-id="dc342-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="dc342-185">發行之後，您可能需要使用不同的認證比當您測試本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="dc342-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="dc342-186">如果您的電子郵件提供者會使用加密，請設定`WebMail.EnableSsl`至`true`。</span><span class="sxs-lookup"><span data-stu-id="dc342-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="dc342-187">如果沒有傳送電子郵件時發生錯誤，您可能會看到標準 ASP.NET 錯誤訊息，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="dc342-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![使用電子郵件問題時，ASP.NET 錯誤訊息](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="dc342-189">您也可以偵錯問題的電子郵件傳送使用`try-catch`區塊，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="dc342-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="dc342-190">當您使用`try-catch`區塊中，ASP.NET 不會顯示其標準錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="dc342-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="dc342-191">相反地，您可以在此擷取中的錯誤`catch`區塊部分。</span><span class="sxs-lookup"><span data-stu-id="dc342-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="dc342-192">將適當的值取代`your-SMTP-server-name`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="dc342-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="dc342-193">某些錯誤訊息，您可能會看到這種方式包括：</span><span class="sxs-lookup"><span data-stu-id="dc342-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="dc342-194">*傳送郵件失敗。*</span><span class="sxs-lookup"><span data-stu-id="dc342-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="dc342-195">-或-</span><span class="sxs-lookup"><span data-stu-id="dc342-195">-or-</span></span>

    <span data-ttu-id="dc342-196">*連線嘗試失敗，因為連線對象並未正確回應之後一段時間，或是連線建立失敗，因為連線的主機無法回應*</span><span class="sxs-lookup"><span data-stu-id="dc342-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="dc342-197">此錯誤通常表示應用程式無法連接到 SMTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="dc342-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="dc342-198">請檢查伺服器名稱和連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="dc342-198">Check the server name and port number.</span></span>
- <span data-ttu-id="dc342-199"><em>信箱無法使用。伺服器回應為： 5.1.0 &lt; someuser@invaliddomain &gt;寄件者已拒絕： 無效的寄件者網域</em></span><span class="sxs-lookup"><span data-stu-id="dc342-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="dc342-200">此訊息可能表示`From`位址不正確或遺失。</span><span class="sxs-lookup"><span data-stu-id="dc342-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="dc342-201">*指定的字串不是所需的電子郵件地址的格式。*</span><span class="sxs-lookup"><span data-stu-id="dc342-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="dc342-202">此錯誤可能表示的值`To`或`From`屬性不會視為電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="dc342-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="dc342-203">(ASP.NET 無法檢查電子郵件地址是否有效，只有它的正確格式，例如*name@domain.com*。)</span><span class="sxs-lookup"><span data-stu-id="dc342-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="dc342-204">移除會顯示錯誤的標記 (`@errorMessage`) 頁面發佈給即時網站之前。</span><span class="sxs-lookup"><span data-stu-id="dc342-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="dc342-205">它不是個不錯的主意，可讓使用者看到您從伺服器取得的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="dc342-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="dc342-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="dc342-206">Additional Resources</span></span>

[<span data-ttu-id="dc342-207">ASP.NET Web Pages (Razor) 常見問題集</span><span class="sxs-lookup"><span data-stu-id="dc342-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="dc342-208">[WebMatrix 及 ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET 網站上的論壇</span><span class="sxs-lookup"><span data-stu-id="dc342-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
