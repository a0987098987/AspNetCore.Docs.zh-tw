---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: "建立電影資料庫應用程式在 15 分鐘內，搭配 ASP.NET MVC (VB) |Microsoft 文件"
author: StephenWalther
description: "作者： Stephen Walther 建置整個資料庫驅動 ASP.NET MVC 應用程式從開始到完成。 本教學課程是不錯的介紹人士新 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: b87a69df24a410161dfaf055519eb6137fa76c06
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>建立電影資料庫應用程式在 15 分鐘內，搭配 ASP.NET MVC (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

[下載程式碼](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> 作者： Stephen Walther 建置整個資料庫驅動 ASP.NET MVC 應用程式從開始到完成。 本教學課程是不錯的介紹人熟悉 ASP.NET MVC Framework，並想要了解建立 ASP.NET MVC 應用程式的程序。


本教學課程的目的是要讓您了解它是什麼像 」 來建立 ASP.NET MVC 應用程式。 在本教學課程中，我炸毀透過建置整個 ASP.NET MVC 應用程式從開始到完成。 我將示範您如何建置簡單資料庫驅動的應用程式如何清單、 建立和編輯資料庫中的記錄。

若要簡化建立我們的應用程式的程序，我們 Visual Studio 2008 的 scaffolding 的功能。 我們會讓 Visual Studio 產生的初始程式碼和我們的控制器、 模型和檢視表的內容。

如果您已使用 Active Server Pages 或 ASP.NET，然後您應該會發現 ASP.NET MVC 非常熟悉。 ASP.NET MVC 檢視很像是 Active Server Pages 應用程式中的頁面。 此外，就像傳統的 ASP.NET Web Form 應用程式中，ASP.NET MVC 為您提供的完整存取權的一組豐富的語言和.NET framework 所提供的類別。

我希望是本教學課程會提供了解如何建立 ASP.NET MVC 應用程式的經驗是類似，不同於建置 Active Server Pages 或 ASP.NET Web Form 應用程式的體驗。

## <a name="overview-of-the-movie-database-application"></a>電影資料庫應用程式的概觀

因為我們的目標是為了簡單起見，我們將建置簡單的電影資料庫應用程式。 簡單的電影資料庫應用程式可讓我們進行三項作業：

1. 列出一組影片資料庫記錄
2. 建立新的電影資料庫記錄
3. 編輯現有的電影資料庫記錄

同樣地，由於我們想要讓事情變簡單，我們將利用 ASP.NET MVC 架構建置應用程式所需功能的最小數目。 例如，我們將不會利用有如開發。

若要建立應用程式，我們需要完成下列步驟：

1. 建立 ASP.NET MVC Web 應用程式專案
2. 建立資料庫
3. 建立資料庫模型
4. 建立 ASP.NET MVC 控制器
5. 建立 ASP.NET MVC 檢視

## <a name="preliminaries"></a>準備工作

您將需要 Visual Studio 2008 或 Visual Web Developer 2008 Express 建置 ASP.NET MVC 應用程式。 您也需要下載 ASP.NET MVC framework。

如果您沒有 Visual Studio 2008，您可以從此網站下載 Visual Studio 2008 的 90 天試用的版：

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

或者，您可以建立 ASP.NET MVC 應用程式與 Visual Web Developer Express 2008。 如果您決定要使用 Visual Web Developer Express，您必須已安裝 Service Pack 1。 您可以從此網站下載 Visual Web Developer 2008 Express Service Pack 1:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

安裝 Visual Studio 2008 或 Visual Web Developer 2008 之後，您需要安裝 ASP.NET MVC 架構。 您可以從下列網站下載 ASP.NET MVC 架構：

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> 而不是個別下載 ASP.NET framework 和 ASP.NET MVC 架構，您可以利用 Web Platform Installer。 Web Platform Installer 是可讓您輕鬆地管理已安裝的應用程式的應用程式是您的電腦：
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>建立 ASP.NET MVC Web 應用程式專案

讓我們開始在 Visual Studio 2008 中建立新的 ASP.NET MVC Web 應用程式專案。 選取功能表選項**檔案、 新的專案**，您會看到在圖 1 中新增專案 對話方塊。 選取 Visual Basic 程式設計語言，然後選取 ASP.NET MVC Web 應用程式專案範本。 為您的專案名稱 MovieApp，然後按一下 [確定] 按鈕。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**圖 01**: 新的專案 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


請確定您從 新增專案 對話方塊頂端的下拉式清單中選取 .NET Framework 3.5 或 ASP.NET MVC Web 應用程式專案範本就不會出現。


每當您建立新的 MVC Web 應用程式專案，Visual Studio 會提示您建立個別的單元測試專案。 圖 2 中的對話方塊隨即出現。 因為我們不會建立測試本教學課程中因為時間條件約束 （甚至是，我們應該覺得有點連載有關此） 選取**否**選項，然後按一下**確定** 按鈕。

> [!NOTE] 
> 
> Visual Web Developer 不支援測試專案。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**圖 02**: 建立單元測試專案 對話方塊 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


ASP.NET MVC 應用程式有一組標準的資料夾： 模型、 檢視和控制器的資料夾。 您可以查看這組標準的 [方案總管] 視窗中的資料夾。 我們需要將檔案新增至每個模型、 檢視和控制器的資料夾，才能建立電影資料庫應用程式。

當您使用 Visual Studio 建立新的 MVC 應用程式時，您會取得範例應用程式。 由於我們想要從頭開始，我們需要刪除此範例應用程式的內容。 您必須刪除下列檔案和下列資料夾：

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>建立資料庫

我們需要建立資料庫來保存我們影片的資料庫記錄。 幸運的是，Visual Studio 會包含名為 SQL Server Express 免費的資料庫。 請遵循下列步驟來建立資料庫：

1. 以滑鼠右鍵按一下應用程式\_Data 資料夾中的方案總管 視窗，然後選取功能表選項**新增、 新項目**。
2. 選取**資料**類別目錄，然後選取**SQL Server 資料庫**範本 （請參閱圖 3）。
3. 將您的新資料庫命名*MoviesDB.mdf*按一下**新增** 按鈕。

建立資料庫之後，您可以按兩下 MoviesDB.mdf 檔案位於 應用程式連接到資料庫\_Data 資料夾。 按兩下 MoviesDB.mdf 檔案會開啟 [伺服器總管] 視窗。

> [!NOTE] 
> 
> 在 Visual Web Developer 的情況下的 [資料庫總管] 視窗名為 [伺服器總管] 視窗。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**圖 03**： 建立 Microsoft SQL Server 資料庫 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


接下來，我們需要建立新的資料庫資料表。 從 [伺服器總管] 視窗中以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。 選取此功能表選項會開啟資料庫資料表設計工具。 建立下列的資料庫資料行：

<a id="0.2_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(100) | False |
| 導向器 | Nvarchar(100) | False |
| DateReleased | DateTime | False |


第一個資料行的識別碼資料行中，有兩個特殊的屬性。 首先，您需要識別碼資料行標示為主要索引鍵資料行。 選取 [Id] 資料行之後, 按**設定主索引鍵**（它是機碼看起來的圖示） 的按鈕。 第二，您需要識別碼資料行標示為識別資料行。 在 [資料行屬性] 視窗中向下捲動至 [識別規格] 區段，然後展開它。 變更**為識別**屬性設為值**是**。 當您完成時，資料表看起來應該像圖 4。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**圖 04**: 電影資料庫資料表 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


最後一個步驟是要儲存新的資料表。 按一下 [儲存] 按鈕 （磁片圖示），並提供新的資料表名稱電影。

完成 建立資料表之後，某些影片將記錄新增至資料表。 以滑鼠右鍵按一下 [伺服器總管] 視窗中的電影資料表，然後選取功能表選項**顯示資料表資料**。 輸入 （請參閱圖 5） 您最愛的電影清單。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**圖 05**： 輸入電影的記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>建立模型

接下來，我們必須建立一組類別來代表我們的資料庫。 我們需要建立資料庫模型。 我們會利用 Microsoft Entity Framework 自動產生的類別為我們的資料庫模型。

> [!NOTE] 
> 
> ASP.NET MVC 架構未繫結至 Microsoft 的 Entity Framework。 您可以建立您的資料庫模型類別藉由運用各種不同的物件關聯式對應 (或者 / M) 工具，包括 LINQ to SQL、 Subsonic 和 NHibernate。


請遵循下列步驟來啟動實體資料模型精靈：

1. 以滑鼠右鍵按一下 [模型] 資料夾，在 [方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。
2. 選取**資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。
3. 將您的資料模型命名*MoviesDBModel.edmx*按一下**新增** 按鈕。

按一下 [新增] 按鈕之後，實體資料模型精靈就會出現 （請參閱圖 6）。 請遵循下列步驟來完成精靈：

1. 在**選擇模型內容**步驟中，選取**從資料庫產生**選項。
2. 在**選擇資料連接**步驟，請使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連接設定。 按一下**下一步** 按鈕。
3. 在**選擇您的資料庫物件**步驟中，展開資料表節點，選取 電影資料表。 輸入的命名空間*MovieApp.Models*按一下**完成** 按鈕。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**圖 06**： 產生的資料庫模型，使用實體資料模型精靈 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


實體資料模型精靈完成之後，Entity Data Model Designer 隨即開啟。 在設計工具應該會顯示電影資料庫資料表 （請參閱圖 7）。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**圖 07**: Entity Data Model Designer ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


我們需要進行一項變更，我們繼續執行。 實體資料精靈會產生代表電影資料庫資料表的模型類別命名為電影。 由於我們將使用電影類別來代表特定的影片，我們需要修改的類別的類別名稱*影片*而不是*電影*（單數而複數）。

按兩下設計工具介面上的類別名稱，並將電影從類別的名稱，變更 電影。 進行這項變更之後, 按**儲存**按鈕 （磁片圖示），以產生影片類別。

## <a name="creating-the-aspnet-mvc-controller"></a>建立 ASP.NET MVC 控制器

下一個步驟是建立 ASP.NET MVC 控制器。 控制器負責控制使用者如何與 ASP.NET MVC 應用程式互動。

請依照下列步驟：

1. 在 [方案總管] 視窗中，以滑鼠右鍵按一下 [控制器] 資料夾並選取功能表選項**新增]、 [控制站**。
2. 在 [加入控制器] 對話方塊中，輸入名稱*HomeController*並標示核取**將動作方法，如 Create、 Update 和 Details 案例加入**（請參閱圖 8）。
3. 按一下**新增** 按鈕，將新的控制站新增至您的專案。

完成這些步驟之後，會建立程式碼範例 1 中的控制站。 請注意，它包含方法，名為索引的詳細資訊，建立，並編輯。 在下列章節中，我們會將新增所需的程式碼，讓這些方法才能運作。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**圖 08**： 加入新的 ASP.NET MVC 控制器 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**列出 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>列出資料庫中的記錄

主控制器的 index （） 方法是 ASP.NET MVC 應用程式的預設方法。 當您執行 ASP.NET MVC 應用程式時，index （） 方法是呼叫第一個控制器方法。

我們會使用 index （） 方法來顯示的電影資料庫資料表中的記錄清單。 我們會利用資料庫我們稍早建立的 index （） 方法擷取影片資料庫記錄的模型類別。

我已經修改 HomeController 類別清單 2 中的，使其包含名為的新私用欄位\_db。 MoviesDBEntities 類別代表我們的資料庫模型中，我們將使用此類別與資料庫通訊。

我也修改了 index （） 方法，在列出 2。 Index （） 方法使用 MoviesDBEntities 類別來擷取所有的電影記錄電影資料庫資料表中。 運算式 *\_db。MovieSet.ToList()*電影資料庫資料表中傳回的所有電影記錄清單。

電影清單會傳遞至檢視。 取得傳遞給 View() 方法的任何項目取得傳遞至檢視，以檢視資料。

**列出 2 – Controllers/HomeController.vb （修改索引的方法）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Index （） 方法會傳回名為索引的檢視。 我們需要建立此檢視來顯示影片資料庫記錄的清單。 請依照下列步驟：


您應該建置專案時 (選取功能表選項**建置時，建置方案**) 才能開啟**加入檢視**對話 」 或 「 無類別會出現在**檢視資料類別**下拉式清單。


1. 以滑鼠右鍵按一下程式碼編輯器中的 index （） 方法，然後選取功能表選項**加入檢視**（請參閱圖 9）。
2. 在 新增檢視 對話方塊中，確認 核取方塊標示為**建立強型別檢視**已核取。
3. 從**檢視內容**下拉式清單中，選取值*清單*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。
5. 按一下 [新增] 按鈕來建立新檢視 （請參閱圖 10）。

完成這些步驟之後，新的檢視，名為 Index.aspx 便加入 Views\Home 資料夾中。 列出的 3 會包含索引檢視的內容。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**圖 09**： 從控制器動作中加入的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**圖 10**： 使用 [新增檢視] 對話方塊建立新的檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

索引檢視會顯示所有電影記錄的 HTML 資料表中的電影資料庫資料表。 此檢視包含 For each 逐一查看每個影片 ViewData.Model 屬性所代表。 如果您按下 F5 鍵執行應用程式，您會看到圖 11 網頁。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**圖 11**: 索引檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>建立新的資料庫資料錄

我們在上一節中所建立的索引檢視包含用於建立新的資料庫記錄的連結。 現在讓我們實作的邏輯和建立檢視建立新的電影資料庫記錄所需。

主控制器包含兩個方法，名為 create （）。 第一個 create （） 方法沒有任何參數。 這個 create （） 方法的多載用來顯示的 HTML 表單建立新的電影資料庫記錄。

第二個 create （） 方法具有 FormCollection 參數。 建立新的電影的 HTML 表單張貼至伺服器時，會呼叫這個 create （） 方法的多載。 請注意這個第二個 create （） 方法有 AcceptVerbs 屬性，使方法呼叫除非 HTTP Post 作業。

更新 HomeController 類別中列出的 4 中已修改這個第二個 create （） 方法。 新版本的 create （） 方法會接受影片參數，並包含電影資料庫資料表中插入新的電影的邏輯。

> [!NOTE] 
> 
> 請注意繫結屬性。 因為我們不想要更新的影片識別碼屬性，從 HTML 表單，我們需要明確地排除這個屬性。


**列出 4 – Controllers\HomeController.vb （已修改的建立方式）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio 可讓您輕鬆建立的表單建立新的電影資料庫記錄 （請參閱圖 12）。 請依照下列步驟：

1. 以滑鼠右鍵按一下程式碼編輯器中的 create （） 方法，然後選取功能表選項**加入檢視**。
2. 請確認此核取方塊標示為**建立強型別檢視**已核取。
3. 從**檢視內容**下拉式清單中，選取值*建立*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。
5. 按一下**新增**按鈕以建立新的檢視。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**圖 12**： 加入建立檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio 檢視列出 5 中會自動產生。 這個檢視包含 HTML 表單，其中包含欄位對應至每個影片類別的屬性。

**列出 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> [新增檢視] 對話方塊所產生的 HTML 格式產生 Id 的表單欄位。 因為識別碼資料行是識別資料行，所以我們不需要這個表單欄位，您可以安全地將它移除。


加入建立檢視之後，您可以新增電影記錄至資料庫。 按 F5 鍵執行應用程式，然後按一下 建立新的連結，以查看圖 13 中的表單。 如果您填寫並提交表單時，會建立新的電影資料庫記錄。

請注意，自動取得表單驗證。 如果沒有輸入電影，發行日期，或是您輸入無效的發行日期，然後會重新顯示表單，並發行日期 欄位會反白顯示。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**圖 13**： 建立新的電影資料庫記錄 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>編輯現有的資料庫記錄

上一節中，我們將討論如何列出，並建立新的資料庫記錄。 在這最後一節中，我們會討論如何編輯現有的資料庫記錄。

首先，我們需要產生的編輯表單。 這個步驟很簡單，因為 Visual Studio 會產生的編輯表單為我們自動。 在 Visual Studio 程式碼編輯器中開啟 HomeController.vb 類別，並遵循下列步驟：

1. 在程式碼編輯器的 Edit() 方法上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱圖 14）。
2. 核取方塊標示為**建立強型別檢視**。
3. 從**檢視內容**下拉式清單中，選取值*編輯*。
4. 從**檢視資料類別**下拉式清單中，選取值*MovieApp.Movie*。
5. 按一下**新增**按鈕以建立新的檢視。

完成這些步驟，新增名為 Edit.aspx Views\Home 資料夾檢視。 這個檢視包含編輯電影錄製之 HTML 表單。


[![新增專案 對話方塊](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**圖 14**： 加入編輯檢視 ([按一下以檢視完整大小的影像](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> 編輯檢視包含 HTML 表單欄位對應到電影識別碼屬性。 您不想讓使用者編輯 Id 屬性的值，您應該移除此表單欄位。


最後，我們需要修改主控制器，讓它支援 編輯資料庫記錄。 更新的 HomeController 類別被包含在程式碼範例 6。

**列出 6 – Controllers\HomeController.vb （編輯方法）**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

在列出 6 中，我已將額外的邏輯加入到 Edit() 方法的兩個多載。 第一個 Edit() 方法會傳回對應的電影資料庫記錄，Id 參數傳遞給方法。 第二個多載會在資料庫中執行電影錄製到的更新。

請注意，您必須擷取原始的影片，然後呼叫 ApplyPropertyChanges()，若要更新資料庫中現有的電影。

## <a name="summary"></a>總結

本教學課程的目的是經驗的要提供您的建置 ASP.NET MVC 應用程式。 希望您會發現建置 ASP.NET MVC web 應用程式是非常類似於建置 Active Server Pages 或 ASP.NET 應用程式的體驗。

在本教學課程中，我們會檢查 ASP.NET MVC 架構的最基本功能。 在未來的教學課程中，我們深入主題，例如控制器、 控制器的動作、 檢視、 檢視資料和 HTML helper。

>[!div class="step-by-step"]
[上一步](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
