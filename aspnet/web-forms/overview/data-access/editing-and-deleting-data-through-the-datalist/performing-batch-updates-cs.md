---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: 執行批次更新 (C#) |Microsoft 文件
author: rick-anderson
description: 了解如何建立完全可編輯 DataList 其中所有的項目位於編輯模式，而且其值可以儲存 ' 全部更新 按鈕，即可...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: af19104edb1849272773193befe1f5b2c7347683
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="performing-batch-updates-c"></a>執行批次更新 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe)或[下載 PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> 了解如何建立完全可編輯 DataList 其中所有的項目位於編輯模式，而且其值可以儲存在頁面上的 [更新全部] 按鈕，即可。


## <a name="introduction"></a>簡介

在[前述教學課程](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)我們檢查如何建立項目層級資料清單。 像是標準可以讓您編輯 GridView 資料清單中的每個項目包含編輯 按鈕，按下時，會讓項目可供編輯。 雖然這項目層級編輯適用於僅會不定期更新的資料，則特定使用案例會需要使用者編輯多筆記錄。 如果使用者需要編輯數十個記錄，並且會強制按一下 編輯、 其變更，並按一下每個更新，按一下 的數量可能會妨礙其產能。 在這種情況下，較好的選擇是要提供完全可編輯的資料清單中，一個 where*所有*的項目會處於編輯模式，可以編輯其值，依序按一下 [全部更新] 頁面上的按鈕 （請參閱圖 1）。


[![可以修改每個完整的可編輯 DataList 項目](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**圖 1**： 完整的可編輯 DataList 中的每個項目可以進行修改 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image3.png))


在本教學課程中，我們將檢驗如何讓使用者以更新使用完全可進行編輯 DataList 供應商位址資訊。

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>步驟 1: S DataList ItemTemplate 中建立的可編輯的使用者介面

前述教學課程中，我們建立標準的項目層級的可編輯 DataList，我們使用的位置兩個範本：

- `ItemTemplate` 包含唯讀的使用者介面 （Label Web 控制項來顯示每個產品的名稱與價格）。
- `EditItemTemplate` 包含編輯模式使用者介面 （兩個文字方塊中的 Web 控制項）。

DataList s`EditItemIndex`屬性規定哪些`DataListItem`（如果有的話） 使用轉譯`EditItemTemplate`。 特別是，`DataListItem`其`ItemIndex`值符合 DataList s`EditItemIndex`屬性使用呈現`EditItemTemplate`。 只有一個項目都是可編輯的時間，但分開的落在建立完全可編輯的資料清單時，此模型會運作。

對於完全可進行編輯的資料清單中，我們希望*所有*的`DataListItem`s 來呈現使用可編輯的介面。 若要完成這項作業最簡單的方式是定義中的可編輯介面`ItemTemplate`。 修改供應商位址資訊，可編輯的介面會包含地址、 縣市和國家 （地區） 的值做為文字，再文字方塊供應商名稱。

先開啟`BatchUpdate.aspx`頁面上，加入 DataList 控制項，然後設定其`ID`屬性`Suppliers`。 從 DataList s 智慧標籤，選擇 加入新的 ObjectDataSource 控制項，名為`SuppliersDataSource`。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**圖 2**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image6.png))


設定要使用擷取資料 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法 （請參閱圖 3）。 與先前的教學課程中，而非更新供應商的資訊透過 ObjectDataSource，我們將直接使用商務邏輯層。 因此，在 更新 索引標籤中設定下拉式清單，為 （無） （請參閱圖 4）。


[![擷取使用 GetSuppliers() 方法的供應商資訊](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**圖 3**： 擷取供應商資訊使用`GetSuppliers()`方法 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image9.png))


[![在 [更新] 索引標籤中設定為 （無） 下拉式清單](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**圖 4**： 在 [更新] 索引標籤中設定為 （無） 下拉式清單 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image12.png))


完成精靈之後，Visual Studio 會自動產生 DataList 的`ItemTemplate`顯示標籤 Web 控制項中的資料來源傳回的每個資料欄位。 我們需要修改此範本，使其改為提供編輯介面。 `ItemTemplate`可以自訂透過使用從 DataList s 智慧標籤的 [編輯樣板] 選項設計工具，或直接透過宣告式語法。

請花一點時間來建立供應商的名稱顯示為文字，但包含供應商 s 地址、 縣市和國家 （地區） 值的文字方塊的編輯介面。 進行這些變更之後，頁面 s 的宣告式語法看起來應該如下所示：


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> 為先前的教學課程中，在本教學課程 DataList 必須具有啟用其檢視狀態。


在`ItemTemplate`我使用兩個新的 CSS 類別、 m`SupplierPropertyLabel`和`SupplierPropertyValue`，其中已新增至`Styles.css`類別，並設定為使用相同的樣式設定`ProductPropertyLabel`和`ProductPropertyValue`CSS 類別。


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

進行這些變更之後，請瀏覽此頁面，透過瀏覽器。 如圖 5 所示，每個 DataList 項目顯示為文字的供應商名稱，並使用文字方塊顯示地址、 縣市和國家/地區。


[![在 DataList 中的每個供應商是編輯](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**圖 5**: DataList 中的每個供應商是編輯 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>步驟 2： 加入更新所有的按鈕

圖 5 中的每個供應商具有其地址、 縣市和國家/地區欄位顯示在文字方塊中，而目前沒有任何更新 按鈕。 而不是讓每個項目 更新 按鈕，以完全可進行編輯 DataLists 則通常單一全部更新 按鈕在頁面上，按一下時，更新*所有*DataList 的記錄。 本教學課程，可讓 s （雖然按一下任一個 按鈕會有相同的效果） 中加入兩個全部更新 按鈕-一頁頂端和底部的其中一個。

藉由新增按鈕 Web 控制項上方 DataList 組開始其`ID`屬性`UpdateAll1`。 接下來，加入第二個按鈕 Web 控制項下方 DataList，設定其`ID`至`UpdateAll2`。 設定`Text`要更新所有的兩個按鈕的屬性。 最後，建立兩個按鈕的事件處理常式`Click`事件。 而不是複製每個事件處理常式中的更新邏輯，可讓 s 重構給第三個方法時，該邏輯`UpdateAllSupplierAddresses`，有直接叫用這個第三個方法的事件處理常式。


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

圖 6 顯示頁面之後已加入全部更新 按鈕。


[![兩個更新的所有按鈕已都加入至頁面](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**圖 6**： 兩個更新的所有按鈕已都加入至頁面 ([按一下以檢視完整大小的影像](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>步驟 3： 更新的所有供應商的位址資訊

所有顯示的編輯介面 DataList s 項目加上的 [全部更新] 按鈕，而所有維持正在寫入的程式碼執行批次更新。 具體而言，我們需要迴圈 DataList s 項目並呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`針對每個方法。

集合`DataListItem`DataList 可以透過 DataList s 該結構的執行個體[`Items`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)。 參考`DataListItem`，我們可以抓取對應`SupplierID`從`DataKeys`集合並以程式設計方式參考的文字方塊中 Web 控制項內`ItemTemplate`如下列程式碼所示：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

當使用者按一下 [全部更新] 按鈕，其中`UpdateAllSupplierAddresses`方法逐一查看每個`DataListItem`中`Suppliers`DataList 和呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法，傳入的對應值。 地址、 縣市或國家/地區會傳遞非輸入值是值為`Nothing`至`UpdateSupplierAddress`（而不是空白的字串），這會導致資料庫`NULL`基礎 s 記錄欄位。

> [!NOTE]
> 為了加強，您可以加入標籤 Web 控制項的狀態可提供一些確認訊息，執行批次更新後的頁面。


## <a name="updating-only-those-addresses-that-have-been-modified"></a>更新已修改這些地址

用於此教學課程的呼叫的批次更新演算法`UpdateSupplierAddress`方法*每*供應商在 DataList，不論是否已變更其位址資訊。 這類 blind 更新通常並不是效能問題，而它們可能會導致過多記錄如果作為稽核您變更至資料庫資料表。 例如，如果您使用觸發程序來記錄所有`UPDATE`s`Suppliers`資料表至稽核的資料表，每次使用者按一下 [全部更新] 按鈕，每個供應商在系統中，不論使用者是否進行任何建立新的稽核記錄變更。

ADO.NET DataTable 和資料配接器類別被設計來支援批次更新只修改、 刪除及新的記錄會導致任何資料庫進行通訊。 在 DataTable 中的每個資料列都有[`RowState`屬性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)，指出是否已加入至 DataTable，從它修改、 刪除或維持不變的資料列。 當一開始填入 DataTable 時，所有資料列會標示為不變。 變更任何資料列 s 資料行的值標示資料列，為已修改。

在`SuppliersBLL`我們更新成單一的供應商記錄中的第一個讀取指定的供應商 s 地址資訊的類別`SuppliersDataTable`，然後設定`Address`， `City`，和`Country`使用下列程式碼的資料行值：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

傳入的地址、 縣市和國家 （地區） 值，這個會指派此程式碼`SuppliersRow`中`SuppliersDataTable`不論是否已變更的值。 這些修改會導致`SuppliersRow`s`RowState`將會標示為已修改的屬性。 當資料存取層 s`Update`方法呼叫，它會看到`SupplierRow`已修改，因此傳送`UPDATE`命令到資料庫。

想像一下，我們將程式碼加入至這個方法，只能指定傳入的地址、 縣市和國家 （地區） 值，如果有何差異的`SuppliersRow`s 現有的值。 地址、 縣市和國家/地區的現有資料相同的情況下，將會進行任何變更， `SupplierRow` s`RowState`會被標示為不變。 最後結果就是，當 DAL s`Update`方法呼叫，將會進行任何資料庫的呼叫，因為`SuppliersRow`尚未修改。

若要制定這項變更，取代盲目地指派傳入的地址、 縣市和國家 （地區） 值，以下列程式碼陳述式：


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

與此加入程式碼中，DAL s`Update`方法傳送`UPDATE`陳述式的資料庫，其地址相關的值已變更的記錄。

或者，我們無法追蹤是否有任何傳入的地址欄位和資料庫資料之間的差異，並，如果沒有任何項目，只是略過呼叫 DAL 的`Update`方法。 這個方法適用於您會使用 DB 直接方法，因為傳遞的 DB 直接方法目前 t 如果`SuppliersRow`執行個體，其`RowState`可以檢查以判斷是否確實需要資料庫的呼叫。

> [!NOTE]
> 每次`UpdateSupplierAddress`叫用方法時，資料庫進行呼叫以擷取有關更新的記錄資訊。 然後，如果資料中有任何變更，另一個資料庫呼叫來更新資料表資料列。 此工作流程無法最佳化藉由建立`UpdateSupplierAddress`方法多載接受`EmployeesDataTable`具有執行個體*所有*從變更的`BatchUpdate.aspx`頁面。 然後，它可以讓一個呼叫資料庫取得所有記錄的`Suppliers`資料表。 然後可以列舉的兩個結果集，因此無法更新已發生變更的記錄。


## <a name="summary"></a>總結

在此教學課程中我們可了解如何建立完整的可編輯 DataList，讓使用者能夠快速修改多個供應商的地址資訊。 我們已開始編輯介面供應商 s 地址、 縣市和國家 （地區） 值的文字方塊中的 Web 控制項定義在 DataList 的`ItemTemplate`。 接下來，我們加入上方和下方 DataList 全部更新 按鈕。 使用者在其變更，並按下的 [全部更新] 按鈕，其中一個之後`DataListItem`s 會列舉而呼叫`SuppliersBLL`類別的`UpdateSupplierAddress`方法進行。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Zack Jones，Ken Pespisa。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [下一頁](handling-bll-and-dal-level-exceptions-cs.md)
