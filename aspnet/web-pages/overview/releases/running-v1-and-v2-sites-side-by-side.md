---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 執行不同版本的 ASP.NET Web Pages (Razor) 並排顯示 |Microsoft Docs
author: tfitzmac
description: 這篇文章說明如何在相同電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，當網站已設定為使用不同版本...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831140"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="e0e99-103">並存執行不同版本的 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="e0e99-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="e0e99-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e0e99-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e0e99-105">這篇文章說明如何執行相同的電腦或伺服器上的 ASP.NET Web Pages (Razor) 網站，當網站已設定為使用不同版本的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="e0e99-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="e0e99-106">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="e0e99-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e0e99-107">功能的預設行為是在 ASP.NET 中當您有站台，建置以 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="e0e99-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="e0e99-108">如何設定新站台執行較舊版本的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="e0e99-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="e0e99-109">這是發行項中導入的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="e0e99-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="e0e99-110">`webPages:Version`組態設定。</span><span class="sxs-lookup"><span data-stu-id="e0e99-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="e0e99-111">軟體版本</span><span class="sxs-lookup"><span data-stu-id="e0e99-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="e0e99-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="e0e99-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="e0e99-113">本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。</span><span class="sxs-lookup"><span data-stu-id="e0e99-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="e0e99-114">ASP.NET Web Pages 支援執行網站並排顯示的能力。</span><span class="sxs-lookup"><span data-stu-id="e0e99-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="e0e99-115">這可讓您繼續執行舊版的 ASP.NET Web Pages 應用程式，建置新的 ASP.NET Web Pages 應用程式，並執行所有人都在相同電腦上。</span><span class="sxs-lookup"><span data-stu-id="e0e99-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="e0e99-116">以下是要記住，當您使用 WebMatrix 安裝網頁時的一些事項：</span><span class="sxs-lookup"><span data-stu-id="e0e99-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="e0e99-117">根據預設，現有的 Web Pages 應用程式會在電腦上執行的最新版本。</span><span class="sxs-lookup"><span data-stu-id="e0e99-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="e0e99-118">（組件安裝在全域組件快取 (GAC) 中的而且系統會自動使用）。</span><span class="sxs-lookup"><span data-stu-id="e0e99-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="e0e99-119">如果您想要執行使用不同版本的 ASP.NET Web Pages 站台，您可以設定站台，若要這麼做。</span><span class="sxs-lookup"><span data-stu-id="e0e99-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="e0e99-120">如果您的網站尚未*web.config*檔案根目錄中的站台，建立一個新，並將下列 XML 複製到其中，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="e0e99-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="e0e99-121">如果網站已包含*web.config*檔案中，新增`<appSettings>`至如下所示的項目`<configuration>`一節。</span><span class="sxs-lookup"><span data-stu-id="e0e99-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="e0e99-122">'-如果您未指定的版本*web.config*檔案，網站會部署為最新版本。</span><span class="sxs-lookup"><span data-stu-id="e0e99-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="e0e99-123">(組件複製到*bin*資料夾中已部署的站台。)</span><span class="sxs-lookup"><span data-stu-id="e0e99-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="e0e99-124">您在 Webmatrix 中使用的網站範本所建立的新應用程式納入站台的網頁版的組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e0e99-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="e0e99-125">一般情況下，您一律可以控制的網頁，以搭配您的網站，使用 NuGet 將適當的組件安裝到站台的版本*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e0e99-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="e0e99-126">若要尋找套件，請造訪[NuGet.org](http://NuGet.org)。</span><span class="sxs-lookup"><span data-stu-id="e0e99-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0e99-127">其他資源</span><span class="sxs-lookup"><span data-stu-id="e0e99-127">Additional Resources</span></span>

[<span data-ttu-id="e0e99-128">ASP.NET Web pages 2 最上層的功能</span><span class="sxs-lookup"><span data-stu-id="e0e99-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
