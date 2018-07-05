---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: 二進位資料顯示在資料 Web 控制項 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們看看要呈現在網頁上，包括影像檔的顯示和 [下載] 連結 f 的佈建的二進位資料的選項...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 62cc931b670931677b4e9632dccd6634715b3c71
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814850"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>顯示的二進位資料，以資料 Web 控制項 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe)或[下載 PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> 在本教學課程，我們看看要呈現在網頁上，包括影像檔的顯示和佈建的 [下載] 連結，PDF 檔案的二進位資料的選項。


## <a name="introduction"></a>簡介

在先前的教學課程中我們探討兩種技術，來將關聯的應用程式 s 基礎資料模型中的二進位資料，並使用 FileUpload 控制項從瀏覽器到 web 伺服器檔案系統中的檔案上傳。 我們尚未以了解如何上傳的二進位資料相關聯的資料模型發生。 也就是說，檔案已上傳並儲存至檔案系統之後，檔案的路徑必須儲存於適當的資料庫記錄。 如果直接在資料庫中儲存資料上, 傳的二進位資料就不需要儲存至檔案系統中，檔案，但必須插入資料庫中。

我們查看關聯的資料模型中的資料之前，不過，可讓 s 先看看如何為使用者提供的二進位資料。 呈現的文字資料是相當簡單，但應該如何二進位資料呈現？ 其類型而定，當然，二進位資料。 針對映像，我們可能想要顯示影像，Pdf、 Microsoft Word 文件、 ZIP 檔和其他類型的二進位資料，將下載連結提供對於可能會比較適合。

在本教學課程，我們將探討如何呈現及使用資料及其相關聯的文字資料的二進位資料 Web 控制項 GridView 和 DetailsView 等。 在下一個教學課程中我們將把焦點轉到將上傳的檔案與資料庫產生關聯。

## <a name="step-1-providingbrochurepathvalues"></a>步驟 1： 提供`BrochurePath`值

`Picture`中的資料行`Categories`資料表已包含各種類別目錄映像的二進位資料。 具體來說，`Picture`每一筆記錄的資料行具有有顆粒狀、 低品質、 16 色點陣圖影像的二進位內容。 每個類別目錄影像為 172 個像素寬和 120 像素，高度，並取用大約 11 KB。 更多哪些 s 中的二進位內容`Picture`資料行包含 78 位元組[OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding)必須顯示映像前移除的標頭。 此標頭資訊會出現，因為 Northwind 資料庫在 Microsoft Access 有自己的根目錄。 在 Access 中，是使用 OLE 物件的資料類型，此標頭會添加到儲存二進位資料。 現在，我們會看到如何移除這些低品質影像中的標頭，以顯示圖片。 在未來的教學課程中，我們會建置用於更新類別目錄的介面`Picture`資料行，並取代這些對等的 JPG 影像，而不需要的不必要的 OLE 標頭搭配使用 OLE 標頭的點陣圖影像。

在先前的教學課程中，我們了解如何使用 FileUpload 控制項的內容。 因此，您可以繼續，並將摺頁冊檔案新增至 web 伺服器檔案系統。 這種方式，不過，不會更新`BrochurePath`中的資料行`Categories`資料表。 在下一個教學課程中，我們將看到如何達成此目的，但現在我們需要以手動方式提供值，這個資料行。

在此教學課程的下載中，您會發現在七個 PDF 摺頁冊檔案`~/Brochures`資料夾中，各個 Seafood 以外的類別。 我故意省略新增 Seafood 摺頁冊，說明如何處理不是所有記錄具有相關都聯的二進位資料的案例。 若要更新`Categories`資料表包含下列的值，以滑鼠右鍵按一下`Categories`從伺服器總管] 節點，然後選擇 [顯示資料表資料。 然後，輸入每個類別目錄具有摺頁冊，如 圖 1 所示摺頁冊檔案虛擬路徑。 因為沒有任何手冊 Seafood 分類，保留其`BrochurePath`做為資料行的值`NULL`。


[![以手動方式輸入類別目錄資料表的 BrochurePath 資料行的值](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**圖 1**： 以手動方式輸入的值`Categories`表格 s`BrochurePath`資料行 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>步驟 2： 將下載連結提供如 GridView 中本小手冊

具有`BrochurePath`值提供給`Categories`資料表中，我們準備好要建立的 GridView 會列出每個類別，以及下載類別目錄的手冊的連結。 在步驟 4 中，我們會擴大此 GridView，以也會顯示類別 s 的映像。

開始從工具箱拖曳至設計工具的拖曳的 GridView`DisplayOrDownloadData.aspx`頁面中`BinaryData`資料夾。 設定 GridView s`ID`至`Categories`透過 GridView s 智慧標籤，選擇 繫結至新的資料來源。 具體而言，將它繫結至名為 ObjectDataSource `CategoriesDataSource` ，它會擷取使用資料`CategoriesBLL`物件的`GetCategories()`方法。


[![建立名為 CategoriesDataSource 新 ObjectDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`CategoriesDataSource`([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![設定使用 CategoriesBLL 類別 ObjectDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![擷取使用 GetCategories() 方法的類別目錄的清單](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**圖 4**： 擷取清單的類別使用`GetCategories()`方法 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會自動加入至 BoundField`Categories`如 GridView `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath` `DataColumn` s。 請繼續並移除`NumberOfProducts`自 BoundField`GetCategories()`方法的查詢不會擷取這項資訊。 也會移除`CategoryID`BoundField 和重新命名`CategoryName`並`BrochurePath`BoundFields`HeaderText`屬性類別目錄] 和 [摺頁冊，分別。 進行這些變更之後，您的 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

檢視此頁面，透過瀏覽器 （請參閱 [圖 5]）。 每個八個類別目錄會列出。 有七個類別`BrochurePath`值有`BrochurePath`個別 BoundField 中顯示的值。 Seafood，其具有`NULL`值及其`BrochurePath`，會顯示空白儲存格。


[![列出每個類別的名稱、 描述和 BrochurePath 值](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**圖 5**： 每個類別 s 的名稱，描述，並`BrochurePath`值都會列 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


而不是顯示的文字`BrochurePath` 欄中，我們想要建立手冊的連結。 若要這麼做，請移除`BrochurePath`BoundField 和 HyperLinkField 取代它。 設定新的 HyperLinkField s`HeaderText`屬性來摺頁冊，其`Text`屬性，以檢視摺頁冊，並將其`DataNavigateUrlFields`屬性設`BrochurePath`。


![新增 BrochurePath HyperLinkField](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**圖 6**： 新增的 HyperLinkField `BrochurePath`


如 [圖 7] 所示，這會新增至 GridView，連結的資料行。 按一下檢視摺頁冊連結將直接在瀏覽器中顯示的 PDF 或提示使用者下載檔案，取決於是否已安裝 PDF 閱讀程式和瀏覽器的設定。


[![按一下 [檢視] 摺頁冊連結也可以檢視類別的手冊](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**圖 7**： 利用類別的手冊可以檢視依序按一下 [檢視] 摺頁冊連結 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![隨即出現類別目錄 s 摺頁冊 PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**圖 8**： 顯示摺頁冊 PDF 類別 s ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>隱藏檢視摺頁冊文字不摺頁冊類別

如 [圖 7] 所示， `BrochurePath` HyperLinkField 顯示其`Text`屬性值 （檢視摺頁冊） 的所有記錄，不論是否那里 s 非`NULL`值`BrochurePath`。 當然，如果`BrochurePath`是`NULL`，連結會顯示為文字，然後在此情況下，與 Seafood 分類 （請參閱上一步 圖 7）。 而不是顯示的文字檢視摺頁冊，可能是很好有這些類別，而不需要`BrochurePath`顯示一些替代的文字，例如無摺頁冊可用的值。

若要提供這種行為，我們要使用其內容會透過根據適當的輸出就會發出的頁面方法的呼叫產生 TemplateField`BrochurePath`值。 我們先探討這格式化技術年代[使用 GridView 控制項中的 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程。

選取將 HyperLinkField 變成 TemplateField `BrochurePath` HyperLinkField，然後按一下 轉換此欄位為 TemplateField 到 編輯資料行 對話方塊中的連結。


![HyperLinkField 轉換為 TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**圖 9**: HyperLinkField 轉換為 TemplateField


這會建立使用 TemplateField`ItemTemplate`包含超連結 Web 控制項`NavigateUrl`屬性的繫結至`BrochurePath`值。 將此標記方法的呼叫取代`GenerateBrochureLink`，並傳入的值`BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

接下來，建立`protected`方法在 asp.net 頁面上名為 s 程式碼後置類別`GenerateBrochureLink`會傳回`string`並接受`object`做為輸入參數。


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

這個方法會判斷傳入`object`值是資料庫`NULL`而且，如果是的話，會傳回訊息，指出類別缺少摺頁冊。 否則，如果沒有`BrochurePath`值，它顯示超連結。 請注意，如果`BrochurePath`值是呈現 s 傳入[`ResolveUrl(url)`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)。 這個方法會解析傳入*url*，並將`~`字元搭配適當的虛擬路徑。 例如，如果應用程式根目錄`/Tutorial55`，`ResolveUrl("~/Brochures/Meats.pdf")`會傳回`/Tutorial55/Brochures/Meat.pdf`。

套用這些變更之後，圖 10 顯示頁面。 請注意，Seafood 類別的`BrochurePath`欄位現在會顯示沒有手冊提供的文字。


[![文字不摺頁冊提供顯示這些類別不摺頁冊](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**圖 10**: 文字否手冊提供顯示這些類別不摺頁冊 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>步驟 3： 加入網頁顯示類別 s 的圖片

當使用者造訪 ASP.NET 網頁時，他們會收到 ASP.NET 頁面的 HTML。 收到的 HTML 也只是文字，而且不包含任何二進位資料。 任何其他的二進位資料，例如影像、 音效檔、 Macromedia Flash 應用程式、 內嵌 Windows Media Player 的視訊，等等，web 伺服器上的個別資源的形式存在。 HTML 會包含這些檔案中，參考，但不包含檔案的實際內容。

例如，在 HTML`<img>`元素用來參考一張圖片，`src`指向影像檔的屬性就像這樣：


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

當瀏覽器收到此 HTML 時，它會發出另一個要求至 web 伺服器，以擷取該影像檔，接著會顯示在瀏覽器中的二進位內容。 相同的概念適用於任何二進位資料。 在步驟 2 中，摺頁冊未向下傳送至瀏覽器頁面的 HTML 標記的一部分。 相反地，轉譯的 HTML 時，提供超連結，按一下，造成瀏覽器直接要求 PDF 文件。

若要顯示或允許使用者下載位於資料庫中的二進位資料，我們需要建立個別的網頁傳回的資料。 我們的應用程式，那裡 s 直接儲存在資料庫類別的圖只有一個二進位資料欄位。 因此，我們需要一個頁面時，呼叫時，會傳回特定類別的影像資料。

加入新的 ASP.NET 頁面，以便`BinaryData`名為資料夾`DisplayCategoryPicture.aspx`。 當這種方式，將選取的主版頁面核取方塊未核取。 此頁面所預期`CategoryID`中的查詢字串並傳回該類別 s 的二進位資料值`Picture`資料行。 此頁面會傳回二進位資料，並且沒有其他項目，所以不需要任何 HTML 一節中的標記。 因此，按一下左下角的 [來源] 索引標籤上，並移除所有的頁面的標記以外的`<%@ Page %>`指示詞。 也就是`DisplayCategoryPicture.aspx`s 宣告式標記應該包含單一行：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

如果您看到`MasterPageFile`屬性中`<%@ Page %>`指示詞，將它移除。

在頁面 s 程式碼後置類別中加入下列程式碼`Page_Load`事件處理常式：


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

此程式碼開始讀取`CategoryID`查詢字串值放入變數，名為`categoryID`。 接下來，將圖片會擷取資料，透過對呼叫`CategoriesBLL`類別的`GetCategoryWithBinaryDataByCategoryID(categoryID)`方法。 這項資料時，會傳回至用戶端上，使用`Response.BinaryWrite(data)`方法，但這在呼叫之前，`Picture`必須移除資料行值的 OLE 標頭。 這透過建立`byte`名為陣列`strippedImageData`將保留精確 78 字元小於功能的`Picture`資料行。 [ `Array.Copy`方法](https://msdn.microsoft.com/library/z50k9bft.aspx)用來將資料從複製`category.Picture`位置 78 從頭開始來`strippedImageData`。

`Response.ContentType`屬性會指定[MIME 類型](http://en.wikipedia.org/wiki/MIME)，讓瀏覽器知道如何呈現它所傳回的內容。 由於`Categories`表格的`Picture`資料行的點陣圖影像，點陣圖 MIME 型別使用這裡 (image/bmp)。 如果您省略的 MIME 類型，大部分的瀏覽器仍會顯示映像正確因為它們可以推斷為基礎的映像檔案 s 的二進位資料內容的型別。 不過，它包含 MIME 容錯度加倍 s 輸入，可能的話。 請參閱[Internet Assigned Numbers Authority 的網站](http://www.iana.org/)如需完整的清單[MIME 媒體類型](http://www.iana.org/assignments/media-types/)。

建立此頁面上，使用特定類別的圖片可以檢視瀏覽`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 [圖 11] 顯示飲料類別的圖片，您可以從檢視`DisplayCategoryPicture.aspx?CategoryID=1`。


[![圖片顯示 s 的飲料類別目錄](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**圖 11**： 顯示圖片的飲料 」 分類 s ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


如果瀏覽時`DisplayCategoryPicture.aspx?CategoryID=categoryID`，您會將型別的物件轉換為類型 'System.Byte []' 的 ' System.DBNull' 讀取無法發生例外狀況，有可能會造成這兩件事。 首先，`Categories`表格 s`Picture`資料行允許`NULL`值。 `DisplayCategoryPicture.aspx`  頁面上，不過，假設沒有非`NULL`目前的值。 `Picture`的屬性`CategoriesDataTable`無法直接存取其`NULL`值。 如果您想要允許`NULL`值`Picture`d 您想要包含下列條件的資料行：


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

上述程式碼假設一些映像名為的檔案，就 s`NoPictureAvailable.gif`在`Images`您想要顯示這些類別，而不需要一張圖片的資料夾。

如果，可能也會造成這個例外狀況`CategoriesTableAdapter`s`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`陳述式已還原回到主查詢 s 資料行清單，如果您使用特定 SQL 陳述式，而且您已重新執行精靈，供使用的 TableAdapter 秒就會發生主要的查詢。 檢查，確定`GetCategoryWithBinaryDataByCategoryID`方法 s`SELECT`陳述式仍包含`Picture`資料行。

> [!NOTE]
> 每次`DisplayCategoryPicture.aspx`是瀏覽、 存取資料庫，並傳回指定的分類的圖片資料。 如果自使用者上次檢視過它，就會變更類別的圖片項目 t，不過，這會是浪費的精力。 幸運的是，HTTP 是用來*條件式取得*。 使用條件式 GET，提出 HTTP 要求的用戶端會傳送沿著[ `If-Modified-Since` HTTP 標頭](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)所提供的日期和上次從 web 伺服器擷取這項資源的用戶端的時間。 如果內容尚未變更，因為這會指定日期，web 伺服器可能回應[不會修改 (304) 的狀態碼](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)並放棄傳送回要求的資源的內容。 簡單地說，這項技術會減輕不必傳送資源的內容，如果它尚未修改用戶端上次存取後的 web 伺服器。


若要實作此行為，不過，會要求您新增`PictureLastModified`資料行`Categories`資料表擷取時`Picture`用以檢查程式碼以及上次更新資料行`If-Modified-Since`標頭。 如需詳細資訊`If-Modified-Since`標頭 」 和 「 條件式 GET 工作流程，請參閱[HTTP 條件式 GET RSS 駭客](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)並[更深入了解 ASP.NET 網頁中執行 HTTP 要求](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx)。

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>步驟 4: GridView 中顯示類別目錄的圖片

既然我們已經以顯示特定分類的圖片的網頁時，我們可以將它顯示使用[映像的 Web 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx)或 HTML`<img>`指向的項目`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 GridView 或使用 ImageField DetailsView 中，可以顯示其 URL 取決於資料庫資料的映像。 包含 ImageField`DataImageUrlField`及`DataImageUrlFormatString`HyperLinkField s 類似的屬性`DataNavigateUrlFields`和`DataNavigateUrlFormatString`屬性。

讓 s 擴大`Categories`GridView 中的`DisplayOrDownloadData.aspx`加 ImageField 來顯示每個類別目錄的圖片。 只需新增 ImageField，設定其`DataImageUrlField`並`DataImageUrlFormatString`屬性，以`CategoryID`和`DisplayCategoryPicture.aspx?CategoryID={0}`分別。 這會建立 GridView 資料行，會呈現`<img>`項目其`src`屬性參考`DisplayCategoryPicture.aspx?CategoryID={0}`，其中{0}GridView 資料列 s 取代`CategoryID`值。


![新增 ImageField 至 GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**圖 12**: ImageField 加入 GridView


在新增之後 ImageField，GridView s 的宣告式語法看起來應該像 soothe 下列：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

請花一點時間才能檢視此頁面，透過瀏覽器。 請注意如何每一筆記錄現在包含類別目錄的圖片。


[![類別的圖片會顯示每個資料列](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**圖 13**: 類別的圖片會顯示每個資料列 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>總結

在本教學課程中，我們檢查如何將二進位資料呈現。 呈現資料的方式取決於資料的型別。 針對 PDF 小冊子檔案中，我們提供使用者檢視摺頁冊連結，按一下時，直接將使用者用以 PDF 檔案。 為類別 s 的圖片，我們將第一次建立頁面，即可擷取，並從資料庫傳回二進位資料，然後該頁面中用來顯示每個類別的圖片 GridView。

現在我們已討論過如何顯示二進位資料，我們準備好要檢查如何執行插入、 更新和刪除針對資料庫使用的二進位資料。 在下一個教學課程中我們將探討如何關聯其對應的資料庫記錄上傳的檔案。 在本教學課程之後，我們會看到如何更新現有的二進位資料，以及如何移除其相關聯的記錄時，刪除的二進位資料。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa Murphy 和 Dave Gardner。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](uploading-files-cs.md)
> [下一頁](including-a-file-upload-option-when-adding-a-new-record-cs.md)
