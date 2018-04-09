---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: 二進位資料顯示在資料 Web 控制項 (C#) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中我們查看要呈現在網頁上，包括顯示的映像檔和 [下載] 5d 連結 f 的佈建的二進位資料的選項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: c5b56fc45ea8fb5aee934530fc62e23b9d364242
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>二進位資料顯示在 Web 控制項的資料 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe)或[下載 PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> 在本教學課程中我們查看要呈現在網頁上，包括顯示的映像檔和 PDF 檔案的 [下載] 5d 連結提供的二進位資料的選項。


## <a name="introduction"></a>簡介

前述教學課程中我們會瀏覽關聯的應用程式 s 基礎資料模型中的二進位資料的兩個技術，用來從瀏覽器到 web 伺服器的檔案系統中的檔案上傳的檔案上傳控制項。 我們尚未以了解如何上傳二進位資料關聯的資料模型發生。 也就是說，檔案已上傳並儲存至檔案系統之後，檔案的路徑必須儲存於適當的資料庫記錄。 如果直接在資料庫中儲存資料時上, 傳二進位資料不需要儲存至檔案系統中，但必須插入資料庫。

我們會審視關聯的資料模型中的資料之前，不過，可讓 s 先看看如何向使用者提供的二進位資料。 呈現文字資料相當簡單，但應該二進位資料會呈現的方式？ 其類型而定，當然，二進位資料。 針對映像，我們可能想要顯示的影像。Pdf，如 Microsoft Word 文件、 ZIP 檔案和其他類型的二進位資料，提供的下載連結可能更適合。

在此教學課程中我們將探討如何一起使用的資料及其相關聯的文字資料的二進位資料的呈現 Web 控制項 GridView 和 DetailsView 等。 在下一個教學課程中我們將會開啟我們注意到將上傳的檔案與資料庫產生關聯。

## <a name="step-1-providingbrochurepathvalues"></a>步驟 1： 提供`BrochurePath`值

`Picture`中的資料行`Categories`資料表已包含二進位資料的各種類別映像。 具體來說，`Picture`每一筆記錄的資料行保有有顆粒狀、 低品質、 16 色點陣圖影像的二進位內容。 每個類別目錄影像 172 像素寬和 120 像素，高，會取用大約 11 KB。 多個哪些 s 中的二進位內容`Picture`資料行包括 78 位元組[OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding)必須顯示映像前移除的標頭。 因為 Microsoft Access 中的 Northwind 資料庫已經使用其根，在於使用這個標頭資訊。 在 Access 中，是使用 OLE 物件的資料類型，此標頭 tacks 儲存二進位資料。 現在，我們會看到如何刪除標頭，從這些低品質映像以顯示圖片。 在未來的教學課程中，我們會建置更新 s 類別的介面`Picture`資料行，並取代這些對等的 JPG 影像，不含不必要的 OLE 標頭搭配使用 OLE 標頭的點陣圖影像。

在前述教學課程中，我們看到如何使用檔案上傳控制項。 因此，您可以繼續，並將冊檔案加入至 web 伺服器的檔案系統。 這樣做，不過，不會更新`BrochurePath`中的資料行`Categories`資料表。 在下一個教學課程中，我們會看到如何達成此目的，但現在，我們需要以手動方式提供值，這個資料行。

在此教學課程的下載中，您會發現七個 PDF 冊檔案中的`~/Brochures`資料夾中，一個用於每個除了 Seafood 類別。 我故意省略加入 Seafood 冊，說明如何處理案例，並非所有的記錄具有相關聯的二進位資料。 若要更新`Categories`表格包含下列的值，以滑鼠右鍵按一下`Categories`節點從 伺服器總管，然後選擇 顯示資料表資料。 然後，輸入具有冊，如圖 1 所示的每個分類的冊檔案虛擬路徑。 因為沒有任何冊 Seafood 類別，將保留其`BrochurePath`做為資料行的值`NULL`。


[![手動輸入類別目錄資料表的 BrochurePath 資料行的值](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**圖 1**： 以手動方式輸入的值`Categories`資料表 s`BrochurePath`資料行 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>步驟 2： 提供中 GridView 冊下載連結

與`BrochurePath`值提供給`Categories`資料表中，我們已備妥可建立會列出每個類別目錄，以及連結下載類別目錄的冊的 GridView re。 步驟 4 中，我們將會延長此 GridView 也會顯示類別目錄 s 映像。

開始從工具箱拖曳至設計工具的拖曳 GridView`DisplayOrDownloadData.aspx`頁面`BinaryData`資料夾。 設定 GridView s`ID`至`Categories`透過 GridView s 智慧標籤，選擇 繫結至新的資料來源。 具體而言，繫結到名為 ObjectDataSource`CategoriesDataSource`會擷取使用資料`CategoriesBLL`物件的`GetCategories()`方法。


[![建立名為 CategoriesDataSource 新 ObjectDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**圖 2**： 建立新的 ObjectDataSource 具名`CategoriesDataSource`([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![設定使用 CategoriesBLL 類別 ObjectDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**圖 3**： 設定要使用 ObjectDataSource`CategoriesBLL`類別 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![擷取使用 GetCategories() 方法的類別目錄的清單](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**圖 4**： 擷取清單的類別使用`GetCategories()`方法 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會自動加入至 BoundField`Categories`的 GridView `CategoryID`， `CategoryName`， `Description`， `NumberOfProducts`，和`BrochurePath` `DataColumn` s。 請繼續進行，並移除`NumberOfProducts`自 BoundField`GetCategories()`方法的查詢不會擷取這項資訊。 也會移除`CategoryID`BoundField 並重新命名`CategoryName`和`BrochurePath`BoundFields`HeaderText`屬性，以分類和冊，分別。 進行這些變更之後，您 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

檢視此頁面，透過瀏覽器 （請參閱圖 5）。 每個八個類別目錄會列出。 七個類別`BrochurePath`值有`BrochurePath`個別 BoundField 中顯示的值。 Seafood 具有`NULL`值及其`BrochurePath`，會顯示空白資料格。


[![列出每個類別目錄的名稱、 描述和 BrochurePath 值](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**圖 5**： 每個類別的名稱、 描述和`BrochurePath`值都會列 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


而不是顯示的文字`BrochurePath`資料行中，我們想要建立冊的連結。 若要這麼做，請移除`BrochurePath`BoundField 並取代 HyperLinkField。 設定新 HyperLinkField s`HeaderText`屬性冊，其`Text`屬性，以檢視冊及其`DataNavigateUrlFields`屬性`BrochurePath`。


![加入 BrochurePath HyperLinkField](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**圖 6**： 新增的 HyperLinkField `BrochurePath`


如圖 7 所示，這會新增至 GridView，連結的資料行。 按一下檢視冊連結將會是直接在瀏覽器中顯示 PDF 或提示使用者下載的檔案，根據是否已安裝 PDF 閱讀程式和瀏覽器的設定。


[![按一下 [檢視] 冊連結也可以檢視分類的冊](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**圖 7**: 類別的冊可檢視依序按一下 [檢視] 冊連結 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![會顯示類別目錄的冊 PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**圖 8**： 冊 PDF 會顯示類別 s ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>隱藏檢視冊文字沒有冊類別

如圖 7 所示， `BrochurePath` HyperLinkField 顯示其`Text`屬性值 （檢視冊） 的所有記錄，而不論是否那里 s 非`NULL`值`BrochurePath`。 當然，如果`BrochurePath`是`NULL`，連結會顯示為文字，然後使用 Seafood 類別案例 （請參閱上一步圖 7）。 而不是顯示的文字檢視冊，可能是很好具有這些類別，而不`BrochurePath`值顯示一些替代文字，例如沒有冊可用。

若要提供這種行為，我們要使用透過頁面的方法，會發出適當的輸出為基礎的呼叫產生其內容為 TemplateField`BrochurePath`值。 我們第一次瀏覽此格式的技術回[GridView 控制項中的使用 TemplateFields](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md)教學課程。

TemplateField HyperLinkField 變成選取`BrochurePath`HyperLinkField，然後按一下 轉換此欄位為 TemplateField 到 編輯資料行 對話方塊中的連結。


![HyperLinkField 轉換為 TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**圖 9**: HyperLinkField 轉換為 TemplateField


這會建立具有為 TemplateField`ItemTemplate`包含超連結 Web 控制項`NavigateUrl`屬性繫結至`BrochurePath`值。 這個標記取代方法的呼叫`GenerateBrochureLink`的值傳遞給`BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

接下來，建立`protected`方法在 ASP.NET 頁面上名為 s 程式碼後置類別`GenerateBrochureLink`傳回`string`並接受`object`做為輸入參數。


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

這個方法會判斷傳入`object`值是資料庫`NULL`而且，如果是的話，會傳回訊息，指出類別是否缺少冊。 否則，如果沒有`BrochurePath`值，它顯示超連結。 請注意，如果`BrochurePath`值是呈現 s 傳入[`ResolveUrl(url)`方法](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx)。 這個方法會將傳入的解析*url*，並將`~`字元搭配適當的虛擬路徑。 例如，如果應用程式根目錄`/Tutorial55`，`ResolveUrl("~/Brochures/Meats.pdf")`會傳回`/Tutorial55/Brochures/Meat.pdf`。

套用這些變更之後，圖 10 說明頁面。 請注意，Seafood 類別的`BrochurePath`欄位現在會顯示沒有冊可用的文字。


[![文字沒有冊可用為顯示那些類別沒有冊](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**圖 10**: 文字沒有冊可用的顯示的那些類別沒有冊 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>步驟 3： 加入 Web 頁面的顯示類別 s 的圖片

當使用者造訪 ASP.NET 網頁時，他們會收到 ASP.NET 的 HTML 頁面。 接收的 HTML 只不過是文字，並不包含任何的二進位資料。 當做 web 伺服器上的個別資源存在任何其他的二進位資料，例如影像、 音效檔、 Macromedia Flash 應用程式、 內嵌 Windows Media Player 影片和其他等等。 HTML 會包含這些檔案的參考，但不包含檔案的實際內容。

例如，在 HTML`<img>`元素用來參考與圖片，`src`指向影像檔的屬性，就像這樣：


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

當瀏覽器收到此 HTML 時，它會發出另一個要求至 web 伺服器，以擷取映像檔，接著會顯示在瀏覽器中的二進位內容。 相同的概念適用於任何的二進位資料。 在步驟 2 中冊未向下傳送至瀏覽器頁面的 HTML 標記的一部分。 相反地，轉譯的 HTML 提供超連結，在按下時，造成瀏覽器直接要求 PDF 文件。

若要顯示或允許使用者下載存在於資料庫中的二進位資料，我們需要建立個別的網頁，傳回的資料。 應用程式，那里 s 直接儲存在資料庫類別的圖片只能有一個二進位資料欄位。 因此，我們需要頁面時，呼叫時，傳回特定類別目錄的影像資料。

加入新的 ASP.NET 網頁的`BinaryData`資料夾名為`DisplayCategoryPicture.aspx`。 當此情況下，核取 選取的主版頁面的核取方塊。 此頁面所預期`CategoryID`中查詢字串並傳回該類別 s 的二進位資料值`Picture`資料行。 此頁面會傳回二進位資料，而無其他內容，所以不需要任何 HTML 區段中的標記。 因此，按一下左上角的 [來源] 索引標籤上，移除網頁的標記以外的所有`<%@ Page %>`指示詞。 也就是說， `DisplayCategoryPicture.aspx` s 宣告式標記應該包含單一行：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

如果您看到`MasterPageFile`屬性`<%@ Page %>`指示詞，將它移除。

在頁面 s 程式碼後置類別中，加入下列程式碼加入`Page_Load`事件處理常式：


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

此程式碼開始讀取`CategoryID`querystring 值到變數，名為`categoryID`。 接下來，圖片擷取的資料，透過對呼叫`CategoriesBLL`類別的`GetCategoryWithBinaryDataByCategoryID(categoryID)`方法。 這項資料由用戶端使用`Response.BinaryWrite(data)`方法，但之前呼叫，`Picture`必須移除資料行值的 OLE 標頭。 這可以藉由建立`byte`名為陣列`strippedImageData`將保留精確 78 字元小於功能的`Picture`資料行。 [ `Array.Copy`方法](https://msdn.microsoft.com/library/z50k9bft.aspx)用來將資料從複製`category.Picture`開始位置 78 透過`strippedImageData`。

`Response.ContentType`屬性會指定[MIME 類型](http://en.wikipedia.org/wiki/MIME)如此瀏覽器可得知如何呈現它所傳回的內容。 因為`Categories`資料表的`Picture`資料行是點陣圖影像、 點陣圖 MIME 類型會使用這裡 (image/bmp)。 如果您省略的 MIME 類型，大部分的瀏覽器仍會顯示影像正確因為它們可以推斷型別根據映像檔案 s 的二進位資料的內容。 不過，它必須包含 MIME s 輸入盡可能。 請參閱[Internet Assigned Numbers Authority 的網站](http://www.iana.org/)的完整清單[MIME 媒體類型](http://www.iana.org/assignments/media-types/)。

與建立此頁面，可以檢視特定分類的圖片造訪`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 圖 11 顯示飲料類別的圖片，可以從檢視`DisplayCategoryPicture.aspx?CategoryID=1`。


[![圖片顯示飲料 」 分類 s](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**圖 11**: 飲料 」 分類的圖片顯示 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


如果瀏覽時`DisplayCategoryPicture.aspx?CategoryID=categoryID`取得無法讀取類型的物件轉換至 'System.DBNull' type 'System.Byte []' 的例外狀況，可能會造成這兩項。 首先，`Categories`資料表 s`Picture`資料行允許`NULL`值。 `DisplayCategoryPicture.aspx`  頁面上，不過，假設有非`NULL`目前的值。 `Picture`屬性`CategoriesDataTable`無法在有直接存取`NULL`值。 如果您希望允許`NULL`值`Picture`d 您想要包含下列條件的資料行：


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

上述程式碼會假設那里 s 某些影像檔名為`NoPictureAvailable.gif`中`Images`您想要顯示這些類別沒有圖片的資料夾。

這個例外狀況也可能造成如果`CategoriesTableAdapter`s`GetCategoryWithBinaryDataByCategoryID`方法的`SELECT`陳述式已還原回主查詢 s 資料行清單，如果您使用特定 SQL 陳述式，而且您已重新執行的 tableadapter 精靈就會發生主要的查詢。 檢查以確定`GetCategoryWithBinaryDataByCategoryID`方法 s`SELECT`陳述式仍然包含`Picture`資料行。

> [!NOTE]
> 每次`DisplayCategoryPicture.aspx`是瀏覽、 存取資料庫，並傳回指定之分類的圖片資料。 如果類別的圖片未經 t 變更自使用者上次檢視過它，不過，這是浪費。 幸運的是，HTTP 允許*條件取得*。 使用條件式 GET，提出 HTTP 要求的用戶端會傳送沿[ `If-Modified-Since` HTTP 標頭](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)提供的日期和時間，上次從 web 伺服器擷取這項資源的用戶端。 內容尚未變更，因為這會指定日期，如果可能回應的 web 伺服器[未修改 (304) 的狀態碼](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)並且放棄傳送回要求的資源的內容。 簡單地說，這項技術可減輕傳送資源的內容，如果它尚未修改因為用戶端上次被存取的 web 伺服器。


若要實作這種行為，不過，會要求您加入`PictureLastModified`資料行`Categories`要擷取當資料表`Picture`用以檢查程式碼以及上次更新資料行`If-Modified-Since`標頭。 如需有關`If-Modified-Since`標頭和條件式 GET 工作流程，請參閱[HTTP 條件式 GET RSS 駭客](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers)和[更深入查看執行中的 ASP.NET 網頁的 HTTP 要求](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx)。

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>步驟 4： 在 GridView 中顯示的類別目錄的圖片

現在，我們已經顯示特定類別的圖片的網頁時，我們就可以顯示使用[映像 Web 控制項](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx)或 HTML`<img>`項目指向`DisplayCategoryPicture.aspx?CategoryID=categoryID`。 GridView 或使用 ImageField DetailsView 中可以顯示的影像的 URL 由資料庫資料。 ImageField 包含`DataImageUrlField`和`DataImageUrlFormatString`屬性可如 HyperLinkField s`DataNavigateUrlFields`和`DataNavigateUrlFormatString`屬性。

將 s 加強`Categories`GridView 中的`DisplayOrDownloadData.aspx`加 ImageField 來顯示每個類別 s 的圖片。 只要加入 ImageField 並設定其`DataImageUrlField`和`DataImageUrlFormatString`屬性`CategoryID`和`DisplayCategoryPicture.aspx?CategoryID={0}`分別。 這會建立 GridView 資料行，會呈現`<img>`項目其`src`屬性參考`DisplayCategoryPicture.aspx?CategoryID={0}`、 {0} 其中取代 GridView 資料列的`CategoryID`值。


![ImageField 加入 GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**圖 12**: ImageField 加入 GridView


在新增之後 ImageField，GridView s 的宣告式語法看起來應該像 soothe 下列：


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

請花一點時間來檢視此頁面，透過瀏覽器。 請注意如何每個記錄現在包含類別目錄的圖片。


[![類別目錄的圖片會顯示每個資料列](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**圖 13**: 類別的圖片會顯示每個資料列 ([按一下以檢視完整大小的影像](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>總結

在本教學課程，我們會檢查如何呈現二進位資料。 呈現資料的方式而定的資料類型。 PDF 冊檔案中，我們提供使用者檢視冊連結，按一下時，直接將使用者用以 PDF 檔案。 為類別 s 的圖片，我們會先建立來擷取並從資料庫傳回二進位資料的頁面，然後再用於該頁面在 GridView 中顯示每個類別 s 的圖片。

現在我們 ve 看我們準備好要檢查如何執行插入、 更新和刪除針對資料庫使用的二進位資料的二進位資料顯示方式。 在下一個教學課程中我們將探討如何將上傳的檔案與其對應的資料庫記錄產生關聯。 在本教學課程之後，我們看到如何更新現有的二進位資料，以及如何移除其相關聯的記錄時刪除的二進位資料。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已本文菲和 Dave Gardner。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](uploading-files-cs.md)
> [下一頁](including-a-file-upload-option-when-adding-a-new-record-cs.md)
