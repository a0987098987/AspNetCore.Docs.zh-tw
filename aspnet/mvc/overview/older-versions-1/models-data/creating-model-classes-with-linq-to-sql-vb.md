---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: 使用 LINQ to SQL (VB) 建立模型類別 |Microsoft Docs
author: microsoft
description: 本教學課程的目標是說明建立 ASP.NET MVC 應用程式的模型類別的一種方法。 在本教學課程中，您會學習如何建置模型 c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 168fbb914b54f88a78db63c16b03c55cc59c4a11
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833007"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>使用 LINQ to SQL (VB) 建立模型類別
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> 本教學課程的目標是說明建立 ASP.NET MVC 應用程式的模型類別的一種方法。 在本教學課程中，您已了解如何建置模型類別，並藉由利用 Microsoft LINQ to SQL 中執行資料庫存取權。


本教學課程的目標是說明建立 ASP.NET MVC 應用程式的模型類別的一種方法。 在本教學課程中，您已了解如何建置模型類別，並藉由利用 Microsoft LINQ to SQL 中執行資料庫存取權。

在本教學課程中，我們會建置基本的影片資料庫應用程式。 我們先以最快、 最簡單的方式可能建立影片資料庫應用程式。 我們會執行所有存取我們的資料直接從我們的控制器動作。

接下來，您會了解如何使用儲存機制模式。 使用儲存機制模式需要多一點的工作。 不過，採用此模式的優點是它可讓您建置可適用於應用程式變更，而且可以輕鬆地測試。

## <a name="what-is-a-model-class"></a>模型類別為何？

MVC 模型包含的所有未包含在 MVC 檢視或 MVC 控制器中的應用程式邏輯。 特別是，MVC 模型包含的所有應用程式的商務和資料存取邏輯。

您可以使用各種不同的技術來實作資料存取邏輯。 例如，您可以建置使用 Microsoft Entity Framework、 NHibernate、 Subsonic 或 ADO.NET 類別的資料存取類別。

在本教學課程中，我可以使用 LINQ to SQL 查詢和更新資料庫。 LINQ to SQL 會提供您與 Microsoft SQL Server 資料庫互動的非常簡單的方法。 不過，務必了解，ASP.NET MVC 架構未繫結至 LINQ to SQL 以任何方式。 ASP.NET MVC 是與任何資料存取技術相容。

## <a name="create-a-movie-database"></a>建立影片資料庫

在本教學課程中，以說明如何建立模型類別，我們建置一個簡單的電影資料庫應用程式。 第一個步驟是建立新的資料庫。 以滑鼠右鍵按一下 [應用程式\_Data 資料夾中的方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。 選取 SQL Server 資料庫範本，讓它名稱 MoviesDB.mdf，再按一下**新增**按鈕 （請參閱 圖 1）。


[![加入新的 SQL Server 資料庫](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**圖 01**： 加入新的 SQL Server 資料庫 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


建立新的資料庫之後，您可以按兩下 MoviesDB.mdf 檔案的應用程式中開啟資料庫\_Data 資料夾。 按兩下 MoviesDB.mdf 檔案隨即開啟 伺服器總管 視窗 （請參閱 圖 2）。


|   | 使用 Visual Web Developer 時，[伺服器總管] 視窗會呼叫 [資料庫總管] 視窗。 |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**圖 02**： 使用 [伺服器總管] 視窗 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


我們需要將一個資料表加入至資料庫，表示我們的影片。 以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。 選取此功能表選項會開啟 資料表設計工具 （請參閱 圖 3）。


[![使用 [伺服器總管] 視窗](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**圖 03**： 資料表設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


我們需要將下列資料行加入至資料庫資料表：

| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(200 | False |
| 總監 | nvarchar(50) | False |

您需要執行兩個特殊的動作識別碼資料行。 首先，您需要將識別碼資料行標示為主要的索引鍵資料行中，資料表設計工具中選取資料行，然後按一下 機碼的圖示。 LINQ to SQL 會要求您指定您主要的索引鍵資料行，當執行插入或更新對資料庫。

接下來，您需要將識別碼資料行標示為識別欄位中，是將值指派給**是識別**屬性 （請參閱 [圖 3]）。 識別資料行是會自動指派給新的數字每當您將新的資料列加入資料表的資料行。

進行這些變更之後，請使用名稱 tblMovie 儲存資料表。 您可以依序按一下 [儲存] 按鈕來儲存資料表。

## <a name="create-linq-to-sql-classes"></a>建立 LINQ to SQL 類別

我們 MVC 的模型會包含 LINQ to SQL 類別代表 tblMovie 資料庫資料表。 若要建立這些 LINQ to SQL 類別最簡單方式是以滑鼠右鍵按一下 模型 資料夾中，選取**新增、 新項目**、 選取 LINQ to SQL 類別範本，將類別命名 Movie.dbml，然後按一下**新增**按鈕 （請參閱 圖 4）。


[![建立 LINQ to SQL 類別](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**圖 04**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


立即建立影片的 LINQ to SQL 類別之後，會顯示物件關聯式設計工具。 您可以將資料庫資料表從 [伺服器總管] 視窗拖曳至物件關聯式設計工具建立 LINQ to SQL 類別代表特定的資料庫資料表。 我們需要加入 tblMovie 資料庫資料表拖曳至物件關聯式設計工具 （請參閱 圖 4）。


[![使用物件關聯式設計工具](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**圖 05**： 使用物件關聯式設計工具 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


根據預設，物件關聯式設計工具會建立具有相同名稱做為資料庫資料表拖曳至設計工具的類別。 不過，我們不想要呼叫我們的類別 tblMovie。 因此，按一下 類別設計工具中的名稱，並將變更影片的類別名稱。

最後，請記得按一下 **儲存**按鈕 （磁片的圖片） 來儲存 LINQ to SQL 類別。 否則，LINQ to SQL 類別不會產生由物件關聯式設計工具。

## <a name="using-linq-to-sql-in-a-controller-action"></a>使用 LINQ to SQL 中的控制器動作

現在，我們有我們的 LINQ to SQL 類別時，我們可以使用這些類別來擷取資料庫中的資料。 在本節中，您將了解如何使用 LINQ to SQL 類別，直接在控制器動作。 我們會在 MVC 檢視中顯示 tblMovies 資料庫資料表中的電影的清單。

首先，我們需要修改 HomeController 類別。 這個類別可在您的應用程式的 [控制器] 資料夾中。 修改類別，使它看起來像 列表 1 中的類別。

**列表 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

在 列表 1 中的 index （） 動作會使用 LINQ to SQL DataContext 類別 (MovieDataContext) 來代表 MoviesDB 資料庫。 MoveDataContext 類別是由 Visual Studio 物件關聯式設計工具產生。

LINQ 查詢會針對從 tblMovies 資料庫資料表擷取所有電影 DataContext 執行。 電影清單會指派給名為電影的本機變數。 最後的電影清單會傳遞至檢視檢視資料。

若要顯示電影，我們接下來要修改索引檢視表。 您可以找到 [索引] 檢視 Views\Home\ 資料夾中。 更新 索引 檢視，使它看起來像 列表 2 中的檢視。

**列表 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

請注意，修改過的 [索引] 檢視包含&lt;匯入命名空間 %@ %&gt;頂端的檢視指示詞。 這個指示詞會匯入 MvcApplication1 命名空間。 我們需要此命名空間，才能使用模型類別 – 特別是，電影類別-在檢視中。

列表 2 中的檢視包含一個 For Each 迴圈來逐一查看所有 ViewData.Model 屬性所表示的項目。 Title 屬性的值會顯示每一部電影。

請注意 ViewData.Model 屬性的值會轉換成 IEnumerable。 這是必備的 ViewData.Model 內容執行迴圈。 這裡的另一個選項是建立強型別檢視。 當您建立的強型別檢視時，您可以轉換 ViewData.Model 屬性，為特定的型別檢視的程式碼後置類別中。

如果您在執行之後修改 HomeController 類別和 [索引] 檢視，則會出現空白頁面，您的應用程式。 因為沒有任何影片記錄 tblMovies 資料庫資料表中，您會收到空白頁面。

若要將記錄新增至 tblMovies 資料庫資料表，tblMovies 資料庫資料表，在 [伺服器總管] 視窗 （在 Visual Web Developer 的 [資料庫總管] 視窗） 上按一下滑鼠右鍵，然後選取功能表選項**顯示資料表資料**。 您可以插入電影記錄所使用的方格，會出現 （請參閱 [圖 5]）。


[![插入電影](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**圖 06**： 插入電影 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


您將某些資料庫記錄加入 tblMovies 資料表，並執行應用程式之後，您會看到 [圖 7] 中的頁面。 所有的電影資料庫記錄會顯示在項目符號清單。


[![顯示與索引檢視的電影](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**圖 07**： 顯示與索引檢視的影片 ([按一下以檢視完整大小的影像](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>使用儲存機制模式

在上一節中，我們會使用 LINQ to SQL 類別，直接在控制器動作。 我們使用 MovieDataContext 類別，直接從 index （） 的控制器動作。 沒有任何問題，在簡單的應用程式的情況下這項操作。 不過，在控制器類別中的直接使用 LINQ to SQL 工作會產生問題，當您要建置更複雜的應用程式。

使用 LINQ to SQL 中的控制器類別使您更難以在未來切換資料存取技術。 例如，您可能會決定切換到使用做為您的資料存取技術的 Microsoft Entity Framework 使用 Microsoft LINQ to SQL。 在此情況下，您必須重寫每個控制站可存取您的應用程式內的資料庫。

使用 LINQ to SQL 中的控制器類別也很難建置您的應用程式的單元測試。 一般來說，您不想要執行單元測試時，與資料庫互動。 您想要使用您的單元測試來測試您的應用程式邏輯而非您資料庫伺服器。

若要建立 MVC 應用程式中，這是更適用於未來的變更可以更輕鬆地測試，您應該考慮使用儲存機制模式。 當您使用儲存機制模式時，您會建立個別的存放庫類別，其中包含所有的資料庫存取邏輯。

當您建立儲存機制類別時，您會建立表示所有儲存機制類別所使用的方法的介面。 在您的控制器，您撰寫您的程式碼的介面，而不是存放庫。 如此一來，您可以實作存放庫在未來使用不同的資料存取技術。

列表 3 中的介面為 IMovieRepository，它代表一個名為 ListAll() 的單一方法。

**列表 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

在 列表 4 中的儲存機制類別會實作 IMovieRepository 介面。 請注意，它會包含名為 ListAll() 對應至 IMovieRepository 介面所需的方法的方法。

**列表 4 – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

最後，在 列表 5 MoviesController 類別會使用儲存機制模式。 它不會再使用 LINQ to SQL 類別直接。

**列表 5 – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

請注意，在 列表 5 MoviesController 類別有兩個建構函式。 您的應用程式執行時，會呼叫第一個建構函式的無參數建構函式。 這個建構函式會建立 MovieRepository 類別的執行個體，並將它傳遞給第二個建構函式。

第二個建構函式具有單一參數： IMovieRepository 參數。 這個建構函式只會將參數的值指派給名為的類別層級欄位\_存放庫。

MoviesController 類別就利用一種軟體設計模式，稱為相依性插入模式。 特別是，它使用所謂的建構函式相依性插入。 您可以深入了解此模式，請閱讀下列文章 Martin fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

請注意所有 MoviesController 類別 （除了第一個建構函式） 中的程式碼與 IMovieRepository 介面，而不是實際的 MovieRepository 類別互動。 程式碼互動的抽象的介面，而不是介面的具象實作。

如果您想要修改應用程式所使用的資料存取技術然後您就可以直接實作 IMovieRepository 介面，以使用替代的資料庫存取技術的類別。 例如，您可以建立 EntityFrameworkMovieRepository 類別或 SubSonicMovieRepository 類別。 因為控制器類別撰寫的介面，您可以將新的實作的 IMovieRepository 傳遞至控制器類別，該類別會繼續運作。

此外，如果您想要測試 MoviesController 類別，然後您可以將假的電影儲存機制類別 MoviesController。 您可以實作 IMovieRepository 類別實際上不會存取資料庫，但包含所有必要的 IMovieRepository 介面方法的類別。 如此一來，您會進行單元測試 MoviesController 類別而不需要實際存取實際的資料庫。

## <a name="summary"></a>總結

本教學課程的目標是要示範如何建立 MVC 模型類別，藉由利用 Microsoft LINQ to SQL。 我們會檢查 ASP.NET MVC 應用程式中顯示資料庫資料的兩種策略。 首先，我們會建立 LINQ to SQL 類別，並使用直接在控制器動作的類別。 使用 LINQ to SQL 類別的控制器內，可讓您快速並輕鬆地顯示在 MVC 應用程式中的 資料庫資料。

接下來，我們探討了稍微困難，但一定更良性，顯示資料庫資料路徑。 我們花了儲存機制模式的優點和所有我們資料庫存取邏輯放在個別的存放庫類別。 在我們的控制器中，我們會撰寫所有我們的程式碼，根據介面而不是具象類別。 儲存機制模式的優點是，它可讓我們將在未來輕鬆變更資料庫存取技術，而且它可讓我們輕鬆地測試我們的控制器類別。

> [!div class="step-by-step"]
> [上一頁](creating-model-classes-with-the-entity-framework-vb.md)
> [下一頁](displaying-a-table-of-database-data-vb.md)
