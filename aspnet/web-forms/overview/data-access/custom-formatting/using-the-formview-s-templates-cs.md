---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: 使用 FormView 的範本 (C#) |Microsoft Docs
author: rick-anderson
description: 不同 DetailsView 中，於 FormView 不包含的欄位。 相反地，FormView 轉譯使用範本。 在本教學課程中，我們將檢驗使用 F...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: cea8cad7c3e049ac5b608a51840bde98c825a90f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366724"
---
<a name="using-the-formviews-templates-c"></a>使用 FormView 的範本 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe)或[下載 PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> 不同 DetailsView 中，於 FormView 不包含的欄位。 相反地，FormView 轉譯使用範本。 在本教學課程中我們將檢驗使用 FormView 控制項來呈現較不嚴格的顯示的資料。


## <a name="introduction"></a>簡介

在最後兩個教學課程中，我們了解如何自訂使用 TemplateFields 的 GridView 和 DetailsView 控制項的輸出的內容。 TemplateFields 允許可高度自訂的特定欄位的內容，但最後 GridView 和 DetailsView 擁有而 boxy、 類似方格的外觀。 許多情況下，這類類似方格的版面配置是理想的做法，但有時需要更流暢、 較不嚴格的顯示。 顯示單一記錄，這類流暢的版面配置時，可以使用 FormView 控制項。

不同 DetailsView 中，於 FormView 不包含的欄位。 您無法加入 BoundField 或 TemplateField FormView。 相反地，FormView 轉譯使用範本。 FormView 視為包含單一的 TemplateField DetailsView 控制項。 FormView 設定支援下列範本：

- `ItemTemplate` 用來呈現特定的記錄顯示在 FormView 中
- `HeaderTemplate` 用來指定選擇性標頭資料列
- `FooterTemplate` 用來指定選擇性的頁尾資料列
- `EmptyDataTemplate` 當 FormView`DataSource`缺少任何記錄，`EmptyDataTemplate`用來取代`ItemTemplate`來呈現控制項的標記
- `PagerTemplate` 可用來為已啟用分頁的 FormViews 自訂分頁介面
- `EditItemTemplate` / `InsertItemTemplate` 用來支援這類功能的 FormViews 自訂編輯介面或插入介面

在本教學課程中我們將檢驗使用 FormView 控制項來呈現以較不嚴格的顯示的產品。 而不是讓名稱、 類別、 供應商，並依此類推，FormView 的欄位`ItemTemplate`會顯示使用的標頭項目組合這些值和`<table>`（請參閱 圖 1）。


[![在 DetailsView 中看到的類似方格的版面配置的 FormView 會中斷](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**圖 1**: FormView 中斷 Grid-Like 配置出現在 DetailsView 中 ([按一下以檢視完整大小的影像](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>步驟 1: 資料繫結至 FormView

開啟`FormView.aspx`頁面上，然後從 [工具箱] 拖曳至設計工具拖曳 FormView。 第一次新增 FormView 時它會顯示為灰色方塊，指示我們，`ItemTemplate`需要。


[![FormView 無法轉譯設計工具中，提供一個 ItemTemplate 之前](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**圖 2**： 在設計工具之前就 FormView 無法轉譯`ItemTemplate`提供 ([按一下以檢視完整大小的影像](using-the-formview-s-templates-cs/_static/image6.png))


`ItemTemplate` （透過宣告式語法中） 可以用手動方式建立，也可以藉由繫結至資料來源控制項透過設計工具的 FormView 是自動建立。 這個自動建立`ItemTemplate`包含 HTML，清單的每個欄位和標籤名稱控制其`Text`屬性繫結至該欄位的值。 這個方法也自動-建立`InsertItemTemplate`和`EditItemTemplate`，這兩者都針對每個傳回的資料來源控制項的資料欄位填入輸入控制項。

如果您想要自動建立範本，從 FormView 的智慧標籤加入新的 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。 這會建立使用 FormView `ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 從原始碼 檢視中，移除`InsertItemTemplate`和`EditItemTemplate`因為我們不想要建立 FormView 支援編輯，或尚未插入。 下一步，清除的標記內`ItemTemplate`，讓我們能夠從任何記錄。

如果您可能會相當建置`ItemTemplate`以手動的方式，您可以加入，並將它從 [工具箱] 拖曳至設計工具設定 ObjectDataSource。 不過，不需要設定 FormView 的資料來源從設計工具。 相反地，請移至來源檢視，並手動設定 FormView`DataSourceID`屬性設`ID`ObjectDataSource 的值。 接下來，以手動方式新增`ItemTemplate`。

無論何種方法，您決定採取，此時 FormView 的宣告式標記應該看起來像：


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

花點時間檢查啟用分頁 核取方塊在 FormView 的智慧標籤;這會新增`AllowPaging="True"`屬性 FormView 的宣告式語法。 此外，設定`EnableViewState`屬性設定為 False。

## <a name="step-2-defining-theitemtemplates-markup"></a>步驟 2： 定義`ItemTemplate`的標記

使用 FormView 繫結至 ObjectDataSource 控制項，並設定，以支援分頁我們已經準備好指定的內容`ItemTemplate`。 本教學課程中，讓我們的產品名稱顯示在`<h3>`標題。 接下來，讓我們使用 HTML`<table>`顯示剩餘的產品內容中的四個資料行資料表，其中的第一個和第三個資料行列出屬性名稱，而第二個和第四個清單及其值。

這個標記可以透過設計工具中的 [FormView 的範本] 編輯介面中輸入，或透過宣告式語法以手動方式輸入。 使用範本時通常找到更快速地直接使用宣告式語法，但可以自由使用您最熟悉想用的技巧。

下列標記顯示 FormView 宣告式標記之後`ItemTemplate`的結構已完成：


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

請注意，資料繫結語法- `<%# Eval("ProductName") %>`，範例可以直接插入至範本的輸出。 也就是需要不將它指派給 Label 控制項`Text`屬性。 比方說，我們有`ProductName`中顯示的值`<h3>`項目使用`<h3><%# Eval("ProductName") %></h3>`，其中產品 Chai 會轉譯為`<h3>Chai</h3>`。

`ProductPropertyLabel`並`ProductPropertyValue`CSS 類別用來指定產品的屬性名稱和值中的樣式`<table>`。 這些的 CSS 類別定義在`Styles.css`，而且會導致粗體且靠右對齊，並新增 padding 屬性值右邊的屬性名稱。

因為沒有任何 CheckBoxFields 使用 FormView，以便顯示`Discontinued`值為核取方塊中，我們必須新增我們自己的核取方塊控制項。 `Enabled`屬性設定為 False，將它變成唯讀的並核取方塊`Checked`屬性的值繫結`Discontinued`資料欄位。

使用`ItemTemplate`完成時，會顯示產品資訊更流暢的方式。 在本教學課程 (圖 4) 產生 FormView 的輸出比較 DetailsView 輸出從最後一個教學課程 (圖 3)。


[![固定的 DetailsView 輸出](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**圖 3**: Rigid DetailsView Output ([按一下以檢視完整大小的影像](using-the-formview-s-templates-cs/_static/image9.png))


[![流暢的 FormView 輸出](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**圖 4**： 流體 FormView Output ([按一下以檢視完整大小的影像](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>總結

雖然 GridView 和 DetailsView 控制項可以具有已經使用 TemplateFields 來自訂其輸出，同時仍會其資料類似方格的 boxy 的格式。 適用於顯示單一記錄的需要時使用較不嚴格的版面配置，FormView 是理想的選擇。 DetailsView，像 FormView 呈現單一資料錄從其`DataSource`，但與 DetailsView 不同方法，它可以只包含範本，並不支援欄位。

如我們所見本教學課程中，顯示一筆記錄時更有彈性的版面配置可讓 FormView。 在未來我們將檢驗 DataList 與重複項控制項，提供相同等級的彈性不如 FormsView，但可以顯示多筆記錄 （例如 GridView) 教學課程。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 E.R. Gilmore。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-templatefields-in-the-detailsview-control-cs.md)
> [下一頁](displaying-summary-information-in-the-gridview-s-footer-cs.md)
