---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: 顯示資料表的資料庫資料 (VB) |Microsoft 文件
author: microsoft
description: 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會顯示兩種格式的 HTML 中的資料庫記錄的一組 ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872269"
---
<a name="displaying-a-table-of-database-data-vb"></a>顯示資料表的資料庫資料 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> 在本教學課程中，我會示範兩種方法可以顯示一組資料庫記錄。 我會顯示兩種格式的 HTML 表格中的資料庫記錄的一組方法。 首先，我會顯示如何設定格式直接在檢視中的資料庫記錄。 接下來，我會示範如何您可以利用 partials 格式化資料庫記錄時。


本教學課程的目標是要說明如何在 ASP.NET MVC 應用程式中顯示 HTML 表格的資料庫資料。 首先，您會了解如何使用 Visual Studio 中所含的 scaffolding 工具產生自動顯示一組記錄的檢視。 接下來，您會學習如何格式化資料庫記錄時，使用了部分做為範本。

## <a name="create-the-model-classes"></a>建立模型類別

我們將會顯示一組的電影資料庫資料表中的記錄。 電影資料庫資料表包含下列資料行：

<a id="0.4_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | Int | False |
| 標題 | Nvarchar(200) | False |
| 導向器 | Nvarchar （50) | False |
| DateReleased | DateTime | False |


若要代表電影的資料表在 ASP.NET MVC 應用程式中，我們要建立模型類別。 在本教學課程中，我們可以使用 Microsoft Entity Framework 來建立模型類別。

> [!NOTE] 
> 
> 在本教學課程中，我們會使用 Microsoft Entity Framework。 不過，務必了解，您可以使用各種不同的技術與 ASP.NET MVC 應用程式，包括 LINQ to SQL、 NHibernate 或 ADO.NET 的資料庫互動。


請遵循下列步驟來啟動實體資料模型精靈：

1. 以滑鼠右鍵按一下 [模型] 資料夾，在 [方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。
2. 選取**資料**類別目錄，然後選取**ADO.NET 實體資料模型**範本。
3. 將您的資料模型命名*MoviesDBModel.edmx*按一下**新增** 按鈕。

按一下 [新增] 按鈕之後，實體資料模型精靈就會出現 （請參閱圖 1）。 請遵循下列步驟來完成精靈：

1. 在**選擇模型內容**步驟中，選取**從資料庫產生**選項。
2. 在**選擇資料連接**步驟，請使用*MoviesDB.mdf*資料連接和名稱*MoviesDBEntities*連接設定。 按一下**下一步** 按鈕。
3. 在**選擇您的資料庫物件**步驟中，展開資料表節點，選取 電影資料表。 輸入的命名空間*模型*按一下**完成** 按鈕。


[![建立 LINQ to SQL 類別](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**圖 01**： 建立 LINQ to SQL 類別 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image2.png))


實體資料模型精靈完成之後，Entity Data Model Designer 隨即開啟。 在設計工具應該會顯示電影實體 （請參閱圖 2）。


[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**圖 02**: Entity Data Model Designer ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image4.png))


我們需要進行一項變更，我們繼續執行。 實體資料精靈產生模型類別，名為*電影*表示電影資料庫資料表。 由於我們將使用電影類別來代表特定的影片，我們需要修改的類別的類別名稱*影片*而不是*電影*（單數而複數）。

按兩下設計工具介面上的類別名稱，並將電影從類別的名稱，變更 電影。 進行這項變更之後, 按**儲存**按鈕 （磁片圖示），以產生影片類別。

## <a name="create-the-movies-controller"></a>建立電影控制站

現在，我們已經呈現我們的資料庫記錄的方式，我們可以建立傳回之集合的電影的控制站。 在 Visual Studio 方案總管 視窗中，以滑鼠右鍵按一下 控制器 資料夾，然後選取功能表選項**新增、 控制站**（請參閱圖 3）。


[![加入控制器 功能表](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**圖 03**: [加入控制器] 功能表中 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image6.png))


當**加入控制器**對話方塊出現，請輸入控制器名稱 MovieController （請參閱圖 4）。 按一下**新增**按鈕以加入新的控制器。


[![[加入控制器] 對話方塊](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**圖 04**: 加入控制器 對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image8.png))


我們需要修改，使它傳回一組資料庫記錄，影片控制器所公開的 index （） 動作。 修改控制器，讓它看起來像是列表 1 中的控制站。

**列出 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

列出第 1 MoviesDBEntities 類別用來代表 MoviesDB 資料庫。 運算式*實體。MovieSet.ToList()* 電影資料庫資料表中傳回的所有電影的集合。

## <a name="create-the-view"></a>建立檢視

HTML 表格中顯示的一組資料庫記錄的最簡單方式是樣板的利用 Visual Studio 所提供。

功能表選項來建立您的應用程式**建置時，建置方案**。 您必須建置應用程式，再開啟**加入檢視**對話 」 或 「 資料類別不會出現在對話方塊中。

Index （） 動作上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱圖 5）。


[![加入的檢視](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**圖 05**： 加入的檢視 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image10.png))


在**加入檢視** 對話方塊中，選取核取方塊標示為**建立強型別檢視**。 選取影片類別，做為**檢視資料類別**。 選取*清單*為**檢視內容**（請參閱圖 6）。 選取這些選項會產生強型別 檢視顯示的電影清單。


[![新增檢視對話方塊](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**圖 06**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image12.png))


按一下 之後**新增**時自動產生按鈕、 列表 2 中的檢視。 這個檢視包含逐一查看集合的影片和顯示每個影片屬性所需的程式碼。

**列出 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

您可以藉由選取功能表選項來執行應用程式**偵錯，開始偵錯**（或按下 F5 鍵）。 執行應用程式會啟動 Internet Explorer。 如果您導覽至 /Movie URL，您會看到圖 7 中的頁面。


[![資料表的電影](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**圖 07**： 電影的資料表 ([按一下以檢視完整大小的影像](displaying-a-table-of-database-data-vb/_static/image14.png))


如果您不喜歡任何有關資料庫中的記錄圖 7 格線的外觀就可以只修改索引檢視。 例如，您可以變更*DateReleased*標頭*發行日期*藉由修改索引檢視。

## <a name="create-a-template-with-a-partial"></a>使用部分建立範本

當檢視表太複雜時，最好先啟動分成 partials 的檢視。 使用 partials 可讓您更輕鬆地了解和維護您的檢視。 我們將會建立部分我們可以使用當做範本來設定每個影片資料庫記錄的格式。

請遵循下列步驟來建立部分：

1. Views\Movie 資料夾上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。
2. 核取方塊標示為*建立部分檢視 (.ascx)*。
3. 命名部分*MovieTemplate*。
4. 核取方塊標示為**建立強型別檢視**。
5. 選取將電影當作*檢視資料類別*。
6. 選取空白做為*檢視內容*。
7. 按一下**新增**按鈕，將部分加入至您的專案。

完成這些步驟之後，修改成類似列出 3 部分 MovieTemplate。

**列出 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

列出的 3 部分包含單一資料列的記錄的範本。

修改的索引檢視表中列出的 4 使用 MovieTemplate 部分。

**列出 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

列出的 4 中的檢視包含 For each 逐一查看所有的電影。 針對每個影片中，部分 MovieTemplate 用來格式化影片。 MovieTemplate 會藉由呼叫 RenderPartial() helper 方法轉譯。

修改過的索引檢視呈現資料庫記錄相同的 HTML 的表格。 不過，已大幅簡化檢視。


RenderPartial() 方法是比大多數其他 helper 方法不同，因為它不會傳回字串。 因此，您必須呼叫 RenderPartial() 方法使用&lt;%html.renderpartial()&gt;而不是&lt;%= Html.RenderPartial() %&gt;。


## <a name="summary"></a>總結

本教學課程的目的是要說明如何顯示一組資料庫記錄，以及在 HTML 表格中。 首先，您學會如何從控制器動作傳回的一組資料庫記錄，藉由運用 Microsoft Entity Framework。 接下來，您學會如何使用 Visual Studio scaffolding 產生檢視自動顯示的項目集合。 最後，您會學到如何利用部分簡化檢視。 您已學習如何使用部分做為範本，以便您可以格式化每個資料庫的記錄。

> [!div class="step-by-step"]
> [上一頁](creating-model-classes-with-linq-to-sql-vb.md)
> [下一頁](performing-simple-validation-vb.md)
