---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: 上傳檔案 (C#) |Microsoft Docs
author: rick-anderson
description: 您的網站，可能會將它們儲存在伺服器的檔案系統的位置，了解如何允許使用者上傳二進位檔 （例如 Word 或 PDF 文件）...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: f420797b7c06b9063b70b784a5b61c7d02162c1d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820071"
---
<a name="uploading-files-c"></a>上傳檔案 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe)或[下載 PDF](uploading-files-cs/_static/datatutorial54cs1.pdf)

> 您的網站，可能會將它們儲存在伺服器的檔案系統或資料庫的位置，了解如何允許使用者上傳二進位檔 （例如 Word 或 PDF 文件）。


## <a name="introduction"></a>簡介

所有教學課程，我們已檢查到目前為止已以獨佔方式使用文字資料。 不過，許多應用程式必須擷取文字和二進位資料的資料模型。 Online dating 網站可能會允許使用者上傳其設定檔相關聯的圖片。 徵才及用人的網站可能會讓使用者上傳其繼續為 Microsoft Word 或 PDF 文件。

使用二進位資料會加入一組新的挑戰。 我們必須決定應用程式中儲存二進位資料的方式。 用於插入新記錄的介面已更新，以允許使用者從他們的電腦將檔案上傳，必須採取額外的步驟，來顯示或提供一種方式下載記錄 s 相關聯的二進位資料。 在本教學課程和下一步 的第三，我們將探討如何障礙這些挑戰。 這些教學課程的結尾我們會建立功能完整的應用程式與每個類別目錄產生關聯的圖片和 PDF 小冊子。 在這個特定的教學課程中我們將看看不同的技術，用於存放二進位資料，並探討如何讓使用者能夠從他們的電腦將檔案上傳，並已儲存在 web 伺服器檔案系統上。

> [!NOTE]
> 二進位資料的應用程式的資料模型的一部分，有時稱為[BLOB](http://en.wikipedia.org/wiki/Binary_large_object)，二進位大型物件的首字母縮寫。 在這些教學課程中我選擇使用術語的二進位資料，雖然 BLOB 是同義詞彙。


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>步驟 1： 建立工作與二進位資料 Web Pages

我們開始探索新增支援二進位資料的相關挑戰之前，可讓 s 先花點時間，我們需要針對本教學課程和下列三個我們網站專案中建立 ASP.NET 網頁。 藉由新增新的資料夾，名為啟動`BinaryData`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![加入 ASP.NET 網頁的二進位資料相關教學課程](uploading-files-cs/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁的二進位資料相關教學課程


在其他資料夾，例如`Default.aspx`在`BinaryData`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](uploading-files-cs/_static/image2.png))


最後，將這些頁面新增項目為`Web.sitemap`檔案。 具體來說，強化之後新增下列標記 GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含二進位資料的教學課程使用的項目。


![網站導覽現在包含二進位資料的教學課程使用的項目](uploading-files-cs/_static/image3.gif)

**圖 3**： 網站地圖現在包含二進位資料的教學課程使用的項目


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>步驟 2： 決定儲存二進位資料的位置

應用程式的資料模型與相關聯的二進位資料可以儲存在兩個地方的其中一個： 在 web 伺服器檔案系統的參考儲存在資料庫中的檔案或直接在資料庫本身內 （請參閱 圖 4）。 每一種方法都有它自己組的優缺點，並需要更詳細的討論。


[![二進位資料可以儲存在檔案系統上，或直接在資料庫中](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**圖 4**： 可以儲存二進位資料，在檔案系統上，或直接在資料庫中 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image4.png))


假設我們想要擴充 Northwind 資料庫建立關聯與每個產品的圖片。 其中一個選項是儲存在 web 伺服器檔案系統上的這些映像檔案，並記錄中的路徑`Products`資料表。 使用此方法時，我們的 d 新增`ImagePath`資料行`Products`型別的資料表`varchar(200)`，也或許是。 時為 Chai 使用者上傳的圖片，可能會在 web 伺服器檔案系統上儲存該圖片`~/Images/Tea.jpg`，其中`~`代表應用程式 s 的實體路徑。 也就是說，如果網站根目錄的實體路徑`C:\Websites\Northwind\`，`~/Images/Tea.jpg`相當於`C:\Websites\Northwind\Images\Tea.jpg`。 上傳映像檔之後, 我們 d 更新 Chai 中的記錄`Products`資料表，讓其`ImagePath`參考新的映像的路徑資料行。 我們可以使用`~/Images/Tea.jpg`或簡稱`Tea.jpg`如果我們決定要將所有的產品映像會置於應用程式的`Images`資料夾。

在 檔案系統上儲存的二進位資料的主要優點如下：

- **實作簡單**儲存和擷取直接在資料庫中儲存的二進位資料如我們所見，牽涉到更多程式碼的程式時使用透過檔案系統的資料。 此外，若要檢視或下載的二進位資料的使用者必須向他們該資料的 URL。 如果資料位於 web 伺服器檔案系統上，則 URL 會是簡單。 如果資料儲存在資料庫中，不過，網頁必須建立，會擷取，並從資料庫傳回的資料。
- **更多的存取權的二進位資料**二進位資料可能需要存取其他服務或應用程式，是無法提取資料庫中的資料。 例如，每個產品相關聯的映像可能也需要可供使用者透過[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)，d 我們想要在此情況下將二進位資料儲存在檔案系統上。
- **效能**二進位資料會儲存在檔案系統中，如果資料庫伺服器和 web 伺服器之間的需求和網路壅塞會少於如果直接在資料庫內儲存的二進位資料。

將二進位資料儲存在檔案系統上的主要缺點是，它減少了資料庫中的資料。 如果記錄遭到刪除從`Products`資料表相關聯的檔案，在 web 伺服器檔案系統上不會自動刪除。 我們必須撰寫額外的程式碼，若要刪除檔案或檔案系統將會變得雜亂無章未使用、 被遺棄的檔案。 此外，當備份資料庫，我們必須確定對檔案系統，以及相關聯的二進位資料的備份。 將資料庫移到另一個網站或伺服器帶來類似的挑戰。

藉由建立類型的資料行，或者，可以直接在 Microsoft SQL Server 2005 資料庫中預存的二進位資料`varbinary`。 像其他可變長度資料類型，您可以在此資料行中指定的二進位資料，可以保留最大長度。 例如，若要保留最多 5,000 個位元組使用`varbinary(5000)`;`varbinary(MAX)`允許儲存體大小上限，約 2GB。

直接在資料庫中儲存二進位資料的主要優點是二進位資料和資料庫記錄之間緊密的關聯性。 這可大幅簡化資料庫管理工作，例如備份或將資料庫移至不同的網站或伺服器。 此外，自動刪除記錄會刪除相對應的二進位資料。 另外還有其他難以察覺的優點，在資料庫中儲存二進位資料。 請參閱[儲存二進位檔案直接在資料庫使用 ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx)如更深入的討論。

> [!NOTE]
> 在 Microsoft SQL Server 2000 和舊版中，`varbinary`資料型別有最大限制為 8000 個位元組。 若要儲存最多 2 GB 的二進位資料[`image`資料型別](https://msdn.microsoft.com/library/ms187993.aspx)必須改為使用。 加上`MAX`在 SQL Server 2005，不過，`image`資料型別已被取代。 它 s 仍支援回溯相容性，但是 Microsoft 已經宣布， `image` SQL Server 的未來版本將移除的資料型別。


如果您正在使用較舊的資料模型可能會看到`image`資料型別。 Northwind 資料庫 s`Categories`資料表有`Picture`可以用來儲存影像檔的分類的二進位資料的資料行。 因為 Northwind 資料庫都有自己的根目錄，在 Microsoft Access 和舊版的 SQL Server 中，這個資料行是型別的`image`。

如需本教學課程和下列三個，我們將使用這兩種方法。 `Categories`資料表已經有`Picture`來儲存二進位映像內容的類別目錄的資料行。 我們將新增額外的資料行`BrochurePath`，可用來提供列印品質、 亮眼的概觀，類別目錄的 web 伺服器檔案系統上儲存成 PDF 的路徑。

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>步驟 3： 加入`BrochurePath`資料行`Categories`資料表

Categories 資料表目前有只有四個資料行： `CategoryID`， `CategoryName`， `Description`，和`Picture`。 除了這些欄位中，我們需要加入新的檔案 （如果有的話），將會指向類別 s 摺頁冊。 若要將此資料行，請移至 伺服器總管 中，向下切入至資料表，以滑鼠右鍵按一下`Categories`資料表，然後選擇 開啟資料表定義 （請參閱 圖 5）。 如果看不到 [伺服器總管] 中，從 [檢視] 功能表中，選取 [伺服器總管] 選項開啟它，或按 Ctrl + Alt + S。

加入新`varchar(200)`資料行`Categories`名為資料表`BrochurePath`，並允許`NULL`s，並按一下 [儲存] 圖示 （或按 Ctrl + S）。


[![將 BrochurePath 資料行新增至 Categories 資料表](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**圖 5**： 新增`BrochurePath`資料行`Categories`資料表 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>步驟 4： 更新的架構，以使用`Picture`和`BrochurePath`資料行

`CategoriesDataTable`資料存取層 (DAL) 中目前有四個`DataColumn`定義： `CategoryID`， `CategoryName`， `Description`，和`NumberOfProducts`。 當我們原本是設計在此 DataTable[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，`CategoriesDataTable`只有前三個資料行;`NumberOfProducts`資料行已加入[主要/詳細資料使用的項目符號具有詳細資料 DataList 的主要記錄的份](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)教學課程。

中所述*建立資料存取層*，Datatable 中具類型資料集組成商務物件。 Tableadapter 負責與資料庫通訊，並填入商務物件，含有查詢結果。 `CategoriesDataTable`會填入`CategoriesTableAdapter`，其中包含三個資料擷取方法：

- `GetCategories()` 執行 TableAdapter s 主查詢並傳回`CategoryID`， `CategoryName`，並`Description`欄位中的所有記錄的`Categories`資料表。 主要的查詢是什麼由自動產生`Insert`和`Update`方法。
- `GetCategoryByCategoryID(categoryID)` 會傳回`CategoryID`， `CategoryName`，並`Description`欄位類別目錄的其`CategoryID`等於*categoryID*。
- `GetCategoriesAndNumberOfProducts()` -傳回`CategoryID`， `CategoryName`，並`Description`欄位中的所有記錄`Categories`資料表。 也會使用子查詢傳回的每個類別目錄相關聯的產品數目。

請注意，其中一個項目查詢會傳回`Categories`資料表 s`Picture`或是`BrochurePath`資料行; 也不`CategoriesDataTable`提供`DataColumn`的這些欄位。 若要使用的圖片及`BrochurePath`屬性，我們需要先將它們加入`CategoriesDataTable`，然後再更新`CategoriesTableAdapter`類別，以傳回這些資料行。

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>新增`Picture`和`BrochurePath``DataColumn`s

藉由加入這些兩個資料行，來啟動`CategoriesDataTable`。 以滑鼠右鍵按一下`CategoriesDataTable`s 標頭中，從內容功能表中選取 新增，然後選擇 資料行選項。 這會建立新`DataColumn`名為 DataTable 中`Column1`。 重新命名此資料行`Picture`。 從 [屬性] 視窗中，設定`DataColumn`s`DataType`屬性設`System.Byte[]`（這不是下拉式清單中的選項; 您需要在輸入時）。


[![建立 DataColumn 名為圖片的資料類型是 System.Byte](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**圖 6**： 建立`DataColumn`具名`Picture`其`DataType`會`System.Byte[]`([按一下以檢視完整大小的影像](uploading-files-cs/_static/image8.png))


新增另一個`DataColumn`至 DataTable，將它命名為`BrochurePath`使用預設`DataType`值 (`System.String`)。

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>傳回`Picture`和`BrochurePath`TableAdapter 中的值

使用這兩個`DataColumn`加入至`CategoriesDataTable`，我們準備好更新`CategoriesTableAdapter`。 我們可能會有這兩個主要的 TableAdapter 查詢中傳回這些資料行值，但這會帶回二進位資料，每次`GetCategories()`已叫用方法。 相反地，可讓 s 更新主要的 TableAdapter 查詢，以恢復`BrochurePath`並建立額外的資料擷取方法會傳回特定類別的`Picture`資料行。

若要更新主要的 TableAdapter 查詢，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後選擇 從內容功能表的 設定選項。 這會帶出資料表配接器組態精靈 」，這點我們發生了許多過去的教學課程所示。 更新查詢，以帶回`BrochurePath`按一下 [完成]。


[![更新資料行清單中 SELECT 陳述式也會傳回 BrochurePath](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**圖 7**： 更新中的資料行清單`SELECT`陳述式，也會傳回`BrochurePath`([按一下以檢視完整大小的影像](uploading-files-cs/_static/image10.png))


當使用 TableAdapter 的特定 SQL 陳述式，更新主查詢中的資料行清單的所有更新的資料行清單`SELECT`查詢中的 TableAdapter 方法。 這表示`GetCategoryByCategoryID(categoryID)`方法來傳回已更新`BrochurePath`資料行，這可能是我們想要的結果。 不過，它也更新中的資料行清單`GetCategoriesAndNumberOfProducts()`方法，移除子查詢傳回的每個類別目錄的產品數目 ！ 因此，我們需要更新此方法的`SELECT`查詢。 以滑鼠右鍵按一下`GetCategoriesAndNumberOfProducts()`方法中，選擇 [設定]，並還原`SELECT`回到其原始值的查詢：


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

接下來，建立新的 TableAdapter 方法會傳回特定類別的`Picture`資料行的值。 以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後選擇 加入查詢選項，以啟動 TableAdapter 查詢組態精靈。 此精靈的第一個步驟會要求我們是否我們想要使用特定 SQL 陳述式的查詢資料時，新的預存程序或現有的帳戶。 選取 使用 SQL 陳述式，然後按一下 下一步。 因為我們將會傳回一個資料列，選擇 SELECT 會傳回第二個步驟中的資料列的選項。


[![選取 使用 SQL 陳述式選項](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**圖 8**： 選取 使用 SQL 陳述式選項 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image12.png))


[![因為查詢會傳回一筆記錄，從 Categories 資料表，選擇選取 傳回資料列](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**圖 9**： 因為查詢會傳回一筆記錄，從 Categories 資料表中，選擇 選取會傳回資料列 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image14.png))


在第三個步驟中，輸入下列 SQL 查詢，然後按一下 [下一步]:


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

最後一個步驟是選擇新方法的名稱。 使用`FillCategoryWithBinaryDataByCategoryID`和`GetCategoryWithBinaryDataByCategoryID`填滿的 DataTable 和傳回 DataTable 模式，分別。 按一下 完成 以完成精靈。


[![選擇 TableAdapter 的方法的名稱](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**圖 10**： 選擇 tableadapter 方法的名稱 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image16.png))


> [!NOTE]
> 資料表配接器查詢組態精靈完成後，您可能會看到對話方塊，通知您新命令文字傳回資料，包含結構描述上的主查詢的結構描述不同。 簡單地說，精靈會注意的是，TableAdapter s 主查詢`GetCategories()`傳回不同的結構描述比我們剛剛建立的一個。 但是，這是我們想，因此您可以忽略此訊息。


此外，請記住，如果您使用特定 SQL 陳述式，並使用精靈來變更 TableAdapter s 主查詢，在稍後的時間，它會修改`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`陳述式 s 資料行清單中包含的資料行主要的查詢 (也就是它會移除`Picture`來自查詢的資料行)。 您必須手動更新傳回的資料行清單`Picture`資料行，類似於我們與`GetCategoriesAndNumberOfProducts()`稍早在此步驟中的方法。

新增兩個後`DataColumn`s`CategoriesDataTable`而`GetCategoryWithBinaryDataByCategoryID`方法，以`CategoriesTableAdapter`，這些型別 DataSet 設計工具中的類別應該看起來像 [圖 11] 中的螢幕擷取畫面。


![DataSet 設計工具包括新的資料行和方法](uploading-files-cs/_static/image11.gif)

**圖 11**: DataSet 設計工具包括新的資料行和方法


## <a name="updating-the-business-logic-layer-bll"></a>正在更新商務邏輯層 (BLL)

使用更新的 DAL，剩下的就是以增強的 「 商務邏輯層 (BLL) 」，以納入新的方法`CategoriesTableAdapter`方法。 將下列方法加入`CategoriesBLL`類別：


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>步驟 5： 上傳至 Web 伺服器從用戶端檔案

收集二進位資料時，通常這項資料是由使用者所提供。 若要擷取這項資訊，使用者必須能夠從他們的電腦到 web 伺服器將檔案上傳。 然後上傳的資料必須與資料模型中，這可能表示將檔案儲存至 web 伺服器檔案系統並將路徑新增至資料庫中的檔案或二進位內容直接寫入資料庫的整合。 在此步驟中我們將探討如何讓使用者從他們的電腦與伺服器的檔案上傳。 在下一個教學課程中我們將把焦點轉到整合資料模型中的上傳的檔案。

ASP.NET 2.0 的新[FileUpload Web 控制項](https://msdn.microsoft.com/library/ms227677(VS.80).aspx)提供一個機制，將檔案從他們的電腦傳送到 web 伺服器的使用者。 FileUpload 控制項轉譯成`<input>`項目其`type`屬性設為瀏覽 按鈕與文字方塊的瀏覽器顯示的檔案。 按一下 [瀏覽] 按鈕會顯示的對話方塊，使用者可以從中選取檔案。 當表單回傳時，所選的檔案的內容與回傳一起傳送。 伺服器端上傳檔案的相關資訊是透過 FileUpload 控制項的內容存取。

若要示範如何將檔案上傳，開啟`FileUpload.aspx`頁面中`BinaryData`資料夾中，將 FileUpload 控制項從工具箱拖曳至設計工具中，並將控制項 s`ID`屬性設`UploadTest`。 接下來，新增按鈕 Web 控制項設定其`ID`並`Text`屬性，以`UploadButton`並分別上傳選取的檔案。 最後，清除中放置標籤 Web 控制項下方的按鈕，其`Text`屬性並設定其`ID`屬性設`UploadDetails`。


[![FileUpload 控制項加入 ASP.NET 網頁](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**圖 12**: FileUpload 控制項加入 ASP.NET 網頁 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image18.png))


[圖 13] 顯示此頁面上，當透過瀏覽器檢視。 請注意，按一下 [瀏覽] 按鈕會開啟檔案選取對話方塊中，可讓使用者能夠選取的檔案，從他們的電腦。 選取的檔案，按一下 [上傳選取的檔案] 按鈕後，將選取的檔案二進位內容傳送至 web 伺服器回傳。


[![使用者可以選取要從他們的電腦上傳到伺服器的檔案](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**圖 13**： 使用者可以選取要上傳檔案從伺服器電腦 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image20.png))


在回傳時，請上傳的檔案可以儲存至檔案系統，或可以直接透過 Stream 使用過其二進位資料。 此範例中，讓建立`~/Brochures`資料夾，並將儲存並上傳的檔案。 首先新增`Brochures`至網站根目錄的子資料夾的資料夾。 接下來，建立的事件處理常式`UploadButton`s`Click`事件，並新增下列程式碼：


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

FileUpload 控制項提供各種不同的使用上傳資料的屬性。 比方說， [ `HasFile`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx)指出檔案是否已上傳的使用者，雖然[`FileBytes`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx)為位元組陣列提供已上傳二進位資料的存取權。 `Click`事件處理常式一開始會確保已上傳檔案。 如果已上傳檔案，則標籤會顯示上傳的檔案，以位元組為單位，其大小和其內容類型的名稱。

> [!NOTE]
> 若要確保在使用者上傳的檔案，您可以檢查`HasFile`屬性，並顯示一則警告，如果它 s `false`，或您可以使用[RequiredFieldValidator 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx)改為。


S FileUpload`SaveAs(filePath)`將上傳的檔案儲存至指定*filePath*。 *filePath*必須是*實體路徑*(`C:\Websites\Brochures\SomeFile.pdf`) 而非*虛擬**路徑*(`/Brochures/SomeFile.pdf`)。 [ `Server.MapPath(virtPath)`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx)虛擬路徑，並傳回其對應的實體路徑。 這裡的虛擬路徑是`~/Brochures/fileName`，其中*fileName*是上傳的檔案名稱。 請參閱[使用 Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml)如需有關虛擬和實體路徑，並使用`Server.MapPath`。

完成之後`Click`事件處理常式，請花一點時間瀏覽器中測試。 按一下 瀏覽按鈕並選取您的硬碟中的檔案，然後按一下 上傳選取的檔案 按鈕。 回傳會將所選檔案的內容傳送至 web 伺服器，然後將會顯示檔案的相關資訊，再將它儲存為`~/Brochures`資料夾。 上傳檔案之後, 返回 Visual Studio，然後按一下 方案總管 中的重新整理 按鈕。 您應該會看到您剛才上傳 ~/Brochures 資料夾中的檔案 ！


[![檔案 EvolutionValley.jpg 已上傳至 Web 伺服器](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**圖 14**: File`EvolutionValley.jpg`已上傳至 Web 伺服器 ([按一下以檢視完整大小的影像](uploading-files-cs/_static/image22.png))


![EvolutionValley.jpg 已儲存至 ~/Brochures 資料夾](uploading-files-cs/_static/image15.gif)

**圖 15**:`EvolutionValley.jpg`已儲存至`~/Brochures`資料夾


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>上傳的檔案儲存至檔案系統的微妙之處

有數種微妙之處，儲存檔案上傳至 web 伺服器檔案系統時必須加以解決。 首先，那里安全性問題。 若要將檔案儲存至檔案系統中，執行 ASP.NET 網頁的安全性內容必須具有寫入權限。 在目前的使用者帳戶的內容下執行 ASP.NET Development Web Server。 如果您使用 Microsoft 的 Internet Information Services (IIS) 做為 web 伺服器，安全性內容取決於 IIS 和其組態的版本。

將檔案儲存至檔案系統的另一項挑戰的檔案命名為中心。 目前，我們的頁面便會儲存所有上傳的檔案，以`~/Brochures`為 s 的用戶端電腦上的檔案使用相同名稱的目錄。 如果使用者的上傳同名摺頁冊`Brochure.pdf`，檔案會儲存為`~/Brochure/Brochure.pdf`。 但如果一段時間的更新版本的使用者 B 將檔案上傳不同的手冊，剛好有相同的檔案名稱 (`Brochure.pdf`)？ 使用程式碼現在有，s 檔案將會覆寫與使用者 B s 上傳的使用者。

有幾個方法來解析檔案名稱衝突。 其中一個選項是禁止上傳檔案，如果已存在具有相同名稱的其中一個。 使用此方法，當使用者 B 嘗試上傳檔案，名為`Brochure.pdf`，系統不會儲存其檔案，並不會改為顯示訊息，通知使用者 B 重新命名檔案，再試一次。 另一個方法是使用唯一的檔案名稱，這可能是儲存檔案[全域唯一識別碼 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)或從對應的資料庫記錄的 s 主索引鍵資料行的值 (假設上傳相關聯特定資料列的資料模型中）。 在下一個教學課程中，我們將探討這些選項的詳細資料。

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>涉及極大量的二進位資料的挑戰

這些教學課程會假設擷取二進位資料是在大小太大。 使用非常大量的二進位資料檔案，則數 mb 或更大導入了新的挑戰，討論這些教學課程的範圍內。 例如，根據預設 ASP.NET 會拒絕上的傳超過 4 mb，雖然這可以透過設定[`<httpRuntime>`項目](https://msdn.microsoft.com/library/e1f13641.aspx)在`Web.config`。 IIS 會為它自己的檔案上傳大小的限制，過。 請參閱[IIS 上傳的檔案大小](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html)如需詳細資訊。 此外上, 傳大型檔案所花費的時間可能會要求將會等到 ASP.NET 110 秒超過預設值。 也有使用大型檔案時所發生的記憶體和效能問題。

FileUpload 控制項是不切實際的大型檔案上傳。 因為檔案的內容張貼至伺服器，終端使用者必須耐心等待而不需要任何其上傳正在進行的確認。 處理較小的檔案可上傳在幾秒鐘，但可以處理較大可能需要分鐘的時間才能上傳的檔案時，會發生問題時，這是不這麼多的問題。 有各種不同的第三方檔案上傳控制項更適合處理大上傳，許多這些廠商提供進度指標和 ActiveX 上傳管理員，可提供更完備的使用者體驗。

如果您的應用程式需要處理大型檔案，您必須仔細調查所面臨的挑戰，並針對您特定的需求尋找適當的解決方案。

## <a name="summary"></a>總結

建置的應用程式必須擷取二進位資料導入了幾項挑戰。 在本教學課程中，我們探討前兩個： 決定儲存二進位資料的位置，並讓使用者透過網頁的二進位內容上傳。 透過接下來三個教學課程中，我們會看到如何上傳的資料關聯的資料庫中的記錄，以及如何顯示其文字資料欄位旁邊的二進位資料。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [使用大數值資料類型](https://msdn.microsoft.com/library/ms178158.aspx)
- [FileUpload 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 FileUpload 伺服器控制項](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [深色的側邊的檔案上傳](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 Bernadette Leigh。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](displaying-binary-data-in-the-data-web-controls-cs.md)
