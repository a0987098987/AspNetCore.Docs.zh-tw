---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: 限制資料修改功能會根據使用者 (VB) |Microsoft 文件
author: rick-anderson
description: 允許使用者編輯資料的 web 應用程式，在不同的使用者帳戶可能有不同的資料編輯權限。 在本教學課程中我們將檢驗如何 t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: eccb8e114e0ecf772680a2e5b36a761736cf8025
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887869"
---
<a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>限制使用者 (VB) 為基礎的資料修改功能
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe)或[下載 PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> 允許使用者編輯資料的 web 應用程式，在不同的使用者帳戶可能有不同的資料編輯權限。 在本教學課程中，我們將檢驗如何動態調整正在瀏覽的使用者為基礎的資料修改功能。


## <a name="introduction"></a>簡介

許多 web 應用程式支援的使用者帳戶，並提供不同的選項、 報表和已登入使用者為基礎的功能。 比方說，與教學課程我們可能會想要允許使用者登入站台和更新的一般資訊其產品，其名稱和單位，數量可能是-以及供應商資訊、 他們的公司名稱，例如供應商公司地址、 連絡人的資訊等等。 此外，我們可能需要以包含部分使用者帳戶，以便我們公司的人員，使他們可以登入和更新產品資訊，像是單位股票圖、 重新排序層級，等等。 我們的 web 應用程式也可能會允許匿名使用者瀏覽 （人不登入），但會限制他們只能檢視資料。 此使用者帳戶系統位置中，我們可能會想在我們 ASP.NET 網頁中插入、 編輯和刪除功能適用於目前登入的使用者提供的 Web 控制項的資料。

在本教學課程中，我們將檢驗如何動態調整正在瀏覽的使用者為基礎的資料修改功能。 特別是，我們將建立顯示供應商資訊中會列出供應商所提供的產品的 GridView 以及編輯 DetailsView 的頁面。 如果是從本公司的瀏覽頁面的使用者，他們可以： 可以檢視任何供應商的資訊;編輯其位址。並編輯任何產品供應商所提供的資訊。 如果，不過，是從某家公司的使用者，他們可以只檢視和編輯自己的位址資訊和只能編輯其未標示為已中止的產品。


[![我們公司的使用者可以編輯任何供應商的資訊](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**圖 1**： 使用者從我們公司可以編輯任何供應商的資訊 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))


[![從特定供應商只能檢視和編輯其資訊的使用者](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**圖 2**： 使用者從特定供應商可以只檢視和編輯其資訊 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))


Let s 開始 ！

> [!NOTE]
> ASP.NET 2.0 s 成員資格系統提供標準化、 可延伸的平台，用於建立、 管理及驗證使用者帳戶。 因為成員資格系統檢查超出這些教學課程的範圍，本教學課程改為"fakes"成員資格允許匿名的使用者選擇他們是否從特定供應商或本公司。 如需成員資格的詳細資訊，請參閱我[檢查 ASP.NET 2.0 s 成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)文章系列。


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>步驟 1： 可讓使用者指定其存取權限

在真實世界的 web 應用程式中，使用者的帳戶資訊會包括是否我們公司或特定供應商，他們的工作，因此這項資訊會以程式設計方式存取從我們的 ASP.NET 網頁，一旦使用者已登入站台。 這項資訊可以透過 ASP.NET 2.0 的角色的系統，被擷取成使用者層級帳戶資訊，透過設定檔的系統，或透過一些自訂的方式。

由於本教學課程的目的在於示範調整登入的使用者為基礎的資料修改功能，而且並非用於展示 ASP.NET 2.0 s 成員資格、 角色和設定檔的系統，我們會使用非常簡單的機制來判斷使用者瀏覽頁面-從中使用者可以指出是否它們應該能夠檢視和編輯任何供應商資訊或，或者，目標的 DropDownList 功能也可以檢視和編輯的特定供應商的資訊。 如果使用者表示她可以檢視和編輯所有供應商資訊 （預設值），她可以逐頁查看所有供應商、 編輯任何供應商的位址資訊和編輯的名稱和任何所選取的供應商提供的產品單位數量。 如果使用者表示，她只能檢視和編輯特定供應商，不過，然後她可以只有一個供應商檢視詳細資料和產品，且只可更新的名稱和每個單位為這些產品的資訊數量*不*已停止。

在本教學課程中，我們第一個步驟，是建立此 DropDownList 並填入供應商中系統中。 開啟`UserLevelAccess.aspx`頁面`EditInsertDelete`資料夾中，加入 DropDownList 其`ID`屬性設定為`Suppliers`，並將此 DropDownList 繫結至名為新 ObjectDataSource `AllSuppliersDataSource`。


[![建立名為 AllSuppliersDataSource 新 ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**圖 3**： 建立新的 ObjectDataSource 具名`AllSuppliersDataSource`([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))


因為我們想要這個 DropDownList 以包含所有供應商，設定要叫用 ObjectDataSource`SuppliersBLL`類別的`GetSuppliers()`方法。 也請確認 ObjectDataSource s`Update()`方法對應到`SuppliersBLL`類別的`UpdateSupplierAddress`方法，為這個 ObjectDataSource 也會使用由 DetailsView 我們將加入在步驟 2 中。

完成 ObjectDataSource 精靈之後，完成設定步驟`Suppliers`DropDownList，它會顯示`CompanyName`資料欄位，並使用`SupplierID`資料欄位的值都為`ListItem`。


[![設定供應商 DropDownList 以使用 CompanyName 和 SupplierID 資料欄位](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**圖 4**： 設定`Suppliers`使用 DropDownList`CompanyName`和`SupplierID`資料欄位 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))


此時，DropDownList 會列出供應商在資料庫中的公司名稱。 不過，我們也必須包括 「 顯示或編輯所有供應商 」 選項，以 DropDownList。 若要達成此目的，設定`Suppliers`DropDownList s`AppendDataBoundItems`屬性`true`然後再加入`ListItem`其`Text`屬性是 「 顯示或編輯所有供應商 」，而其值為`-1`。 這可以加入透過宣告式標記或直接在設計工具移至 [屬性] 視窗，然後按一下省略符號，DropDownList 的`Items`屬性。

> [!NOTE]
> 回頭參考[*主要/詳細資料篩選與 DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)教學課程，如需將所有選取的項目加入至資料繫結 DropDownList 的更詳細討論。


之後`AppendDataBoundItems`屬性已設定和`ListItem`加入，DropDownList s 宣告式標記看起來應該像：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

圖 5 顯示目前進度的螢幕擷取畫面，透過瀏覽器檢視時。


[![供應商 DropDownList 包含顯示，所有的清單項目，再加上另一個用於每個供應商](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**圖 5**: `Suppliers` DropDownList 包含全部顯示`ListItem`，加上一個每個供應商 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))


由於我們想要使用者已變更選取範圍之後，立即更新的使用者介面，將設定`Suppliers`DropDownList s`AutoPostBack`屬性`true`。 在步驟 2 中，我們將建立會顯示的資訊為根據的 DropDownList 選擇 supplier(s) DetailsView 控制項。 然後，在步驟 3 中，我們將建立此 DropDownList s 的事件處理常式`SelectedIndexChanged`事件，在其中我們會加入程式碼，將適當的供應商資訊繫結至 DetailsView 根據選取的供應商。

## <a name="step-2-adding-a-detailsview-control"></a>步驟 2： 加入 DetailsView 控制項

可讓 s 使用 DetailsView 顯示供應商的資訊。 可以檢視及編輯所有供應商的使用者，DetailsView 會支援分頁，讓使用者逐步執行供應商資訊一筆記錄，一次。 如果使用者適用於特定供應商，不過，DetailsView 會顯示該特定供應商的資訊，將不會包含分頁介面。 在任一情況下，DetailsView 需要允許使用者編輯供應商 s 地址、 縣市和國家/地區欄位。

加入至頁面下方的 DetailsView `Suppliers` DropDownList，設定其`ID`屬性`SupplierDetails`，並將它繫結`AllSuppliersDataSource`ObjectDataSource 先前步驟中建立。 接下來，檢查從 DetailsView s 智慧標籤啟用分頁，並啟用編輯核取方塊。

> [!NOTE]
> 如果您不要看到智慧 s 的 DetailsView 中啟用編輯選項標記 s 因為未對應 ObjectDataSource s`Update()`方法`SuppliersBLL`類別的`UpdateSupplierAddress`方法。 請花一點時間回頭進行此設定變更之後, 啟用編輯 選項應該會出現在 DetailsView s 智慧標籤。


因為`SuppliersBLL`類別 s`UpdateSupplierAddress`方法只會接受四個參數- `supplierID`， `address`， `city`，和`country`-修改 DetailsView 的 BoundFields 以便`CompanyName`和`Phone`BoundFields 處於唯讀狀態。 此外，移除`SupplierID`BoundField 完全。 最後， `AllSuppliersDataSource` ObjectDataSource 目前具有其`OldValuesParameterFormatString`屬性設定為`original_{0}`。 請花一點時間從宣告式語法完全中，移除此屬性設定值，或將它設定為預設值， `{0}`。

設定之後`SupplierDetails`DetailsView 和`AllSuppliersDataSource`ObjectDataSource，我們將會在下列的宣告式標記：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

此時 DetailsView 可以透過分頁，並選取供應商的位址資訊可以更新，不論所做的選擇`Suppliers`DropDownList （請參閱圖 6）。


[![您可以檢視任何供應商的資訊，且其位址更新](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**圖 6**: Any 供應商可以檢視資訊，並更新其位址 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>步驟 3： 顯示只將選取的供應商的資訊

目前我們的頁面就會顯示的資訊，不論是否已從選取特定供應商的所有供應商`Suppliers`DropDownList。 若要顯示供應商資訊選取供應商我們要將另一個 ObjectDataSource 加入至我們的頁面上，擷取特定供應商的相關資訊的其中一個。

加入新 ObjectDataSource 頁面上，其命名為`SingleSupplierDataSource`。 從其智慧標籤上，按一下 設定資料來源連結，並讓它使用`SuppliersBLL`類別的`GetSupplierBySupplierID(supplierID)`方法。 如同`AllSuppliersDataSource`ObjectDataSource，具有`SingleSupplierDataSource`ObjectDataSource s`Update()`方法對應到`SuppliersBLL`類別的`UpdateSupplierAddress`方法。


[![設定為使用 GetSupplierBySupplierID(supplierID) 方法 SingleSupplierDataSource ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**圖 7**： 設定`SingleSupplierDataSource`使用 ObjectDataSource`GetSupplierBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))


接下來，我們重新系統提示您指定的參數來源`GetSupplierBySupplierID(supplierID)`方法的`supplierID`輸入的參數。 因為我們想要顯示的資訊從 DropDownList，使用選取的供應商`Suppliers`DropDownList 的`SelectedValue`屬性做為參數的來源。


[![供應商 DropDownList 做 supplierID 參數來源](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**圖 8**： 使用`Suppliers`為 DropDownList`supplierID`參數來源 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))


加入這個第二個 ObjectDataSource，即使有 DetailsView 控制項目前設定為一律使用`AllSuppliersDataSource`ObjectDataSource。 我們需要加入邏輯，以調整 DetailsView 取決於所使用的資料來源`Suppliers`選取 DropDownList 項目。 若要達成此目的，建立`SelectedIndexChanged`供應商 DropDownList 的事件處理常式。 這最容易建立按兩下設計工具中的 DropDownList。 這個事件處理常式必須決定要使用何種資料來源，且必須重新繫結在 detailsview 的資料。 使用下列程式碼完成這個動作：


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

此事件處理常式一開始會判斷是否已選取 「 顯示或編輯所有供應商 」 選項。 如果是，它會設定`SupplierDetails`DetailsView s`DataSourceID`至`AllSuppliersDataSource`並傳回給供應商的集合中第一筆記錄的使用者，藉由設定`PageIndex`屬性設為 0。 不過，使用者已選取 特定供應商的 DropDownList DetailsView s 從`DataSourceID`指派給`SingleSuppliersDataSource`。 無論何種資料來源使用，`SuppliersDetails`模式會還原到唯讀模式且資料會重新繫結至 DetailsView 呼叫`SuppliersDetails`控制項的`DataBind()`方法。

與就地此事件處理常式，DetailsView 控制項現在會顯示所選的供應商，除非已選取 「 顯示或編輯所有供應商 」 選項，在此情況下的所有供應商可以檢視透過分頁介面。 圖 9 顯示頁面 」 顯示或編輯所有供應商 」 選取的選項。請注意，分頁介面，讓使用者瀏覽並更新任何供應商。 圖 10 顯示頁面以選取錦供應商。 錦的資訊會在此情況下是可檢視和編輯。


[![所有供應商資訊可以檢視和編輯](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**圖 9**： 的所有供應商資訊檢視和編輯 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))


[![可以檢視和編輯選取的供應商的資訊](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**圖 10**： 只有選取的供應商 s 資訊可以 Viewed 和編輯 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))


> [!NOTE]
> 本教學課程，DropDownList 和 DetailsView 控制 s`EnableViewState`必須設為`true`（預設值） 因為 DropDownList s`SelectedIndex`和 DetailsView 的`DataSourceID`屬性的變更必須記住在回傳。


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>步驟 4： 列出供應商產品的可編輯的 GridView

完成 DetailsView 下一步是包含可編輯的 GridView 會列出所選取的供應商提供的產品。 此 GridView 應該允許編輯，只能`ProductName`和`QuantityPerUnit`欄位。 此外，如果使用者瀏覽頁面是來自特定供應商，它應該只允許這些產品的更新*不*已停止。 若要完成此我們需要先新增的多載`ProductsBLL`類別 s`UpdateProducts`中採用的方法只`ProductID`， `ProductName`，和`QuantityPerUnit`做為輸入的欄位。 我們已事先在許多教學課程中，會逐步執行此程序讓 s 一下這裡的程式碼，應該會加入到`ProductsBLL`:


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

使用這個多載建立，我們將 GridView 控制項和其相關聯的 ObjectDataSource 準備好。 將新的 GridView 加入頁面，設定其`ID`屬性`ProductsBySupplier`，並將它設定為使用名為新 ObjectDataSource `ProductsBySupplierDataSource`。 因為我們想要這個 GridView 列出所選取的供應商的那些產品，請使用`ProductsBLL`類別的`GetProductsBySupplierID(supplierID)`方法。 也會將對應`Update()`方法，以新`UpdateProduct`剛才所建立的多載。


[![設定要使用剛才建立的 UpdateProduct 多載 ObjectDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**圖 11**： 設定要使用 ObjectDataSource`UpdateProduct`多載剛建立 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))


我們 re 提示您選取的參數來源`GetProductsBySupplierID(supplierID)`方法的`supplierID`輸入的參數。 因為我們想要顯示的產品中 DetailsView，使用選取的供應商`SuppliersDetails`DetailsView 控制項的`SelectedValue`屬性做為參數的來源。


[![在 SuppliersDetails DetailsView 的 SelectedValue 屬性做為參數來源](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**圖 12**： 使用`SuppliersDetails`DetailsView s`SelectedValue`做為參數的來源屬性 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))


返回 GridView 中移除所有的 GridView 欄位除外`ProductName`， `QuantityPerUnit`，和`Discontinued`、 標記`Discontinued`CheckBoxField 以唯讀狀態。 另請檢查 GridView s 智慧標籤的 啟用編輯選項。 在進行這些變更之後，GridView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

如同我們先前 ObjectDataSources 這個一個 s`OldValuesParameterFormatString`屬性設定為`original_{0}`，嘗試更新產品的名稱或每個單位的數量時，這樣將會造成問題。 完全移除這個屬性的宣告式語法，或將它設定為其預設`{0}`。

此設定完成後，我們的頁面現在會列出供應商在 GridView 中選取所提供的產品 （請參閱圖 13）。 目前*任何*更新產品的名稱或每個單位的數量。 不過，我們需要更新網頁邏輯，使這類功能禁止使用的特定供應商相關聯的使用者不再生產的產品。 我們將會處理這個步驟 5 中的最後一段。


[![會顯示所選取的供應商提供的產品](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**圖 13**： 會顯示所選取的供應商的產品提供 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))


> [!NOTE]
> 這可以讓您編輯 GridView 加入`Suppliers`DropDownList 的`SelectedIndexChanged`應該更新事件處理常式，以傳回 GridView 唯讀狀態。 否則，如果仍執行時編輯產品資訊中選取不同的供應商，則新的供應商的 GridView 中對應的索引也會編輯。 若要避免這個問題，只要設定 GridView s`EditIndex`屬性`-1`中`SelectedIndexChanged`事件處理常式。


此外，請記住，它是很重要的 GridView 的檢視狀態是啟用 （預設行為）。 如果您將 GridView s`EnableViewState`屬性`false`，執行並行的使用者不小心刪除或編輯記錄的風險。 請參閱[警告： 已停用並行問題與 ASP.NET 2.0 GridViews/DetailsView/FormViews 該支援編輯和/或刪除與的檢視狀態](http://scottonwriting.net/sowblog/posts/10054.aspx)如需詳細資訊。

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>步驟 5： 不允許編輯已停止的產品時顯示或編輯所有供應商是未選取

雖然`ProductsBySupplier`GridView 可完整運作，它目前太多存取權授與這些使用者從特定供應商。 根據我們的商務規則，這類使用者應無法更新不再生產的產品。 若要強制執行這種情況，我們可以隱藏 （或停用） 在不再生產的產品時所瀏覽頁面時由使用者從供應商的 GridView 資料列中的 [編輯] 按鈕。

建立事件處理常式 GridView s`RowDataBound`事件。 此事件處理常式中，我們需要判斷使用者是否為特定的供應商，而這在此教學課程中，可以決定藉由檢查供應商 DropDownList s 相關聯`SelectedValue`屬性--如果它是 s 以外的項目-1，則使用者與特定供應商相關聯。 這類使用者，我們再需要判斷產品已停止。 我們可以抓取參考實際`ProductRow`執行個體繫結至 GridView 資料列，以透過`e.Row.DataItem`屬性，如下所述[ *GridView 的頁尾中顯示摘要資訊*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md)教學課程。 如果產品停用，我們可以抓取 GridView s CommandField 使用先前的教學課程中所討論的技術中的 [編輯] 按鈕的程式設計參考[*新增用戶端確認時刪除*](adding-client-side-confirmation-when-deleting-vb.md). 一旦我們可以隱藏或停用按鈕的參考。


[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

與此事件處理常式的位置時瀏覽此頁面為使用者特定供應商的已停用這些產品是不能編輯，, [編輯] 按鈕以隱藏這些產品。 例如，Chef Anton 的 Gumbo 混合是新增奧良印地安歡樂供應商停售的產品。 當瀏覽這個特定供應商的頁面，看不到隱藏這項產品的 [編輯] 按鈕 （請參閱圖 14）。 不過，瀏覽使用 「 顯示或編輯所有供應商 」，[編輯] 按鈕時使用 （請參閱圖 15）。


[![Chef Anton 的 Gumbo 混合的 [編輯] 按鈕是隱藏的供應商特有的使用者](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**圖 14**： 針對供應商特有的使用者隱藏 Chef Anton 的 Gumbo 混合的 [編輯] 按鈕 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))


[![顯示或編輯的所有供應商使用者，會顯示 [編輯] 按鈕，Chef Anton s Gumbo 混合](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**圖 15**： 為顯示或編輯所有供應商的使用者、 Chef Anton s Gumbo 混合會顯示 [編輯] 按鈕 ([按一下以檢視完整大小的影像](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>檢查商務邏輯層中的存取權限

在本教學課程將 ASP.NET 頁面會處理使用者可以看到哪些資訊與所有邏輯而且他可以更新哪些產品。 在理想情況下，這個邏輯也會出現在商務邏輯層。 例如，`SuppliersBLL`類別 s`GetSuppliers()`方法 （可傳回所有供應商） 可能會包含檢查，以確定目前登入的使用者是*不*特定供應商相關聯。 同樣地，`UpdateSupplierAddress`方法可能包括檢查，以確認目前登入的使用者是本公司的工作 （及可以更新所有供應商的位址資訊） 或其資料正在更新供應商相關聯。

因為在我們的教學課程中，使用者 s 權限由在頁面上，無法存取 BLL 類別 DropDownList 我不包含這類 BLL 層檢查。 使用成員資格系統，或其中一個 （例如 Windows 驗證），ASP.NET 所提供的方塊外的驗證配置時目前登入的使用者的資訊及角色資訊可從存取 BLL，因此這類存取權限檢查可能在簡報和 BLL 圖層。

## <a name="summary"></a>總結

提供使用者帳戶的大部分站台需要自訂登入的使用者為基礎的資料修改介面。 系統管理使用者可以刪除和編輯任何筆記錄，而非系統管理使用者可能會限制為僅更新或刪除自己建立的記錄。 任何案例可能是，Web 控制項，ObjectDataSource，資料和商務邏輯層類別可以擴充來新增或拒絕登入的使用者為基礎的特定功能。 在本教學課程中，我們看到如何限制根據使用者是否為特定供應商相關聯，或如果他們的工作我們公司的檢視和編輯資料。

本教學課程結束時，我們檢驗插入、 更新和刪除使用 GridView、 DetailsView 和 FormView 控制項的資料。 從開始下一個教學課程，我們將會開啟我們注意到新增分頁和排序的支援。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一步](adding-client-side-confirmation-when-deleting-vb.md)
