---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: 編輯和刪除 DataList (VB) 中的資料的概觀 |Microsoft 文件
author: rick-anderson
description: 在 DataList 沒有內建的編輯和刪除功能，而在此教學課程中我們會看到如何建立支援編輯和刪除 o DataList...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 6956777e91184a92e189db7aa716a4bd7dbbfccd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>編輯和刪除 DataList (VB) 中的資料的概觀
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe)或[下載 PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> DataList 沒有內建的編輯和刪除功能，而在此教學課程中我們會看到如何建立支援編輯和刪除其基礎資料的資料清單。


## <a name="introduction"></a>簡介

在[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)我們探討如何插入、 更新和刪除資料使用的應用程式架構、 ObjectDataSource，和 GridView、 DetailsView 及在 FormView 的教學課程控制項。 ObjectDataSource 和下列三個資料 Web 控制項，以實作簡單的資料修改介面嵌入式管理單元，涉及只計時從智慧標籤的核取方塊。 不需要撰寫的程式碼。

不幸的是，DataList 缺少編輯和刪除功能的 GridView 控制項中固有的內建。 遺漏的功能是部分因為事實，DataList relic 從先前版本的 ASP.NET，宣告式資料來源控制項和程式碼可用的資料修改網頁時無法使用。 雖然在 DataList ASP.NET 2.0 中的沒有提供現成提供的相同資料修改功能為 GridView，因此我們可以使用 ASP.NET 1.x 技術來包含這類功能。 這個方法需要許多程式碼，但在 DataList 中用來在此程序中幫助我們會看到此教學課程中，有一些事件和屬性。

在本教學課程中，我們會看到如何建立支援編輯和刪除其基礎資料的資料清單。 未來的教學課程會檢查更進階的編輯和刪除案例輸入的欄位的驗證，包括依正常程序將從資料存取或商務邏輯層等引發的例外狀況處理。

> [!NOTE]
> DataList，像是在中繼器控制項缺少不足的插入、 更新或刪除的方塊功能。 雖然可以加入這類功能，DataList 包含屬性和事件中繼器中找不到，簡化新增這類功能。 因此，本教學課程，以及未來的查看編輯和刪除焦點嚴格來說，資料清單。


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>步驟 1： 建立編輯和刪除教學課程的 Web 網頁

我們可以開始探索如何更新及刪除資料清單中的資料之前，可讓 s 先花點時間在我們的網站專案，我們需要本教學課程和下一步的數個項目中建立 ASP.NET 網頁。 加入新的資料夾，名為啟動`EditDeleteDataList`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![如需教學課程加入 ASP.NET 網頁](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**圖 1**： 加入 ASP.NET 網頁，針對教學課程


如同在其他資料夾，`Default.aspx`中`EditDeleteDataList`資料夾 > 一節中列出的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


最後，將頁面加入做為項目至`Web.sitemap`檔案。 具體來說，主要/詳細資料報表之後加入下列標記 DataList 與中繼器`<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 左側功能表現在包含編輯和刪除教學課程 DataList 項目。


![DataList 編輯和刪除教學課程的站台地圖現在包含的項目](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**圖 3**: DataList 編輯和刪除教學課程的站台地圖現在包含的項目


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>步驟 2： 檢查更新和刪除資料的技術

編輯和刪除與 GridView 的資料是很容易，因為基本上，GridView 和 ObjectDataSource 正常運作。 中所述[檢查插入、 更新和刪除與事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教學課程中，按一下資料列的更新按鈕時，GridView 會自動將指派其使用雙向資料繫結的欄位`UpdateParameters`其 ObjectDataSource 集合然後再叫用該 ObjectDataSource 的`Update()`方法。

可惜的是，DataList 不提供任何內建這項功能。 它是我們要負責確保使用者的值會指派至 ObjectDataSource 的參數，而且其`Update()`方法呼叫。 為了協助我們在這個工作中，DataList 提供下列屬性和事件：

- **[ `DataKeyField`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**當更新或刪除時，我們需要能夠唯一識別每個在 DataList 項目。 這個屬性設為顯示資料的主索引鍵欄位。 這樣將會填入 DataList s [ `DataKeys`集合](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx)具有指定`DataKeyField`每個 DataList 項目的值。
- **[ `EditCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**按鈕、 LinkButton 或 ImageButton 時引發其`CommandName`屬性設定為按下編輯。
- **[ `CancelCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**按鈕、 LinkButton 或 ImageButton 時引發其`CommandName`屬性設定為按下 [取消]。
- **[ `UpdateCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**按鈕、 LinkButton 或 ImageButton 時引發其`CommandName`屬性設定為按下更新。
- **[ `DeleteCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**按鈕、 LinkButton 或 ImageButton 時引發其`CommandName`屬性設定為按下 Delete。

使用這些屬性和事件，有四個方法，我們可以用來更新及刪除資料，從在 DataList:

1. **使用 ASP.NET 1.x 技術**DataList ASP.NET 2.0 和 ObjectDataSources 前存在，並且已經更新及刪除資料完全透過程式設計的方式。 這項技術完全 ditches ObjectDataSource 且需要，我們將資料繫結至 DataList 直接從商務邏輯層中擷取要顯示的資料時，以及更新或刪除記錄。
2. **使用單一 ObjectDataSource 控制項在頁面上選取、 更新和刪除**DataList 缺少 GridView s 固有編輯和刪除功能，而沒有 s 我們可以 t 不需要將它們加入自己。 使用這個方法，我們使用 ObjectDataSource 就像是在 GridView 範例中，但必須建立事件處理常式，如 DataList s`UpdateCommand`事件，我們設定 ObjectDataSource 的參數和呼叫其`Update()`方法。
3. **ObjectDataSource 控制項用於選取，但正在更新及刪除直接針對 BLL**使用選項 2 時，我們必須撰寫的程式碼中的位元`UpdateCommand`事件，將參數值指定，依此類推。 相反地，我們可以盡可能使用用於 ObjectDataSource 選取，但直接針對 BLL （例如使用選項 1） 更新和刪除呼叫。 我認為，直接與 BLL 互動來更新的資料會導致更容易閱讀的程式碼比指派 ObjectDataSource s `UpdateParameters` ，然後呼叫其`Update()`方法。
4. **使用宣告式的方式，透過多個 ObjectDataSources**所有先前三種方法需要許多程式碼。 如果您 d 而繼續使用作為儘可能多的宣告式語法，最後一個選擇是包含多個 ObjectDataSources 頁面上。 第一個 ObjectDataSource BLL 從擷取資料，並將它繫結至資料清單。 如需更新，另一個 ObjectDataSource 是加入，但是 s 在 DataList 內直接加入`EditItemTemplate`。 若要包含刪除的支援，尚未另一個 ObjectDataSource 會需要在`ItemTemplate`。 使用這個方法，這些內嵌 ObjectDataSource 的使用`ControlParameters`來以宣告方式將 ObjectDataSource 的參數繫結至使用者的輸入控制項 (而不需要以程式設計方式指定在 DataList 的`UpdateCommand`事件處理常式)。 這種方法仍然需要許多程式碼，我們必須呼叫內嵌的 ObjectDataSource s`Update()`或`Delete()`命令，但是需要遠小於與其他三個方法。 這裡的缺點是，多個 ObjectDataSources 不要弄亂 頁面上，detracting 從整體可讀性。

如果強制永遠只使用其中一種方法，我 d 選擇選項 1，因為它提供最大的彈性，而且在 DataList 原先設計來容納此模式。 在 DataList 已擴充，可使用 ASP.NET 2.0 的資料來源控制項，而它沒有所有擴充功能或官方 ASP.NET 2.0 Web 控制項 （GridView、 DetailsView 和 FormView） 資料的功能。 選項 2 到 4 並非不值得，不過。

這和未來編輯和刪除教學課程會使用 ObjectDataSource 擷取資料以顯示並直接呼叫 BLL 更新和刪除資料 （選項 3）。

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>步驟 3： 加入 DataList 和設定其 ObjectDataSource

在本教學課程中，我們將建立資料清單，列出產品資訊，並針對每個產品，提供使用者名稱和價格的編輯，並完全刪除產品的能力。 特別的是，我們將會擷取顯示使用 ObjectDataSource，記錄，但執行更新和刪除動作以直接與 BLL 介面。 我們會擔心編輯和刪除功能實作至 DataList 之前，可讓 s 先取得頁面以唯讀狀態的介面中顯示的產品。 因為我們 ve 檢查這些步驟在上一個教學課程中，我將會繼續執行它們快速。

先開啟`Basics.aspx`頁面`EditDeleteDataList`資料夾並從 [設計] 檢視中，DataList 新增至頁面。 接下來，從 DataList s 智慧標籤，建立新的 ObjectDataSource。 因為我們正在使用產品資料，請將它設定為使用`ProductsBLL`類別。 若要擷取*所有*產品中，選擇`GetProducts()`選取索引標籤中的方法。


[![設定使用 ProductsBLL 類別 ObjectDataSource](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![傳回的產品資訊。 使用 GetProducts() 方法](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**圖 5**： 傳回產品資訊使用`GetProducts()`方法 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


在 DataList，像是 GridView 中不是用於插入新資料。因此，選取 （無）] 選項從下拉式清單中 [插入] 索引標籤。也選擇 （無） UPDATE 和 DELETE 的索引標籤會透過 BLL 以程式設計方式執行 [更新與刪除。


[![確認下拉式清單會列出在 Sqldatasource 插入、 更新和刪除的索引標籤設定為 （無）](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**圖 6**： 確認 ObjectDataSource 的插入、 更新和刪除的索引標籤中的下拉式清單設定為 （無） ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


設定後 ObjectDataSource，按一下 完成，返回 設計工具。 當我們建立時自動完成 ObjectDataSource 組態，Visual Studio，在過去的範例中，看到 ve`ItemTemplate`的 DropDownList，顯示每個資料欄位。 取代此項`ItemTemplate`與一個顯示只有 s 產品名稱和價格。 此外，設定`RepeatColumns`2 的屬性。

> [!NOTE]
> 中所述*概觀的插入、 更新和刪除資料*教學課程中，當修改資料使用我們的架構需要我們移除 ObjectDataSource`OldValuesParameterFormatString`從 ObjectDataSource 的屬性宣告式標記 (或其重設為預設值，為`{0}`)。 在此教學課程中，不過，我們會使用 ObjectDataSource 僅適用於擷取資料。 因此，我們不需要修改 ObjectDataSource 的`OldValuesParameterFormatString`屬性值 (雖然它不 t 會降低，若要這樣做)。


取代預設 DataList 後`ItemTemplate`與一個自訂您的頁面上的宣告式標記看起來應該類似下列：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

請花一點時間來檢視進度透過瀏覽器。 如圖 7 所示，DataList 將兩個資料行中顯示每項產品的產品名稱和單位價格。


[![產品名稱及價格會顯示在兩個資料行的資料清單](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**圖 7**: 產品名稱和價格會顯示在兩個資料行 DataList ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> 在 DataList 的所需的更新和刪除處理程序的屬性數目，這些值會儲存檢視狀態中。 因此，當建置 DataList 支援編輯或刪除資料時，很重要，啟用 DataList 的檢視狀態。  
>   
>  精明讀取器可能還記得我們能夠建立可編輯 GridViews、 DetailsViews 和 FormViews 時停用檢視狀態。 這是因為 ASP.NET 2.0 Web 控制項可以包含*控制狀態*，它會保存在回傳，例如檢視狀態，但會被視為重要的狀態。


停用檢視狀態在 GridView 只省略了一般狀態資訊，但維持控制項狀態 （其中包含所需的編輯和刪除的狀態）。 在 ASP.NET 1.x 時間範圍內，已經建立，在 DataList 未利用控制項狀態，因此必須啟用檢視狀態。 請參閱[vs 控制項狀態。檢視狀態](https://msdn.microsoft.com/library/1whwt1k7.aspx)如需詳細資訊的用途上的控制項狀態，並從檢視狀態的不同方式。

## <a name="step-4-adding-an-editing-user-interface"></a>步驟 4： 加入編輯的使用者介面

GridView 控制項所組成 （BoundFields、 CheckBoxFields、 TemplateFields，等等） 的欄位的集合。 這些欄位可以調整其呈現的標記其模式而有所不同。 例如，處於唯讀模式時，BoundField 會顯示其資料欄位值為文字。處於編輯模式時，它會呈現文字方塊 Web 控制項`Text`指派資料欄位值的屬性。

在 DataList，相反地，呈現其使用範本的項目。 使用轉譯唯讀項目的`ItemTemplate`而在編輯模式中的項目會呈現透過`EditItemTemplate`。 此時，我們 DataList 只有`ItemTemplate`。 若要支援項目層級編輯的功能，我們需要加入`EditItemTemplate`，其中包含要編輯的項目顯示的標記。 此教學課程中，我們會使用文字方塊中的 Web 控制項編輯 s 產品名稱和單位價格。

`EditItemTemplate`可以建立以宣告方式或透過設計工具 （從 DataList s 智慧標籤，選取 [編輯樣板] 選項）。 若要使用 編輯樣板 選項，先按一下 編輯範本中的連結的智慧標籤，然後選取`EditItemTemplate`下拉式清單中的項目。


[![選擇使用 DataList 的 EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**圖 8**： 選擇使用 DataList s `EditItemTemplate` ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


接下來，在 產品名稱中輸入： 和 Price： 然後拖曳兩個文字方塊控制項從工具箱拖曳到`EditItemTemplate`設計工具上的介面。 設定文字方塊`ID`屬性`ProductName`和`UnitPrice`。


[![加入產品的名稱和價格的文字方塊](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**圖 9**： 加入文字方塊中的產品名稱和價格 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


我們需要繫結到對應的產品資料欄位值`Text`兩個文字方塊的內容。 文字方塊的智慧標籤，按一下 編輯資料繫結連結，然後將適當的資料欄位產生關聯`Text`屬性，如圖 10 所示。

> [!NOTE]
> 繫結時`UnitPrice`資料欄位至價格文字方塊 s `Text`  欄位中，您可能會將它格式化為貨幣值 (`{0:C}`)，一般數字 (`{0:N}`)，或將它保留為未格式化。


![將 ProductName 和 UnitPrice 資料欄位繫結至文字方塊的文字屬性](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**圖 10**： 繫結`ProductName`和`UnitPrice`資料欄位至`Text`文字方塊的內容


請注意圖 10 中的 [編輯資料繫結] 對話方塊的運作*不*包含存在於當編輯為 TemplateField GridView 或 DetailsView 或在 FormView 中的範本的雙向資料繫結核取方塊。 雙向資料繫結功能允許輸入要自動指派至對應的 ObjectDataSource s 與輸入 Web 控制項的值`InsertParameters`或`UpdateParameters`插入或更新資料時。 在 DataList 不支援雙向資料繫結，因為我們會看到稍後在本教學課程中，使用者可讓她變更，並已準備好更新資料後，我們需要以程式設計方式存取這些文字方塊`Text`屬性，並傳入其值適當`UpdateProduct`方法中的`ProductsBLL`類別。

最後，我們需要加入更新和取消按鈕到`EditItemTemplate`。 中[主要/詳細說明使用主要記錄項目符號清單與詳細資料 DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教學課程中，當按鈕、 LinkButton 或 ImageButton 其`CommandName`屬性會設定從中繼器或 DataList，內按下中繼器或 DataList 的`ItemCommand`就會引發事件。 在 DataList，如如果`CommandName`屬性設定為某個值，也可能引發其他事件。 特殊`CommandName`和其他項目屬性值包括：

- 取消引發`CancelCommand`事件
- 編輯引發`EditCommand`事件
- 更新會引發`UpdateCommand`事件

請記住，會引發這些事件*除了*`ItemCommand`事件。

加入`EditItemTemplate`兩個按鈕 Web 控制項，其`CommandName`設為更新和其他 s 設定為 [取消]。 在之後加入這兩個按鈕 Web 控制項設計工具看起來應該如下所示：


[![新增更新和取消 EditItemTemplate 的按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**圖 11**： 加入更新和取消按鈕`EditItemTemplate`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


與`EditItemTemplate`完成 DataList s 宣告式標記看起來應該類似下列：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>步驟 5： 加入的配管進入編輯模式

此時我們 DataList 已編輯的介面，定義透過其`EditItemTemplate`; 不過，那里 s 目前無法讓使用者瀏覽我們的頁面來表示他想要編輯 s 產品資訊。 我們需要加入每個產品，按一下時，會轉譯該 DataList 項目在編輯模式中編輯 按鈕。 藉由新增 [編輯] 按鈕以啟動`ItemTemplate`，透過設計工具或以宣告方式。 一定要在設定 [編輯] 按鈕的`CommandName`編輯的屬性。

加入這個編輯 按鈕之後，請花一點時間檢視透過瀏覽器頁面。 此步驟中，每個產品清單中應該包含編輯 按鈕。


[![新增更新和取消 EditItemTemplate 的按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**圖 12**： 加入更新和取消按鈕`EditItemTemplate`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


按一下按鈕導致回傳，但不*不*使產品清單進入編輯模式。 若要讓產品可編輯，我們需要：

1. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)索引的`DataListItem`剛才已按下的 [編輯] 按鈕。
2. 重新繫結資料清單的資料。 在 DataList 時重新轉譯，`DataListItem`其`ItemIndex`對應於 DataList s`EditItemIndex`會呈現使用其`EditItemTemplate`。

DataList s 自`EditCommand`時會引發事件，按一下 [編輯] 按鈕時，請建立`EditCommand`事件處理常式，以下列程式碼：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand`事件處理常式會傳入類型的物件`DataListCommandEventArgs`做為其第二個輸入參數，其中包含的參考`DataListItem`其編輯 按鈕已按下 (`e.Item`)。 此事件處理常式會先設定 DataList s`EditItemIndex`至`ItemIndex`的可編輯`DataListItem`並再重新 DataList 資料繫結呼叫 DataList 的`DataBind()`方法。

加入此事件處理常式之後, 重新檢查瀏覽器中。 現在按一下 [編輯] 按鈕可按下的產品可編輯 （請參閱圖 13）。


[![按一下 [編輯] 按鈕可編輯產品](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**圖 13**： 按一下 [編輯] 按鈕可讓產品可編輯 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>步驟 6： 儲存使用者的變更

按一下 [取消] 按鈕的已編輯的產品 s 更新此時; 沒有任何作用新增此功能，我們建立所需的事件處理常式 DataList s`UpdateCommand`和`CancelCommand`事件。 藉由建立啟動`CancelCommand`事件處理常式中，按一下 編輯的產品 s 取消 按鈕和其一項工作傳回 DataList 預先編輯狀態時執行。

若要讓 DataList 呈現其所有項在唯讀模式中，我們需要：

1. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)索引不存在的`DataListItem`索引。 `-1` 因為是安全的選擇，`DataListItem`索引開始`0`。
2. 重新繫結資料清單的資料。 因為沒有`DataListItem` `ItemIndex` es 對應至 DataList 的`EditItemIndex`，整個 DataList 會呈現在唯讀模式。

下列事件處理常式程式碼就可完成下列步驟：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

此步驟中，按一下 [取消] 5d; 按鈕傳回 DataList 成預先編輯狀態。

我們需要完成最後一個事件處理常式是`UpdateCommand`事件處理常式。 此事件處理常式需要：

1. 以程式設計方式存取的使用者輸入的產品名稱和價格，以及編輯的產品的`ProductID`。
2. 起始更新程序，透過呼叫適當`UpdateProduct`中多載`ProductsBLL`類別。
3. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)索引不存在的`DataListItem`索引。 `-1` 因為是安全的選擇，`DataListItem`索引開始`0`。
4. 重新繫結資料清單的資料。 因為沒有`DataListItem` `ItemIndex` es 對應至 DataList 的`EditItemIndex`，整個 DataList 會呈現在唯讀模式。

步驟 1 和 2 負責儲存使用者的變更;步驟 3 和 4 返回 DataList 預先編輯狀態所做的變更已儲存並與相同執行中的步驟之後`CancelCommand`事件處理常式。

若要取得更新的產品名稱和價格，我們需要使用`FindControl`方法來以程式設計方式控制內的文字方塊中 Web 參考`EditItemTemplate`。 我們也要取得已編輯的產品的`ProductID`值。 當我們一開始在 DataList 結合 ObjectDataSource 時，Visual Studio 指派 DataList s`DataKeyField`屬性從資料來源的主索引鍵值 (`ProductID`)。 這個值便可以擷取從 DataList 的`DataKeys`集合。 請花一點時間，以確保`DataKeyField`屬性確實設定為`ProductID`。

下列程式碼會實作四個步驟：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

此事件處理常式一開始會讀取已編輯的產品 s`ProductID`從`DataKeys`集合。 下一步，在兩個文字方塊`EditItemTemplate`參考及其`Text`屬性儲存在本機變數，`productNameValue`和`unitPriceValue`。 我們使用`Decimal.Parse()`方法，以從中讀取值`UnitPrice`文字方塊中，因此，如果輸入的值有貨幣符號，它仍然正確可轉換為`Decimal`值。

> [!NOTE]
> 將值從`ProductName`和`UnitPrice`如果文字方塊的文字屬性具有指定的值的文字方塊只會 productNameValue 和 unitPriceValue 變數指派。 否則，值為`Nothing`用於變數，其具有與資料庫中更新資料的效果`NULL`值。 也就是我們的程式碼會將空字串，以資料庫`NULL`是編輯介面 GridView、 DetailsView 和 FormView 控制項中的預設行為的值。


在讀取值之後`ProductsBLL`類別 s`UpdateProduct`呼叫方法、 傳入 s 產品名稱、 價格和`ProductID`。 此事件處理常式完成所傳回前編輯狀態中的完全相同的邏輯在 DataList`CancelCommand`事件處理常式。

與`EditCommand`， `CancelCommand`，和`UpdateCommand`事件處理常式完成，訪客可以編輯的名稱和產品的價格。 圖 14-16 顯示此編輯工作流程中的動作。


[![當第一次瀏覽頁面、 所有產品處於唯讀模式](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**圖 14**： 所有產品時第一次瀏覽頁面、 都處於唯讀模式 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![若要更新的產品名稱的價格，按一下 [編輯] 按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**圖 15**： 若要更新的產品名稱或價格，按一下 [編輯] 按鈕 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![值，變更之後按一下 還原成唯讀模式的更新](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**圖 16**： 之後變更的值、 按一下 更新要傳回以唯讀模式 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>步驟 7： 加入刪除功能

加入 DataList 刪除功能的步驟都與類似的加入編輯功能。 簡單地說，我們需要加入至 [刪除] 按鈕`ItemTemplate`，按下時：

1. 在對應的產品 s 讀取`ProductID`透過`DataKeys`集合。
2. 執行刪除藉由呼叫`ProductsBLL`類別的`DeleteProduct`方法。
3. 重新繫結資料清單的資料。

將加入 [刪除] 按鈕以啟動 s `ItemTemplate`。

按下按鈕時其`CommandName`是編輯、 更新、 或取消引發 DataList s`ItemCommand`事件及其他事件 (例如，當使用編輯`EditCommand`也會引發事件)。 同樣地，任何按鈕、 LinkButton 或在 DataList ImageButton 其`CommandName`屬性設定為刪除的原因`DeleteCommand`引發事件 (連同`ItemCommand`)。

新增在 [編輯] 按鈕旁邊的 [刪除] 按鈕`ItemTemplate`，設定其`CommandName`要刪除的屬性。 在新增之後此按鈕控制項 DataList 的`ItemTemplate`宣告式語法的樣子：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

接下來，建立事件處理常式，如 DataList 的`DeleteCommand`事件時，使用下列程式碼：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

按一下 [刪除] 按鈕會導致回傳，並引發 DataList 的`DeleteCommand`事件。 事件處理常式中，按下的產品 s`ProductID`從存取值`DataKeys`集合。 下一步，藉由呼叫刪除產品`ProductsBLL`類別的`DeleteProduct`方法。

刪除產品後它 s 重要我們重新繫結的資料在 DataList (`DataList1.DataBind()`)，否則在 DataList 將會繼續顯示的產品，已將它刪除。

## <a name="summary"></a>總結

DataList 缺少點，然後按一下 編輯和刪除滿意的 GridView 的支援，而短位元的程式碼就可以增強以包括這些功能。 在此教學課程中我們可了解如何建立兩個資料行清單的產品，無法刪除，而且無法編輯其名稱和價格。 加入編輯和刪除的支援是在適當的 Web 控制項，其中包括`ItemTemplate`和`EditItemTemplate`、 建立對應的事件處理常式、 讀取使用者輸入與主索引鍵值，以及商務與連結邏輯層。

雖然我們已加入基本編輯和刪除資料清單的功能，它會缺少更進階的功能。 比方說，沒有任何輸入的欄位驗證-如果使用者輸入太價格昂貴，例外狀況會擲回的`Decimal.Parse`時嘗試轉換太昂貴到`Decimal`。 同樣地，如果在更新的資料，在商務邏輯或資料存取層級問題的使用者會有標準錯誤螢幕。 確認 [刪除] 按鈕上的任何排序，沒有不小心刪除產品所有太可能是。

在未來教學課程，我們會看到如何改善編輯的使用者體驗。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones、 Ken Pespisa 和袁 Schmidt。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](customizing-the-datalist-s-editing-interface-cs.md)
> [下一頁](performing-batch-updates-vb.md)
