---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "呈現 ASP.NET Web Pages (Razor) 站台行動裝置 |Microsoft 文件"
author: tfitzmac
description: "本文說明如何建立會適當地呈現在行動裝置的 ASP.NET Web Pages (Razor) 網站中的網頁。 您將學習： 您如何..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 08b714eb2ffaefc7c7e2e5c9a7428106b231e5b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="73dda-104">呈現 ASP.NET Web Pages (Razor) 站台的行動裝置</span><span class="sxs-lookup"><span data-stu-id="73dda-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="73dda-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="73dda-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="73dda-106">本文說明如何建立會適當地呈現在行動裝置的 ASP.NET Web Pages (Razor) 網站中的網頁。</span><span class="sxs-lookup"><span data-stu-id="73dda-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="73dda-107">您將學習：</span><span class="sxs-lookup"><span data-stu-id="73dda-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="73dda-108">如何使用命名慣例，以指定頁面所專用的行動裝置。</span><span class="sxs-lookup"><span data-stu-id="73dda-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73dda-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="73dda-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73dda-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="73dda-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="73dda-111">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="73dda-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="73dda-112">ASP.NET Web 網頁可讓您建立來呈現內容的自訂顯示行動裝置或其他裝置上。</span><span class="sxs-lookup"><span data-stu-id="73dda-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="73dda-113">在 ASP.NET Web Pages 站台中建立裝置的特定頁面的最簡單方式是使用檔案命名模式如下：*檔名。**行動**.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73dda-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="73dda-114">您可以建立兩個版本的網頁 (例如，一個名為*MyFile.cshtml* ，而另一個名為*MyFile.Mobile.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="73dda-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="73dda-115">在執行的階段，當行動裝置的要求*MyFile.cshtml*，ASP.NET 會呈現從內容*MyFile.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73dda-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="73dda-116">否則， *MyFile.cshtml*轉譯。</span><span class="sxs-lookup"><span data-stu-id="73dda-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="73dda-117">下列範例會示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。</span><span class="sxs-lookup"><span data-stu-id="73dda-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="73dda-118">*Page1.cshtml*包含內容加上導覽提要欄位。</span><span class="sxs-lookup"><span data-stu-id="73dda-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="73dda-119">*Page1.Mobile.cshtml*包含相同的內容，但會省略 [資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="73dda-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="73dda-120">在 ASP.NET Web Pages 站台中，建立名為*Page1.cshtml* ，並以下列標記取代目前的內容。</span><span class="sxs-lookup"><span data-stu-id="73dda-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="73dda-121">建立名為*Page1.Mobile.cshtml* ，並以下列標記取代現有的內容。</span><span class="sxs-lookup"><span data-stu-id="73dda-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="73dda-122">請注意行動版的頁面會省略更好的呈現較小螢幕上的瀏覽區段。</span><span class="sxs-lookup"><span data-stu-id="73dda-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="73dda-123">執行桌面瀏覽器並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73dda-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="73dda-124">![mobilesites 1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="73dda-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="73dda-125">執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73dda-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="73dda-126">(請注意，不包含*.mobile。*</span><span class="sxs-lookup"><span data-stu-id="73dda-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="73dda-127">做為 URL 的一部分。）即使該要求是*Page1.cshtml*，ASP.NET 會呈現*Page1.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73dda-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites 2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="73dda-129">若要測試行動頁面，您可以使用行動裝置模擬器，桌面的電腦上執行。</span><span class="sxs-lookup"><span data-stu-id="73dda-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="73dda-130">此工具可讓您測試網頁，因為它們會在行動裝置上看起來 （也就是通常具有較小顯示區域）。</span><span class="sxs-lookup"><span data-stu-id="73dda-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="73dda-131">模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox，這可讓您模擬 Firefox 桌面版本從各種行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="73dda-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="73dda-132">其他資源</span><span class="sxs-lookup"><span data-stu-id="73dda-132">Additional Resources</span></span>


<span data-ttu-id="73dda-133">[Windows Phone 模擬器](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="73dda-133">[Windows Phone Emulator](https://msdn.microsoft.com/en-us/library/ff402563(v=VS.92).aspx)</span></span>
