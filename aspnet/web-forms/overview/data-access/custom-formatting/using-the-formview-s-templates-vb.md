---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: 使用在 FormView 的範本 (VB) |Microsoft 文件
author: rick-anderson
description: 不同於在 DetailsView 中，欄位不被由 FormView。 相反地，在 FormView 呈現使用範本。 在本教學課程中，我們將檢驗使用 F...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 16293960f5d8758c93646844bd159547f5e0f38c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-the-formviews-templates-vb"></a>使用在 FormView 的範本 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe)或[下載 PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> 不同於在 DetailsView 中，欄位不被由 FormView。 相反地，在 FormView 呈現使用範本。 在此教學課程中我們將檢驗使用 FormView 控制項呈現資料的較不顯示。


## <a name="introduction"></a>簡介

最後兩個教學課程中我們可了解如何自訂使用 TemplateFields GridView 和 DetailsView 控制項的輸出。 TemplateFields 允許可高度自訂的特定欄位的內容，但最後 GridView 和 DetailsView 都有而 boxy、 類似格線的外觀。 許多情況下這類類似格線版面配置是理想的做法，但有時候需要更流暢、 較不嚴謹的顯示。 顯示單一記錄，這類流暢的版面配置時，可能使用 FormView 控制項。

不同於在 DetailsView 中，欄位不被由 FormView。 您無法加入 BoundField 或 TemplateField FormView。 相反地，在 FormView 呈現使用範本。 在 FormView 視為包含單一 TemplateField DetailsView 控制項。 在 FormView 支援下列範本：

- `ItemTemplate` 用來呈現特定 FormView 中顯示的資料錄
- `HeaderTemplate` 用來指定選擇性標頭資料列
- `FooterTemplate` 用來指定選擇性頁尾資料列
- `EmptyDataTemplate` 當在 FormView 的`DataSource`缺少任何記錄，`EmptyDataTemplate`用來取代`ItemTemplate`呈現控制項的標記
- `PagerTemplate` 可用來啟用分頁的 FormViews 自訂分頁介面
- `EditItemTemplate` / `InsertItemTemplate` 用來支援這類功能的 FormViews 自訂編輯介面或插入介面

在此教學課程中我們將檢驗使用 FormView 控制項來顯示產品的較不顯示。 而不是欄位名稱、 類別、 供應商和等等，在 FormView 的`ItemTemplate`會顯示這些值使用的標頭項目組合和`<table>`（請參閱圖 1）。


[![在 FormView 中斷的 DetailsView 中看到的類似格線版面配置](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**圖 1**: FormView 超出 Grid-Like 配置看到中斷 DetailsView 中 ([按一下以檢視完整大小的影像](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>資料繫結至 FormView 步驟 1:

開啟`FormView.aspx`頁面上，並從 [工具箱] 拖曳至設計工具拖曳 FormView。 第一次加入 FormView 時它會顯示為灰色方塊，指示我們的`ItemTemplate`需要。


[![在 FormView 無法轉譯設計工具中，直到提供 ItemTemplate 為止](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**圖 2**: FormView 無法轉譯設計工具之前`ItemTemplate`提供 ([按一下以檢視完整大小的影像](using-the-formview-s-templates-vb/_static/image6.png))


`ItemTemplate`可以 （透過宣告式語法中） 以手動方式建立，或者可以透過繫結至資料來源控制項，透過設計工具的 FormView 是自動建立。 這會自動建立`ItemTemplate`包含，列出每個欄位名稱和標籤控制項的 HTML`Text`屬性繫結至欄位的值。 這個方法也自動-建立`InsertItemTemplate`和`EditItemTemplate`，這兩種會針對每個資料來源控制項所傳回的資料欄位填入具有輸入控制項。

如果您想要自動建立範本，從在 FormView 的智慧標籤新增 ObjectDataSource 控制項叫用`ProductsBLL`類別的`GetProducts()`方法。 這會建立與 FormView `ItemTemplate`， `InsertItemTemplate`，和`EditItemTemplate`。 從原始碼 檢視中，移除`InsertItemTemplate`和`EditItemTemplate`因為我們不感興趣建立 FormView 支援編輯，或尚未插入。 下一步，清除內標記`ItemTemplate`，所以我們需要從重新開始。

如果您而是會往上建置`ItemTemplate`以手動方式，您可以加入並設定從 [工具箱] 拖曳至設計工具拖曳 ObjectDataSource。 不過，不會設定在 FormView 的資料來源從設計工具。 相反地，請移至來源檢視，並手動設定在 FormView 的`DataSourceID`屬性`ID`ObjectDataSource 的值。 接下來，以手動方式加入`ItemTemplate`。

不論哪種方法，您決定採取，此時您在 FormView 的宣告式標記應該看起來：


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

請花一點時間檢查啟用分頁 核取方塊在 FormView 的智慧標籤。這會新增`AllowPaging="True"`屬性設定為在 FormView 的宣告式語法。 此外，設定`EnableViewState`屬性設定為 False。

## <a name="step-2-defining-theitemtemplates-markup"></a>步驟 2： 定義`ItemTemplate`的標記

在 FormView ObjectDataSource 控制項繫結並設定以支援分頁我們就可以指定的內容`ItemTemplate`。 本教學課程，讓我們有產品名稱顯示在`<h3>`標題。 接下來，我們將使用 HTML`<table>`顯示剩餘的產品屬性中包含的第一個和第三個資料行清單的屬性名稱和第二個和第四個清單及其值的四個資料行資料表。

這個標記可以透過在設計工具在 FormView 的範本編輯介面中輸入，或手動輸入透過宣告式語法。 使用範本時通常找到它更快速地進行直接使用宣告式語法，但可以自由使用您最熟悉任何技術。

下列標記會顯示在 FormView 宣告式標記之後`ItemTemplate`的結構已完成：


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

請注意，資料繫結語法- `<%# Eval("ProductName") %>`，針對範例直接插入的範本輸出。 也就是說，它需要指派給 Label 控制項`Text`屬性。 比方說，我們有`ProductName`值顯示在`<h3>`項目使用`<h3><%# Eval("ProductName") %></h3>`，其中產品 Chai 會轉譯為`<h3>Chai</h3>`。

`ProductPropertyLabel`和`ProductPropertyValue`CSS 類別用於指定的產品屬性名稱和值中的樣式`<table>`。 這些 CSS 類別定義於`Styles.css`，而且會導致粗體且靠右對齊，並加入填補的屬性值右邊的屬性名稱。

因為沒有任何 CheckBoxFields 適用於在 FormView 中，以便顯示`Discontinued`值為核取方塊中，我們必須加入自己的核取方塊控制項。 `Enabled`屬性設為 False，因此唯讀狀態，並核取方塊的`Checked`屬性的值繫結`Discontinued`資料欄位。

與`ItemTemplate`完成時，會顯示產品資訊以更流暢的方式。 比較上一個教學課程 (圖 3) 的 DetailsView 輸出產生 FormView 在本教學課程 (圖 4) 的輸出。


[![固定的 DetailsView 輸出](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**圖 3**： 固定 DetailsView Output ([按一下以檢視完整大小的影像](using-the-formview-s-templates-vb/_static/image9.png))


[![流暢 FormView 輸出](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**圖 4**： 流體 FormView Output ([按一下以檢視完整大小的影像](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>總結

雖然 GridView 和 DetailsView 控制項可以有使用 TemplateFields 自訂其輸出，同時仍會其資料以類似格線 boxy 的格式。 為單一記錄需要顯示這些時間 FormView 使用較不嚴謹的配置時，是理想的選擇。 在 DetailsView 中，例如 FormView 呈現的單一記錄其`DataSource`，但不同於 DetailsView 方法，它可以是只組成的範本，並不支援欄位。

如我們所見本教學課程中，在 FormView 允許更彈性的配置顯示單一記錄時。 在未來我們將檢驗 DataList 和中繼器控制項提供相同層級的彈性不如 FormsView，但可以顯示多筆記錄 （例如 GridView) 教學課程。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 本教學課程是 E.R.導致檢閱者 Gilmore。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](using-templatefields-in-the-detailsview-control-vb.md)
> [下一頁](displaying-summary-information-in-the-gridview-s-footer-vb.md)
