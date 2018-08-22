---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: 顯示地圖，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 這篇文章說明如何依據對應 Bing、 Google、 Ma 所提供的服務，ASP.NET Web Pages (Razor) 網站中的頁面上顯示互動式地圖...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 0b0879047bc71057884ebef98a598eed8c28cddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833827"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="304ea-103">在 ASP.NET Web Pages (Razor) 網站中顯示地圖</span><span class="sxs-lookup"><span data-stu-id="304ea-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="304ea-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="304ea-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="304ea-105">這篇文章說明如何依據對應 Bing、 Google、 MapQuest 和 Yahoo 所提供的服務，ASP.NET Web Pages (Razor) 網站中的頁面上顯示互動式地圖。</span><span class="sxs-lookup"><span data-stu-id="304ea-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="304ea-106">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="304ea-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="304ea-107">如何產生在地圖上的位址。</span><span class="sxs-lookup"><span data-stu-id="304ea-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="304ea-108">如何產生根據緯度和經度座標的對應。</span><span class="sxs-lookup"><span data-stu-id="304ea-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="304ea-109">如何註冊 Bing 地圖服務開發人員帳戶，並取得使用與 Bing 地圖服務金鑰。</span><span class="sxs-lookup"><span data-stu-id="304ea-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="304ea-110">這是發行項中導入的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="304ea-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="304ea-111">`Maps`協助程式。</span><span class="sxs-lookup"><span data-stu-id="304ea-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="304ea-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="304ea-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="304ea-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="304ea-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="304ea-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="304ea-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="304ea-115">本教學課程也適用於 WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="304ea-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="304ea-116">在網頁中，您可以顯示對應頁面上使用`Maps`協助程式。</span><span class="sxs-lookup"><span data-stu-id="304ea-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="304ea-117">您可以產生根據位址或一組經度和緯度座標的對應。</span><span class="sxs-lookup"><span data-stu-id="304ea-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="304ea-118">`Maps`類別可讓您呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 的熱門地圖引擎。</span><span class="sxs-lookup"><span data-stu-id="304ea-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="304ea-119">將對應加入至頁面的步驟都相同不論哪一個 map 引擎呼叫。</span><span class="sxs-lookup"><span data-stu-id="304ea-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="304ea-120">您只要新增 JavaScript 檔案參考，讓可用的方法，以顯示地圖上，然後再呼叫的方法`Maps`協助程式。</span><span class="sxs-lookup"><span data-stu-id="304ea-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="304ea-121">您可以選擇依據地圖服務`Maps`您所使用的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="304ea-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="304ea-122">您可以使用其中任何一項：</span><span class="sxs-lookup"><span data-stu-id="304ea-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="304ea-123">安裝您需要的片段</span><span class="sxs-lookup"><span data-stu-id="304ea-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="304ea-124">若要顯示地圖，您需要這些項目：</span><span class="sxs-lookup"><span data-stu-id="304ea-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="304ea-125">`Maps`協助程式。</span><span class="sxs-lookup"><span data-stu-id="304ea-125">The `Maps` helper.</span></span> <span data-ttu-id="304ea-126">此協助程式是 ASP.NET Web Helpers Library 第 2 版中。</span><span class="sxs-lookup"><span data-stu-id="304ea-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="304ea-127">如果您尚未新增程式庫，您可以將它安裝在您的網站以 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="304ea-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="304ea-128">如需詳細資訊，請參閱 <<c0> [ 安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)。</span><span class="sxs-lookup"><span data-stu-id="304ea-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="304ea-129">(在 資源庫中，搜尋`microsoft-web-helpers`封裝。)</span><span class="sxs-lookup"><span data-stu-id="304ea-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="304ea-130">JQuery 程式庫。</span><span class="sxs-lookup"><span data-stu-id="304ea-130">The jQuery library.</span></span> <span data-ttu-id="304ea-131">有幾個 WebMatrix 的網站範本包含 jQuery 程式庫中的他們*指令碼*資料夾。</span><span class="sxs-lookup"><span data-stu-id="304ea-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="304ea-132">如果您沒有這些程式庫，您可以下載最新的 jQuery 程式庫，直接從[jQuery.org](http://jQuery.org)站台。</span><span class="sxs-lookup"><span data-stu-id="304ea-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="304ea-133">或者您可以建立新的站台使用範本 (例如**入門網站**範本)，然後從該網站的 jQuery 檔案複製到您目前的站台。</span><span class="sxs-lookup"><span data-stu-id="304ea-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="304ea-134">最後，如果您想要使用 Bing 地圖服務，您必須先建立 （免費） 帳戶並取得金鑰。</span><span class="sxs-lookup"><span data-stu-id="304ea-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="304ea-135">若要取得索引鍵，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="304ea-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="304ea-136">在建立帳戶[Bing 地圖服務開發人員帳戶](https://www.microsoft.com/maps/developers/web.aspx)。</span><span class="sxs-lookup"><span data-stu-id="304ea-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="304ea-137">您必須具有 Microsoft 帳戶 (Windows Live ID)。</span><span class="sxs-lookup"><span data-stu-id="304ea-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="304ea-138">您可以指定您想要使用的索引鍵**評估/測試**。</span><span class="sxs-lookup"><span data-stu-id="304ea-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="304ea-139">如果您要測試對應函式，在您自己使用 WebMatrix 和 IIS Express 的電腦上，請移**站台**工作區並記下您網站的 URL (例如`http://localhost:50408`，不過，當您的連接埠號碼可能會不同)。</span><span class="sxs-lookup"><span data-stu-id="304ea-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="304ea-140">您可以使用此*localhost*為當您註冊的站台的位址。</span><span class="sxs-lookup"><span data-stu-id="304ea-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="304ea-141">您已註冊的帳戶後，請移至 Bing 地圖服務帳戶中心，然後按一下**建立或檢視表的索引鍵**:</span><span class="sxs-lookup"><span data-stu-id="304ea-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![對應-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="304ea-143">記錄 Bing 所建立的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="304ea-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="304ea-144">建立在地圖上的位址 （使用 Google）</span><span class="sxs-lookup"><span data-stu-id="304ea-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="304ea-145">下列範例示範如何建立可呈現在地圖上的地址的頁面。</span><span class="sxs-lookup"><span data-stu-id="304ea-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="304ea-146">此範例示範如何使用 Google Maps。</span><span class="sxs-lookup"><span data-stu-id="304ea-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="304ea-147">建立名為*MapAddress.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="304ea-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="304ea-148">此頁面將會產生對應，根據您傳遞給它的位址。</span><span class="sxs-lookup"><span data-stu-id="304ea-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="304ea-149">將下列程式碼複製到檔案中，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="304ea-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="304ea-150">請注意頁面上的下列功能：</span><span class="sxs-lookup"><span data-stu-id="304ea-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="304ea-151">`<script>`中的項目`<head>`項目。</span><span class="sxs-lookup"><span data-stu-id="304ea-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="304ea-152">在範例中，`<script>`項目參考*jquery 1.6.4.min.js*檔案，這是 jQuery 程式庫版本 1.6.4 縮短 （壓縮） 版本。</span><span class="sxs-lookup"><span data-stu-id="304ea-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="304ea-153">請注意，參考假設 *.js*檔案位於*指令碼*您的站台的資料夾。</span><span class="sxs-lookup"><span data-stu-id="304ea-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="304ea-154">如果您使用不同版本的 jQuery 程式庫，只要確定，您指向該版本正確。</span><span class="sxs-lookup"><span data-stu-id="304ea-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="304ea-155">若要呼叫`@Maps.GetGoogleHtml`中頁面的主體。</span><span class="sxs-lookup"><span data-stu-id="304ea-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="304ea-156">若要對應的位址，您必須傳遞地址字串。</span><span class="sxs-lookup"><span data-stu-id="304ea-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="304ea-157">類似的方式運作的其他對應引擎的方法 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。</span><span class="sxs-lookup"><span data-stu-id="304ea-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="304ea-158">執行頁面並輸入位址。</span><span class="sxs-lookup"><span data-stu-id="304ea-158">Run the page and enter an address.</span></span> <span data-ttu-id="304ea-159">此頁面會顯示對應中，根據 Google Maps，顯示您所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="304ea-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![對應-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="304ea-161">建立對應，根據緯度和經度座標 （使用 Bing）</span><span class="sxs-lookup"><span data-stu-id="304ea-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="304ea-162">此範例示範如何建立在地圖上座標。</span><span class="sxs-lookup"><span data-stu-id="304ea-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="304ea-163">此範例示範如何使用 Bing 地圖服務以及如何包含 Bing 金鑰。</span><span class="sxs-lookup"><span data-stu-id="304ea-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="304ea-164">（您可以建立對應，而不使用 Bing 金鑰以及使用其他的地圖引擎的座標為基礎）。</span><span class="sxs-lookup"><span data-stu-id="304ea-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="304ea-165">建立名為*MapCoordinates.cshtml*根目錄中的站台和取代現有的內容，使用下列程式碼和標記：</span><span class="sxs-lookup"><span data-stu-id="304ea-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="304ea-166">取代`your-key-here`與您稍早產生的 Bing 地圖服務金鑰。</span><span class="sxs-lookup"><span data-stu-id="304ea-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="304ea-167">執行*MapCoordinates.cshtml*頁面上，輸入緯度和經度座標，然後按一下**Map It ！**</span><span class="sxs-lookup"><span data-stu-id="304ea-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="304ea-168">按鈕。</span><span class="sxs-lookup"><span data-stu-id="304ea-168">button.</span></span> <span data-ttu-id="304ea-169">（如果您不知道任何座標，請嘗試下列方法。</span><span class="sxs-lookup"><span data-stu-id="304ea-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="304ea-170">這是 Microsoft Redmond 園區上的位置）。</span><span class="sxs-lookup"><span data-stu-id="304ea-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="304ea-171">緯度： 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="304ea-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="304ea-172">經度：-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="304ea-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="304ea-173">會顯示頁面，使用您指定的座標。</span><span class="sxs-lookup"><span data-stu-id="304ea-173">The page is displayed using the coordinates that you specified.</span></span>

     ![對應-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="304ea-175">其他資源</span><span class="sxs-lookup"><span data-stu-id="304ea-175">Additional Resources</span></span>


[<span data-ttu-id="304ea-176">Microsoft.Maps API 參考</span><span class="sxs-lookup"><span data-stu-id="304ea-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
