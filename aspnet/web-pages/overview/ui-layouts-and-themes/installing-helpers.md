---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: 安裝協助程式中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 本文說明如何安裝 ASP.NET Web Pages (Razor) 網站中的協助程式。 協助程式是包含程式碼和標記每個可重複使用元件...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896772"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="b5ad4-104">安裝 ASP.NET Web Pages (Razor) 網站中的協助程式</span><span class="sxs-lookup"><span data-stu-id="b5ad4-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="b5ad4-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b5ad4-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b5ad4-106">本文說明如何安裝 ASP.NET Web Pages (Razor) 網站中的協助程式。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="b5ad4-107">A *helper*是可重複使用的元件，包括程式碼和標記，以執行可能很繁瑣或是複雜的工作。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="b5ad4-108">您將學習：</span><span class="sxs-lookup"><span data-stu-id="b5ad4-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="b5ad4-109">如何在使用 WebMatrix 3 所建立的網站上安裝的協助程式。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b5ad4-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="b5ad4-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b5ad4-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="b5ad4-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="b5ad4-112">協助程式的概觀</span><span class="sxs-lookup"><span data-stu-id="b5ad4-112">Overview of Helpers</span></span>

<span data-ttu-id="b5ad4-113">人們通常想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="b5ad4-114">範例包括顯示的圖表資料。將 Twitter [跟隨] 按鈕放在頁面上。傳送電子郵件是來自您的網站。裁剪或調整大小的影像。PayPal 用於您的網站。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="b5ad4-115">若要可讓您輕鬆執行這類項目，ASP.NET Web 網頁可讓您使用*helper*。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="b5ad4-116">協助程式是您安裝站台和，可讓您的元件會使用一條線或 Razor 程式碼的兩個執行一般工作。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="b5ad4-117">ASP.NET Web 網頁會有幾個內建的協助程式。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="b5ad4-118">然而，許多協助程式中的封裝 （增益集） 提供透過 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="b5ad4-119">NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="b5ad4-120">在 WebMatrix 3 安裝協助程式</span><span class="sxs-lookup"><span data-stu-id="b5ad4-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="b5ad4-121">在 WebMatrix 3 中，按一下 [ **NuGet** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![在 WebMatrix NuGet Gallery 對話方塊](installing-helpers/_static/image1.png)
2. <span data-ttu-id="b5ad4-123">這會啟動 NuGet 封裝管理員，並顯示可用的封裝。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="b5ad4-124">在 [搜尋] 方塊中，輸入您想要安裝的協助程式的關鍵字。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![在 WebMatrix NuGet Gallery 對話方塊](installing-helpers/_static/image2.png)
3. <span data-ttu-id="b5ad4-126">選取封裝，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="b5ad4-127">按一下**是**當系統詢問您要安裝封裝，並指出您接受條款。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="b5ad4-128">如果這是您已安裝的協助程式第一次，NuGet 會構成協助程式程式碼的網站中建立資料夾。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="b5ad4-129">若要解除安裝協助程式，請按一下**圖庫**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="b5ad4-130">安裝 Twitter helper</span><span class="sxs-lookup"><span data-stu-id="b5ad4-130">Installing the Twitter helper</span></span>

<span data-ttu-id="b5ad4-131">無法與您透過 NuGet 所安裝的 Twitter helper 相容 Twitter API 的最新版本。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="b5ad4-132">請改為參閱[使用 WebMatrix 的 Twitter Helper](twitter-helper.md)主題以取得如何設定您的專案中的 Twitter helper 的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="b5ad4-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="b5ad4-133">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5ad4-133">Additional Resources</span></span>


[<span data-ttu-id="b5ad4-134">介紹的 ASP.NET Web Pages 2-程式設計基本概念</span><span class="sxs-lookup"><span data-stu-id="b5ad4-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="b5ad4-135">有了 WebMatrix 的 twitter Helper</span><span class="sxs-lookup"><span data-stu-id="b5ad4-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
