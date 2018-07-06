---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 第 4 部分： 模型和資料存取 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 023350e882afe049ce3800921825b1b2bec8e415
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818949"
---
<a name="part-4-models-and-data-access"></a>第 4 部分： 模型和資料存取
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。
> 
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 4 部分涵蓋模型和資料存取。


到目前為止，我們已只已傳遞 「 虛擬資料 」 從我們的控制站到我們的檢視範本。 現在我們已經準備好要連結實際的資料庫。 在本教學課程中我們要探討如何使用 SQL Server Compact Edition （通常稱為 SQL CE） 做為我們的資料庫引擎。 SQL CE 是免費的內嵌，檔案型的資料庫，並不需要任何安裝或組態，可以很方便的本機開發。

## <a name="database-access-with-entity-framework-code-first"></a>資料庫存取與 Entity Framework Code First

我們將使用隨附於 ASP.NET MVC 3 專案，以查詢和更新資料庫中的 Entity Framework (EF) 支援。 EF 是彈性物件關聯式對應 (ORM) 資料的 API，可讓開發人員以物件導向方式儲存在資料庫中的查詢及更新資料。

Entity Framework 第 4 版支援稱為 code first 開發架構。 Code first 可讓您撰寫簡單的類別 (也稱為 POCO 「 單純 」 的 CLR 物件)，來建立模型物件，並甚至可以建立您的類別從即時資料庫。

### <a name="changes-to-our-model-classes"></a>我們的模型類別變更

我們將在本教學課程利用 Entity Framework 中的資料庫建立功能。 這樣做之前，不過，讓我們進行了一些微幅變更給我們的模型類別，以增益集我們將在稍後使用的一些事項。

#### <a name="adding-the-artist-model-classes"></a>加入演出者模型類別

我們的專輯將會與演出者、 相關聯，因此我們將新增一個簡單的模型類別來描述演出者。 加入名為 Artist.cs 使用程式碼如下所示的 [模型] 資料夾中的新類別。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>更新我們的模型類別

更新專輯類別，如下所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

接下來，進行下列更新內容類型的類別。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>新增應用程式\_Data 資料夾

我們將新增應用程式\_至我們的專案，以保留 SQL Server Express 資料庫檔案的資料目錄。 應用程式\_資料是一個特殊的目錄，在 ASP.NET 中已經有資料庫存取權的正確的安全性存取權限。 從 專案 功能表中，選取 新增 ASP.NET 資料夾，然後應用程式\_資料。

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>在 web.config 檔案中建立的連接字串

我們將網站的組態檔加入幾行，好讓 Entity Framework 知道如何連線至我們的資料庫。 按兩下專案的根目錄中找到 Web.config 檔案。

![](mvc-music-store-part-4/_static/image2.png)

捲動至底部，此檔案，並新增&lt;connectionStrings&gt;區段正上方，而最後一行，如下所示。

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>新增的內容類別

以滑鼠右鍵按一下 [模型] 資料夾，並加入新的類別，名為 MusicStoreEntities.cs。

![](mvc-music-store-part-4/_static/image3.png)

這個類別會表示 Entity Framework 資料庫內容中，和會處理我們建立、 讀取、 更新和刪除作業，讓我們。 此類別的程式碼如下所示。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

就這麼簡單-沒有任何其他組態、 特殊的介面等。透過擴充 DbContext 基底類別，我們 MusicStoreEntities 類別是能夠處理我們對我們的資料庫作業。 現在，我們有連結，讓我們新增更多屬性給我們的模型類別，以利用我們的資料庫中的一些額外的資訊。

### <a name="adding-our-store-catalog-data"></a>加入我們的儲存目錄資料

我們會利用一項功能在 Entity Framework 將 「 種子 」 資料加入至新建立的資料庫。 這會預先填入我們的存放區目錄的內容類型、 演出者，與專輯清單。 -這包含我們稍早在本教學課程中使用的站台設計檔案-MvcMusicStore Assets.zip 下載具有與這個種子資料，位於名為程式碼的資料夾中的類別檔案。

在程式碼 / Models 資料夾中，找出 SampleData.cs 檔案，並將它放在專案中，於 Models 資料夾，如下所示。

![](mvc-music-store-part-4/_static/image4.png)

現在，我們需要新增一行告訴該 SampleData 類別中的 Entity Framework 的程式碼。 按兩下要開啟它，然後加入下面這行至頂端應用程式的專案根目錄中的 Global.asax 檔案\_Start 方法。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

到目前為止，我們已經完成我們的專案設定 Entity Framework 所需的工作。

## <a name="querying-the-database"></a>查詢資料庫

現在讓我們更新我們 StoreController，使而不是使用 「 虛擬資料 」 會改為呼叫來查詢其資訊的所有資料庫。 我們一開始先宣告上的欄位**StoreController**來保存 MusicStoreEntities 類別，名為 storeDB 的執行個體：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>更新存放區索引來查詢資料庫

MusicStoreEntities 類別由 Entity Framework 所維護，並公開我們的資料庫中每個資料表的集合屬性。 讓我們更新我們 StoreController 索引動作，以擷取資料庫中的所有內容類型。 先前我們採用的方法來硬式編碼的字串資料。 現在我們可以改為只使用 Entity Framework 內容 Generes 集合：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

不需要剛好我們檢視範本，因為我們仍然傳回相同的 StoreIndexViewModel 之前-我們只傳回即時資料從資料庫現在，我們會傳回任何變更。

當我們再次執行專案，並瀏覽"/ 儲存"URL 時，我們現在會在資料庫中看到所有內容類型的清單：

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>更新存放區瀏覽和詳細資料來使用即時資料

使用存放區/瀏覽？ 內容類型 =*[部分內容類型]* 動作方法中，我們要依名稱搜尋的內容類型。 我們只預期結果，由於我們以往不應該有兩個項目相同的內容類型名稱，因此我們可以使用。在 LINQ 查詢適當的內容類型物件，就像這樣的 single （） 延伸模組 （不輸入這尚未）：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

單一的方法是採用 Lambda 運算式做為參數，指定我們想要將單一的內容類型物件，其名稱符合已定義的值。 在上述案例中，我們載入單一內容類型的物件名稱值相符的 Disco。

我們將利用 Entity Framework 功能，可讓我們指出我們想要在擷取的內容類型的物件時所一併載入其他相關的實體。 這項功能稱為查詢結果成形，並可讓我們縮短的次數，我們需要存取資料庫，以擷取所有我們所需的資訊。 我們想要預先提取我們擷取時，內容類型的專輯，因此我們將會更新查詢，以便包含從 Genres.Include("Albums") 表示我們想要將相關的相簿。 這是更有效率，，因為它會擷取我們內容類型和專輯資料都在單一資料庫的要求中。

簡潔的說明，我們已更新的瀏覽控制器動作外觀如下：

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

我們現在可以更新存放區瀏覽 檢視來顯示每個內容類型中，您可以使用哪些相簿。 開啟檢視範本 (在中找到 /Views/Store/Browse.cshtml) 並新增專輯的項目符號清單，如下所示。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

執行我們的應用程式，並瀏覽至存放區/瀏覽？ 內容類型 = Jazz 顯示我們的結果現在正在從資料庫中，我們選取的內容類型中顯示所有的專輯提取。

![](mvc-music-store-part-4/_static/image2.jpg)

我們要讓相同的變更為我們 /Store/詳細資料 / [id] URL，和我們的虛擬資料取代成資料庫查詢以載入的 Album 的 ID 符合參數的值。

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

執行我們的應用程式，並瀏覽至 /Store/Details/1 示範，我們的結果現在正在從資料庫提取。

![](mvc-music-store-part-4/_static/image5.png)

現在，我們的存放區詳細資料頁面設定 album 顯示專輯識別碼，讓我們更新**瀏覽**連結到詳細資料檢視的檢視。 完全一樣從存放區索引存放區瀏覽連結結尾的上一節，我們將使用 Html.ActionLink。 瀏覽 檢視的完整來源會出現下方。

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

現在我們就可以從我們的存放區頁面瀏覽到內容類型 頁面，列出可用的專輯和專輯上即可，我們可以檢視該專輯的詳細資料。

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-3.md)
> [下一頁](mvc-music-store-part-5.md)
