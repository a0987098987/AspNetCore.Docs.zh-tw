---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: "建立模型類別搭配 LINQ to SQL (VB) |Microsoft 文件"
author: microsoft
description: "本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型 c..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 972d5b11049825e84e070ef1c4b2b90116654397
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>使用 LINQ to SQL (VB) 建立模型類別
====================
由[Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> 本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權。


本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權。

在本教學課程中，我們會建置基本的電影資料庫應用程式。 我們先建立電影資料庫應用程式中的最快速且最簡單的方式可能。 我們會執行所有存取我們的資料直接從我們控制器的動作。

接下來，您會了解如何使用儲存機制模式。 使用儲存機制模式將需要更多的工作。 不過，採用這個模式的優點是它可讓您建置應用程式，則具有可調適性，若要變更，並可以輕鬆地測試。

## <a name="what-is-a-model-class"></a>模型類別為何？

MVC 模型包含的所有未包含在 MVC 檢視或 MVC 控制器中的應用程式邏輯。 特別是，MVC 模型包含的所有應用程式商務和資料存取邏輯。

您可以使用各種不同的技術來實作資料存取邏輯。 例如，您可以建立資料存取類別使用 Microsoft Entity Framework、 NHibernate、 Subsonic 或 ADO.NET 類別。

在本教學課程中，我可以使用 LINQ to SQL 查詢，並更新資料庫。 LINQ to SQL 提供非常簡單的方法，與 Microsoft SQL Server 資料庫互動。 不過，務必了解，ASP.NET MVC 架構未繫結至 LINQ to SQL 以任何方式。 ASP.NET MVC 是與任何資料存取技術相容。

## <a name="create-a-movie-database"></a>建立電影資料庫

在此教學課程--以說明如何建置模型類別-我們建置簡單的電影資料庫應用程式。 第一個步驟是建立新的資料庫。 以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目**。 選取的 SQL Server 資料庫範本，讓它名稱 MoviesDB.mdf，再按一下**新增**按鈕 （請參閱圖 1）。


[![加入新的 SQL Server 資料庫](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**圖 01**： 加入新的 SQL Server 資料庫 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


建立新的資料庫之後，您可以開啟資料庫應用程式中按兩下 MoviesDB.mdf\_Data 資料夾。 按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗 （請參閱圖 2）。

|  | [伺服器總管] 視窗使用 Visual Web Developer 中時呼叫 [資料庫總管] 視窗。 |
| --- | --- |


[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**圖 02**： 使用 [伺服器總管] 視窗 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


我們需要將一個資料表加入至資料庫，表示我們電影。 以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。 選取此功能表選項會開啟 資料表設計工具 （請參閱圖 3）。


[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**圖 03**： 資料表設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


我們需要將下列資料行加入至資料庫資料表：

| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(200) | False |
| 導向器 | Nvarchar （50) | False |

您需要執行兩個特殊的動作識別碼資料行。 首先，您必須將標示為主要索引鍵資料行的識別碼資料行，資料表設計工具中選取資料行，然後按一下 索引鍵的圖示。 LINQ to SQL 會要求您執行插入或更新資料庫時，指定主要的索引鍵資料行。

接下來，您必須將標示為識別資料行的識別碼資料行所指定的值給**為識別**屬性 （請參閱圖 3）。 識別資料行是指派新的數字自動每當您將新的資料列加入資料表的資料行。

進行這些變更之後，請使用名稱 tblMovie 儲存資料表。 您可以依序按一下 [儲存] 按鈕儲存資料表。

## <a name="create-linq-to-sql-classes"></a>建立 LINQ to SQL 類別

我們的 MVC 模型會包含 LINQ to SQL 類別代表 tblMovie 資料庫資料表。 若要建立這些 LINQ to SQL 類別最簡單方式是以滑鼠右鍵按一下 模型 資料夾中，選取**新增、 新項目**，選取 LINQ to SQL 類別範本，將類別命名 Movie.dbml，然後按一下**新增**按鈕 （請參閱圖 4）。


[![建立 LINQ to SQL 類別](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**圖 04**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


立即建立電影 LINQ to SQL 類別之後，便會顯示物件關聯式設計工具。 您可以從伺服器總管] 視窗拖曳至 [物件關聯式設計工具建立 LINQ to SQL 類別代表特定的資料庫資料表拖曳資料庫資料表。 我們需要加入 tblMovie 資料庫資料表拖曳至 物件關聯式設計工具 （請參閱圖 4）。


[![使用物件關聯式設計工具](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**圖 05**： 使用物件關聯式設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


根據預設，物件關聯式設計工具建立資料庫資料表拖曳至設計工具的非常與同名的類別。 不過，我們不想要呼叫我們類別 tblMovie。 因此，按一下 類別設計工具中的名稱，將類別的名稱變更為影片。

最後，請記得要按一下**儲存**按鈕 （磁片的圖片） 儲存 LINQ to SQL 類別。 否則，LINQ to SQL 類別不會產生的物件關聯式設計工具。

## <a name="using-linq-to-sql-in-a-controller-action"></a>使用 LINQ to SQL 中的控制器動作

現在，我們已經我們 LINQ to SQL 類別時，我們可以使用這些類別，從資料庫擷取資料。 在本節中，您可以了解如何使用 LINQ to SQL 類別，直接在控制器動作。 我們將 MVC 檢視中顯示 tblMovies 資料庫資料表中的電影的清單。

首先，我們需要修改 HomeController 類別。 這個類別可以找到您的應用程式的 [控制器] 資料夾中。 修改類別，讓它看起來像是列表 1 中的類別。

**列出 1 –`Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Index （） 中的動作清單 1 來表示 MoviesDB 資料庫使用 LINQ to SQL DataContext 類別 (MovieDataContext)。 MoveDataContext 類別是由 Visual Studio 物件關聯式設計工具產生。

LINQ 查詢會針對從 tblMovies 資料庫資料表擷取所有的電影 DataContext 執行。 電影清單會指派給名為電影的本機變數。 最後的電影清單會傳遞至檢視檢視資料。

若要顯示電影，我們接下來要修改索引檢視表。 您可以找到的索引檢視 Views\Home\ 資料夾中。 更新索引檢視，讓它看起來像是列表 2 中的檢視。

**列出 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

請注意，修改的索引檢視表包含&lt;%@ 匯入命名空間 %&gt;指示詞上方的檢視。 這個指示詞會匯入 MvcApplication1 命名空間。 我們需要此命名空間，才能使用模型類別，尤其是，在檢視中的影片類別。

列出 2 中的檢視包含 For each 逐一查看所有 ViewData.Model 屬性所代表的項目。 Title 屬性的值會顯示每個影片。

請注意 ViewData.Model 屬性的值會轉換成 IEnumerable。 這是必要的 ViewData.Model 內容執行迴圈。 這裡的另一個選項是建立強型別檢視。 建立強型別檢視時，您會轉換至特定類型的 ViewData.Model 屬性檢視的程式碼後置類別中。

如果您執行應用程式之後修改 HomeController 類別和索引檢視，然後就會出現空白頁面。 因為沒有影片記錄 tblMovies 資料庫資料表中，您會得到空白頁。

若要將記錄新增至 tblMovies 資料庫資料表，tblMovies 資料庫資料表，在 [伺服器總管] 視窗 （在 Visual Web Developer 的 [資料庫總管] 視窗） 中以滑鼠右鍵按一下並選取功能表選項**顯示資料表資料**。 您可以插入影片記錄所使用的方格，會出現 （請參閱圖 5）。


[![插入影片](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**圖 06**： 插入影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


您將某些資料庫記錄加入 tblMovies 資料表，並執行應用程式之後，您會看到圖 7 中的頁面。 所有的電影資料庫記錄會顯示在項目符號清單。


[![顯示與索引檢視的電影](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**圖 07**： 顯示與索引檢視的影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>使用儲存機制模式

在上一節中，我們會使用 LINQ to SQL 類別，直接在控制器動作。 我們使用 MovieDataContext 類別，直接從 index （） 控制器動作。 並沒有任何在簡單的應用程式的情況下執行這個問題。 不過，在控制器類別中的直接使用 LINQ to SQL 工作會產生問題時您需要建置更複雜的應用程式。

控制器類別中使用 LINQ to SQL 難以未來切換資料存取技術。 例如，您可能決定切換使用為您的資料存取技術的 Microsoft Entity Framework 使用 Microsoft LINQ to SQL。 在此情況下，您需要重寫存取的資料庫應用程式內的每個控制站。

使用 LINQ to SQL 中控制器類別也很難建置您的應用程式的單元測試。 一般來說，您不想執行單元測試時，與資料庫互動。 您想要使用您的單元測試來測試應用程式邏輯而非您資料庫伺服器。

若要建立的 MVC 應用程式更適應未來的變更和可以更輕鬆地測試，您應該考慮使用儲存機制模式。 當您使用儲存機制模式時，您會建立個別的儲存機制類別，其中包含所有您資料庫的存取邏輯。

當您建立的儲存機制類別時，您會建立介面，代表所有儲存機制類別所使用的方法。 中的控制站，您可以撰寫您的程式碼，而不是儲存機制介面。 這樣一來，您可以實作在未來使用不同的資料存取技術的儲存機制。

介面中列出的 3 為 IMovieRepository，它代表名為 ListAll() 的單一方法。

**列出 3 –`Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

儲存機制中的類別清單 4 實作 IMovieRepository 介面。 請注意，它會包含名為 ListAll() 對應至 IMovieRepository 介面所需的方法的方法。

**列出 4 –`Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

最後，在列出 5 MoviesController 類別會使用儲存機制模式。 它不會再使用 LINQ to SQL 類別直接。

**列出 5 –`Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

請注意在列出 5 MoviesController 類別有兩個建構函式。 應用程式執行時，會呼叫第一個建構函式，無參數建構函式。 這個建構函式建立 MovieRepository 類別的執行個體，並將其傳遞給第二個建構函式。

第二個建構函式具有單一參數： IMovieRepository 參數。 這個建構函式只會將參數的值指派給名為的類別層級欄位\_儲存機制。

MoviesController 類別利用一種軟體設計模式，呼叫相依性插入模式。 特別是，它使用呼叫建構函式相依性插入的項目。 閱讀更多有關閱讀下列文章依 Martin Fowler 此模式：

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

請注意，所有 MoviesController 類別 （除了第一個建構函式） 中的程式碼與 IMovieRepository 介面，而不是實際 MovieRepository 類別互動。 程式碼互動的抽象介面，而不是介面的具象實作。

如果您想要修改應用程式所使用的資料存取技術您可以直接實作的類別，會使用替代的資料庫存取技術的 IMovieRepository 介面。 例如，您可以建立 EntityFrameworkMovieRepository 類別或 SubSonicMovieRepository 類別。 控制器類別所撰寫的介面，因為您可以將 IMovieRepository 的新實作傳遞至控制器類別，類別會繼續運作。

此外，如果您想要測試 MoviesController 類別，則您可以傳遞假的電影儲存機制的類別來 MoviesController。 您可以實作 IMovieRepository 類別實際上不會存取資料庫，但是包含所有必要的 IMovieRepository 介面方法的類別。 這樣一來，進行單元測試 MoviesController 類別而不需要實際存取實際的資料庫。

## <a name="summary"></a>總結

本教學課程的目標是為了示範如何建立 MVC 模型類別，藉由運用 Microsoft LINQ to SQL。 我們會檢查兩種策略，在 ASP.NET MVC 應用程式中顯示資料庫的資料。 首先，我們建立 LINQ to SQL 類別，並使用直接內控制器動作的類別。 使用 LINQ to SQL 類別的控制器內，可讓您快速並輕鬆地在 MVC 應用程式中顯示資料庫的資料。

接下來，我們已探索的稍微更困難，但更明確地正向，顯示資料庫資料路徑。 我們已利用儲存機制模式，所有資料庫存取邏輯放在不同的儲存機制類別。 中我們控制站，我們會寫入所有我們的程式碼，根據介面而不是具象類別。 儲存機制模式的優點是它可讓我們來輕鬆地在未來變更資料庫存取技術，可讓我們來輕鬆地測試控制器類別。

>[!div class="step-by-step"]
[上一頁](creating-model-classes-with-the-entity-framework-vb.md)
[下一頁](displaying-a-table-of-database-data-vb.md)
