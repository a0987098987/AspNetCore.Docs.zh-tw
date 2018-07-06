---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: URL 路由 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 968517100275822071e2101a4cfb978320f222f2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822712"
---
<a name="url-routing"></a>URL 路由
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


在本教學課程中，您將修改 「 Wingtip Toys 範例應用程式支援 URL 路由。 路由可讓您的 web 應用程式，以使用好記，請記住，變得更容易而且更支援的搜尋引擎的 Url。 此教學課程的上一個教學課程 [成員資格和系統管理]，並且是 Wingtip Toys 教學課程系列的一部分。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何註冊 ASP.NET Web Forms 應用程式的路由。
- 如何將路由新增至網頁。
- 如何從某個資料庫支援路由選取資料。

## <a name="aspnet-routing-overview"></a>ASP.NET 路由概觀

URL 路由可讓您設定應用程式以接受要求不會對應至實體檔案的 Url。 要求 URL 會是只要將使用者輸入他們的瀏覽器，在您的網站上尋找頁面的 URL。 您使用路由來定義使用者具有語意意義，而且可以幫助搜尋引擎最佳化 (SEO) 的 Url。

根據預設，Web Form 範本包含[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)。 大部分基本的路由工作將會透過實作*Friendly URLs*。 不過，在本教學課程中，您將加入自訂的路由功能。

在自訂之前 URL 路由，Wingtip Toys 範例應用程式可以連結至產品，使用下列 URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

藉由自訂 URL 路由，Wingtip Toys 範例應用程式將會連結至相同的產品使用容易閱讀的 URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>路由

路由是對應至處理常式的 URL 模式。 處理常式可以是實體的檔案，例如.aspx 檔案中的 Web Form 應用程式。 處理常式也可以處理要求的類別。 若要定義的路由，您可以建立路由類別的執行個體藉由指定的 URL 模式、 處理常式中，並選擇性地路由的名稱。

將路由新增至應用程式的加`Route`靜態物件`Routes`屬性`RouteTable`類別。 路由屬性是`RouteCollection`儲存應用程式的所有路由的物件。

### <a name="url-patterns"></a>URL 模式

URL 模式可以包含常值和變數預留位置 （稱為 URL 參數）。 常值和預留位置位於的 URL，這以斜線分隔的區段 (`/`) 字元。

您的 web 應用程式的要求進行時，URL 會剖析為區段和預留位置，和變數的值可供要求處理常式。 此程序是類似的方式剖析查詢字串中的資料並將其傳遞至要求處理常式。 在這兩種情況下，會在 URL 中包含變數的資訊，並將其傳遞給索引鍵 / 值組的形式中的處理常式中。 查詢字串的索引鍵和值是在 URL 中。 對路由索引鍵的 URL 模式中定義的預留位置名稱而只會在 URL 中。

在 URL 模式中，您必須定義的預留位置中括號括住 (`{`和`}`)。 您可以定義一個以上的預留位置，在區段中，但必須以常值分隔的預留位置。 比方說，`{language}-{country}/{action}`是有效的路由的模式。 不過，`{language}{country}/{action}`不是有效的模式，因為沒有任何常值或預留位置之間的分隔符號。 因此，路由不能判斷如何區隔值語言預留位置值的國家/地區的預留位置。

### <a name="mapping-and-registering-routes"></a>對應和註冊的路由

您可以包含在 Wingtip Toys 範例應用程式頁面的路由之前，您必須在應用程式啟動時註冊的路由。 若要註冊的路由，您將修改`Application_Start`事件處理常式。

1. 在 **方案總管**的 Visual Studio 中，尋找並開啟*Global.asax.cs*檔案。
2. 加入程式碼，以黃色反白顯示*Global.asax.cs*檔案，如下所示：   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wingtip Toys 範例應用程式會啟動，它會呼叫`Application_Start`事件處理常式。 這個事件處理常式中，結尾`RegisterCustomRoutes`呼叫方法。 `RegisterCustomRoutes`方法會將每個路由，藉由呼叫`MapPageRoute`方法`RouteCollection`物件。 使用路由名稱的路由 URL 和實體的 URL 進行定義的路由。

第一個參數 ("`ProductsByCategoryRoute`」) 是路由名稱。 它用來在需要時呼叫的路由。 第二個參數 ("`Category/{categoryName}`」) 會定義好記的取代程式碼為基礎的可以是動態的 URL。 您會填入資料控制項產生資料為基礎的連結時，您可以使用此路由。 路由會顯示，如下所示：

[!code-csharp[Main](url-routing/samples/sample2.cs)]

路由的第二個參數中包含大括號中指定的動態值 (`{ }`)。 在此情況下，`categoryName`是一個變數，將用來判斷適當的路由路徑。

> [!NOTE] 
> 
> **Optional**
> 
> 您可能會發現它更輕鬆地管理您的程式碼，藉由移動`RegisterCustomRoutes`的不同類別的方法。 在 *邏輯*資料夾中，建立個別`RouteActions`類別。 移動上述`RegisterCustomRoutes`方法，從*Global.asax.cs*到新的檔案`RoutesActions`類別。 使用`RoleActions`類別和`createAdmin`為了舉例說明如何呼叫的方法`RegisterCustomRoutes`方法，從*Global.asax.cs*檔案。


您也可能會發現`RegisterRoutes`方法呼叫使用`RouteConfig`開頭的物件`Application_Start`事件處理常式。 進行此呼叫，以實作預設路由。 當您建立使用 Visual Studio 的 Web Form 範本的應用程式時，它是隨附預設的程式碼。

## <a name="retrieving-and-using-route-data"></a>擷取並使用路線資料

如先前所述，您可以定義的路由。 您加入的程式碼`Application_Start`中的事件處理常式*Global.asax.cs*檔案載入的可定義的路由。

### <a name="setting-routes"></a>設定路由

路由需要您將新增額外的程式碼。 在本教學課程中，您將使用模型繫結來擷取`RouteValueDictionary`產生使用的資料控制項資料的路由時，會使用的物件。 `RouteValueDictionary`物件會包含屬於特定類別的產品的產品名稱的清單。 根據資料和路由每項產品建立連結。

#### <a name="enable-routes-for-categories-and-products"></a>為分類和產品啟用的路由

接下來，您將在其中更新應用程式使用`ProductsByCategoryRoute`來判斷正確的路由，以包含每個產品類別目錄連結。 您也會更新*ProductList.aspx*加入每個產品的路由的連結的頁面。 連結會顯示之前變更，但連結現在會使用 URL 路由。

1. 在 **方案總管**，開啟*Site.Master*頁面上，如果它尚未開啟。
2. 更新**ListView**控制項，名為"`categoryList`」 以黃色反白顯示變更，因此標記看起來像這樣：   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. 在 **方案總管**，開啟*ProductList.aspx*頁面。
4. 更新`ItemTemplate`項目*ProductList.aspx*頁面更新黃色反白顯示，讓標記看起來像這樣：   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. 開啟的程式碼後置*ProductList.aspx.cs*並新增下列命名空間為中反白顯示黃色：  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. 取代`GetProducts`方法的程式碼後置 (*ProductList.aspx.cs*) 為下列程式碼：   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>將程式碼新增的產品詳細資料

現在，更新程式碼後置 (*ProductDetails.aspx.cs*) 的*ProductDetails.aspx*頁面，即可使用路由資料。 請注意，新`GetProduct`方法也可接受的情況下使用者具有設為書籤的連結，會使用較舊的非友善、 非路由 URL 的查詢字串值。

1. 取代`GetProduct`方法的程式碼後置 (*ProductDetails.aspx.cs*) 為下列程式碼：   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在若要查看更新的路由。

1. 按下**F5**執行 Wingtip Toys 範例應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 按一下 **產品**在頁面頂端的連結。  
 顯示所有產品*ProductList.aspx*頁面。 針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：  
    `https://localhost:44300/ProductList`
3. 接下來，按一下**汽車**靠近頁面頂端的 類別目錄連結。  
 只有汽車會顯示在*ProductList.aspx*頁面。 針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：  
    `https://localhost:44300/Category/Cars`
4. 按一下頁面列出包含名稱的第一輛車的連結 ("**可轉換汽車**") 來顯示產品詳細資料。  
 針對瀏覽器顯示下列 URL （使用您的連接埠號碼）：  
    `https://localhost:44300/Product/Convertible%20Car`
5. 接下來，瀏覽器中輸入下列非路由 URL （使用您的連接埠號碼）：  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 程式碼仍會辨識包含查詢字串，使用者有設為書籤的連結的 URL。

## <a name="summary"></a>總結

在本教學課程中，您已新增為分類和產品的路由。 您已了解如何整合的路由，以使用模型繫結的資料控制項。 在下一個教學課程中，您將實作全域錯誤處理。

## <a name="additional-resources"></a>其他資源

[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [上一頁](membership-and-administration.md)
> [下一頁](aspnet-error-handling.md)
