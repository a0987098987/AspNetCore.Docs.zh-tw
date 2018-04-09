---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: 加入 GridView 的資料行的選項按鈕 (C#) |Microsoft 文件
author: rick-anderson
description: 本教學課程會如何為使用者提供更直覺的方式，選取單一資料列的 GridView 控制項中加入資料行的選項按鈕，查看...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 32bfe8463d80dfcff925f3bd6f31de67b071fc0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>加入 GridView 的資料行的選項按鈕 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe)或[下載 PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> 本教學課程會查看如何為使用者提供更直覺的方式，選取單一資料列的 GridView 的 GridView 控制項中加入資料行的選項按鈕。


## <a name="introduction"></a>簡介

GridView 控制項提供了許多內建功能。 包含一些不同欄位的顯示文字、 影像、 超連結和按鈕。 它支援進一步的自訂範本。 按幾下滑鼠，與它 s 可能進行的 GridView 透過按鈕，其中可以選取每個資料列，或啟用編輯或刪除功能。 提供的功能方面，儘管通常會有一些情況下，額外，非支援的功能會需要加入。 在本教學課程和下一步的兩個，我們將檢驗如何增強為包括其他功能的 GridView 的功能。

本教學課程和下一個專注於強化的資料列選取程序。 當檢查[主要/詳細說明可選取的主要 GridView 使用詳細資料 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)，我們可以將 CommandField 加入至包含選取按鈕的 GridView。 按一下時，回傳展示和 GridView 的`SelectedIndex`屬性會更新為其選取的 button 已按下資料列索引。 在[主要/詳細說明可選取的主要 GridView 使用詳細資料 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程，我們可了解如何使用這項功能，以顯示所選的 GridView 資料列的詳細資料。

當選取的按鈕在許多情況下，它可能不供其他人也適用。 而不是使用按鈕，兩個其他使用者介面項目通常用於選取項目: 選項按鈕和核取方塊。 我們可以使每個資料列包含的選項按鈕或核取方塊而不是選取的按鈕，來加強 GridView。 在其中，使用者可以只選取一個 GridView 記錄的情況下，選項按鈕可能慣用移至選取的按鈕。 在使用者可以可能選取的情況下這類網頁型電子郵件應用程式，使用者可能要選取要刪除的核取方塊的多個訊息中的多個記錄提供不是可從選取的按鈕或選項按鈕的功能使用者介面。

本教學課程會查看如何將資料行的選項按鈕加入至 GridView。 繼續進行本教學課程中，瀏覽使用核取方塊。

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>步驟 1： 建立增強 GridView 網頁

強化 GridView 来包含的資料行的選項按鈕，開始之前，可讓 s 先花點時間來建立 ASP.NET 網頁中，我們需要本教學課程和後面兩個網站專案。 加入新的資料夾，名為啟動`EnhancedGridView`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![如需 SqlDataSource 相關教學課程加入 ASP.NET 網頁](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁的 SqlDataSource 相關教學課程


如同在其他資料夾，`Default.aspx`中`EnhancedGridView`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


最後，將這些四個頁面加入做為項目至`Web.sitemap`檔案。 具體來說，加入下列標記後使用 SqlDataSource 控制項`<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入和刪除教學課程的項目。


![增強的 GridView 教學課程的站台地圖現在包含的項目](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**圖 3**： 網站導覽現在包含項目，可以提升 GridView 教學課程


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>步驟 2： 在 GridView 中顯示的供應商

針對本教學課程，可讓 s 建置的 GridView 會來自美國，供應商列出與每個提供的選項按鈕的 GridView 資料列。 選取選項按鈕透過供應商之後, 使用者可以按一下按鈕來檢視供應商的產品。 雖然這項工作可能看似簡單，有進行尤其很難解釋的微妙的數目。 我們深入瞭解這些微妙之前，可讓第一次取得列出供應商的 GridView 的 s。

先開啟`RadioButtonField.aspx`頁面`EnhancedGridView`拖曳 GridView 從 [工具箱] 拖曳至設計工具的資料夾。 設定 GridView s`ID`至`Suppliers`和從其智慧標籤上，選擇 建立新的資料來源。 具體而言，建立名為 ObjectDataSource `SuppliersDataSource` ，其會從提取資料`SuppliersBLL`物件。


[![建立名為 SuppliersDataSource 新 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**圖 4**： 建立新的 ObjectDataSource 具名`SuppliersDataSource`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**圖 5**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


因為我們只是要列出供應商在美國，選用`GetSuppliersByCountry(country)`方法，從下拉式清單中選取索引標籤。


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**圖 6**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


從 更新 索引標籤，選取 （無） 選項，然後按一下 下一步。


[![設定使用 SuppliersBLL 類別 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**圖 7**： 設定要使用 ObjectDataSource`SuppliersBLL`類別 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


因為`GetSuppliersByCountry(country)`方法接受參數、 設定資料來源精靈會提示輸入我們該參數的來源。 若要指定硬式編碼的值 (USA，在此範例中)，會保留來源下拉式清單設定為 None，在文字方塊中輸入的預設值的參數。 按一下 [完成] 5d; 以完成精靈。


[![使用美國做為預設值的國家/地區參數](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**圖 8**： 做為預設值為使用美國`country`參數 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


完成精靈之後，GridView 會 BoundField 包含以每個供應商資料欄位。 移除以外的所有`CompanyName`， `City`，和`Country`BoundFields，並重新命名`CompanyName`BoundFields`HeaderText`供應商的屬性。 之後，請 GridView 和 ObjectDataSource 宣告式語法看起來應該如下所示。


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

本教學課程，可讓 s 允許使用者檢視選取的供應商產品與供應商清單中，在相同頁面上，或不同的頁面上。 若要做到這一點，將兩個按鈕 Web 控制項加入頁面。 我 ve 組`ID`s 至這兩個按鈕的`ListProducts`和`SendToProducts`，與概念，當`ListProducts`按下會發生回傳，且會在相同頁面上，但是在列出所選的供應商的產品`SendToProducts`按一下時，使用者會檢視列出產品的另一個頁面。

圖 9 顯示`Suppliers`GridView 和兩個按鈕 Web 控制項透過瀏覽器檢視時。


[![美國從供應商有其名稱、 縣市和國家/地區資訊列](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**圖 9**： 那些來自供應商 USA 有其名稱、 縣 （市），並列出國家/地區資訊 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>步驟 3： 加入資料行的選項按鈕

此時`Suppliers`GridView 有三個 BoundFields 美國顯示公司名稱、 縣市和國家/地區的每個供應商。 它仍然不過缺少資料行的選項按鈕。 不幸的是，GridView 不包含內建 RadioButtonField，否則我們無法將會加入至方格，來完成。 相反地，我們新增為 TemplateField 並設定其`ItemTemplate`呈現選項按鈕，導致每個 GridView 資料列的選項按鈕。

一開始，我們可能會假設可以實作所需的使用者介面，藉由新增 RadioButton Web 控制項`ItemTemplate`TemplateField。 雖然這確實會增加 GridView 的每個資料列的單一選項按鈕，選項按鈕無法進行分組，並因此不會互斥。 也就是說，終端使用者是能夠同時從 GridView 中選取多個選項按鈕。

即使使用 RadioButton Web 控制項的 TemplateField 不提供我們需要的功能，可讓 s 實作這種方式，因為它 s 值得檢查為什麼未分組結果的選項按鈕。 啟動新增為 TemplateField 至供應商 GridView，因此最左邊的欄位。 接下來，從 GridView s 智慧標籤，按一下 [編輯樣板] 連結，並將 RadioButton Web 控制項從 [工具箱] 拖曳到 TemplateField 的`ItemTemplate`（請參閱圖 10）。 設定 RadioButton s`ID`屬性`RowSelector`和`GroupName`屬性`SuppliersGroup`。


[![ItemTemplate 中加入選項按鈕 Web 控制項](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**圖 10**： 將 RadioButton Web 控制項加入`ItemTemplate`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


進行這些新增項目，透過設計工具後, GridView 的標記看起來應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

RadioButton s [ `GroupName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx)並用來群組一系列的選項按鈕。 所有具有相同的 RadioButton 控制項`GroupName`會被視為分組; 可以從群組選取一個選項按鈕，一次。 `GroupName`屬性指定的值轉譯的選項按鈕的`name`屬性。 瀏覽器會檢查選項按鈕`name`屬性來判斷選項按鈕群組。

使用 RadioButton Web 控制項加入至`ItemTemplate`，請造訪此頁透過瀏覽器，然後按一下 格線 s 資料列中的選項按鈕。 請注意，選項按鈕未分組的方式，讓您可以選取所有資料列，作為圖 11 顯示。


[![GridView 的選項按鈕都不相同的群組](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**圖 11**: W 的選項按鈕是不相同的群組 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


未分組，選項按鈕的原因是因為其呈現`name`屬性會以不同，儘管有具有相同`GroupName`屬性設定。 若要查看這些差異，請從瀏覽器進行檢視/來源檢查的選項按鈕標記：


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

請注意這兩種`name`和`id`屬性不是在 [屬性] 視窗中所述的確切值，但會加上一些其他`ID`值。 其他`ID`加到前面的轉譯值`id`和`name`屬性`ID`之選項按鈕的父控制項`GridViewRow`s `ID` s、 GridView 的`ID`，內容控制項 s `ID`，和 Web Form 的`ID`。 這些`ID`s 會新增，以便分別轉譯在 GridView 的 Web 控制項有唯一`id`和`name`值。

每個呈現控制項的需求不同`name`和`id`因為這是如何瀏覽器可唯一識別每個用戶端和如何它識別到 web 伺服器的動作上的控制項，或已在回傳發生變更。 例如，假設我們想要執行一些伺服器端程式碼，每當 RadioButton s 檢查狀態已變更。 我們無法完成這項作業所設定的選項按鈕 s`AutoPostBack`屬性`true`及建立事件處理常式`CheckChanged`事件。 不過，如果轉譯`name`和`id`的所有選項按鈕都相同，在回傳我們無法判斷哪些特定值選項按鈕已按下。

它的簡短是我們無法使用 RadioButton Web 控制項的 GridView 中建立的資料行的選項按鈕。 相反地，我們必須使用而不是完善的技巧，以確保適當的標記插入每個 GridView 資料列。

> [!NOTE]
> 像 RadioButton Web 控制項，選項按鈕 HTML 控制項加入至範本，將包含唯一`name`屬性，在 「 已取消群組的方格中進行，選項按鈕。 如果您不熟悉 HTML 控制項，放心地忽略這個附註，很少使用 HTML 控制項，尤其是在 ASP.NET 2.0。 但如果您有興趣進一步了解更多資訊，請參閱[K.Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s 部落格文章： [Web 控制項和 HTML 控制項](http://www.odetocode.com/Articles/348.aspx)。


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>將選項按鈕標記中使用常值的控制項

若要正確群組所有 GridView 中選項按鈕，我們要以手動方式插入到的選項按鈕標記`ItemTemplate`。 每個選項按鈕需要相同`name`屬性，但應該具備唯一`id`屬性 （如果我們想要存取透過用戶端指令碼的選項按鈕）。 使用者選取選項按鈕，並張貼回頁面之後，瀏覽器會送回選取的選項按鈕的值`value`屬性。 因此，每個選項按鈕將會需要唯一`value`屬性。 最後，在回傳我們需要確定加入`checked`屬性來選取時，否則會選取範圍和文章回一個選項按鈕，選項按鈕會回復為其預設狀態 （所有未選取）。

有兩種方法可以採取以插入範本中的低層級的標記。 其中一個是不要混用標記和格式化程式碼後置類別中定義的方法呼叫。 第一次述這項技術[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程。 在我們的案例它看起來可能像這樣：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

在這裡，`GetUniqueRadioButton`和`GetRadioButtonValue`會傳回適當的程式碼後置類別中定義的方法`id`和`value`屬性值，每個選項按鈕。 這個方法適用於指派`id`和`value`屬性，但是落短時需要指定`checked`屬性值，因為當資料第一次繫結至 GridView，才會執行資料繫結語法。 因此，如果在 GridView 啟用檢視狀態，格式化的方法將只引發時第一次載入頁面 （或當 GridView 會明確地重新繫結至資料來源），因此函式，可設定`checked`贏了 t 屬性上呼叫回傳。 它 s 而微妙的問題和位元超出範圍的這篇文章，因此這會保留。 不過，我不要鼓勵您嘗試使用上述的方法和透過它運作來，您將會碰到的點。 雖然這類贏了練習 t 協助您取得任何接近工作版本，它有助於促進更深入了解 GridView 和資料繫結的生命週期。

另一種方法來插入自訂，低層級的標記中的範本及我們將使用此教學課程中的方法是加入[literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx)範本。 然後，在 GridView s`RowCreated`或`RowDataBound`事件處理常式，常值，可以透過程式設計方式存取控制項及其`Text`屬性來標記設定為發出。

開始移除 TemplateField 的 RadioButton `ItemTemplate`，取代常值的控制項。 設定 literal s`ID`至`RadioButtonMarkup`。


[![將常值的控制項加入至 ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**圖 12**： 將控制項加入常值`ItemTemplate`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


接下來，建立事件處理常式 GridView s`RowCreated`事件。 `RowCreated`事件引發之後加入的每個資料列，不論資料正在重新繫結至 GridView。 這表示即使在回傳資料會從檢視狀態，重新載入時`RowCreated`仍就會引發事件，這是我們使用，而不是原因`RowDataBound`（這就會引發只有當資料明確繫結至 Web 控制項的資料）。

在此事件處理常式中，我們只想要繼續進行，如果我們重新處理資料列。 我們想要以程式設計方式參考每個資料列`RadioButtonMarkup`常值的控制項並設定其`Text`發出標記的屬性。 如下列程式碼所示，發出的標記建立選項按鈕時`name`屬性設為`SuppliersGroup`、 其`id`屬性設為`RowSelectorX`，其中*X* GridView 資料列的索引且`value`屬性設為 GridView 資料列索引。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

GridView 資料列已選取，並回傳時，我們有興趣`SupplierID`選取供應商。 因此，其中可能會認為每個選項按鈕的值應該是實際`SupplierID`（而不是 GridView 資料列的索引）。 雖然這可能適用於某些情況下，它就會有安全性風險盲目地接受和處理`SupplierID`。 比方說，我們 GridView 中會列出美國供應商。 不過，如果`SupplierID`傳遞直接從選項按鈕，停止惡作劇使用者管理哪些 s`SupplierID`傳送回在回傳值嗎？ 您可以使用資料列索引，做為`value`，然後取得`SupplierID`在回傳時從`DataKeys`集合中，我們可以確保使用者只會使用其中一種`SupplierID`GridView 資料列的其中一個相關聯的值。

加入之後此事件處理常式程式碼，請花一分鐘測試瀏覽器中。 首先，請注意，只有一個選項一次選取方格中的按鈕。 不過，選取選項按鈕，然後按一下其中一個按鈕，就會發生回傳，而且所有的選項按鈕會還原為初始狀態 （也就是在回傳時，選取的選項按鈕已無法再選取）。 若要修正此問題，我們需要加強`RowCreated`事件處理常式，它會檢查寄件者回傳的已選取的選項按鈕索引並新增`checked="checked"`發出標記的資料列的索引比對的屬性。

回傳發生時，瀏覽器送回`name`和`value`的選取的選項按鈕。 擷取的值可以透過程式設計方式使用`Request.Form["name"]`。 [ `Request.Form`屬性](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx)提供[ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx)代表表單變數。 表單變數的名稱和值的表單中的欄位網頁上，而且會傳送回網頁瀏覽器時回傳展示。 因為呈現`name`GridView 中的選項按鈕的屬性就是`SuppliersGroup`、 網頁瀏覽器將會傳送的回張貼時`SuppliersGroup=valueOfSelectedRadioButton`回到網頁伺服器 （以及其他表單欄位）。 然後從存取這項資訊`Request.Form`屬性使用： `Request.Form["SuppliersGroup"]`。

因為我們將需要判斷選取的選項按鈕索引不只在`RowCreated`事件處理常式，但在`Click`按鈕 Web 控制項的事件處理常式，可讓 s 新增`SuppliersSelectedIndex`屬性至程式碼後置類別傳回`-1`如果已選取 [無] 選項按鈕和選取的索引，如果已選取其中一個選項按鈕。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

加入此屬性，我們會知道要新增`checked="checked"`中的標記`RowCreated`事件處理常式時`SuppliersSelectedIndex`等於`e.Row.RowIndex`。 更新的事件處理常式加入這個邏輯：


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

這項變更，與選取之圓鈕仍為選取狀態回傳之後。 現在，我們已經讓您指定哪些選項按鈕已選取時，我們就無法變更行為，使時第一次瀏覽的頁面，選取第一個的 GridView 資料列的選項按鈕 （而不需要任何預設選取的選項按鈕，這是目前行為）。 若要讓第一個預設選取的選項按鈕，只要變更`if (SuppliersSelectedIndex == e.Row.RowIndex)`下列陳述式： `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`。

此時我們加入群組的選項按鈕資料的行至 GridView，可讓單一的 GridView 資料列，選取並記住在回傳。 我們接下來的步驟會顯示所選取的供應商提供的產品。 步驟 4 中我們會看到如何將使用者重新導向至另一個頁面上，沿著所選傳送`SupplierID`。 在步驟 5 中，我們會看到如何在相同頁面上的 GridView 中顯示選取的供應商的產品。

> [!NOTE]
> 而不是使用為 TemplateField （這個冗長的步驟 3 的焦點），我們可以建立自訂`DataControlField`呈現適當的使用者介面與功能的類別。 [ `DataControlField`類別](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx)BoundField、 CheckBoxField、 TemplateField 和其他內建的 GridView 和 DetailsView 欄位衍生自的基底類別。 建立自訂`DataControlField`類別表示的資料行的選項按鈕，可以只使用宣告式語法，加入和也會讓複寫其他網頁和其他大幅簡化的 web 應用程式的功能。


如果您已曾在建立自訂，編譯 ASP.NET 中的控制項，不過，您知道，這樣做需要相當多的 legwork，並且會夾帶微妙和邊緣案例，必須小心處理的主機。 因此，我們將會放棄實作的選項按鈕，以做為自訂的資料行`DataControlField`現在類別，並盡可能 TemplateField 選項使用。 可能是我們必須有機會建立、 使用和部署自訂的瀏覽`DataControlField`未來教學課程中的類別 ！

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>步驟 4： 在個別的頁面中顯示選取的供應商的產品

使用者選取 GridView 資料列之後，我們需要顯示選取的供應商的產品。 在某些情況下，我們可能會想要在個別的頁面中，我們可能會想要以相同的頁面進行其他顯示這些產品。 將第一次檢查如何顯示產品中的個別頁面; s在步驟 5 中我們將探討加入 GridView`RadioButtonField.aspx`顯示選取的供應商的產品。

目前有兩個頁面上的按鈕 Web 控制項`ListProducts`和`SendToProducts`。 當`SendToProducts`按一下按鈕時，我們想要傳送使用者`~/Filtering/ProductsForSupplierDetails.aspx`。 此頁面是在[主要/詳細資料篩選跨兩個頁面](../masterdetail/master-detail-filtering-across-two-pages-cs.md)教學課程，並顯示產品的供應商的`SupplierID`會傳遞名為的 querystring 欄位`SupplierID`。

若要提供這項功能，建立事件處理常式`SendToProducts`按鈕的`Click`事件。 步驟 3 新增`SuppliersSelectedIndex`選取屬性，傳回的資料列索引的選項按鈕。 對應`SupplierID`可以擷取從 GridView s`DataKeys`集合和使用者可以傳送至`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`使用`Response.Redirect("url")`。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

此程式碼作出運作，只要從 GridView 已選取的其中一個選項按鈕。 如果一開始，GridView 沒有選取任何選項按鈕，使用者按一下`SendToProducts` 按鈕，`SuppliersSelectedIndex`將`-1`，這樣將會造成例外狀況之後擲回`-1`超出索引的範圍`DataKeys`集合。 這不是問題，不過，如果您決定要更新`RowCreated`以便在一開始會選取在 GridView 中有第一個選項按鈕的步驟 3 中所述的事件處理常式。

若要容納`SuppliersSelectedIndex`值`-1`，將標籤 Web 控制項上方 GridView 頁面加入。 設定其`ID`屬性`ChooseSupplierMsg`、 其`CssClass`屬性`Warning`、 其`EnableViewState`和`Visible`屬性`false`，並將其`Text`屬性，請選擇供應商的方格中。 CSS 類別`Warning`紅色、 粗體、 斜體大字型顯示的文字，並定義於`Styles.css`。 藉由設定`EnableViewState`和`Visible`屬性`false`，標籤就不會轉譯以外的只有回傳 where 控制項 s`Visible`屬性以程式設計方式設定為`true`。


[![加入標籤 Web 控制項上方的 GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**圖 13**： 加入標籤 Web 控制項上方 GridView ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


接下來，加強`Click`事件處理常式，以顯示`ChooseSupplierMsg`標籤如果`SuppliersSelectedIndex`為小於零，且若要將使用者重新導向`~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID`否則。


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

瀏覽的頁面，在瀏覽器，並按一下`SendToProducts`按鈕，然後再從 GridView 中選取 供應商。 如圖 14 所示，這會顯示`ChooseSupplierMsg`標籤。 接下來，選取 [供應商，然後按一下`SendToProducts`] 按鈕。 這會將您 whisk 到頁面，其中列出所選取的供應商提供的產品。 圖 15 所示`ProductsForSupplierDetails.aspx`網頁時選取 Bigfoot Breweries 供應商。


[![如果已選取 否供應商，會顯示 ChooseSupplierMsg 標籤](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**圖 14**:`ChooseSupplierMsg`標籤會顯示，如果已選取 否供應商 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![選取 供應商的產品會顯示在 ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**圖 15**： 中會顯示選取的供應商的產品`ProductsForSupplierDetails.aspx`([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>步驟 5： 在相同頁面上顯示選取的供應商的產品

步驟 4 中，我們了解如何將使用者傳送至另一個 web 網頁，以顯示所選的供應商產品的內容。 或者，在相同頁面上可以顯示選取的供應商的產品。 為了說明這點，我們會將新增到另一個 GridView`RadioButtonField.aspx`顯示選取的供應商的產品。

因為我們只想要選取供應商之後顯示此 GridView 的產品，將面板 Web 控制項下方`Suppliers`GridView 中設定其`ID`至`ProductsBySupplierPanel`及其`Visible`屬性`false`。 面板中加入文字產品選取供應商，後面加上名為的 GridView `ProductsBySupplier`。 從 GridView s 智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsBySupplierDataSource`。


[![將 ProductsBySupplier GridView 繫結至新 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**圖 16**： 繫結`ProductsBySupplier`新 ObjectDataSource 的 GridView ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


接下來，設定要使用 ObjectDataSource`ProductsBLL`類別。 因為我們只是要擷取這些選取的供應商所提供的產品，指定應叫用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法來擷取其資料。 （無） 從清單中選取下拉式清單中更新、 插入和刪除索引標籤。


[![設定為使用 GetProductsBySupplierID(supplierID) 方法 ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**圖 17**： 設定要使用 ObjectDataSource`GetProductsBySupplierID(supplierID)`方法 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![設定為 （無） 的下拉式清單中更新、 插入和刪除索引標籤](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**圖 18**： 設定更新、 插入和刪除的索引標籤中的下拉式清單會列出為 （無） ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


設定 SELECT 之後, 更新、 插入，和刪除索引標籤，按一下 [下一步]。 因為`GetProductsBySupplierID(supplierID)`方法預期的輸入的參數，建立資料來源精靈會提示我們指定參數的值的來源。

我們有幾個這裡在指定的參數 s 的值來源的選項。 我們可以使用預設參數的物件，並以程式設計方式將指定的值`SuppliersSelectedIndex`參數 s 的屬性`DefaultValue`ObjectDataSource s 中的屬性`Selecting`事件處理常式。 回頭參考[以程式設計方式設定 ObjectDataSource 參數值](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)教學課程的重新整理程式以程式設計方式將值指派給 ObjectDataSource 的參數。

或者，我們可以使用 ControlParameter，請參閱`Suppliers`GridView s [ `SelectedValue`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx)（請參閱圖 19）。 GridView s`SelectedValue`屬性會傳回`DataKey`值對應至[`SelectedIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx)。 這個選項才能正常運作，我們必須以程式設計方式設定 GridView s`SelectedIndex`屬性至所選資料列時`ListProducts`按鈕。 額外的好處在於，藉由設定`SelectedIndex`，將不執行選取的記錄`SelectedRowStyle`中定義`DataWebControls`佈景主題 （黃色背景）。


[![使用 ControlParameter 指定 GridView 的 SelectedValue 做為參數來源](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**圖 19**： 用於 ControlParameter GridView 的 SelectedValue 指定做為參數的來源 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


正在完成精靈，在 Visual Studio 會自動加入為產品的資料欄位的欄位。 移除以外的所有`ProductName`， `CategoryName`，和`UnitPrice`BoundFields，並變更`HeaderText`產品、 類別和價格的屬性。 設定`UnitPrice`BoundField，使其值的格式為貨幣。 進行這些變更之後，面板、 GridView 中和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

若要完成此練習中，所以我們需要將 GridView s`SelectedIndex`屬性`SelectedSuppliersIndex`和`ProductsBySupplierPanel`面板 s`Visible`屬性`true`時`ListProducts`按鈕。 若要達成此目的，建立事件處理常式`ListProducts`按鈕 Web 控制項的`Click`事件並加入下列程式碼：


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

如果尚未從 GridView 中選取供應商`ChooseSupplierMsg`標籤會顯示與`ProductsBySupplierPanel`隱藏面板。 否則，如果選取了供應商，`ProductsBySupplierPanel`會顯示與 GridView 的`SelectedIndex`更新屬性。

圖 20 在已選取 Bigfoot Breweries 供應商，並顯示產品頁面 5d; 按鈕已按下之後顯示的結果。


[![Bigfoot Breweries 所提供的產品會列在相同的頁面](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**圖 20**: 產品所提供的 Bigfoot Breweries 會列在相同的頁面 ([按一下以檢視完整大小的影像](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>總結

中所述[主要/詳細說明可選取的主要 GridView 使用詳細資料 DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)教學課程，可以從使用 CommandField GridView 中選取記錄其`ShowSelectButton`屬性設定為`true`。 但 CommandField 顯示按鈕為標準按鈕、 連結或影像。 替代的資料列選取使用者介面是提供的選項按鈕或核取方塊位於每個 GridView 資料列。 在此教學課程中我們會檢查如何加入資料行的選項按鈕。

不幸的，加入資料行的選項按鈕且 t 為簡單或簡單可能會以為。 沒有任何內建 RadioButtonField 可以在按一下按鈕，加入並使用選項按鈕 Web 控制項內 TemplateField 介紹組自己的問題。 最後，為了提供這類介面我們可能需要建立自訂`DataControlField`類別或適當的 HTML 注入 TemplateField 期間的解決辦法`RowCreated`事件。

具有探索如何將資料行的選項按鈕，讓我們開啟我們注意到新增的資料行的核取方塊。 具有資料行的核取方塊，使用者可以選取一個或多個 GridView 資料列，並接著執行所有選取的資料列 （例如，選取一組電子郵件從網頁型電子郵件用戶端，然後選擇刪除所有選取的電子郵件） 的某些作業。 在下一個教學課程中，我們會看到如何將這類資料行。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 David Suru。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](adding-a-gridview-column-of-checkboxes-cs.md)
