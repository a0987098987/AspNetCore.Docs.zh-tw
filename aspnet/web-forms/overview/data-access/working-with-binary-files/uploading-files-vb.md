---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: 上傳檔案 (VB) |Microsoft 文件
author: rick-anderson
description: 了解如何允許使用者 （例如 Word 或 PDF 文件） 的二進位檔案上傳至您的網站，可能會將它們儲存在該伺服器的檔案系統中的其中...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fbc4aaf80ac7e0f960e140b492055fe35cd2b6ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="uploading-files-vb"></a>上傳檔案 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe)或[下載 PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> 了解如何允許使用者上載二進位檔案 （例如 Word 或 PDF 文件） 可能會將它們儲存在伺服器之檔案系統或資料庫中的其中您的網站。


## <a name="introduction"></a>簡介

所有的教學課程，我們 ve 檢查到目前為止已獨佔使用文字資料。 不過，許多應用程式必須擷取文字和二進位資料的資料模型。 Online dating 網站可能允許使用者上載圖片與他們的設定檔關聯。 招募網站可能會讓使用者將其恢復為 Microsoft Word 或 PDF 文件上傳。

使用二進位資料加入一組新的挑戰。 我們必須決定應用程式中儲存二進位資料的方式。 用於插入新記錄的介面已更新，以允許使用者從他們的電腦將檔案上傳，必須採取額外步驟來顯示或提供的方法來下載記錄 s 相關聯的二進位資料。 在本教學課程中，下一步的三個，我們將探討如何 hurdle 這些挑戰。 在這些教學課程結尾處我們將建立完整功能的應用程式與每個類別目錄產生關聯的圖片和 PDF 冊。 此特定教學課程中，我們會查看不同的技術，用於存放二進位資料，探索如何讓使用者從他們的電腦將檔案上傳和已儲存 web 伺服器的檔案系統上。

> [!NOTE]
> 二進位資料的應用程式的資料模型的一部分，有時稱為[BLOB](http://en.wikipedia.org/wiki/Binary_large_object)，二進位大型物件的縮略字。 在這些教學課程我選擇要使用的術語二進位資料，雖然詞彙 BLOB 是同義字。


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>步驟 1： 建立工作以二進位資料的 Web 網頁

若要瀏覽與支援的二進位資料相關聯的驗證題目開始之前，可讓 s 先花點時間我們網站專案中，我們需要針對此教學課程中，下一步的三個建立的 ASP.NET 網頁。 加入新的資料夾，名為啟動`BinaryData`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![加入 ASP.NET 網頁的二進位資料相關的教學課程](uploading-files-vb/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁的二進位資料相關的教學課程


如同在其他資料夾，`Default.aspx`中`BinaryData`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](uploading-files-vb/_static/image2.png))


最後，將這些頁面加入做為項目至`Web.sitemap`檔案。 具體來說，強化之後加入下列標記 GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 左側功能表現在包含二進位資料的教學課程使用的項目。


![站台地圖現在包含二進位資料的教學課程使用的項目](uploading-files-vb/_static/image3.gif)

**圖 3**： 網站導覽現在包含二進位資料的教學課程使用的項目


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>步驟 2： 決定儲存二進位資料的位置

應用程式的資料模型相關聯的二進位資料可以儲存在兩個地方的其中一個： web server s 檔案系統上的參考儲存在資料庫中的檔案或直接在資料庫中 （請參閱圖 4）。 每一種方法都有它自己組的優缺點，而且需要更詳細的討論。


[![在檔案系統上，或直接在資料庫可以儲存二進位資料](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**圖 4**： 在檔案系統上，或直接在資料庫可以儲存二進位資料 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image4.png))


假設我們想要擴充與每個產品圖片 Northwind 資料庫。 其中一個選項是儲存在 web 伺服器檔案系統上的這些映像檔案，並記錄中的路徑`Products`資料表。 使用此方法時，我們 d 加入`ImagePath`欄`Products`類型資料表`varchar(200)`，也或許是。 使用者針對 Chai 上載圖片，該圖片可能存放在 web 伺服器檔案系統上`~/Images/Tea.jpg`，其中`~`代表應用程式 s 實體路徑。 也就是說，如果網站根目錄的實體路徑`C:\Websites\Northwind\`，`~/Images/Tea.jpg`相當於`C:\Websites\Northwind\Images\Tea.jpg`。 上傳映像檔之後, 我們 d 更新 Chai 記錄`Products`資料表使其`ImagePath`參考資料行的新映像的路徑。 我們可以使用`~/Images/Tea.jpg`或簡稱`Tea.jpg`如果我們決定要將所有產品映像會將都放置在應用程式的`Images`資料夾。

二進位資料儲存在檔案系統上的主要優點包括：

- **簡化實作**我們會發現，儲存和擷取二進位資料儲存在資料庫中直接牽涉到更多程式碼的程式時使用透過檔案系統的資料。 此外，若要檢視或下載二進位資料讓使用者在他們必須呈現該資料的 url。 如果資料位於 web 伺服器的檔案系統中，URL 為簡單。 如果資料儲存在資料庫中，不過，網頁需要建立，它會擷取並從資料庫傳回資料。
- **更多的存取權的二進位資料**二進位資料可能需要存取其他服務或應用程式，則無法提取資料庫中的資料。 例如，每個產品相關聯的映像可能也需要能讓使用者透過[FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol)，d 我們想要在此情況下將二進位資料儲存在檔案系統上。
- **效能**二進位資料儲存在檔案系統上，如果資料庫伺服器和 web 伺服器之間的需求和網路壅塞會小於如果直接在資料庫中儲存二進位資料。

將二進位資料儲存在檔案系統上的主要缺點在於，它會減少資料庫的資料。 如果從刪除記錄`Products`不會自動刪除資料表中，web 伺服器檔案系統上相關聯的檔案。 我們必須撰寫額外的程式碼，若要刪除的檔案或檔案系統將就會散布與未使用，被遺棄的檔案。 此外，備份時資料庫，我們必須確定檔案系統上，以及讓相關聯的二進位資料的備份。 將資料庫移到另一個網站或伺服器帶來類似挑戰。

藉由建立類型的資料行，或者，可以直接在 Microsoft SQL Server 2005 資料庫中預存的二進位資料`varbinary`。 類似其他可變長度資料型別中，您可以此資料行中指定的二進位資料，可以保留的最大長度。 例如，若要保留最多 5,000 個位元組使用`varbinary(5000)`;`varbinary(MAX)`允許儲存體大小上限，約 2 GB。

直接在資料庫中儲存二進位資料的主要優點是二進位資料和資料庫記錄之間的緊密結合。 這可以大幅簡化資料庫管理工作，例如備份或將資料庫移至不同的網站或伺服器。 此外，自動刪除記錄會刪除對應的二進位資料。 也有二進位資料儲存在資料庫中的許多微妙的優點。 請參閱[儲存二進位檔案直接在資料庫使用 ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx)如更深入的討論。

> [!NOTE]
> 在 Microsoft SQL Server 2000 及更新版本、`varbinary`資料型別有最大限制為 8000 個位元組。 若要儲存多達 2 GB 的二進位資料[`image`資料型別](https://msdn.microsoft.com/library/ms187993.aspx)需要改為使用。 加上的`MAX`在 SQL Server 2005，不過，`image`資料型別已被取代。 它 s 仍支援回溯相容性，但 Microsoft 已宣布， `image` SQL Server 的未來版本將移除的資料型別。


如果您正在使用較舊的資料模型可能會看到`image`資料型別。 Northwind 資料庫 s`Categories`資料表有`Picture`可以用來儲存類別目錄的映像檔的二進位資料的資料行。 Northwind 資料庫有其 Microsoft Access 和舊版的 SQL Server 中的根，因為此資料行是類型`image`。

如需本教學課程和下一步的三個，我們會使用這兩種方法。 `Categories`資料表已經有`Picture`儲存影像分類的二進位內容的資料行。 我們會將額外的資料行新增`BrochurePath`，可以用來提供類別目錄的列印品質、 亮眼概觀在 web 伺服器檔案系統上儲存成 PDF 的路徑。

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>步驟 3： 加入`BrochurePath`欄`Categories`資料表

Categories 資料表目前有只能有四個資料行： `CategoryID`， `CategoryName`， `Description`，和`Picture`。 除了這些欄位中，我們需要加入新的檔案，將會為類別的冊 （如果有的話）。 若要加入此資料行，請移至 伺服器總管 中，向下鑽研至資料表、 以滑鼠右鍵按一下`Categories`資料表，並選擇 開啟資料表定義 （請參閱圖 5）。 如果看不到 [伺服器總管] 中，請從 [檢視] 功能表中，選取 [伺服器總管] 選項啟動，或按 Ctrl + Alt + S。

加入新`varchar(200)`欄`Categories`資料表名為`BrochurePath`並允許`NULL`s，然後按一下 [儲存] 圖示 （或按 Ctrl + S）。


[![將 BrochurePath 資料行加入至 Categories 資料表](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**圖 5**： 新增`BrochurePath`欄`Categories`資料表 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>步驟 4： 更新架構以使用`Picture`和`BrochurePath`資料行

`CategoriesDataTable`資料存取層 (DAL) 中目前有四個`DataColumn`s 定義： `CategoryID`， `CategoryName`， `Description`，和`NumberOfProducts`。 當我們原先設計在這個 DataTable[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，`CategoriesDataTable`只有前三個資料行;`NumberOfProducts`資料行已加入[主要/詳細資料使用的項目符號主要記錄詳細資料 DataList 清單](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)教學課程。

中所述*建立資料存取層*，具類型資料集中 Datatable 組成商務物件。 Tableadapter 負責與資料庫通訊，並填入與查詢結果的商務物件。 `CategoriesDataTable`會填入`CategoriesTableAdapter`，其具有三個資料擷取方法：

- `GetCategories()` 執行 TableAdapter s 主查詢並傳回`CategoryID`， `CategoryName`，和`Description`內的所有記錄欄位`Categories`資料表。 主要的查詢是什麼由自動產生`Insert`和`Update`方法。
- `GetCategoryByCategoryID(categoryID)` 傳回`CategoryID`， `CategoryName`，和`Description`類別目錄欄位的`CategoryID`等於*categoryID*。
- `GetCategoriesAndNumberOfProducts()` -傳回`CategoryID`， `CategoryName`，和`Description`欄位中的所有記錄`Categories`資料表。 也會使用子查詢傳回的每個類別目錄相關聯的產品數目。

請注意，其中一個項目查詢傳回`Categories`資料表 s`Picture`或`BrochurePath`資料行; 也無法`CategoriesDataTable`提供`DataColumn`s 這些欄位。 若要使用的圖片和`BrochurePath`屬性，我們需要先將它們加入`CategoriesDataTable`然後更新`CategoriesTableAdapter`類別，以傳回這些資料行。

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>加入`Picture`和`BrochurePath``DataColumn`s

加入下列兩個欄以啟動`CategoriesDataTable`。 以滑鼠右鍵按一下`CategoriesDataTable`s 標頭中，從內容功能表選取 加入，然後選擇 資料行選項。 這會建立新`DataColumn`名為 DataTable 中`Column1`。 重新命名此資料行`Picture`。 從 [屬性] 視窗中，設定`DataColumn`s`DataType`屬性`System.Byte[]`（這不是下拉式清單中的選項; 您要在輸入時）。


[![建立 DataColumn 名為圖片的資料類型是 System.Byte]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**圖 6**： 建立`DataColumn`具名`Picture`其`DataType`是`System.Byte[]`([按一下以檢視完整大小的影像](uploading-files-vb/_static/image8.png))


加入另一個`DataColumn`datatable 命名`BrochurePath`使用預設`DataType`值 (`System.String`)。

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>傳回`Picture`和`BrochurePath`TableAdapter 中的值

使用這兩個`DataColumn`加入至`CategoriesDataTable`，我們準備好更新`CategoriesTableAdapter`。 我們可能會有兩個主要的 TableAdapter 查詢，傳回這些資料行值，但這會帶回二進位資料每次`GetCategories()`叫用方法。 相反地，可讓 s 更新主要 TableAdapter 查詢以回復`BrochurePath`並建立可傳回特定類別 s 的其他資料擷取方法`Picture`資料行。

若要更新主要 TableAdapter 查詢，以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後從內容功能表選擇設定選項。 這會開啟資料表配接器組態精靈 的我們我們看到在過去的教學課程的數字。 更新查詢來回復`BrochurePath`按一下 [完成]。


[![更新資料行中的清單也會傳回 BrochurePath SELECT 陳述式](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**圖 7**： 更新中的資料行清單`SELECT`陳述式，也會傳回`BrochurePath`([按一下以檢視完整大小的影像](uploading-files-vb/_static/image10.png))


當使用 TableAdapter 臨機操作 SQL 陳述式，更新主查詢中的資料行清單的所有更新的資料行清單`SELECT`查詢的 TableAdapter 方法。 這表示`GetCategoryByCategoryID(categoryID)`已更新方法來傳回`BrochurePath`資料行，這可能是我們所預期。 不過，它也更新中的資料行清單`GetCategoriesAndNumberOfProducts()`方法，移除子查詢傳回的每個類別目錄的產品數目 ！ 因此，我們需要更新此方法的`SELECT`查詢。 以滑鼠右鍵按一下`GetCategoriesAndNumberOfProducts()`方法中，選擇 [設定]，並還原`SELECT`回其原始值的查詢：


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

接下來，建立新的 TableAdapter 方法會傳回特定類別的`Picture`資料行值。 以滑鼠右鍵按一下`CategoriesTableAdapter`s 標頭，然後選擇 加入查詢選項，以啟動 TableAdapter 查詢組態精靈。 此精靈的第一個步驟會詢問您是否我們想要使用特定 SQL 陳述式的查詢資料時，新的預存程序或現有的我的最愛。 選取 使用 SQL 陳述式，然後按一下 下一步。 因為我們將會傳回一個資料列，選擇 選取會傳回第二個步驟中的資料列的選項。


[![選取 使用 SQL 陳述式選項](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**圖 8**： 選取 使用 SQL 陳述式選項 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image12.png))


[![由於查詢會傳回一筆記錄，從 Categories 資料表，選擇選取 傳回資料列](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**圖 9**： 因為查詢會傳回一筆記錄，從 Categories 資料表中，選擇 選取會傳回資料列 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image14.png))


在第三個步驟中，輸入下列 SQL 查詢，並按一下 下一步：


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

最後一個步驟是選擇新方法的名稱。 使用`FillCategoryWithBinaryDataByCategoryID`和`GetCategoryWithBinaryDataByCategoryID`填滿 DataTable 和傳回 DataTable 模式，分別。 按一下 [完成] 5d; 以完成精靈。


[![選擇 TableAdapter 的方法的名稱](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**圖 10**： 選擇的 tableadapter 方法的名稱 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image16.png))


> [!NOTE]
> 資料表配接器查詢組態精靈完成後您可能會看到對話方塊，通知您新命令文字傳回資料，包含結構描述上的主要查詢結構描述不同。 簡單地說，精靈會建議 TableAdapter s 主查詢`GetCategories()`傳回不同的結構描述比剛才所建立。 但是，這是我們要因此您可以忽略此訊息。


此外，請記住，如果您使用特定 SQL 陳述式，並使用精靈 」 將 TableAdapter s 主查詢，在稍後的時間，它會修改`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`陳述式 s 資料行清單中包含僅擁有的資料行主要查詢 (也就是它會移除`Picture`從查詢的資料行)。 您必須手動更新資料行清單，以傳回`Picture`資料行，類似於我們與`GetCategoriesAndNumberOfProducts()`稍早在此步驟中的方法。

新增兩個後`DataColumn`s`CategoriesDataTable`和`GetCategoryWithBinaryDataByCategoryID`方法`CategoriesTableAdapter`，這些型別 DataSet 設計工具中的類別應該如下圖 11 螢幕擷取畫面。


![DataSet 設計工具包括新的資料行和方法](uploading-files-vb/_static/image11.gif)

**圖 11**: DataSet 設計工具包括新的資料行和方法


## <a name="updating-the-business-logic-layer-bll"></a>更新商務邏輯層 (BLL)

與更新 DAL，剩下的就是以增強的 「 商務邏輯層 (BLL) 」，以納入新的方法`CategoriesTableAdapter`方法。 將下列方法加入`CategoriesBLL`類別：


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>步驟 5： 上傳至 Web 伺服器從用戶端檔案

當收集二進位資料，有時候這項資料是由使用者提供。 若要擷取這項資訊，使用者必須能夠從他們的電腦到 web 伺服器將檔案上傳。 然後上傳的資料必須與資料模型，這可能表示將檔案儲存至 web 伺服器的檔案系統和加入資料庫中的檔案路徑或二進位內容直接寫入資料庫的整合。 在此步驟中我們將探討如何讓使用者從他們的電腦與伺服器的檔案上傳。 在下一個教學課程中我們將會開啟我們注意到整合資料模型上傳的檔案。

ASP.NET 2.0 的新[檔案上傳 Web 控制項](https://msdn.microsoft.com/library/ms227677(VS.80).aspx)提供一個機制，讓使用者從他們的電腦傳送檔案到 web 伺服器。 檔案上傳控制項轉譯為`<input>`項目其`type`屬性設為瀏覽器以瀏覽按鈕的文字方塊所顯示的檔案。 按一下 [瀏覽] 按鈕會顯示的對話方塊，使用者可以從中選取檔案。 回傳的表單，當選取的檔案的內容會傳送以及回傳。 伺服器端上傳檔案的相關資訊是透過檔案上傳控制內容存取。

若要上傳的檔案進行示範，請開啟`FileUpload.aspx`頁面`BinaryData`資料夾中，將檔案上傳控制項從工具箱拖曳至設計工具中，並設定控制 s`ID`屬性`UploadTest`。 接下來，加入按鈕 Web 控制項設定其`ID`和`Text`屬性`UploadButton`並分別上傳選取的檔案。 最後，將標籤 Web 控制項下方的按鈕，清除其`Text`屬性並設定其`ID`屬性`UploadDetails`。


[![將檔案上傳控制項加入 ASP.NET 網頁](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**圖 12**： 將檔案上傳控制項加入 ASP.NET 網頁 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image18.png))


圖 13 顯示透過瀏覽器檢視時，此頁面。 請注意，按一下 [瀏覽] 按鈕會開啟檔案選取對話方塊中，讓使用者選取檔案，以從他們的電腦。 一旦選取檔案後，按一下 [上傳選取的檔案] 按鈕會導致將選取的檔案 s 二進位內容傳送至 web 伺服器的回傳。


[![使用者可以選取要從他們的電腦上載至伺服器的檔案](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**圖 13**： 使用者可以選取要上傳檔案從其電腦與伺服器 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image20.png))


在回傳時上, 傳的檔案可以儲存至檔案系統，或可以直接透過資料流使用過其二進位資料。 此範例中，可讓建立`~/Brochures`資料夾並儲存已上傳的檔案。 啟動新增`Brochures`至網站根目錄的子資料夾的資料夾。 接下來，建立事件處理常式`UploadButton`s`Click`事件並加入下列程式碼：


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

檔案上傳控制項提供各種屬性，使用 上傳的資料。 比方說， [ `HasFile`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx)指出檔案是否已由使用者上傳時[`FileBytes`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx)為位元組陣列提供已上傳二進位資料的存取權。 `Click`事件處理常式一開始會確保已上傳的檔案。 如果已上傳檔案，標籤會顯示上傳的檔案，以位元組為單位，其大小和其內容類型的名稱。

> [!NOTE]
> 若要確保使用者可以檢查檔案上傳`HasFile`屬性並顯示警告，如果它 s `False`，或使用[RequiredFieldValidator 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx)改為。


檔案上傳 s`SaveAs(filePath)`將上傳的檔案儲存至指定*filePath*。 *filePath*必須*實體路徑*(`C:\Websites\Brochures\SomeFile.pdf`) 而非*虛擬**路徑*(`/Brochures/SomeFile.pdf`)。 [ `Server.MapPath(virtPath)`方法](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx)虛擬路徑，並傳回其對應的實體路徑。 此處的虛擬路徑是`~/Brochures/fileName`，其中*fileName*是上傳的檔案名稱。 請參閱[使用 Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml)如需有關虛擬和實體路徑，並使用`Server.MapPath`。

完成之後`Click`事件處理常式，請花一點時間瀏覽器中測試。 按一下瀏覽按鈕並選取您的硬碟中的檔案，然後按一下 [上傳選取檔案] 按鈕。 回傳會傳送選取的檔案內容的 web 伺服器，然後將會顯示檔案的相關資訊，然後再將它儲存至`~/Brochures`資料夾。 後上傳檔案時，返回 Visual Studio 並按一下 [方案總管] 的 [重新整理] 按鈕。 您應該會看到您剛剛上傳 ~/Brochures 資料夾中的檔案 ！


[![檔案 EvolutionValley.jpg 已上傳至 Web 伺服器](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**圖 14**: File`EvolutionValley.jpg`已上傳至 Web 伺服器 ([按一下以檢視完整大小的影像](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg 已儲存至 ~/Brochures 資料夾](uploading-files-vb/_static/image15.gif)

**圖 15**:`EvolutionValley.jpg`已儲存至`~/Brochures`資料夾


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>上傳的檔案儲存至檔案系統的微妙

有數個微妙必須處理儲存在檔案上傳至 web 伺服器的檔案系統時。 首先，那里安全性問題。 若要將檔案儲存至檔案系統中，執行 ASP.NET 網頁的安全性內容必須具有寫入權限。 目前的使用者帳戶的內容下執行 ASP.NET 開發 Web 伺服器。 如果您使用 Microsoft 的 Internet Information Services (IIS) 與網頁伺服器，安全性內容取決於 IIS 及其組態的版本。

另一項挑戰，將檔案儲存至檔案系統的檔案命名為中心。 目前，我們的頁面會儲存要上傳的檔案的所有`~/Brochures`目錄為 s 的用戶端電腦上的檔案使用相同的名稱。 如果使用者 A 將上傳名稱冊`Brochure.pdf`，檔案會儲存為`~/Brochure/Brochure.pdf`。 但如果一段時間以後的使用者 B 將檔案上傳不同冊剛好有相同的檔名 (`Brochure.pdf`)？ 使用程式碼我們現在已經 s 檔案就會覆寫使用者 B s 上傳的使用者。

有一些技術來解決檔案名稱衝突。 其中一個選項是禁止上傳檔案，如果已經有一個具有相同名稱。 使用這個方法，當使用者 B 嘗試上傳檔案，名為`Brochure.pdf`，系統不會儲存其檔案，並不會顯示訊息，通知使用者 B 重新命名檔案，再試一次。 另一個方法是使用唯一的檔案名稱，這可能是儲存檔案[全域唯一識別碼 (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)或從對應的資料庫記錄 s 主索引鍵資料行的值 (假設上傳相關聯特定資料列的資料模型中）。 在下一個教學課程中，我們將探討這些選項更多詳細資料。

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>處理非常大量的二進位資料的挑戰

這些教學課程假設擷取二進位資料的大小太大。 使用非常大量的二進位資料檔案的數個 mb 或更大導入了新的挑戰，已超出這些教學課程的範圍。 例如，根據預設 ASP.NET 將會拒絕上的傳超過 4 mb，雖然這可以透過設定[`<httpRuntime>`元素](https://msdn.microsoft.com/library/e1f13641.aspx)中`Web.config`。 IIS 會為它自己的檔案上傳大小的限制，太。 請參閱[IIS 上傳檔案大小](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html)如需詳細資訊。 此外，若要上傳大型檔案所花費的時間可能會 110 ASP.NET 會等待要求的秒數超過預設值。 另外還有處理大型檔案時，會發生的記憶體和效能問題。

大型檔案上傳的檔案上傳控制項不切實際。 檔案的內容張貼至伺服器，因為使用者必須耐心等待不用任何確認其上傳的進度。 處理較小的檔案可以在幾秒鐘上, 傳，但可以處理較大可能需要分鐘來上傳的檔案時，會發生問題時，這是不太多問題。 有各種不同的第三方檔案較適合處理大量上傳的上傳控制項，而且許多這些廠商提供進度指標和 ActiveX 上傳提供更精美的使用者經驗的管理員。

如果您的應用程式必須處理大型檔案，您必須謹慎調查挑戰，並尋找您的特定需求的合適解決方案。

## <a name="summary"></a>總結

建置應用程式來擷取二進位資料需要導入了一些挑戰。 我們在本教學課程已探索的前兩個： 決定儲存二進位資料的位置，並讓使用者透過網頁的二進位內容上傳。 在接下來三個教學課程中，我們會看到如何將上傳的資料與資料庫中的記錄產生關聯，以及如何顯示其文字資料欄位旁邊的二進位資料。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [使用大數值資料類型](https://msdn.microsoft.com/library/ms178158.aspx)
- [檔案上傳控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 的檔案上傳伺服器控制項](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [在檔案上傳深色的一端](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和 Bernadette Leigh。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](updating-and-deleting-existing-binary-data-cs.md)
> [下一頁](displaying-binary-data-in-the-data-web-controls-vb.md)
