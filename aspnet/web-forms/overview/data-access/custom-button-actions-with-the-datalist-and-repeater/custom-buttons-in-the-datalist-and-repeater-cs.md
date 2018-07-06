---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: 自訂按鈕在 DataList 與重複項 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將建置的介面，用以在系統中，列出分類，且每個分類都提供按鈕，以顯示其 associ Repeater...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 04dc12ed20fcda0b4074add065022c42343e5ffc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822117"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>在 DataList 與重複項 (C#) 中自訂按鈕
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe)或[下載 PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> 在本教學課程中，我們將建置在系統中，列出類別，而提供的按鈕，以顯示其相關聯的產品使用 BulletedList 控制每個類別會使用重複項的介面。


## <a name="introduction"></a>簡介

在過去十七 DataList 與重複項教學課程中，我們已建立唯讀的範例和編輯和刪除範例。 若要利於進行編輯和刪除的資料清單中的功能，我們新增至 DataList 的按鈕`ItemTemplate`，按一下時，造成回傳並引發對應的按鈕 s DataList 事件`CommandName`屬性。 比方說，加入至按鈕`ItemTemplate`與`CommandName`編輯屬性值會導致 DataList s`EditCommand`引發回傳; 一個包含`CommandName`刪除引發`DeleteCommand`。

此外若要編輯和刪除按鈕，DataList 與重複項控制項也可以包含按鈕、 Linkbutton 或 ImageButtons，按一下時，執行一些自訂的伺服器端邏輯。 在本教學課程中，我們將建置介面來列出系統中的類別使用的重複項。 針對每個類別，Repeater 將包含按鈕以顯示類別目錄相關聯的產品使用 BulletedList 控制項 （請參閱 圖 1）。


[![按一下 顯示產品連結會顯示類別 s 中的產品項目符號清單](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**圖 1**： 按一下 顯示產品連結會顯示在項目符號清單中的 s 產品類別目錄 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>步驟 1： 加入自訂按鈕的教學課程 Web 網頁

我們看看如何新增自訂按鈕之前，讓先花點時間在網站專案中，我們需要在此教學課程中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`CustomButtonsDataListRepeater`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的下列兩個 ASP.NET 頁面`Site.master`主版頁面：

- `Default.aspx`
- `CustomButtons.aspx`


![加入 ASP.NET 網頁的自訂按鈕相關教學課程](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁的自訂按鈕相關教學課程


在其他資料夾，例如`Default.aspx`在`CustomButtonsDataListRepeater`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 將此使用者控制項加入`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


最後，將頁面新增項目，以作為`Web.sitemap`檔案。 具體來說，使用 DataList 與重複項分頁和排序之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入及刪除教學課程的項目。


![網站導覽現在包含項目自訂按鈕教學課程](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**圖 4**： 網站地圖現在包含項目自訂按鈕教學課程


## <a name="step-2-adding-the-list-of-categories"></a>步驟 2： 加入類別目錄的清單

我們要建立列出所有的類別，以及顯示產品 LinkButton Repeater 本教學課程中，按一下，便會顯示相關聯的分類的產品項目符號清單中。 可讓第一次建立簡單的 Repeater 列出系統中的類別。 首先開啟`CustomButtons.aspx`頁面中`CustomButtonsDataListRepeater`資料夾。 拖曳 Repeater 從工具箱拖曳至設計工具和設定其`ID`屬性設`Categories`。 接下來，建立新的資料來源控制項從 Repeater s 智慧標籤。 具體來說，建立名為的新 ObjectDataSource 控制項`CategoriesDataSource`，選取其資料來源`CategoriesBLL`類別的`GetCategories()`方法。


[![設定為使用 CategoriesBLL 類別的 GetCategories() 方法的 ObjectDataSource](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories()`方法 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


不像 DataList 控制項，Visual Studio 會建立預設`ItemTemplate`根據資料來源，Repeater 的範本必須以手動方式定義。 此外，Repeater 的範本必須建立和編輯以宣告方式 （也就是沒有 s 沒有編輯的範本選項中 Repeater s 智慧標籤）。

按一下左下角的 [來源] 索引標籤，然後加入`ItemTemplate`顯示中的類別目錄的名稱`<h3>`項目和其描述在段落標記，包括`SeparatorTemplate`顯示水平尺規 (`<hr />`) 之間類別目錄。 也將新增 LinkButton 其`Text`屬性設定為顯示的產品。 完成這些步驟之後，頁面 s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

[圖 6] 顯示頁面透過瀏覽器檢視時。 會列出每個類別目錄名稱和描述。 顯示產品按鈕，按一下時，會導致回傳，但還未執行任何動作。


[![每個類別目錄名稱和描述會顯示，以及顯示產品 LinkButton](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**圖 6**： 每個類別目錄名稱和描述會顯示，以及顯示產品 LinkButton ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>步驟 3： 執行伺服器端邏輯時顯示產品 LinkButton 已按下

只要按一下按鈕、 LinkButton 或在範本中 DataList 或 Repeater 內 ImageButton 時，就會發生回傳和 DataList 或 Repeater 的`ItemCommand`引發事件。 除了`ItemCommand`事件，DataList 控制項也可能引發另一個、 更特定的事件，如果 [s] 按鈕`CommandName`屬性設定為其中一個保留的字串 （刪除、 編輯、 取消、 更新或選取），但`ItemCommand`事件是否*一律*引發。

DataList 或 Repeater 內按一下按鈕時，通常我們需要傳遞所按的按鈕 （在案例中可能有多個控制項，例如兩個編輯中的按鈕和 [刪除] 按鈕） 和其他資訊 （例如主索引鍵值所按之按鈕的項目）。 按鈕、 LinkButton 和 ImageButton 提供兩個屬性，其值會傳遞給`ItemCommand`事件處理常式：

- `CommandName` 字串，通常用來識別每個範本中的按鈕
- `CommandArgument` 常用來保存部分的資料欄位，例如主索引鍵值的值

對於此範例中，設定 LinkButton s`CommandName`屬性設為 ShowProducts 並繫結的目前記錄 s 主索引鍵值`CategoryID`來`CommandArgument`屬性使用的資料繫結語法`CategoryArgument='<%# Eval("CategoryID") %>'`。 指定這兩個屬性之後, 的 LinkButton s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

當按一下按鈕時，會發生回傳和 DataList 或 Repeater 的`ItemCommand`引發事件。 事件處理常式傳遞 [s] 按鈕`CommandName`和`CommandArgument`值。

建立事件處理常式，如 Repeater s`ItemCommand`事件並記下第二個參數傳遞至事件處理常式 (名為`e`)。 此第二個參數的類型是[ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx)並具有下列四個屬性：

- `CommandArgument` s 的已按下按鈕值`CommandArgument`屬性
- `CommandName` [s] 按鈕的值`CommandName`屬性
- `CommandSource` 已按下按鈕控制項的參考
- `Item` 參考[ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) ，其中包含所按的按鈕; 繫結至 Repeater 每一筆記錄，會顯示為 `RepeaterItem`

因為選取的類別 s`CategoryID`透過傳入`CommandArgument`屬性中，我們可以取得與所選類別目錄中相關聯的產品集`ItemCommand`事件處理常式。 這些產品可以再繫結至在 BulletedList 控制項`ItemTemplate`(這點我們將尚未 ve)。 所有剩餘，然後是新增 BulletedList，參考在`ItemCommand`事件處理常式，並繫結至其產品的所選的分類中，我們將會處理在步驟 4 中的集合。

> [!NOTE]
> DataList s`ItemCommand`事件處理常式傳遞類型的物件[ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)，提供相同的四個屬性`RepeaterCommandEventArgs`類別。


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>步驟 4： 在項目符號清單顯示選取的類別目錄產品

Repeater s 中，可以顯示選取的類別的產品`ItemTemplate`使用任何數目的控制項。 我們可以新增另一個巢狀 Repeater、 DataList、 DropDownList、 GridView，等等。 因為我們想要為項目符號清單顯示的產品，不過，我們將使用 BulletedList 控制項。 返回`CustomButtons.aspx`頁面上 s 宣告式標記，加入 BulletedList 控制項`ItemTemplate`顯示產品 LinkButton 之後。 設定 BulletedLists s`ID`至`ProductsInCategory`。 BulletedList 顯示透過指定資料欄位的值`DataTextField`屬性，因為這個控制項都具有與它繫結，請將設定的產品資訊`DataTextField`屬性設`ProductName`。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

在 `ItemCommand`事件處理常式中，參考此控制項使用`e.Item.FindControl("ProductsInCategory")`並將它繫結至選取的類別相關聯的產品集。


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

在執行中的任何動作之前`ItemCommand`事件處理常式，它必須先檢查內送值的 s `CommandName`。 由於`ItemCommand`時，會引發事件處理常式*任何*按一下按鈕時，如果有多個按鈕，在範本使用`CommandName`分辨要採取什麼動作的值。 檢查`CommandName`以下是毫無意義，因為我們只需要一個按鈕，但它是很好的習慣，到表單。 下一步`CategoryID`所選類別擷取自`CommandArgument`屬性。 然後參考和繫結至結果的 BulletedList 控制項範本中的`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。

在先前的教學課程使用的按鈕內的資料清單，例如[概觀的編輯和刪除資料 DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)，我們判斷指定的項目，透過的主索引鍵值`DataKeys`集合。 雖然這個方法也適用於 DataList，重複項並沒有`DataKeys`屬性。 相反地，我們必須使用另一個方法來提供這類的主索引鍵值，透過 [s] 按鈕`CommandArgument`屬性或方法是指派至隱藏標籤 Web 控制項範本內的主索引鍵值，並讀取其值在`ItemCommand`事件處理常式使用`e.Item.FindControl("LabelID")`。

完成之後`ItemCommand`事件處理常式，請花一點時間來測試此頁面在瀏覽器中。 如 [圖 7] 所示，按一下顯示的產品連結造成回傳並 BulletedList 顯示選取之類別目錄的產品。 此外，請注意，會保留這項產品資訊，即使按一下其他類別顯示產品連結。

> [!NOTE]
> 如果您想要修改這份報表的行為，列出一次只有一個類別目錄的產品的方式，只要將設定 BulletedList 控制項 s`EnableViewState`屬性設`False`。


[![BulletedList 用來顯示選取的類別目錄的產品](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**圖 7**: BulletedList 用來顯示選取的類別目錄的產品 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>總結

DataList 與重複項控制項可以包含任意數目的選項按鈕、 Linkbutton 或 ImageButtons 其範本內。 這類的按鈕，按一下時，會造成回傳並引發`ItemCommand`事件。 若要自訂的伺服器端動作相關聯的按鈕被按下，建立的事件處理常式`ItemCommand`事件。 這個事件處理常式中先檢查傳入`CommandName`值，以判斷所按的按鈕。 （選擇性） 透過 [s] 按鈕提供額外的資訊`CommandArgument`屬性。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dennis Patterson 邀請。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](custom-buttons-in-the-datalist-and-repeater-vb.md)
