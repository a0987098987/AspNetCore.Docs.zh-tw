---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: 從 [內容] 頁面 (VB) 的主版頁面與互動 |Microsoft Docs
author: rick-anderson
description: 檢驗如何呼叫方法，從 [內容] 頁面中的程式碼中設定屬性等的主版頁面。
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 59a00305cdcaf41ac0b37649382b9c3dc9ce1b0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823678"
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>互動主版頁面，從 [內容] 頁面 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> 檢驗如何呼叫方法，從 [內容] 頁面中的程式碼中設定屬性等的主版頁面。


## <a name="introduction"></a>簡介

過去五個教學課程的課程，我們已討論過如何建立主版頁面、 定義內容區域、 繫結的 ASP.NET 頁面主版頁面，然後定義頁面特定內容。 當訪客要求特定的內容頁面時，會在執行階段，導致 統一的控制項階層架構的轉譯融合內容和主版頁面的標記。 因此，我們已經看過其中一個方法可以互動的主版頁面和其內容頁面的其中一個: [內容] 頁面詳細說明要 transfuse 主版頁面的 ContentPlaceHolder 控制項的標記。

我們的成果尚未檢查是主版頁面和內容頁面可以互動方式以程式設計的方式。 除了定義的主版頁面 ContentPlaceHolder 控制項的標記，內容頁面也可以將值指派給其主版頁面的公用屬性，並叫用它的公用方法。 同樣地，可能會與其內容頁互動主版頁面。 較不常見，其宣告式標記之間的互動比以程式設計方式互動的主要和內容頁面時，有許多情況下，需要這類程式設計互動的位置。

在本教學課程中我們將探討如何以程式設計方式進行內容的頁面互動與其主版頁面;在下一個教學課程中，我們將探討主版頁面可以同樣的互動方式使用其內容的頁面。

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>內容頁面和其主版頁面的程式設計互動的範例

當特定頁面的需要設定頁面的頁面為基礎時，我們會使用 ContentPlaceHolder 控制項。 但大部分的網頁需要發出特定的輸出，但一小部分的頁面的情況下需要加以自訂以顯示其他東西呢？ 其中一個範例，我們在中檢查這[*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-vb.md)教學課程中，牽涉到每個頁面上顯示登入介面。 雖然大部分頁面都應該包含登入介面，它應該隱藏少數幾個頁面，例如： 主要登入頁面 (`Login.aspx`); 建立帳戶 頁面中，與其他頁面，才可存取已驗證的使用者。 [*多個 ContentPlaceHolders 和預設內容*](multiple-contentplaceholders-and-default-content-vb.md)教學課程示範了如何為 ContentPlaceHolder 主版頁面中定義的預設內容，然後如何覆寫它，這些頁面的位置預設的內容已不想要的。

另一個選項是建立的公用屬性或主版頁面，指出是否要顯示或隱藏登入介面中的方法。 例如，主版頁面可能包含名為公用屬性`ShowLoginUI`其值用來設定`Visible`主版頁面中的登入控制項的屬性。 然後可以透過程式設計方式設定這些登入使用者介面應該隱藏其中的內容頁面`ShowLoginUI`屬性設`False`。

可能是主版頁面需要重新整理後的某些動作發生在 [內容] 頁面中顯示的資料時，就會發生的內容和主版頁面互動最常見的範例。 請考慮從特定的資料庫資料表時，主版頁面，其中包含的 GridView 會顯示五個最近新增的記錄，其中一種其內容頁包含的介面，將新記錄新增至該相同的資料表。

當使用者造訪的頁面，即可加入新的記錄時，她會看到五個最新加入的主版頁面中顯示的記錄。 之後填入新資料錄的資料行的值，她送出表單。 假設 GridView 的主版頁面中有其`EnableViewState`屬性設為 True （預設值），其內容會重新載入從檢視狀態，即使較新的記錄剛新增到資料庫，因此，顯示五個相同的資料錄。 這可能會混淆使用者。

> [!NOTE]
> 即使您停用 GridView 的檢視狀態，讓它重新繫結至每個回傳上其基礎資料來源時，它仍不會顯示剛加入的資料錄因為資料繫結至 GridView 稍早在網頁生命週期，比新記錄新增至資料庫物件時ase。


若要解決這個問題，因此剛加入的記錄會顯示在主版頁面的回傳，我們必須指示重新繫結至其資料來源 GridView GridView*之後*新記錄加入資料庫。 因為加入新的記錄 （和其事件處理常式） 會在 [內容] 頁面，但需要重新整理的 GridView 介面是主版頁面中，這會需要的內容和主版頁面之間的互動。

因為重新整理從 [內容] 頁面中的事件處理常式的主版頁面的顯示是其中一個最常見的需求，內容和主版頁面互動，讓我們來探索此主題的更多詳細資料。 本教學課程中下載包含名為 Microsoft SQL Server 2005 Express 的 Edition 資料庫`NORTHWIND.MDF`中網站的`App_Data`資料夾。 Northwind 資料庫會儲存產品、 員工及針對虛構的公司，Northwind Traders 的銷售資訊。

步驟 1 逐步顯示五個最近新增產品 GridView 中的主版頁面中。 步驟 2 建立的內容頁面中加入新的產品。 探討如何建立主版頁面中中的公用屬性和方法的步驟 3 和步驟 4 說明如何以程式設計方式使用這些屬性和方法，從 [內容] 頁面的介面。

> [!NOTE]
> 本教學課程不會不深入探討在 ASP.NET 中使用資料的詳細資訊。 設定主版頁面，以顯示資料和插入資料的 [內容] 頁面的步驟會完成，尚未 breezy。 如需更深入了解顯示和插入資料並使用 SqlDataSource 和 GridView 控制項，請參閱在本教學課程結尾處的進一步的讀數一節中的資源。


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>步驟 1： 顯示五個最近新增主版頁面的產品

開啟 Site.master 主版頁面，然後新增一個 Label 和 GridView 控制項`leftContent` `<div>`。 清除標籤`Text`屬性，設定其`EnableViewState`屬性設`False`，並將其`ID`屬性設`GridMessage`; 設定 GridView 的`ID`屬性設`RecentProducts`。 接下來，從設計工具中，展開 GridView 的智慧標籤，然後選擇繫結至新的資料來源。 這會啟動 資料來源組態精靈。 因為 Northwind 資料庫中`App_Data`資料夾是 Microsoft SQL Server 資料庫中，選擇 選取 （請參閱 圖 1） 建立 SqlDataSource; 名稱 SqlDataSource `RecentProductsDataSource`。


[![繫結至名為 RecentProductsDataSource SqlDataSource 控制項的 GridView](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**圖 01**: SqlDataSource 控制項名為繫結 GridView `RecentProductsDataSource` ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


下一個步驟會要求我們指定項目連接到資料庫。 選擇`NORTHWIND.MDF`資料庫檔案，從下拉式清單中，按一下 [下一步]。 因為這是我們使用此資料庫的第一次時，精靈會將提供給連接字串儲存在`Web.config`。 它使用的名稱將連接字串儲存`NorthwindConnectionString`。


[![連接到 Northwind 資料庫](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**圖 02**： 連接到 Northwind 資料庫 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


設定資料來源精靈會提供兩種方法，我們可以指定用來擷取資料的查詢：

- 藉由指定自訂 SQL 陳述式或預存程序，或
- 挑選資料表或檢視表，，然後指定要傳回的資料行

因為我們想要傳回只是五個最近新增的產品，我們需要指定自訂的 SQL 陳述式。 使用下列項目`SELECT`查詢：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

`TOP 5`關鍵字的查詢傳回前五筆記錄。 `Products`資料表的主索引鍵`ProductID`，是`IDENTITY`資料行，以確保我們每個新的產品加入至資料表會有較大的值，比先前的項目。 因此，排序結果`ProductID`以遞減順序傳回開頭為最新建立的產品。


[![傳回 5 個最近新增的產品](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**圖 03**： 傳回五個最最近新增的產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


完成精靈之後，Visual Studio 會產生如 GridView，以顯示兩個 BoundFields`ProductName`和`UnitPrice`從資料庫傳回的欄位。 目前主版頁面的宣告式標記應包含標記如下所示：


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

如您所見，標記包含： 標籤 Web 控制項 (`GridMessage`); GridView `RecentProducts`，兩個 BoundFields; 它會傳回 5 個最近新增的產品 SqlDataSource 控制項。

以此方式建立的 GridView 和設定，其 SqlDataSource 控制項瀏覽的網站，透過瀏覽器。 如 [圖 4] 所示，您會看到在左下角會列出五個最新的方格加入產品。


[![GridView 會顯示五個最近新增的產品](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**圖 04**: GridView 會顯示五個最最近新增的產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> 請放心清除 GridView 的外觀。 格式化顯示的一些建議包括`UnitPrice`值做為貨幣，並使用背景色彩和字型來改善格線的外觀。


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>步驟 2： 建立內容的頁面，即可加入新的產品

下一步是建立內容頁面，使用者可以從此處新增的新產品`Products`資料表。 加入新的內容頁面，以便`Admin`名為資料夾`AddProduct.aspx`，並確定將它繫結`Site.master`主版頁面。 此頁面已加入至網站之後，圖 5 顯示 方案總管。


[![將新的 ASP.NET 網頁新增至 [Admin] 資料夾](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**圖 05**： 加入新的 ASP.NET 頁面，以便`Admin`資料夾 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


請注意，在[*指定主版頁面的標題、 中繼標籤及其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中我們建立名為自訂的基底頁面類別`BasePage`如果它已產生頁面的標題未明確設定。 移至`AddProduct.aspx`頁面的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。

最後，更新`Web.sitemap`檔案，以包含這一課中的項目。 加入下列標記下方`<siteMapNode>`控制項識別碼命名的問題一課：


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

圖 6，新增此所示`<siteMapNode>`項目會反映在課程清單。

返回`AddProduct.aspx`。 在適用於內容的控制項`MainContent`ContentPlaceHolder，新增 DetailsView 控制項並將它命名`NewProduct`。 繫結至新的 SqlDataSource 控制項，名為的 DetailsView `NewProductDataSource`。 例如，以 sqldatasource 進行步驟 1 中，設定精靈，讓它使用 Northwind 資料庫，以及選擇指定自訂的 SQL 陳述式。 因為 DetailsView 會用來將項目加入至資料庫中，我們需要同時指定`SELECT`陳述式和`INSERT`陳述式。 使用下列項目`SELECT`查詢：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

然後，從 [插入] 索引標籤中，新增下列`INSERT`陳述式：


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

完成精靈之後移至 DetailsView 的智慧標籤並選取 「 啟用插入 」 的核取方塊。 這樣會增加與 DetailsView CommandField 其`ShowInsertButton`屬性設為 True。 因為此 DetailsView 將用於僅用於插入資料，設定 DetailsView`DefaultMode`屬性設`Insert`。

這樣就完成了 ！ 我們來測試此頁面。 請瀏覽`AddProduct.aspx`透過瀏覽器中，輸入 （請參閱 圖 6） 的名稱和價格。


[![將新的產品加入至資料庫](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**圖 06**： 將新的產品加入至資料庫 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


輸入之後的名稱和新的產品價格中，按一下 [插入] 按鈕。 這會導致表單回傳。 在回傳時，SqlDataSource 控制項的`INSERT`陳述式; 它的兩個參數會填入使用者輸入的值，在兩個文字方塊控制項 DetailsView。 不幸的是，發生插入沒有視覺回應。 它會是可有可無的訊息，確認已新增新的記錄。 我不要更動此練習的讀取器。 此外之後加入新的記錄從 DetailsView, GridView 的主版頁面中仍會顯示五個記錄與相同之前;它不包含剛加入的記錄。 我們將檢驗如何解決這個問題在後續的步驟。

> [!NOTE]
> 除了新增某種形式的成功插入的視覺化回應，我會建議您一併更新以包含驗證的 DetailsView 插入介面。 目前沒有任何驗證。 如果使用者輸入的值為 「 無效 」`UnitPrice`欄位，例如 「 太昂貴，"將會擲回例外狀況在回傳時，系統會嘗試將該字串轉換成十進位數。 如需有關自訂插入介面，請參閱[*自訂資料修改介面*教學課程](https://asp.net/learn/data-access/tutorial-20-vb.aspx)從我[使用資料教學課程系列](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>步驟 3： 建立主版頁面的公用屬性和方法

在步驟 1 中，我們新增標籤 Web 控制項，名為`GridMessage`上方 GridView 的主版頁面中。 此標籤被要選擇性地顯示一則訊息。 比方說之後加入新的記錄，以,`Products`資料表中，我們可能會想要顯示讀取的訊息:"*ProductName*已加入至資料庫。 」 與其硬式編碼主版頁面中此標籤的文字，我們可能會想要的 [內容] 頁面可自訂訊息。

因為標籤控制項會實作為內無法直接從內容頁面存取主版頁面的受保護的成員變數中。 若要使用主版頁面的 [內容] 頁面 （或就此而言，主版頁面中的任何 Web 控制項） 中的標籤，我們需要建立主版頁面公開的 Web 控制項，或做為 proxy 的其中一個屬性可以是公用屬性 存取。 將下列語法新增至主版頁面的程式碼後置類別，以公開 （expose） 的標籤`Text`屬性：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

若要新增新的記錄時`Products`資料表從內容頁面`RecentProducts`主版頁面中的 GridView 需要重新繫結至其基礎資料來源。 若要重新繫結 GridView 呼叫其`DataBind`方法。 主版頁面中的 GridView 不是以程式設計方式存取內容的頁面，因為我們需要建立的公用方法主版頁面中，呼叫時，會重新繫結至 GridView 資料。 將下列方法新增至主版頁面的程式碼後置類別：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

與`GridMessageText`屬性和`RefreshRecentProductsGrid`方法中的地方，任何內容的頁面可以以程式設計方式設定或讀取的值`GridMessage`標籤`Text`屬性或重新繫結至資料`RecentProducts`GridView。 步驟 4 會檢驗如何從內容頁面存取主版頁面的公用屬性和方法。

> [!NOTE]
> 別忘了將主版頁面的屬性和方法`Public`。 如果您不明確地代表這些屬性和方法`Public`，他們將無法從 [內容] 頁面。


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>步驟 4： 從內容頁面呼叫主版頁面的公用成員

現在，主版頁面的所需的公用屬性和方法，我們已經準備好叫用這些屬性和方法，從`AddProduct.aspx`內容頁面。 具體而言，我們需要設定主版頁面`GridMessageText`屬性並呼叫其`RefreshRecentProductsGrid`方法之後新產品已經新增到資料庫。 所有 ASP.NET 資料 Web 控制項都引發事件，前後立即完成各種工作，讓您輕鬆網頁程式開發人員採取一些程式設計之前或之後的工作。 例如，當使用者按一下 DetailsView 的 [插入] 按鈕，在回傳 DetailsView 引發其`ItemInserting`開始插入的工作流程之前的事件。 然後，它會將記錄插入資料庫。 接下來，DetailsView 會引發其`ItemInserted`事件。 因此，若要加入新的產品後，請使用主版頁面，建立事件處理常式的 DetailsView`ItemInserted`事件。

有兩個內容頁面可以以程式設計方式介面與其主版頁面的方式：

- 使用`Page.Master`屬性，會傳回主版頁面的鬆散型別參考，或
- 在頁面的主版頁面類型或檔案以指定的路徑`@MasterType`指示詞; 這會自動將強型別屬性加入至名為頁面`Master`。

讓我們來檢查這兩種方法。

### <a name="using-the-loosely-typedpagemasterproperty"></a>使用鬆散型別`Page.Master`屬性

所有的 ASP.NET 網頁必須衍生自`Page`類別，其位於`System.Web.UI`命名空間。 `Page`類別包含[`Master`屬性](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx)傳回頁面的主版頁面的參考。 如果網頁沒有主版頁面`Master`傳回`Nothing`。

`Master`屬性會傳回類型的物件[ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (同樣位於`System.Web.UI`命名空間) 這是從中所有主版頁面衍生自的基底類型。 因此，若要使用的公用屬性或方法定義於我們的網站主版頁面，我們必須轉型`MasterPage`所傳回的物件`Master`適當類型的屬性。 因為我們命名為我們的主版頁面檔案`Site.master`，程式碼後置類別命名為`Site`。 因此，下列程式碼轉換`Page.Master`的執行個體的內容`Site`類別。


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

既然我們已經轉換鬆散型別`Page.Master`屬性至站台類型，我們可以參考的屬性和方法的特定站台。 如 [圖 7] 所示，公用屬性`GridMessageText`出現在 IntelliSense 下拉式清單中。


[![IntelliSense 會顯示我們主頁公用屬性和方法](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**圖 07**: IntelliSense 會顯示我們主頁公用屬性和方法 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> 如果名為您的主版頁面檔案`MasterPage.master`則主版頁面的程式碼後置類別名稱就是`MasterPage`。 這可能會導致模稜兩可的程式碼從型別轉型時`System.Web.UI.MasterPage`到您`MasterPage`類別。 簡單地說，您需要完整限定的類型為，可能會較難使用網站專案模型時。 我的建議是確認您的主版頁面建立時您將它命名的項目以外的其他`MasterPage.master`或更棒的是，建立主版頁面的強型別參考。


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>建立強型別參考與`@MasterType`指示詞

如果您仔細看您可以看到 ASP.NET 網頁的程式碼後置類別是部分類別 (請注意`Partial`類別定義中的關鍵字)。 部分類別在 C# 和 Visual Basic.net Framework 2.0 中推出，和簡單的說，允許跨多個檔案中定義的類別的成員。 程式碼後置類別檔中- `AddProduct.aspx.vb`，例如，包含頁面開發人員，我們建立的程式碼。 除了我們的程式碼中，ASP.NET 引擎會自動建立個別的類別檔案屬性和事件處理常式中，會轉譯頁面的類別階層架構中的宣告式標記。

只要瀏覽 ASP.NET 網頁時，就會發生自動程式碼產生奠定一些相當有趣且實用的可能性。 在主版頁面的情況下如果我們告訴我們的內容頁面正在使用哪些主版頁面的 ASP.NET 引擎就會產生強型別`Master`為我們的屬性。

使用[`@MasterType`指示詞](https://msdn.microsoft.com/library/ms228274.aspx)，告知 ASP.NET 引擎的 [內容] 頁面的主版頁面類型。 `@MasterType`指示詞可接受的類型名稱的主版頁面或檔案路徑。 若要指定`AddProduct.aspx`頁面上使用`Site.master`作為其主版頁面的頂端加入下列指示詞`AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

這個指示詞會指示 ASP.NET 引擎加入主版頁面，透過名為屬性的強型別參考`Master`。 與`@MasterType`我們可以呼叫指示詞就地`Site.master`主要頁面上的公用屬性和方法直接透過`Master`屬性，而不需要任何轉換 （cast）。

> [!NOTE]
> 如果您省略`@MasterType`指示詞，則語法`Page.Master`和`Master`傳回相同的動作： 在頁面的主版頁面的鬆散型別物件。 如果您納入`@MasterType`指示詞再`Master`傳回指定的主版頁面的強型別參考。 `Page.Master`不過，仍會傳回鬆散型別參考。 針對更徹底的了解為什麼是這樣的情況以及如何`Master`屬性的建構時`@MasterType`指示詞之後，請參閱[K.Scott Allen](http://odetocode.com/blogs/scott/default.aspx)的部落格項目[`@MasterType`在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>在之後加入新的產品更新主版頁面

既然我們已經知道如何叫用主版頁面的公用屬性和方法，從內容頁面，我們已經準備好更新`AddProduct.aspx`頁面，如此主版頁面會重新整理之後加入新的產品。 在步驟 4 開始，我們建立 DetailsView 控制項的事件處理常式`ItemInserting`事件，它會立即在新產品已經新增到資料庫之後，才執行。 將下列程式碼新增至該事件處理常式中：


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

上述程式碼會使用這兩個鬆散型`Page.Master`屬性的強型別`Master`屬性。 請注意，`GridMessageText`屬性設定為"*ProductName*新增至資料格...」剛加入之產品的值是可透過存取`e.Values`集合，您可以看到，剛`ProductName`的值透過存取`e.Values("ProductName")`。

[圖 8] 顯示`AddProduct.aspx`之後將新的產品-Scott Soda-立即的頁面已加入至資料庫。 請注意，主版頁面的標籤中註明剛加入的產品名稱和 GridView 已經過改版，包括產品，而且其價格。


[![主版頁面的標籤和 GridView 會顯示剛加入的產品](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**圖 08**: 主版頁面的標籤和 GridView 顯示 Just-Added 產品 ([按一下以檢視完整大小的影像](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>總結

在理想情況下，主版頁面和其內容的頁面是完全獨立的另一個，並需要互動的任何層級。 雖然主版頁面和內容頁面應該設計的心，有幾個常見的案例中內容的頁面必須與其主版頁面互動。 其中一個最常見的原因都著重於更新主版頁面顯示根據該批 [內容] 頁面中的某些動作的特定部分。

好消息是相當容易就能以程式設計的方式互動其主版頁面的內容頁面。 開始建立封裝的功能，需要的內容頁面所叫用主版頁面中的公用屬性或方法。 然後，在 [內容] 頁面存取主版頁面的屬性和方法透過鬆散型別`Page.Master`屬性或使用`@MasterType`指示詞，以建立主版頁面的強型別參考。

在下一個教學課程中，我們會檢驗如何以程式設計的方式互動的其中一個其內容頁的主版頁面。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新在 ASP.NET 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 主版頁面： 秘訣、 技巧和陷阱](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` 在 ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [內容與主版頁面之間傳遞資訊](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [在 ASP.NET 教學課程中使用的資料](../../data-access/index.md)

### <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP 本書籍，他是 4GuysFromRolla.com 的創辦人，一直從事 Microsoft Web 技術自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。 Scott 要聯絡[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Zack Jones。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](control-id-naming-in-content-pages-vb.md)
> [下一頁](interacting-with-the-content-page-from-the-master-page-vb.md)
