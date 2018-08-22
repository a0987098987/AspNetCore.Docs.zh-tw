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
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>在 ASP.NET Web Pages (Razor) 網站中顯示地圖
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何依據對應 Bing、 Google、 MapQuest 和 Yahoo 所提供的服務，ASP.NET Web Pages (Razor) 網站中的頁面上顯示互動式地圖。
> 
> 您將學到什麼：
> 
> - 如何產生在地圖上的位址。
> - 如何產生根據緯度和經度座標的對應。
> - 如何註冊 Bing 地圖服務開發人員帳戶，並取得使用與 Bing 地圖服務金鑰。
> 
> 這是發行項中導入的 ASP.NET 功能：
> 
> - `Maps`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教學課程也適用於 WebMatrix 3。


在網頁中，您可以顯示對應頁面上使用`Maps`協助程式。 您可以產生根據位址或一組經度和緯度座標的對應。 `Maps`類別可讓您呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 的熱門地圖引擎。

將對應加入至頁面的步驟都相同不論哪一個 map 引擎呼叫。 您只要新增 JavaScript 檔案參考，讓可用的方法，以顯示地圖上，然後再呼叫的方法`Maps`協助程式。

您可以選擇依據地圖服務`Maps`您所使用的 helper 方法。 您可以使用其中任何一項：

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>安裝您需要的片段

若要顯示地圖，您需要這些項目：

- `Maps`協助程式。 此協助程式是 ASP.NET Web Helpers Library 第 2 版中。 如果您尚未新增程式庫，您可以將它安裝在您的網站以 NuGet 套件。 如需詳細資訊，請參閱 <<c0> [ 安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)。 (在 資源庫中，搜尋`microsoft-web-helpers`封裝。)
- JQuery 程式庫。 有幾個 WebMatrix 的網站範本包含 jQuery 程式庫中的他們*指令碼*資料夾。 如果您沒有這些程式庫，您可以下載最新的 jQuery 程式庫，直接從[jQuery.org](http://jQuery.org)站台。 或者您可以建立新的站台使用範本 (例如**入門網站**範本)，然後從該網站的 jQuery 檔案複製到您目前的站台。

最後，如果您想要使用 Bing 地圖服務，您必須先建立 （免費） 帳戶並取得金鑰。 若要取得索引鍵，請遵循下列步驟：

1. 在建立帳戶[Bing 地圖服務開發人員帳戶](https://www.microsoft.com/maps/developers/web.aspx)。 您必須具有 Microsoft 帳戶 (Windows Live ID)。

    您可以指定您想要使用的索引鍵**評估/測試**。 如果您要測試對應函式，在您自己使用 WebMatrix 和 IIS Express 的電腦上，請移**站台**工作區並記下您網站的 URL (例如`http://localhost:50408`，不過，當您的連接埠號碼可能會不同)。 您可以使用此*localhost*為當您註冊的站台的位址。
2. 您已註冊的帳戶後，請移至 Bing 地圖服務帳戶中心，然後按一下**建立或檢視表的索引鍵**:

    ![對應-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. 記錄 Bing 所建立的索引鍵。

## <a name="creating-a-map-based-on-an-address-using-google"></a>建立在地圖上的位址 （使用 Google）

下列範例示範如何建立可呈現在地圖上的地址的頁面。 此範例示範如何使用 Google Maps。

1. 建立名為*MapAddress.cshtml*根目錄中的站台。 此頁面將會產生對應，根據您傳遞給它的位址。
2. 將下列程式碼複製到檔案中，覆寫現有的內容。

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    請注意頁面上的下列功能：

    - `<script>`中的項目`<head>`項目。 在範例中，`<script>`項目參考*jquery 1.6.4.min.js*檔案，這是 jQuery 程式庫版本 1.6.4 縮短 （壓縮） 版本。 請注意，參考假設 *.js*檔案位於*指令碼*您的站台的資料夾。 

        > [!NOTE]
        > 如果您使用不同版本的 jQuery 程式庫，只要確定，您指向該版本正確。
    - 若要呼叫`@Maps.GetGoogleHtml`中頁面的主體。 若要對應的位址，您必須傳遞地址字串。 類似的方式運作的其他對應引擎的方法 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。
3. 執行頁面並輸入位址。 此頁面會顯示對應中，根據 Google Maps，顯示您所指定的位置。

     ![對應-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>建立對應，根據緯度和經度座標 （使用 Bing）

此範例示範如何建立在地圖上座標。 此範例示範如何使用 Bing 地圖服務以及如何包含 Bing 金鑰。 （您可以建立對應，而不使用 Bing 金鑰以及使用其他的地圖引擎的座標為基礎）。

1. 建立名為*MapCoordinates.cshtml*根目錄中的站台和取代現有的內容，使用下列程式碼和標記：

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. 取代`your-key-here`與您稍早產生的 Bing 地圖服務金鑰。
3. 執行*MapCoordinates.cshtml*頁面上，輸入緯度和經度座標，然後按一下**Map It ！** 按鈕。 （如果您不知道任何座標，請嘗試下列方法。 這是 Microsoft Redmond 園區上的位置）。

   - 緯度： 47.6781005859375
   - 經度：-122.158317565918

     會顯示頁面，使用您指定的座標。

     ![對應-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[Microsoft.Maps API 參考](https://msdn.microsoft.com/library/gg427611.aspx)
