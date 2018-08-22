---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: 執行批次更新 (VB) |Microsoft Docs
author: rick-anderson
description: 了解如何建立完全可編輯模式，且其值可以儲存更新全部 按鈕，即可，其中所有的項目位於的 DataList 編輯...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 67ab034880c8140e6156721956059b7cdd3f077b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830220"
---
<a name="performing-batch-updates-vb"></a>執行批次更新 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe)或[下載 PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> 了解如何建立完全可編輯 DataList 其中所有的項目位於編輯模式，以及要儲存其值，請按一下頁面上的 [全部更新] 按鈕。


## <a name="introduction"></a>簡介

在 [前述教學課程](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)我們檢查如何建立項目層級 DataList。 例如包含標準的可編輯 GridView，DataList 中每個項目編輯 按鈕，按下時，會讓項目可供編輯。 雖然這項目層級編輯適用於只會偶爾會更新的資料，則特定使用案例會需要使用者編輯多筆記錄。 如果使用者需要編輯數十個記錄，並且會強制按一下 編輯，進行其的變更，並按一下 每個更新，按一下  的數量可能會拖累她產能。 在這種情況下，更好的選項是提供完全可編輯的 DataList，那個*所有*其項目處於編輯模式，而且其值可以編輯的頁面上的全部更新] 按鈕，即可 （請參閱 [圖 1）。


[![在完整的可編輯 DataList 中每個項目可以進行修改。](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**圖 1**： 在完整的可編輯 DataList 中的每個項目可以進行修改 ([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image3.png))


在本教學課程中，我們將檢驗如何讓使用者可以更新供應商使用完整的可編輯的 DataList 的地址資訊。

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>步驟 1： 建立可編輯的使用者介面中 DataList 的 ItemTemplate

在先前教學課程中，是建立標準的項目層級的可編輯 DataList，我們使用兩個範本：

- `ItemTemplate` 包含唯讀的使用者介面 （Label Web 控制項來顯示每個產品的名稱和價格）。
- `EditItemTemplate` 包含編輯模式的使用者介面 （兩個 TextBox Web 控制項）。

DataList s`EditItemIndex`屬性會指定什麼`DataListItem`（如果有的話） 會使用呈現`EditItemTemplate`。 特別是，`DataListItem`其`ItemIndex`值符合 DataList s`EditItemIndex`屬性使用呈現`EditItemTemplate`。 只有一個項目都是可編輯的時間，但已落在間隔在建立完全可編輯的 DataList 時，此模型會運作。

對於完全可進行編輯的資料清單中，我們希望*所有*的`DataListItem`s 使用的可編輯的介面來呈現。 若要這麼做最簡單的方法是定義中的可編輯介面`ItemTemplate`。 修改的供應商的位址資訊，可編輯的介面會包含供應商名稱做為文字，然後文字方塊的地址、 城市和國家/地區值。

首先開啟`BatchUpdate.aspx`頁面上，新增 DataList 控制項，然後設定其`ID`屬性設`Suppliers`。 從 DataList s 智慧標籤，選擇 加入新的 ObjectDataSource 控制項，名為`SuppliersDataSource`。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**圖 2**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image6.png))


設定要使用擷取資料的 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法 （請參閱 [圖 3]）。 與先前的教學課程中，而不是更新透過 ObjectDataSource 的供應商資訊，我們會直接處理商務邏輯層。 因此，在 更新 索引標籤中設定為 （無） 下拉式清單 （請參閱 圖 4）。


[![擷取使用 GetSuppliers() 方法的供應商資訊](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**圖 3**： 擷取供應商資訊使用`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image9.png))


[![在 [更新] 索引標籤中設定為 （無） 下拉式清單](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**圖 4**： 在 [更新] 索引標籤中設定為 （無） 下拉式清單 ([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image12.png))


完成精靈之後，Visual Studio 會自動產生 DataList 的`ItemTemplate`顯示標籤 Web 控制項中的資料來源傳回的每個資料欄位。 我們需要修改此範本，使其改為提供的編輯介面。 `ItemTemplate`可以自訂透過設計工具使用的 DataList s 智慧標籤的 [編輯範本] 選項，或直接透過宣告式語法。

請花一點時間建立編輯介面，顯示供應商的名稱為文字，但包含供應商的地址、 城市和國家/地區值的文字方塊。 進行這些變更之後，頁面 s 的宣告式語法看起來應該如下所示：


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> 如先前的教學課程中，在本教學課程 DataList 必須啟用其檢視狀態。


在`ItemTemplate`我使用兩個新的 CSS 類別、 m`SupplierPropertyLabel`和`SupplierPropertyValue`，其中已加入至`Styles.css`類別，並設定為使用相同的樣式設定為`ProductPropertyLabel`和`ProductPropertyValue`CSS 類別。


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

進行這些變更之後，請瀏覽此頁面，透過瀏覽器。 如 [圖 5] 所示，每個 DataList 項目會顯示為文字的供應商名稱，並使用文字方塊顯示地址、 城市和國家/地區。


[![DataList 中的每個供應商是可編輯](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**圖 5**: DataList 中的每個供應商是可編輯 ([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>步驟 2： 加入更新所有的按鈕

雖然 [圖 5] 中的每個供應商及其地址、 城市和國家/地區欄位顯示在文字方塊中，目前沒有任何 [更新] 按鈕可用。 而不是讓每個項目的 更新 按鈕，以完全可進行編輯 DataLists 則通常單一的全部更新 按鈕的頁面上，按一下時，更新*所有*DataList 中的記錄。 本教學課程中，可讓 s （雖然按一下任一按鈕會有相同的效果），新增兩個全部更新 按鈕-在頁面頂端和底部的其中一個。

藉由新增按鈕 Web 控制項上方 DataList 組的開始及其`ID`屬性設`UpdateAll1`。 接下來，新增第二個按鈕 Web 控制項下方 DataList 和設定其`ID`至`UpdateAll2`。 設定`Text`兩個按鈕來更新所有屬性。 最後，建立兩個按鈕事件處理常式`Click`事件。 而不是複製每個事件處理常式中的更新邏輯，可讓 s 重構至第三個方法，該邏輯`UpdateAllSupplierAddresses`，有直接叫用這個第三個方法的事件處理常式。


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

全部更新] 按鈕後，[圖 6 顯示的頁面。


[![兩個更新的所有按鈕已都加入至頁面](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**圖 6**： 頁面已加入兩個更新的所有按鈕 ([按一下以檢視完整大小的影像](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>步驟 3： 更新所有供應商的地址資訊

所有顯示的編輯介面的 DataList s 項目，再加上的 [全部更新] 按鈕的仍然撰寫程式碼來執行批次更新。 具體而言，我們需要循環 DataList s 項目並呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`針對每個方法。

集合`DataListItem`執行個體可以透過 DataList s 存取 DataList 該結構[`Items`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)。 參考`DataListItem`，我們可以擷取相對應`SupplierID`從`DataKeys`集合並以程式設計方式參考文字方塊 Web 控制項內`ItemTemplate`如下列程式碼所示：


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

當使用者按一下 [全部更新] 按鈕，其中`UpdateAllSupplierAddresses`方法逐一查看每個`DataListItem`中`Suppliers`DataList 和呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法並傳入對應的值。 地址、 縣市或國家/地區傳遞非輸入值是值為`Nothing`要`UpdateSupplierAddress`（而不是空白的字串），這會導致資料庫`NULL`基礎記錄 s 欄位。

> [!NOTE]
> 為增強功能，您可以提供一些確認訊息，執行批次更新後的頁面加入標籤 Web 控制項的狀態。


## <a name="updating-only-those-addresses-that-have-been-modified"></a>更新只有在已修改的位址

用於此教學課程的呼叫的批次更新演算法`UpdateSupplierAddress`方法*每個*DataList，不論是否已變更其位址資訊中的供應商。 而這類 blind 更新通常並不是效能問題，它們可能會導致過多記錄您稽核變更為資料庫資料表。 例如，如果您使用觸發程序來記錄所有`UPDATE`s`Suppliers`資料表至稽核資料表，每次使用者按一下 [全部更新] 按鈕，為每個供應商在系統中，不論使用者是否進行任何中建立新的稽核記錄變更。

ADO.NET DataTable 和資料配接器類別被設計來支援只修改、 刪除及新的記錄會導致任何資料庫通訊的批次更新。 在 DataTable 中的每個資料列具有[`RowState`屬性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)，指出資料列已加入至 DataTable，從它修改、 刪除或維持不變。 當一開始填入 DataTable 時，所有資料列會標示不變。 變更任何資料列 s 資料行的值標示資料列，為已修改。

在 `SuppliersBLL`我們更新成單一的供應商記錄中的第一個讀取指定的供應商的位址資訊的類別`SuppliersDataTable`，然後設定`Address`， `City`，和`Country`使用下列程式碼的資料行值：


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

傳入的地址、 城市和國家/地區值，這個會將指派此程式碼`SuppliersRow`在`SuppliersDataTable`不論是否有變更的值。 這些修改會導致`SuppliersRow`s`RowState`會標示為已修改的屬性。 當 s 中的資料存取層`Update`呼叫方法，它會看到`SupplierRow`已經過修改，並因此傳送`UPDATE`命令到資料庫。

想像一下，我們加入這個方法，以僅指派 傳入的地址、 城市和國家/地區值，如果從不同的程式碼`SuppliersRow`s 現有的值。 在現有的資料相同，是地址、 城市和國家/地區的情況下，將會進行任何變更， `SupplierRow` s`RowState`會被標示為保持不變。 最後結果就是，當的 DAL s`Update`呼叫方法，將會進行任何資料庫的呼叫，因為`SuppliersRow`未遭修改。

若要制定這項變更，來取代盲目地指派傳入的地址、 城市和國家/地區值，以下列程式碼的陳述式：


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

以此方式新增程式碼中，DAL s`Update`方法傳送`UPDATE`陳述式，以其地址相關的值已變更的記錄的資料庫。

或者，我們無法追蹤是否有任何傳入的地址欄位和資料庫的資料之間的差異，並，如果沒有任何項目，只是略過呼叫 DAL 的`Update`方法。 此方法適用於您正在使用資料庫直接方法，因為資料庫直接的方法不是 t 傳遞`SuppliersRow`執行個體，其`RowState`可以檢查以判斷是否確實需要資料庫呼叫為止。

> [!NOTE]
> 每次`UpdateSupplierAddress`叫用方法時，呼叫就會建立資料庫中，以擷取已更新資料錄的相關資訊。 然後，如果資料中有任何變更，另一個資料庫進行呼叫以更新資料表資料列。 此工作流程可以透過建立最佳化`UpdateSupplierAddress`方法多載，接受`EmployeesDataTable`具有執行個體*所有*變更的`BatchUpdate.aspx`頁面。 然後，這也可能導致資料庫取得所有記錄的一個呼叫`Suppliers`資料表。 然後要列舉的兩個結果集，因此無法更新已發生變更的記錄。


## <a name="summary"></a>總結

在本教學課程中，我們了解如何建立完全可進行編輯的 DataList，讓使用者能夠快速修改多個供應商的位址資訊的內容。 我們開始著手定義 DataList s 中的供應商的地址、 城市和國家/地區值的文字方塊中的 Web 控制項的編輯介面`ItemTemplate`。 接下來，我們會新增上方和下方 DataList 的全部更新 按鈕。 使用者已做了他的變更，並按下其中一個全部更新 按鈕之後, `DataListItem` s 會列舉並呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones 和 Ken Pespisa。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [下一頁](handling-bll-and-dal-level-exceptions-vb.md)
