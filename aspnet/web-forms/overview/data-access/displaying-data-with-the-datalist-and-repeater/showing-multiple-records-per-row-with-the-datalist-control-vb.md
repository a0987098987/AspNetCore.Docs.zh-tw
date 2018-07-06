---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: 顯示每個資料列與 DataList 控制項 (VB) 的多筆記錄 |Microsoft Docs
author: rick-anderson
description: 在這個簡短的教學課程中，我們將探討如何以自訂其 RepeatColumns 和 Flow 屬性透過 DataList 的版面配置。
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 55e07159fd9d0f4c750a2522feb0538a1cfb4bea
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831883"
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>顯示多筆記錄，每個資料列與 DataList 控制項 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe)或[下載 PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> 在這個簡短的教學課程中，我們將探討如何以自訂其 RepeatColumns 和 Flow 屬性透過 DataList 的版面配置。


## <a name="introduction"></a>簡介

DataList 範例我們發生在過去兩個教學課程中看到的單欄式 HTML 中的資料列轉譯的資料來源的每一筆記錄`<table>`。 雖然這是預設的 DataList 行為時，就很容易就能自訂 DataList 顯示，使得資料來源項目會分散到多個資料行、 多重資料列的資料表。 此外，它可以將所有資料來源顯示單一資料列、 多重資料行的資料清單中的項目。

我們可以自訂透過 DataList 的配置其`RepeatColumns`和`RepeatDirection`屬性，可分別指出轉譯多少資料行，且不論這些項目配置是水平或垂直。 圖 1，比方說，會顯示具有三個資料行的資料表中顯示產品資訊 DataList。


[![DataList 顯示每個資料列的三個產品](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**圖 1**: DataList 顯示三個產品每個資料列 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


藉由顯示每個資料列的多個資料來源項目，DataList 可以更有效地利用水平的螢幕空間。 在這個簡短的教學課程中，我們將探討這兩個的 DataList 屬性。

## <a name="step-1-displaying-product-information-in-a-datalist"></a>步驟 1： 在資料清單中顯示產品資訊

我們將探討之前`RepeatColumns`和`RepeatDirection`屬性，可讓 s 第一次建立 DataList 我們列出產品資訊使用標準的單一資料行、 多列資料表配置的頁面上。 此範例中，讓顯示產品名稱、 類別和價格使用下列標記的 s:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

我們已了解如何讓我將快速移動完成這些步驟，將資料繫結至在前一個範例中，DataList。 首先開啟`RepeatColumnAndDirection.aspx`頁面中`DataListRepeaterBasics`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 DataList。 從 DataList s 智慧標籤，選擇 建立新的 ObjectDataSource，並將它設定為提取資料的來源`ProductsBLL`類別的`GetProducts`方法中，選擇 （無） 選項精靈的插入、 更新和刪除索引標籤。

Visual Studio 會自動建立之後建立和繫結至 DataList 的新 ObjectDataSource， `ItemTemplate` ，會針對每個產品資料欄位的顯示名稱和值。 調整`ItemTemplate`直接透過宣告式標記，或從 編輯範本選項 DataList s 智慧標籤，讓它使用如上所示，取代標記*Product Name*，*類別名稱*，並*價格*文字將值指派給使用適當的資料繫結語法的 Label 控制項其`Text`屬性。 在更新之後`ItemTemplate`，頁面 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

請注意，已包含在格式規範`Eval`資料繫結語法`UnitPrice`，格式化為貨幣-傳回的值 `Eval("UnitPrice", "{0:C}").`

請花一點時間瀏覽您的網頁瀏覽器中。 如 [圖 2] 所示，DataList 會呈現為產品的單一資料行、 多列資料表。


[![根據預設，DataList 會轉譯成單一資料行中，多重資料列的資料表](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**圖 2**： 根據預設，DataList 會轉譯為單一資料行，多重資料列的資料表 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>步驟 2： 變更 DataList s 版面配置方向

預設行為時 DataList 是配置以垂直方式在單一資料行中，多重資料列的資料表中，其項目的變更此行為可以輕鬆地透過 DataList s [ `RepeatDirection`屬性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx)。 `RepeatDirection`屬性可以接受兩個可能值的其中一個：`Horizontal`或`Vertical`（預設值）。

藉由變更`RepeatDirection`屬性從`Vertical`到`Horizontal`，DataList 呈現其記錄在單一的資料列中，建立一個資料行，每個資料來源項目。 為了說明這種效果，DataList 設計工具中按一下，然後從 [屬性] 視窗中，變更`RepeatDirection`屬性從`Vertical`至`Horiztonal`。 立即在這種方式，設計工具調整 DataList 的配置，建立單一資料列、 多重資料行的介面 （請參閱 [圖 3]）。


[![Flow 屬性會指定如何方向 DataList s 項目會配置](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**圖 3**:`RepeatDirection`屬性會指定如何方向 DataList s 項目就無法配置 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


當顯示少量資料，單一資料列，多重資料行的資料表可能會最大化螢幕使用空間的理想方式。 不過，針對較大的小量的資料，單一資料列則需要多個資料行，那些項目右邊放在螢幕上關閉該無法的推播。 圖 4 顯示在單一資料列 DataList 中呈現時的產品。 由於有許多產品 (超過 80)，使用者必須捲動至右方以檢視每個產品的相關資訊到目前為止。


[![夠大的資料來源的單一資料行 DataList 需要水平捲軸](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**圖 4**： 針對夠大的資料來源、 單一資料行 DataList 會需要水平捲動 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>步驟 3： 多重資料行中，多重資料列的資料表中顯示資料

若要建立多重資料行中，多重資料列的資料清單，我們需要設定[`RepeatColumns`屬性](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx)要顯示的資料行的數目。 根據預設，`RepeatColumns`屬性設定為 0，這會導致 DataList 的單一資料列或資料行中顯示所有的項目 (根據的值`RepeatDirection`屬性)。

讓我們的範例，顯示每個資料表資料列的三種產品的 s。 因此，設定`RepeatColumns`屬性設定為 3。 完成此變更之後，請花一點時間瀏覽器中檢視結果。 如 [圖 5] 所示，是現在會在三個資料行中，多重資料列的資料表中列出的產品。


[![三個產品都會顯示每個資料列](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**圖 5**： 三個產品都會顯示每個資料列 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection`屬性會影響資料清單中的項目配置方式。[圖 5] 顯示其結果與`RepeatDirection`屬性設定為`Horizontal`。 請注意前, 三個產品 Chai、 變更，以及 Aniseed Syrup 配置是由左到右、 由上而下。 接下來三個產品 （起 Chef Anton 的印地安 Seasoning） 會出現在下方的第三個資料列。 變更`RepeatDirection`屬性回`Vertical`，不過，這些產品，從上到下配置，由左至右，如 圖 6 所示。


[![這裡的產品現已垂直配置出](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**圖 6**： 在這裡，產品會垂直配置時 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


所產生的資料表中顯示的資料列數目取決於繫結至 DataList 的總記錄數目。 精確地說，它的資料來源項目總數的上限除以的 s`RepeatColumns`屬性值。 因為`Products`資料表目前沒有 84 產品，也就是被 3 整除、 有 28 個資料列。 如果資料來源中的項目數和`RepeatColumns`屬性值不能整除，則最後一個資料列或資料行，則會有空白資料格。 如果`RepeatDirection`設定為`Vertical`，然後最後一個資料行都使用空的資料格; 如果`RepeatDirection`是`Horizontal`，則最後一個資料列會有空白資料格。

## <a name="summary"></a>總結

DataList，根據預設，列出其在單一資料行、 多列資料表，可以模擬使用單一的 TemplateField GridView 的版面配置中的項目。 可接受此預設版面配置時，我們可以透過顯示多個資料來源項目，每個資料列最大化螢幕使用空間。 完成這項作業很簡單，只要設定 DataList 的`RepeatColumns`屬性的資料行，顯示每個資料列數目。 此外，DataList 的`RepeatDirection`屬性可以用來指示是否多重資料行、 多列資料表的內容應該配置以水平方式從左到右，從上到下或垂直由上至下，由左到右。

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 John Suru。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [下一頁](nested-data-web-controls-vb.md)
