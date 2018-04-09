---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 第 4 部分： 模型和資料存取 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a>第 4 部分： 模型和資料存取
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。
> 
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。


目前為止，我們已只已傳遞 「 虛擬資料 」 從我們的控制站我們檢視範本。 現在我們已經準備好實際資料庫相連結。 本教學課程中我們將會涵蓋如何使用 SQL Server Compact Edition （通常稱為 SQL CE） 做為我們的資料庫引擎。 SQL CE 是免費的內嵌，檔案型的資料庫，不需要任何安裝或組態，讓它的本機開發很方便。

## <a name="database-access-with-entity-framework-code-first"></a>資料庫存取權與 Entity Framework 程式碼優先 （contract-first）

我們會使用包含在 ASP.NET MVC 3 專案，以查詢和更新資料庫的 Entity Framework (EF) 支援。 EF 是彈性物件關聯式對應 (ORM) 資料的 API，可讓開發人員查詢及更新的資料儲存在資料庫中的物件導向的方式。

Entity Framework 第 4 版支援呼叫程式碼優先開發架構。 程式碼優先 （contract-first） 可讓您撰寫簡單的類別 (也稱為 POCO 從 「 純舊"CLR 物件)，來建立模型物件，甚至可以從您的類別上建立資料庫。

### <a name="changes-to-our-model-classes"></a>變更模型類別

我們將在本教學課程充分利用 Entity Framework 中的資料庫建立功能。 這樣做之前，不過，讓我們進行一些次要變更給我們的模型類別，以新增中我們將在稍後使用的一些事項。

#### <a name="adding-the-artist-model-classes"></a>加入演出者模型類別

我們專輯會演出者、 相關聯，所以我們會將簡單的模型類別來描述演出者。 新類別加入名為 Artist.cs 使用如下所示的程式碼的 [模型] 資料夾。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>更新模型類別

更新專輯類別，如下所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

接下來，進行下列更新類型類別。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>新增應用程式\_資料資料夾

我們會將應用程式新增\_專案來保存我們 SQL Server Express 資料庫檔案的資料目錄。 應用程式\_資料是在 ASP.NET 中已經有資料庫存取權的正確的安全性存取權限的特殊目錄。 從 專案 功能表中，選取 新增 ASP.NET 資料夾，然後應用程式\_資料。

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>在 web.config 檔案中建立的連接字串

如此 Entity Framework 可得知如何連接到我們的資料庫，我們將幾行加入至網站的組態檔。 按兩下專案的根目錄中找到 Web.config 檔案。

![](mvc-music-store-part-4/_static/image2.png)

捲動到這個檔案最下方，新增&lt;connectionStrings&gt;區段正上方的最後一行，如下所示。

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>加入內容類別

以滑鼠右鍵按一下 [模型] 資料夾，並加入新的類別，名為 MusicStoreEntities.cs。

![](mvc-music-store-part-4/_static/image3.png)

這個類別將代表 Entity Framework 資料庫內容中，和將處理我們建立、 讀取、 更新和刪除作業，讓我們。 這個類別的程式碼如下所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

就這麼簡單-沒有任何其他組態、 特殊的介面和其他內容。透過擴充 DbContext 基底類別，我們 MusicStoreEntities 類別是能夠處理為我們我們資料庫作業。 現在，我們有連結，讓我們加入一些其他屬性給我們的模型類別，以利用我們的資料庫中的一些其他資訊。

### <a name="adding-our-store-catalog-data"></a>加入我們的儲存目錄資料

我們會利用一項功能在 Entity Framework 將 「 種子 」 資料加入至新建立的資料庫。 這會預先填入和清單的內容類型、 演出者、 專輯我們存放區的目錄。 -其中包含我們稍早在本教學課程中的站台設計檔案-MvcMusicStore Assets.zip 下載有這種子資料，位於名為程式碼的資料夾中的類別檔案。

在程式碼 / Models 資料夾找到 SampleData.cs 檔案，並將其放置到我們的受測專案，[模型] 資料夾，如下所示。

![](mvc-music-store-part-4/_static/image4.png)

現在我們需要加入一行程式碼，以告訴 SampleData 該類的 Entity Framework。 按兩下加以開啟，並加入下列行至頂端應用程式專案的根目錄中的 Global.asax 檔案\_Start 方法。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

此時，我們已經完成設定 Entity Framework 的受測專案的必要工作。

## <a name="querying-the-database"></a>查詢資料庫

現在讓我們來更新我們 StoreController，使而不是使用 「 虛擬資料 」 會改為呼叫以查詢其資訊的所有資料庫。 藉由宣告上的欄位，就會開始**StoreController**來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>更新查詢資料庫存放區索引

MusicStoreEntities 類別由 Entity Framework 維護，並公開在資料庫中的每個資料表的集合屬性。 讓我們來更新我們 StoreController 索引動作，以擷取資料庫中的所有內容類型。 先前是由硬式編碼的字串資料。 現在我們可以改為只使用 Entity Framework 內容 Generes 集合：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

需要剛好我們檢視的範本，因為我們仍在傳回之前-我們正在只會傳回即時資料從資料庫現在，我們會傳回相同 StoreIndexViewModel 沒有變更。

當我們重新執行專案，並瀏覽"/ 存放區 」 的 URL 時，我們現在會看到資料庫中的所有內容類型清單：

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>更新存放區瀏覽和詳細資料使用即時資料

與儲存區/瀏覽？ 內容類型 =*[某些內容類型]*動作方法，我們要依名稱搜尋的內容類型。 我們只預期有一個結果，因為我們永遠不應該有相同的內容類型名稱的兩個項目，因此我們可以使用。在 LINQ 查詢以取得適當的內容類型物件，像這樣的 Single() 延伸模組 （但不輸入）：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

單一方法會使用 Lambda 運算式做為參數，指定我們想要將單一類型物件，例如其名稱符合，我們已經定義的值。 在上述情況中，我們正在載入的單一類型的物件名稱值相符的 Disco。

我們將利用 Entity Framework 功能，可讓我們來表示我們想要在擷取的內容類型的物件時所載入以及其他相關的實體。 這項功能稱為查詢結果成形，並可讓我們來減少的次數，我們需要存取資料庫，以擷取所有需要的資訊。 我們想要預先提取專輯我們擷取時，內容類型，因此我們會將更新我們的查詢來包含從 Genres.Include("Albums") 表示我們想要將相關的相簿。 這是更有效率，，因為它會擷取我們內容類型和專輯資料都在單一資料庫的要求中。

開清楚，我們已更新的瀏覽控制器動作外觀如下：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

我們現在可以更新存放區瀏覽檢視來顯示每個內容類型中可用的專輯。 開啟檢視範本 (在中找到 /Views/Store/Browse.cshtml) 並加入項目清單的相簿，如下所示。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

執行我們的應用程式，並瀏覽至存放區/瀏覽？ 內容類型 = Jazz 顯示我們的結果現在會從資料庫中，我們選取內容類型顯示所有的專輯提取。

![](mvc-music-store-part-4/_static/image2.jpg)

我們要變更至我們 /Store/詳細資料 / [id] URL，和我們空的資料取代資料庫查詢而載入的相簿的 ID 符合參數的值相同。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

執行我們的應用程式，並瀏覽至 /Store/Details/1 顯示的現在從資料庫提取我們的結果。

![](mvc-music-store-part-4/_static/image5.png)

既然我們存放區詳細資料頁面設定來顯示專輯識別碼相簿，讓我們更新**瀏覽**連結到詳細資料檢視的檢視。 我們將使用 Html.ActionLink，如同我們從存放區索引儲存區瀏覽連結結尾的上一節。 下方瀏覽檢視完整的來源會出現。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

我們現在可以從我們的存放區頁面瀏覽到內容類型 頁面，列出可用的專輯和專輯上按一下我們可以檢視該專輯的詳細資料。

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-3.md)
> [下一頁](mvc-music-store-part-5.md)
