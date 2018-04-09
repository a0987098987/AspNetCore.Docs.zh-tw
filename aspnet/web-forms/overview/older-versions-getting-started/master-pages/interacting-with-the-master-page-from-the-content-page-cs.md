---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: 使用主版頁面，從 [內容] 頁面 (C#) 互動 |Microsoft 文件
author: rick-anderson
description: 會檢查呼叫方法、 從內容頁面中的程式碼中設定屬性的主版頁面等等的方式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: b550d8c2a64bb2ad91e1db7b2c25433f73dbd5b7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>互動主版頁面，從 [內容] 頁面 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> 會檢查呼叫方法、 從內容頁面中的程式碼中設定屬性的主版頁面等等的方式。


## <a name="introduction"></a>簡介

在我們已經探討如何建立主版頁面的過去五個教學課程的過程中，定義內容區域、 將 ASP.NET 網頁繫結至主版頁面中，以及定義頁面的特定內容。 當訪客要求特定的內容頁面上時，也會在執行階段，導致統一的控制項階層架構的轉譯 fused 內容和主版頁面的標記。 因此，我們已經看過另一種方式在主版頁面和其內容頁的其中一個可以互動: [內容] 頁面會列出要 transfuse 主版頁面的 ContentPlaceHolder 控制項的標記。

我們配置了尚未檢查是主版頁面和內容頁面可以互動方式以程式設計的方式。 除了定義的主版頁面 ContentPlaceHolder 控制項的標記，內容頁面也可以將值指派給其主版頁面的公用屬性，並叫用其公用方法。 同樣地，可能會與其內容的頁面互動主版頁面。 較不常見，其宣告式標記之間的互動比主要和內容頁面之間的程式設計互動時，有許多情況都需要這類的程式設計互動的位置。

在本教學課程中我們檢驗如何以程式設計方式進行內容的頁面互動其主版頁面;在下一個教學課程中，我們將探討如何主版頁面可以同樣地互動其內容頁。

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>內容頁面和其主版頁面之間的程式設計互動的範例

當頁面的特定區域，就必須設定頁面的頁面為基礎時，我們會使用 ContentPlaceHolder 控制項。 但大部分的網頁要發出特定的輸出，但量少的頁面的情況下需要自訂它以顯示其他項目呢？ 其中一個這類範例，其中我們探討[*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-cs.md)教學課程中，包含每個頁面上顯示登入介面。 雖然大部分頁面都應該包括登入介面，它應該隱藏少數幾個頁面，例如： 主登入頁面 (`Login.aspx`); 建立帳戶 頁面中; 與其他頁面，才可存取已驗證的使用者。 [*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-cs.md)教學課程說明如何針對 ContentPlaceHolder 主版頁面中定義的預設內容，然後如何覆寫它，這些頁面的位置預設的內容已不需要。

另一個選項是建立的公用屬性或方法中的主版頁面，指出是否要顯示或隱藏登入介面。 例如，主版頁面可能會包含名為的公用屬性`ShowLoginUI`其值用來設定`Visible`主版頁面中的登入控制項的屬性。 然後無法以程式設計方式設定這些內容的頁面，其中應該隱藏登入使用者介面`ShowLoginUI`屬性`false`。

可能是主版頁面需要重新整理後一些動作發生的內容頁面中顯示的資料時，就會發生最常見的內容和主版頁面互動的範例。 請考慮包含的 GridView 會顯示五個最新的主版頁面從特定的資料庫資料表時，加入記錄和其內容頁的其中一種包含將新記錄新增至相同資料表的介面。

當使用者造訪的頁面，即可加入新的記錄時，她看到五個最新加入的主版頁面中顯示的記錄。 之後填入新資料錄的資料行的值，她送出表單。 假設 GridView 主版頁面中的具有其`EnableViewState`屬性設定為 true （預設值），其內容會重新載入從檢視狀態，即使較新的記錄剛新增到資料庫，因此，顯示五個相同的記錄。 這可能會混淆使用者。

> [!NOTE]
> 即使您停用 GridView 的檢視狀態，讓它重新繫結至其在每次回傳的基礎資料來源時，它仍不會顯示只是加入資料錄因為資料繫結至 GridView 稍網頁生命週期，比 datab 當加入新的記錄ase。


若要補救這種情況，使剛加入的記錄會顯示在主版頁面的回傳我們需要以指示其資料來源重新繫結至 GridView GridView*之後*資料庫已加入新的記錄。 這需要的內容和主版頁面之間的互動，因為加入新的記錄 （和其事件處理常式） 會在 [內容] 頁面，但是需要重新整理的 GridView 的介面是主版頁面中。

因為重新整理從內容頁面中的事件處理常式的主版頁面的顯示是其中一個最常見的需求，對內容和主版頁面互動，讓我們來探索更多詳細資料中的這個主題。 在此教學課程下載包含名為 Microsoft SQL Server 2005 Express 的 Edition 資料庫`NORTHWIND.MDF`之網站中`App_Data`資料夾。 Northwind 資料庫儲存產品、 員工和虛構的公司，Northwind Traders 的銷售資訊。

步驟 1 會逐步解說顯示五個最新加入的 GridView 主版頁面中的產品。 步驟 2 建立的內容頁面加入新的產品。 會查看如何在主版頁面中，建立公用屬性及方法的步驟 3 和步驟 4 說明如何以程式設計方式使用這些屬性和方法，從 [內容] 頁面的介面。

> [!NOTE]
> 本教學課程不會深入在 ASP.NET 中使用資料的細節。 設定主版頁面，來顯示資料和插入資料的內容頁面的步驟都完成，尚未 breezy。 顯示和插入資料並使用 SqlDataSource 和 GridView 控制項更深入探討，請參閱本教學課程結尾處的進一步讀取區段中的資源。


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>步驟 1： 顯示五個最近新增主版頁面中的產品

開啟`Site.master`主版頁面，並加入標籤和 GridView 控制項`leftContent` `<div>`。 清除標籤的`Text`屬性，將其`EnableViewState`屬性設定為 false，且其`ID`屬性`GridMessage`; 設定 GridView 的`ID`屬性`RecentProducts`。 接下來，從設計工具中，展開 GridView 的智慧標籤，然後選擇繫結至新的資料來源。 這會啟動 資料來源組態精靈。 因為 Northwind 資料庫中`App_Data`資料夾是 Microsoft SQL Server 資料庫中，選擇所選取 （請參閱圖 1） 建立 SqlDataSource; SqlDataSource `RecentProductsDataSource`。


[![將 GridView 繫結至名為 RecentProductsDataSource SqlDataSource 控制項](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**圖 01**: SqlDataSource 控制項名為繫結的 GridView `RecentProductsDataSource` ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


下一個步驟會詢問我們指定連接到目標資料庫。 選擇`NORTHWIND.MDF`資料庫檔案，從下拉式清單中，按一下 [下一步]。 因為這是我們會使用此資料庫的第一次，精靈會提供連接字串中的儲存`Web.config`。 已使用名稱的連接字串儲存它`NorthwindConnectionString`。


[![連接至 Northwind 資料庫](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**圖 02**： 連接至 Northwind 資料庫 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


設定資料來源精靈會提供兩個方法，我們可以指定用來擷取資料的查詢：

- 藉由指定自訂 SQL 陳述式或預存程序，或
- 選取資料表或檢視表，，然後指定要傳回的資料行

由於我們想要傳回只五個最新加入的產品，我們需要指定自訂的 SQL 陳述式。 使用下列的 SELECT 查詢：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5`關鍵字的查詢傳回前五筆記錄。 `Products`資料表的主索引鍵， `ProductID`，是`IDENTITY`資料行，以確保我們新產品加入至資料表會有比先前的項目較大的值。 因此，排序結果，您可以`ProductID`以遞減順序傳回開頭為最新建立的產品。


[![傳回最近加入的五項產品](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**圖 03**： 傳回五個最最近加入產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


完成精靈之後，Visual Studio 會產生兩個顯示 gridview BoundFields`ProductName`和`UnitPrice`從資料庫傳回的欄位。 此時主版頁面的宣告式標記應包含標記如下所示：


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

如您所見，標記包含： 標籤 Web 控制項 (`GridMessage`); GridView `RecentProducts`，兩個 BoundFields; 它會傳回 5 個最近新增產品的 SqlDataSource 控制項。

與這個 GridView 建立和設定，其 SqlDataSource 控制項，請造訪的網站透過瀏覽器。 如圖 4 所示，您會看到的方格中列出，這 5 個最近的左下角新增產品。


[![在 GridView 會顯示最近加入的五項產品](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**圖 04**: GridView 會顯示五個最最近加入產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> 請隨意清除 GridView 的外觀。 某些建議包括格式顯示`UnitPrice`值做為貨幣，並使用背景色彩和字型，以改善格線的外觀。


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>步驟 2： 建立內容的頁面，即可將新的產品

下一步是建立內容頁面，使用者可以從此處新增至新的產品`Products`資料表。 加入新的內容頁面以`Admin`資料夾名為`AddProduct.aspx`，務必將它繫結讓`Site.master`主版頁面。 此頁面已加入至網站之後，圖 5 顯示 [方案總管]。


[![將新的 ASP.NET 網頁新增至 [Admin] 資料夾](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**圖 05**： 加入新的 ASP.NET 網頁的`Admin`資料夾 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


請記得，在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`它產生頁面的標題未明確設定。 移至`AddProduct.aspx`網頁的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案至這一課中加入一個項目。 加入下列標記下方`<siteMapNode>`控制識別碼命名的問題一課：


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

這個加法圖 6 所示`<siteMapNode>`項目會反映在課程清單中。

返回`AddProduct.aspx`。 在內容控制項中`MainContent`ContentPlaceHolder，加入 DetailsView 控制項並將其命名`NewProduct`。 繫結至新的 SqlDataSource 控制項，名為的 DetailsView `NewProductDataSource`。 要使用在步驟 1 中 SqlDataSource，設定精靈以便讓它使用 Northwind 資料庫，然後選擇指定自訂的 SQL 陳述式。 在 DetailsView 將用來將項目加入至資料庫，因為我們需要同時指定`SELECT`陳述式和`INSERT`陳述式。 使用下列`SELECT`查詢：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

然後，從 [插入] 索引標籤中，加入下列`INSERT`陳述式：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

完成精靈之後前往 DetailsView 的智慧標籤，選取 「 啟用插入 」 的核取方塊。 這會將 CommandField DetailsView 加入其`ShowInsertButton`屬性設為 true。 因為此 DetailsView 會用來插入資料，設定在 DetailsView 的`DefaultMode`屬性`Insert`。

這就是這麼簡單 ！ 我們來測試此頁面。 請瀏覽`AddProduct.aspx`透過瀏覽器中，輸入名稱和價格 （請參閱圖 6）。


[![將新的產品加入資料庫](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**圖 06**： 將新的產品加入資料庫 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


在輸入中的名稱和新的產品的價格後, 按一下 [插入] 按鈕。 這會導致回傳表單。 在回傳時，SqlDataSource 控制項的`INSERT`執行陳述式; 它的兩個參數會填入使用者輸入控制項中的值在 DetailsView 的兩個文字方塊。 很遺憾，目前沒有視覺回應可插入已發生。 是最好的訊息，確認已加入新的記錄。 我將保留此為一項工作讀取器。 此外之後從 DetailsView 加入新的記錄, 中的主版頁面 GridView 仍會顯示五個記錄與相同。它不包含剛加入的記錄。 我們將檢驗如何補救這種情況中的後續步驟。

> [!NOTE]
> 除了新增某種形式的視覺化回應，已成功插入，我會建議您也會更新以包含驗證的 DetailsView 的插入介面。 目前沒有任何驗證。 如果使用者輸入的值無效`UnitPrice`欄位，例如 「 太昂貴，"將會擲回例外狀況在回傳時系統會嘗試將該字串轉換成十進位。 如需有關自訂插入介面，請參閱[*自訂的資料修改介面*教學課程](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)從我[使用教學課程系列資料](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>步驟 3： 建立主版頁面中的公用屬性和方法

在步驟 1 中，我們新增標籤 Web 控制項，名為`GridMessage`GridView 主版頁面上方。 此標籤會選擇性地顯示一則訊息。 比方說之後加入新的記錄，到,`Products`資料表中，我們可能會想要顯示讀取的訊息:"*ProductName*已加入至資料庫。 」 而不是硬式編碼為主版頁面中的標籤文字，我們可能會想要依內容頁可自訂訊息。

Label 控制項會實作為主版頁面內的受保護的成員變數，因為它無法直接從內容頁面存取。 若要使用主版頁面，從 [內容] 頁面 （或，而且任何主版頁面中的 Web 控制項） 中的標籤我們要公開 Web 控制項，或做為 proxy 的其中一個屬性可以是主版頁面中建立的公用屬性 存取。 主版頁面的程式碼後置類別公開的標籤加入下列語法`Text`屬性：


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

若要加入新的記錄時`Products`內容頁面的表格`RecentProducts`GridView 主版頁面中的，需要重新繫結至其基礎資料來源。 若要重新繫結的 GridView 呼叫其`DataBind`方法。 GridView 主版頁面中的不是透過程式設計方式存取內容頁面，因為我們建立的公用方法主版頁面中，呼叫時，需要重新繫結至 GridView 的資料。 將下列方法加入至主版頁面的程式碼後置類別：


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

與`GridMessageText`屬性和`RefreshRecentProductsGrid`方法就地任何內容的頁面可以透過程式設計方式設定或讀取的值`GridMessage`標籤的`Text`屬性或重新繫結至資料`RecentProducts`GridView。 步驟 4 會檢查如何從內容頁面存取主版頁面的公用屬性和方法。

> [!NOTE]
> 別忘了標記主版頁面的屬性和方法`public`。 如果您不明確代表這些屬性和方法做為`public`，它們將無法再從 [內容] 頁面存取。


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>步驟 4： 從內容頁面呼叫主版頁面的公用成員

現在，主版頁面都有必要的公用屬性和方法，我們要叫用這些屬性和方法，從準備`AddProduct.aspx`內容頁面。 具體來說，我們需要設定主版頁面的`GridMessageText`屬性並呼叫其`RefreshRecentProductsGrid`方法之後，新的產品加入資料庫。 所有 ASP.NET 資料 Web 控制項都引發事件之前和之後完成各種工作，簡化網頁開發人員来採取某些程式設計動作之前或之後的工作。 例如，當使用者按一下 DetailsView 的 [插入] 按鈕，在回傳 DetailsView 引發其`ItemInserting`開始插入的工作流程之前的事件。 然後，它會將記錄插入資料庫。 接下來，在 DetailsView 引發其`ItemInserted`事件。 因此，為了加入新的產品後，請使用主版頁面，建立事件處理常式的 DetailsView 的`ItemInserted`事件。

有兩個內容頁面以程式設計的方式可在其主版頁面互動的方式：

- 使用`Page.Master`屬性，傳回主版頁面的鬆散型別參考，或
- 指定網頁的主版頁面類型或檔案路徑透過`@MasterType`指示詞; 這會自動將強型別屬性加入名為頁面`Master`。

讓我們來討論這兩種方法。

### <a name="using-the-loosely-typedpagemasterproperty"></a>使用鬆散型別`Page.Master`屬性

所有的 ASP.NET web pages 必須衍生自`Page`類別位於`System.Web.UI`命名空間。 `Page`類別包含[`Master`屬性](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx)傳回網頁的主版頁面的參考。 如果頁面沒有主版頁面`Master`傳回`null`。

`Master`屬性會傳回型別的物件[ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (同樣位於`System.Web.UI`命名空間) 即從衍生自所有主版頁面的基底類型。 因此，若要使用的公用屬性或方法定義在我們的網站的主版頁面我們必須轉換`MasterPage`從傳回的物件`Master`適當類型的屬性。 因為我們名為我們的主版頁面檔案`Site.master`，程式碼後置類別命名為`Site`。 因此，下列程式碼轉換`Page.Master`網站類別的執行個體的屬性。


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

現在，我們已經轉換鬆散型別`Page.Master`屬性`Site`我們可以參考的屬性和站台的特定方法的型別。 如圖 7 所示的公用屬性`GridMessageText`出現在 IntelliSense 下拉式清單中。


[![IntelliSense 會顯示我們主版頁面的公用屬性和方法](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**圖 07**: IntelliSense 會顯示我們主版頁面的公用屬性和方法 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> 如果您命名為您的主版頁面檔案`MasterPage.master`主版頁面的程式碼後置類別名稱是`MasterPage`。 這可能會導致模稜兩可的程式碼的型別轉型時`System.Web.UI.MasterPage`到您`MasterPage`類別。 簡單地說，您需要完整限定的類型，您和轉換，可能會有點時使用的網站專案模型。 我的建議，可請確定當您建立您的主版頁面您命名它的項目以外`MasterPage.master`或更好的是，建立強型別參考的主版頁面。


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>建立具有的強型別參考`@MasterType`指示詞

如果您仔細查看您可以看到 ASP.NET 網頁的程式碼後置類別是部分類別 (請注意`partial`類別定義中的關鍵字)。 部分類別 C# 和 Visual Basic 以 Framework 2.0 中導入，以及簡單的說，允許跨多個檔案會定義類別的成員。 程式碼後置類別檔中- `AddProduct.aspx.cs`，例如包含網頁開發人員，我們建立的程式碼。 我們的程式碼中，除了 ASP.NET 引擎會自動建立個別的類別檔案屬性和事件處理常式中，會轉譯頁面的類別階層架構中的宣告式標記。

每當瀏覽 ASP.NET 網頁時就會發生自動程式碼產生微不足道部分而不是相關且實用的可能性。 在主版頁面中，如果我們告訴我們內容的頁面正在使用哪些主版頁面的 ASP.NET 引擎就會產生強型別`Master`為我們的屬性。

使用[`@MasterType`指示詞](https://msdn.microsoft.com/library/ms228274.aspx)，告知 ASP.NET 引擎的內容頁面的主版頁面類型。 `@MasterType`指示詞可接受主版頁面的型別名稱，或是其檔案路徑。 若要指定`AddProduct.aspx`頁面使用`Site.master`做為其主版頁面的頂端加入下列指示詞`AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

這個指示詞會指示 ASP.NET 引擎加入主版頁面透過名為屬性的強型別參考`Master`。 與`@MasterType`就地指示詞時，我們就可以呼叫`Site.master`主要頁面上的公用屬性及方法直接透過`Master`沒有任何轉換的屬性。

> [!NOTE]
> 如果您省略`@MasterType`指示詞，語法`Page.Master`和`Master`傳回相同的作業： 在頁面的主版頁面的鬆散型別物件。 如果您包含`@MasterType`指示詞再`Master`傳回指定的主版頁面的強型別參考。 `Page.Master`不過，仍會傳回鬆散型別的參考。 以更徹底地查看原因是這種情況以及如何`Master`屬性的建構時`@MasterType`指示詞之後，請參閱[K.Scott Allen](http://odetocode.com/blogs/scott/default.aspx)的部落格項目[`@MasterType`在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>加入新的產品之後更新主版頁面

既然我們已經知道如何叫用主版頁面的公用屬性和內容頁面的方法，我們準備好更新`AddProduct.aspx`頁面，如此在主版頁面重新整理之後加入新的產品。 步驟 4 的開頭，我們建立 DetailsView 控制項的事件處理常式`ItemInserting`事件，新的產品加入資料庫之後，立即執行。 將下列程式碼加入至該事件處理常式：


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

上述程式碼會使用這兩個鬆散型別`Page.Master`屬性和強型別`Master`屬性。 請注意，`GridMessageText`屬性設定為"*ProductName*加入至資料格...」剛加入的產品值的存取透過`e.Values`集合; 您可以看到，只要加入`ProductName`透過存取值`e.Values["ProductName"]`。

圖 8 顯示`AddProduct.aspx`立即在新的產品-Scott Soda-之後的頁面已加入至資料庫。 請注意剛加入的產品名稱述主版頁面的標籤和 GridView 已重新整理包含產品和其價格。


[![主版頁面的標籤和 GridView 會顯示剛加入的產品](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**圖 08**: 主版頁面的標籤和 GridView 會顯示 Just-Added 產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>總結

在理想情況下，主版頁面和其內容的頁面是完全獨立的另一個，而且需要互動的任何層級。 雖然主版頁面和內容頁面設計時應該記住該目標，但是會有其主版頁面內容的頁面必須介面中的常見案例的數目。 其中一個最常見的原因而中心更新該批內容頁面中的某些動作為基礎的主版頁面顯示的特定部分。

好消息是相當簡單，有一個以程式設計方式互動其主版頁面的內容頁面。 開始建立封裝的功能，需要叫用的內容頁的主版頁面中的公用屬性或方法。 然後，在 [內容] 頁面，存取主版頁面的屬性和方法透過鬆散型別`Page.Master`屬性或使用`@MasterType`指示詞，以建立強型別參考主版頁面。

在下一個教學課程中，我們檢驗如何以程式設計方式互動的其中一個其內容頁的主版頁面。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新在 ASP.NET 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 主版頁面： 秘訣、 竅門與設陷](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` 在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [主版頁面內容之間傳遞資訊](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教學課程中使用的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Zack Jones。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](control-id-naming-in-content-pages-cs.md)
> [下一頁](interacting-with-the-content-page-from-the-master-page-cs.md)
