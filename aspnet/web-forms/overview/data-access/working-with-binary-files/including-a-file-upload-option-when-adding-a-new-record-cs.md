---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: 其中包括的檔案上傳選項，當加入新的記錄 (C#) |Microsoft Docs
author: rick-anderson
description: 本教學課程會示範如何建立的 Web 介面，可讓使用者輸入的文字資料，並將二進位檔案上傳。 為了說明選項可用 t...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 5cc1db20a724c8a060e978e2360b977fb16f1e0c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842377"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>包含檔案上傳選項，當加入新的記錄 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe)或[下載 PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> 本教學課程會示範如何建立的 Web 介面，可讓使用者輸入的文字資料，並將二進位檔案上傳。 為了說明可用來儲存二進位資料的選項，一個檔案會儲存在資料庫中而其他則會儲存在檔案系統中。


## <a name="introduction"></a>簡介

先前的兩個教學課程中，我們探討儲存應用程式的資料模型，查看如何從用戶端的檔案傳送到 web 伺服器，並了解如何在資料 W 呈現這些二進位資料使用 FileUpload 控制項相關聯的二進位資料的技術eb 控制項。 我們尚未以了解如何上傳的資料關聯的資料模型，但發生。

在本教學課程中，我們將建立網頁，以新增新的類別。 除了類別目錄的名稱和描述文字方塊中，此頁面將會需要包含兩個 FileUpload 控制項另一個用於新的類別目錄的圖片，另一個用於摺頁冊。 上傳的圖片會直接儲存在新的記錄 s`Picture`資料行，而摺頁冊會儲存到`~/Brochures`以儲存新記錄 s 中的檔案路徑的資料夾`BrochurePath`資料行。

建立這個新的 web 網頁之前, 我們將需要更新架構。 `CategoriesTableAdapter` s 主查詢不會擷取`Picture`資料行。 因此，自動產生`Insert`方法只會有輸入`CategoryName`， `Description`，和`BrochurePath`欄位。 因此，我們需要建立另一種方法中會提示您輸入所有的四個 TableAdapter`Categories`欄位。 `CategoriesBLL`商業邏輯層中的類別也必須更新。

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>步驟 1： 加入`InsertWithPicture`方法`CategoriesTableAdapter`

我們在建立時`CategoriesTableAdapter`年代[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，我們將其設定為自動產生`INSERT`， `UPDATE`，和`DELETE`主要查詢為基礎的陳述式。 此外，我們指示採用 DB 直接的方法中，建立方法 TableAdapter `Insert`， `Update`，和`Delete`。 這些方法可讓您執行自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式，因此，接受 根據主查詢所傳回的資料行的輸入的參數。 在[上傳的檔案](uploading-files-cs.md)教學課程中我們增加`CategoriesTableAdapter`s 若要使用的主查詢`BrochurePath`資料行。

由於`CategoriesTableAdapter`s 主查詢不會參考`Picture`資料行中，我們將無法新增新的記錄，或使用的值更新現有的記錄`Picture`資料行。 若要擷取這項資訊，我們可以建立新的方法中會特別用來插入記錄包含二進位資料的 TableAdapter，或者我們可以自訂自動產生`INSERT`陳述式。 自訂自動產生的問題`INSERT`陳述式時，我們可能會產生精靈所覆寫我們自訂項目。 例如，假設我們自訂`INSERT`陳述式，包括使用`Picture`資料行。 這會更新 tableadapter`Insert`方法以包含額外的輸入的參數為類別的圖片 s 二進位資料。 我們接著可以建立方法中使用這個 DAL 方法，並透過叫用此 BLL 方法展示層、 商務邏輯層和人員，一切都會正常。 亦即，直到下一次設定透過 [TableAdapter 組態精靈] 的 TableAdapter。 當精靈完成時，我們的自訂`INSERT`陳述式會覆寫，`Insert`方法就會還原成原來的形式，並不會再進行編譯程式碼 ！

> [!NOTE]
> 使用預存程序，而不特定 SQL 陳述式時的不便會是問題。 未來的教學課程將探討在資料存取層中使用替代特定 SQL 陳述式的預存程序。


若要避免此潛在傷腦筋，而不是自訂的自動產生 SQL 陳述式，可讓 s 改為建立 TableAdapter 的新的方法。 這種方法，名為`InsertWithPicture`，將接受的值`CategoryName`， `Description`， `BrochurePath`，並`Picture`資料行，然後執行`INSERT`陳述式來儲存所有的四個值的新記錄。

開啟 輸入資料集，並從設計工具中，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後從內容功能表中選擇 加入查詢。 這會啟動 TableAdapter 查詢組態精靈，一開始會詢問 TableAdapter 查詢應該如何存取資料庫。 選擇 使用 SQL 陳述式，然後按一下 下一步。 下一個步驟會產生提示的查詢類型。 因為我們重新建立要新增新的記錄，以查詢`Categories`資料表中，選擇 [插入] 並按一下 [下一步]。


[![選取 [插入] 選項](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**圖 1**： 選取 [插入] 選項 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


我們現在需要指定`INSERT`SQL 陳述式。 精靈會自動建議`INSERT`對應至 TableAdapter s 主查詢的陳述式。 在此情況下，它 s`INSERT`陳述式來插入`CategoryName`， `Description`，和`BrochurePath`值。 Update 陳述式以便`Picture`資料行是否包含連同`@Picture`參數，就像這樣：


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

在精靈的最後一個畫面會要求我們命名新的 TableAdapter 方法。 輸入`InsertWithPicture`按一下 [完成]。


[![命名新的 TableAdapter 方法 InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**圖 2**： 將新的 TableAdapter 方法命名`InsertWithPicture`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>步驟 2： 更新商務邏輯層

展示層應該只有介面與商務邏輯層，而不是我們要建立 BLL 方法叫用我們剛剛建立的 DAL 方法略過它，直接移至資料存取層，因為 (`InsertWithPicture`)。 本教學課程中建立方法`CategoriesBLL`名為類別`InsertWithPicture`它會接受作為輸入的三`string`s 和`byte`陣列。 `string`輸入的參數都是為類別 s 的名稱、 描述和冊檔案路徑，而`byte`陣列為類別的圖片的二進位內容。 如下列程式碼所示，這個 BLL 方法會叫用對應的 DAL 方法：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> 請確定您已儲存輸入資料集之前加入`InsertWithPicture`BLL 的方法。 由於`CategoriesTableAdapter`類別的程式碼會自動產生型別資料集時，如果您不要先將您的變更儲存至具類型資料集`Adapter`贏得 t 屬性知道`InsertWithPicture`方法。


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>步驟 3： 列出現有的類別和其二進位資料

我們將在本教學課程建立的頁面，可讓使用者將新類別新增至系統，提供新的類別目錄的圖片和手冊。 在 [前述教學課程](displaying-binary-data-in-the-data-web-controls-cs.md)我們 GridView ImageField TemplateField 與用來顯示每個類別 s 的名稱，描述、 圖片和下載其手冊的連結。 可讓此教學課程中，建立頁面，同時列出所有現有的類別，並允許建立新的複寫功能的 s。

首先開啟`DisplayOrDownload.aspx`頁面上，從`BinaryData`資料夾。 移至來源檢視，並將複製的 GridView 和 ObjectDataSource s 宣告式語法，將其內貼`<asp:Content>`中的項目`UploadInDetailsView.aspx`。 同時，不要忘了透過複製`GenerateBrochureLink`方法的程式碼後置類別`DisplayOrDownload.aspx`至`UploadInDetailsView.aspx`。


[![複製並貼上 UploadInDetailsView.aspx 從 DisplayOrDownload.aspx 的宣告式語法](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**圖 3**： 複製和貼上從宣告式語法`DisplayOrDownload.aspx`要`UploadInDetailsView.aspx`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


複製的宣告式語法後及`GenerateBrochureLink`方法，透過以`UploadInDetailsView.aspx`頁面上，檢視透過瀏覽器來確認所有項目透過正確複製該頁面。 您應該會看到 GridView 列出的八個類別，其中包含下載手冊，以及類別的圖片的連結。


[![您現在應該會看到每個類別目錄以及其二進位資料](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**圖 4**： 您應該現在請參閱每個類別目錄以及其二進位資料 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>步驟 4： 設定`CategoriesDataSource`來支援插入

`CategoriesDataSource` ObjectDataSource 使用`Categories`GridView 目前不提供讓您插入資料。 若要支援插入到這個資料來源控制項，我們要將對應其`Insert`方法，以在其基礎的物件，方法`CategoriesBLL`。 特別是，我們要將它對應至`CategoriesBLL`方法，您於步驟 2 中，我們新增`InsertWithPicture`。

按一下 [從 ObjectDataSource s 智慧標籤的設定資料來源] 連結啟動。 第一個畫面會顯示的物件的資料來源設定為使用`CategoriesBLL`。 保留此設定設為-定義資料方法畫面前往旁邊按一下。 移至 [插入] 索引標籤，並挑選`InsertWithPicture`方法，從下拉式清單。 按一下 完成 以完成精靈。


[![設定為使用 InsertWithPicture 方法的 ObjectDataSource](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**圖 5**： 設定要使用 ObjectDataSource`InsertWithPicture`方法 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> 在完成精靈，Visual Studio 可能會要求是否您想要重新整理欄位和索引鍵，這會重新產生資料 Web 控制項欄位。 選擇 否，因為選擇 是 會覆寫您所做的任何欄位自訂項目。


完成精靈之後，ObjectDataSource 現在將包含的值及其`InsertMethod`屬性，以及`InsertParameters`四個類別資料行中，下列宣告式標記為說明：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>步驟 5： 建立插入介面

First 涵蓋[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)，DetailsView 控制項提供內建的插入介面支援插入資料來源控制項所使用時可加以利用。 可讓 s DetailsView 控制項加入至這個網頁上方 GridView，將會永久會呈現其插入的介面，讓使用者快速新增新的類別。 在 DetailsView 中新的類別，其下的 GridView 會自動重新整理，並顯示新的類別。

從 [工具箱] 拖曳至上方 GridView，設定設計工具拖曳 DetailsView 開始其`ID`屬性，以`NewCategory`並清除`Height`和`Width`屬性值。 從 DetailsView s 智慧標籤，將它繫結至現有`CategoriesDataSource`然後核取 啟用插入核取方塊。


[![結合 CategoriesDataSource DetailsView 和啟用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**圖 6**： 繫結至 DetailsView`CategoriesDataSource`並啟用插入 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


若要永久轉譯 DetailsView 在其插入的介面中，設定其`DefaultMode`屬性設`Insert`。

請注意，DetailsView 具有五個 BoundFields `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，並`BrochurePath`雖然`CategoryID`因為 BoundField 不會插入介面中呈現其`InsertVisible`屬性設定為`false`。 因為它們是所傳回的資料行，存在於這些 BoundFields`GetCategories()`方法，這是什麼 ObjectDataSource 叫用以擷取其資料。 插入此項目，不過，我們不想要讓使用者指定的值`NumberOfProducts`。 此外，我們需要讓使用者上傳新的類別目錄的圖片，以及上傳的摺頁冊的 PDF。

移除`NumberOfProducts`DetailsView 完全與然後更新 BoundField`HeaderText`的屬性`CategoryName`和`BrochurePath`BoundFields 到類別目錄] 和 [摺頁冊，分別。 接下來，將轉換`BrochurePath`BoundField 到 TemplateField 並加入新的 TemplateField 的圖片中，提供這個新的 TemplateField`HeaderText`圖片的值。 移動`Picture`使其之間的 TemplateField `BrochurePath` TemplateField 和 CommandField。


![結合 CategoriesDataSource DetailsView 和啟用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**圖 7**： 繫結至 DetailsView`CategoriesDataSource`並啟用插入


如果您轉換`BrochurePath`成透過 [編輯欄位] 對話方塊中的 TemplateField BoundField TemplateField 包含`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 只有`InsertItemTemplate`是有需要不過，可以隨意移除其他兩個範本。 此時 DetailsView s 的宣告式語法看起來應該如下所示：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>新增 FileUpload 控制項摺頁冊和圖片欄位

目前`BrochurePath`TemplateField s`InsertItemTemplate`包含文字方塊中，雖然`Picture`TemplateField 不包含任何範本。 我們需要更新這些兩個的 TemplateField s`InsertItemTemplate`以使用 FileUpload 控制項。

從 DetailsView s 智慧標籤，選擇 [編輯範本] 選項，然後按`BrochurePath`TemplateField 的`InsertItemTemplate`從下拉式清單。 移除文字方塊，然後將 FileUpload 控制項從 [工具箱] 拖曳至範本。 設定 FileUpload 控制項 s`ID`至`BrochureUpload`。 同樣地，新增 FileUpload 控制項`Picture`TemplateField 的`InsertItemTemplate`。 設定這個 FileUpload 控制項 s`ID`至`PictureUpload`。


[![加入 InsertItemTemplate FileUpload 控制項](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**圖 8**： 新增 FileUpload 控制項`InsertItemTemplate`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


完成之後這些新增項目，會使用兩個的 TemplateField s 宣告式語法：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

當使用者將新的類別時，我們想要確保的手冊和圖片的正確檔案類型。 手冊，使用者必須提供 PDF。 圖中，我們需要使用者上傳影像檔，但我們允許*任何*映像檔案或僅映像檔案的特定型別，例如 Gif 或 Jpg 嗎？ 若要允許不同的檔案類型，d 我們要擴充`Categories`包含此類型可以傳送到用戶端透過擷取的檔案類型的資料行的結構描述`Response.ContentType`在`DisplayCategoryPicture.aspx`。 因為我們不具有這類的資料行，它會限制使用者只提供特定的映像檔案類型容錯度加倍。 `Categories`資料表 s 現有的映像的點陣圖，但 Jpg 是更適當的檔案格式，透過 web 提供的映像。

如果使用者上傳不正確的檔案類型，我們需要取消插入，並顯示訊息，指出此問題。 加入標籤 Web 控制項 DetailsView 下方。 設定其`ID`屬性，以`UploadWarning`，清除出其`Text`屬性，設定`CssClass`屬性為 [警告] 和`Visible`並`EnableViewState`屬性，以`false`。 `Warning` CSS 類別定義於`Styles.css`並呈現大型、 紅色、 斜體、 粗體字型中的文字。

> [!NOTE]
> 在理想情況下，`CategoryName`和`Description`BoundFields 會轉換成 TemplateFields 和自訂其插入介面。 `Description`插入介面，例如，會有可能更適合透過多行文字方塊。 而由於`CategoryName`資料行不接受`NULL`值 RequiredFieldValidator 應新增以確保使用者提供新的類別的名稱的值。 這些步驟會留給您作為練習的讀取器。 回頭[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)的擴充資料修改介面將深入探討。


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>步驟 6： 儲存已上傳的手冊，到 Web 伺服器檔案系統

使用者輸入新的類別目錄的值，然後按一下 [插入] 按鈕，就會發生回傳，而且可以掀起插入工作流程。 首先，在 DetailsView s [ `ItemInserting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx)引發。 接下來，ObjectDataSource s`Insert()`方法會叫用，這會導致新的記錄新增至`Categories`資料表。 在那之後，在 DetailsView s [ `ItemInserted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx)引發。

ObjectDataSource s 前面`Insert()`叫用方法時，就必須先確認使用者已上傳適當的檔案類型，並再儲存到 web 伺服器檔案系統的 手冊為 PDF。 建立事件處理常式 DetailsView s`ItemInserting`事件，並新增下列程式碼：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

事件處理常式一開始會參考`BrochureUpload`FileUpload 控制項 DetailsView 的範本。 然後，如果已上傳摺頁冊，就會檢查上傳的 s 副檔名。 如果不是延伸模組。PDF、 然後警告會顯示，插入取消，而且事件處理常式在執行結束。

> [!NOTE]
> 信賴憑證者上傳的 s 副檔名不是萬無一失的技巧，以確保上傳的檔案的 PDF 文件。 使用者可能會有有效的 PDF 文件副檔名`.Brochure`，或無法取得非 PDF 文件也提供`.pdf`延伸模組。 檔案二進位內容必須以更明確地驗證 檔案類型，就要以程式設計的方式。 這類全面的方法，不過，往往是多餘的;正在檢查擴充功能已足以應付大部分的情況。


中所述[上傳檔案](uploading-files-cs.md)教學課程中，儲存檔案至檔案系統，讓一位使用者的傳不能覆寫另一個 s 時，就必須特別注意。 本教學課程中，我們會嘗試上傳的檔案使用相同的名稱。 如果已存在的檔案中`~/Brochures`目錄中具有該相同的檔案名稱，不過，我們將會附加一個數字結尾，直到找到唯一的名稱。 比方說，如果使用者將名為摺頁冊檔案上傳`Meats.pdf`，但已經有名稱為的檔案`Meats.pdf`中`~/Brochures`資料夾中，我們將會變更已儲存的檔案名稱以`Meats-1.pdf`。 如果已存在時，我們會試著`Meats-2.pdf`，依此類推，直到找到唯一的檔案名稱。

下列程式碼會使用[`File.Exists(path)`方法](https://msdn.microsoft.com/library/system.io.file.exists.aspx)來判斷檔案是否已存在具有指定的檔案名稱。 如果是的話，它會繼續嘗試新的檔案名稱，如摺頁冊，直到找到沒有衝突。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

一旦已找到有效的檔案名稱，必須將檔案儲存至檔案系統和 ObjectDataSource 的`brochurePath``InsertParameter`值需要更新，讓這個檔案名稱寫入資料庫。 如我們所見年代*上傳的檔案*教學課程中，儲存檔案，可以使用 FileUpload 控制項的`SaveAs(path)`方法。 若要更新的 ObjectDataSource 秒`brochurePath`參數，請使用`e.Values`集合。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>步驟 7： 將已上傳的圖片儲存到資料庫

若要儲存在新的已上傳的圖片`Categories`記錄，我們需要將已上傳二進位內容指派到 ObjectDataSource `picture` DetailsView s 中的參數`ItemInserting`事件。 不過，我們進行這項指派之前，我們需要先確定已上傳的圖片是 JPG 與不一些其他的影像類型。 如同步驟 6 中，可讓 s 使用已上傳的圖片的副檔名來確定其型別。

雖然`Categories`資料表允許`NULL`值`Picture`資料行，目前所有類別都有一張圖片。 可讓 s 強制使用者進行時加入新的類別，透過本頁面提供一張圖片。 下列程式碼會檢查以確保已上傳圖片，且具有適當的擴充功能。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

此程式碼應該放*之前*步驟 6 中的程式碼，讓圖片上傳發生問題時，事件處理常式將會終止之前摺頁冊檔案儲存至檔案系統。

假設已上傳適當的檔案，請將上傳的二進位內容指派給圖片參數 s 的值與下列程式碼行：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>完整`ItemInserting`事件處理常式

為求完整起見，以下是`ItemInserting`完整的事件處理常式：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>步驟 8： 修正`DisplayCategoryPicture.aspx`頁面

可讓 s 花一點時間來測試插入介面和`ItemInserting`建立過去幾個步驟的事件處理常式。 請瀏覽`UploadInDetailsView.aspx`逐頁瀏覽器，並嘗試新增的類別，但省略該圖片，或指定非 JPG 圖片或非 PDF 摺頁冊。 在任一種情況下，將會顯示錯誤訊息，並插入工作流程已取消。


[![一則警告訊息會顯示如果上傳的檔案類型無效](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**圖 9**: 警告訊息會顯示如果上傳無效的檔案類型 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


一旦您已確認頁面需要上傳圖片和成交的 t 接受非 PDF 或非 JPG 檔案，新增新的類別，以有效的 JPG 圖片，將摺頁冊欄位保留空白。 頁面會回傳之後按一下 [插入] 按鈕，以及新的記錄將會新增至`Categories`資料表直接在資料庫中儲存的上傳的影像 s 二進位內容。 GridView 會更新，並顯示在新加入的類別中，一個資料列，但是，如 [圖 10] 所示，新的類別目錄的圖片就無法正確呈現。


[![新的類別不會顯示圖片的 s](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**圖 10**： 不會顯示圖片的新類別 s ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


不會顯示新的圖片的原因是因為`DisplayCategoryPicture.aspx`傳回指定的分類的圖片的網頁設定成可以處理具有 OLE 標頭的點陣圖。 從中移除此 78 位元組標頭`Picture`再進行傳送，則資料行 s 二進位內容傳回至用戶端。 但沒有此 OLE 標頭; JPG 檔案我們剛才上傳新的類別目錄。因此，有效的必要位元組正在移除從影像 s 的二進位資料。

因為現在有兩個點陣圖與 OLE 標頭中的 Jpg`Categories`資料表中，我們需要更新`DisplayCategoryPicture.aspx`，使其沒有原始的八個類別目錄移除 OLE 標頭，並會略過較新的類別目錄記錄此移除。 在我們的下一個教學課程中，我們將檢驗如何更新現有的記錄 s 映像，並使其 Jpg 我們將更新的所有舊的分類圖片。 現在，不過，使用中的下列程式碼`DisplayCategoryPicture.aspx`移除 OLE 標頭，只針對這些原始的八個類別：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

透過這項變更，JPG 影像現在才能正確轉譯在 gridview 裡。


[![新的類別目錄的 JPG 影像會正確轉譯](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**圖 11**： 新的類別目錄的 JPG 影像會正確轉譯 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>步驟 9： 刪除摺頁冊面對例外狀況

將二進位資料儲存在 web 伺服器檔案系統上的挑戰之一是會導致資料模型和二進位資料之間的落差。 因此，每當刪除記錄時，對應的二進位資料，在檔案系統上也必須移除。 這可以派上用場時插入，以及。 請考慮下列案例： 使用者加入新的類別，指定有效的圖片和手冊。 按一下 [插入] 按鈕，就會發生回傳和 DetailsView 的`ItemInserting`事件引發時，將摺頁冊儲存至 web 伺服器檔案系統。 接下來，ObjectDataSource s`Insert()`方法會叫用，它會呼叫`CategoriesBLL`類別 s`InsertWithPicture`方法，呼叫`CategoriesTableAdapter`s`InsertWithPicture`方法。

現在，如果資料庫處於離線狀態，會發生什麼事或是否有錯誤`INSERT`SQL 陳述式？ 清楚地插入將會失敗，因此沒有新的類別目錄資料列會加入至資料庫。 但我們還是必須在 web 伺服器檔案系統上已上傳摺頁冊檔案 ！ 這個檔案必須先刪除時插入工作流程期間發生例外狀況。

如先前所討論[處理 BLL 和 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程中，於擲回例外狀況從中透過各種不同的層級昇的架構的深度。 在展示層中，我們可以決定從 DetailsView s 是否發生例外狀況`ItemInserted`事件。 這個事件處理常式也會提供值的 ObjectDataSource 的`InsertParameters`。 因此，我們可以在其中建立事件處理常式`ItemInserted`事件，以檢查發生例外狀況時，如果是的話，會刪除 ObjectDataSource s 所指定的檔案`brochurePath`參數：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>總結

有幾個步驟，您必須執行才能提供網頁介面來加入包含二進位資料的記錄。 如果直接在資料庫儲存的二進位資料，有可能是您將需要更新之架構、 加入特定的方法，以處理二進位資料會被插入的情況。 架構已更新之後下, 一個步驟建立插入介面，這可以使用已自訂為包含二進位資料的每個欄位的 FileUpload 控制項 DetailsView 來達成。 上傳的資料可以再儲存到 web 伺服器檔案系統或指派給 DetailsView s 中的資料來源參數`ItemInserting`事件處理常式。

將二進位資料儲存至檔案系統時，需要更詳細的規劃，比直接在資料庫中儲存資料。 您必須選擇命名配置，以避免覆寫另一個 s 的一個使用者 s 上傳。 此外，您必須採取額外的步驟刪除上傳的檔案，如果資料庫的插入失敗。

我們現在可以將新類別新增至系統摺頁冊和圖片，但我們 ve 尚未來看看如何更新現有類別 s 的二進位資料或如何正確地移除已刪除的類別目錄的二進位資料。 在下一個教學課程中，我們將探討下列兩個主題。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dave Gardner、 Teresa Murphy 和 Bernadette Leigh。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](displaying-binary-data-in-the-data-web-controls-cs.md)
> [下一頁](updating-and-deleting-existing-binary-data-cs.md)
