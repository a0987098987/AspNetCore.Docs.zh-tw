---
uid: ajax/cdn/overview
title: "Microsoft Ajax 內容傳遞網路 |Microsoft 文件"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="4bbed-102">Microsoft Ajax 內容傳遞網路</span><span class="sxs-lookup"><span data-stu-id="4bbed-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="4bbed-103">注意： Microsoft Ajax CDN 有沒有超過使用 Azure CDN 的 SLA。</span><span class="sxs-lookup"><span data-stu-id="4bbed-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="4bbed-104">目錄</span><span class="sxs-lookup"><span data-stu-id="4bbed-104">Table of Contents</span></span>

<span data-ttu-id="4bbed-105">**[重新命名為 ajax.aspnetcdn.com ajax.microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="4bbed-106">**[Visual Studio.vsdoc 支援](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="4bbed-107">**[使用 ASP.NET Ajax cdn](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="4bbed-108">**[使用 jQuery cdn](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="4bbed-109">**[使用 jQuery UI CDN。](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="4bbed-110">**[CDN 的協力廠商檔案](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="4bbed-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="4bbed-111">jQuery 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="4bbed-112">在 CDN 上 jQuery 移轉版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="4bbed-113">jQuery UI 版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="4bbed-114">jQuery 驗證版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="4bbed-115">jQuery Mobile 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="4bbed-116">jQuery 範本版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="4bbed-117">jQuery 在 CDN 上循環版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="4bbed-118">jQuery Datatable 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="4bbed-119">在 CDN 上 Modernizr 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="4bbed-120">在 CDN 上 JSHint 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="4bbed-121">在 CDN 上 knockout 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="4bbed-122">全球化在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="4bbed-123">回應在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="4bbed-124">啟動程序在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="4bbed-125">在 CDN 上的啟動程序 TouchCarousel 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="4bbed-126">在 CDN 上 Hammer.js 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="4bbed-127">ASP.NET Web Form 和 Ajax CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="4bbed-128">ASP.NET MVC 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="4bbed-129">ASP.NET SignalR 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="4bbed-130">Microsoft Ajax 內容傳遞網路 (CDN) 可裝載受歡迎的協力廠商的 JavaScript 程式庫，例如 jQuery 並讓您輕鬆地將它們新增到您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4bbed-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="4bbed-131">比方說，您可以啟動這個 CDN 上使用 jQuery 裝載，只要加入&lt;指令碼&gt;標記加入至您 ajax.aspnetcdn.com 所指向的頁面。</span><span class="sxs-lookup"><span data-stu-id="4bbed-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="4bbed-132">利用 CDN，您可以大幅改善 Ajax 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="4bbed-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="4bbed-133">CDN 的內容會在位於世界各地的伺服器上快取。</span><span class="sxs-lookup"><span data-stu-id="4bbed-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="4bbed-134">此外，CDN 可讓瀏覽器與重複使用快取的第三方 JavaScript 檔案位於不同網域中的網站。</span><span class="sxs-lookup"><span data-stu-id="4bbed-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="4bbed-135">如果您需要提供使用 Secure Sockets Layer 的網頁，CDN 會支援 SSL (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="4bbed-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="4bbed-136">CDN 裝載下列第三方指令碼程式庫已上傳，且這些程式庫的擁有者滹砅凎您：</span><span class="sxs-lookup"><span data-stu-id="4bbed-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="4bbed-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4bbed-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="4bbed-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="4bbed-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="4bbed-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="4bbed-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="4bbed-140">jQuery 驗證 (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="4bbed-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="4bbed-141">jQuery 循環 (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="4bbed-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="4bbed-142">jQuery Datatable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="4bbed-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="4bbed-143">Microsoft Ajax CDN 也包含由 Microsoft 已上傳的下列程式庫：</span><span class="sxs-lookup"><span data-stu-id="4bbed-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="4bbed-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="4bbed-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="4bbed-145">ASP.NET MVC JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="4bbed-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="4bbed-146">ASP.NET SignalR JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="4bbed-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="4bbed-147">Microsoft 不會聲稱此 CDN 上裝載任何協力廠商程式庫的擁有權。</span><span class="sxs-lookup"><span data-stu-id="4bbed-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4bbed-148">版權擁有者的程式庫會授權給您這些程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4bbed-149">您可能要下載並使用這類程式庫的任何權限會授與完全各著作權擁有人。</span><span class="sxs-lookup"><span data-stu-id="4bbed-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4bbed-150">因為這些不是 Microsoft 文件庫，Microsoft 提供任何瑕疵責任擔保或財產權限授權 （包括任何隱含的專利權限） 這個 CDN 上裝載的第三方廠商程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="4bbed-151">如果您想要提交您的 JavaScript 程式庫，而且您的程式庫是其中一個最上層的 JavaScript 程式庫 (上所列http://trends.builtwith.com)或延伸模組/外掛程式，以這些程式庫 （a） 常用; 或 （b） 很有幫助在 ASP.NET 上使用，然後請連絡AjaxCDNSubmission@Microsoft.com。</span><span class="sxs-lookup"><span data-stu-id="4bbed-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="4bbed-152">重新命名為 ajax.aspnetcdn.com ajax.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4bbed-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="4bbed-153">CDN 用於使用 microsoft.com 網域名稱，而且已變更為使用 aspnetcdn.com 網域名稱。</span><span class="sxs-lookup"><span data-stu-id="4bbed-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="4bbed-154">這項變更已對提升效能，因為瀏覽器參考 microsoft.com 網域時它會隨著每項要求網路上從該網域傳送任何 cookie。</span><span class="sxs-lookup"><span data-stu-id="4bbed-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="4bbed-155">重新命名以外 microsoft.com 網域名稱可以提高效能的最多 25%。</span><span class="sxs-lookup"><span data-stu-id="4bbed-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="4bbed-156">請注意 ajax.microsoft.com 仍會繼續運作，但 ajax.aspnetcdn.com 建議。</span><span class="sxs-lookup"><span data-stu-id="4bbed-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="4bbed-157">舊的格式： http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4bbed-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="4bbed-158">新的格式： http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="4bbed-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="4bbed-159">Visual Studio.vsdoc 支援</span><span class="sxs-lookup"><span data-stu-id="4bbed-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="4bbed-160">正確使用.vsdoc 檔案，您必須確定您有與 2008 SP1 的 Visual Studio 2008 安裝且已安裝 hotfix vsdoc 檔案。</span><span class="sxs-lookup"><span data-stu-id="4bbed-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="4bbed-161">您可以取得這些從這裡：</span><span class="sxs-lookup"><span data-stu-id="4bbed-161">You can get these from here:</span></span>

- [<span data-ttu-id="4bbed-162">下載 Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="4bbed-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "下載 Visual Studio 2008 SP1")
- [<span data-ttu-id="4bbed-163">下載 Visual Studio 2008 SP1 的.vsdoc hotfix</span><span class="sxs-lookup"><span data-stu-id="4bbed-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc hotfix 下載 Visual Studio 2008 SP1 的")

<span data-ttu-id="4bbed-164">Visual Studio 2010 支援.vsdoc 檔案，而任何額外的修補程式。</span><span class="sxs-lookup"><span data-stu-id="4bbed-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="4bbed-165">使用 ASP.NET Ajax cdn</span><span class="sxs-lookup"><span data-stu-id="4bbed-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="4bbed-166">當使用 ASP.NET 4 時，可以將所有要求的 ASP.NET framework 指令碼重新都導向至 CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="4bbed-167">從 CDN，而不是本機 web 伺服器擷取指令碼，可以大幅改善公用 ASP.NET 網站的效能。</span><span class="sxs-lookup"><span data-stu-id="4bbed-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="4bbed-168">若要將所有的 ASP.NET framework 指令碼要求重新導向至 Microsoft Ajax CDN 使用 ScriptManager EnableCDN 屬性：</span><span class="sxs-lookup"><span data-stu-id="4bbed-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="4bbed-169">使用 jQuery cdn</span><span class="sxs-lookup"><span data-stu-id="4bbed-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="4bbed-170">您可以使用 Web 應用程式中裝載在 CDN 上將下列指令碼項目加入至頁面的 jQuery 指令碼：</span><span class="sxs-lookup"><span data-stu-id="4bbed-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="4bbed-171">CDN 也會包含縮短的版本 jQuery 指令碼，就可以使用下列項目：</span><span class="sxs-lookup"><span data-stu-id="4bbed-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="4bbed-172">若要允許您的頁面載入 jQuery 從您自己的網站上的本機路徑，如果發生無法使用的 CDN 遞補，請參考 CDN 的項目之後，立即新增下列項目：</span><span class="sxs-lookup"><span data-stu-id="4bbed-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="4bbed-173">下列範例頁面會使用 CDN 版本 jQuery 程式庫 （後援至本機的複製） 若要在按下按鈕時顯示的 div 元素的內容。</span><span class="sxs-lookup"><span data-stu-id="4bbed-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="4bbed-174">您可以深入了解 jQuery 並造訪下載 jQuery 的本機副本[jQuery](http://jquery.com/)網站。</span><span class="sxs-lookup"><span data-stu-id="4bbed-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="4bbed-175">使用 jQuery UI CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="4bbed-176">CDN 也會裝載 jQuery UI 程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="4bbed-177">JQuery UI 程式庫包含一組豐富的 widget 和您可以在 ASP.NET 應用程式中使用的效果。</span><span class="sxs-lookup"><span data-stu-id="4bbed-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="4bbed-178">例如，下列頁面說明如何使用 jQuery UI 日期選擇器中 ASP.NET Web Form 應用程式的內容，以顯示快顯行事曆：</span><span class="sxs-lookup"><span data-stu-id="4bbed-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="4bbed-179">當您將焦點移至文字方塊中，使用鍵盤時，會顯示行事曆：</span><span class="sxs-lookup"><span data-stu-id="4bbed-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![建立日期選擇器快顯行事曆](overview/_static/image1.png)

<span data-ttu-id="4bbed-181">請注意，您必須包含三個檔案從 CDN 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="4bbed-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="4bbed-182">JQuery 程式庫&mdash;jQuery UI 程式庫相依於 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="4bbed-183">新增 jQuery UI 程式庫之前，您必須 jQuery 程式庫加入至您的頁面。</span><span class="sxs-lookup"><span data-stu-id="4bbed-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="4bbed-184">JQuery UI 程式庫&mdash;jQuery UI 程式庫包含所有的 jQuery UI 效果，例如 [日期選擇器] widget 中用於頁面上方的 widget。</span><span class="sxs-lookup"><span data-stu-id="4bbed-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="4bbed-185">JQuery UI 佈景主題&mdash;jQuery UI 支援不同的佈景主題。</span><span class="sxs-lookup"><span data-stu-id="4bbed-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="4bbed-186">頁面上方包括 CSS 檔案匯入 Redmond 佈景主題的連結。</span><span class="sxs-lookup"><span data-stu-id="4bbed-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="4bbed-187">所有標準的 jQuery UI 佈景主題會裝載在 CDN 上。</span><span class="sxs-lookup"><span data-stu-id="4bbed-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="4bbed-188">[造訪此頁](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 上 Microsoft Ajax CDN")檢視每個佈景主題的縮圖。</span><span class="sxs-lookup"><span data-stu-id="4bbed-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="4bbed-189">若要深入了解 jQuery UI 程式庫，請造訪正式[jQuery UI 網站](http://jQueryUI.com "jQuery UI 網站")。</span><span class="sxs-lookup"><span data-stu-id="4bbed-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="4bbed-190">CDN 的協力廠商檔案</span><span class="sxs-lookup"><span data-stu-id="4bbed-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="4bbed-191">CDN 裝載一些最受歡迎的第三方 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="4bbed-192">Microsoft 不會聲稱此 CDN 上裝載任何協力廠商程式庫的擁有權。</span><span class="sxs-lookup"><span data-stu-id="4bbed-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="4bbed-193">版權擁有者的程式庫會授權給您這些程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="4bbed-194">您可能要下載並使用這類程式庫的任何權限會授與完全各著作權擁有人。</span><span class="sxs-lookup"><span data-stu-id="4bbed-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="4bbed-195">因為這些不是 Microsoft 文件庫，Microsoft 提供任何瑕疵責任擔保或財產權限授權 （包括任何隱含的專利權限） 這個 CDN 上裝載的第三方廠商程式庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="4bbed-196">jQuery 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="4bbed-197">下列版本的 jQuery 裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="4bbed-198">jQuery 版本 3.3.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="4bbed-199">jQuery 版本 3.2.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="4bbed-200">jQuery 版本 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="4bbed-201">3.1.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="4bbed-202">jQuery 3.1.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="4bbed-203">jQuery 版本 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="4bbed-204">jQuery 版本 2.2.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="4bbed-205">jQuery 版本 2.2.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="4bbed-206">jQuery 版本 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="4bbed-207">jQuery 2.2.1 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="4bbed-208">jQuery 版本 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="4bbed-209">jQuery 版本 2.1.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="4bbed-210">jQuery 版本 2.1.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="4bbed-211">jQuery 版本 2.1.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="4bbed-212">jQuery 版本 2.1.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="4bbed-213">jQuery 版本 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="4bbed-214">jQuery 版本 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="4bbed-215">jQuery 版本 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="4bbed-216">jQuery 版本 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="4bbed-217">jQuery 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="4bbed-218">jQuery 版本 1.12.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="4bbed-219">jQuery 版本 1.12.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="4bbed-220">jQuery 版本 1.12.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="4bbed-221">jQuery 版本 1.12.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="4bbed-222">jQuery 版本 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="4bbed-223">jQuery 版本 1.11.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="4bbed-224">jQuery 版本 1.11.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="4bbed-225">jQuery 版本 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="4bbed-226">jQuery 版本 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="4bbed-227">jQuery 1.10.2 的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="4bbed-228">jQuery 版本 1.10.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="4bbed-229">jQuery 版本 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="4bbed-230">jQuery 版本 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="4bbed-231">jQuery 版本 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="4bbed-232">jQuery 版本 1.8.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="4bbed-233">jQuery 版本 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="4bbed-234">1.8.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="4bbed-235">jQuery 版本 1.8.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="4bbed-236">jQuery 版本 1.7.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="4bbed-237">jQuery 版本 1.7.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="4bbed-238">jQuery 1.7 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="4bbed-239">jQuery 版本 1.6.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="4bbed-240">jQuery 版本 1.6.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="4bbed-241">jQuery 版本 1.6.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="4bbed-242">jQuery 版本 1.6.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="4bbed-243">jQuery 1.6 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="4bbed-244">1.5.2 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="4bbed-245">1.5.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="4bbed-246">jQuery 1.5 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="4bbed-247">jQuery 版本 1.4.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="4bbed-248">jQuery 版本 1.4.3 將</span><span class="sxs-lookup"><span data-stu-id="4bbed-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="4bbed-249">1.4.2 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="4bbed-250">jQuery 版本 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="4bbed-251">jQuery 1.4 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="4bbed-252">jQuery 1.3.2 的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="4bbed-253">在 CDN 上 jQuery 移轉版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="4bbed-254">下列版本的 jQuery 移轉裝載於 CDN:</span><span class="sxs-lookup"><span data-stu-id="4bbed-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="4bbed-255">jQuery 3.0.0 版本移轉</span><span class="sxs-lookup"><span data-stu-id="4bbed-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="4bbed-256">jQuery 移轉 1.2.1 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="4bbed-257">jQuery 1.2.0 版本移轉</span><span class="sxs-lookup"><span data-stu-id="4bbed-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="4bbed-258">jQuery 1.1.1 版本移轉</span><span class="sxs-lookup"><span data-stu-id="4bbed-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="4bbed-259">jQuery 移轉 1.1.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="4bbed-260">jQuery 移轉 1.0.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="4bbed-261">jQuery UI 版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="4bbed-262">下列版本的 jQuery UI 程式庫被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-263">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="4bbed-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="4bbed-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="4bbed-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="4bbed-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="4bbed-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="4bbed-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="4bbed-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="4bbed-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="4bbed-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="4bbed-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="4bbed-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="4bbed-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="4bbed-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="4bbed-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="4bbed-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="4bbed-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="4bbed-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="4bbed-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="4bbed-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="4bbed-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="4bbed-299">jQuery 驗證版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="4bbed-300">下列版本的 jQuery 驗證程式庫被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-301">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-302">jQuery 驗證 1.17.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 驗證 1.17.0")
- [<span data-ttu-id="4bbed-303">jQuery 驗證 1.16.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 驗證 1.16.0")
- [<span data-ttu-id="4bbed-304">jQuery 驗證 1.15.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 驗證 1.15.1")
- [<span data-ttu-id="4bbed-305">jQuery 驗證 1.15.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 驗證 1.15.0")
- [<span data-ttu-id="4bbed-306">jQuery 驗證 1.14.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 驗證 1.14.0")
- [<span data-ttu-id="4bbed-307">jQuery 驗證 1.13.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 驗證 1.13.1")
- [<span data-ttu-id="4bbed-308">jQuery 驗證 1.13.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 驗證 1.13.0")
- [<span data-ttu-id="4bbed-309">jQuery 驗證 1.12.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 驗證 1.12.0")
- [<span data-ttu-id="4bbed-310">jQuery 驗證 1.11.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 驗證 1.11.1")
- [<span data-ttu-id="4bbed-311">jQuery 驗證 1.11.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 驗證 1.11.0")
- [<span data-ttu-id="4bbed-312">jQuery 驗證 1.10.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 驗證 1.10.0")
- [<span data-ttu-id="4bbed-313">jQuery 驗證 1.9</span><span class="sxs-lookup"><span data-stu-id="4bbed-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate 1.9 版的版本")
- [<span data-ttu-id="4bbed-314">jQuery 驗證 1.8.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate 版本 1.8.1")
- [<span data-ttu-id="4bbed-315">jQuery 驗證 1.8</span><span class="sxs-lookup"><span data-stu-id="4bbed-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate 1.8 版")
- [<span data-ttu-id="4bbed-316">jQuery 驗證 1.7</span><span class="sxs-lookup"><span data-stu-id="4bbed-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate 1.7 版")
- [<span data-ttu-id="4bbed-317">jQuery 驗證 1.6</span><span class="sxs-lookup"><span data-stu-id="4bbed-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery 驗證 1.6")
- [<span data-ttu-id="4bbed-318">jQuery 驗證 1.5.5</span><span class="sxs-lookup"><span data-stu-id="4bbed-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery 驗證 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="4bbed-319">jQuery Mobile 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="4bbed-320">此 CDN 上裝載下列 jQuery 行動文件庫的版本。</span><span class="sxs-lookup"><span data-stu-id="4bbed-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-321">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="4bbed-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4bbed-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="4bbed-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="4bbed-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="4bbed-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "上 Microsoft Ajax CDN 的 jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="4bbed-338">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="4bbed-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 上 Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="4bbed-339">jQuery 範本版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="4bbed-340">此 CDN 上裝載下列版本的 jQuery 範本的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="4bbed-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-341">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-342">jQuery 範本 Beta 1</span><span class="sxs-lookup"><span data-stu-id="4bbed-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery 範本 Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="4bbed-343">jQuery 在 CDN 上循環版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="4bbed-344">下列版本的 jQuery 循環外掛程式被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-345">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-346">jQuery 循環 2.99</span><span class="sxs-lookup"><span data-stu-id="4bbed-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 循環 2.99")
- [<span data-ttu-id="4bbed-347">jQuery 循環 2.94</span><span class="sxs-lookup"><span data-stu-id="4bbed-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 循環 2.94")
- [<span data-ttu-id="4bbed-348">jQuery 循環 2.88 b</span><span class="sxs-lookup"><span data-stu-id="4bbed-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 循環 2.88 b")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="4bbed-349">jQuery Datatable 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="4bbed-350">下列版本的 jQuery Datatable 外掛程式被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="4bbed-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="4bbed-351">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-352">jQuery Datatable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="4bbed-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery Datatable 1.10.5")
- [<span data-ttu-id="4bbed-353">jQuery Datatable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery Datatable 1.10.4")
- [<span data-ttu-id="4bbed-354">jQuery Datatable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery Datatable 1.9.4")
- [<span data-ttu-id="4bbed-355">jQuery Datatable 1.9.3 放</span><span class="sxs-lookup"><span data-stu-id="4bbed-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery Datatable 1.9.3 放")
- [<span data-ttu-id="4bbed-356">jQuery Datatable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery Datatable 1.9.2")
- [<span data-ttu-id="4bbed-357">jQuery Datatable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery Datatable 1.9.1")
- [<span data-ttu-id="4bbed-358">jQuery Datatable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery Datatable 1.9.0")
- [<span data-ttu-id="4bbed-359">jQuery Datatable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery Datatable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="4bbed-360">在 CDN 上 Modernizr 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="4bbed-361">下列版本的[Modernizr](http://www.modernizr.com "Modernizr")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="4bbed-362">在 CDN 上 JSHint 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="4bbed-363">下列版本的[JSHint](http://www.jshint.com "JSHint")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="4bbed-364">在 CDN 上 knockout 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="4bbed-365">下列版本的[Knockout](http://www.knockoutjs.com "Knockout")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="4bbed-366">全球化在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="4bbed-367">下列版本的[Globalize](https://github.com/jquery/globalize "Globalize")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="4bbed-368">全球化 1.0.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="4bbed-369">全球化版本 0.1.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="4bbed-370">所有文化特性</span><span class="sxs-lookup"><span data-stu-id="4bbed-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="4bbed-371">"{文化特性的程式碼取代} 」 所需的文化特性代碼，例如 globalize.culture.en GB.js== Microsoft 檔案 CDN = = 這些 Microsoft 上傳文件庫。</span><span class="sxs-lookup"><span data-stu-id="4bbed-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="4bbed-372">回應在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="4bbed-373">下列版本的[ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ")回應裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="4bbed-374">回應版本 1.4.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="4bbed-375">回應版本 1.4.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="4bbed-376">回應版本 1.4.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="4bbed-377">回應版本 1.3.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="4bbed-378">回應版本 1.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="4bbed-379">啟動程序在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="4bbed-380">下列版本的[getbootstrap.com](http://getbootstrap.com "getbootstrap.com")啟動程序在 CDN:</span><span class="sxs-lookup"><span data-stu-id="4bbed-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="4bbed-381">啟動程序 4.0.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="4bbed-382">啟動程序版本 3.3.7</span><span class="sxs-lookup"><span data-stu-id="4bbed-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="4bbed-383">啟動程序版本 3.3.6</span><span class="sxs-lookup"><span data-stu-id="4bbed-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="4bbed-384">啟動程序版本 3.3.5</span><span class="sxs-lookup"><span data-stu-id="4bbed-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="4bbed-385">啟動程序版本 3.3.4 章</span><span class="sxs-lookup"><span data-stu-id="4bbed-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="4bbed-386">啟動程序版本第 3.3.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="4bbed-387">啟動程序版本 3.3.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="4bbed-388">啟動程序版本 3.3.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="4bbed-389">啟動程序版本 3.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="4bbed-390">啟動程序版本 3.1.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="4bbed-391">啟動程序 3.1.0 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="4bbed-392">啟動程序版本 3.0.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="4bbed-393">啟動程序版本 3.0.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="4bbed-394">啟動程序版本 3.0.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="4bbed-395">啟動程序版本 3.0.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="4bbed-396">啟動程序版本 2.3.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="4bbed-397">啟動程序版本 2.3.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="4bbed-398">在 CDN 上的啟動程序 TouchCarousel 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="4bbed-399">下列版本的[ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") CDN 上裝載啟動程序 TouchCarousel 版本：</span><span class="sxs-lookup"><span data-stu-id="4bbed-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="4bbed-400">Bootstrap TouchCarousel version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="4bbed-401">在 CDN 上 Hammer.js 版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="4bbed-402">下列版本的[ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js 版本裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="4bbed-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="4bbed-403">Hammer.js 版本 2.0.4</span><span class="sxs-lookup"><span data-stu-id="4bbed-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="4bbed-404">ASP.NET Web Form 和 Ajax CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="4bbed-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="4bbed-405">下列版本的 ASP.NET Ajax 程式庫會在 CDN 上裝載。</span><span class="sxs-lookup"><span data-stu-id="4bbed-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="4bbed-406">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="4bbed-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="4bbed-407">ASP.NET Web Form 和 Ajax 4.5.2 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Form 和 Ajax 4.5.2")
- [<span data-ttu-id="4bbed-408">ASP.NET Web Form 和 Ajax 第 4 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Form 和 Ajax 4")
- [<span data-ttu-id="4bbed-409">ASP.NET Ajax 3.5 版</span><span class="sxs-lookup"><span data-stu-id="4bbed-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="4bbed-410">ASP.NET MVC 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="4bbed-411">此 CDN 上裝載下列 ASP.NET MVC JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="4bbed-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="4bbed-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="4bbed-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="4bbed-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="4bbed-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="4bbed-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="4bbed-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="4bbed-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="4bbed-419">ASP.NET SignalR 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="4bbed-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="4bbed-420">此 CDN 上裝載下列 ASP.NET SignalR JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="4bbed-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="4bbed-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="4bbed-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="4bbed-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="4bbed-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="4bbed-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="4bbed-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="4bbed-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="4bbed-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="4bbed-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="4bbed-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="4bbed-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="4bbed-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="4bbed-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="4bbed-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="4bbed-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="4bbed-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="4bbed-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="4bbed-434">如需使用條款 CDN 的資訊，請參閱[Microsoft Ajax CDN 使用規定](https://www.asp.net/terms-of-use "Microsoft Ajax CDN 使用規定")。</span><span class="sxs-lookup"><span data-stu-id="4bbed-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
