---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: 編輯和刪除 DataList (VB) 中的資料的概觀 |Microsoft Docs
author: rick-anderson
description: 雖然 DataList 缺少內建的編輯和刪除功能，在本教學課程中我們將看到如何建立支援編輯和刪除 o DataList...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5e2a0f672a9be074abd3ab92eb5b36c18d64d1b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364011"
---
<a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>編輯和刪除 DataList (VB) 中的資料的概觀
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe)或[下載 PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> 雖然 DataList 缺少內建的編輯和刪除功能，在本教學課程中我們會看到如何建立支援編輯和刪除其基礎資料的 DataList。


## <a name="introduction"></a>簡介

在 [概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教學課程中我們探討了如何插入、 更新和刪除資料使用的應用程式架構、 ObjectDataSource，和 GridView、 DetailsView 和 FormView控制項。 使用 ObjectDataSource 和下列三個資料 Web 控制項，實作簡單的資料修改介面是嵌入式管理單元，以及涉及只計時從智慧標籤的核取方塊。 不需要撰寫的程式碼。

不幸的是，DataList 缺少內建編輯和刪除 GridView 控制項中固有的功能。 缺少的功能是部分 DataList 是從舊版的 ASP.NET，relic 宣告式資料來源控制項和免程式碼的資料修改網頁時無法使用。 DataList ASP.NET 2.0 中的未提供立即可用的相同資料修改功能為 GridView，而我們可以使用 ASP.NET 1.x 技術，以包含這類功能。 此方法需要的程式碼，但 DataList 來協助此程序中，我們會看到在本教學課程中，有一些事件和屬性。

在本教學課程中，我們會看到如何建立支援編輯和刪除其基礎資料的 DataList。 未來的教學課程將探討更進階的編輯和刪除案例輸入的欄位驗證，包括依正常程序處理例外狀況引發的資料存取或商務邏輯層，依此類推。

> [!NOTE]
> 像 DataList 和 Repeater 控制項缺少外的插入、 更新或刪除預設功能。 雖然這類功能，可以加入 DataList 會包含屬性和事件中繼器中找不到可簡化新增這類功能。 因此，本教學課程和查看編輯和刪除的未來的焦點嚴格來說，資料清單。


## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>步驟 1： 建立編輯和刪除教學課程網頁

我們開始探索如何更新及刪除資料清單中的資料之前，讓先花點時間在本教學課程的下一步 的數個工具，我們將需要我們網站專案中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`EditDeleteDataList`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

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

**圖 1**： 加入 ASP.NET 網頁的教學課程


在其他資料夾，例如`Default.aspx`在`EditDeleteDataList`資料夾 > 一節中列出的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))


最後，將頁面新增項目，以作為`Web.sitemap`檔案。 具體來說，在使用 DataList 與重複項的主版/詳細報告之後新增下列標記`<siteMapNode>`:


[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含 DataList 編輯和刪除教學課程的項目。


![網站導覽現在包含項目 DataList 編輯和刪除教學課程](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**圖 3**： 網站地圖現在包含項目 DataList 編輯和刪除教學課程


## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>步驟 2： 檢查更新和刪除的資料技術

編輯和刪除的 GridView 資料是很容易，因為基本上，GridView 和 ObjectDataSource 正常運作。 中所述[檢查與插入、 更新和刪除事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教學課程中，按一下資料列 s 的更新 按鈕時，GridView 會自動指派使用雙向資料繫結其欄位`UpdateParameters`其 ObjectDataSource 的集合，然後叫用該 ObjectDataSource 的`Update()`方法。

可惜的是，DataList 不提供任何此內建的功能。 它是我們有責任確保使用者的值會指派至 ObjectDataSource 的參數，且其`Update()`呼叫方法。 為了協助我們在此工作中，DataList 會提供下列屬性和事件：

- **[ `DataKeyField`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)** 當更新或刪除時，我們必須要能夠唯一識別 DataList 中的每個項目。 這個屬性設為顯示資料的主索引鍵的欄位。 如此一來，將會填入 DataList s [ `DataKeys`集合](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx)具有指定`DataKeyField`每個 DataList 項目的值。
- **[ `EditCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  按鈕、 LinkButton 或 ImageButton 時，就會引發其`CommandName`屬性設定為按下編輯。
- **[ `CancelCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  按鈕、 LinkButton 或 ImageButton 時，就會引發其`CommandName`屬性設定為按下 [取消]。
- **[ `UpdateCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  按鈕、 LinkButton 或 ImageButton 時，就會引發其`CommandName`屬性設定為按下更新。
- **[ `DeleteCommand`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  按鈕、 LinkButton 或 ImageButton 時，就會引發其`CommandName`屬性設定為按下 Delete。

使用這些屬性和事件，有四個方法可用來更新和刪除資料清單中的資料：

1. **使用 ASP.NET 1.x 技術**DataList ASP.NET 2.0 和 ObjectDataSources 前存在且能夠更新及刪除資料，完全透過程式設計的方式。 這項技術完全 ditches ObjectDataSource 和需要，我們將資料繫結至 DataList 直接從商務邏輯層，同時在擷取資料時要顯示和更新或刪除記錄時。
2. **用於選取、 更新和刪除的頁面上的單一 ObjectDataSource 控制項**DataList 缺少的 GridView s 固有編輯與刪除功能，而沒有 s 沒有理由，我們可以 t 中加入它們自己。 使用此方法時，我們在 GridView 範例中，就像使用 ObjectDataSource，但必須建立事件處理常式，如 DataList s`UpdateCommand`事件，我們可在此設定 ObjectDataSource 的參數並呼叫其`Update()`方法。
3. **使用 ObjectDataSource 控制項的選取，但正在更新及刪除直接針對 BLL**時使用選項 2，我們必須撰寫的程式碼中`UpdateCommand`事件，並指派參數值，依此類推。 相反地，我們可以堅持使用 ObjectDataSource 選取，但更新和刪除對進行呼叫直接 BLL （例如與選項 1）。 在我看來，直接與 BLL 互動來更新的資料會導致更容易閱讀程式碼與指派的 ObjectDataSource 秒`UpdateParameters`，然後呼叫其`Update()`方法。
4. **使用宣告式的方式，透過多個 ObjectDataSources**所有先前三種方法需要的程式碼。 如果您 d 而繼續使用做為盡可能多宣告式語法，最後一個選項是包含多個頁面上的 ObjectDataSources。 第一個 ObjectDataSource BLL 從擷取資料，並將它繫結至 DataList。 更新，另一個的 ObjectDataSource 會新增，但新增直接在 DataList 的`EditItemTemplate`。 若要包含刪除的支援，還另一個的 ObjectDataSource 會需要在`ItemTemplate`。 使用此方法時，這些內嵌使用 ObjectDataSource`ControlParameters`若要以宣告方式繫結到使用者輸入控制項的 ObjectDataSource 的參數 (而不需要以程式設計方式指定 DataList s 中`UpdateCommand`事件處理常式)。 此方法仍需要的程式碼，我們必須呼叫內嵌的 ObjectDataSource s`Update()`或`Delete()`命令但需要遠小於使用其他三個方法。 這裡的缺點是，多個 ObjectDataSources 執行弄亂 頁面上，從整體可讀性 detracting。

如果強制只會使用其中一種方式，d 我選擇選項 1，因為它提供最大的彈性，而且 DataList 原先設計來容納此模式也一樣。 雖然 DataList 已擴充，可使用 ASP.NET 2.0 資料來源控制項，它並沒有擴充點的所有或正式的 ASP.NET 2.0 資料 Web 控制項 （GridView、 DetailsView 和 FormView） 的功能。 選項 2 到 4 不值得一讀，不過。

這和未來編輯和刪除教學課程會使用 ObjectDataSource 擷取資料以顯示，並直接呼叫來更新和刪除資料 （選項 3） BLL。

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>步驟 3： 加入 DataList 和設定其 ObjectDataSource

在本教學課程中，我們將建立資料清單，列出產品資訊，並針對每個產品，提供使用者編輯的名稱和價格，並一併刪除產品的能力。 特別的是，我們將擷取的記錄，以顯示使用 ObjectDataSource，但執行更新和刪除動作直接與 BLL 互動。 我們會擔心至 DataList 的編輯和刪除功能實作之前，讓 s 首先取得唯讀介面中顯示的產品頁面。 因為我們 ve 檢查下列步驟在上一個教學課程中，我將繼續透過這些快速。

首先開啟`Basics.aspx`頁面中`EditDeleteDataList`資料夾並從 設計 檢視中，加入至頁面的 DataList。 接下來，從 DataList s 智慧標籤，建立新的 ObjectDataSource。 因為我們正與產品資料，將它設定為使用`ProductsBLL`類別。 若要擷取*所有*產品中，選擇`GetProducts()`選取索引標籤中的方法。


[![設定使用 ProductsBLL 類別 ObjectDataSource](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**圖 4**： 設定要使用 ObjectDataSource`ProductsBLL`類別 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))


[![傳回使用 GetProducts() 方法的產品資訊](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**圖 5**： 傳回產品資訊使用`GetProducts()`方法 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))


DataList，像是 GridView，不是插入新的資料;因此，請選取 （無）] 選項從下拉式清單中 [插入] 索引標籤。也選擇 （無） 更新和刪除索引標籤，因為會透過 BLL 以程式設計方式執行 [更新與刪除。


[![確認下拉式清單會列出在 Sqldatasource 插入、 更新和刪除索引標籤設為 （無）](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**圖 6**： 確認 ObjectDataSource 的插入、 更新和刪除索引標籤中的下拉式清單是否設定為 （無） ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))


設定 ObjectDataSource 之後, 按一下 完成，返回 設計工具。 為我們建立時自動完成的 ObjectDataSource 組態時，Visual Studio，在過去的範例中，看到 ve`ItemTemplate`的下拉式清單中，顯示每個資料欄位。 取代此項`ItemTemplate`與其中一個會顯示只有產品的名稱和價格。 此外，設定`RepeatColumns`屬性設為 2。

> [!NOTE]
> 中所述*概觀的插入、 更新和刪除的資料*教學課程中，修改資料使用我們的架構需要我們移除 ObjectDataSource 時`OldValuesParameterFormatString`從 ObjectDataSource 的屬性宣告式標記 (或其重設為其預設值`{0}`)。 在本教學課程中，不過，我們會使用 ObjectDataSource 僅適用於擷取資料。 因此，我們不需要修改 ObjectDataSource 的`OldValuesParameterFormatString`屬性值 (雖然它不 t 會降低，若要這樣做)。


取代預設 DataList 後`ItemTemplate`一個自訂頁面上的宣告式標記看起來應該如下所示：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

請花一點時間檢閱我們透過瀏覽器的進度。 如 [圖 7] 所示，DataList 會顯示在兩個資料行中的每個產品的產品名稱和單位價格。


[![產品名稱和優惠的價格會顯示在兩個資料行的資料清單](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**圖 7**: 產品名稱和價格會顯示在兩個資料行 DataList ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))


> [!NOTE]
> DataList 的所需的更新和刪除處理程序的屬性數目，這些值會儲存檢視狀態中。 因此，當建置 DataList 支援編輯或刪除資料，很重要，啟用 DataList 的檢視狀態。  
>   
>  精明的讀者可能還記得我們才能夠建立可編輯的 Gridview、 DetailsViews 和 FormViews 時停用檢視狀態。 這是因為 ASP.NET 2.0 Web 控制項可以包含*控制狀態*，它會在類似的檢視狀態，但會被視為必要回傳間保存狀態。


停用檢視 GridView 內的狀態只是省略了簡單的狀態資訊，但維持控制狀態 （其中包含所需的編輯和刪除的狀態）。 DataList，已經建立在 ASP.NET 1.x 時間範圍內，不會使用控制項狀態，因此必須啟用檢視狀態。 請參閱[vs 控制項狀態。檢視狀態](https://msdn.microsoft.com/library/1whwt1k7.aspx)如用途的控制項狀態，並從檢視狀態的不同方式的詳細資訊。

## <a name="step-4-adding-an-editing-user-interface"></a>步驟 4： 加入編輯的使用者介面

GridView 控制項是由欄位 （BoundFields CheckBoxFields、 TemplateFields，等等） 的集合所組成。 這些欄位可以調整其呈現的標記，視其模式而定。 例如，在唯讀模式中，BoundField 顯示其資料欄位值為文字;處於編輯模式時，它會轉譯文字方塊的 Web 控制項`Text`屬性會被指派資料欄位值。

DataList，相反地，會呈現其使用範本的項目。 唯讀項目會使用來呈現`ItemTemplate`而在編輯模式中的項目會轉譯透過`EditItemTemplate`。 到目前為止，我們 DataList 只有`ItemTemplate`。 若要支援項目層級編輯的功能，我們需要加入`EditItemTemplate`，其中包含可編輯的項目要顯示的標記。 本教學課程中，我們將使用 TextBox Web 控制項編輯產品的名稱與單位價格。

`EditItemTemplate`可以建立以宣告方式或透過設計工具 （從 DataList s 智慧標籤中選取 [編輯範本] 選項）。 若要使用 編輯範本 選項，請先按一下 編輯範本中的 連結的智慧標籤，然後按`EditItemTemplate`從下拉式清單中的項目。


[![使用 DataList 的 EditItemTemplate 選擇](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**圖 8**： 選擇使用 DataList s `EditItemTemplate` ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))


接下來，輸入產品名稱： 和價格:，然後拖曳兩個 TextBox 控制項，從工具箱拖曳到`EditItemTemplate`設計工具上的介面。 設定文字方塊`ID`屬性，以`ProductName`和`UnitPrice`。


[![產品名稱和價格加入文字方塊](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**圖 9**： 新增文字方塊中的產品名稱和價格 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))


我們要繫結到對應的產品資料欄位值`Text`兩個文字方塊的屬性。 從文字方塊的智慧標籤，按一下 [編輯資料繫結] 連結，並將關聯的適當的資料欄位`Text`屬性，如 [圖 10] 所示。

> [!NOTE]
> 當繫結`UnitPrice`價格文字方塊 s 的資料欄位`Text`欄位中，您可能會將它格式化為貨幣值 (`{0:C}`)，一般數字 (`{0:N}`)，或將它保留為未格式化。


![將 ProductName 和 UnitPrice 資料欄位繫結到文字方塊的 Text 屬性](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**圖 10**： 繫結`ProductName`並`UnitPrice`資料欄位加入`Text`文字方塊的屬性


請注意圖 10 中的 編輯資料繫結 對話方塊是怎麼*不*包含編輯為 TemplateField GridView 或 DetailsView 或 FormView 的範本時顯示的雙向資料繫結核取方塊。 雙向資料繫結功能允許自動指派給對應的 ObjectDataSource s 的輸入 Web 控制項的輸入值`InsertParameters`或`UpdateParameters`插入或更新資料時。 DataList 不支援雙向資料繫結，因為我們將於稍後看到在本教學課程中，使用者可讓她變更，並已準備好更新的資料之後，我們必須以程式設計方式存取這些文字方塊`Text`屬性，並傳入其值適當`UpdateProduct`方法中的`ProductsBLL`類別。

最後，我們需要新增更新] 和 [取消按鈕`EditItemTemplate`。 如我們在中所見[主要/詳細說明使用具有詳細資料 DataList 的主要記錄項目符號清單](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教學課程中，或按鈕時，LinkButton、 ImageButton 其`CommandName`設定 DataList 和 Repeater 內按一下從Repeater 或 DataList`ItemCommand`就會引發事件。 針對 DataList，如果`CommandName`屬性設定為某個值，也可能引發其他事件。 特殊`CommandName`包含屬性值，其他項目：

- 取消引發`CancelCommand`事件
- 編輯引發`EditCommand`事件
- 更新會引發`UpdateCommand`事件

請記住，這些事件會引發*除了*`ItemCommand`事件。

若要新增`EditItemTemplate`兩個按鈕 Web 控制項、 一個其`CommandName`設定為 更新和其他 s 設定為 取消。 在之後加入這兩個按鈕 Web 控制項設計工具看起來應該如下所示：


[![新增更新和取消 EditItemTemplate 的按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**圖 11**： 新增更新和取消按鈕，以`EditItemTemplate`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))


使用`EditItemTemplate`完整 DataList s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>步驟 5： 新增要進入編輯模式

此時我們 DataList 有編輯介面透過定義其`EditItemTemplate`; 不過，那里 s 目前無法讓使用者瀏覽我們的頁面來表示他想要編輯產品的資訊。 我們需要新增至每個產品，按一下時，會轉譯該 DataList 項目處於編輯模式的 [編輯] 按鈕。 新增至 [編輯] 按鈕開始`ItemTemplate`，透過設計工具或以宣告方式。 一定要在設定 [編輯] 按鈕的`CommandName`若要編輯的屬性。

您加入此 [編輯] 按鈕之後，請花一點時間檢視透過瀏覽器頁面。 此步驟中，每個產品清單應包含 [編輯] 按鈕。


[![新增更新和取消 EditItemTemplate 的按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**圖 12**： 新增更新和取消按鈕，以`EditItemTemplate`([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))


按一下按鈕會導致回傳，但卻*不*將進入編輯模式中列出的產品。 若要讓產品可供編輯，我們需要：

1. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)目的索引`DataListItem`只按下的 [編輯] 按鈕。
2. 重新繫結至 DataList 的資料。 DataList 時重新呈現`DataListItem`其`ItemIndex`DataList s 與相對應`EditItemIndex`會呈現使用其`EditItemTemplate`。

因為 DataList s`EditCommand`時會引發事件，按一下 [編輯] 按鈕時，建立`EditCommand`為下列程式碼的事件處理常式：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

`EditCommand`型別的物件傳遞的事件處理常式`DataListCommandEventArgs`做為其第二個輸入參數，其中包含的參考`DataListItem`按下的 [編輯] 按鈕 (`e.Item`)。 事件處理常式會先設定 DataList s`EditItemIndex`要`ItemIndex`的可編輯`DataListItem`，然後會重新繫結至 DataList 的資料藉由呼叫 DataList 的`DataBind()`方法。

加入這個事件處理常式之後, 重新瀏覽的瀏覽器頁面。 現在按一下 編輯 按鈕，讓已按下的產品可以讓您編輯 （請參閱 圖 13）。


[![按一下 [編輯] 按鈕可讓產品可編輯](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**圖 13**： 按一下 [編輯] 按鈕，讓產品可編輯 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))


## <a name="step-6-saving-the-user-s-changes"></a>步驟 6： 儲存使用者的變更

按一下 [取消] 按鈕的已編輯的產品 s Update 此時; 沒有任何作用若要新增此功能，我們需要建立事件處理常式的 DataList s`UpdateCommand`和`CancelCommand`事件。 開始建立`CancelCommand`事件處理常式中，按一下 編輯的產品 s 取消 按鈕和它負責傳回 DataList 預先編輯狀態時執行。

若要讓 DataList 呈現其所有項在唯讀模式中，我們需要：

1. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)不存在的索引`DataListItem`索引。 `-1` 是安全的選擇，因為`DataListItem`的索引開始`0`。
2. 重新繫結至 DataList 的資料。 因為沒有`DataListItem` `ItemIndex` es 對應至 DataList 的`EditItemIndex`，整個 DataList 會呈現在唯讀模式。

下列事件處理常式程式碼就可完成下列步驟：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

此步驟中，按一下 [取消] 按鈕便會回到 DataList 其預先編輯的狀態。

我們必須完成最後一個事件處理常式是`UpdateCommand`事件處理常式。 這個事件處理常式必須：

1. 以程式設計方式存取使用者輸入的產品名稱和價格，以及編輯的產品的`ProductID`。
2. 藉由呼叫適當的起始更新程序`UpdateProduct`多載中`ProductsBLL`類別。
3. 設定 DataList s [ `EditItemIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx)不存在的索引`DataListItem`索引。 `-1` 是安全的選擇，因為`DataListItem`的索引開始`0`。
4. 重新繫結至 DataList 的資料。 因為沒有`DataListItem` `ItemIndex` es 對應至 DataList 的`EditItemIndex`，整個 DataList 會呈現在唯讀模式。

步驟 1 和 2 會負責儲存使用者的變更;步驟 3 和 4 返回 DataList 預先編輯狀態所做的變更尚未儲存，並與執行中的步驟之後`CancelCommand`事件處理常式。

若要取得更新的產品名稱和價格，我們必須使用`FindControl`方法來以程式設計方式 TextBox Web 控制項內的參考`EditItemTemplate`。 我們也需要取得已編輯的產品的`ProductID`值。 當我們一開始會結合至 DataList 的 ObjectDataSource 時，Visual Studio 指派 DataList s`DataKeyField`屬性從資料來源的主索引鍵值 (`ProductID`)。 此值則可以擷取從 DataList 的`DataKeys`集合。 請花一點時間確認`DataKeyField`確實設定為`ProductID`。

下列程式碼會實作四個步驟：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

事件處理常式一開始會讀取已編輯的產品 s`ProductID`從`DataKeys`集合。 下一步，在兩個文字方塊`EditItemTemplate`參考和其`Text`屬性儲存在區域變數`productNameValue`和`unitPriceValue`。 我們會使用`Decimal.Parse()`方法來讀取的值從`UnitPrice`文字方塊中，因此，如果輸入的值有貨幣符號，它仍然正確可轉換為`Decimal`值。

> [!NOTE]
> 將值從`ProductName`和`UnitPrice`的文字方塊只會指派給在 productNameValue 和 unitPriceValue 變數如果文字方塊的 Text 屬性具有指定的值。 否則，值為`Nothing`用於變數中，使用資料庫來更新資料的效果`NULL`值。 也就是我們的程式碼會將空字串，以資料庫`NULL`是 GridView、 DetailsView 和 FormView 控制項的編輯介面的預設行為的值。


在讀取值之後`ProductsBLL`類別 s`UpdateProduct`呼叫方法，傳入 s 產品名稱、 價格和`ProductID`。 事件處理常式完成藉由使用完全相同的邏輯中做為其預先編輯狀態傳回 DataList`CancelCommand`事件處理常式。

具有`EditCommand`， `CancelCommand`，和`UpdateCommand`事件處理常式完成，訪客可以編輯的名稱和產品的價格。 圖 14-16 會顯示作用中的此編輯工作流程。


[![當第一次瀏覽頁面、 所有產品是在唯讀模式中](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**圖 14**： 所有的產品時第一次瀏覽的頁面，會在唯讀模式中 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))


[![若要更新的產品名稱的價格，請按一下 [編輯] 按鈕](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**圖 15**： 若要更新的產品名稱或價格，請按一下 [編輯] 按鈕 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))


[![在之後變更的值中，按一下 更新以唯讀模式返回](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**圖 16**： 在之後變更的值，按一下 [更新]，以回復成唯讀模式 ([按一下以檢視完整大小的影像](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))


## <a name="step-7-adding-delete-capabilities"></a>步驟 7： 加入刪除功能

將刪除功能新增至 DataList 的步驟會類似於將編輯功能。 簡單地說，我們需要加入至 [刪除] 按鈕`ItemTemplate`，按下時：

1. 會在對應的產品 s 中讀取`ProductID`透過`DataKeys`集合。
2. 藉由呼叫執行 delete`ProductsBLL`類別的`DeleteProduct`方法。
3. 重新繫結至 DataList 的資料。

讓 s 新增到 [刪除] 按鈕開始`ItemTemplate`。

按下按鈕時其`CommandName`編輯、 更新、 或取消引發 DataList s`ItemCommand`以及其他事件的事件 (例如，當使用編輯`EditCommand`以及引發事件)。 同樣地，任何按鈕、 LinkButton 或 ImageButton DataList 中的其`CommandName`屬性設定為刪除的原因`DeleteCommand`引發的事件 (連同`ItemCommand`)。

新增在 [編輯] 按鈕旁邊的 [刪除] 按鈕`ItemTemplate`，將其`CommandName`要刪除的屬性。 之後新增此按鈕會控制您 DataList 的`ItemTemplate`宣告式語法應該看起來像：


[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

接下來，建立事件處理常式，如 DataList 的`DeleteCommand`事件，使用下列程式碼：


[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

按一下 [刪除] 按鈕會造成回傳，並引發 DataList 的`DeleteCommand`事件。 在 事件處理常式中，按下的產品 s`ProductID`值從存取`DataKeys`集合。 接下來，呼叫刪除產品後`ProductsBLL`類別的`DeleteProduct`方法。

刪除產品之後, 它重要我們重新繫結至 DataList 的資料 (`DataList1.DataBind()`)，否則 DataList 會繼續顯示只是已刪除的產品。

## <a name="summary"></a>總結

雖然 DataList 缺少點，然後按一下 編輯和刪除支援藉由 GridView，簡短的程式碼稍微它可以增強以包括這些功能。 在本教學課程中，我們了解如何建立，無法被刪除，而且無法編輯其名稱和價格的產品的兩個資料行清單的內容。 加入編輯和刪除支援是包括在適當的 Web 控制項的事宜`ItemTemplate`和`EditItemTemplate`、 建立相對應的事件處理常式、 讀取使用者輸入和主要金鑰值，以及配合企業邏輯層。

雖然我們已加入基本編輯和刪除至 DataList 的功能，它會缺少更進階的功能。 比方說，沒有任何輸入的欄位驗證-如果使用者輸入太價格昂貴，將會擲回例外狀況`Decimal.Parse`時嘗試轉換太昂貴到`Decimal`。 同樣地，如果在更新商務邏輯的資料或資料存取層級問題使用者會看到標準錯誤畫面。 沒有任何一種確認刪除 按鈕，不小心刪除產品是所有太可能。

教學課程，我們會了解如何改善編輯的使用者體驗的未來。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones、 Ken Pespisa 和 Randy Schmidt。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](customizing-the-datalist-s-editing-interface-cs.md)
> [下一頁](performing-batch-updates-vb.md)
