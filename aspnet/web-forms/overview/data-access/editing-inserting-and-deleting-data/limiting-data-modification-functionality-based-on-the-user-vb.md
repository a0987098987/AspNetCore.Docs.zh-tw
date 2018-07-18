---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: 限制資料修改功能以使用者 (VB) |Microsoft Docs
author: rick-anderson
description: 允許使用者編輯資料的 web 應用程式，在不同的使用者帳戶可能有不同的資料編輯權限。 在本教學課程中我們將檢驗如何 t...
ms.author: aspnetcontent
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: 23e23c288ccceab7f7e1c07aa9a902bef4045de0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836805"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>限制資料修改功能，以使用者 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe)或[下載 PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> 允許使用者編輯資料的 web 應用程式，在不同的使用者帳戶可能有不同的資料編輯權限。 在本教學課程中，我們將檢驗如何動態調整造訪的使用者為基礎的資料修改功能。


## <a name="introduction"></a>簡介

許多 web 應用程式支援使用者帳戶，並提供不同的選項、 報表和登入的使用者為基礎的功能。 比方說，與我們的教學課程我們可能會想要允許使用者從供應商公司或許-供應商資訊，例如公司名稱，以及站台和更新的一般資訊及其產品，其名稱和數量，每個單位，來登入地址、 連絡人的資訊等等。 此外，我們也可能會想要從我們的公司的人包含部分使用者帳戶，以便他們可以登入並更新產品資訊，例如股票圖、 單位重新排序層級，等等。 我們的 web 應用程式也可能會允許匿名使用者造訪 （人不登入），但會限制它們只能檢視資料。 與這類使用者帳戶系統，我們會想在我們 ASP.NET 網頁中插入、 編輯和刪除功能適用於目前登入的使用者提供的資料 Web 控制項。

在本教學課程中，我們將檢驗如何動態調整造訪的使用者為基礎的資料修改功能。 特別是，我們將建立的 GridView 會列出供應商所提供的產品以及編輯 DetailsView 中顯示的供應商資訊的頁面。 如果使用者瀏覽的頁面是從我們的公司，他們可以： 可以檢視任何供應商的資訊;編輯其位址;然後編輯任何供應商所提供的產品資訊。 如果，不過，使用者是從某家公司，他們可以只檢視和編輯自己的位址資訊和只能編輯他們未標示為已停用的產品。


[![我們公司的使用者可以編輯任何供應商的資訊](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**圖 1**： 從我們的公司可以編輯任何供應商 s 資訊的使用者 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![從特定的供應商只能檢視和編輯其資訊的使用者](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**圖 2**： 從特定供應商可以只檢視和編輯其資訊的使用者 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


讓 s 開始 ！

> [!NOTE]
> ASP.NET 2.0 成員資格系統提供標準化、 可延伸的平台建立、 管理和驗證使用者帳戶。 由於成員資格系統的檢查是這些教學課程的範圍之外，本教學課程中改為 「 fakes"的成員資格允許匿名的使用者選擇是否是特定供應商，或從我們的公司。 如需成員資格的詳細資訊，請參閱我[檢查 ASP.NET 2.0 s 成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)系列文章。


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>步驟 1： 可讓使用者指定其存取權限

在真實世界的 web 應用程式，是否他們工作本公司或特定供應商，以及這項資訊會從我們的 ASP.NET 網頁以程式設計方式存取，使用者已登入站台之後，會包含使用者的帳戶資訊。 透過設定檔的系統，或透過一些自訂方法的使用者層級帳戶資訊，可以透過 ASP.NET 2.0 角色系統，擷取此資訊。

因為本教學課程的目的是為了示範調整登入的使用者為基礎的資料修改功能，而不是以 ASP.NET 2.0 的展示 s 成員資格、 角色和設定檔的系統，我們將使用非常簡單的機制來判斷瀏覽頁面-dropdownlist 進行的使用者可以指出是否他們應該能夠檢視及編輯任何供應商資訊，或者哪些使用者的功能也可以檢視和編輯的特定供應商的資訊。 如果使用者表示她可以檢視和編輯所有供應商資訊 （預設值），她可以逐頁瀏覽所有供應商、 編輯任何供應商 s 的位址資訊，並編輯所選取的供應商提供的任何產品的每單位的數量與名稱。 如果使用者表示，她只可檢視和編輯的特定供應商，不過，她可以只檢視該一家供應商的詳細資訊和產品然後只能更新名稱和每單位這些產品的資訊數量*不*停用。

在本教學課程中，我們第一個步驟，則要建立此 DropDownList 並填入供應商中系統中。 開啟`UserLevelAccess.aspx`頁面中`EditInsertDelete`資料夾中，新增 dropdownlist 進行其`ID`屬性設定為`Suppliers`，並將此 DropDownList 繫結至名為新 ObjectDataSource `AllSuppliersDataSource`。


[![建立名為 AllSuppliersDataSource 新 ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**圖 3**： 建立新的 ObjectDataSource 具名`AllSuppliersDataSource`([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


因為我們希望此 DropDownList 以包含所有的供應商，設定要叫用 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法。 也請確認 ObjectDataSource s`Update()`方法會對應至`SuppliersBLL`類別的`UpdateSupplierAddress`方法，為這個 ObjectDataSource 會也可供我們將加入在步驟 2 中 DetailsView。

完成 ObjectDataSource 精靈之後，完成設定步驟`Suppliers`DropDownList，它會顯示`CompanyName`資料欄位，以及使用`SupplierID`做為每個值的資料欄位`ListItem`。


[![設定供應商 DropDownList 以使用 [CompanyName] 和 SupplierID 資料欄位](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**圖 4**： 設定`Suppliers`使用 DropDownList`CompanyName`並`SupplierID`資料欄位 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


此時，DropDownList 列出資料庫中的供應商的公司名稱。 不過，我們也必須包含 DropDownList"Show/編輯所有供應商 」 選項。 若要達成此目的，將`Suppliers`DropDownList s`AppendDataBoundItems`屬性設`true`，然後加入`ListItem`其`Text`屬性是 「 Show/編輯所有供應商 」，而其值為`-1`。 這可以加入直接透過宣告式標記，或透過設計工具移至 [屬性] 視窗，然後按一下省略符號，DropDownList 的`Items`屬性。

> [!NOTE]
> 回頭[*主版/詳細篩選使用 DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教學課程，如需將所有選取的項目加入資料繫結 DropDownList 的更詳細討論。


在後`AppendDataBoundItems`屬性已設定和`ListItem`新增，DropDownList s 宣告式標記看起來應該像：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

透過瀏覽器檢視時，圖 5 顯示我們目前的進度的螢幕擷取畫面。


[![供應商 DropDownList 所有清單項目，再加上另一個用於每個供應商，包含顯示](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**圖 5**: `Suppliers` DropDownList 包含顯示所有`ListItem`，再加上一個每個供應商 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


因為我們想要使用者已變更其選取項目之後，立即更新的使用者介面，設定`Suppliers`DropDownList s`AutoPostBack`屬性設`true`。 在步驟 2 中，我們將建立會顯示的資訊取決於選擇的 DropDownList supplier(s) DetailsView 控制項。 接著，在步驟 3 中，我們要建立這個 DropDownList s 的事件處理常式`SelectedIndexChanged`事件，在其中我們將新增 DetailsView 所繫結的適當供應商資訊的程式碼會根據選取的供應商。

## <a name="step-2-adding-a-detailsview-control"></a>步驟 2： 加入 DetailsView 控制項

可讓 s 使用 DetailsView 來顯示供應商的資訊。 可以檢視和編輯所有供應商的使用者，DetailsView 會支援分頁，讓使用者能夠一次逐步供應商資訊一筆記錄。 如果使用者適用於特定的供應商，DetailsView 會顯示該特定供應商的資訊但不會包含分頁介面。 在任一情況下，DetailsView 需要允許使用者編輯供應商的地址、 城市和國家/地區欄位。

加入下方網頁的 DetailsView`Suppliers`下拉式清單中，設定其`ID`屬性設`SupplierDetails`，並將它繫結`AllSuppliersDataSource`上一個步驟中建立的 ObjectDataSource。 接下來，核取方塊啟用分頁，以及啟用編輯從 DetailsView s 智慧標籤。

> [!NOTE]
> 如果您不要看到智慧 DetailsView s 中的 [啟用編輯] 選項加以標記 s 因為未對應的 ObjectDataSource 秒`Update()`方法，以`SuppliersBLL`類別的`UpdateSupplierAddress`方法。 請花一點時間返回再做此變更之後，[啟用編輯] 選項應該會出現在 DetailsView s 智慧標籤的設定。


由於`SuppliersBLL`類別 s`UpdateSupplierAddress`方法只會接受四個參數- `supplierID`， `address`， `city`，和`country`-修改 DetailsView 的 BoundFields 以便`CompanyName`和`Phone`BoundFields 處於唯讀狀態。 此外，移除`SupplierID`BoundField 完全。 最後， `AllSuppliersDataSource` ObjectDataSource 目前有其`OldValuesParameterFormatString`屬性設定為`original_{0}`。 花點時間從宣告式語法完全移除此屬性設定值，或將它設定為預設值， `{0}`。

在設定後`SupplierDetails`DetailsView 和`AllSuppliersDataSource`ObjectDataSource，我們會有下列的宣告式標記：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

DetailsView 此時可以透過分頁，而且選取的供應商的位址資訊可以更新，不論所做的選擇`Suppliers`DropDownList （請參閱 圖 6）。


[![您可以檢視任何供應商資訊，並更新其位址](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**圖 6**： 任何供應商可以檢視資訊，並更新其位址 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>步驟 3： 顯示只有選取的供應商的資訊

我們的頁面目前顯示的所有供應商，不論是否已從選取的特定供應商資訊`Suppliers`DropDownList。 若要顯示供應商資訊所選的供應商我們要將另一個的 ObjectDataSource 新增至我們的頁面上，它會擷取特定的供應商的相關資訊。

加入新的 ObjectDataSource 頁面上，將它命名為`SingleSupplierDataSource`。 從它的智慧標籤，按一下 設定資料來源連結，讓它使用`SuppliersBLL`類別的`GetSupplierBySupplierID(supplierID)`方法。 如同`AllSuppliersDataSource`ObjectDataSource，具有`SingleSupplierDataSource`ObjectDataSource s`Update()`方法對應至`SuppliersBLL`類別的`UpdateSupplierAddress`方法。


[![設定為使用 GetSupplierBySupplierID(supplierID) 方法 SingleSupplierDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**圖 7**： 設定`SingleSupplierDataSource`使用 ObjectDataSource`GetSupplierBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


接下來，我們重新提示您指定的參數來源`GetSupplierBySupplierID(supplierID)`方法的`supplierID`輸入的參數。 因為我們想要顯示的資訊，從下拉式清單中，使用選取的供應商`Suppliers`DropDownList 的`SelectedValue`做為參數來源屬性。


[![供應商 DropDownList 做 supplierID 參數來源](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**圖 8**： 使用`Suppliers`做為 DropDownList`supplierID`參數的來源 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


新增第二個 ObjectDataSource，即使有了 DetailsView 控制項目前設定為一律使用`AllSuppliersDataSource`ObjectDataSource。 我們需要加入邏輯，以調整 DetailsView 取決於所使用的資料來源`Suppliers`選取 DropDownList 項目。 若要達成此目的，建立`SelectedIndexChanged`供應商 DropDownList 的事件處理常式。 這最容易建立按兩下設計工具中的 DropDownList。 這個事件處理常式必須決定要使用哪些資料來源，且必須重新繫結至 DetailsView 資料。 這是由下列程式碼來完成：


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

事件處理常式一開始會判斷是否已選取 "Show/編輯所有供應商 」 選項。 如果是，它會設定`SupplierDetails`DetailsView s`DataSourceID`要`AllSuppliersDataSource`並傳回給供應商的集合中第一筆記錄的使用者，藉由設定`PageIndex`屬性設為 0。 如果使用者不過，從下拉式清單中，在 DetailsView s 選取特定的供應商`DataSourceID`指派給`SingleSuppliersDataSource`。 無論何種資料來源使用，`SuppliersDetails`模式會還原回唯讀模式，且資料會重新繫結至 DetailsView 藉由呼叫`SuppliersDetails`控制的`DataBind()`方法。

使用就地這個事件處理常式，DetailsView 控制項現在會顯示所選的供應商，除非已選取"Show/編輯所有供應商 」 選項，在此情況下檢視所有的供應商透過分頁介面。 圖 9 顯示的頁面為"Show/編輯所有供應商 」 選項;請注意，分頁介面，讓使用者能夠瀏覽，並更新任何供應商。 圖 10 顯示頁面選取錦供應商。 錦的資訊會在此情況下是可檢視和編輯。


[![所有的供應商資訊可以檢視和編輯](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**圖 9**： 所有的供應商資訊檢視和編輯 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![只有選取的供應商的資訊可以檢視和編輯](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**圖 10**： 只有選取的供應商 s 資訊可以是 Viewed 並編輯 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> 本教學課程中，在 DropDownList 和 DetailsView 控制項 s`EnableViewState`必須設為`true`（預設值） 因為 DropDownList s`SelectedIndex`和 DetailsView 的`DataSourceID`屬性的變更必須記住在回傳之間。


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>步驟 4： 列出供應商產品的可編輯的 GridView

完成 DetailsView 中，我們下一步是要包含可編輯的 GridView 會列出所選取的供應商提供這些產品。 此 GridView 應該允許編輯為僅限`ProductName`和`QuantityPerUnit`欄位。 此外，如果使用者瀏覽的頁面是來自特定供應商，它應該只允許這些產品的更新*不*停用。 若要這麼做我們需要先將新增的多載`ProductsBLL`類別 s`UpdateProducts`採用的方法只`ProductID`， `ProductName`，和`QuantityPerUnit`做為輸入的欄位。 我們已事先在許多教學課程中，會逐步執行此程序讓 s 不妨看看程式碼，應該會加入到`ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

使用這個多載建立，我們準備好將 GridView 控制項和其相關聯的 ObjectDataSource。 新增至頁面的新 GridView、 設定其`ID`屬性，以`ProductsBySupplier`，並將它設定為使用名為新 ObjectDataSource `ProductsBySupplierDataSource`。 因為我們希望此 GridView，以列出所選的供應商提供的這些產品時，使用`ProductsBLL`類別的`GetProductsBySupplierID(supplierID)`方法。 也將對應`Update()`方法，以新`UpdateProduct`我們剛剛建立的多載。


[![設定要使用剛才建立的 UpdateProduct 多載 ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**圖 11**： 設定要使用 ObjectDataSource`UpdateProduct`多載只會建立 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


我們重新提示您選取參數的來源`GetProductsBySupplierID(supplierID)`方法的`supplierID`輸入的參數。 因為我們想要顯示的產品中使用 DetailsView 選取供應商`SuppliersDetails`DetailsView 控制項的`SelectedValue`做為參數來源屬性。


[![SuppliersDetails DetailsView 的 SelectedValue 屬性做為參數來源](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**圖 12**： 使用`SuppliersDetails`DetailsView s`SelectedValue`做為參數來源屬性 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


傳回至 GridView，移除所有的 GridView 欄位除外`ProductName`， `QuantityPerUnit`，並`Discontinued`標記、 `Discontinued` CheckBoxField 以唯讀模式。 此外，檢查 GridView s 智慧標籤的 啟用編輯選項。 在進行這些變更之後，GridView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

如同我們先前的 ObjectDataSources，此一 s`OldValuesParameterFormatString`屬性設定為`original_{0}`，當您嘗試更新產品的名稱或每個單位的數量時，這樣將會造成問題。 從宣告式語法中完全移除此屬性，或將它設定為其預設`{0}`。

此設定完成後，我們的頁面現在會列出在 GridView 中選取的供應商所提供的產品 （請參閱 圖 13）。 目前*任何*可以更新產品的名稱或每個單位的數量。 不過，我們需要更新我們的網頁邏輯，使這類功能禁止使用的特定供應商相關聯的使用者不再生產的產品。 我們將會處理在步驟 5 中的這個最後一個片段。


[![顯示所選取的供應商提供的產品](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**圖 13**： 會顯示所選取的供應商的產品提供 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> 加上此可編輯的 GridView `Suppliers` DropDownList 的`SelectedIndexChanged`應該更新事件處理常式，以恢復 GridView 唯讀狀態。 否則如果仍執行時編輯產品資訊中選取不同的供應商，則在新的供應商的 GridView 中對應的索引也會可編輯。 若要避免這個問題，請設定 GridView s`EditIndex`屬性，以`-1`在`SelectedIndexChanged`事件處理常式。


此外，您應該記得，是很重要的 GridView 的檢視狀態是已啟用 （預設行為）。 如果您將設定 GridView s`EnableViewState`屬性設`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行處理問題與 ASP.NET 2.0 Gridview/DetailsView/FormViews 該支援編輯和/或刪除與的 View State](http://scottonwriting.net/sowblog/posts/10054.aspx)如需詳細資訊。

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>步驟 5： 不允許編輯已停止的產品時顯示或編輯所有供應商是未選取

雖然`ProductsBySupplier`GridView 會正常運作，其目前授與過多存取來自特定供應商的使用者。 根據我們的商務規則，這類使用者應該無法更新已停止的產品。 若要強制這種情況，我們可以隱藏 （或停用） 已停止的產品時所瀏覽頁面時由使用者從供應商的 GridView 資料列的 [編輯] 按鈕。

建立事件處理常式 GridView s`RowDataBound`事件。 這個事件處理常式中，我們需要判斷使用者是否為特定的供應商，而這，本教學課程中，可決定藉由檢查供應商 DropDownList s 相關聯`SelectedValue`屬性--如果它是 s 以外的項目-1，則使用者相關聯的特定供應商。 這類使用者，我們再需要判斷產品已停止。 我們可以擷取參考的實際`ProductRow`執行個體繫結至 GridView 資料列，透過`e.Row.DataItem`屬性中所述[ *GridView s 頁尾顯示摘要資訊*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)本教學課程。 如果產品已經停售，我們可以抓取 GridView s CommandField 使用先前的教學課程中討論的技術中的 [編輯] 按鈕的程式設計參考[*新增用戶端確認時刪除*](adding-client-side-confirmation-when-deleting-vb.md). 一旦我們擁有我們再以隱藏或停用按鈕的參考。


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

與這個事件處理常式的位置，當瀏覽此頁面的使用者身分從特定的供應商已停用這些產品都不是可編輯的為 [編輯] 按鈕會隱藏這些產品。 比方說，Chef Anton 的 Gumbo 混合是紐奧良印地安 Delights 供應商停產的產品。 當這個特定的供應商，瀏覽的頁面，此產品的 編輯 按鈕隱藏看不到 （請參閱 圖 14）。 不過，瀏覽時使用 「 顯示或編輯所有供應商 」，編輯 按鈕會是 可用 （請參閱 圖 15）。


[![Chef Anton s Gumbo 混用 [編輯] 按鈕會隱藏特定供應商的使用者](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**圖 14**： 針對供應商特定的使用者隱藏 Chef Anton s Gumbo 混用 [編輯] 按鈕 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![Chef Anton s Gumbo 混用 [編輯] 按鈕會顯示所有供應商使用者顯示或編輯，](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**圖 15**： 為顯示或編輯所有供應商的使用者、 Chef Anton s Gumbo 混合會顯示 [編輯] 按鈕 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>檢查商務邏輯層中的存取權限

在本教學課程的 ASP.NET 頁面可處理使用者可以看到哪些資訊與所有邏輯，而且他可以更新哪些產品。 在理想情況下，此邏輯也會出現在商業邏輯層。 例如，`SuppliersBLL`類別 s `GetSuppliers()` （它會傳回所有的供應商） 的方法可能會包含檢查，以確保目前登入的使用者*不*特定供應商相關聯。 同樣地，`UpdateSupplierAddress`方法可能包括檢查，以確認目前登入的使用者請本公司合作 （並因此可以更新供應商的所有位址資訊） 或其資料正在更新供應商相關聯。

因為在本教學課程中的使用者 s 權限取決於在頁面上，BLL 類別無法存取 dropdownlist 進行，所以我不包括這類 BLL 層檢查。 使用成員資格系統，或其中一個 （例如 Windows 驗證）、 ASP.NET 所提供的方塊外的驗證配置時，目前登上使用者 s 存取資訊和角色資訊可以從 BLL，因此這類存取權權限檢查可能的簡報和 BLL 層級。

## <a name="summary"></a>總結

大部分的網站提供的使用者帳戶需要自訂資料修改介面，根據登入的使用者。 系統管理使用者可以刪除和編輯任何記錄，而非系統管理使用者可能會限制為僅更新或刪除自己建立的記錄。 諸如此類的情節可能是的資料 Web 控制項，ObjectDataSource，且商務邏輯層類別可加以擴充，若要新增或拒絕登入的使用者為基礎的特定功能。 在本教學課程中，我們看到如何限制根據使用者是否為特定的供應商相關聯，或如果他們工作本公司可檢視和編輯資料。

本教學課程結束時，我們檢查插入、 更新和刪除使用 GridView、 DetailsView 和 FormView 控制項的資料。 從開始下一個教學課程，我們將把焦點轉到新增分頁和排序支援。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](adding-client-side-confirmation-when-deleting-vb.md)
