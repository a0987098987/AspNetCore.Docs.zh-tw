---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: 更新和刪除現有的二進位資料 (VB) |Microsoft 文件
author: rick-anderson
description: 在先前的教學課程中，我們看到 GridView 控制項如何讓輕鬆地編輯及刪除文字資料。 在本教學課程中，我們看到 GridView 控制項也讓...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 37e32167cccd1b9a98b629179cdaeb9e193f88b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>更新和刪除現有的二進位資料 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe)或[下載 PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> 在先前的教學課程中，我們看到 GridView 控制項如何讓輕鬆地編輯及刪除文字資料。 在本教學課程，我們看到如何 GridView 控制項也可以編輯和刪除該二進位資料是儲存在資料庫或儲存在檔案系統中的二進位資料。


## <a name="introduction"></a>簡介

過去三個教學課程透過我們已新增許多程式使用二進位資料的功能。 我們新增的開始使用`BrochurePath`欄`Categories`資料表，並據以更新架構。 我們也加入資料存取層和商務邏輯層方法，來使用現有類別目錄資料表的`Picture`保存的映像檔的二進位內容 s 資料行。 我們已經建立網頁中顯示的類別目錄 s 圖片呈現 GridView 中的二進位資料冊的下載連結`<img>`項目並已新增的 DetailsView 允許使用者加入新的類別目錄和上傳其冊視訊和圖片的資料。

剩下要實作所有的就是編輯和刪除現有的類別，我們將會完成在本教學課程使用 GridView s 內建編輯和刪除功能的能力。 當編輯類別目錄時，使用者可以選擇性地將上傳新的圖片，或繼續使用現有的類別。 如冊，他們可以選擇使用現有的冊上, 傳新的冊，或指示類別目錄不再具有與其相關聯冊。 Let s 開始 ！

## <a name="step-1-updating-the-data-access-layer"></a>步驟 1： 更新資料存取層

DAL 已自動產生`Insert`， `Update`，和`Delete`方法，但這些方法所產生根據`CategoriesTableAdapter`s 主查詢，不包括`Picture`資料行。 因此，`Insert`和`Update`方法未包含的參數來指定為類別的圖片的二進位資料。 像我們在[前述教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)，我們必須建立新的 TableAdapter 方法，來更新`Categories`資料表時指定的二進位資料。

開啟 輸入資料集，並從設計工具中，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後選擇 加入查詢從內容功能表來 launche TableAdapter 查詢組態精靈。 此精靈會啟動要求我們 TableAdapter 查詢應該如何存取資料庫。 選擇 使用 SQL 陳述式，然後按一下 下一步。 下一個步驟會產生提示的查詢類型。 因為我們重新建立查詢以新增新記錄以`Categories`資料表，選擇 更新，按一下 下一步。


[![選取 [更新] 選項](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**圖 1**： 選取 [更新] 選項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


我們現在需要指定`UPDATE`SQL 陳述式。 精靈會自動建議`UPDATE`對應至 TableAdapter s 主查詢的陳述式 (更新的一個`CategoryName`， `Description`，和`BrochurePath`值)。 變更陳述式，讓`Picture`資料行是否包含連同`@Picture`參數，就像這樣：


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

在精靈的最後一個畫面會詢問您要將新的 TableAdapter 方法。 輸入`UpdateWithPicture`按一下 [完成]。


[![新的 TableAdapter 方法 UpdateWithPicture 名稱](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**圖 2**： 將新的 TableAdapter 方法`UpdateWithPicture`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>步驟 2： 加入商務邏輯層方法

除了更新 DAL，我們需要更新 BLL 以包含用於更新及刪除類別目錄的方法。 這些是從展示層將會叫用的方法。

刪除類別目錄，我們可以使用`CategoriesTableAdapter`自動產生的 s`Delete`方法。 將下列方法加入`CategoriesBLL`類別：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

本教學課程，讓 s 建立更新類別目錄-預期圖片二進位資料，會叫用的兩個方法`UpdateWithPicture`我們剛加入的方法`CategoriesTableAdapter`，而另一個可接受只`CategoryName`， `Description`，和`BrochurePath`值，並使用`CategoriesTableAdapter`類別自動產生的 s`Update`陳述式。 使用兩種方式背後的原理是，在某些情況下，使用者可能想要更新分類的圖片，以及其其他欄位，在其中情況下使用者必須上傳新的圖片。 上載的圖片 s 二進位資料可再用於`UPDATE`陳述式。 在其他情況下，使用者可能只感興趣更新，請說出、 名稱和描述。 但是如果`UPDATE`陳述式必須要有二進位資料`Picture`資料行，則我們 d 需要提供相關資訊。 這會需要另外連至資料庫以圖片資料回復正在編輯的記錄。 因此，我們希望這兩個`UPDATE`方法。 商務邏輯層會決定要使用哪一個依據更新分類時，是否提供圖片資料。

若要達成此目的，將加入兩種方法可以`CategoriesBLL`類別，這兩名為`UpdateCategory`。 第一個應該接受三個`String`s，`Byte`陣列和`Integer`做為輸入參數、 第二、 三`String`s 和`Integer`。 `String`輸入的參數都是為類別 s 的名稱、 描述和冊檔案路徑，`Byte`陣列為類別的圖片的二進位內容和`Integer`識別`CategoryID`来更新的記錄。 請注意，第一個多載會叫用第二個如果傳入的`Byte`陣列是`Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>步驟 3： 複製 Insert 和檢視功能

在[前述教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)我們建立了名為的頁面`UploadInDetailsView.aspx`，列出在 GridView 中的所有類別目錄，並提供將新的類別目錄新增至系統的 DetailsView。 在本教學課程中，我們將延伸包含編輯和刪除支援 GridView。 而不是持續從`UploadInDetailsView.aspx`，讓 s 改為放在此教學課程的變更`UpdatingAndDeleting.aspx`相同資料夾中的頁面`~/BinaryData`。 複製並貼上的宣告式標記，然後從程式碼`UploadInDetailsView.aspx`至`UpdatingAndDeleting.aspx`。

先開啟`UploadInDetailsView.aspx`頁面。 複製的所有宣告式語法中`<asp:Content>`項目，如圖 3 所示。 接下來，開啟`UpdatingAndDeleting.aspx`並貼上此標記內其`<asp:Content>`項目。 同樣地，程式碼複製`UploadInDetailsView.aspx`頁面 s 程式碼後置類別`UpdatingAndDeleting.aspx`。


[![宣告式標記複製 UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**圖 3**： 複製從宣告式標記`UploadInDetailsView.aspx`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


複製後的宣告式標記和程式碼，請瀏覽`UpdatingAndDeleting.aspx`。 您應該會看到相同的輸出和有相同的使用者經驗與`UploadInDetailsView.aspx`頁面上，從上一個教學課程。

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>步驟 4： 加入刪除 ObjectDataSource 和 GridView 的支援

如我們所述回[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，GridView 提供內建的刪除功能，而且可以在核取方塊的刻度啟用這些功能，如果格線 s 基礎資料來源支援刪除。 目前 ObjectDataSource GridView 繫結至 (`CategoriesDataSource`) 不支援刪除。

若要補救這種情況，按一下從 ObjectDataSource s 智慧標籤的設定資料來源選項來啟動精靈。 第一個畫面會顯示確認 ObjectDataSource 已設定為使用`CategoriesBLL`類別。 按 [下一步]。 目前，只有 ObjectDataSource s`InsertMethod`和`SelectMethod`指定屬性。 不過，精靈自動填入下拉式清單，以更新和刪除索引標籤中的`UpdateCategory`和`DeleteCategory`方法，分別。 這是因為在`CategoriesBLL`我們標示為使用這些方法的類別`DataObjectMethodAttribute`為更新和刪除的預設方法。

現在，設定為 （無） 的更新 s 索引標籤下拉式清單，但保留設為刪除的索引標籤的下拉式清單`DeleteCategory`。 我們會返回此精靈，在步驟 6 來新增更新的支援。


[![設定為使用 DeleteCategory 方法 ObjectDataSource](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**圖 4**： 設定要使用 ObjectDataSource`DeleteCategory`方法 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> 正在完成精靈，在 Visual Studio 可能會要求是否您想要重新整理欄位和索引鍵，其會重新產生資料 Web 控制項欄位。 選擇 否，因為選擇 是 將會覆寫您所做的任何自訂欄位。


ObjectDataSource 現在包含的值及其`DeleteMethod`屬性以及`DeleteParameter`。 請記得使用精靈來指定方法，當 Visual Studio 設定 ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`，這會造成問題更新和刪除方法引動過程。 因此，完全清除這個屬性，或其重設為預設值， `{0}`。 如果您需要重新整理您在這個 ObjectDataSource 屬性上的記憶體，請參閱[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程。

正在完成精靈並修正後`OldValuesParameterFormatString`，ObjectDataSource s 宣告式標記看起來應該類似如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

設定後 ObjectDataSource，將刪除的功能加入至 GridView，藉由檢查 GridView s 智慧標籤啟用刪除核取方塊。 這將會加入 GridView CommandField 其`ShowDeleteButton`屬性設定為`True`。


[![刪除在 GridView 啟用支援](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**圖 5**： 刪除在 GridView 啟用支援 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


請花一點時間測試的刪除功能。 沒有外部索引鍵之間`Products`資料表 s`CategoryID`和`Categories`資料表的`CategoryID`，因此如果您嘗試刪除任何前八個類別，就會收到 foreign key 條件約束違規的例外狀況。 若要測試這項功能時，新增新的類別，提供冊和圖片。 圖 6 所示我測試類別包含名為測試冊檔`Test.pdf`和測試圖片。 圖 7 顯示 GridView 之後已加入測試分類。


[![加入測試分類與冊和映像](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**圖 6**： 加入測試分類與冊和映像 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![在插入之後測試分類，它會顯示在 GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**圖 7**： 插入測試分類，它會顯示在 GridView ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


在 Visual Studio 中，重新整理 [方案總管]。 您現在應該會看到新的檔案中`~/Brochures`資料夾， `Test.pdf` （請參閱圖 8）。

接下來，按一下 [刪除] 連結，在測試分類的資料列，造成頁面回傳而`CategoriesBLL`類別的`DeleteCategory`引發的方法。 這將會叫用 DAL s`Delete`方法，導致適當`DELETE`傳送至資料庫的陳述式。 資料然後重新繫結至 GridView，標記會傳送回用戶端使用測試分類不再存在。

同時刪除工作流程已成功移除從測試分類記錄`Categories`資料表，它並未從 web 伺服器檔案系統中移除其冊檔案。 重新整理 [方案總管] 中，您會看到`Test.pdf`仍然位於`~/Brochures`資料夾。


![Test.pdf 檔案並未從 Web 伺服器的檔案系統刪除](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**圖 8**:`Test.pdf`檔案並未從 Web 伺服器檔案系統刪除


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>步驟 5： 移除已刪除的分類的冊檔

儲存在資料庫外部的二進位資料的缺點是必須採取額外的步驟來刪除相關聯的資料庫記錄時，清除這些檔案。 GridView 和 ObjectDataSource 提供事件引發之前和之後執行 delete 命令。 實際上，我們需要建立前置和後置動作事件的事件處理常式。 之前`Categories`刪除記錄我們必須判定其 PDF 檔的路徑，但是我們不想要刪除 PDF，以防沒有某些例外狀況，而且不會刪除類別目錄刪除類別目錄之前。

GridView s [ `RowDeleting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)引發已叫用 ObjectDataSource s delete 命令之前，而其[`RowDeleted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx)之後引發。 建立使用下列程式碼這兩個事件的事件處理常式：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

在`RowDeleting`事件處理常式，`CategoryID`的資料列在從 GridView s 捕捉正在刪除`DataKeys`集合，可以透過此事件處理常式中存取`e.Keys`集合。 下一步`CategoriesBLL`類別的`GetCategoryByCategoryID(categoryID)`會叫用來傳回所刪除的記錄的相關資訊。 如果傳回`CategoriesDataRow`物件具有非`NULL``BrochurePath`值，則它會儲存在頁面變數`deletedCategorysPdfPath`，讓檔案可以刪除在`RowDeleted`事件處理常式。

> [!NOTE]
> 而不是擷取`BrochurePath`詳細資料`Categories`記錄中正在刪除`RowDeleting`事件處理常式中，我們可以另外加入`BrochurePath`GridView s`DataKeyNames`屬性和存取記錄的值透過`e.Keys`集合。 這樣會稍微增加 GridView 的檢視狀態大小，但會減少所需的程式碼數量以及儲存至資料庫的路線。


已叫用 s 基礎的 delete 命令，ObjectDataSource GridView s 之後`RowDeleted`事件處理常式引發。 如果沒有在刪除資料的例外狀況和沒有值`deletedCategorysPdfPath`，則會從檔案系統中刪除 PDF。 請注意此額外的程式碼時不需要它的相片與相關聯的類別目錄的 s 二進位資料進行清除。 S 由於圖片資料直接在資料庫中儲存，因此刪除`Categories`資料列也會刪除該類別的圖片資料。

在新增之後的兩個事件處理常式，請再次執行此測試案例。 當刪除類別目錄，也會刪除其相關聯的 PDF。

更新現有記錄 s 相關聯的二進位資料會提供一些有趣的挑戰。 本教學課程的其餘部分會探究加入冊及圖片的更新功能。 步驟 6-探索更新步驟 7 會查看更新的圖片及冊資訊的技術。

## <a name="step-6-updating-a-category-s-brochure"></a>步驟 6： 更新分類的冊

如概觀的插入、 更新和刪除資料 」 教學課程中所討論，GridView 會提供內建資料列層級編輯支援可以由刻度核取方塊的實作，如果已適當地設定其基礎資料來源。 目前， `CategoriesDataSource` ObjectDataSource 不包含更新的支援，以便讓 s 新增，在尚未設定。

按一下 設定資料來源連結從 ObjectDataSource 的精靈並繼續進行第二個步驟。 因為`DataObjectMethodAttribute`用於`CategoriesBLL`，更新下拉式清單應該會自動填入`UpdateCategory`接受四個輸入參數的多載 (所有資料行，但`Picture`)。 使其以五個參數使用的多載，可以變更此選項。


[![設定為使用包含參數的圖片 UpdateCategory 方法 ObjectDataSource](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**圖 9**： 設定要使用 ObjectDataSource`UpdateCategory`包含參數的方法`Picture`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource 現在包含的值及其`UpdateMethod`屬性以及對應`UpdateParameter`s。 如步驟 4 中所述，Visual Studio 會將設定 ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`使用設定資料來源精靈 時。 這將導致問題的更新，並刪除方法引動過程。 因此，完全清除這個屬性，或其重設為預設值， `{0}`。

正在完成精靈並修正後`OldValuesParameterFormatString`，ObjectDataSource s 宣告式標記應該如下所示：


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

若要開啟的 GridView s 內建編輯功能，請檢查 GridView s 智慧標籤的 啟用編輯選項。 這會將 CommandField s`ShowEditButton`屬性`True`，並產生編輯按鈕 （和更新 和 取消 5d; 按鈕正在編輯的資料列） 的加法。


[![設定支援編輯 GridView](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**圖 10**： 設定來支援編輯 GridView ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


瀏覽的頁面，透過瀏覽器，然後按一下其中一個資料列的編輯按鈕。 `CategoryName`和`Description`BoundFields 會轉譯為文字方塊。 `BrochurePath` TemplateField 缺少`EditItemTemplate`，因此它會繼續顯示其`ItemTemplate`冊的連結。 `Picture` ImageField 會轉譯成文字方塊的`Text`ImageField s 的值指派給屬性`DataImageUrlField`值，在此情況下`CategoryID`。


[![在 GridView 的 BrochurePath 缺少編輯介面](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**圖 11**: GridView 缺少編輯介面`BrochurePath`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>自訂`BrochurePath`s 編輯介面

我們要建立的編輯介面`BrochurePath`TemplateField，可讓任何使用者的其中一個：

- 將保留做為類別的冊-，
- 更新類別目錄的冊上傳新的冊，或
- 完全移除分類的冊 （在類別目錄不再具有相關聯的冊案例）。

我們也需要更新`Picture`ImageField s 編輯介面，但我們會開始將此步驟 7 中。

從 GridView s 智慧標籤，請按一下 [編輯樣板] 連結，然後選取`BrochurePath`TemplateField 的`EditItemTemplate`從下拉式清單。 RadioButtonList Web 控制項加入此範本中，設定其`ID`屬性`BrochureOptions`及其`AutoPostBack`屬性`True`。 從 [屬性] 視窗中，按一下省略符號在`Items`屬性，將會出現`ListItem`集合編輯器。 新增具有下列三個選項`Value`的 1、 2 和 3，分別：

- 使用目前冊
- 移除目前冊
- 上傳新冊

設定第一個`ListItem`s`Selected`屬性`True`。


![加入 RadioButtonList 三個 ListItems](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**圖 12**： 將三個`ListItem`RadioButtonList 的 s


RadioButtonList 下方加入名為的檔案上傳控制項`BrochureUpload`。 設定其`Visible`屬性`False`。


[![加入 EditItemTemplate RadioButtonList 和檔案上傳控制項](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**圖 13**： 加入 RadioButtonList 和檔案上傳控制項`EditItemTemplate`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


此 RadioButtonList 提供三個選項的使用者。 這個概念是，將最後一個選項，也就是上傳新冊，已選取時，才會顯示檔案上傳控制項。 若要達成此目的，建立事件處理常式 RadioButtonList s`SelectedIndexChanged`事件並加入下列程式碼：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

由於 RadioButtonList 和檔案上傳控制項範本內，我們必須撰寫許多程式碼以程式設計方式存取這些控制項。 `SelectedIndexChanged`事件處理常式中 RadioButtonList 的參考會傳遞給`sender`輸入的參數。 若要取得的檔案上傳控制項，我們需要取得 RadioButtonList 的父控制項和使用`FindControl("controlID")`從該處的方法。 一旦 RadioButtonList 和檔案上傳控制項的參考，檔案上傳控制 s`Visible`屬性設定為`True`才 RadioButtonList s`SelectedValue`等於 3，這是`Value`的上傳新冊`ListItem`.

此位置的程式碼，請花一點時間測試編輯介面。 按一下 [編輯] 按鈕的資料列。 一開始，您應選取 [使用目前冊] 選項。 變更選取的索引，會導致回傳。 如果選取第三個選項時，會顯示檔案上傳控制項，否則它會隱藏。 圖 14 顯示編輯介面時，第一次按一下 [編輯] 按鈕。選取上傳新冊選項之後，圖 15 會顯示介面。


[![一開始，使用目前冊選項](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**圖 14**： 一開始，選取選項是使用目前冊 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![選擇上傳新冊選項顯示檔案上傳控制項](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**圖 15**： 選擇上傳新冊選項顯示檔案上傳控制項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>儲存冊檔案並更新`BrochurePath`資料行

按一下 [GridView 的更新] 按鈕時，其`RowUpdating`事件引發。 叫用 s update 命令 ObjectDataSource 然後 GridView 的`RowUpdated`事件引發。 刪除工作流程中，我們必須建立這些事件的兩個事件處理常式。 在`RowUpdating`事件處理常式中，我們需要決定要採取的動作將會根據`SelectedValue`的`BrochureOptions`RadioButtonList:

- 如果`SelectedValue`為 1，我們想要繼續使用相同`BrochurePath`設定。 因此，我們需要設定 ObjectDataSource s`brochurePath`至現有的參數`BrochurePath`正在更新記錄的值。 ObjectDataSource s`brochurePath`參數可以使用設定`e.NewValues["brochurePath"] = value`。
- 如果`SelectedValue`為 2，則我們想要設定記錄 s`BrochurePath`值設定為`NULL`。 這可藉由設定 ObjectDataSource s`brochurePath`參數`Nothing`，這會導致資料庫`NULL`用於`UPDATE`陳述式。 如果沒有現有的冊檔案被移除，就必須刪除現有的檔案。 不過，我們只想要執行這項操作，如果更新完成，而不會引發例外狀況。
- 如果`SelectedValue`為 3，我們會想要確保使用者具有上傳的 PDF 檔案，然後將它儲存到檔案系統，並更新記錄的`BrochurePath`資料行值。 此外，如果沒有現有的冊檔案已被取代，我們需要刪除先前的檔案。 不過，我們只想要執行這項操作，如果更新完成，而不會引發例外狀況。

當完成所需的步驟 RadioButtonList s`SelectedValue`是 3 幾乎是相同的所使用的 DetailsView 的`ItemInserting`事件處理常式。 這個事件處理常式執行時從我們在加入 DetailsView 控制項加入新的類別目錄記錄[上一個教學課程](including-a-file-upload-option-when-adding-a-new-record-vb.md)。 因此，它 behooves 我們重構出此功能分成不同的方法。 特別是，我移出的通用功能分成兩個方法：

- `ProcessBrochureUpload(FileUpload, out bool)` 接受做為輸入檔案上傳控制項執行個體和輸出布林值，指定是否刪除或編輯作業應該繼續進行，或如果應該取消發生某個驗證錯誤。 這個方法會傳回儲存的檔案路徑或`null`如果沒有檔案已儲存。
- `DeleteRememberedBrochurePath` 刪除的頁面變數中的路徑所指定的檔案`deletedCategorysPdfPath`如果`deletedCategorysPdfPath`不`null`。

這兩種方法的程式碼後面。 請注意之間的相似度`ProcessBrochureUpload`和 DetailsView 的`ItemInserting`從上一個教學課程的事件處理常式。 在本教學課程中，我已更新 DetailsView 的事件處理常式，才能使用這些新方法。 下載此教學課程，請參閱 DetailsView 的事件處理常式的修改相關聯的程式碼。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

GridView s`RowUpdating`和`RowUpdated`事件處理常式使用`ProcessBrochureUpload`和`DeleteRememberedBrochurePath`方法，如下列程式碼所示：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

請注意如何`RowUpdating`事件處理常式會使用一系列的條件陳述式執行適當的動作，根據`BrochureOptions`RadioButtonList 的`SelectedValue`屬性值。

此位置的程式碼，您可以編輯類別目錄，讓它使用其目前冊、 使用沒有冊，或上傳新的。 請繼續並現在就試試看。在中設定中斷點`RowUpdating`和`RowUpdated`具體的工作流程的事件處理常式。

## <a name="step-7-uploading-a-new-picture"></a>步驟 7： 上傳新的圖片

`Picture` ImageField s 作為值填入文字方塊中編輯介面呈現其`DataImageUrlField`屬性。 在編輯的工作流程期間 GridView 將參數傳遞至參數 s 的名稱與 ObjectDataSource ImageField 的值`DataImageUrlField`屬性和參數值的文字方塊中編輯介面中輸入的值。 映像儲存為檔案系統上的檔案時，此行為是適合和`DataImageUrlField`包含映像的完整 URL。 這類情況下，以編輯介面會顯示在文字方塊中，且使用者可以變更儲存回資料庫影像的 URL。 授與，此預設介面不允許使用者上傳新的映像，但是會讓它們將之影像的 URL 與目前的值變更為另一個。 此教學課程中，不過，ImageField 的預設的編輯介面不敷使用因為`Picture`直接在資料庫中儲存二進位資料而`DataImageUrlField`屬性會保存只`CategoryID`。

若要進一步了解當使用者編輯 ImageField 具有的資料列時，在我們的教學課程中會發生什麼事，請考慮下列範例： 使用者編輯包含的資料列`CategoryID`10，造成`Picture`ImageField 呈現文字方塊值為 10。 假設使用者在此文字方塊中的值變更為 50，並按一下 [更新] 按鈕。 回傳和 GridView 一開始會建立名為`CategoryID`值 50。 不過，GridView 會傳送此參數之前 (和`CategoryName`和`Description`參數)，它會在中的值加入`DataKeys`集合。 因此，它會覆寫`CategoryID`參數與目前的資料列 s 基礎`CategoryID`值 10。 簡單地說，編輯介面 ImageField s 會有任何作用的編輯工作流程在此教學課程因為 ImageField 的名稱`DataImageUrlField`屬性和格線的`DataKey`值是在相同的其中一種。

雖然 ImageField 並輕鬆顯示資料庫的資料為基礎的映像，我們不想要提供的文字方塊中編輯介面。 相反地，我們想要提供使用者可用來變更類別的圖片的檔案上傳控制項。 不同於`BrochurePath`值，這些教學課程中我們 ve 決定需要每個類別都必須有一張圖片。 因此，我們不讓使用者指示沒有任何相關聯的圖片使用者可能會將.vhd 檔案新增圖片，或保留為目前的圖片 t 需要-是。

若要自訂 ImageField s 編輯介面，我們需要將其轉換為 TemplateField。 從 GridView s 智慧標籤，按一下 編輯資料行連結、 選取 ImageField，然後按一下轉換此欄位為 TemplateField 連結。


![ImageField 轉換為 TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**圖 16**: ImageField 轉換為 TemplateField


ImageField 轉換為 TemplateField 以這種方式，就會產生兩個範本具有為 TemplateField。 如下列宣告式語法所示，`ItemTemplate`包含映像 Web 控制項`ImageUrl`屬性會被指派使用 ImageField s 為基礎的資料繫結語法`DataImageUrlField`和`DataImageUrlFormatString`屬性。 `EditItemTemplate`包含文字方塊的`Text`屬性繫結至所指定的值`DataImageUrlField`屬性。


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

我們需要更新`EditItemTemplate`使用檔案上傳控制項。 從 GridView s 智慧標籤按一下 編輯樣板連結，然後選取  `Picture` TemplateField 的`EditItemTemplate`從下拉式清單。 在範本中應該會看到移除此文字方塊。 接下來，將檔案上傳控制項從 [工具箱] 拖曳放入範本，並設定其`ID`至`PictureUpload`。 也將文字變更的類別目錄的圖片，請指定新的圖片。 要保留相同類別的圖片，請將欄位保留空白範本，以及。


[![將檔案上傳控制項加入 EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**圖 17**： 將檔案上傳控制項加入`EditItemTemplate`([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


自訂之後編輯介面，請在瀏覽器中檢視進度。 當資料列在唯讀模式下，檢視類別的影像會顯示前，但是按一下 [編輯] 按鈕會呈現圖片資料行以包含檔案上傳控制項的文字。


[![編輯介面包含檔案上傳控制項](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**圖 18**： 編輯介面包含檔案上傳控制項 ([按一下以檢視完整大小的影像](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


前文提過 ObjectDataSource 設定成呼叫`CategoriesBLL`類別 s`UpdateCategory`接受做為輸入圖片的二進位資料的方法`Byte`陣列。 如果此陣列是`Nothing`，不過替代`UpdateCategory`多載呼叫時，哪些問題`UPDATE`SQL 陳述式不會修改`Picture`資料行，使句子類別 s 目前圖片保持不變。 因此，在 GridView s`RowUpdating`我們要以程式設計方式參考的事件處理常式`PictureUpload`檔案上傳控制項，並決定 檔案已上傳。 如果其中一個未上傳，則我們*不*想要指定的值`picture`參數。 另一方面，如果檔案已上傳中`PictureUpload`檔案上傳控制項，我們想要確保它是 JPG 檔案。 如果是，則我們可以將其二進位內容傳送到透過 ObjectDataSource`picture`參數。

步驟 6 中使用的程式碼，大部分已經這裡需要的程式碼中是否存在具有 DetailsView s 像`ItemInserting`事件處理常式。 因此，我已重構為新方法的通用功能`ValidPictureUpload`，並更新`ItemInserting`事件處理常式來使用這個方法。

下列程式碼加入開頭的 GridView 的`RowUpdating`事件處理常式。 它將冊儲存至 web 伺服器檔案系統，如果無效的圖片檔案上傳要 s 重要，這段程式碼前面的程式碼，因為我們不會將冊檔案儲存。


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

`ValidPictureUpload(FileUpload)`方法會將檔案上傳控制項做為其唯一的輸入參數中，並檢查已上傳的檔案 s 延伸模組，以確保上傳的檔案是 JPG; 它只會呼叫如果上載圖片檔案。 如果已上不傳任何檔案，然後圖片參數未設定，因此會使用其預設值`Nothing`。 如果圖片已上傳和`ValidPictureUpload`傳回`True`、`picture`參數會指派給已上傳的映像; 的二進位資料，如果該方法會傳回`False`、 已取消更新工作流程和事件處理常式已結束。

`ValidPictureUpload(FileUpload)`方法程式碼重構從 DetailsView 的`ItemInserting`事件處理常式，會遵循：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>步驟 8: JPGs 以取代原始的分類圖片

請記得原始八個類別目錄圖片會包裝 OLE 標頭中的點陣圖檔案。 既然我們已加入的功能來編輯現有的記錄的圖片，請花一點時間以 JPGs 取代這些點陣圖。 如果您想要繼續使用目前的類別目錄圖片，您可以將它們轉換成 JPGs 藉由執行下列步驟：

1. 將點陣圖影像儲存到硬碟機。 請瀏覽`UpdatingAndDeleting.aspx`瀏覽器中前, 八個類別的每個頁面上，映像上按一下滑鼠右鍵，然後選擇要儲存圖片。
2. 在您選擇的映像編輯器中開啟影像。 您可以使用 Microsoft 小畫家 中，例如。
3. 將點陣圖儲存為 JPG 影像。
4. 更新類別目錄的圖片透過編輯介面，並使用該 JPG 檔案。

之後編輯類別目錄和上傳的 JPG 影像，影像將不會呈現在瀏覽器中因為`DisplayCategoryPicture.aspx`頁面條狀配置的圖片前, 八個類別中的第一個 78 位元組。 藉由移除 OLE 標頭條狀配置所執行的程式碼，修正此問題。 如此一來後,`DisplayCategoryPicture.aspx``Page_Load`事件處理常式應該有單純的下列程式碼：


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> `UpdatingAndDeleting.aspx`頁面插入和編輯介面無法使用一些工作。 `CategoryName`和`Description`BoundFields DetailsView 和 GridView 中的應該轉換成 TemplateFields。 因為`CategoryName`不允許`NULL`值，應該加入 RequiredFieldValidator。 和`Description`文字方塊中應該可能會被轉換為多行文字方塊。 我將保留這些修飾做為練習您。


## <a name="summary"></a>總結

本教學課程完成我們查看使用二進位資料。 在本教學課程和先前的三個，我們可了解如何二進位資料可以儲存在檔案系統上，或直接在資料庫內。 使用者從他們的硬碟中選取檔案，並將它上傳至 web 伺服器，是儲存在檔案系統上或插入至資料庫提供系統的二進位資料。 ASP.NET 2.0 包含檔案上傳控制項，可提供簡單，拖曳和卸除這類的介面。 不過，如中所述[上傳檔案](uploading-files-vb.md)教學課程中，檔案上傳控制項只是適合用於相對較小的檔案上傳，理想的情況下不會超過的 mb 數閾值。 我們也可以探索如何將上傳的資料與基礎資料模型，產生關聯，以及如何編輯和刪除的二進位資料，從現有的記錄。

我們的教學課程的下一組探索各種快取技術。 快取會提供方法來改善應用程式 s 耗費資源的作業的結果，以及將其儲存在可以更快速地存取的位置中的整體效能。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一步](including-a-file-upload-option-when-adding-a-new-record-vb.md)
