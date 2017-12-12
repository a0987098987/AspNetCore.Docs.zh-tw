---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: "顯示多個記錄，每個資料列與資料清單控制項 (VB) |Microsoft 文件"
author: rick-anderson
description: "在這個簡短的教學課程中，我們將探討如何自訂透過其 RepeatColumns 和 Flow 屬性 DataList 版面配置。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d6a9c6aef42d1f165567d1a1802bffa853a320e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>顯示每個資料列與資料清單控制項 (VB) 的多筆記錄
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe)或[下載 PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> 在這個簡短的教學課程中，我們將探討如何自訂透過其 RepeatColumns 和 Flow 屬性 DataList 版面配置。


## <a name="introduction"></a>簡介

DataList 範例我們看到在過去的兩個教學課程中我們有轉譯為單欄式 HTML 中的資料列的每一筆記錄的資料來源`<table>`。 雖然這是預設 DataList 行為時，它是很容易就能自訂 DataList 顯示資料來源項目分散到多個資料行、 多重資料列的資料表。 此外，它 s 可能有的所有資料來源的單一資料列、 多重資料行的資料清單中顯示的項目。

我們可以自訂 DataList 的配置透過其`RepeatColumns`和`RepeatDirection`屬性，它們分別表示系統會轉譯多少資料行，而且是否這些配置項目的垂直或水平。 圖 1 中，例如，顯示包含三個資料行的資料表中顯示產品資訊 DataList。


[![在 DataList 顯示每個資料列的三個產品](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**圖 1**: DataList 顯示三個產品每個資料列 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))


藉由顯示每個資料列的多個資料來源項目，DataList 可以更有效地利用水平的螢幕空間。 在這個簡短的教學課程中，我們將探討這兩個 DataList 屬性。

## <a name="step-1-displaying-product-information-in-a-datalist"></a>步驟 1： 在資料清單中顯示產品資訊

我們檢查之前`RepeatColumns`和`RepeatDirection`屬性，可讓 s 先 DataList 上建立我們列出產品資訊，請使用標準的單一資料行中，多重資料列的資料表配置的頁面。 例如，可讓顯示產品的名稱、 類別和價格，使用下列標記的 s:


[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

我們已看到如何讓我將會快速移動完成這些步驟，將資料繫結至在前一個範例中，DataList。 先開啟`RepeatColumnAndDirection.aspx`頁面`DataListRepeaterBasics`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳資料清單。 從 DataList s 智慧標籤，選擇建立新的 ObjectDataSource 並將它設定為提取資料的來源`ProductsBLL`類別的`GetProducts`方法中，選擇 （無） 選項從精靈的插入、 更新和刪除索引標籤。

Visual Studio 會自動建立之後建立和繫結至資料清單的新 ObjectDataSource，`ItemTemplate`會顯示每個產品資料欄位的名稱和值。 調整`ItemTemplate`直接透過宣告式標記，或從 編輯樣板選項 DataList s 智慧標籤，讓它使用如上所示，取代標記*產品名稱*，*類別名稱*，和*價格*Label 控制項，用於將值指派給適當的資料繫結語法的文字及其`Text`屬性。 在更新之後， `ItemTemplate`，頁面 s 宣告式標記看起來應該類似下列：


[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

請注意，已包含在格式規範`Eval`資料繫結語法`UnitPrice`，格式化為貨幣-傳回的值`Eval("UnitPrice", "{0:C}").`

請花一點時間瀏覽您的網頁瀏覽器中。 如圖 2 所示，DataList 會轉譯成單一資料行中，多重資料列的產品資料表中。


[![根據預設，DataList 呈現為單一資料行中，多重資料列的資料表](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**圖 2**： 根據預設，DataList 會轉譯成單一資料行，多重資料列的資料表 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))


## <a name="step-2-changing-the-datalist-s-layout-direction"></a>步驟 2： 變更 DataList s 版面配置方向

預設行為時配置它的項目，以垂直方式在單一資料行中，多重資料列的資料表中，DataList 為這種行為可輕鬆地透過變更 DataList s [ `RepeatDirection`屬性](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalist.repeatdirection.aspx)。 `RepeatDirection`屬性可以接受兩個可能值的其中一個：`Horizontal`或`Vertical`（預設值）。

藉由變更`RepeatDirection`屬性從`Vertical`至`Horizontal`，DataList 呈現其記錄在單一資料列，建立一個資料行，每個資料來源項目。 為了說明這個效果，設計工具中 DataList 上按一下，然後從 [屬性] 視窗中，變更`RepeatDirection`屬性從`Vertical`至`Horiztonal`。 立即在此情況下，在設計工具調整 DataList 的配置，建立單一資料列、 多重資料行的介面 （請參閱圖 3）。


[![Flow 屬性決定如何方向 DataList s 項目會配置掉](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**圖 3**:`RepeatDirection`屬性決定如何方向 DataList s 項目會配置掉 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))


當顯示少量的資料，單一資料列，多重資料行的資料表可能會最大化螢幕使用空間的理想方式。 不過，較大的磁碟區的資料，單一資料列就需要多個資料行，那些項目，無法符合螢幕大小關閉右邊的推播通知。 圖 4 顯示在單一資料列 DataList 轉譯時的產品。 由於有許多產品 (超過 80)，使用者必須捲動來檢視每個產品的相關資訊的權限。


[![夠大的資料來源的單一資料行 DataList 需要水平捲軸](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**圖 4**： 為夠大的資料來源、 單一資料行 DataList 將需要水平捲軸 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))


## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>步驟 3： 在多個資料行、 多重資料列的資料表中顯示資料

若要建立多重資料行中，多重資料列的資料清單，所以我們需要將[`RepeatColumns`屬性](https://msdn.microsoft.com/en-us/system.web.ui.webcontrols.datalist.repeatcolumns.aspx)要顯示的資料行的數字。 根據預設，`RepeatColumns`屬性設定為 0，這會導致在 DataList 以單一資料列或資料行中顯示所有的項目 (根據的值`RepeatDirection`屬性)。

我們的範例，可讓 s 顯示三個產品每個資料表資料列。 因此，設定`RepeatColumns`屬性設定為 3。 變更之後，請花一點時間瀏覽器中檢視結果。 如圖 5 所示，是現在會在三個資料行中，多重資料列的資料表中列出的產品。


[![每個資料列會顯示三個的產品](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**圖 5**： 三項產品會顯示每個資料列 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))


`RepeatDirection`屬性會影響在 DataList 內的項目配置方式。圖 5 顯示的結果與`RepeatDirection`屬性設定為`Horizontal`。 請注意，從左到右，從上到下的前三個產品 Chai、 變更，以及 Aniseed Syrup 會配置。 接下來三個產品 （起 Chef Anton 的印地安 Seasoning） 會出現在下方的第三個資料列。 變更`RepeatDirection`屬性設回`Vertical`，不過，這些產品，從上到下配置，由左至右，如圖 6 所示。


[![此處的產品為垂直配置出](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**圖 6**： 在這裡的產品為垂直配置外 ([按一下以檢視完整大小的影像](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))


所產生的資料表中顯示的資料列數目取決於繫結至資料清單的總記錄數目。 明確地說，它的資料來源項目總數的上限除以的 s`RepeatColumns`屬性值。 因為`Products`資料表目前有 84 產品，這是整除 3、 有 28 個資料列。 如果資料來源中的項目數和`RepeatColumns`屬性值不可以整除，則最後一個資料列或資料行，則會有空白資料格。 如果`RepeatDirection`設`Vertical`，最後一個資料行就需要空儲存格; 如果`RepeatDirection`是`Horizontal`，則最後一個資料列會有空白資料格。

## <a name="summary"></a>總結

在 DataList，根據預設，列出它是單一資料行中，多重資料列表，會模擬單一 TemplateField 的 GridView 的版面配置中的項目。 可接受此預設版面配置時，我們可以藉由顯示每個資料列的多個資料來源項目來最大化螢幕面積。 完成此很簡單，只要設定 DataList 的`RepeatColumns`屬性的資料行，顯示每個資料列數目。 此外，DataList 的`RepeatDirection`屬性可以用來指示是否多重資料行中，多重資料列的資料表的內容應該配置以水平方式從左到右，從上到下或垂直由上至下，由左到右。

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 John Suru。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
[下一頁](nested-data-web-controls-vb.md)
