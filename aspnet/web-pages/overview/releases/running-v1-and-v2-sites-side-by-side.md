---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: 執行不同版本的 ASP.NET Web Pages (Razor) 並存 |Microsoft 文件
author: tfitzmac
description: 這篇文章說明如何在同一部電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，網站設定為使用不同版本時...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="a3eb3-103">並存執行不同版本的 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a3eb3-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="a3eb3-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a3eb3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a3eb3-105">本文說明如何在同一部電腦或伺服器上執行 ASP.NET Web Pages (Razor) 網站，網站設定為使用不同版本的 ASP.NET Web Pages 時。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="a3eb3-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="a3eb3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a3eb3-107">時，什麼的預設行為是在 ASP.NET 中您有建立以 ASP.NET Web Pages 站台。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="a3eb3-108">如何設定新的站台，以執行較舊版本的 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="a3eb3-109">這是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="a3eb3-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="a3eb3-110">`webPages:Version`組態設定。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="a3eb3-111">軟體版本</span><span class="sxs-lookup"><span data-stu-id="a3eb3-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="a3eb3-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a3eb3-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a3eb3-113">本教學課程也適用於 ASP.NET Web Pages 2 和 ASP.NET Web Pages 1.0。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="a3eb3-114">ASP.NET Web Pages 支援的功能執行並存的網站。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="a3eb3-115">這可讓您可以繼續執行舊版 ASP.NET Web Pages 應用程式、 建立新的 ASP.NET Web Pages 應用程式，以及在相同電腦上執行所有程式。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="a3eb3-116">以下是您使用 WebMatrix 安裝網頁時要注意一些事項：</span><span class="sxs-lookup"><span data-stu-id="a3eb3-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="a3eb3-117">根據預設，現有的 Web Pages 應用程式會在電腦上執行做為最新版本。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="a3eb3-118">（組件安裝在全域組件快取 (GAC) 中的並且會自動使用）。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="a3eb3-119">如果您想要執行不同版本的 ASP.NET Web Pages 站台，您可以設定站台執行此作業。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="a3eb3-120">如果還沒有網站*web.config*檔根目錄中的站台、 建立一個新並將下列 XML 複製到其中，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="a3eb3-121">如果網站已包含*web.config* file、 add`<appSettings>`元素類似下列的其中一個來`<configuration>`> 一節。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="a3eb3-122">'-如果您未指定的版本中*web.config*檔案，網站會部署為最新版本。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="a3eb3-123">(組件會複製到*bin*資料夾中已部署的站台。)</span><span class="sxs-lookup"><span data-stu-id="a3eb3-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="a3eb3-124">您使用 Web 矩陣中的網站範本所建立的新應用程式納入站台的網頁版組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="a3eb3-125">一般情況下，您一律可以控制哪個版本的網頁使用與您的網站使用 NuGet 將適當的組件安裝到站台的*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="a3eb3-126">若要尋找的封裝，請瀏覽[NuGet.org](http://NuGet.org)。</span><span class="sxs-lookup"><span data-stu-id="a3eb3-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3eb3-127">其他資源</span><span class="sxs-lookup"><span data-stu-id="a3eb3-127">Additional Resources</span></span>

[<span data-ttu-id="a3eb3-128">在 ASP.NET Web Pages 2 上的功能</span><span class="sxs-lookup"><span data-stu-id="a3eb3-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
