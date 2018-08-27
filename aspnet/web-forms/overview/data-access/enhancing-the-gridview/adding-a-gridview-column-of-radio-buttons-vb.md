---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
title: 新增 GridView 資料行的選項按鈕 (VB) |Microsoft Docs
author: rick-anderson
description: 本教學課程會探討如何將為使用者提供更直覺的方式，選取單一資料列的 GridView 控制項中的選項按鈕的資料行...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 2e31b60b-8723-4f14-b7ee-37859454dc3b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-vb
msc.type: authoredcontent
ms.openlocfilehash: 76b3dbd502eff7c97f57fdacd120ac2312aaceae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830205"
---
<a name="adding-a-gridview-column-of-radio-buttons-vb"></a>新增 GridView 資料行的選項按鈕 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_VB.exe)或[下載 PDF](adding-a-gridview-column-of-radio-buttons-vb/_static/datatutorial51vb1.pdf)

> 本教學課程會探討如何將為使用者提供更直覺的方式，選取單一資料列的 GridView 的 GridView 控制項中的選項按鈕的資料行。


## <a name="introduction"></a>簡介

GridView 控制項提供許多內建的功能。 它包含數個不同的欄位，來顯示文字、 影像、 超連結和按鈕。 它支援進一步的自訂範本。 使用按幾下滑鼠，它可以讓 GridView，其中可透過按鈕，選取每個資料列，或啟用編輯或刪除功能。 提供的功能眾多，儘管通常會有一些情況下，其他，不受支援的功能將會需要加入。 在本教學課程，後面兩個我們將檢驗如何增強 GridView 的功能，以包括其他功能。

本教學課程中，另一個，著重於強化的資料列選取程序。 檢查在做[主要/詳細說明使用具有詳細資料 detailview 之可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)，我們可以新增 CommandField 至 GridView，其中包含 [選取] 按鈕。 回傳是兩邊彼此乾瞪眼按一下時，與 GridView 的`SelectedIndex`屬性更新為其選取按鍵被按一下之資料列的索引。 在 [主要/詳細說明使用具有詳細資料 detailview 之可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)教學課程中，我們了解如何使用這項功能，以顯示所選的 GridView 資料列的詳細資料。

而 [選取] 按鈕則適用於在許多情況下，它可能無法供其他人一樣正常運作。 而不是使用按鈕，兩個其他使用者介面項目通常用於選項： 選項按鈕，然後核取方塊。 我們可以加強 GridView，因此，而不是選取的按鈕，每個資料列都包含選項按鈕或核取方塊。 在其中使用者只能選取其中一個 GridView 記錄的情況下，選項按鈕可能偏好透過 [選取] 按鈕。 在使用者可以可能選取的情況下這類 web 型電子郵件應用程式，使用者可能要選取要刪除此核取方塊的多個訊息中的多個記錄提供不是可從 [選取] 按鈕或選項按鈕的功能使用者介面。

本教學課程會探討如何將選項按鈕的資料行加入至 GridView。 繼續進行教學課程會探討使用核取方塊。

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>步驟 1： 建立增強 GridView 網頁

增強 GridView，以包含資料行的選項按鈕，開始之前，可讓 s 先花點時間，我們需要針對本教學課程和下一步 的兩個我們網站專案中建立 ASP.NET 網頁。 藉由新增新的資料夾，名為啟動`EnhancedGridView`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程


在其他資料夾，例如`Default.aspx`在`EnhancedGridView`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image2.png))


最後，將這些四個頁面新增項目為`Web.sitemap`檔案。 具體來說，在使用之後新增下列標記 SqlDataSource 控制項`<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入及刪除教學課程的項目。


![網站導覽現在包含項目，來增強 GridView 教學課程](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.gif)

**圖 3**： 網站地圖現在包含項目，來增強 GridView 教學課程


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>步驟 2: GridView 中顯示供應商

本教學課程可讓 s 建置的 GridView 會列出美國，從供應商提供的選項按鈕的每個 GridView 資料列。 選取的選項按鈕透過供應商之後, 使用者可以按一下按鈕來檢視供應商產品。 雖然這項工作乍聽之下 trivial，有一些微妙之處，以便尤為棘手。 我們會探討這些微妙之處之前，讓第一次有個 gridview 列出供應商的 s。

首先開啟`RadioButtonField.aspx`頁面中`EnhancedGridView`拖曳的 GridView，從 [工具箱] 拖曳至設計工具的資料夾。 設定 GridView s`ID`至`Suppliers`和從它的智慧標籤，選擇 建立新的資料來源。 具體來說，建立名為 ObjectDataSource `SuppliersDataSource` ，其會從提取資料`SuppliersBLL`物件。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image3.png)

**圖 4**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image4.png))


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image5.png)

**圖 5**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.png))


因為我們只想要列出這些供應商在美國，選擇`GetSuppliersByCountry(country)`方法，從下拉式清單中選取的索引標籤。


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.png)

**圖 6**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.png))


從 更新 索引標籤，選取 （無） 選項，然後按一下 下一步。


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.png)

**圖 7**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.png))


因為`GetSuppliersByCountry(country)`方法接受參數、 設定資料來源精靈會提示我們輸入該參數的來源。 若要指定硬式編碼的值 (USA，在此範例中)，會保留來源下拉式清單設定為 None，在文字方塊中輸入的預設值的參數。 按一下 完成 以完成精靈。


[![使用做為預設值的美國國家/地區參數](adding-a-gridview-column-of-radio-buttons-vb/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.png)

**圖 8**： 做為預設值為使用美國`country`參數 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.png))


完成精靈之後，GridView 會針對每個供應商資料欄位包含 BoundField。 移除以外的所有`CompanyName`， `City`，並`Country`BoundFields，並重新命名`CompanyName`BoundFields`HeaderText`供應商的屬性。 完成之後，請的 GridView 和 ObjectDataSource 的宣告式語法看起來應該如下所示。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample2.aspx)]

本教學課程中，可讓允許使用者檢視選取的供應商產品與供應商清單中，在相同頁面上，或在不同的頁面上的 s。 若要做到這一點，加入頁面中的兩個按鈕 Web 控制項。 我 ve 組`ID`至這兩個按鈕的`ListProducts`並`SendToProducts`，其概念，當`ListProducts`按下會發生回傳，而且選取的供應商的產品將列在相同頁面上，但是在`SendToProducts`按一下時，使用者將會列出產品的另一個頁面檢視。

[圖 9] 顯示`Suppliers`GridView 和兩個按鈕 Web 控制項透過瀏覽器檢視時。


[![從美國這些供應商擁有其名稱、 城市和國家/地區資訊列](adding-a-gridview-column-of-radio-buttons-vb/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.png)

**圖 9**： 這些來自供應商美國有其名稱、 城市和國家/地區資訊列 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>步驟 3： 加入資料行的選項按鈕

此時`Suppliers`GridView 有三個 BoundFields 顯示公司名稱、 城市和國家/地區的每個供應商在美國。 它不過仍缺乏選項按鈕的資料行。 不幸的是，GridView 不包含內建 RadioButtonField，否則為我們可以只將它加入至方格，並完成。 相反地，我們可以在 新增為 TemplateField，並設定其`ItemTemplate`呈現選項按鈕，導致每個 GridView 資料列的選項按鈕。

一開始，我們可能會假設所需的使用者介面，可以實作加入至 RadioButton Web 控制項`ItemTemplate`TemplateField。 雖然這確實會增加 GridView 的每個資料列的單一選項按鈕，選項按鈕無法進行分組，並因此不會互斥。 亦即，使用者就能夠從 GridView 中同時選取多個選項按鈕。

即使使用 TemplateField RadioButton Web 控制項不會提供我們需要的功能，可讓 s 實作這種方法，因為它 s 值得檢查產生的選項按鈕未分組的原因。 開始為 TemplateField 新增至供應商 GridView，使得最左邊的欄位。 接下來，從 GridView s 智慧標籤，按一下 編輯範本 連結，並將 RadioButton Web 控制項從 工具箱 拖曳至 TemplateField 的`ItemTemplate`（請參閱 圖 10）。 設定 RadioButton s`ID`屬性，以`RowSelector`並`GroupName`屬性設`SuppliersGroup`。


[![將 RadioButton Web 控制項加入 ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.png)

**圖 10**： 加入 RadioButton Web 控制項來`ItemTemplate`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.png))


之後透過設計工具的這些新增項目，您的 GridView 的標記看起來應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample3.aspx)]

S 的 RadioButton [ `GroupName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx)是用來將一系列的選項按鈕。 所有具有相同的 RadioButton 控制項`GroupName`值會被視為分組; 可以從群組選取一個選項按鈕，一次。 `GroupName`屬性會指定轉譯的選項按鈕 s`name`屬性。 瀏覽器檢查選項按鈕`name`屬性，用以判斷選項按鈕群組。

使用 RadioButton Web 控制項新增至`ItemTemplate`，瀏覽此頁面，透過瀏覽器，然後按一下方格 s 資料列中的選項按鈕。 請注意，選項按鈕未分組的方式，讓您可以選取所有資料列，以 圖 11 顯示。


[![GridView 的選項按鈕是 未分組](adding-a-gridview-column-of-radio-buttons-vb/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.png)

**圖 11**: W 的選項按鈕是 未分組 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.png))


選項按鈕未分組的原因是因為其呈現`name`屬性會不同，而不論是否具有相同`GroupName`屬性設定。 若要查看這些差異，請從瀏覽器進行檢視/原始檔並檢查選項 按鈕的標記：


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample4.html)]

請注意這兩種`name`並`id`屬性不是在 [屬性] 視窗中所指定的確切值，但是會加上一些其他`ID`值。 額外`ID`值加入至前端的轉譯`id`並`name`屬性是`ID`的選項按鈕的父控制項`GridViewRow`s `ID` s、 GridView 的`ID`，內容控制項 s `ID`，和 Web Form 的`ID`。 這些`ID`s 會新增，以便分別轉譯 gridview Web 控制項都有唯一`id`和`name`值。

每個呈現控制項的需求不同`name`和`id`因為這是如何在瀏覽器可唯一識別每個用戶端和其識別方式到 web 伺服器採取何種動作上的控制項，或在回傳發生變更。 例如，假設我們想要執行一些伺服器端程式碼，每當 RadioButton s 檢查狀態已變更。 我們無法完成這項作業所設定的 RadioButton 秒`AutoPostBack`屬性，以`True`並建立事件處理常式`CheckChanged`事件。 不過，如果轉譯`name`和`id`所有選項按鈕都相同，在回傳無法判定哪些特定的都值選項按鈕已按下。

簡短的它的是，我們就無法使用 RadioButton Web 控制項 GridView 中建立的資料行的選項按鈕。 相反地，我們必須使用而不是製作的技術，以確保適當的標記會插入每個 GridView 資料列。

> [!NOTE]
> 例如 RadioButton Web 控制項，選項按鈕 HTML 控制項，當加入至範本，將包含唯一`name`屬性，讓未分組的方格中的選項按鈕。 如果您不熟悉 HTML 控制項，放心忽略這個附註，很少使用 HTML 控制項，尤其是在 ASP.NET 2.0。 但如果您有興趣進一步了解更多資訊，請參閱[K.Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s 部落格文章[Web 控制項和 HTML 控制項](http://www.odetocode.com/Articles/348.aspx)。


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>使用常值控制項以插入選項按鈕標記

若要正確群組所有 GridView 中選項按鈕，我們要以手動方式插入選項按鈕標記`ItemTemplate`。 每個選項按鈕必須相同`name`屬性，但應該有唯一`id`屬性 （如果我們想要存取透過用戶端指令碼的選項按鈕）。 使用者選取的選項按鈕，並張貼頁面之後，瀏覽器會送回所選的選項按鈕的值`value`屬性。 因此，每個選項按鈕必須唯一`value`屬性。 最後，在回傳我們需要確定新增`checked`屬性來選取，否則使用者進行選取和文章上一步之後的一個選項按鈕，選項按鈕將回復為其預設狀態 （所有未選取）。

有兩種方法可以用才能插入範本中的低層級的標記。 其中一個是不要混合使用標記和格式化程式碼後置類別中定義的方法呼叫。 在第一次討論這項技術[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md)教學課程。 在本例中它可能會看起來像：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample5.aspx)]

在這裡，`GetUniqueRadioButton`並`GetRadioButtonValue`會傳回適當的程式碼後置類別中定義的方法`id`和`value`屬性值的每個選項按鈕。 這種方法適用於指派`id`並`value`屬性，但在需要指定`checked`屬性值，因為當資料第一次繫結至 GridView，才會執行資料繫結語法。 因此，如果 GridView 檢視狀態已啟用，格式化的方法才會觸發時第一次載入頁面 （或當 GridView 會明確地重新繫結至資料來源），並因此函式，以設定`checked`贏得 t 屬性上呼叫回傳。 它 s 而微妙的問題和超出指令碼的本文中，我就把它在這個範圍。 不過，我不要鼓勵您嘗試使用上述方法並透過努力，您將會停滯的點。 雖然這類練習贏得 t 取得您工作版本，就能協助促進更深入的了解 GridView 和資料繫結的生命週期。

另一個方法來插入自訂，低層級的標記中的範本和我們將在本教學課程中使用的方法是將[常值控制項](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx)範本。 然後，在 GridView s`RowCreated`或`RowDataBound`事件處理常式，以程式設計方式存取常值控制項並將其`Text`屬性設定為標記，以發出。

藉由移除 TemplateField s 中的選項按鈕來啟動`ItemTemplate`，取代常值的控制項。 將常值控制項 s`ID`至`RadioButtonMarkup`。


[![將常值的控制項新增至 ItemTemplate](adding-a-gridview-column-of-radio-buttons-vb/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.png)

**圖 12**： 將常值的控制項，加入`ItemTemplate`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.png))


接下來，建立事件處理常式 GridView s`RowCreated`事件。 `RowCreated`事件引發之後加入每個資料列，zda bude 資料要重新繫結至 GridView。 這表示即使在回傳資料會從檢視狀態，重新載入時`RowCreated`還是會引發事件，這是的因為我們會使用它，而不要`RowDataBound`（這會引發只有當資料明確繫結至資料 Web 控制項）。

在這個事件處理常式中，我們只想要繼續進行，如果我們重新處理資料列。 我們想要以程式設計方式參考每個資料列`RadioButtonMarkup`常值控制項並設定其`Text`發出標記的屬性。 送出的標記如下列程式碼所示，建立選項按鈕時`name`屬性設定為`SuppliersGroup`，其`id`屬性設為`RowSelectorX`，其中*X*是 GridView 資料列的索引而`value`屬性設為 GridView 資料列索引。


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample6.vb)]

GridView 資料列已選取，並回傳時，我們有興趣`SupplierID`選取供應商。 因此，有人可能會認為每個選項按鈕的值應該是實際`SupplierID`（而不是 GridView 資料列的索引）。 雖然這可能會在某些情況下運作，它就會有安全性風險盲目地接受和處理`SupplierID`。 我們的 GridView，比方說，會列出美國供應商。 不過，如果`SupplierID`直接從選項按鈕，以停止操作的惡作劇使用者哪些 s 傳遞`SupplierID`傳送回在回傳值？ 使用資料列索引`value`，然後取得並`SupplierID`從回傳`DataKeys`集合中，我們可以確保使用者只會使用其中一個`SupplierID`GridView 資料列的其中一個相關聯的值。

加入這個事件處理常式程式碼之後, 請花幾分鐘瀏覽器中測試。 首先，請注意，只有一個選項可以選取在方格中的按鈕，一次。 不過，選取選項按鈕，然後按一下其中一個按鈕，就會發生回傳，並還原為其初始狀態的所有選項按鈕 （也就是在回傳時，所選的選項按鈕已無法再選取）。 若要修正此問題，我們需要擴大`RowCreated`事件處理常式，因此它會檢查寄件者回傳的已選取的選項按鈕索引，並將`checked="checked"`屬性的資料列的索引比對的發出標記。

回傳發生時，瀏覽器傳送回`name`和`value`的選取的選項按鈕。 擷取的值，可以透過程式設計方式使用`Request.Form("name")`。 [ `Request.Form`屬性](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx)提供[ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx)代表表單變數。 表單變數的名稱和值的表單欄位，在網頁上，而且會傳送回網頁瀏覽器時回傳是兩邊彼此乾瞪眼。 因為轉譯`name`屬性的選項按鈕，在 gridview 裡`SuppliersGroup`，當 web 網頁回傳的後瀏覽器會將`SuppliersGroup=valueOfSelectedRadioButton`至 web 伺服器 （以及其他表單欄位）。 這項資訊可供存取從`Request.Form`屬性使用： `Request.Form("SuppliersGroup")`。

因為我們將需要判斷選取的選項按鈕索引不只能在`RowCreated`事件處理常式，而是位於`Click`Button Web 控制項的事件處理常式，可讓 s 新增`SuppliersSelectedIndex`屬性至程式碼後置類別，會傳回`-1`如果已選取 [無] 選項按鈕和選取的索引，如果已選取其中一個選項按鈕。


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample7.vb)]

新增此屬性，我們知道要新增`checked="checked"`中的標記`RowCreated`事件處理常式時`SuppliersSelectedIndex`等於`e.Row.RowIndex`。 更新以包含此邏輯的事件處理常式：


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample8.vb)]

這項變更，與選取的選項按鈕保持選取狀態回傳之後。 既然我們已經指定哪些選項按鈕已選取的功能，我們就無法變更行為，如此當第一次瀏覽的頁面，選取第一個的 GridView 資料列的選項按鈕 （而非不有任何預設選取的選項按鈕，這是目前行為）。 若要預設選取第一個選項按鈕，只需要變更`If SuppliersSelectedIndex = e.Row.RowIndex Then`陳述式如下： `If SuppliersSelectedIndex = e.Row.RowIndex OrElse (Not Page.IsPostBack AndAlso e.Row.RowIndex = 0) Then`。

現在我們已新增群組的選項按鈕的資料行至 GridView，可讓單一的 GridView 資料列，選取並記住在回傳之間。 我們接下來的步驟會顯示所選取的供應商提供的產品。 步驟 4 中我們會了解如何將使用者重新導向至另一個頁面，以及所選傳送`SupplierID`。 在步驟 5 中，我們會看到如何在相同頁面上的 GridView 中顯示所選的供應商產品。

> [!NOTE]
> 而不是使用 TemplateField （這個冗長的步驟 3 的焦點），我們可以建立自訂`DataControlField`呈現適當的使用者介面和功能的類別。 [ `DataControlField`類別](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx)BoundField、 CheckBoxField、 TemplateField 和其他內建的 GridView 和 DetailsView 欄位衍生自的基底類別。 建立自訂`DataControlField`類別表示的選項按鈕欄無法只使用宣告式語法中，新增，和也會讓複寫其他網頁和其他大幅簡化的 web 應用程式的功能。


如果您 ve 曾經建立過自訂，編譯的 ASP.NET 中的控制項，不過，您知道，這樣做需要相當多的跑腿並帶來許多微妙之處和邊緣案例，必須謹慎處理。 因此，我們將會放棄實作做為自訂的選項按鈕的資料行`DataControlField`類別現在並貫徹 TemplateField 選項。 或許我們將有機會探索建立、 使用和部署自訂的`DataControlField`未來的教學課程中的類別 ！

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>步驟 4： 在個別的頁面中顯示所選的供應商產品

使用者選取的 GridView 資料列之後，我們需要顯示選取的供應商產品。 在某些情況下，我們可能會想要在個別的頁面中，我們可能會想要在相同的頁面中執行的其他項目中顯示這些產品。 可讓第一次檢查如何在個別的頁面，顯示產品的 s步驟 5 中我們將探討新增到 GridView`RadioButtonField.aspx`顯示選取的供應商 s 的產品。

目前有兩個頁面上的按鈕 Web 控制項`ListProducts`和`SendToProducts`。 當`SendToProducts`按一下按鈕時，我們想要傳送至使用者`~/Filtering/ProductsForSupplierDetails.aspx`。 此頁面中建立[主版/詳細篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-vb.md)教學課程，並顯示產品的供應商其`SupplierID`就會傳遞名為的 querystring 欄位`SupplierID`。

若要提供這項功能，建立事件處理常式`SendToProducts` 按鈕的`Click`事件。 我們在步驟 3 中新增`SuppliersSelectedIndex`選取屬性，傳回的資料列索引的選項按鈕。 對應`SupplierID`可以擷取從 GridView s`DataKeys`收集和使用者可以再傳送至`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`使用`Response.Redirect("url")`。


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample9.vb)]

此程式碼適用於人員，只要 GridView 從選取的其中一個選項按鈕。 如果一開始，GridView 並沒有選取任何選項按鈕，而且使用者按下`SendToProducts` 按鈕，`SuppliersSelectedIndex`會`-1`，這樣將會造成例外狀況會擲回自`-1`超出索引`DataKeys`集合。 這是不需要顧慮，不過，如果您決定要更新`RowCreated`以便在一開始選取 GridView 中的第一個選項按鈕的步驟 3 中所述的事件處理常式。

若要容納`SuppliersSelectedIndex`的值`-1`，將標籤 Web 控制項新增至 GridView 上方頁面。 設定其`ID`屬性，以`ChooseSupplierMsg`、 其`CssClass`屬性設`Warning`、 其`EnableViewState`並`Visible`屬性，以`False`，並將其`Text`屬性，請選擇供應商的方格中。 CSS 類別`Warning`紅色、 斜體、 粗體、 大字型顯示文字，並定義於`Styles.css`。 藉由設定`EnableViewState`並`Visible`屬性，以`False`，針對只回傳的位置不在標籤呈現除了控制 s`Visible`屬性以程式設計方式設定為`True`。


[![新增 GridView 上方的標籤 Web 控制項](adding-a-gridview-column-of-radio-buttons-vb/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image21.png)

**圖 13**： 新增標籤 Web 控制項上方的 GridView ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image22.png))


接下來，加強`Click`事件處理常式，以顯示`ChooseSupplierMsg`標籤，如果`SuppliersSelectedIndex`小於零，且若要將使用者重新導向`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`否則。


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample10.vb)]

瀏覽的頁面在瀏覽器，並按一下`SendToProducts`按鈕，然後再從 GridView 中選取 供應商。 如 [圖 14] 所示，這會顯示`ChooseSupplierMsg`標籤。 接下來，選取 供應商，然後按一下  `SendToProducts`  按鈕。 這將 whisk 您列出所選取的供應商提供的產品頁面。 [圖 15] 顯示`ProductsForSupplierDetails.aspx`頁面選取 Bigfoot Breweries 供應商。


[![如果已選取 [否] 供應商，就會顯示 ChooseSupplierMsg 標籤](adding-a-gridview-column-of-radio-buttons-vb/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image23.png)

**圖 14**:`ChooseSupplierMsg`標籤會顯示，如果已選取 [否] 供應商 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image24.png))


[![選取的供應商產品會顯示在 ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-vb/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image25.png)

**圖 15**: 選取的供應商的產品都會顯示在`ProductsForSupplierDetails.aspx`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>步驟 5： 在相同頁面上顯示選取的供應商產品

步驟 4 中，我們了解如何將使用者傳送至另一個網頁來顯示選取的供應商產品的內容。 或者，選取的供應商產品可以在相同頁面上顯示。 為了說明這一點，我們會將另一個 GridView，以新增`RadioButtonField.aspx`顯示選取的供應商 s 的產品。

因為我們只想要的產品此 GridView，以顯示選取的供應商之後，新增面板 Web 控制項下方`Suppliers`GridView，設定其`ID`要`ProductsBySupplierPanel`及其`Visible`屬性設`False`。 在面板中，新增文字產品選取供應商，後面接著 GridView，名為`ProductsBySupplier`。 從 GridView s 智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsBySupplierDataSource`。


[![繫結至新的 ObjectDataSource 的 ProductsBySupplier GridView](adding-a-gridview-column-of-radio-buttons-vb/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image27.png)

**圖 16**： 繫結`ProductsBySupplier`GridView，以新的 ObjectDataSource ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image28.png))


接下來，設定要使用 ObjectDataSource`ProductsBLL`類別。 因為我們只想要擷取這些選取的供應商所提供的產品，指定應叫用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法來擷取其資料。 （無） 從清單中選取下拉式清單中更新、 插入和刪除索引標籤。


[![設定為使用 GetProductsBySupplierID(supplierID) 方法的 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-vb/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image29.png)

**圖 17**： 設定要使用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image30.png))


[![設定為 (None) 的下拉式清單中更新、 插入和刪除索引標籤](adding-a-gridview-column-of-radio-buttons-vb/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image31.png)

**圖 18**： 設定為 （無） 下拉式清單中更新、 插入和刪除索引標籤 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image32.png))


設定 選取之後, 更新、 插入，並刪除索引標籤、 按一下 下一步。 因為`GetProductsBySupplierID(supplierID)`方法預期的輸入的參數，建立資料來源精靈會提示我們指定的來源參數 s 的值。

我們有幾個這裡在指定的參數 s 的值來源的選項。 我們可以使用預設的參數物件，並以程式設計方式將指定的值`SuppliersSelectedIndex`屬性，以參數 s`DefaultValue`屬性中之 ObjectDataSource`Selecting`事件處理常式。 回頭[以程式設計方式設定 ObjectDataSource 的參數值](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)複習一下以程式設計方式將值指派給 ObjectDataSource 的參數的教學課程。

或者，我們可以使用 ControlParameter 和是指`Suppliers`GridView s [ `SelectedValue`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx)（請參閱圖 19）。 GridView s`SelectedValue`屬性會傳回`DataKey`值，對應[`SelectedIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)。 為了讓這個選項才會運作，我們需要以程式設計方式設定 GridView s`SelectedIndex`屬性，以所選資料列時`ListProducts`按一下按鈕時。 作為額外的權益，藉由設定`SelectedIndex`，選取的記錄將會擔任`SelectedRowStyle`中所定義`DataWebControls`佈景主題 （黃色背景）。


[![若要指定為參數來源的 GridView 的 SelectedValue 使用 ControlParameter](adding-a-gridview-column-of-radio-buttons-vb/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image33.png)

**圖 19**： 用以 ControlParameter GridView 的 SelectedValue 指定做為參數的來源 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image34.png))


在完成精靈，Visual Studio 會自動加入產品的資料欄位的欄位。 移除以外的所有`ProductName`， `CategoryName`，並`UnitPrice`BoundFields，並將變更`HeaderText`產品、 類別和價格的屬性。 設定`UnitPrice`BoundField，讓其值會格式化為貨幣。 進行這些變更之後，面板、 GridView，與 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample11.aspx)]

若要完成此練習中，我們需要設定 GridView s`SelectedIndex`屬性，以`SelectedSuppliersIndex`並`ProductsBySupplierPanel`面板 s`Visible`屬性設`True`時`ListProducts`按一下按鈕時。 若要達成此目的，建立 事件處理常式`ListProducts`Button Web 控制項的`Click`事件，並新增下列程式碼：


[!code-vb[Main](adding-a-gridview-column-of-radio-buttons-vb/samples/sample12.vb)]

如果尚未從 gridview 裡，選取供應商`ChooseSupplierMsg`標籤會顯示與`ProductsBySupplierPanel`隱藏面板。 否則，如果已選取的供應商，`ProductsBySupplierPanel`會顯示與 GridView 的`SelectedIndex`屬性更新。

[圖 20] 顯示結果之後選取 Bigfoot Breweries 供應商，並且已按下頁按鈕顯示產品。


[![Bigfoot Breweries 產品提供相同的頁面上列出](adding-a-gridview-column-of-radio-buttons-vb/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-vb/_static/image35.png)

**圖 20**： 在相同頁面上列出產品提供 Bigfoot Breweries ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-vb/_static/image36.png))


## <a name="summary"></a>總結

中所述[主要/詳細說明使用具有詳細資料 detailview 之可選取主要 GridView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)教學課程中，可以從使用 CommandField GridView 中選取記錄其`ShowSelectButton`屬性設定為`True`。 但 CommandField 做為一般的推播按鈕、 連結或影像會顯示其按鈕。 替代的資料列選取範圍的使用者介面是要提供的選項按鈕或核取方塊，在每個 GridView 資料列。 在本教學課程中，我們檢查如何加入選項按鈕的資料行。

不幸的，將選項按鈕不是 t 的資料行新增為簡單或簡單，如預期一般。 可以在按一下按鈕，新增任何內建 RadioButtonField 而且使用 RadioButton Web 控制項內為 TemplateField 導入了自己的問題集。 最後，若要提供這樣的介面我們已經建立自訂`DataControlField`類別或適當的 HTML 插入 TemplateField 期間所採取的手段`RowCreated`事件。

具有探索如何加入資料行的選項按鈕，讓我們把焦點轉到新增的資料行的核取方塊。 具有資料行的核取方塊，使用者可以選取一或多個 GridView 資料列，然後再執行 對所有選取的資料列 （例如，選取一組電子郵件從 web 型電子郵件用戶端，然後選擇刪除所有選取的電子郵件） 的某些作業。 在下一個教學課程中，我們會看到如何將這類資料行。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者為 David Suru。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
> [下一頁](adding-a-gridview-column-of-checkboxes-vb.md)
