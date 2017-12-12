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
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="5375e-102">Microsoft Ajax 內容傳遞網路</span><span class="sxs-lookup"><span data-stu-id="5375e-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="5375e-103">注意： Microsoft Ajax CDN 有沒有超過使用 Azure CDN 的 SLA。</span><span class="sxs-lookup"><span data-stu-id="5375e-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="5375e-104">目錄</span><span class="sxs-lookup"><span data-stu-id="5375e-104">Table of Contents</span></span>

<span data-ttu-id="5375e-105">**[重新命名為 ajax.aspnetcdn.com ajax.microsoft.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="5375e-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="5375e-106">**[Visual Studio.vsdoc 支援](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="5375e-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="5375e-107">**[使用 ASP.NET Ajax cdn](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="5375e-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="5375e-108">**[使用 jQuery cdn](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="5375e-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="5375e-109">**[使用 jQuery UI CDN。](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="5375e-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="5375e-110">**[CDN 的協力廠商檔案](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="5375e-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="5375e-111">jQuery 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="5375e-112">在 CDN 上 jQuery 移轉版本</span><span class="sxs-lookup"><span data-stu-id="5375e-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="5375e-113">jQuery UI 版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="5375e-114">jQuery 驗證版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="5375e-115">jQuery Mobile 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="5375e-116">jQuery 範本版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="5375e-117">jQuery 在 CDN 上循環版本</span><span class="sxs-lookup"><span data-stu-id="5375e-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="5375e-118">jQuery Datatable 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="5375e-119">在 CDN 上 Modernizr 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="5375e-120">在 CDN 上 JSHint 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="5375e-121">在 CDN 上 knockout 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="5375e-122">全球化在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="5375e-123">回應在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="5375e-124">啟動程序在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="5375e-125">在 CDN 上的啟動程序 TouchCarousel 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="5375e-126">在 CDN 上 Hammer.js 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="5375e-127">ASP.NET Web Form 和 Ajax CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="5375e-128">ASP.NET MVC 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="5375e-129">ASP.NET SignalR 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="5375e-130">Microsoft Ajax 內容傳遞網路 (CDN) 可裝載受歡迎的協力廠商的 JavaScript 程式庫，例如 jQuery 並讓您輕鬆地將它們新增到您的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5375e-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="5375e-131">比方說，您可以啟動這個 CDN 上使用 jQuery 裝載，只要加入&lt;指令碼&gt;標記加入至您 ajax.aspnetcdn.com 所指向的頁面。</span><span class="sxs-lookup"><span data-stu-id="5375e-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="5375e-132">利用 CDN，您可以大幅改善 Ajax 應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="5375e-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="5375e-133">CDN 的內容會在位於世界各地的伺服器上快取。</span><span class="sxs-lookup"><span data-stu-id="5375e-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="5375e-134">此外，CDN 可讓瀏覽器與重複使用快取的第三方 JavaScript 檔案位於不同網域中的網站。</span><span class="sxs-lookup"><span data-stu-id="5375e-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="5375e-135">如果您需要提供使用 Secure Sockets Layer 的網頁，CDN 會支援 SSL (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="5375e-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="5375e-136">CDN 裝載下列第三方指令碼程式庫已上傳，且這些程式庫的擁有者滹砅凎您：</span><span class="sxs-lookup"><span data-stu-id="5375e-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="5375e-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="5375e-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="5375e-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="5375e-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="5375e-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="5375e-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="5375e-140">jQuery 驗證 (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="5375e-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="5375e-141">jQuery 循環 (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="5375e-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="5375e-142">jQuery Datatable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="5375e-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="5375e-143">Microsoft Ajax CDN 也包含由 Microsoft 已上傳的下列程式庫：</span><span class="sxs-lookup"><span data-stu-id="5375e-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="5375e-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="5375e-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="5375e-145">ASP.NET MVC JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="5375e-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="5375e-146">ASP.NET SignalR JavaScript 檔案</span><span class="sxs-lookup"><span data-stu-id="5375e-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="5375e-147">Microsoft 不會聲稱此 CDN 上裝載任何協力廠商程式庫的擁有權。</span><span class="sxs-lookup"><span data-stu-id="5375e-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="5375e-148">版權擁有者的程式庫會授權給您這些程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="5375e-149">您可能要下載並使用這類程式庫的任何權限會授與完全各著作權擁有人。</span><span class="sxs-lookup"><span data-stu-id="5375e-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="5375e-150">因為這些不是 Microsoft 文件庫，Microsoft 提供任何瑕疵責任擔保或財產權限授權 （包括任何隱含的專利權限） 這個 CDN 上裝載的第三方廠商程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="5375e-151">如果您想要提交您的 JavaScript 程式庫，而且您的程式庫是其中一個最上層的 JavaScript 程式庫 （如 http://trends.builtwith.com 上所列） 或延伸模組/外掛程式至這些程式庫 (a) 常用;或 (b) 有助於在 ASP.NET 上使用，請連絡AjaxCDNSubmission@Microsoft.com。</span><span class="sxs-lookup"><span data-stu-id="5375e-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="5375e-152">重新命名為 ajax.aspnetcdn.com ajax.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="5375e-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="5375e-153">CDN 用於使用 microsoft.com 網域名稱，而且已變更為使用 aspnetcdn.com 網域名稱。</span><span class="sxs-lookup"><span data-stu-id="5375e-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="5375e-154">這項變更已對提升效能，因為瀏覽器參考 microsoft.com 網域時它會隨著每項要求網路上從該網域傳送任何 cookie。</span><span class="sxs-lookup"><span data-stu-id="5375e-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="5375e-155">重新命名以外 microsoft.com 網域名稱可以提高效能的最多 25%。</span><span class="sxs-lookup"><span data-stu-id="5375e-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="5375e-156">請注意 ajax.microsoft.com 仍會繼續運作，但 ajax.aspnetcdn.com 建議。</span><span class="sxs-lookup"><span data-stu-id="5375e-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="5375e-157">舊的格式： http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="5375e-158">新的格式： http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="5375e-159">Visual Studio.vsdoc 支援</span><span class="sxs-lookup"><span data-stu-id="5375e-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="5375e-160">正確使用.vsdoc 檔案，您必須確定您有與 2008 SP1 的 Visual Studio 2008 安裝且已安裝 hotfix vsdoc 檔案。</span><span class="sxs-lookup"><span data-stu-id="5375e-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="5375e-161">您可以取得這些從這裡：</span><span class="sxs-lookup"><span data-stu-id="5375e-161">You can get these from here:</span></span>

- [<span data-ttu-id="5375e-162">下載 Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="5375e-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "下載 Visual Studio 2008 SP1")
- [<span data-ttu-id="5375e-163">下載 Visual Studio 2008 SP1 的.vsdoc hotfix</span><span class="sxs-lookup"><span data-stu-id="5375e-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc hotfix 下載 Visual Studio 2008 SP1 的")

<span data-ttu-id="5375e-164">Visual Studio 2010 支援.vsdoc 檔案，而任何額外的修補程式。</span><span class="sxs-lookup"><span data-stu-id="5375e-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="5375e-165">使用 ASP.NET Ajax cdn</span><span class="sxs-lookup"><span data-stu-id="5375e-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="5375e-166">當使用 ASP.NET 4 時，可以將所有要求的 ASP.NET framework 指令碼重新都導向至 CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="5375e-167">從 CDN，而不是本機 web 伺服器擷取指令碼，可以大幅改善公用 ASP.NET 網站的效能。</span><span class="sxs-lookup"><span data-stu-id="5375e-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="5375e-168">若要將所有的 ASP.NET framework 指令碼要求重新導向至 Microsoft Ajax CDN 使用 ScriptManager EnableCDN 屬性：</span><span class="sxs-lookup"><span data-stu-id="5375e-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="5375e-169">使用 jQuery cdn</span><span class="sxs-lookup"><span data-stu-id="5375e-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="5375e-170">您可以使用 Web 應用程式中裝載在 CDN 上將下列指令碼項目加入至頁面的 jQuery 指令碼：</span><span class="sxs-lookup"><span data-stu-id="5375e-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="5375e-171">CDN 也會包含縮短的版本 jQuery 指令碼，就可以使用下列項目：</span><span class="sxs-lookup"><span data-stu-id="5375e-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="5375e-172">若要允許您的頁面載入 jQuery 從您自己的網站上的本機路徑，如果發生無法使用的 CDN 遞補，請參考 CDN 的項目之後，立即新增下列項目：</span><span class="sxs-lookup"><span data-stu-id="5375e-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="5375e-173">下列範例頁面會使用 CDN 版本 jQuery 程式庫 （後援至本機的複製） 若要在按下按鈕時顯示的 div 元素的內容。</span><span class="sxs-lookup"><span data-stu-id="5375e-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="5375e-174">您可以深入了解 jQuery 並造訪下載 jQuery 的本機副本[jQuery](http://jquery.com/)網站。</span><span class="sxs-lookup"><span data-stu-id="5375e-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="5375e-175">使用 jQuery UI CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="5375e-176">CDN 也會裝載 jQuery UI 程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="5375e-177">JQuery UI 程式庫包含一組豐富的 widget 和您可以在 ASP.NET 應用程式中使用的效果。</span><span class="sxs-lookup"><span data-stu-id="5375e-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="5375e-178">例如，下列頁面說明如何使用 jQuery UI 日期選擇器中 ASP.NET Web Form 應用程式的內容，以顯示快顯行事曆：</span><span class="sxs-lookup"><span data-stu-id="5375e-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="5375e-179">當您將焦點移至文字方塊中，使用鍵盤時，會顯示行事曆：</span><span class="sxs-lookup"><span data-stu-id="5375e-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![建立日期選擇器快顯行事曆](overview/_static/image1.png)

<span data-ttu-id="5375e-181">請注意，您必須包含三個檔案從 CDN 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="5375e-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="5375e-182">JQuery 程式庫&mdash;jQuery UI 程式庫相依於 jQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="5375e-183">新增 jQuery UI 程式庫之前，您必須 jQuery 程式庫加入至您的頁面。</span><span class="sxs-lookup"><span data-stu-id="5375e-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="5375e-184">JQuery UI 程式庫&mdash;jQuery UI 程式庫包含所有的 jQuery UI 效果，例如 [日期選擇器] widget 中用於頁面上方的 widget。</span><span class="sxs-lookup"><span data-stu-id="5375e-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="5375e-185">JQuery UI 佈景主題&mdash;jQuery UI 支援不同的佈景主題。</span><span class="sxs-lookup"><span data-stu-id="5375e-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="5375e-186">頁面上方包括 CSS 檔案匯入 Redmond 佈景主題的連結。</span><span class="sxs-lookup"><span data-stu-id="5375e-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="5375e-187">所有標準的 jQuery UI 佈景主題會裝載在 CDN 上。</span><span class="sxs-lookup"><span data-stu-id="5375e-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="5375e-188">[造訪此頁](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 上 Microsoft Ajax CDN")檢視每個佈景主題的縮圖。</span><span class="sxs-lookup"><span data-stu-id="5375e-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="5375e-189">若要深入了解 jQuery UI 程式庫，請造訪正式[jQuery UI 網站](http://jQueryUI.com "jQuery UI 網站")。</span><span class="sxs-lookup"><span data-stu-id="5375e-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="5375e-190">CDN 的協力廠商檔案</span><span class="sxs-lookup"><span data-stu-id="5375e-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="5375e-191">CDN 裝載一些最受歡迎的第三方 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="5375e-192">Microsoft 不會聲稱此 CDN 上裝載任何協力廠商程式庫的擁有權。</span><span class="sxs-lookup"><span data-stu-id="5375e-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="5375e-193">版權擁有者的程式庫會授權給您這些程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="5375e-194">您可能要下載並使用這類程式庫的任何權限會授與完全各著作權擁有人。</span><span class="sxs-lookup"><span data-stu-id="5375e-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="5375e-195">因為這些不是 Microsoft 文件庫，Microsoft 提供任何瑕疵責任擔保或財產權限授權 （包括任何隱含的專利權限） 這個 CDN 上裝載的第三方廠商程式庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="5375e-196">jQuery 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="5375e-197">下列版本的 jQuery 裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="5375e-198">jQuery 版本 3.2.1</span><span class="sxs-lookup"><span data-stu-id="5375e-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="5375e-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="5375e-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="5375e-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="5375e-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span><span class="sxs-lookup"><span data-stu-id="5375e-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="5375e-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="5375e-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="5375e-205">jQuery 版本 3.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="5375e-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="5375e-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="5375e-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="5375e-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="5375e-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="5375e-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="5375e-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="5375e-212">3.1.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="5375e-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="5375e-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="5375e-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="5375e-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span><span class="sxs-lookup"><span data-stu-id="5375e-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="5375e-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="5375e-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="5375e-219">jQuery 3.1.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="5375e-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="5375e-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="5375e-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="5375e-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="5375e-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="5375e-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="5375e-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="5375e-226">jQuery 版本 3.0.0</span><span class="sxs-lookup"><span data-stu-id="5375e-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="5375e-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="5375e-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="5375e-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="5375e-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span><span class="sxs-lookup"><span data-stu-id="5375e-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="5375e-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="5375e-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="5375e-233">jQuery 版本 2.2.4</span><span class="sxs-lookup"><span data-stu-id="5375e-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="5375e-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="5375e-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="5375e-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="5375e-237">jQuery 版本 2.2.3</span><span class="sxs-lookup"><span data-stu-id="5375e-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="5375e-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="5375e-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="5375e-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="5375e-241">jQuery 版本 2.2.2</span><span class="sxs-lookup"><span data-stu-id="5375e-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="5375e-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="5375e-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="5375e-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="5375e-245">jQuery 2.2.1 版</span><span class="sxs-lookup"><span data-stu-id="5375e-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="5375e-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="5375e-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="5375e-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="5375e-249">jQuery 版本 2.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="5375e-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="5375e-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="5375e-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="5375e-253">jQuery 版本 2.1.4</span><span class="sxs-lookup"><span data-stu-id="5375e-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="5375e-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="5375e-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="5375e-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="5375e-257">jQuery 版本 2.1.3</span><span class="sxs-lookup"><span data-stu-id="5375e-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="5375e-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="5375e-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="5375e-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="5375e-261">jQuery 版本 2.1.2</span><span class="sxs-lookup"><span data-stu-id="5375e-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="5375e-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="5375e-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="5375e-264">jQuery 版本 2.1.1</span><span class="sxs-lookup"><span data-stu-id="5375e-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="5375e-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="5375e-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="5375e-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="5375e-268">jQuery 版本 2.1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="5375e-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="5375e-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="5375e-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="5375e-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="5375e-273">jQuery 版本 2.0.3</span><span class="sxs-lookup"><span data-stu-id="5375e-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="5375e-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="5375e-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="5375e-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="5375e-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="5375e-278">jQuery 版本 2.0.2</span><span class="sxs-lookup"><span data-stu-id="5375e-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="5375e-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="5375e-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="5375e-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="5375e-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="5375e-283">jQuery 版本 2.0.1</span><span class="sxs-lookup"><span data-stu-id="5375e-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="5375e-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="5375e-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="5375e-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="5375e-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="5375e-288">jQuery 2.0.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="5375e-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="5375e-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="5375e-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="5375e-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="5375e-293">jQuery 版本 1.12.4</span><span class="sxs-lookup"><span data-stu-id="5375e-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="5375e-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="5375e-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="5375e-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="5375e-297">jQuery 版本 1.12.3</span><span class="sxs-lookup"><span data-stu-id="5375e-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="5375e-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="5375e-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="5375e-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="5375e-301">jQuery 版本 1.12.2</span><span class="sxs-lookup"><span data-stu-id="5375e-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="5375e-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="5375e-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="5375e-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="5375e-305">jQuery 版本 1.12.1</span><span class="sxs-lookup"><span data-stu-id="5375e-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="5375e-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="5375e-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="5375e-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="5375e-309">jQuery 版本 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5375e-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="5375e-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="5375e-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="5375e-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="5375e-313">jQuery 版本 1.11.3</span><span class="sxs-lookup"><span data-stu-id="5375e-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="5375e-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="5375e-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="5375e-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="5375e-317">jQuery 版本 1.11.2</span><span class="sxs-lookup"><span data-stu-id="5375e-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="5375e-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="5375e-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="5375e-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="5375e-321">jQuery 版本 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5375e-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="5375e-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="5375e-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="5375e-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="5375e-325">jQuery 版本 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5375e-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="5375e-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="5375e-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="5375e-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="5375e-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="5375e-330">jQuery 1.10.2 的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="5375e-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="5375e-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="5375e-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="5375e-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="5375e-335">jQuery 版本 1.10.1</span><span class="sxs-lookup"><span data-stu-id="5375e-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="5375e-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="5375e-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="5375e-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="5375e-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="5375e-340">jQuery 版本 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5375e-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="5375e-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="5375e-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="5375e-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="5375e-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="5375e-345">jQuery 版本 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5375e-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="5375e-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="5375e-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="5375e-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="5375e-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="5375e-350">jQuery 版本 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5375e-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="5375e-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="5375e-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="5375e-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="5375e-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="5375e-355">jQuery 版本 1.8.3</span><span class="sxs-lookup"><span data-stu-id="5375e-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="5375e-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="5375e-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="5375e-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="5375e-359">jQuery 版本 1.8.2</span><span class="sxs-lookup"><span data-stu-id="5375e-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="5375e-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="5375e-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="5375e-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="5375e-363">1.8.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="5375e-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="5375e-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="5375e-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="5375e-367">jQuery 版本 1.8.0</span><span class="sxs-lookup"><span data-stu-id="5375e-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="5375e-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="5375e-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="5375e-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="5375e-371">jQuery 版本 1.7.2</span><span class="sxs-lookup"><span data-stu-id="5375e-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="5375e-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="5375e-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="5375e-374">jQuery 版本 1.7.1</span><span class="sxs-lookup"><span data-stu-id="5375e-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="5375e-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="5375e-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="5375e-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="5375e-378">jQuery 1.7 版</span><span class="sxs-lookup"><span data-stu-id="5375e-378">jQuery version 1.7</span></span>

- <span data-ttu-id="5375e-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="5375e-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="5375e-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="5375e-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="5375e-382">jQuery 版本 1.6.4</span><span class="sxs-lookup"><span data-stu-id="5375e-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="5375e-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="5375e-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="5375e-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="5375e-386">jQuery 版本 1.6.3</span><span class="sxs-lookup"><span data-stu-id="5375e-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="5375e-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="5375e-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="5375e-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="5375e-390">jQuery 版本 1.6.2</span><span class="sxs-lookup"><span data-stu-id="5375e-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="5375e-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="5375e-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="5375e-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="5375e-394">jQuery 版本 1.6.1</span><span class="sxs-lookup"><span data-stu-id="5375e-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="5375e-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="5375e-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="5375e-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="5375e-398">jQuery 1.6 版</span><span class="sxs-lookup"><span data-stu-id="5375e-398">jQuery version 1.6</span></span>

- <span data-ttu-id="5375e-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="5375e-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="5375e-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="5375e-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="5375e-402">1.5.2 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="5375e-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="5375e-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="5375e-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="5375e-406">1.5.1 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="5375e-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="5375e-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="5375e-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="5375e-410">jQuery 1.5 版</span><span class="sxs-lookup"><span data-stu-id="5375e-410">jQuery version 1.5</span></span>

- <span data-ttu-id="5375e-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="5375e-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="5375e-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="5375e-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="5375e-414">jQuery 版本 1.4.4</span><span class="sxs-lookup"><span data-stu-id="5375e-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="5375e-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="5375e-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="5375e-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="5375e-418">jQuery 版本 1.4.3 將</span><span class="sxs-lookup"><span data-stu-id="5375e-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="5375e-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="5375e-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="5375e-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="5375e-422">1.4.2 的 jQuery 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="5375e-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="5375e-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="5375e-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="5375e-426">jQuery 版本 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5375e-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="5375e-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="5375e-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="5375e-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="5375e-430">jQuery 1.4 版</span><span class="sxs-lookup"><span data-stu-id="5375e-430">jQuery version 1.4</span></span>

- <span data-ttu-id="5375e-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="5375e-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="5375e-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="5375e-433">jQuery 1.3.2 的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="5375e-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="5375e-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="5375e-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="5375e-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="5375e-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="5375e-438">在 CDN 上 jQuery 移轉版本</span><span class="sxs-lookup"><span data-stu-id="5375e-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="5375e-439">下列版本的 jQuery 移轉裝載於 CDN:</span><span class="sxs-lookup"><span data-stu-id="5375e-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="5375e-440">jQuery 3.0.0 版本移轉</span><span class="sxs-lookup"><span data-stu-id="5375e-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="5375e-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="5375e-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="5375e-443">jQuery 移轉 1.2.1 版</span><span class="sxs-lookup"><span data-stu-id="5375e-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="5375e-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="5375e-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="5375e-446">jQuery 1.2.0 版本移轉</span><span class="sxs-lookup"><span data-stu-id="5375e-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="5375e-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="5375e-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="5375e-449">jQuery 1.1.1 版本移轉</span><span class="sxs-lookup"><span data-stu-id="5375e-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="5375e-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="5375e-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="5375e-452">jQuery 移轉 1.1.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="5375e-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="5375e-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="5375e-455">jQuery 移轉 1.0.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="5375e-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="5375e-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="5375e-458">jQuery UI 版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="5375e-459">下列版本的 jQuery UI 程式庫被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="5375e-460">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="5375e-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5375e-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="5375e-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="5375e-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="5375e-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5375e-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5375e-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="5375e-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="5375e-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="5375e-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="5375e-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5375e-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="5375e-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5375e-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5375e-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="5375e-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="5375e-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="5375e-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="5375e-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="5375e-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="5375e-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="5375e-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="5375e-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="5375e-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="5375e-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="5375e-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="5375e-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="5375e-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="5375e-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="5375e-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="5375e-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="5375e-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="5375e-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="5375e-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="5375e-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="5375e-496">jQuery 驗證版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="5375e-497">下列版本的 jQuery 驗證程式庫被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="5375e-498">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-499">jQuery 驗證 1.17.0</span><span class="sxs-lookup"><span data-stu-id="5375e-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery 驗證 1.17.0")
- [<span data-ttu-id="5375e-500">jQuery 驗證 1.16.0</span><span class="sxs-lookup"><span data-stu-id="5375e-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery 驗證 1.16.0")
- [<span data-ttu-id="5375e-501">jQuery 驗證 1.15.1</span><span class="sxs-lookup"><span data-stu-id="5375e-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery 驗證 1.15.1")
- [<span data-ttu-id="5375e-502">jQuery 驗證 1.15.0</span><span class="sxs-lookup"><span data-stu-id="5375e-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery 驗證 1.15.0")
- [<span data-ttu-id="5375e-503">jQuery 驗證 1.14.0</span><span class="sxs-lookup"><span data-stu-id="5375e-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery 驗證 1.14.0")
- [<span data-ttu-id="5375e-504">jQuery 驗證 1.13.1</span><span class="sxs-lookup"><span data-stu-id="5375e-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery 驗證 1.13.1")
- [<span data-ttu-id="5375e-505">jQuery 驗證 1.13.0</span><span class="sxs-lookup"><span data-stu-id="5375e-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery 驗證 1.13.0")
- [<span data-ttu-id="5375e-506">jQuery 驗證 1.12.0</span><span class="sxs-lookup"><span data-stu-id="5375e-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery 驗證 1.12.0")
- [<span data-ttu-id="5375e-507">jQuery 驗證 1.11.1</span><span class="sxs-lookup"><span data-stu-id="5375e-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery 驗證 1.11.1")
- [<span data-ttu-id="5375e-508">jQuery 驗證 1.11.0</span><span class="sxs-lookup"><span data-stu-id="5375e-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery 驗證 1.11.0")
- [<span data-ttu-id="5375e-509">jQuery 驗證 1.10.0</span><span class="sxs-lookup"><span data-stu-id="5375e-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery 驗證 1.10.0")
- [<span data-ttu-id="5375e-510">jQuery 驗證 1.9</span><span class="sxs-lookup"><span data-stu-id="5375e-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate 1.9 版的版本")
- [<span data-ttu-id="5375e-511">jQuery 驗證 1.8.1</span><span class="sxs-lookup"><span data-stu-id="5375e-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate 版本 1.8.1")
- [<span data-ttu-id="5375e-512">jQuery 驗證 1.8</span><span class="sxs-lookup"><span data-stu-id="5375e-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate 1.8 版")
- [<span data-ttu-id="5375e-513">jQuery 驗證 1.7</span><span class="sxs-lookup"><span data-stu-id="5375e-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate 1.7 版")
- [<span data-ttu-id="5375e-514">jQuery 驗證 1.6</span><span class="sxs-lookup"><span data-stu-id="5375e-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery 驗證 1.6")
- [<span data-ttu-id="5375e-515">jQuery 驗證 1.5.5</span><span class="sxs-lookup"><span data-stu-id="5375e-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery 驗證 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="5375e-516">jQuery Mobile 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="5375e-517">此 CDN 上裝載下列 jQuery 行動文件庫的版本。</span><span class="sxs-lookup"><span data-stu-id="5375e-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="5375e-518">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="5375e-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="5375e-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5375e-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="5375e-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="5375e-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="5375e-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="5375e-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="5375e-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="5375e-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="5375e-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="5375e-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="5375e-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 上 Microsoft Ajax CDN")
- [<span data-ttu-id="5375e-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="5375e-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "上 Microsoft Ajax CDN 的 jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="5375e-535">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="5375e-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 上 Microsoft Ajax CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="5375e-536">jQuery 範本版本，在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="5375e-537">此 CDN 上裝載下列版本的 jQuery 範本的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="5375e-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="5375e-538">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-539">jQuery 範本 Beta 1</span><span class="sxs-lookup"><span data-stu-id="5375e-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery 範本 Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="5375e-540">jQuery 在 CDN 上循環版本</span><span class="sxs-lookup"><span data-stu-id="5375e-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="5375e-541">下列版本的 jQuery 循環外掛程式被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="5375e-542">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-543">jQuery 循環 2.99</span><span class="sxs-lookup"><span data-stu-id="5375e-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 循環 2.99")
- [<span data-ttu-id="5375e-544">jQuery 循環 2.94</span><span class="sxs-lookup"><span data-stu-id="5375e-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery 循環 2.94")
- [<span data-ttu-id="5375e-545">jQuery 循環 2.88 b</span><span class="sxs-lookup"><span data-stu-id="5375e-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 循環 2.88 b")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="5375e-546">jQuery Datatable 版本在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="5375e-547">下列版本的 jQuery Datatable 外掛程式被在這個 CDN。</span><span class="sxs-lookup"><span data-stu-id="5375e-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="5375e-548">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-549">jQuery Datatable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="5375e-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery Datatable 1.10.5")
- [<span data-ttu-id="5375e-550">jQuery Datatable 1.10.4</span><span class="sxs-lookup"><span data-stu-id="5375e-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery Datatable 1.10.4")
- [<span data-ttu-id="5375e-551">jQuery Datatable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="5375e-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery Datatable 1.9.4")
- [<span data-ttu-id="5375e-552">jQuery Datatable 1.9.3 放</span><span class="sxs-lookup"><span data-stu-id="5375e-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery Datatable 1.9.3 放")
- [<span data-ttu-id="5375e-553">jQuery Datatable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="5375e-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery Datatable 1.9.2")
- [<span data-ttu-id="5375e-554">jQuery Datatable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="5375e-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery Datatable 1.9.1")
- [<span data-ttu-id="5375e-555">jQuery Datatable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="5375e-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery Datatable 1.9.0")
- [<span data-ttu-id="5375e-556">jQuery Datatable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="5375e-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery Datatable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="5375e-557">在 CDN 上 Modernizr 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="5375e-558">下列版本的[Modernizr](http://www.modernizr.com "Modernizr")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="5375e-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="5375e-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="5375e-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="5375e-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="5375e-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span><span class="sxs-lookup"><span data-stu-id="5375e-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="5375e-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span><span class="sxs-lookup"><span data-stu-id="5375e-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="5375e-565">在 CDN 上 JSHint 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="5375e-566">下列版本的[JSHint](http://www.jshint.com "JSHint")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="5375e-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="5375e-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="5375e-568">在 CDN 上 knockout 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="5375e-569">下列版本的[Knockout](http://www.knockoutjs.com "Knockout")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="5375e-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="5375e-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="5375e-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="5375e-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="5375e-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="5375e-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="5375e-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="5375e-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="5375e-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="5375e-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="5375e-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="5375e-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="5375e-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="5375e-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="5375e-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="5375e-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="5375e-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="5375e-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="5375e-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="5375e-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="5375e-590">全球化在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="5375e-591">下列版本的[Globalize](https://github.com/jquery/globalize "Globalize")裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="5375e-592">全球化 1.0.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="5375e-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="5375e-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="5375e-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span><span class="sxs-lookup"><span data-stu-id="5375e-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="5375e-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span><span class="sxs-lookup"><span data-stu-id="5375e-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="5375e-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span><span class="sxs-lookup"><span data-stu-id="5375e-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="5375e-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span><span class="sxs-lookup"><span data-stu-id="5375e-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="5375e-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span><span class="sxs-lookup"><span data-stu-id="5375e-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="5375e-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="5375e-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="5375e-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span><span class="sxs-lookup"><span data-stu-id="5375e-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="5375e-601">全球化版本 0.1.1</span><span class="sxs-lookup"><span data-stu-id="5375e-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="5375e-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="5375e-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="5375e-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="5375e-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="5375e-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="5375e-605">所有文化特性</span><span class="sxs-lookup"><span data-stu-id="5375e-605">all cultures</span></span>
- <span data-ttu-id="5375e-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture。{文化特性代碼}.js</span><span class="sxs-lookup"><span data-stu-id="5375e-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="5375e-607">"{文化特性的程式碼取代} 」 所需的文化特性代碼，例如 globalize.culture.en GB.js== Microsoft 檔案 CDN = = 這些 Microsoft 上傳文件庫。</span><span class="sxs-lookup"><span data-stu-id="5375e-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="5375e-608">回應在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="5375e-609">下列版本的[https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond")回應裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="5375e-610">回應版本 1.4.2</span><span class="sxs-lookup"><span data-stu-id="5375e-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="5375e-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span><span class="sxs-lookup"><span data-stu-id="5375e-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="5375e-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="5375e-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5375e-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5375e-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="5375e-615">回應版本 1.4.1</span><span class="sxs-lookup"><span data-stu-id="5375e-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="5375e-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span><span class="sxs-lookup"><span data-stu-id="5375e-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="5375e-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="5375e-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5375e-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5375e-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="5375e-620">回應版本 1.4.0</span><span class="sxs-lookup"><span data-stu-id="5375e-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="5375e-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="5375e-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="5375e-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="5375e-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="5375e-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="5375e-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="5375e-625">回應版本 1.3.0</span><span class="sxs-lookup"><span data-stu-id="5375e-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="5375e-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="5375e-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="5375e-627">回應版本 1.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="5375e-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span><span class="sxs-lookup"><span data-stu-id="5375e-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="5375e-629">啟動程序在 CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="5375e-630">下列版本的[getbootstrap.com](http://getbootstrap.com "getbootstrap.com")啟動程序在 CDN:</span><span class="sxs-lookup"><span data-stu-id="5375e-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="5375e-631">啟動程序版本 3.3.7</span><span class="sxs-lookup"><span data-stu-id="5375e-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="5375e-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="5375e-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5375e-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5375e-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="5375e-645">啟動程序版本 3.3.6</span><span class="sxs-lookup"><span data-stu-id="5375e-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="5375e-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="5375e-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5375e-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5375e-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="5375e-659">啟動程序版本 3.3.5</span><span class="sxs-lookup"><span data-stu-id="5375e-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="5375e-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="5375e-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5375e-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5375e-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="5375e-673">啟動程序版本 3.3.4 章</span><span class="sxs-lookup"><span data-stu-id="5375e-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="5375e-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="5375e-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5375e-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5375e-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="5375e-687">啟動程序版本第 3.3.2</span><span class="sxs-lookup"><span data-stu-id="5375e-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="5375e-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="5375e-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="5375e-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="5375e-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="5375e-701">啟動程序版本 3.3.1</span><span class="sxs-lookup"><span data-stu-id="5375e-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="5375e-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="5375e-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="5375e-714">啟動程序版本 3.3.0</span><span class="sxs-lookup"><span data-stu-id="5375e-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="5375e-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="5375e-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="5375e-727">啟動程序版本 3.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="5375e-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="5375e-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="5375e-740">啟動程序版本 3.1.1</span><span class="sxs-lookup"><span data-stu-id="5375e-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="5375e-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="5375e-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="5375e-753">啟動程序 3.1.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="5375e-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="5375e-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="5375e-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="5375e-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="5375e-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="5375e-766">啟動程序版本 3.0.3</span><span class="sxs-lookup"><span data-stu-id="5375e-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="5375e-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="5375e-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="5375e-777">啟動程序版本 3.0.2</span><span class="sxs-lookup"><span data-stu-id="5375e-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="5375e-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="5375e-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="5375e-788">啟動程序版本 3.0.1</span><span class="sxs-lookup"><span data-stu-id="5375e-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="5375e-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="5375e-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="5375e-799">啟動程序版本 3.0.0</span><span class="sxs-lookup"><span data-stu-id="5375e-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="5375e-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="5375e-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="5375e-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="5375e-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="5375e-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="5375e-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="5375e-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span><span class="sxs-lookup"><span data-stu-id="5375e-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="5375e-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="5375e-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="5375e-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="5375e-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="5375e-810">啟動程序版本 2.3.2</span><span class="sxs-lookup"><span data-stu-id="5375e-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="5375e-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="5375e-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="5375e-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="5375e-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="5375e-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="5375e-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="5375e-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span><span class="sxs-lookup"><span data-stu-id="5375e-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="5375e-819">啟動程序版本 2.3.1</span><span class="sxs-lookup"><span data-stu-id="5375e-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="5375e-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="5375e-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="5375e-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="5375e-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="5375e-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="5375e-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="5375e-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="5375e-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="5375e-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span><span class="sxs-lookup"><span data-stu-id="5375e-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="5375e-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="5375e-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="5375e-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span><span class="sxs-lookup"><span data-stu-id="5375e-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="5375e-828">在 CDN 上的啟動程序 TouchCarousel 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="5375e-829">下列版本的[https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel 版本在 CDN:</span><span class="sxs-lookup"><span data-stu-id="5375e-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="5375e-830">啟動程序 TouchCarousel 0.8.0 版</span><span class="sxs-lookup"><span data-stu-id="5375e-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="5375e-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="5375e-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="5375e-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="5375e-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="5375e-833">在 CDN 上 Hammer.js 版本</span><span class="sxs-lookup"><span data-stu-id="5375e-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="5375e-834">下列版本的[http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js 版本裝載在 CDN 上：</span><span class="sxs-lookup"><span data-stu-id="5375e-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="5375e-835">Hammer.js 版本 2.0.4</span><span class="sxs-lookup"><span data-stu-id="5375e-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="5375e-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="5375e-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="5375e-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="5375e-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span><span class="sxs-lookup"><span data-stu-id="5375e-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="5375e-839">ASP.NET Web Form 和 Ajax CDN 上的版本</span><span class="sxs-lookup"><span data-stu-id="5375e-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="5375e-840">下列版本的 ASP.NET Ajax 程式庫會在 CDN 上裝載。</span><span class="sxs-lookup"><span data-stu-id="5375e-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="5375e-841">按一下每個連結，以查看實際的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="5375e-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="5375e-842">ASP.NET Web Form 和 Ajax 4.5.2 版</span><span class="sxs-lookup"><span data-stu-id="5375e-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Form 和 Ajax 4.5.2")
- [<span data-ttu-id="5375e-843">ASP.NET Web Form 和 Ajax 第 4 版</span><span class="sxs-lookup"><span data-stu-id="5375e-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Form 和 Ajax 4")
- [<span data-ttu-id="5375e-844">ASP.NET Ajax 3.5 版</span><span class="sxs-lookup"><span data-stu-id="5375e-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="5375e-845">ASP.NET MVC 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="5375e-846">此 CDN 上裝載下列 ASP.NET MVC JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="5375e-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="5375e-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="5375e-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="5375e-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5375e-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5375e-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="5375e-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="5375e-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="5375e-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5375e-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5375e-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="5375e-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="5375e-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="5375e-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5375e-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5375e-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="5375e-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="5375e-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="5375e-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5375e-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5375e-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="5375e-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="5375e-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="5375e-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span><span class="sxs-lookup"><span data-stu-id="5375e-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="5375e-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="5375e-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="5375e-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="5375e-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="5375e-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5375e-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5375e-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="5375e-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="5375e-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5375e-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5375e-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="5375e-869">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="5375e-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="5375e-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="5375e-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span><span class="sxs-lookup"><span data-stu-id="5375e-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="5375e-872">ASP.NET SignalR 釋放在 CDN 上</span><span class="sxs-lookup"><span data-stu-id="5375e-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="5375e-873">此 CDN 上裝載下列 ASP.NET SignalR JavaScript 檔案：</span><span class="sxs-lookup"><span data-stu-id="5375e-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="5375e-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="5375e-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="5375e-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="5375e-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="5375e-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="5375e-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="5375e-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="5375e-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="5375e-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="5375e-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="5375e-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="5375e-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="5375e-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="5375e-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="5375e-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="5375e-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="5375e-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="5375e-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="5375e-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="5375e-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="5375e-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="5375e-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="5375e-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="5375e-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="5375e-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="5375e-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="5375e-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="5375e-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="5375e-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="5375e-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="5375e-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="5375e-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="5375e-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="5375e-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="5375e-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="5375e-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="5375e-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="5375e-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="5375e-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="5375e-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="5375e-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="5375e-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="5375e-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="5375e-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="5375e-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="5375e-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="5375e-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="5375e-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="5375e-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="5375e-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="5375e-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="5375e-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="5375e-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="5375e-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="5375e-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="5375e-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="5375e-913">如需使用條款 CDN 的資訊，請參閱[Microsoft Ajax CDN 使用規定](https://www.asp.net/terms-of-use "Microsoft Ajax CDN 使用規定")。</span><span class="sxs-lookup"><span data-stu-id="5375e-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
