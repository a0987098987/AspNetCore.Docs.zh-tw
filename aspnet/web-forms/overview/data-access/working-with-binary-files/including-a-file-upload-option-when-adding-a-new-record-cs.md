---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
title: "加入新的記錄 (C#) 時，其中包括的檔案上傳選項 |Microsoft 文件"
author: rick-anderson
description: "本教學課程會示範如何建立可讓使用者輸入的文字資料，並上傳二進位檔案的 Web 介面。 為了說明選項可用 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 362ade25-3965-4fb2-88d2-835c4786244f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-cs
msc.type: authoredcontent
ms.openlocfilehash: 384251e5d0d72c6d1cc014c929a5d504be11d1d6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="including-a-file-upload-option-when-adding-a-new-record-c"></a>包括檔案上傳選項，當加入新的記錄 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_CS.exe)或[下載 PDF](including-a-file-upload-option-when-adding-a-new-record-cs/_static/datatutorial56cs1.pdf)

> 本教學課程會示範如何建立可讓使用者輸入的文字資料，並上傳二進位檔案的 Web 介面。 為了說明可用來儲存二進位資料的選項，一個檔案會儲存在資料庫中而其他則會儲存在檔案系統中。


## <a name="introduction"></a>簡介

我們在先前的兩個教學課程中瀏覽技巧來儲存應用程式的資料模型，查看如何使用檔案上傳控制項，將檔案從用戶端傳送到 web 伺服器，並可了解如何在資料 W 呈現這項二進位資料與相關聯的二進位資料eb 控制項。 我們尚未以討論如何上傳的資料關聯的資料模型，不過我們。

在本教學課程中，我們將建立網頁新增新的類別。 除了類別 s 的名稱和描述的文字方塊，將需要此頁面包含兩個檔案上傳控制項的新類別的圖片，另一個用於冊。 上傳的圖片會直接儲存在新的記錄 s`Picture`資料行，而冊儲存`~/Brochures`資料夾以儲存新的記錄 s 中的檔案路徑`BrochurePath`資料行。

之前建立這個新的網頁，我們需要更新架構。 `CategoriesTableAdapter` s 主查詢未擷取必要`Picture`資料行。 因此，自動產生`Insert`方法只會有輸入`CategoryName`， `Description`，和`BrochurePath`欄位。 因此，我們需要建立額外的方法會提示輸入所有的四個 TableAdapter 中`Categories`欄位。 `CategoriesBLL`商務邏輯層中的類別也需要更新。

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>步驟 1： 加入`InsertWithPicture`方法`CategoriesTableAdapter`

當我們建立`CategoriesTableAdapter`回[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，我們設定為自動產生`INSERT`， `UPDATE`，和`DELETE`主要查詢為基礎的陳述式。 此外，我們指示採用 DB 直接的方法中，建立方法 TableAdapter `Insert`， `Update`，和`Delete`。 這些方法可讓您執行自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式，因此，接受根據主查詢所傳回的資料行的輸入的參數。 在[上傳檔案](uploading-files-cs.md)教學課程中我們夾帶`CategoriesTableAdapter`s 主查詢使用`BrochurePath`資料行。

因為`CategoriesTableAdapter`s 主查詢沒有參考`Picture`資料行中，我們可以加入新的記錄都有的值更新現有的記錄`Picture`資料行。 若要擷取這項資訊，我們可以建立新的方法會特別用來插入含二進位資料的資料錄的 TableAdapter 中，或者我們可以自訂自動產生`INSERT`陳述式。 自訂自動產生的問題`INSERT`陳述式是我們風險讓我們的自訂覆寫精靈。 例如，假設我們自訂`INSERT`陳述式，包括使用`Picture`資料行。 這會更新 tableadapter`Insert`方法，將包含額外的輸入的參數為類別的圖片 s 二進位資料。 我們無法再建立方法中使用這個 DAL 方法和展示層中，透過這個 BLL 方法叫用商務邏輯層及的所有項目會處理作出。 亦即，直到下一次設定 TableAdapter 透過 TableAdapter 組態精靈。 當精靈完成時，我們的自訂`INSERT`陳述式可能會被覆寫，`Insert`方法會還原成原來的形式，並將不會再編譯的程式碼 ！

> [!NOTE]
> 不便使用預存程序，而不特定 SQL 陳述式時非問題。 未來的教學課程將探索資料存取層中使用這個特定 SQL 陳述式的預存程序。


若要避免此可能性麻煩，而不是自訂的自動產生 SQL 陳述式，可讓 s 改為建立 TableAdapter 的新方法。 這個方法，名為`InsertWithPicture`，將會接受的值`CategoryName`， `Description`， `BrochurePath`，和`Picture`資料行，並執行`INSERT`新記錄中儲存所有的四個值的陳述式。

開啟 輸入資料集，並從設計工具中，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭和內容功能表選擇 加入查詢。 這會啟動 TableAdapter 查詢組態精靈，一開始會詢問我們 TableAdapter 查詢應該如何存取資料庫。 選擇 使用 SQL 陳述式，然後按一下 下一步。 下一個步驟會產生提示的查詢類型。 因為我們重新建立查詢以新增新記錄以`Categories`資料表，選擇 [插入]，按一下 [下一步]。


[![選取 [插入] 選項](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image1.png)

**圖 1**： 選取 [插入] 選項 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.png))


我們現在需要指定`INSERT`SQL 陳述式。 精靈會自動建議`INSERT`對應至 TableAdapter s 主查詢的陳述式。 在此情況下，它 s`INSERT`插入的陳述式`CategoryName`， `Description`，和`BrochurePath`值。 Update 陳述式，讓`Picture`資料行是否包含連同`@Picture`參數，就像這樣：


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample1.sql)]

在精靈的最後一個畫面會詢問您要將新的 TableAdapter 方法。 輸入`InsertWithPicture`按一下 [完成]。


[![新的 TableAdapter 方法 InsertWithPicture 名稱](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.png)

**圖 2**： 將新的 TableAdapter 方法`InsertWithPicture`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>步驟 2： 更新商務邏輯層

展示層應該只有介面與商務邏輯層，而不是我們要建立 BLL 方法叫用剛才所建立的 DAL 方法略過它，直接移至資料存取層，因為 (`InsertWithPicture`)。 本教學課程中建立方法`CategoriesBLL`名為類別`InsertWithPicture`接受作為輸入的三`string`s 和`byte`陣列。 `string`輸入的參數都是為類別 s 的名稱、 描述和冊檔案路徑，雖然`byte`陣列為類別的圖片的二進位內容。 如下列程式碼所示，這個 BLL 方法會叫用對應的 DAL 方法：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample2.cs)]

> [!NOTE]
> 請確定您已經加入之前儲存型別資料集`InsertWithPicture`BLL 的方法。 因為`CategoriesTableAdapter`類別程式碼是以自動產生基礎具類型資料集時，如果您不要先將您的變更儲存至型別資料集`Adapter`屬性贏了 t 知道`InsertWithPicture`方法。


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>步驟 3： 列出現有的類別和其二進位資料

在本教學課程中我們將建立頁面，好讓使用者將新類別新增至系統，提供新的類別目錄的圖片和冊。 在[前述教學課程](displaying-binary-data-in-the-data-web-controls-cs.md)我們 GridView ImageField TemplateField 與用來顯示每個類別 s 的名稱，描述、 圖片和下載其冊的連結。 可讓此教學課程中，建立網頁，同時列出所有現有的類別，並允許建立新的複寫功能的 s。

先開啟`DisplayOrDownload.aspx`頁面`BinaryData`資料夾。 請移至來源檢視，並將複製的 GridView 和 ObjectDataSource s 宣告式語法，在貼上`<asp:Content>`中的項目`UploadInDetailsView.aspx`。 此外，不要忘了透過複製`GenerateBrochureLink`方法從程式碼後置類別`DisplayOrDownload.aspx`至`UploadInDetailsView.aspx`。


[![複製並貼上 UploadInDetailsView.aspx 從 DisplayOrDownload.aspx 的宣告式語法](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.png)

**圖 3**： 複製和貼上從宣告式語法`DisplayOrDownload.aspx`至`UploadInDetailsView.aspx`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.png))


複製的宣告式語法之後和`GenerateBrochureLink`方法，透過以`UploadInDetailsView.aspx`頁面上，檢視網頁透過瀏覽器，確認所有項目已透過正確複製。 您應該會看到 GridView 列出八個類別目錄，其中包含下載冊為類別的圖片的連結。


[![您現在應該會看到每個類別目錄以及其二進位資料](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.png)

**圖 4**： 您應該立即請參閱每個類別目錄沿著其二進位資料 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>步驟 4： 設定`CategoriesDataSource`支援插入

`CategoriesDataSource` ObjectDataSource 所使用`Categories`GridView 目前不提供讓您插入資料。 若要支援插入透過此資料來源控制項，我們要對應其`Insert`方法，其基礎物件中的方法以`CategoriesBLL`。 特別是，我們要將它對應至`CategoriesBLL`方法回在步驟 2 中，我們已加入`InsertWithPicture`。

按一下 ObjectDataSource s 智慧標籤的設定資料來源連結來啟動。 第一個畫面會顯示要使用的設定資料來源物件`CategoriesBLL`。 保留此設定設為-按一下前進至定義資料方法螢幕旁邊。 移至 [插入] 索引標籤，並挑選`InsertWithPicture`方法，從下拉式清單。 按一下 [完成] 5d; 以完成精靈。


[![設定為使用 InsertWithPicture 方法 ObjectDataSource](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.png)

**圖 5**： 設定用於 ObjectDataSource`InsertWithPicture`方法 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.png))


> [!NOTE]
> 正在完成精靈，在 Visual Studio 可能會要求是否您想要重新整理欄位和索引鍵，其會重新產生資料 Web 控制項欄位。 選擇 否，因為選擇 是 將會覆寫您所做的任何自訂欄位。


完成精靈之後，ObjectDataSource 現在包含的值及其`InsertMethod`屬性以及`InsertParameters`四個類別資料行中，為下列宣告式標記說明：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>步驟 5： 建立插入介面

First 涵蓋[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)，DetailsView 控制項提供內建的插入介面，可以利用使用支援插入的資料來源控制項時。 可讓 s DetailsView 控制項加入此頁面上方 GridView，永遠會轉譯其插入的介面，讓使用者快速新增新的類別。 在 DetailsView 加入新的類別，其下的 GridView 會自動重新整理，並顯示新的類別。

從 [工具箱] 拖曳至上方 GridView 中設定設計工具拖曳 DetailsView 開始其`ID`屬性`NewCategory`和清除`Height`和`Width`屬性值。 在 DetailsView s 智慧標籤，從繫結到現有`CategoriesDataSource`然後核取 啟用插入核取方塊。


[![結合 CategoriesDataSource DetailsView 和啟用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.png)

**圖 6**： 繫結至 DetailsView`CategoriesDataSource`和啟用插入 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image12.png))


若要永久轉譯 DetailsView 插入介面中的，設定其`DefaultMode`屬性`Insert`。

請注意，在 DetailsView 的五個 BoundFields `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath`雖然`CategoryID`因為 BoundField 不會插入介面中呈現它`InsertVisible`屬性設定為`false`。 因為它們是所傳回的資料行，這些 BoundFields 存在`GetCategories()`方法，後者為 ObjectDataSource 叫用來擷取其資料。 插入，不過，我們不想要讓使用者指定的值`NumberOfProducts`。 此外，我們需要允許它們上傳新的類別目錄的圖片，以及上傳的冊 PDF。

移除`NumberOfProducts`DetailsView 完全與更新 BoundField`HeaderText`屬性`CategoryName`和`BrochurePath`BoundFields 類別目錄和冊，分別。 接下來，將轉換`BrochurePath`到 TemplateField BoundField 並加入新的圖片，TemplateField 提供此新 TemplateField`HeaderText`圖片的值。 移動`Picture`TemplateField，讓它變成之間`BrochurePath`TemplateField 和 CommandField。


![結合 CategoriesDataSource DetailsView 和啟用插入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image7.gif)

**圖 7**： 繫結至 DetailsView`CategoriesDataSource`和啟用插入


如果您轉換`BrochurePath`到透過編輯欄位 對話方塊為 TemplateField BoundField TemplateField 包含`ItemTemplate`， `EditItemTemplate`，和`InsertItemTemplate`。 只有`InsertItemTemplate`是有需要不過，可以隨意移除兩個範本。 此時 DetailsView s 的宣告式語法看起來應該如下所示：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>將檔案上傳控制項加入冊和圖片欄位

目前， `BrochurePath` TemplateField s`InsertItemTemplate`包含文字方塊中，雖然`Picture`TemplateField 不包含任何範本。 我們需要更新這些兩個 TemplateField 的`InsertItemTemplate`以使用檔案上傳控制項。

從 DetailsView s 智慧標籤，選擇 [編輯樣板] 選項，然後選取`BrochurePath`TemplateField 的`InsertItemTemplate`從下拉式清單。 移除文字方塊中，然後將檔案上傳控制項從 [工具箱] 拖曳至範本。 將檔案上傳控制項 s`ID`至`BrochureUpload`。 同樣地，將檔案上傳控制項加入`Picture`TemplateField 的`InsertItemTemplate`。 設定此檔案上傳控制項 s`ID`至`PictureUpload`。


[![將檔案上傳控制項上加入](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image13.png)

**圖 8**： 將檔案上傳控制項加入`InsertItemTemplate`([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image14.png))


這些新增項目之後，請將兩個 TemplateField s 宣告式語法：


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample5.aspx)]

當使用者將新的類別時，我們想要確保冊及圖片檔案類型正確。 冊，使用者必須提供 PDF。 圖片中，我們需要使用者上傳的映像檔，但我們允許*任何*映像檔案或僅映像檔案的特定型別，例如 Gif 或 JPGs 嗎？ 若要允許不同的檔案類型，d 我們要擴充`Categories`包含擷取的檔案類型，以便可以透過用戶端傳送此類型的資料行的結構描述`Response.ContentType`中`DisplayCategoryPicture.aspx`。 因為我們不 t 有一個資料行，就必須在限制使用者僅提供特定的映像檔案類型。 `Categories`資料表 s 現有的映像是點陣圖，但 JPGs 更適當的檔案格式，透過 web 提供的映像。

如果使用者上傳不正確的檔案類型，我們需要取消插入，並顯示訊息，指出此問題。 加入標籤 Web 控制項 DetailsView 下方。 設定其`ID`屬性`UploadWarning`，清除出其`Text`屬性，將`CssClass`警告的屬性和`Visible`和`EnableViewState`屬性`false`。 `Warning` CSS 類別定義於`Styles.css`並呈現大型、 紅色、 斜體、 粗體字型中的文字。

> [!NOTE]
> 在理想情況下，`CategoryName`和`Description`BoundFields 會轉換成 TemplateFields 和自訂其插入介面。 `Description`插入介面，比方說，就可能會更適合透過多行文字方塊。 由於`CategoryName`資料行不接受`NULL`值 RequiredFieldValidator 應該加入以確保使用者提供新的類別的名稱的值。 這些步驟可供讀取器做為練習。 回頭參考[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)的深入了解加強資料修改介面。


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>步驟 6： 將上傳的冊儲存至 Web 伺服器的檔案系統

當使用者輸入新的類別目錄的值，然後按一下 [插入] 按鈕時，就會發生回傳，而且插入工作流程可以掀起。 首先，DetailsView s [ `ItemInserting`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx)引發。 下一步，ObjectDataSource s`Insert()`叫用方法時，結果會在新的記錄新增至`Categories`資料表。 之後，DetailsView s [ `ItemInserted`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx)引發。

在 ObjectDataSource s 前面`Insert()`叫用方法，我們必須先確認使用者已上傳適當的檔案類型，然後將冊 PDF 儲存到 web 伺服器檔案系統。 建立事件處理常式 DetailsView s`ItemInserting`事件並加入下列程式碼：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample6.cs)]

此事件處理常式啟動藉由參考`BrochureUpload`DetailsView 的範本的檔案上傳控制項。 然後，如果已上傳冊，會檢查已上傳的 s 副檔名。 如果沒有延伸。PDF、 然後警告會顯示、 插入已取消，並執行的事件處理常式結束。

> [!NOTE]
> 信賴憑證者上傳的 s 副檔名不是萬全技巧，確保上傳的檔案的 PDF 文件。 使用者可能會有有效的 PDF 文件副檔名`.Brochure`，或無法擁有採用非 PDF 文件，並提供`.pdf`延伸模組。 若要更明確地驗證檔案類型必須以程式設計方式檢查需要檔案 s 二進位內容。 這類徹底的方法，不過，通常是大材小用;檢查擴充功能就足以應付大部分的狀況。


中所述[上傳檔案](uploading-files-cs.md)教學課程，必須小心儲存檔案至檔案系統，讓一位使用者的傳不會覆寫另一個 s 時。 此教學課程中，我們會嘗試上傳的檔案使用相同的名稱。 如果已經存在的檔案中`~/Brochures`目錄與該相同檔名，不過，我們將附加的數字結尾，直到找到唯一的名稱。 例如，如果使用者將名為冊檔案上傳`Meats.pdf`，但已經有名稱為的檔案`Meats.pdf`中`~/Brochures`資料夾中，我們將會變更已儲存的檔案名稱以`Meats-1.pdf`。 如果已存在，我們將嘗試`Meats-2.pdf`，依此類推，直到找到唯一的檔案名稱。

下列程式碼會使用[`File.Exists(path)`方法](https://msdn.microsoft.com/library/system.io.file.exists.aspx)來判斷檔案是否已存在具有指定的檔案名稱。 如果是的話，它會繼續嘗試冊新檔案名稱，直到找到沒有衝突。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample7.cs)]

一旦找到有效的檔名，檔案必須儲存到檔案系統和 ObjectDataSource 的`brochurePath``InsertParameter`值需要更新，讓此檔案名稱寫入資料庫。 如我們所見回*上傳檔案*教學課程中，可以儲存檔案使用的檔案上傳控制項的`SaveAs(path)`方法。 若要更新 ObjectDataSource s`brochurePath`參數，使用`e.Values`集合。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample8.cs)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>步驟 7： 將上傳的圖片儲存到資料庫

若要上傳的圖片儲存在新`Categories`記錄，我們需要上傳的二進位內容指派至 ObjectDataSource s `picture` DetailsView s 中的參數`ItemInserting`事件。 不過，我們進行此作業之前，我們需要先確定已上傳的圖片是 JPG 和不一些其他的影像類型。 如同步驟 6 中，可讓 s 使用上載的圖片的副檔名來確定其型別。

雖然`Categories`資料表允許`NULL`值`Picture`資料行，所有分類目前有一張圖片。 可讓 s 會強制要求使用者提供圖片，加入新的類別，透過此頁面時。 下列程式碼會檢查以確保已上傳的圖片，而且它有適當的副檔名。


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample9.cs)]

這個程式碼應該放置*之前*步驟 6 中的程式碼，以便冊檔案儲存至檔案系統之前，如果沒有在圖片上傳的問題，將會終止事件處理常式。

假設已上傳適當的檔案，請將上傳的二進位內容指派給圖片參數 s 的值與下列程式碼行：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample10.cs)]

## <a name="the-completeiteminsertingevent-handler"></a>完整`ItemInserting`事件處理常式

為求完整起見，以下是`ItemInserting`完整的事件處理常式：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample11.cs)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>步驟 8： 修正`DisplayCategoryPicture.aspx`頁面

可讓 s 花點時間測試插入介面和`ItemInserting`幾個步驟在最後所建立的事件處理常式。 請瀏覽`UploadInDetailsView.aspx`頁面上透過瀏覽器，並嘗試新增的類別，但省略圖片，或指定非 JPG 圖片或非 PDF 冊。 任何這些情況下，將會顯示錯誤訊息，並取消插入工作流程。


[![警告訊息時，顯示無效的檔案類型已上傳](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image15.png)

**圖 9**: 警告訊息時，顯示無效的檔案類型已上傳 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image16.png))


一旦您已確認頁面需要上載圖片和獲利的 t 接受非 PDF 或非 JPG 檔案，加入新的類別，以有效的 JPG 圖片，將冊欄位保留空白。 頁面會回傳之後按一下 [插入] 按鈕，並將新的記錄將會加入至`Categories`直接儲存在資料庫上傳映像 s 二進位內容的資料表。 GridView 會更新，而且會顯示一個資料列的新加入的分類中，不過，如圖 10 所示，新的類別目錄的圖片就無法正確呈現。


[![新的類別不會顯示圖片的 s](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image17.png)

**圖 10**： 圖片不會顯示新的分類 s ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image18.png))


新的圖片不會顯示的原因是因為`DisplayCategoryPicture.aspx`傳回指定之分類的圖片的網頁設定成可以處理 OLE 標頭的點陣圖。 此 78 位元組的標頭移除`Picture`再進行傳送，則資料行 s 二進位內容傳回至用戶端。 但沒有此 OLE 標頭; JPG 檔案我們剛剛上傳新的類別目錄。因此，有效，需位元組正在移除從影像 s 二進位資料。

因為現在有兩個點陣圖中的 JPGs OLE 標頭與`Categories`資料表中，我們需要更新`DisplayCategoryPicture.aspx`使它不會刪除原始的八個類別目錄的 OLE 標頭和略過較新的類別記錄此條狀配置。 在我們的下一個教學課程中，我們將檢驗如何更新現有的記錄 s 映像，並使它們 JPGs 我們會將更新的所有舊的分類圖片。 現在，不過，使用中的下列程式碼`DisplayCategoryPicture.aspx`去除 OLE 標頭，僅針對那些原始的八個類別：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample12.cs)]

這項變更，使用 JPG 影像是現在正確呈現在 GridView。


[![新的類別目錄的 JPG 影像會正確轉譯](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image19.png)

**圖 11**： 新的類別目錄的 JPG 影像會正確轉譯 ([按一下以檢視完整大小的影像](including-a-file-upload-option-when-adding-a-new-record-cs/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>步驟 9： 刪除冊遇到例外狀況時

二進位資料儲存在 web 伺服器的檔案系統上的挑戰之一就是會導致中斷連接資料模型和二進位資料。 因此，每當刪除記錄時，對應的二進位資料，在檔案系統上也必須移除。 插入時，也可以來自加入遊戲。 請考慮下列狀況： 使用者加入新的類別，指定有效的圖片和冊。 按一下 [插入] 按鈕，就會發生回傳和 DetailsView 的`ItemInserting`事件引發，冊儲存至 web 伺服器檔案系統。 下一步，ObjectDataSource s`Insert()`叫用方法時，呼叫`CategoriesBLL`類別 s`InsertWithPicture`方法，這個方法會呼叫`CategoriesTableAdapter`s`InsertWithPicture`方法。

現在，如果資料庫離線，會發生什麼事或是否有錯誤`INSERT`SQL 陳述式？ 清楚地插入將會失敗，因此沒有新的類別目錄資料列會加入至資料庫。 但我們仍具有上傳的冊檔案位在 web 伺服器的檔案系統 ！ 這個檔案必須先刪除遇到期間插入工作流程的例外狀況時。

如先前所討論在[處理 BLL-以及 DAL 層級例外狀況，在 ASP.NET 網頁](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)教學課程中，於擲回例外狀況從內各層透過昇的架構的深度。 在展示層中，我們可以判斷是否發生了例外狀況從 DetailsView 的`ItemInserted`事件。 這個事件處理常式也會提供值的 ObjectDataSource 的`InsertParameters`。 因此，我們可以建立事件處理常式`ItemInserted`如果時發生例外狀況，如果是的話，會檢查的事件刪除 ObjectDataSource s 所指定的檔案`brochurePath`參數：


[!code-csharp[Main](including-a-file-upload-option-when-adding-a-new-record-cs/samples/sample13.cs)]

## <a name="summary"></a>總結

有幾個步驟，您必須執行才能提供的 web 型介面，來加入包含二進位資料的記錄。 如果直接將資料庫儲存二進位資料時，有可能是您要更新之架構、 加入特定的方法，以處理二進位資料會被插入的情況。 一旦架構已更新下, 一個步驟建立插入介面，可以使用已自訂為包含二進位資料的每個欄位的檔案上傳控制項的 DetailsView 來完成。 上傳的資料可以再儲存到 web 伺服器檔案系統或指派給 DetailsView s 中的資料來源參數`ItemInserting`事件處理常式。

將二進位資料儲存至檔案系統需要詳細規劃比直接在資料庫中儲存資料。 為了避免覆寫另一個 s 的一個使用者的上載，您必須選擇的命名配置。 此外，必須採取額外的步驟來刪除上傳的檔案，如果資料庫插入會失敗。

我們現在有了可將新的分類加入至具有冊和圖片，但我們系統 ve 尚未以查看如何更新現有的類別目錄 s 二進位資料或如何正確地移除已刪除類別目錄的二進位資料。 在下一個教學課程中，我們將探討以下兩個主題。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Dave Gardner、 本文菲和 Bernadette Leigh。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](displaying-binary-data-in-the-data-web-controls-cs.md)
[下一頁](updating-and-deleting-existing-binary-data-cs.md)
