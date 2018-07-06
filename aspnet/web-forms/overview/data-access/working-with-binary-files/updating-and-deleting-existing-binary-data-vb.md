---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: 更新和刪除現有的二進位資料 (VB) |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中，我們看到如何在 GridView 控制項輕鬆編輯和刪除文字資料。 在本教學課程中，我們看到如何在 GridView 控制項也讓...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd5aea4bfc1a38dc3364cdf2657d3dca2b82022c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830451"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>更新和刪除現有的二進位資料 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe)或[下載 PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> 在先前的教學課程中，我們看到如何在 GridView 控制項輕鬆編輯和刪除文字資料。 在本教學課程中，我們看到如何在 GridView 控制項也讓您能夠編輯和刪除二進位資料，無論該二進位資料是儲存在資料庫或儲存在檔案系統。


## <a name="introduction"></a>簡介

過去三個教學課程透過我們已新增一堆使用二進位資料的功能。 我們開始新增`BrochurePath`資料行`Categories`資料表，並據以更新架構。 我們也新增了資料存取層和商務邏輯層的方法，以使用現有類別目錄資料表的`Picture`資料行，其中包含二進位內容 s 的映像檔。 我們已建置網頁以呈現 GridView 中的二進位資料的摺頁冊，下載連結類別的圖片所示`<img>`項目並已新增可允許使用者新增新的類別，並將其摺頁冊] 和 [圖片的資料上傳 DetailsView。

所有實作是編輯和刪除現有的類別，我們將完成的作業在本教學課程使用 GridView s 內建編輯和刪除功能的能力。 當您編輯類別目錄，使用者將能夠選擇上傳新的圖片，或繼續使用現有的類別。 手冊，他們可以選擇使用現有的手冊上, 傳新的手冊，或表示類別目錄不再具有與其相關聯的手冊。 讓 s 開始 ！

## <a name="step-1-updating-the-data-access-layer"></a>步驟 1： 更新資料存取層

DAL 已自動產生`Insert`， `Update`，並`Delete`方法，但這些方法所產生根據`CategoriesTableAdapter`s 主查詢，不包括`Picture`資料行。 因此，`Insert`和`Update`方法不包含的參數來指定類別目錄的圖片的二進位資料。 如同[前述教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)，我們需要建立一個新的 TableAdapter 方法來更新`Categories`資料表時指定的二進位資料。

開啟 輸入資料集，並從設計工具中，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後選擇 加入查詢從內容功能表來 launche TableAdapter 查詢組態精靈。 此精靈一開始會詢問 TableAdapter 查詢應該如何存取資料庫。 選擇 使用 SQL 陳述式，然後按一下 下一步。 下一個步驟會產生提示的查詢類型。 因為我們重新建立要新增新的記錄，以查詢`Categories`資料表中，選擇 更新並按一下 下一步。


[![選取 [更新] 選項](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**圖 1**： 選取 [更新] 選項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


我們現在需要指定`UPDATE`SQL 陳述式。 精靈會自動建議`UPDATE`對應至 TableAdapter s 主查詢的陳述式 (更新的那個`CategoryName`， `Description`，和`BrochurePath`值)。 變更陳述式以便`Picture`資料行是否包含連同`@Picture`參數，就像這樣：


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

在精靈的最後一個畫面會要求我們命名新的 TableAdapter 方法。 輸入`UpdateWithPicture`按一下 [完成]。


[![命名新的 TableAdapter 方法 UpdateWithPicture](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**圖 2**： 將新的 TableAdapter 方法命名`UpdateWithPicture`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>步驟 2： 新增商務邏輯層方法

除了更新 DAL，我們需要更新 BLL，包括更新和刪除類別目錄的方法。 這些是從展示層將會叫用的方法。

正在刪除類別，我們可以使用`CategoriesTableAdapter`自動產生的 s`Delete`方法。 將下列方法加入`CategoriesBLL`類別：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

本教學課程中，可讓 s 會建立兩種方法來更新類別目錄-其中預期圖片的二進位資料，並叫用`UpdateWithPicture`我們剛才加入至方法`CategoriesTableAdapter`另一個則接受只`CategoryName`， `Description`，和`BrochurePath`值，並且使用`CategoriesTableAdapter`類別自動產生的 s`Update`陳述式。 使用兩種方法背後的原理是，在某些情況下，使用者可能想要更新分類的圖片，以及其其他欄位，在其中情況下使用者必須上傳新的圖片。 上傳的圖片 s 二進位資料則用於`UPDATE`陳述式。 在其他情況下，使用者可能只在意更新，說出、 名稱和描述。 但若是`UPDATE`陳述式必須要有的二進位資料`Picture`資料行，則我們 d 需要提供相關資訊。 這會需要額外來回存取資料庫正在編輯之資料錄找回圖片資料。 因此，我們希望這兩個`UPDATE`方法。 商務邏輯層將會決定要使用哪一個根據更新類別目錄時，是否提供圖片的資料。

若要達成此目的，將新增兩種方法可以`CategoriesBLL`類別，這兩名為`UpdateCategory`。 第一個應該接受三個`String`s，`Byte`的陣列，以及`Integer`做為輸入參數; 第二、 三`String`s 和`Integer`。 `String`輸入的參數是類別 s 的名稱、 描述和冊檔案路徑，如`Byte`陣列為類別的圖片的二進位內容並`Integer`識別`CategoryID`来更新的記錄。 請注意，第一個多載會叫用第二個 if 傳入`Byte`陣列是`Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>步驟 3： 複製的插入和檢視功能

在 [前述教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)我們建立名為的頁面`UploadInDetailsView.aspx`，列出 GridView 中的所有類別目錄，並提供將新類別新增至系統的 DetailsView。 在本教學課程中，我們會擴充包含編輯和刪除支援 GridView。 而不是從持續`UploadInDetailsView.aspx`，可讓 s 改為放在此教學課程的變更`UpdatingAndDeleting.aspx`頁面上，從相同的資料夾， `~/BinaryData`。 複製並貼上的宣告式標記，然後從程式碼`UploadInDetailsView.aspx`至`UpdatingAndDeleting.aspx`。

首先開啟`UploadInDetailsView.aspx`頁面。 複製所有的宣告式語法內`<asp:Content>`項目，如 [圖 3] 所示。 接下來，開啟`UpdatingAndDeleting.aspx`並貼上此標記內其`<asp:Content>`項目。 同樣地，程式碼複製`UploadInDetailsView.aspx`頁面上將程式碼後置類別的`UpdatingAndDeleting.aspx`。


[![宣告式標記複製 UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**圖 3**： 複製從宣告式標記`UploadInDetailsView.aspx`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


之後複製的宣告式標記和程式碼，請瀏覽`UpdatingAndDeleting.aspx`。 您應該會看到相同的輸出和具有相同的使用者經驗如同`UploadInDetailsView.aspx`從上一個教學課程的頁面。

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>步驟 4： 加入刪除 ObjectDataSource 和 GridView 的支援

如我們所討論年代[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，GridView 提供內建的刪除功能，並可以在核取方塊的刻度啟用這些功能，如果格線 s 基礎資料來源支援刪除。 目前的 ObjectDataSource GridView 繫結至 (`CategoriesDataSource`) 不支援刪除。

若要解決此問題，按一下從 ObjectDataSource s 智慧標籤的設定資料來源選項，啟動精靈。 第一個畫面會顯示 ObjectDataSource 會設定為搭配`CategoriesBLL`類別。 按 [下一步]。 目前，只有 ObjectDataSource s`InsertMethod`和`SelectMethod`指定屬性。 不過，精靈自動填入下拉式清單中的更新和刪除索引標籤`UpdateCategory`和`DeleteCategory`方法，分別。 這是因為在`CategoriesBLL`我們標記使用這些方法的類別`DataObjectMethodAttribute`更新和刪除的預設方法。

現在，更新 s 索引標籤下拉式清單 （無），但保留刪除 s 索引標籤下拉式清單設為`DeleteCategory`。 我們會返回這個精靈，在步驟 6 來新增更新的支援。


[![設定為使用 DeleteCategory 方法的 ObjectDataSource](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`DeleteCategory`方法 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> 在完成精靈，Visual Studio 可能會要求是否您想要重新整理欄位和索引鍵，這會重新產生資料 Web 控制項欄位。 選擇 否，因為選擇 是 會覆寫您所做的任何欄位自訂項目。


ObjectDataSource 現在將包含的值及其`DeleteMethod`屬性，以及`DeleteParameter`。 您應該記得，使用精靈時指定的方法，Visual Studio 會設定 ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`，這會造成問題的更新和刪除方法引動過程。 因此，完全清除這個屬性，或重設為預設值， `{0}`。 如果您需要重新整理您在這個 ObjectDataSource 的屬性上的記憶體，請參閱[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程。

正在完成精靈並修正後`OldValuesParameterFormatString`，ObjectDataSource s 宣告式標記看起來應該類似如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

設定 ObjectDataSource 之後, 將刪除的功能新增至 GridView，藉由檢查啟用刪除 核取方塊的 GridView s 智慧標籤。 這會新增至 GridView 的 CommandField 其`ShowDeleteButton`屬性設定為`True`。


[![啟用適用於在 GridView 中刪除支援](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**圖 5**： 啟用刪除 GridView 裡的支援 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


請花一點時間來測試刪除功能。 沒有外部索引鍵之間`Products`表格 s`CategoryID`並`Categories`表格的`CategoryID`，因此如果您嘗試刪除任何前八個類別，您會收到外部索引鍵條件約束違規的例外狀況。 若要測試這項功能時，新增新的類別，提供手冊和圖片。 [圖 6] 所示我測試類別包含名為測試摺頁冊檔案`Test.pdf`和測試圖片。 圖 7 顯示 GridView 之後已加入測試分類。


[![新增使用手冊和影像的測試類別](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**圖 6**： 新增使用手冊和影像的測試類別 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![在插入之後測試類別，它會顯示在 GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**圖 7**： 在插入之後測試類別，它會顯示在 GridView ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


在 Visual Studio 中，重新整理 [方案總管]。 您現在應該會看到新的檔案中`~/Brochures`資料夾， `Test.pdf` （請參閱 圖 8）。

接下來，按一下 測試分類的資料列，讓頁面回傳中的 刪除 連結並`CategoriesBLL`類別的`DeleteCategory`方法來引發。 這將會叫用的 DAL 秒`Delete`方法，造成適當`DELETE`傳送至資料庫的陳述式。 資料然後重新繫結至 GridView，標記會傳送回用戶端使用測試分類不會再出現。

雖然刪除工作流程已成功移除從測試分類記錄`Categories`資料表，它並未從 web 伺服器檔案系統中移除其摺頁冊檔案。 重新整理 [方案總管]，然後您會看到`Test.pdf`仍然位於`~/Brochures`資料夾。


![Test.pdf 檔案未刪除從 Web 伺服器檔案系統](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**圖 8**:`Test.pdf`檔案未刪除從 Web 伺服器檔案系統


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>步驟 5： 移除已刪除的分類 s 摺頁冊檔

其中一個儲存在資料庫外部的二進位資料的缺點是必須採取額外的步驟來刪除相關聯的資料庫記錄時，清除這些檔案。 GridView 和 ObjectDataSource 提供事件引發之前和之後執行 delete 命令。 實際上，我們需要建立前置和後置動作事件的事件處理常式。 之前`Categories`記錄刪除我們要確定 PDF 檔案的路徑，但我們不想要刪除 PDF，萬一沒有某些例外狀況，而且不會刪除類別目錄中刪除類別目錄之前。

GridView s [ `RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)引發已叫用 ObjectDataSource s delete 命令之前，而其[`RowDeleted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx)之後，就會引發。 建立使用下列程式碼這兩個事件的事件處理常式：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

在`RowDeleting`事件處理常式`CategoryID`的資料列在從 GridView s 抓取正在刪除`DataKeys`集合，可透過此事件處理常式中存取`e.Keys`集合。 下一步`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`會叫用來傳回正在刪除記錄的相關資訊。 如果傳回`CategoriesDataRow`物件具有非`NULL``BrochurePath`值則儲存在頁面變數`deletedCategorysPdfPath`，以便可以刪除該檔案，在`RowDeleted`事件處理常式。

> [!NOTE]
> 而不是擷取`BrochurePath`詳細資料`Categories`記錄中刪除`RowDeleting`事件處理常式中，我們可以另外新增了`BrochurePath`GridView s`DataKeyNames`屬性和存取記錄的值透過`e.Keys`集合。 這樣會稍微增加 GridView 的檢視狀態大小，但會減少所需的程式碼並將車程儲存至資料庫。


已叫用 s 基礎的 delete 命令，ObjectDataSource GridView s 之後`RowDeleted`引發事件處理常式。 如果沒有在刪除資料的例外狀況，而且沒有值`deletedCategorysPdfPath`，則 PDF 會從檔案系統中刪除。 請注意，此額外的程式碼不需要它的相片與相關聯的類別目錄的 s 二進位資料進行清除。 S 因為圖片資料直接在資料庫中儲存的因此刪除`Categories`資料列也會刪除該類別的圖片資料。

在新增之後的兩個事件處理常式，請再次執行此測試案例。 當刪除類別目錄，也會刪除其相關聯的 PDF。

更新現有的記錄相關聯的 s 二進位資料會提供一些有趣的挑戰。 本教學課程的其餘部分深入探討加入更新功能的手冊和圖片。 步驟 6 將探討當步驟 7 會查看更新的圖片時，更新摺頁冊資訊的技術。

## <a name="step-6-updating-a-category-s-brochure"></a>步驟 6： 更新分類的手冊

概觀的插入、 更新和刪除資料教學課程中所述，GridView 會提供內建資料列層級的編輯支援，可由一個核取方塊的刻度實作，如果已適當地設定其基礎資料來源。 目前， `CategoriesDataSource` ObjectDataSource 不包含更新的支援，因此可讓 s 新增，在尚未設定。

按一下 設定資料來源連結，從 ObjectDataSource 的精靈，請繼續進行第二個步驟。 由於`DataObjectMethodAttribute`用於`CategoriesBLL`，[更新] 下拉式清單應該會自動填入`UpdateCategory`接受四個輸入參數的多載 (所有資料行，但`Picture`)。 使其使用以五個參數的多載，可以變更此選項。


[![設定要使用包含參數圖片 UpdateCategory 方法的 ObjectDataSource](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**圖 9**： 設定要使用 ObjectDataSource`UpdateCategory`方法，其中包含的參數`Picture`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource 現在將包含的值及其`UpdateMethod`屬性，以及對應`UpdateParameter`s。 步驟 4 所述，Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`時使用 [設定資料來源精靈]。 這將會造成問題的更新和刪除方法引動過程。 因此，完全清除這個屬性，或重設為預設值， `{0}`。

正在完成精靈並修正後`OldValuesParameterFormatString`，ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

若要開啟的 GridView s 內建編輯功能，請檢查 GridView s 智慧標籤的 啟用編輯選項。 這將會設定 CommandField s`ShowEditButton`屬性設`True`，產生的新加入的 編輯 按鈕 （正在編輯之資料列的更新 和 取消 按鈕）。


[![設定 GridView，以支援編輯](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**圖 10**： 設定 GridView，以支援編輯 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


請瀏覽透過瀏覽器頁面，然後按一下其中一個資料列的編輯按鈕。 `CategoryName`和`Description`BoundFields 會轉譯為文字方塊。 `BrochurePath` TemplateField 缺少`EditItemTemplate`，因此它會繼續顯示其`ItemTemplate`手冊的連結。 `Picture` ImageField 呈現為文字方塊其`Text`ImageField s 的值指派給屬性`DataImageUrlField`值，在此情況下`CategoryID`。


[![GridView 缺少 BrochurePath 編輯介面](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**圖 11**: GridView 缺少的編輯介面`BrochurePath`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>自訂`BrochurePath`s 編輯介面

我們要建立的編輯介面`BrochurePath`TemplateField，可讓其中一個使用者的其中一個：

- 保留做為類別 s 摺頁冊-，
- 更新類別目錄的手冊上傳新的手冊，或
- （在類別目錄不再具有相關聯的手冊的情況下） 完全移除類別目錄 s 摺頁冊。

我們也需要更新`Picture`ImageField s 編輯介面，但我們將會對此步驟 7。

從 GridView s 智慧標籤，按一下 [編輯範本] 連結，然後選取`BrochurePath`TemplateField 的`EditItemTemplate`從下拉式清單。 RadioButtonList Web 控制項加入此範本中，設定其`ID`屬性，以`BrochureOptions`及其`AutoPostBack`屬性設`True`。 從 [屬性] 視窗中，按一下中的省略符號`Items`屬性，這會顯示`ListItem`集合編輯器。 新增下列三個選項搭配`Value`的 1、 2 和 3，分別：

- 使用目前的手冊
- 移除目前的手冊
- 上傳新的手冊

設定第一個`ListItem`s`Selected`屬性設`True`。


![三個 ListItems 加入 RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**圖 12**： 新增三個`ListItem`RadioButtonList 的 s


RadioButtonList，下方新增 FileUpload 控制項，名為`BrochureUpload`。 設定其`Visible`屬性設`False`。


[![新增 EditItemTemplate RadioButtonList 和 FileUpload 控制項](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**圖 13**： 新增 FileUpload 控制項和 RadioButtonList `EditItemTemplate` ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


此 RadioButtonList 提供三個使用者的選項。 其概念是最後一個選項，也就是上傳新的手冊，已選取時，才會顯示的 FileUpload 控制項。 若要達成此目的，建立 事件處理常式 RadioButtonList s`SelectedIndexChanged`事件，並新增下列程式碼：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

由於 RadioButtonList 及 FileUpload 控制項範本中，我們必須撰寫的程式碼，以程式設計方式存取這些控制項。 `SelectedIndexChanged` RadioButtonList 中的參考傳遞給事件處理常式`sender`輸入的參數。 若要取得 FileUpload 控制項，我們需要取得 RadioButtonList 的父控制項，並使用`FindControl("controlID")`從該處的方法。 一旦我們擁有的 RadioButtonList 」 和 「 FileUpload 控制項的參考，FileUpload 控制項 s`Visible`屬性設定為`True`只有當 RadioButtonList s`SelectedValue`等於 3，這是`Value`的上傳新摺頁冊`ListItem`.

使用此程式碼就緒之後，花點時間來測試編輯介面。 按一下 [編輯] 按鈕的資料列。 一開始，您應該選取目前使用的手冊選項。 變更選取的索引，就會引發回傳。 如果選取第三個選項，則顯示 FileUpload 控制項，否則它會隱藏。 圖 14 顯示的編輯介面，當第一次按一下 編輯 按鈕;選取上傳新的手冊選項後，圖 15 顯示介面。


[![一開始，使用目前摺頁冊選項](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**圖 14**： 一開始，使用目前摺頁冊選項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![選擇上傳新摺頁冊選項會顯示 FileUpload 控制項](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**圖 15**： 選擇上傳新摺頁冊選項會顯示 FileUpload 控制項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>正在儲存摺頁冊檔案和更新`BrochurePath`資料行

按一下 [GridView 的更新] 按鈕時，其`RowUpdating`引發事件。 ObjectDataSource 會叫用 s update 命令，然後 GridView 的`RowUpdated`引發事件。 刪除工作流程中，我們需要這兩個事件建立事件處理常式。 在 `RowUpdating`事件處理常式中，我們必須決定要採取的動作將會根據`SelectedValue`的`BrochureOptions`RadioButtonList:

- 如果`SelectedValue`為 1，我們想要繼續使用相同`BrochurePath`設定。 因此，我們需要設定 ObjectDataSource s`brochurePath`至現有的參數`BrochurePath`正在更新資料錄的值。 ObjectDataSource s`brochurePath`參數可以使用設定`e.NewValues["brochurePath"] = value`。
- 如果`SelectedValue`為 2，則我們想要設定記錄 s`BrochurePath`值`NULL`。 這可藉由設定 ObjectDataSource s`brochurePath`參數來`Nothing`，這會導致資料庫`NULL`使用於`UPDATE`陳述式。 如果沒有要移除的現有摺頁冊檔案，我們必須刪除現有的檔案。 不過，我們只想要這樣做，如果在更新完成，而不會引發例外狀況。
- 如果`SelectedValue`為 3，則我們想要確定使用者已上傳為 PDF 檔案，然後將它儲存到檔案系統並更新該記錄的`BrochurePath`資料行的值。 此外，如果沒有現有的手冊檔案已被取代，我們必須刪除先前的檔案。 不過，我們只想要這樣做，如果在更新完成，而不會引發例外狀況。

當完成所需的步驟 RadioButtonList s`SelectedValue`為 3 會幾乎栖瓾樾 DetailsView s`ItemInserting`事件處理常式。 從我們中新增了 DetailsView 控制項加入新的類別記錄時，會執行此事件處理常式[先前的教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)。 因此，它 behooves 我們重構出此功能分成不同的方法。 具體來說，我移出通用的功能分成兩種方法：

- `ProcessBrochureUpload(FileUpload, out bool)` FileUpload 控制項執行個體和輸出布林值，指定是否刪除或編輯作業應該繼續進行，或如果應該取消發生某個驗證錯誤，請接受做為輸入。 此方法會傳回儲存的檔案路徑或`null`如果不儲存任何檔案。
- `DeleteRememberedBrochurePath` 刪除頁面變數中的路徑所指定的檔案`deletedCategorysPdfPath`如果`deletedCategorysPdfPath`不是`null`。

這兩種方法的程式碼會遵循。 請注意之間的相似性`ProcessBrochureUpload`和 DetailsView 的`ItemInserting`從上一個教學課程的事件處理常式。 在本教學課程中，我已更新 DetailsView 的事件處理常式，若要使用這些新方法。 下載此教學課程，請參閱 DetailsView 的事件處理常式中所做的修改相關聯的程式碼。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s`RowUpdating`並`RowUpdated`事件處理常式會使用`ProcessBrochureUpload`和`DeleteRememberedBrochurePath`方法，如下列程式碼所示：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

請注意如何`RowUpdating`事件處理常式會使用一系列的條件陳述式來執行適當的動作，根據`BrochureOptions`RadioButtonList 的`SelectedValue`屬性值。

使用此程式碼就緒之後，您可以編輯類別目錄，並讓它使用其目前摺頁冊、 不使用任何手冊，或上傳新。 請繼續並試試看。在中設定中斷點`RowUpdating`和`RowUpdated`事件處理常式，以了解工作流程。

## <a name="step-7-uploading-a-new-picture"></a>步驟 7： 上傳新的圖片

`Picture` ImageField s 編輯介面呈現為文字方塊的值填入其`DataImageUrlField`屬性。 在編輯工作流程中，期間 GridView 將參數傳遞至參數的名稱與 ObjectDataSource ImageField 的值`DataImageUrlField`屬性和參數 s 的值輸入到文字方塊中編輯介面。 此行為時，適合映像會儲存為檔案系統上的檔案和`DataImageUrlField`包含映像的完整 URL。 使用這種情況下，編輯介面會顯示影像的 URL，在文字方塊中，而使用者可以變更儲存回資料庫。 授與，此預設介面不允許使用者上傳新的映像，但會讓它們將影像的 URL 從目前的值變更為另一個。 本教學課程中，不過，編輯介面 ImageField 預設不會不敷使用因為`Picture`直接在資料庫中儲存二進位資料而`DataImageUrlField`屬性會保存只`CategoryID`。

若要進一步了解當使用者編輯 ImageField 具有的資料列，在我們的教學課程中會發生什麼情況，請考慮下列範例： 使用者編輯的資料列`CategoryID`10，造成`Picture`ImageField 呈現為文字方塊值為 10。 假設使用者在此文字方塊中的值變更為 50，並按一下 [更新] 按鈕。 回傳和 GridView 一開始會建立名為的參數`CategoryID`值 50。 不過，GridView 會傳送此參數之前 (和`CategoryName`並`Description`參數)，它會在中的值新增`DataKeys`集合。 因此，它會覆寫`CategoryID`參數與目前的資料列 s 基礎`CategoryID`值 10。 簡單地說，編輯介面 ImageField s 就不會影響編輯流程本教學課程中因為名稱 ImageField s`DataImageUrlField`屬性和格線的`DataKey`值是在相同的其中一個。

ImageField 可以輕鬆顯示資料庫的資料為基礎的映像，而我們不想要提供的文字方塊中編輯介面。 相反地，我們想要提供使用者可用來變更類別的圖片 FileUpload 控制項。 不同於`BrochurePath`值，這些教學課程中我們已決定要在每個分類都必須有一張圖片。 因此，我們不讓使用者指示沒有任何相關聯的圖片使用者可能會將.vhd 檔案新增圖片，或保留為目前的圖片的 t 需要-是。

若要自訂 ImageField s 編輯介面，我們需要將其轉換為 TemplateField。 從 GridView s 智慧標籤，按一下 [編輯資料行] 連結、 選取 ImageField，並按一下轉換此欄位轉換 TemplateField 連結。


![ImageField 轉換為 TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**圖 16**: ImageField 轉換為 TemplateField


ImageField 轉換為 TemplateField 以這種方式，就會產生為 TemplateField 與兩個範本。 如下列宣告式語法所示`ItemTemplate`包含映像的 Web 控制項`ImageUrl`屬性會被指派使用 ImageField s 為基礎的資料繫結語法`DataImageUrlField`和`DataImageUrlFormatString`屬性。 `EditItemTemplate`包含文字方塊之`Text`屬性繫結至所指定的值`DataImageUrlField`屬性。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

我們需要更新`EditItemTemplate`使用 FileUpload 控制項。 從 GridView s 智慧標籤，按一下 編輯範本連結，然後選取`Picture`TemplateField 的`EditItemTemplate`從下拉式清單。 您應該會看到 TextBox，移除此範本中。 接下來，從 [工具箱] 拖曳 FileUpload 控制項，在範本中，設定其`ID`至`PictureUpload`。 也加入的文字變更的類別目錄的圖片，請指定新的圖片。 若要保留相同類別目錄的圖片，將欄位保留空白範本，以及。


[![新增 FileUpload 控制項的 EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**圖 17**： 新增 FileUpload 控制項`EditItemTemplate`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


自訂編輯介面之後, 請在瀏覽器中檢視您的進度。 檢視時的資料列在唯讀模式下，類別 s 映像會顯示之前，但按一下 [編輯] 按鈕呈現為使用 FileUpload 控制項文字的圖片資料行。


[![編輯介面包含 FileUpload 控制項](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**圖 18**： 編輯介面包含 FileUpload 控制項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


還記得 ObjectDataSource 會設定為呼叫`CategoriesBLL`類別 s`UpdateCategory`接受做為輸入為圖片的二進位資料的方法`Byte`陣列。 這個陣列是否`Nothing`，不過替代`UpdateCategory`多載呼叫時，哪些問題`UPDATE`不會修改的 SQL 陳述式`Picture`資料行，藉此讓類別 s 目前圖片保持不變。 因此，在 GridView s`RowUpdating`事件處理常式，我們需要以程式設計方式參考`PictureUpload`FileUpload 控制項，並判斷檔案已上傳。 如果其中一個未上傳，則我們*未*想要指定的值`picture`參數。 另一方面，如果檔案已上傳中`PictureUpload`FileUpload 控制項中，我們想要確保它是 JPG 檔案。 如果是，則我們可以將其二進位內容傳送至透過 ObjectDataSource`picture`參數。

像是程式碼後在步驟 6 中，大部分的程式碼所需這裡已經存在於 DetailsView 的`ItemInserting`事件處理常式。 因此，我已重構為新的方法的一般功能`ValidPictureUpload`，並更新`ItemInserting`事件處理常式，才能使用這個方法。

將下列程式碼新增至 GridView 的開頭`RowUpdating`事件處理常式。 其重要這段程式碼前面的程式碼，將儲存摺頁冊檔案，因為我們不想要將摺頁冊儲存到 web 伺服器檔案系統中，如果無效的圖片檔案上傳。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)` ; 方法會採用 FileUpload 控制項作為其唯一的輸入參數中，並檢查上傳的檔案 s 延伸模組，以確保上傳的檔案是 JPG 上傳圖片檔案時，只會呼叫它。 如果上不傳任何檔案時，則圖片參數未設定，並因此會使用其預設值是`Nothing`。 如果已上傳圖片和`ValidPictureUpload`會傳回`True`，則`picture`參數所屬的上傳的映像; 的二進位資料，如果此方法會傳回`False`、 已取消更新工作流程和事件處理常式結束。

`ValidPictureUpload(FileUpload)`方法程式碼重構從 DetailsView 的`ItemInserting`事件處理常式，如下：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>步驟 8: Jpg 以取代原始類別圖片

您應該記得原始的八個類別目錄圖片是包裝在 OLE 標頭中的點陣圖檔案。 既然我們已新增的功能來編輯現有的記錄的圖片，請花一點時間這些點陣圖取代 Jpg。 如果您想要繼續使用目前的類別目錄圖片，您可以將它們轉換成 Jpg 藉由執行下列步驟：

1. 將點陣圖影像儲存到硬碟機。 請瀏覽`UpdatingAndDeleting.aspx`頁面在瀏覽器，並針對每個前八個類別中的映像上按一下滑鼠右鍵，選擇要儲存圖片。
2. 在您選擇的影像編輯器中開啟的影像。 您可以使用 Microsoft 小畫家 中，例如。
3. 將點陣圖儲存為 JPG 影像中。
4. 更新類別目錄的圖片，透過編輯介面，並使用該 JPG 檔案。

編輯類別和之後上傳 JPG 映像，映像不會呈現在瀏覽器因為`DisplayCategoryPicture.aspx`頁面移除前的 78 個位元組的前八個類別中的圖片。 藉由移除 OLE 標頭移除所執行的程式碼修正此問題。 在之後執行此操作，`DisplayCategoryPicture.aspx``Page_Load`事件處理常式應該只是下列的程式碼：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx`頁面插入和編輯介面無法使用更多的工作。 `CategoryName`和`Description`BoundFields GridView 與 DetailsView 中的應該轉換成 TemplateFields。 由於`CategoryName`不允許`NULL`值，應該加入 RequiredFieldValidator。 和`Description`文字方塊可能應該轉換成多行文字方塊中。 我在練習中保留這些最後的修飾，為您。


## <a name="summary"></a>總結

本教學課程中完成我們看看使用二進位資料。 在本教學課程和先前的三個，我們看到如何二進位資料可以儲存在檔案系統上，或直接在資料庫內。 使用者選取檔案，從其硬碟機，然後將它上傳至 web 伺服器，是儲存在檔案系統上或插入至資料庫提供給系統的二進位資料。 ASP.NET 2.0 包含 FileUpload 控制項，可提供這類介面鬆拖放一樣簡單。 不過，如中所述[上傳檔案](uploading-files-vb.md)教學課程中，FileUpload 控制項才適合用於相對較小的檔案上傳，在理想情況下未超過了 1 mb。 此外，我們也會探討如何上傳的資料關聯的基礎資料模型，以及如何編輯和刪除的二進位資料，從現有的記錄。

我們的下一個教學課程一組探索各種不同的快取技術。 快取提供一個方法來改善應用程式 s 的整體效能耗費資源的作業的結果並將它們儲存在可以更快速地存取的位置。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](including-a-file-upload-option-when-adding-a-new-record-vb.md)
