---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: 轉譯 ASP.NET Web Pages (Razor) 網站的行動裝置 |Microsoft Docs
author: tfitzmac
description: 本文說明如何建立會適當地呈現在 行動裝置的 ASP.NET Web Pages (Razor) 網站中的頁面。 您將學到什麼： 您如何...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: d5c94af644c0dbc918544fe5112545c348136587
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369414"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="dd0b0-104">行動裝置轉譯 ASP.NET Web Pages (Razor) 網站</span><span class="sxs-lookup"><span data-stu-id="dd0b0-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="dd0b0-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="dd0b0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="dd0b0-106">本文說明如何建立會適當地呈現在 行動裝置的 ASP.NET Web Pages (Razor) 網站中的頁面。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="dd0b0-107">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="dd0b0-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="dd0b0-108">如何使用命名慣例，以指定的頁面被專為行動裝置。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="dd0b0-109">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="dd0b0-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="dd0b0-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="dd0b0-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="dd0b0-111">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="dd0b0-112">ASP.NET Web Pages 可讓您在行動裝置或其他裝置上建立自訂的顯示，來呈現內容。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="dd0b0-113">在 ASP.NET Web Pages 網站中建立裝置的特定頁面的最簡單方式是藉由使用檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="dd0b0-114">您可以建立兩個版本的頁面 (例如，一個名為<em>MyFile.cshtml</em>一個名為<em>MyFile.Mobile.cshtml</em>)。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-114">You can create two versions of a page (for example, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>).</span></span> <span data-ttu-id="dd0b0-115">在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，ASP.NET 會呈現從內容<em>MyFile.Mobile.cshtml</em>。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-115">At run time, when a mobile device requests <em>MyFile.cshtml</em>, ASP.NET renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="dd0b0-116">否則，請<em>MyFile.cshtml</em>轉譯。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-116">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="dd0b0-117">下列範例示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="dd0b0-118">*Page1.cshtml*包含內容，加上導覽提要欄位。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="dd0b0-119">*Page1.Mobile.cshtml*包含相同的內容，但省略了資訊看板。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="dd0b0-120">在 ASP.NET Web Pages 網站中，建立名為*Page1.cshtml* ，並以下列標記取代目前的內容。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="dd0b0-121">建立名為*Page1.Mobile.cshtml* ，並以下列標記取代現有的內容。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="dd0b0-122">請注意頁面的行動裝置的版本會省略更好的呈現較小螢幕上的 [導覽] 區段。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="dd0b0-123">執行桌面瀏覽器並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="dd0b0-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dd0b0-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="dd0b0-125">執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="dd0b0-126">(請注意，不包括 *.mobile。*</span><span class="sxs-lookup"><span data-stu-id="dd0b0-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="dd0b0-127">為 URL 的一部分。）即使要求是要*Page1.cshtml*，ASP.NET 會呈現*Page1.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="dd0b0-129">若要測試行動網頁，您可以使用行動裝置模擬器，在桌面的電腦上執行。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="dd0b0-130">此工具可讓您測試網頁，因為它們看起來在行動裝置上 （也就是通常具有較小顯示區域）。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="dd0b0-131">模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/firefox/addon/user-agent-switcher/)適用於 Mozilla Firefox，可讓您模擬各種不同的行動瀏覽器，從 Firefox 的桌面版本。</span><span class="sxs-lookup"><span data-stu-id="dd0b0-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="dd0b0-132">其他資源</span><span class="sxs-lookup"><span data-stu-id="dd0b0-132">Additional Resources</span></span>


<span data-ttu-id="dd0b0-133">[Windows Phone 模擬器](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="dd0b0-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
