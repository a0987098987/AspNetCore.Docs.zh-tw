---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) 疑難排解指南 |Microsoft 文件
author: tfitzmac
description: 本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時可能發生的問題。 軟體版本的 ASP.NET Web Pag...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="24719-104">ASP.NET Web Pages (Razor) 疑難排解指南</span><span class="sxs-lookup"><span data-stu-id="24719-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="24719-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="24719-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="24719-106">本文說明使用 ASP.NET Web Pages (Razor) 和一些建議的解決方案時可能發生的問題。</span><span class="sxs-lookup"><span data-stu-id="24719-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="24719-107">軟體版本</span><span class="sxs-lookup"><span data-stu-id="24719-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="24719-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="24719-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="24719-109">本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。</span><span class="sxs-lookup"><span data-stu-id="24719-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="24719-110">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="24719-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="24719-111">執行網頁的問題</span><span class="sxs-lookup"><span data-stu-id="24719-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="24719-112">Razor 程式碼問題</span><span class="sxs-lookup"><span data-stu-id="24719-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="24719-113">安全性與成員資格的問題</span><span class="sxs-lookup"><span data-stu-id="24719-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="24719-114">傳送電子郵件問題</span><span class="sxs-lookup"><span data-stu-id="24719-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="24719-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="24719-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="24719-116">一般問題，請參閱[ASP.NET Web Pages (Razor) 常見問題集](https://go.microsoft.com/fwlink/?LinkId=253000)。</span><span class="sxs-lookup"><span data-stu-id="24719-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="24719-117">執行網頁的問題</span><span class="sxs-lookup"><span data-stu-id="24719-117">Issues with Running Pages</span></span>

<span data-ttu-id="24719-118">各種問題可以防止*.cshtml*和*.vbhtml*頁面無法正常執行。</span><span class="sxs-lookup"><span data-stu-id="24719-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="24719-119">本節列出常見的錯誤訊息，並有可能的原因。</span><span class="sxs-lookup"><span data-stu-id="24719-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="24719-120">HTTP 錯誤 403-禁止： 拒絕存取</span><span class="sxs-lookup"><span data-stu-id="24719-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="24719-121">*您沒有檢視此目錄或使用您提供的認證，網頁的權限。*</span><span class="sxs-lookup"><span data-stu-id="24719-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="24719-122">如果伺服器未執行的.NET framework 正確版本，就會發生此錯誤。</span><span class="sxs-lookup"><span data-stu-id="24719-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="24719-123">請確定正在執行伺服器 （本機或遠端） 的電腦至少已安裝.NET Framework 4。</span><span class="sxs-lookup"><span data-stu-id="24719-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="24719-124">也請確定應用程式本身設定為執行正確的版本。</span><span class="sxs-lookup"><span data-stu-id="24719-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="24719-125">如果您在本機 WebMatrix 中工作時看到此問題，請按一下**網站**工作區中，然後按一下樹狀檢視中的，按一下**設定**。</span><span class="sxs-lookup"><span data-stu-id="24719-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="24719-126">在**選取.NET Framework 版本**清單中，選取**.NET 4 （整合式）**。</span><span class="sxs-lookup"><span data-stu-id="24719-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="24719-127">如果此版本已設定，請以系統管理員身分執行 WebMatrix。</span><span class="sxs-lookup"><span data-stu-id="24719-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="24719-128">請確定您網站的根目錄具有至少一個*.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="24719-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="24719-129">如果遠端伺服器上的 web 伺服器時，您會看到此錯誤，請連絡伺服器系統管理員。</span><span class="sxs-lookup"><span data-stu-id="24719-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="24719-130">請確定伺服器具有.NET Framework 4 或更新的版本。</span><span class="sxs-lookup"><span data-stu-id="24719-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="24719-131">也請確定應用程式正在執行中應用程式集區設定為使用該版本的.net Framework。</span><span class="sxs-lookup"><span data-stu-id="24719-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="24719-132">如果您有伺服器的控制權，請確定它正在執行的.NET framework 正確版本。</span><span class="sxs-lookup"><span data-stu-id="24719-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="24719-133">您也可以嘗試執行修復安裝`aspnet_regiis -iru`命令。</span><span class="sxs-lookup"><span data-stu-id="24719-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="24719-134">（例如，如果您安裝.NET Framework 之後，您會安裝 IIS，IIS 會不正確設定來執行 ASP.NET 網頁。）如需詳細資訊，請參閱[ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="24719-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="24719-135">HTTP 錯誤 403.14-禁止</span><span class="sxs-lookup"><span data-stu-id="24719-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="24719-136">*網頁伺服器設定為不列出此目錄的內容。*</span><span class="sxs-lookup"><span data-stu-id="24719-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="24719-137">如果要求保護的資源，可能會發生這個錯誤 (像是*Web.config*檔案) 或受保護的資料夾中 (類似*應用程式\_資料*或*應用程式\_程式碼*)。</span><span class="sxs-lookup"><span data-stu-id="24719-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="24719-138">HTTP 錯誤 404.17-找不到</span><span class="sxs-lookup"><span data-stu-id="24719-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="24719-139">*要求的內容似乎是指令碼，並不會處理由靜態檔案處理常式。*</span><span class="sxs-lookup"><span data-stu-id="24719-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="24719-140">如果伺服器未正確使用.NET Framework 4 或更新版本中，因此無法辨識中的程式碼，可能會發生這個錯誤`@{ }`區塊。</span><span class="sxs-lookup"><span data-stu-id="24719-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="24719-141">請參閱稍早的描述*HTTP 錯誤 403-禁止： 拒絕存取*。</span><span class="sxs-lookup"><span data-stu-id="24719-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="24719-142">HTTP 錯誤 404.7-找不到</span><span class="sxs-lookup"><span data-stu-id="24719-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="24719-143">*要求篩選模組設定為拒絕副檔名*</span><span class="sxs-lookup"><span data-stu-id="24719-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="24719-144">如果，可能會發生這個錯誤*.cshtml*或*.vbhtml*擴充功能已被明確封鎖在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="24719-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="24719-145">此問題的徵兆時，該 Url 的工作並不包含延伸模組，但包含的 Url *.cshtml*或*.vbhtml*無法運作。</span><span class="sxs-lookup"><span data-stu-id="24719-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="24719-146">可能的解決方案是要重新啟用的擴充功能在網站的*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="24719-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="24719-147">下列範例示範如何啟用*.cshtml*延伸模組。</span><span class="sxs-lookup"><span data-stu-id="24719-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="24719-148">HTTP 錯誤 404.8-找不到</span><span class="sxs-lookup"><span data-stu-id="24719-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="24719-149">*要求篩選模組設定為拒絕包含 hiddenSegment 區段之 URL 中的路徑。*</span><span class="sxs-lookup"><span data-stu-id="24719-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="24719-150">如果要求保護的資源，可能會發生這個錯誤 (像是*Web.config*檔案) 或受保護的資料夾中 (類似*應用程式\_資料*或*應用程式\_程式碼*)。</span><span class="sxs-lookup"><span data-stu-id="24719-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="24719-151">這種類型的頁面不被服務 （伺服器錯誤 '/' 應用程式中）</span><span class="sxs-lookup"><span data-stu-id="24719-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="24719-152">請參閱 HTTP 錯誤 404.17 稍早的描述。</span><span class="sxs-lookup"><span data-stu-id="24719-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="24719-153">Razor 程式碼問題</span><span class="sxs-lookup"><span data-stu-id="24719-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="24719-154">名稱 '*類別*' 不存在於目前的內容</span><span class="sxs-lookup"><span data-stu-id="24719-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="24719-155">通常，您會看到此錯誤的原因在於`class`參考協助程式，但協助專家未安裝。</span><span class="sxs-lookup"><span data-stu-id="24719-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="24719-156">例如，如果您嘗試使用協助程式，但如果您尚未安裝的 NuGet 封裝，您會看到這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="24719-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="24719-157">使用在 WebMatrix 圖庫來尋找和安裝的協助程式。</span><span class="sxs-lookup"><span data-stu-id="24719-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="24719-158">如果協助專家已安裝，但頁面仍然無法辨識它，再試一次新增新增`using`陳述式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="24719-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="24719-159">在`using`陳述式、 參考命名空間包含 helper。</span><span class="sxs-lookup"><span data-stu-id="24719-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="24719-160">例如，ASP.NET Web Helpers 封裝中的基本 helper 位於`System.Web.Helpers`命名空間。</span><span class="sxs-lookup"><span data-stu-id="24719-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="24719-161">在您要使用此 helper 頁面的頂端，加入這一行：</span><span class="sxs-lookup"><span data-stu-id="24719-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="24719-162">安全性與成員資格的問題</span><span class="sxs-lookup"><span data-stu-id="24719-162">Issues with Security and Membership</span></span>

<span data-ttu-id="24719-163">如果您使用內建的安全性 （成員資格） 系統中的 ASP.NET Web Pages (Razor)，您可能會遇到下列問題。</span><span class="sxs-lookup"><span data-stu-id="24719-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="24719-164">若要呼叫此方法時，"Membership.Provider"屬性必須是"ExtendedMembershipProvider"的執行個體</span><span class="sxs-lookup"><span data-stu-id="24719-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="24719-165">這個錯誤可能表示沒有`AspNetSqlMembershipProvider`類別設定。</span><span class="sxs-lookup"><span data-stu-id="24719-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="24719-166">（徵兆是站台在本機上可以正常運作，但當您發行到裝載提供者的伺服器時，會擲回這個錯誤）。一個修正這個問題是，明確地啟用簡單成員資格新增至站台的以下*Web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="24719-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="24719-167">傳送電子郵件問題</span><span class="sxs-lookup"><span data-stu-id="24719-167">Issues with Sending Email</span></span>

<span data-ttu-id="24719-168">傳送電子郵件問題可能項挑戰，若要偵錯。</span><span class="sxs-lookup"><span data-stu-id="24719-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="24719-169">初始的問題可能是您無法連接到 SMTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="24719-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="24719-170">如果連接成功時，ASP.NET 將訊息交給關閉 SMTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="24719-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="24719-171">不過，可以是訊息本身，避免 SMTP 伺服器傳送的問題。</span><span class="sxs-lookup"><span data-stu-id="24719-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="24719-172">如果您的應用程式並不會成功地傳送電子郵件，請嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="24719-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="24719-173">SMTP 伺服器名稱通常會像下面`smtp.provider.com`或`smtp.provider.net`。</span><span class="sxs-lookup"><span data-stu-id="24719-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="24719-174">不過，如果您是主機提供者發行您的網站，SMTP 伺服器名稱此時可能`localhost`。</span><span class="sxs-lookup"><span data-stu-id="24719-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="24719-175">因為已發佈和提供者的伺服器上執行您的站台之後，可能從您的應用程式的觀點來看本機 SMTP 伺服器，就會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="24719-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="24719-176">伺服器名稱中的這項變更可能表示您已變更 SMTP 伺服器名稱做為您發佈程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="24719-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="24719-177">連接埠號碼通常是 25。</span><span class="sxs-lookup"><span data-stu-id="24719-177">The port number is usually 25.</span></span> <span data-ttu-id="24719-178">不過，某些提供者需要您使用的連接埠 587 或某些其他連接埠。</span><span class="sxs-lookup"><span data-stu-id="24719-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="24719-179">檢查 SMTP 伺服器的擁有者哪個連接埠號碼他們期望應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="24719-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="24719-180">請確定您使用正確的認證。</span><span class="sxs-lookup"><span data-stu-id="24719-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="24719-181">如果您已發行您的站台至主控提供者，使用提供者特別指出對於電子郵件的認證。</span><span class="sxs-lookup"><span data-stu-id="24719-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="24719-182">這些認證可能與您用來發行的認證不同。</span><span class="sxs-lookup"><span data-stu-id="24719-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="24719-183">有時候您完全不需要認證。</span><span class="sxs-lookup"><span data-stu-id="24719-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="24719-184">如果您要使用您個人的 ISP 傳送電子郵件，您的電子郵件提供者可能已經知道您的認證。</span><span class="sxs-lookup"><span data-stu-id="24719-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="24719-185">發行之後，您可能需要使用比您本機電腦上進行測試時有不同的認證。</span><span class="sxs-lookup"><span data-stu-id="24719-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="24719-186">如果您的電子郵件提供者會使用加密，設定`WebMail.EnableSsl`至`true`。</span><span class="sxs-lookup"><span data-stu-id="24719-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="24719-187">如果沒有傳送電子郵件時發生錯誤，您可能會看到標準 ASP.NET 錯誤訊息，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="24719-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![電子郵件問題時，ASP.NET 錯誤訊息](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="24719-189">您也可以偵錯使用傳送電子郵件問題`try-catch`區塊，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="24719-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="24719-190">當您使用`try-catch`區塊中，ASP.NET 不會顯示其標準錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24719-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="24719-191">相反地，您可以在此擷取中的錯誤`catch`區塊的部份。</span><span class="sxs-lookup"><span data-stu-id="24719-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="24719-192">取代為適當值`your-SMTP-server-name`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="24719-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="24719-193">您可能會看到這種方式的錯誤訊息如下：</span><span class="sxs-lookup"><span data-stu-id="24719-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="24719-194">*傳送郵件失敗。*</span><span class="sxs-lookup"><span data-stu-id="24719-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="24719-195">-或-</span><span class="sxs-lookup"><span data-stu-id="24719-195">-or-</span></span>

    <span data-ttu-id="24719-196">*連接嘗試失敗，因為連線對象並未正確回應之後一段時間，或已建立的連線失敗，因為已連線的主機無法回應*</span><span class="sxs-lookup"><span data-stu-id="24719-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="24719-197">此錯誤通常表示應用程式無法連線到 SMTP 伺服器。</span><span class="sxs-lookup"><span data-stu-id="24719-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="24719-198">請檢查伺服器名稱和連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="24719-198">Check the server name and port number.</span></span>
- <span data-ttu-id="24719-199"><em>信箱無法使用。伺服器回應為： 5.1.0 &lt; someuser@invaliddomain &gt;拒絕寄件者： 無效的寄件者網域</em></span><span class="sxs-lookup"><span data-stu-id="24719-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="24719-200">此訊息可能表示`From`位址不正確或遺失。</span><span class="sxs-lookup"><span data-stu-id="24719-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="24719-201">*指定的字串不是所需的電子郵件地址的格式。*</span><span class="sxs-lookup"><span data-stu-id="24719-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="24719-202">這個錯誤可能表示的值`To`或`From`屬性不會被視為電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="24719-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="24719-203">(ASP.NET 無法檢查電子郵件地址是否有效，只有它的正確格式，例如*name@domain.com*。)</span><span class="sxs-lookup"><span data-stu-id="24719-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="24719-204">移除的標記，就會顯示錯誤 (`@errorMessage`) 頁面發佈到即時網站之前。</span><span class="sxs-lookup"><span data-stu-id="24719-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="24719-205">它不是個不錯的主意，讓使用者看到您從伺服器取得的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24719-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="24719-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="24719-206">Additional Resources</span></span>

[<span data-ttu-id="24719-207">ASP.NET Web Pages (Razor) 常見問題集</span><span class="sxs-lookup"><span data-stu-id="24719-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="24719-208">[WebMatrix 和 ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET 網站上的論壇</span><span class="sxs-lookup"><span data-stu-id="24719-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
