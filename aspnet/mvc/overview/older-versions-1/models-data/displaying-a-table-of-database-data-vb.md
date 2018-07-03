---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: 顯示資料表的資料庫資料 (VB) |Microsoft Docs
author: microsoft
description: 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會示範兩種格式的一組資料庫記錄，在 HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: f5d9062d901f28b1d64f400e13ed5eb0ca788ab8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362488"
---
<a name="displaying-a-table-of-database-data-vb"></a>顯示資料表的資料庫資料 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會示範兩種格式的一組 HTML 表格中的資料庫記錄。 首先，我會示範如何設定格式直接在檢視內的資料庫記錄。 接下來，我會示範格式化資料庫記錄時，如何充分善用部分。


本教學課程的目標是要說明如何顯示 HTML 表格的資料庫資料，以及在 ASP.NET MVC 應用程式中。 首先，您會了解如何使用隨附於 Visual Studio scaffolding 工具來產生自動顯示一組記錄的檢視。 接下來，您會了解如何格式化資料庫記錄時，使用部分做為範本。

## <a name="create-the-model-classes"></a>建立模型類別

我們要顯示的電影資料庫資料表中的記錄集。 電影資料庫資料表包含下列資料行：

<a id="0.4_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(200 | False |
| 總監 | NVarchar(50) | False |
| DateReleased | DateTime | False |


若要代表在 ASP.NET MVC 應用程式中的電影資料表，我們要建立模型類別。 本教學課程中，我們會使用 Microsoft Entity Framework 來建立我們的模型類別。

> [!NOTE] 
> 
> 在本教學課程中，我們會使用 Microsoft Entity Framework。 不過，務必了解，您可以使用各種不同的技術來包括 LINQ to SQL、 NHibernate 或 ADO.NET 的 ASP.NET MVC 應用程式的資料庫互動。


請遵循下列步驟來啟動 Entity Data Model 精靈：

1. 在 [方案總管] 視窗，然後選取功能表選項中的 [Models] 資料夾上按一下滑鼠右鍵**新增]、 [新項目**。
2. 選取 **資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。
3. 將您的資料模型命名*MoviesDBModel.edmx*然後按一下**新增** 按鈕。

按一下 [新增] 按鈕之後，實體資料模型精靈] 會出現 （請參閱 [圖 1）。 請遵循下列步驟來完成精靈：

1. 在 **選擇模型內容**步驟中，選取**從資料庫產生**選項。
2. 在 **選擇資料連接**步驟中，使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連線設定。 按一下 **下一步**  按鈕。
3. 在 **選擇您的資料庫物件**步驟中，展開 資料表 節點中，選取 電影資料表。 輸入的命名空間*模型*然後按一下**完成** 按鈕。


[![建立 LINQ to SQL 類別](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**圖 01**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image2.png))


Entity Data Model 精靈完成之後，就會開啟實體資料模型設計工具。 設計工具應該會顯示電影實體 （請參閱 圖 2）。


[![實體資料模型設計工具](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**圖 02**: 實體資料模型設計工具 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image4.png))


我們需要進行一項變更，再繼續進行。 實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。 因為我們將使用電影類別來代表特定的影片，我們需要修改的類別名稱*電影*而不是*電影*（單數而複數）。

按兩下設計工具介面上類別名稱，並從電影時使用的類別名稱變更為電影。 完成此變更之後，按一下**儲存**按鈕 （磁片圖示） 來產生電影類別。

## <a name="create-the-movies-controller"></a>建立電影控制器

既然我們已代表我們的資料庫記錄的方式，我們可以建立傳回的電影集合的控制站。 Visual Studio 方案總管 視窗中，以滑鼠右鍵按一下 控制器 資料夾，然後選取功能表選項**新增、 控制站**（請參閱 圖 3）。


[![[加入控制器] 功能表](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**圖 03**: [新增控制器] 功能表中 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image6.png))


當**新增控制器**對話方塊隨即出現，請輸入控制器名稱 MovieController （請參閱 圖 4）。 按一下 **新增**按鈕以新增新的控制器。


[![[新增控制器] 對話方塊](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**[圖 04**: 新增控制器] 對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image8.png))


我們需要修改，使它傳回的一組資料庫記錄由電影控制器的 index （） 動作。 修改控制器，讓它看起來像 列表 1 中的控制站。

**列表 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

在 [列表 1] MoviesDBEntities 類別用來代表 MoviesDB 資料庫。 運算式*實體。MovieSet.ToList()* 電影資料庫資料表中傳回所有影片的集合。

## <a name="create-the-view"></a>建立檢視

顯示 HTML 表格中的一組資料庫記錄的最簡單方式是 scaffolding 的利用 Visual Studio 所提供。

選取功能表選項來建置您的應用程式**建置時，建置方案**。 您必須建置您的應用程式，才能開啟**加入檢視**對話 」 或 「 資料類別就不會出現在對話方塊中。

以滑鼠右鍵按一下 index （） 動作，然後選取功能表選項**加入檢視**（請參閱 圖 5）。


[![新增檢視](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**圖 05**： 新增檢視 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image10.png))


在 [**加入檢視**] 對話方塊中，檢查標示的核取方塊**建立強型別檢視**。 選取的電影類別**檢視資料類別**。 選取 *清單*作為**檢視內容**（請參閱 圖 6）。 選取這些選項會產生強型別 檢視顯示的電影清單。


[![[新增檢視] 對話方塊](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**圖 06**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image12.png))


按一下 [之後**新增**列表 2 中的檢視] 按鈕，會自動產生。 這個檢視包含程式碼，以逐一查看集合的電影，並顯示每個電影的屬性。

**列表 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

您可以選取功能表選項來執行應用程式**偵錯，請啟動偵錯**（或按下 F5 鍵）。 執行應用程式會啟動 Internet Explorer。 如果您導覽至 /Movie URL，您會看到 [圖 7] 中的頁面。


[![之電影資料表](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**圖 07**： 的電影資料表 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image14.png))


如果您不喜歡方格圖 7 中的資料庫記錄的外觀相關的任何項目然後您就可以直接修改 索引 檢視。 例如，您可以變更*DateReleased*標頭*發行日期*藉由修改 [索引] 檢視。

## <a name="create-a-template-with-a-partial"></a>使用部分建立範本

當檢視取得太複雜時，最好先啟動分成部分檢視。 使用部分，可讓您檢視了解和維護變得更加容易。 我們將會建立部分我們可以使用做為範本來設定每個電影資料庫記錄的格式。

請遵循下列步驟來建立部分：

1. Views\Movie 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。
2. 選取標示的核取方塊*建立部分檢視 (.ascx)*。
3. 命名部分*MovieTemplate*。
4. 選取標示的核取方塊**建立強型別檢視**。
5. 選取將電影當作*檢視資料類別*。
6. 選取 為空白*檢視內容*。
7. 按一下 **新增**按鈕，將部分加入至您的專案。

完成這些步驟之後，修改部分 MovieTemplate 看起來像是列表 3。

**列表 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

在 列表 3 部分包含記錄的單一資料列的範本。

在 列表 4 中已修改的 索引 檢視會使用 MovieTemplate 部分。

**列表 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

列表 4 中的檢視包含一個 For Each 迴圈來逐一查看所有的影片。 每一部電影，部分 MovieTemplate 用來格式化影片。 MovieTemplate 會藉由呼叫 RenderPartial() helper 方法轉譯。

修改過的 [索引] 檢視會呈現相同的 HTML 資料表的資料庫記錄。 不過，已大幅簡化檢視。


RenderPartial() 方法是不同於大部分其他的 helper 方法，因為它不會傳回字串。 因此，您必須呼叫 RenderPartial() 方法使用&lt;%html.renderpartial()&gt;而不是&lt;%= Html.RenderPartial() %&gt;。


## <a name="summary"></a>總結

本教學課程的目標是要說明如何顯示一組資料庫記錄，以及在 HTML 表格中。 首先，您已了解如何利用 Microsoft Entity Framework，從控制器動作傳回一組資料庫記錄。 接下來，您已了解如何使用 Visual Studio scaffolding 來產生自動顯示的項目集合的檢視。 最後，您已了解如何利用部分簡化檢視。 您已了解如何使用部分做為範本，以便您可以格式化每個資料庫的記錄。

> [!div class="step-by-step"]
> [上一頁](creating-model-classes-with-linq-to-sql-vb.md)
> [下一頁](performing-simple-validation-vb.md)
