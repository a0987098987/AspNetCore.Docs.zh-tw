---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
title: 在 DataList 和中繼器 (VB) 中的自訂按鈕 |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們將建立一種介面，用於在系統中，列出分類，且每個分類提供按鈕，顯示其 associ 中繼器...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1afdb14d-6e49-4e1f-aead-2934730d472e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e470590252102c486bb72ff46f516180aa09ba8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876081"
---
<a name="custom-buttons-in-the-datalist-and-repeater-vb"></a>在 DataList 和中繼器 (VB) 中的自訂按鈕
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_VB.exe)或[下載 PDF](custom-buttons-in-the-datalist-and-repeater-vb/_static/datatutorial46vb1.pdf)

> 在本教學課程中，我們會建置用來在系統中，列出分類，且提供按鈕，以顯示其相關聯的產品使用 BulletedList 控制每個分類的中繼器的介面。


## <a name="introduction"></a>簡介

在過去十七 DataList 和中繼器教學課程，我們已建立唯讀的範例和編輯和刪除範例。 若要簡化編輯和刪除功能在 DataList 內，我們加入按鈕 DataList s `ItemTemplate` ，按一下時，導致回傳，並引發對應的按鈕 s DataList 事件`CommandName`屬性。 例如，加入至按鈕`ItemTemplate`與`CommandName`編輯屬性值會導致 DataList s`EditCommand`成發生回傳時引發一個`CommandName`刪除引發`DeleteCommand`。

此外編輯和刪除按鈕、 DataList 和中繼器控制項也可以包含按鈕、 LinkButtons 或 ImageButtons，按一下時，執行一些自訂的伺服器端邏輯。 在本教學課程中，我們會建置會使用中繼器列出系統中的類別介面。 為每個類別目錄，中繼器將包含按鈕以顯示類別目錄相關聯產品使用 BulletedList 控制項 （請參閱圖 1）。


[![按一下 顯示產品連結會顯示在項目符號清單中的類別目錄的產品](custom-buttons-in-the-datalist-and-repeater-vb/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image1.png)

**圖 1**： 按一下 顯示產品連結會顯示項目符號清單中的 s 產品類別目錄 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-vb/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>步驟 1： 加入自訂按鈕的教學課程 Web 網頁

我們會審視如何新增自訂按鈕之前，可讓 s 先花點時間在我們的網站專案，我們需要在此教學課程中建立 ASP.NET 網頁。 加入新的資料夾，名為啟動`CustomButtonsDataListRepeater`。 接下來，將下列兩個 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `CustomButtons.aspx`


![如需自訂的按鈕相關教學課程中加入 ASP.NET 網頁](custom-buttons-in-the-datalist-and-repeater-vb/_static/image4.png)

**圖 2**： 加入 ASP.NET 網頁的自訂按鈕相關教學課程


如同在其他資料夾，`Default.aspx`中`CustomButtonsDataListRepeater`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 將此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](custom-buttons-in-the-datalist-and-repeater-vb/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image5.png)

**圖 3**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-vb/_static/image7.png))


最後，將頁面加入做為項目至`Web.sitemap`檔案。 具體來說，加入下列標記之後的分頁和排序 DataList 與中繼器`<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入和刪除教學課程的項目。


![站台地圖現在包含之項目的自訂按鈕教學課程](custom-buttons-in-the-datalist-and-repeater-vb/_static/image8.png)

**圖 4**： 網站導覽現在包含之項目的自訂按鈕教學課程


## <a name="step-2-adding-the-list-of-categories"></a>步驟 2： 加入類別目錄的清單

我們要建立列出所有類別，以及顯示的產品 LinkButton 中繼器本教學課程中，按一下時，會顯示相關的類別的產品項目符號清單中。 可讓第一次建立類別目錄會列出系統中的簡單中繼器 s。 先開啟`CustomButtons.aspx`頁面`CustomButtonsDataListRepeater`資料夾。 從工具箱拖曳至設計工具和設定拖曳中繼器其`ID`屬性`Categories`。 接下來，建立新的資料來源控制項從中繼器 s 智慧標籤。 具體而言，建立新的 ObjectDataSource 控制項，名為`CategoriesDataSource`選取其資料從`CategoriesBLL`類別的`GetCategories()`方法。


[![設定為使用 CategoriesBLL 類別的 GetCategories() 方法 ObjectDataSource](custom-buttons-in-the-datalist-and-repeater-vb/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 s`GetCategories()`方法 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-vb/_static/image11.png))


DataList 控制項，Visual Studio 會建立預設值與`ItemTemplate`根據資料來源，s 中繼器範本必須以手動方式定義。 此外，s 中繼器範本必須進行建立或編輯以宣告方式 （也就是沒有 s 沒有編輯的範本選項在中繼器 s 智慧標籤）。

按一下左上角的 [來源] 索引標籤上，新增`ItemTemplate`顯示中的類別目錄的名稱`<h3>`項目和其描述在段落標記，包括`SeparatorTemplate`顯示水平尺規 (`<hr />`) 之間類別目錄。 也新增 LinkButton 其`Text`屬性設定為顯示的產品。 完成這些步驟之後，頁面 s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample2.aspx)]

圖 6 顯示透過瀏覽器檢視時的頁面。 會列出每個分類名稱和描述。 顯示產品按鈕，按一下時，會導致回傳，但尚未執行任何動作。


[![每個類別名稱和描述會顯示，並顯示產品 LinkButton](custom-buttons-in-the-datalist-and-repeater-vb/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image12.png)

**圖 6**： 每個類別目錄名稱和描述會顯示，並顯示產品 LinkButton ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-vb/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>步驟 3： 執行伺服器端邏輯時顯示產品 LinkButton 已按下

每當按鈕、 LinkButton 或在 DataList 或中繼器範本內的 imagebutton 已按下時，就會發生回傳和 DataList 或中繼器的`ItemCommand`事件引發。 除了`ItemCommand`事件，控制項也可能會引發另一個、 更特定的事件，如果在 DataList 按鈕 s`CommandName`屬性設定為其中一個保留的字串 （刪除、 編輯、 取消、 更新或選取），但`ItemCommand`事件是*一律*引發。

在 DataList 或中繼器中按一下按鈕時，有時候我們需要將傳遞的 button 已按下 （在案例中可能會有多個按鈕，在控制項內，例如兩個的編輯和刪除 按鈕） 和其他資訊 （例如主索引鍵值的 button 已按下的項目）。 按鈕、 LinkButton 和 ImageButton 提供兩個屬性，其值會傳遞給`ItemCommand`事件處理常式：

- `CommandName` 字串，通常用來識別每個範本中的按鈕
- `CommandArgument` 通常用來保留某些資料欄位，例如主索引鍵值的值

這個範例中，將設定 LinkButton s`CommandName`屬性設為 ShowProducts 並繫結的目前記錄 s 主索引鍵值`CategoryID`至`CommandArgument`屬性使用的資料繫結語法`CategoryArgument='<%# Eval("CategoryID") %>'`。 指定這兩個屬性之後, LinkButton s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample3.aspx)]

當按一下按鈕，就會發生回傳，DataList 或中繼器的`ItemCommand`事件引發。 此事件處理常式會傳遞按鈕 s`CommandName`和`CommandArgument`值。

建立事件處理常式的中繼器 s`ItemCommand`事件，請注意第二個參數傳遞至事件處理常式 (名稱為`e`)。 這個第二個參數的型別是[ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx)而且具有下列四個屬性：

- `CommandArgument` 按下按鈕的值`CommandArgument`屬性
- `CommandName` 按鈕的值`CommandName`屬性
- `CommandSource` 已按下按鈕控制項的參考
- `Item` 若要參考[ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx)包含已按下的按鈕; 繫結至中繼器每一筆記錄，會顯示為 `RepeaterItem`

因為選取的類別 s`CategoryID`透過傳入`CommandArgument`屬性，我們可以得到的選取類別目錄中相關聯的產品集`ItemCommand`事件處理常式。 這些產品則會至設定 BulletedList 的控制項繫結`ItemTemplate`(哪些我們將尚未發生)。 所有維持狀態，然後是加入 BulletedList，參考在`ItemCommand`事件處理常式，並繫結至其產品，我們將會處理在步驟 4 中所選取類別目錄的集合。

> [!NOTE]
> DataList s`ItemCommand`事件處理常式會傳遞類型的物件[ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx)，提供相同的四個內容`RepeaterCommandEventArgs`類別。


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>步驟 4： 在項目符號清單中顯示選取的類別的產品

選取的類別的產品可以顯示在中繼器的`ItemTemplate`使用任何數目的控制項。 我們可以加入另一個巢狀中繼器、 DataList、 DropDownList、 GridView 中等等。 因為我們想要為項目符號清單中顯示的產品，不過，我們會使用設定 BulletedList 控制項。 返回`CustomButtons.aspx`頁面 s 宣告式標記中，將設定 BulletedList 控制項加入`ItemTemplate`之後顯示產品 LinkButton。 設定 BulletedLists s`ID`至`ProductsInCategory`。 BulletedList 顯示透過指定資料欄位的值`DataTextField`屬性; 因為此控制項能與它繫結，將產品資訊`DataTextField`屬性`ProductName`。


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample4.aspx)]

在`ItemCommand`事件處理常式，參考這個控制項使用`e.Item.FindControl("ProductsInCategory")`並將它繫結至選取的類別相關聯的產品集。


[!code-vb[Main](custom-buttons-in-the-datalist-and-repeater-vb/samples/sample5.vb)]

然後再執行任何動作中的`ItemCommand`事件處理常式，它 s 審慎的作法是先檢查傳入的值`CommandName`。 因為`ItemCommand`時引發事件處理常式*任何*按一下按鈕時，如果範本使用中有多個按鈕`CommandName`分辨要採取的動作值。 檢查`CommandName`以下是毫無意義，因為我們只需要單一按鈕，但它是個好習慣至表單。 下一步`CategoryID`所選類別擷取自`CommandArgument`屬性。 然後參考範本中的設定 BulletedList 控制項並繫結至結果的`ProductsBLL`類別的`GetProductsByCategoryID(categoryID)`方法。

在這類的使用中 DataList，按鈕的上一個教學課程[概觀的編輯和刪除的資料在 DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)，我們判斷出指定的項目，透過主索引鍵值`DataKeys`集合。 雖然這種方法適用於在 DataList，中繼器並沒有`DataKeys`屬性。 相反地，我們必須使用另一個方法來提供這類的主索引鍵值，透過 [s] 按鈕`CommandArgument`屬性或藉由將主索引鍵值指派給隱藏標籤 Web 控制項範本內，並讀取其值在`ItemCommand`事件處理常式使用`e.Item.FindControl("LabelID")`。

完成之後`ItemCommand`事件處理常式，請花一點時間測試的瀏覽器中的此頁面。 如圖 7 所示，按一下顯示的產品連結導致回傳並設定 BulletedList 中顯示所選取類別目錄的產品。 此外，請注意，就會維持這個產品資訊，即使按一下其他類別顯示的產品連結。

> [!NOTE]
> 如果您想要修改這份報表的行為，使得只有一個類別目錄的產品列出一次，只要設定 BulletedList 控制項 s`EnableViewState`屬性`False`。


[![BulletedList 用來顯示選取的類別目錄的產品](custom-buttons-in-the-datalist-and-repeater-vb/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-vb/_static/image15.png)

**圖 7**: BulletedList 用來顯示選取的類別目錄的產品 ([按一下以檢視完整大小的影像](custom-buttons-in-the-datalist-and-repeater-vb/_static/image17.png))


## <a name="summary"></a>總結

DataList 和中繼器控制項可以包含任意數目的按鈕、 LinkButtons 或 ImageButtons 其範本中。 這類的按鈕，按一下時，會導致回傳，並引發`ItemCommand`事件。 自訂伺服器端動作與在按下的按鈕，建立事件處理常式`ItemCommand`事件。 在此事件處理常式先檢查傳入`CommandName`值，以判斷哪一個按鈕已按下。 其他資訊 （選擇性） 可以透過 [s] 按鈕來提供`CommandArgument`屬性。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Dennis Patterson。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](custom-buttons-in-the-datalist-and-repeater-cs.md)
