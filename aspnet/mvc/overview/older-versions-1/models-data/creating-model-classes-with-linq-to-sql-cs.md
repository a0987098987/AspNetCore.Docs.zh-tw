---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: 建立模型類別搭配 LINQ to SQL (C#) |Microsoft 文件
author: microsoft
description: 本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a56ceb9eab5774906ecc89ce9da570d4f691a82
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2018
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>建立模型類別搭配 LINQ to SQL (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> 本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程中，您可以了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權。


本教學課程的目標是以說明一個方法來建立 ASP.NET MVC 應用程式的模型類別。 在本教學課程，了解如何建置模型類別，並藉由運用 Microsoft LINQ to SQL 中執行資料庫存取權

在本教學課程中，我們會建置基本的電影資料庫應用程式。 我們先建立電影資料庫應用程式中的最快速且最簡單的方式可能。 我們會執行所有存取我們的資料直接從我們控制器的動作。

接下來，您會了解如何使用儲存機制模式。 使用儲存機制模式將需要更多的工作。 不過，採用這個模式的優點是它可讓您建置應用程式，則具有可調適性，若要變更，並可以輕鬆地測試。

## <a name="what-is-a-model-class"></a>模型類別為何？

MVC 模型包含的所有未包含在 MVC 檢視或 MVC 控制器中的應用程式邏輯。 特別是，MVC 模型包含的所有應用程式商務和資料存取邏輯。

您可以使用各種不同的技術來實作資料存取邏輯。 例如，您可以建立資料存取類別使用 Microsoft Entity Framework、 NHibernate、 Subsonic 或 ADO.NET 類別。

在本教學課程中，我可以使用 LINQ to SQL 查詢，並更新資料庫。 LINQ to SQL 提供非常簡單的方法，與 Microsoft SQL Server 資料庫互動。 不過，務必了解，ASP.NET MVC 架構未繫結至 LINQ to SQL 以任何方式。 ASP.NET MVC 是與任何資料存取技術相容。

## <a name="create-a-movie-database"></a>建立電影資料庫

在此教學課程--以說明如何建置模型類別-我們建置簡單的電影資料庫應用程式。 第一個步驟是建立新的資料庫。 以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目**。 選取**SQL Server 資料庫**範本，讓它的名稱 MoviesDB.mdf，再按一下**新增**按鈕 （請參閱圖 1）。


[![加入新的 SQL Server 資料庫](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**圖 01**： 加入新的 SQL Server 資料庫 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


建立新的資料庫之後，您可以開啟資料庫應用程式中按兩下 MoviesDB.mdf\_Data 資料夾。 按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗 （請參閱圖 2）。

[伺服器總管] 視窗使用 Visual Web Developer 中時呼叫 [資料庫總管] 視窗。


[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**圖 02**： 使用 [伺服器總管] 視窗 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


我們需要將一個資料表加入至資料庫，表示我們電影。 以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。 選取此功能表選項會開啟 資料表設計工具 （請參閱圖 3）。


[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**圖 03**： 資料表設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


我們需要將下列資料行加入至資料庫資料表：

| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(200) | False |
| 導向器 | Nvarchar （50) | False |

您需要執行兩個特殊的動作識別碼資料行。 首先，您必須將標示為主要索引鍵資料行的識別碼資料行，資料表設計工具中選取資料行，然後按一下 索引鍵的圖示。 LINQ to SQL 會要求您執行插入或更新資料庫時，指定主要的索引鍵資料行。

接下來，您必須將標示為識別資料行的識別碼資料行所指定的值給**為識別**屬性 （請參閱圖 3）。 識別資料行是指派新的數字自動每當您將新的資料列加入資料表的資料行。

## <a name="create-linq-to-sql-classes"></a>建立 LINQ to SQL 類別

我們的 MVC 模型會包含 LINQ to SQL 類別代表 tblMovie 資料庫資料表。 若要建立這些 LINQ to SQL 類別最簡單方式是以滑鼠右鍵按一下 模型 資料夾中，選取**新增、 新項目**，選取 LINQ to SQL 類別範本，將類別命名 Movie.dbml，然後按一下**新增**按鈕 （請參閱圖 4）。


[![建立 LINQ to SQL 類別](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**圖 04**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


立即建立電影 LINQ to SQL 類別之後，便會顯示物件關聯式設計工具。 您可以從伺服器總管] 視窗拖曳至 [物件關聯式設計工具建立 LINQ to SQL 類別代表特定的資料庫資料表拖曳資料庫資料表。 我們需要加入 tblMovie 資料庫資料表拖曳至 物件關聯式設計工具 （請參閱圖 5）。


[![使用物件關聯式設計工具](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**圖 05**： 使用物件關聯式設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


根據預設，物件關聯式設計工具建立資料庫資料表拖曳至設計工具的非常與同名的類別。 不過，我們不想要呼叫類別`tblMovie`。 因此，按一下 類別設計工具中的名稱，將類別的名稱變更為影片。

最後，請記得要按一下**儲存**按鈕 （磁片的圖片） 儲存 LINQ to SQL 類別。 否則，LINQ to SQL 類別不會產生的物件關聯式設計工具。

## <a name="using-linq-to-sql-in-a-controller-action"></a>使用 LINQ to SQL 中的控制器動作

現在，我們已經我們 LINQ to SQL 類別時，我們可以使用這些類別，從資料庫擷取資料。 在本節中，您可以了解如何使用 LINQ to SQL 類別，直接在控制器動作。 我們將 MVC 檢視中顯示 tblMovies 資料庫資料表中的電影的清單。

首先，我們需要修改 HomeController 類別。 這個類別可以找到您的應用程式的 [控制器] 資料夾中。 修改類別，讓它看起來像是列表 1 中的類別。

**列出 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()`列表 1 中的動作是使用 LINQ to SQL DataContext 類別 ( `MovieDataContext`) 來代表`MoviesDB`資料庫。 `MoveDataContext`由 Visual Studio 物件關聯式設計工具產生的類別。

LINQ 查詢來擷取所有從電影 DataContext 針對執行`tblMovies`資料庫資料表。 電影清單會指派給本機變數名稱`movies`。 最後的電影清單會傳遞至檢視檢視資料。

若要顯示電影，我們接下來要修改索引檢視表。 您可以找到的索引檢視`Views\Home\`資料夾。 更新索引檢視，讓它看起來像是列表 2 中的檢視。

**列出 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

請注意，修改的索引檢視表包含`<%@ import namespace %>`指示詞上方的檢視。 這個指示詞會匯入`MvcApplication1.Models namespace`。 我們需要此命名空間，才能使用`model`類別 – 特別的是，`Movie`類別-檢視中的。

列出 2 中的檢視包含`foreach`逐一查看所有項目所代表的迴圈`ViewData.Model`屬性。 值`Title`屬性會顯示每個`movie`。

請注意，值`ViewData.Model`屬性轉換成`IEnumerable`。 這是必要的內容執行迴圈`ViewData.Model`。 這裡的另一個選項是建立強型別`view`。 當您建立強型別`view`，您將轉型`ViewData.Model`檢視的程式碼後置類別中的特定類型的屬性。

如果您執行應用程式之後修改`HomeController`類別和索引檢視就會得到空白頁。 將會取得空白網頁，因為沒有在電影記錄`tblMovies`資料庫資料表。

才能加入至記錄`tblMovies`資料庫資料表中，以滑鼠右鍵按一下`tblMovies`資料庫 （在 Visual Web Developer 的 [資料庫總管] 視窗） 中的 [伺服器總管] 視窗中的資料表，並選取功能表選項顯示資料表資料。 您可以插入`movie`記錄所使用的方格，會出現 （請參閱圖 6）。


[![插入影片](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**圖 06**： 插入影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


新增某些資料庫記錄之後`tblMovies`資料表，而且您執行應用程式，您會看到圖 7 中的頁面。 所有的電影資料庫記錄會顯示在項目符號清單。


[![顯示與索引檢視的電影](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**圖 07**： 顯示與索引檢視的影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>使用儲存機制模式

在上一節中，我們會使用 LINQ to SQL 類別，直接在控制器動作。 我們使用`MovieDataContext`類別直接從`Index()`控制器動作。 並沒有任何在簡單的應用程式的情況下執行這個問題。 不過，在控制器類別中的直接使用 LINQ to SQL 工作會產生問題時您需要建置更複雜的應用程式。

控制器類別中使用 LINQ to SQL 難以未來切換資料存取技術。 例如，您可能決定切換使用為您的資料存取技術的 Microsoft Entity Framework 使用 Microsoft LINQ to SQL。 在此情況下，您需要重寫存取的資料庫應用程式內的每個控制站。

使用 LINQ to SQL 中控制器類別也很難建置您的應用程式的單元測試。 一般來說，您不想執行單元測試時，與資料庫互動。 您想要使用您的單元測試來測試應用程式邏輯而非您資料庫伺服器。

若要建立的 MVC 應用程式更適應未來的變更和可以更輕鬆地測試，您應該考慮使用儲存機制模式。 當您使用儲存機制模式時，您會建立個別的儲存機制類別，其中包含所有您資料庫的存取邏輯。

當您建立的儲存機制類別時，您會建立介面，代表所有儲存機制類別所使用的方法。 中的控制站，您可以撰寫您的程式碼，而不是儲存機制介面。 這樣一來，您可以實作在未來使用不同的資料存取技術的儲存機制。

介面中列出的 3 名為`IMovieRepository`，而它代表單一方法，名為`ListAll()`。

**列出 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

實作儲存機制中的類別清單 4`IMovieRepository`介面。 請注意，它包含方法，名為`ListAll()`對應於所需的方法`IMovieRepository`介面。

**列出 4 – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

最後，`MoviesController`列出 5 中的類別會使用儲存機制模式。 它不會再使用 LINQ to SQL 類別直接。

**列出 5 – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

請注意，`MoviesController`中列出 5 類別都有兩個建構函式。 應用程式執行時，會呼叫第一個建構函式，無參數建構函式。 這個建構函式建立的執行個體`MovieRepository`類別，並將其傳遞給第二個建構函式。

第二個建構函式具有單一參數：`IMovieRepository`參數。 這個建構函式只會將參數的值指派給名為的類別層級欄位`_repository`。

`MoviesController`類別利用一種軟體設計模式，呼叫相依性插入模式。 特別是，它使用呼叫建構函式相依性插入的項目。 閱讀更多有關閱讀下列文章依 Martin Fowler 此模式：

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

請注意，所有的程式碼中`MoviesController`（除了第一個建構函式） 的類別互動`IMovieRepository`介面而非實際`MovieRepository`類別。 程式碼互動的抽象介面，而不是介面的具象實作。

如果您想要修改應用程式所使用的資料存取技術，則您可以直接實作`IMovieRepository`使用替代的資料庫存取技術的類別與介面。 例如，您可以建立`EntityFrameworkMovieRepository`類別或`SubSonicMovieRepository`類別。 由於控制器類別所撰寫的介面，因此您可以傳遞的新實作`IMovieRepository`至控制器類別和類別會繼續運作。

此外，如果您想要測試`MoviesController`類別，則您可以傳遞假的電影儲存機制類別`HomeController`。 您可以實作`IMovieRepository`類別的類別，並不會實際存取資料庫，但包含所有必要的方法與`IMovieRepository`介面。 這樣一來，您可以單元測試`MoviesController`類別而不需要實際存取實際的資料庫。

## <a name="summary"></a>總結

本教學課程的目標是為了示範如何建立 MVC 模型類別，藉由運用 Microsoft LINQ to SQL。 我們會檢查兩種策略，在 ASP.NET MVC 應用程式中顯示資料庫的資料。 首先，我們建立 LINQ to SQL 類別，並使用直接內控制器動作的類別。 使用 LINQ to SQL 類別的控制器內，可讓您快速並輕鬆地在 MVC 應用程式中顯示資料庫的資料。

接下來，我們已探索的稍微更困難，但更明確地正向，顯示資料庫資料路徑。 我們已利用儲存機制模式，所有資料庫存取邏輯放在不同的儲存機制類別。 中我們控制站，我們會寫入所有我們的程式碼，根據介面而不是具象類別。 儲存機制模式的優點是它可讓我們來輕鬆地在未來變更資料庫存取技術，可讓我們來輕鬆地測試控制器類別。

> [!div class="step-by-step"]
> [上一頁](creating-model-classes-with-the-entity-framework-cs.md)
> [下一頁](displaying-a-table-of-database-data-cs.md)
