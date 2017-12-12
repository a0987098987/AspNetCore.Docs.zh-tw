---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: "URL 路由 |Microsoft 文件"
author: Erikre
description: "此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 279617e4ebb475d935c0d1e01e08a3a2def0f9e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="url-routing"></a>URL 路由
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


在本教學課程中，您將修改 Wingtip Toys 範例應用程式支援 URL 路由。 路由可讓您的 web 應用程式使用好記，請記住，變得更容易而且更支援的搜尋引擎的 Url。 本教學課程是根據上一個教學課程 [成員資格和系統管理]，並是 Wingtip Toys 教學課程系列的一部分。

## <a name="what-youll-learn"></a>您將學習：

- 如何註冊 ASP.NET Web Form 應用程式的路由。
- 如何將路由新增至網頁。
- 如何從某個資料庫支援路由選取資料。

## <a name="aspnet-routing-overview"></a>ASP.NET 路由概觀

URL 路由可讓您設定應用程式以接受要求不會對應到實體檔案的 Url。 要求 URL 是只要將使用者輸入他們的瀏覽器，在網站上尋找頁面的 URL。 若要定義的 Url，在語意上意義給使用者，且搜尋引擎最佳化 (SEO) 可協助使用路由。

根據預設，Web Form 範本包括[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)。 大部分的基本的路由工作將會透過使用實作*Friendly URLs*。 不過，在本教學課程，您會加入自訂路由的功能。

在自訂之前 URL 路由，Wingtip Toys 範例應用程式可以連結至產品，使用下列 URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

藉由自訂 URL 路由，Wingtip Toys 範例應用程式將會連結至相同的產品使用方便讀取 URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>路由

路由是對應至處理常式的 URL 模式。 此處理常式可以是實體檔案，例如 Web Form 應用程式中的.aspx 檔案。 處理常式也可以是處理要求的類別。 若要定義路由，您可以建立路由類別的執行個體所指定的 URL 模式、 處理常式，以及選擇性地路由的名稱。

將路由加入至應用程式的加`Route`靜態物件`Routes`屬性`RouteTable`類別。 路由屬性是`RouteCollection`儲存應用程式的所有路由的物件。

### <a name="url-patterns"></a>URL 模式

URL 模式可以包含常值和變數的預留位置 （稱為 URL 參數）。 常值和預留位置位於其以斜線分隔 URL 區段 (`/`) 字元。

您 web 應用程式進行要求時，URL 會剖析為區段和預留位置，和變數的值提供給要求處理常式。 此程序是在剖析查詢字串中的資料和傳遞給要求處理常式的方式類似。 在這兩種情況下，為 URL 中包含變數的資訊，並將其傳遞給索引鍵-值組表單中的處理常式中。 查詢字串索引鍵和值是在 URL 中。 路由的索引鍵中的 URL 模式中，定義的預留位置名稱，而只有值在 URL 中。

在 URL 模式中，您必須定義預留位置放在大括號 (`{`和`}`)。 您可以定義一個以上的預留位置的區段中，但必須以常值分隔預留位置。 例如，`{language}-{country}/{action}`是有效的路由模式。 不過，`{language}{country}/{action}`不是有效的模式，因為沒有任何常值或預留位置之間的分隔符號。 因此，路由無法判斷分開值語言預留位置值的國家/地區預留位置的位置。

### <a name="mapping-and-registering-routes"></a>對應和註冊路由

您可以包含路由，以便 Wingtip Toys 範例應用程式的頁面之前，您必須在應用程式啟動時註冊路由。 若要註冊的路由，您將修改`Application_Start`事件處理常式。

1. 在**方案總管 中**的 Visual Studio 中，尋找並開啟*Global.asax.cs*檔案。
2. 加入程式碼中以黃色反白顯示*Global.asax.cs*檔案，如下所示：   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

當 Wingtip Toys 範例應用程式啟動時，它會呼叫`Application_Start`事件處理常式。 這個事件處理常式中，結尾`RegisterCustomRoutes`方法呼叫。 `RegisterCustomRoutes`方法呼叫會將每個路由`MapPageRoute`方法`RouteCollection`物件。 路由會定義使用路由名稱，路由 URL 及實體的 URL。

第一個參數 ("`ProductsByCategoryRoute`") 的路由名稱。 它用來在需要時呼叫的路由。 第二個參數 ("`Category/{categoryName}`」) 會定義可以是動態的 URL 為基礎程式碼的好記取代。 會擴展資料控制項產生根據資料的連結時，您可以使用此路由。 路由會顯示，如下所示：

[!code-csharp[Main](url-routing/samples/sample2.cs)]

第二個參數的路由包含大括號所指定的動態值 (`{ }`)。 在此情況下，`categoryName`是將用來判斷適當的路由路徑的變數。

> [!NOTE] 
> 
> **Optional**
> 
> 您可能會發現它更輕鬆地管理您的程式碼移動`RegisterCustomRoutes`的不同類別的方法。 在*邏輯*資料夾中，建立個別`RouteActions`類別。 移動上述`RegisterCustomRoutes`方法從*Global.asax.cs*檔案到新`RoutesActions`類別。 使用`RoleActions`類別和`createAdmin`方法做為範例如何呼叫`RegisterCustomRoutes`方法從*Global.asax.cs*檔案。


您也可能已經發現`RegisterRoutes`方法呼叫使用`RouteConfig`物件的開頭`Application_Start`事件處理常式。 進行此呼叫，以實作預設路由。 當您建立使用 Visual Studio Web 表單範本的應用程式時，它是作為預設程式碼。

## <a name="retrieving-and-using-route-data"></a>擷取和使用資料路由

如上面所述，您可以定義路由。 您加入的程式碼`Application_Start`中的事件處理常式*Global.asax.cs*檔案載入可定義的路由。

### <a name="setting-routes"></a>設定路由

路由需要您將新增額外的程式碼。 在此教學課程中，您將使用模型繫結來擷取`RouteValueDictionary`產生的路由使用資料控制項中的資料時所使用的物件。 `RouteValueDictionary`物件會包含屬於特定類別的產品的產品名稱的清單。 根據資料和路由每項產品會建立連結。

#### <a name="enable-routes-for-categories-and-products"></a>分類和產品啟用的路由

接下來，您要更新應用程式使用`ProductsByCategoryRoute`來判斷要為每個產品類別目錄連結包含正確的路由。 您還會更新*ProductList.aspx*加入每個產品的路由的連結的頁面。 連結將會顯示之前變更，但是連結現在會使用 URL 路由。

1. 在**方案總管 中**，開啟*Site.Master*頁面上，如果它尚未開啟。
2. 更新**ListView**控制項，名為"`categoryList`」 以黃色反白顯示的變更，所以標記會出現，如下所示：   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. 在**方案總管 中**，開啟*ProductList.aspx*頁面。
4. 更新`ItemTemplate`元素*ProductList.aspx*頁面更新中反白顯示黃色，讓標記出現，如下所示：   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. 開啟 程式碼後置的*ProductList.aspx.cs*並加入下列命名空間為中反白黃色：  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. 取代`GetProducts`方法的程式碼後置 (*ProductList.aspx.cs*) 取代下列程式碼：   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>加入程式碼的產品詳細資料

現在，更新程式碼後置 (*ProductDetails.aspx.cs*) 的*ProductDetails.aspx*頁面，即可使用路由資料。 請注意，新`GetProduct`方法也可接受的情況在使用者具有設為書籤的連結，會使用較舊的非易記、 非路由 URL 的查詢字串值。

1. 取代`GetProduct`方法的程式碼後置 (*ProductDetails.aspx.cs*) 取代下列程式碼：   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>執行應用程式

您可以執行應用程式現在以查看更新的路由。

1. 按**F5**執行 Wingtip Toys 範例應用程式。  
 瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 按一下**產品**在頁面頂端的連結。  
 所有產品會都顯示在*ProductList.aspx*頁面。 瀏覽器會顯示下列 URL （使用連接埠號碼）：  
    `https://localhost:44300/ProductList`
3. 接下來，按一下**汽車**靠近頁面頂端的類別目錄連結。  
 只有汽車會顯示在*ProductList.aspx*頁面。 瀏覽器會顯示下列 URL （使用連接埠號碼）：  
    `https://localhost:44300/Category/Cars`
4. 按一下頁面列出的連結，包含名稱的第一個汽車 ("**可轉換汽車**") 以顯示產品詳細資料。  
 瀏覽器會顯示下列 URL （使用連接埠號碼）：  
    `https://localhost:44300/Product/Convertible%20Car`
5. 接下來，瀏覽器中輸入下列非路由 URL （使用連接埠號碼）：  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 程式碼仍會辨識包含查詢字串，使用者有標示為書籤的連結的 URL。

## <a name="summary"></a>總結

在本教學課程中，您已加入為分類和產品的路由。 您已經學會如何整合路由，與使用模型繫結的資料控制項。 在下一個教學課程中，您將實作通用的錯誤處理。

## <a name="additional-resources"></a>其他資源

[ASP.NET 易記 Url](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[將具有成員資格、 OAuth、 和 SQL Database 的安全的 ASP.NET Web Form 應用程式部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[上一頁](membership-and-administration.md)
[下一頁](aspnet-error-handling.md)
