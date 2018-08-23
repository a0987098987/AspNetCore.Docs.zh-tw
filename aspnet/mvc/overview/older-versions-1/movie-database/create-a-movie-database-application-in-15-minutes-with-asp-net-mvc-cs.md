---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: 建立影片資料庫應用程式在 15 分鐘內，使用 ASP.NET MVC (C#) |Microsoft Docs
author: StephenWalther
description: Stephen Walther 建置整個資料庫導向 ASP.NET MVC 應用程式從開始到完成。 本教學課程是針對新的 t 的人了解...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 16d62102713b0b86aa93284e39c82d3ae492f892
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830964"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>建立影片資料庫應用程式在 15 分鐘內，使用 ASP.NET MVC (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

[下載程式碼](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther 建置整個資料庫導向 ASP.NET MVC 應用程式從開始到完成。 本教學課程是了解誰是 ASP.NET MVC Framework 的新手，而且想要了解建置 ASP.NET MVC 應用程式的程序的人員。


本教學課程的目的是讓您的 「 功能很類似 」 的意義上建置 ASP.NET MVC 應用程式。 在本教學課程中，我炸毀透過建置整個 ASP.NET MVC 應用程式從開始到完成。 我會告訴您如何建置的簡單資料庫導向應用程式示範如何列出、 建立和編輯資料庫中的記錄。

若要簡化建置我們的應用程式的程序，我們將利用 Visual Studio 2008 的 scaffolding 功能。 我們會讓 Visual Studio 產生的初始程式碼和我們的控制器、 模型和檢視表的內容。

如果您已使用 Active Server Pages 或 ASP.NET，則您應該會發現 ASP.NET MVC 非常熟悉。 ASP.NET MVC 檢視很像是 Active Server Pages 應用程式中的頁面。 而且就像傳統的 ASP.NET Web Forms 應用程式，ASP.NET MVC 為您提供的完整存取權一組豐富的語言和.NET framework 所提供的類別。

我希望是，本教學課程會提供您了解如何建置 ASP.NET MVC 應用程式的體驗是類似，不同於建置動態伺服器網頁或 ASP.NET Web Forms 應用程式的體驗。

## <a name="overview-of-the-movie-database-application"></a>影片資料庫應用程式的概觀

因為我們的目標是為了簡單起見，我們將建置一個非常簡單的電影資料庫應用程式。 我們簡單的電影資料庫應用程式可讓我們將執行三件事：

1. 列出一組影片資料庫記錄
2. 建立新的電影資料庫記錄
3. 編輯現有的電影資料庫記錄

同樣地，因為我們想要簡單起見，我們會利用建置我們的應用程式所需的 ASP.NET MVC framework 功能的最小數目。 比方說，我們將不會利用測試導向開發。

若要建立我們的應用程式，我們需要完成下列步驟：

1. 建立 ASP.NET MVC Web 應用程式專案
2. 建立資料庫
3. 建立資料庫模型
4. 建立 ASP.NET MVC 控制器
5. 建立 ASP.NET MVC 檢視

## <a name="preliminaries"></a>準備工作

您將需要 Visual Studio 2008 或 Visual Web Developer 2008 Express 建置 ASP.NET MVC 應用程式。 您也需要下載 ASP.NET MVC 架構。

如果您沒有 Visual Studio 2008，您可以從此網站下載 Visual Studio 2008 90 天試用版：

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

或者，您可以建立 ASP.NET MVC 應用程式與 Visual Web Developer Express 2008。 如果您決定要使用 Visual Web Developer Express，您必須已安裝 Service Pack 1。 您可以下載 Visual Web Developer 2008 Express 含 Service Pack 1，從這個網站：

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&ampdisplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

安裝 Visual Studio 2008 或 Visual Web Developer 2008 之後，必須先安裝 ASP.NET MVC 架構。 您可以從下列網站下載 ASP.NET MVC 架構：

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> 而不是個別下載 ASP.NET framework 和 ASP.NET MVC 架構，您可以利用 Web Platform Installer。 Web Platform Installer 是可讓您輕鬆地管理已安裝的應用程式的應用程式有您的電腦：
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>建立 ASP.NET MVC Web 應用程式專案

現在就開始在 Visual Studio 2008 中建立新的 ASP.NET MVC Web 應用程式專案。 選取功能表選項**檔案]、 [新增專案**，您會看到 [圖 1] 中的 [新增專案] 對話方塊。 選取 C# 程式設計語言，然後選取 ASP.NET MVC Web 應用程式專案範本。 為您的專案名稱 MovieApp，然後按一下 [確定] 按鈕。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**[圖 01**: 新的專案] 對話方塊中 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


請確定您從下拉式清單中頂端的 新增專案 對話方塊中選取 .NET Framework 3.5，或 ASP.NET MVC Web 應用程式專案範本不會出現。


每當您建立新的 MVC Web 應用程式專案，Visual Studio 會提示您建立個別的單元測試專案。 [圖 2] 對話方塊隨即出現。 因為我們不會建立測試本教學課程中由於時間限制 （然後是，我們應該會覺得有點怪一點） 選取**No**選項，然後按一下**確定** 按鈕。

> [!NOTE] 
> 
> Visual Web Developer 不支援測試專案。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**[圖 02**: 建立單元測試專案] 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


ASP.NET MVC 應用程式有一組標準的資料夾： 模型、 檢視和控制器的資料夾。 您可以看到這一組標準的 [方案總管] 視窗中的資料夾。 我們必須將檔案新增至每個模型、 檢視和控制器資料夾中，才能建立影片資料庫應用程式。

當您使用 Visual Studio 中建立新的 MVC 應用程式時，您會取得範例應用程式。 因為我們想要從頭開始，我們必須刪除此範例應用程式的內容。 您必須刪除下列檔案和下列資料夾：

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>建立資料庫

我們需要建立資料庫來保存我們的電影資料庫記錄。 幸運的是，Visual Studio 會包含名為 SQL Server Express 的免費資料庫。 請遵循下列步驟來建立資料庫：

1. 以滑鼠右鍵按一下 [應用程式\_Data 資料夾中的方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。
2. 選取 **資料**類別目錄，然後選取**SQL Server 資料庫**範本 （見 圖 3）。
3. 將新資料庫命名*MoviesDB.mdf*然後按一下**新增** 按鈕。

建立您的資料庫之後，您可以按兩下 MoviesDB.mdf 檔案位於應用程式連接到資料庫\_Data 資料夾。 按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗。

> [!NOTE] 
> 
> 資料庫總管 視窗中，在 Visual Web Developer 的情況下名為 伺服器總管 視窗。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**圖 03**： 建立 Microsoft SQL Server 資料庫 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


接下來，我們需要建立新的資料庫資料表。 從 [伺服器總管] 視窗中，在 [資料表] 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入新的資料表**。 選取此功能表選項會開啟資料庫資料表設計工具。 建立下列的資料庫資料行：

<a id="0.1_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(100) | False |
| 總監 | Nvarchar(100) | False |
| DateReleased | DateTime | False |


第一個資料行，[識別碼] 欄中，有兩個特殊的屬性。 首先，您需要識別碼資料行標示為主要索引鍵資料行。 選取 識別碼 資料行之後, 按一下 **設定主索引鍵**（它是看起來像索引鍵的圖示） 按鈕。 第二，您需要識別碼資料行標示為識別欄位。 在資料行屬性] 視窗中，捲動至 [識別規格 」 區段，並加以展開。 變更**是身分識別**屬性設為值**是**。 當您完成時，資料表看起來應該像 圖 4。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**圖 04**: 電影資料庫資料表 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


最後一個步驟是以儲存新的資料表。 按一下 [儲存] 按鈕 （磁片圖示），並提供新的資料表名稱電影。

完成 建立資料表之後，某些電影將記錄新增至資料表。 在 [伺服器總管] 視窗中的電影資料表上按一下滑鼠右鍵，然後選取功能表選項**顯示資料表資料**。 輸入您最愛的電影，（請參閱 [圖 5]） 的清單。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**圖 05**： 輸入影片資料錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>建立模型

接下來，我們需要建立一組類別來代表我們的資料庫。 我們需要建立資料庫模型。 我們會利用 Microsoft Entity Framework 自動產生我們的資料庫模型的類別。

> [!NOTE] 
> 
> ASP.NET MVC 架構不會繫結至 Microsoft Entity Framework。 您可以建立您的資料庫模型類別利用各種不同的物件關聯式對應 (或者 / M) 工具，包括 LINQ to SQL、 Subsonic 和 NHibernate。


請遵循下列步驟來啟動 Entity Data Model 精靈：

1. 在 [方案總管] 視窗，然後選取功能表選項中的 [Models] 資料夾上按一下滑鼠右鍵**新增]、 [新項目**。
2. 選取 **資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。
3. 將您的資料模型命名*MoviesDBModel.edmx*然後按一下**新增** 按鈕。

按一下 [新增] 按鈕之後，實體資料模型精靈] 會出現 （請參閱 [圖 6）。 請遵循下列步驟來完成精靈：

1. 在 **選擇模型內容**步驟中，選取**從資料庫產生**選項。
2. 在 **選擇資料連接**步驟中，使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連線設定。 按一下 **下一步**  按鈕。
3. 在 **選擇您的資料庫物件**步驟中，展開 資料表 節點中，選取 電影資料表。 輸入的命名空間*MovieApp.Models*然後按一下**完成** 按鈕。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**圖 06**： 產生 Entity Data Model 精靈的資料庫模型 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Entity Data Model 精靈完成之後，就會開啟實體資料模型設計工具。 設計工具應該會顯示電影資料庫資料表 （請參閱 圖 7）。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**圖 07**: 實體資料模型設計工具 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


我們需要進行一項變更，再繼續進行。 實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。 因為我們將使用電影類別來代表特定的影片，我們需要修改的類別名稱*電影*而不是*電影*（單數而複數）。

按兩下設計工具介面上類別名稱，並從電影時使用的類別名稱變更為電影。 完成此變更之後，按一下**儲存**按鈕 （磁片圖示） 來產生電影類別。

## <a name="creating-the-aspnet-mvc-controller"></a>建立 ASP.NET MVC 控制器

下一個步驟是建立 ASP.NET MVC 控制器。 控制器負責控制使用者與 ASP.NET MVC 應用程式之間的互動方式。

請依照下列步驟：

1. 在 [方案總管] 視窗中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取功能表選項**新增]、 [控制站**。
2. 在 新增控制器 對話方塊中，輸入名稱*HomeController* ，並檢查標示的核取方塊**將動作方法，如 Create、 Update 和 Details 案例加入**（請參閱 圖 8）。
3. 按一下 [**新增**] 按鈕，將新的控制器新增至您的專案。

完成這些步驟之後，會建立 列表 1 中的控制站。 請注意它所包含的方法，名為索引的詳細資訊，建立、 和編輯。 在下列章節中，我們將新增必要的程式碼，以取得這些方法才能運作。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**圖 08**： 加入新的 ASP.NET MVC 控制站 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**列表 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>列出資料庫中的記錄

主控制器的 index （） 方法是在 ASP.NET MVC 應用程式的預設方法。 當您執行的 ASP.NET MVC 應用程式時，index （） 方法會是第一個控制器方法所呼叫。

我們將使用 index （） 方法，以顯示電影資料庫資料表中的記錄清單。 我們會利用資料庫擷取電影資料庫記錄，index （） 方法與稍早建立的模型類別。

我已修改 HomeController 類別，在 列表 2 中的，使其包含名為的新私用欄位\_db。 MoviesDBEntities 類別代表我們的資料庫模型，我們將使用此類別與資料庫通訊。

我也已修改的列表 2 中的 index （） 方法。 Index （） 方法會使用 MoviesDBEntities 類別來擷取所有電影資料錄的電影資料庫資料表中。 運算式 *\_db。MovieSet.ToList()* 電影資料庫資料表中傳回所有電影資料錄的清單。

電影清單會傳遞至檢視。 取得傳遞至 View() 方法的任何項目取得傳遞至檢視，以檢視資料。

**列表 2 – controllers/Homecontroller.cs （已修改索引方法）**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Index （） 方法會傳回名稱為索引的檢視。 我們需要建立此檢視來顯示電影資料庫記錄的清單。 請依照下列步驟：


您應該會建置您的專案 (選取功能表選項**建置時，建置方案**) 開啟之前**加入檢視**對話 」 或 「 無類別會出現在**檢視資料類別**下拉式清單中。


1. Index （） 方法，在程式碼編輯器上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱 圖 9）。
2. 在 新增檢視 對話方塊中，確認 核取方塊標示**建立強型別檢視**已核取。
3. 從**檢視內容**下拉式清單中，選取值*清單*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。
5. 按一下 新增 按鈕來建立新檢視 （請參閱 圖 10）。

完成這些步驟之後，新的檢視，名為 Index.aspx 會加入 Views\Home 資料夾中。 索引檢視的內容會包含在 列表 3。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**圖 09**： 從控制器動作中加入的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**圖 10**： 使用 [新增檢視] 對話方塊建立新的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**列表 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

[索引] 檢視會顯示所有電影資料錄從 HTML 資料表中的電影資料庫資料表。 此檢視包含的 foreach 迴圈，逐一查看 ViewData.Model 屬性所表示的每個影片。 如果您按下 F5 鍵執行應用程式，您會看到 [圖 11] 中的網頁。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**圖 11**: [索引] 檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>建立新的資料庫資料錄

我們在上一節中建立的 [索引] 檢視包含用於建立新的資料庫記錄的連結。 我們繼續實作邏輯和建立檢視所需建立新的電影資料庫記錄。

主控制器包含兩個方法，名為 create （）。 第一個 create （） 方法沒有任何參數。 這個 create （） 方法的多載用來顯示的 HTML 表單建立新的電影資料庫記錄。

第二個 create （） 方法具有 FormCollection 參數。 建立新的電影的 HTML 表單張貼至伺服器時，會呼叫這個 create （） 方法的多載。 請注意，此第二個 create （） 方法有 AcceptVerbs 屬性，除非執行 HTTP POST 作業呼叫時，避免該方法。

更新 HomeController 類別中列表 4 中已修改這個第二個 create （） 方法。 新版本的 create （） 方法會接受影片參數，並包含電影資料庫資料表中插入新的電影的邏輯。

> [!NOTE] 
> 
> 請注意繫結屬性。 因為我們不想要更新的影片識別碼屬性，從 HTML 表單，我們需要明確地排除這個屬性。


**列表 4 – Controllers\HomeController.cs （已修改的建立方法）**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio 可讓您輕鬆地建立的表單建立新的電影資料庫記錄 （請參閱 圖 12）。 請依照下列步驟：

1. 以滑鼠右鍵按一下 create （） 方法，在程式碼編輯器，然後選取功能表選項**加入檢視**。
2. 確認核取方塊標示**建立強型別檢視**已核取。
3. 從**檢視內容**下拉式清單中，選取值*建立*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。
5. 按一下 **新增**按鈕以建立新的檢視。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**圖 12**： 加入建立檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio 的檢視表 5 中會自動產生。 這個檢視包含 HTML 表單，其中包含欄位對應至每個電影類別的屬性。

**列表 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> [新增檢視] 對話方塊產生的 HTML 格式產生 Id 的表單欄位。 識別碼資料行是識別欄位，因為我們不需要此表單欄位，而且您可以安全地移除它。


加入建立檢視之後，您可以新增影片記錄到資料庫。 按下 F5 鍵執行應用程式，然後按一下 建立新的連結，以查看 圖 13 中的表單。 如果您完成，並送出表單時，會建立新的電影資料庫記錄。

請注意，您會自動取得表單驗證。 如果您沒有輸入影片，發行日期，或是您輸入無效的發行日期，然後會重新顯示表單，並反白顯示 [發行日期] 欄位。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**圖 13**： 建立新的電影資料庫記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>編輯現有的資料庫記錄

在先前章節中，我們會討論如何列出，並建立新的資料庫記錄。 在最後一節中，我們會討論如何編輯現有的資料庫記錄。

首先，我們需要產生的編輯表單。 這個步驟很容易，因為 Visual Studio 會產生的編輯表單為我們自動。 Visual Studio 程式碼編輯器中開啟 HomeController.cs 類別，並遵循下列步驟：

1. 以滑鼠右鍵按一下 edit （） 方法，在程式碼編輯器，然後選取功能表選項**加入檢視**（請參閱 圖 14）。
2. 選取標示的核取方塊**建立強型別檢視**。
3. 從**檢視內容**下拉式清單中，選取值*編輯*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Models.Movie*。
5. 按一下 **新增**按鈕以建立新的檢視。

完成這些步驟，新增名為 Edit.aspx Views\Home 資料夾檢視。 這個檢視包含編輯電影錄製之 HTML 表單。


[![[新增專案] 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**圖 14**： 加入編輯檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> [編輯] 檢視包含 HTML 表單欄位對應到影片識別碼屬性。 您不想讓使用者編輯 Id 屬性的值，您應該移除此表單欄位。


最後，我們需要修改主控制器，讓它支援 編輯資料庫記錄。 更新的 HomeController 類別都包含在 列表 6。

**列表 6 – Controllers\HomeController.cs （編輯方法）**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

在 列表 6，我新增額外的邏輯 edit （） 方法的兩個多載。 第一種 edit （） 方法傳回的電影資料庫記錄，其對應至傳遞給方法的 Id 參數。 第二個多載會執行到電影的記錄更新，在資料庫中。

請注意，您必須擷取原始的影片，然後呼叫 ApplyPropertyChanges()，更新資料庫中現有的影片。

## <a name="summary"></a>總結

本教學課程的目的是體驗的要給您的建置 ASP.NET MVC 應用程式。 我希望您會發現建置 ASP.NET MVC web 應用程式是非常類似於建置動態伺服器網頁或 ASP.NET 應用程式的體驗。

在本教學課程中，我們會檢查 ASP.NET MVC 架構的最基本功能。 在未來的教學課程中，我們更深入地探討的主題，例如控制器、 控制器動作、 檢視、 檢視資料和 HTML 協助程式。

> [!div class="step-by-step"]
> [下一步](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
