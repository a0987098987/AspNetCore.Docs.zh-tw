---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: 建立資料存取層 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將從頭開始，並建立資料存取層 (DAL)，使用具類型的資料集，來存取資料庫中的資訊。
ms.author: aspnetcontent
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 556771994fd80773ec1e86bcbab0f60f2bb821e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838750"
---
<a name="creating-a-data-access-layer-c"></a>建立資料存取層 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe)或[下載 PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> 在本教學課程中，我們將從頭開始，並建立資料存取層 (DAL)，使用具類型的資料集，來存取資料庫中的資訊。


## <a name="introduction"></a>簡介

身為 web 開發人員，我們的生活主要是因為使用的資料。 我們會建立資料庫來儲存資料，也就是程式碼來擷取並修改它，並收集和彙總的網頁。 這是將探討 ASP.NET 2.0 中實作這些常見的模式的技術長系列的第一個教學課程。 我們將開始建立[軟體架構](http://en.wikipedia.org/wiki/Software_architecture)組成的資料存取層 (DAL) 使用具類型的資料集，商務邏輯層 (BLL)，會強制執行自訂的商務規則，並展示層包含 ASP.NET 的頁面共用通用的頁面配置。 一旦這個後端基礎已配置，我們將移動到報告，顯示如何顯示，彙總、 收集和驗證的 web 應用程式的資料。 這些教學課程都被為了很簡潔，並提供足夠的螢幕擷取畫面的逐步指示，可逐步引導您完成此程序以視覺化方式而定。 每個教學課程適用於 C# 和 Visual Basic 版本，而且包含的下載時，使用完整的程式碼。 （本教學課程中第一個是相當冗長，但其餘部分會更容易吸收的區塊 （chunk）。

這些教學課程中我們將使用放在 Northwind 資料庫的 Microsoft SQL Server 2005 Express 的 Edition 版本**應用程式\_資料**目錄。 資料庫檔案，除了**應用程式\_資料**資料夾也包含 SQL 指令碼，用於建立資料庫，如果您想要使用不同的資料庫版本。 這些指令碼也可以[直接從 Microsoft 下載](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)，如果您想使用。 如果您使用不同的 SQL Server 版本的 Northwind 資料庫，您必須更新**NORTHWNDConnectionString**設定應用程式的**Web.config**檔案。 為檔案系統為基礎的網站專案使用 Visual Studio 2005 Professional Edition 建置 web 應用程式。 不過，所有教學課程會運作，也能用於免費版本的 Visual Studio 2005 [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/)。  
  
在本教學課程中，我們將從頭開始，並建立資料存取層 (DAL)，後面接著在第二個教學課程中，建立商務邏輯層 (BLL) 和頁面配置和第三個中的導覽。 之後的第三個將以 foundation 為基礎的教學課程所配置的第三個。 我們有許多来涵蓋此第一個教學課程中，因此啟動 Visual Studio 並讓我們開始 ！

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>步驟 1： 建立 Web 專案，並連接到資料庫

我們建立我們的資料存取層 (DAL) 之前，我們首先要建立的網站，並設定我們的資料庫。 開始建立新檔案系統為基礎 ASP.NET 網站。 若要這麼做，請移至 檔案 功能表並選擇新的網站上，顯示新的網站 對話方塊。 選擇 ASP.NET 網站範本、 到檔案系統設定 位置 下拉式清單、 選擇要放置 web 站台的資料夾和設定的語言為 C#。


[![建立新檔案系統為基礎的網站](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**圖 1**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image3.png))


這會建立新的網站與**Default.aspx** ASP.NET 網頁並**應用程式\_資料**資料夾。

建立網站後下, 一步是要在 Visual Studio 伺服器總管 中加入資料庫參考。 將資料庫加入至 [伺服器總管] 中，您可以新增資料表、 預存程序、 檢視和等全部從 Visual Studio 內。 您也可以檢視資料表資料，或透過 查詢產生器以手動方式或以圖形方式建立您自己的查詢。 此外，當我們建置具類型資料集為 DAL 我們必須點 Visual Studio 應該從中建構輸入資料集的資料庫。 雖然我們可以提供在該時間點的此連接資訊，Visual Studio 會自動填入下拉式清單中，已在 [伺服器總管] 中註冊的資料庫。

將 Northwind 資料庫新增至 [伺服器總管] 中的步驟取決於您是否想要使用 SQL Server 2005 Express Edition 資料庫中的**應用程式\_資料**資料夾，或如果您有 Microsoft SQL Server 2000 或 2005年您想要改為使用的資料庫伺服器設定。

## <a name="using-a-database-in-theappdatafolder"></a>使用資料庫中 theApp\_DataFolder

如果您沒有 SQL Server 2000 或 2005年資料庫伺服器連接，或您只想要避免將資料庫加入至資料庫伺服器，您可以使用位於已下載的應有的 Northwind 資料庫的 SQL Server 2005 Express Edition 版本e's**應用程式\_資料**資料夾 (**NORTHWND。MDF**)。

將資料庫置於**應用程式\_資料**資料夾會自動新增至 [伺服器總管] 中。 假設您有 SQL Server 2005 Express Edition 安裝在您的電腦上應該會看到名為 NORTHWND 的節點。MDF 在 伺服器總管 中，您可以展開及探索其資料表、 檢視、 預存程序，等等 （請參閱 圖 2）。

**應用程式\_資料**資料夾也可以保存 Microsoft Access **.mdb**檔案，如同其 SQL Server 對應項目，會自動新增至 [伺服器總管] 中。 如果您不想使用的任何 SQL Server 選項，您可以隨時[下載 Microsoft Access 版本的 Northwind 資料庫檔案](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN)拖放到**應用程式\_資料**目錄。 請記住，不過，為的 Access 資料庫不是功能豐富與 SQL Server 和其設計目的並非要用於網站案例。 此外，數個 35 個以上教學課程會使用某些不支援的存取權的資料庫層級功能。

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>連接到 Microsoft SQL Server 2000 或 2005年的資料庫伺服器中的資料庫

或者，您可能會連線至資料庫伺服器上安裝 Northwind 資料庫。 如果資料庫伺服器還沒有安裝 Northwind 資料庫，您首先必須將它新增至資料庫伺服器執行包含在本教學課程下載或安裝指令碼[下載 Northwind SQL Server 2000 版安裝指令碼和](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)直接從 Microsoft 的網站。

一旦您已安裝的資料庫，請移以在 Visual Studio 伺服器總管] 中，以滑鼠右鍵按一下 [資料連線] 節點中，然後選擇 [加入連接。 如果您沒有看到 [伺服器總管] 中移至 [檢視 / 伺服器總管] 中或叫用的 Ctrl + Alt + S。 這會顯示 加入連接對話方塊中，其中您可以指定要連接到伺服器，驗證資訊，並輸入資料庫名稱。 一旦您已成功設定資料庫連線資訊並按一下 [確定] 按鈕，資料庫將會新增為 [資料連線] 節點底下的節點。 您可以展開 [資料庫] 節點，若要瀏覽資料表、 檢視、 預存程序，等等。


![將連線新增至您的資料庫伺服器的 Northwind 資料庫](creating-a-data-access-layer-cs/_static/image4.png)

**圖 2**： 將連接加入您的資料庫伺服器的 Northwind 資料庫


## <a name="step-2-creating-the-data-access-layer"></a>步驟 2： 建立資料存取層

使用資料的其中一個選項時的特定資料的邏輯直接嵌入展示層 （在 web 應用程式中，按一下 ASP.NET 網頁會構成的展示層）。 這可能需要在 ASP.NET 網頁的程式碼部分，撰寫 ADO.NET 程式碼或使用 SqlDataSource 控制項標記部分的形式。 在任一情況下，這種方法會緊密結合的資料存取邏輯兩者與展示層。 建議的方法，不過，為分開的展示層中的資料存取邏輯。 此獨立的圖層指資料的存取層，簡稱 DAL，通常會實作為個別的類別庫專案。 此多層式架構的優點有詳細的記錄 （結尾的本教學課程中的這些優點的詳細資訊，請參閱 「 進一步的數據 」 一節），而且我們要在這一系列的方法。

所有的程式碼的特定基礎的資料來源，例如建立資料庫連接，發出**選取 **，**插入**， **UPDATE**，和**刪除**DAL 應該位於命令，並依此類推。 展示層不應該包含這類資料存取程式碼中，任何參考，但應該改為對進行呼叫的所有資料要求 DAL。 資料存取層級通常會包含存取基礎資料庫資料的方法。 Northwind 資料庫中，例如，具有**產品**並**類別**記錄所屬的類別目錄銷售的產品的資料表。 在我們的 DAL 中我們會有類似的方法：

- **GetCategories()，** 這會傳回所有類別的相關資訊
- **GetProducts()**，這會傳回所有產品的相關資訊
- **GetProductsByCategoryID (*categoryID*)**，這會傳回屬於指定分類的所有產品
- **GetProductByProductID (*productID*)**，這會傳回特定產品的相關資訊

這些方法，叫用時，將會連接到資料庫、 發出適當的查詢，並傳回結果。 我們將會如何傳回這些結果很重要。 這些方法可能只會傳回資料集或資料庫查詢中所填入的 DataReader，但在理想情況下這些結果應傳回使用*強型別物件*。 強型別物件是一個結構描述嚴格定義在編譯時期，而相反，也就是鬆散型的物件，是其中一個執行階段之前不知道其結構描述。

例如，DataReader 與 （根據預設值） 的資料集是鬆散型別物件，因為其結構描述由用來填入這些資料庫查詢所傳回的資料行定義。 若要從鬆散型別的 DataTable，我們必須使用下列語法： 存取特定的資料行：  <strong><em>DataTable</em>。資料列 [<em>index</em>] [」<em>columnName</em>"]</strong>。 DataTable 的鬆散型別在此範例中的事實，我們需要存取使用字串或序數索引的資料行名稱時顯露出來。 強型別的 DataTable，相反地，會有每個實作為屬性，其資料行如下所示的程式碼所導致：  <strong><em>DataTable</em>。資料列 [<em>index</em>]。*columnName</strong>*。

若要傳回強型別物件，開發人員可以建立自己的自訂商務物件或使用具類型資料集。 類別，其屬性通常會反映基礎資料庫資料表的商務物件的資料行代表商務物件被實作由開發人員。 輸入資料集是根據資料庫結構描述和其成員是強型別根據此結構描述的 Visual Studio 為您產生的類別。 輸入資料集本身包含的擴充 ADO.NET DataSet、 DataTable 和 DataRow 類別的類別。 除了強型別 DataTables，具類型資料集現在也包含 TableAdapters，也就是類別的方法來填入資料集的 Datatable 和傳播 Datatable 內回到資料庫的修改。

> [!NOTE]
> 如需有關使用具類型資料集與自訂商務物件的優缺點的詳細資訊，請參閱[設計資料層元件和傳遞資料透過層](https://msdn.microsoft.com/library/ms978496.aspx)。


這些教學課程的架構中，我們將使用強型別資料集。 [圖 3] 說明使用具類型資料集的應用程式的不同層之間的工作流程。


[![所有的資料存取程式碼會轉移到 DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**圖 3**： 所有的資料存取程式碼會轉移到 DAL ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>建立具類型資料集和資料表配接器

若要開始建立我們的 DAL 中，我們先將具類型資料集加入至我們的專案。 若要完成這項作業，以滑鼠右鍵按一下 方案總管 中的專案節點並選擇 加入新項目。 從範本清單中選取 [資料集] 選項並將它命名**Northwind.xsd**。


[![選擇將新的資料集加入至您的專案](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**圖 4**： 選擇將新的資料集加入至您的專案 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image10.png))


當系統提示您加入資料集，請按一下 [新增] 之後,**應用程式\_程式碼**資料夾中，選擇 [是]。 輸入資料集的設計工具接著會出現，並將啟動 TableAdapter 組態精靈，可讓您將您的第一個 TableAdapter 加入至具類型資料集。

輸入資料集做為強型別集合的資料;它是由強型別的 DataTable 執行個體，其中每個則包含強型別 DataRow 執行個體所組成。 針對每個基礎資料庫資料表，我們需要使用在本教學課程系列中，我們將建立強型別的 DataTable。 讓我們開始建立的 DataTable**產品**資料表。

請注意，強型別 DataTables 不包含任何有關如何存取其基礎資料庫資料表的資料。 若要擷取的資料來填入 DataTable，我們會使用 TableAdapter 類別，作為我們的資料存取層。 如我們**產品**DataTable，TableAdapter 將包含方法**GetProducts()**， **GetProductByCategoryID (*categoryID*)** 等等，我們將從展示層叫用。 DataTable 的角色是做為用來在各層之間傳遞資料的強型別物件。

TableAdapter 組態精靈 一開始會提示您選取要使用哪一個資料庫。 下拉式清單會顯示伺服器總管 中的這些資料庫。 如果您沒有 Northwind 資料庫新增到 [伺服器總管] 中，您可以按一下 [新增連接] 按鈕，此時若要這樣做。


[![從下拉式清單中選擇 Northwind 資料庫](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**圖 5**： 從下拉式清單中選擇 Northwind 資料庫 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image13.png))


之後選取資料庫，然後按一下 下一步，您需要您是否想要儲存中的連接字串**Web.config**檔案。 儲存連接字串中，您會避免讓硬式編碼在 TableAdapter 類別，可簡化項目，如果連接字串資訊在未來變更。 如果您選擇將連接字串儲存在組態檔中將它放在**&lt;connectionStrings&gt;** 區段中，它可以是[選擇性地加密](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)改善安全性或稍後透過 IIS GUI 管理工具，更適合用於系統管理員在新的 ASP.NET 2.0 屬性頁修改過。


[![將連接字串儲存至 Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**圖 6**： 將連接字串儲存到**Web.config** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image16.png))


接下來，我們需要定義結構描述的第一個強型別的 DataTable，並提供我們 TableAdapter 填入強型別資料集時所要使用的第一個方法。 這兩個步驟來建立我們想要在我們的 DataTable 中反映從資料表傳回資料行的查詢會同時完成的。 在精靈結尾處我們將提供的方法名稱，此查詢。 一旦已達成，則可以叫用這個方法從我們的展示層。 方法會執行已定義的查詢，並填入強型別的 DataTable。

若要開始定義 SQL 查詢，我們必須先指示我們要如何讓發出查詢的 TableAdapter。 我們可以使用特定 SQL 陳述式，建立新的預存程序，或使用現有的預存程序。 這些教學課程中，我們將使用特定 SQL 陳述式。 請參閱[Brian Noyes](http://briannoyes.net/)的文章[建置以 Visual Studio 2005 的 DataSet 設計工具的資料存取層](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)如需使用預存程序的範例。


[![查詢使用特定 SQL 陳述式的資料](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**圖 7**： 查詢使用特定 SQL 陳述式的資料 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image19.png))


此時我們可以輸入 SQL 查詢中以手動方式。 在 TableAdapter 中建立第一種方法時您通常會想要讓查詢傳回需要來表示對應的 DataTable 中的資料行。 我們可以達成此目的建立的查詢會傳回所有資料行和所有資料列**產品**資料表：


[![在文字方塊中輸入 SQL 查詢](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**圖 8**： 輸入 SQL 查詢至文字方塊中 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image22.png))


或者，使用查詢產生器，並以圖形方式建立查詢，如 圖 9 所示。


[![透過查詢編輯器中，以圖形方式建立查詢](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**圖 9**： 建立查詢，以圖形方式透過查詢編輯器中 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image25.png))


建立查詢，之後再移到下一個畫面中，按一下 [進階選項] 按鈕。 在網站專案中，「 產生 Insert、 Update 和 Delete 陳述式 」 是唯一進階預設; 選取的選項如果您從類別庫或 Windows 專案執行此精靈也會選取 使用開放式並行存取 」 選項。 核取 使用開放式並行存取 」 選項現在。 在未來的教學課程中，我們將檢驗開放式並行存取。


[![選取 只產生 Insert、 Update 和 Delete 陳述式選項](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**圖 10**： 選取 只產生 Insert、 Update 和 Delete 陳述式選項 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image28.png))


驗證之後的進階的選項，按一下 [下一步] 繼續進行最後的畫面。 這裡我們會要求選取要加入至 TableAdapter 方法。 有兩種模式適用於填入資料：

- **填入 DataTable**使用這種方法建立方法會採用做為參數的 DataTable 中，並填入它根據查詢的結果。 ADO.NET DataAdapter 類別，例如，實作此模式，以其**fill （)** 方法。
- **傳回 DataTable**使用這種方法方法會建立並會為您填入 DataTable，並傳回為方法傳回值。

您可以實作一個或以上兩種模式的 TableAdapter。 您也可以重新命名此處提供的方法。 我們將保持核取，這兩個核取方塊，即使我們將只使用在這些教學課程的第二個模式。 此外，請將重新命名而不是泛型**GetData**方法來**GetProducts**。

最後一個核取方塊，「 GenerateDBDirectMethods 」，若選取此選項，建立**insert （)**， **update （)**，並**delete （)** TableAdapter 方法。 如果您核取此選項，所有的更新必須透過 TableAdapter 的唯一**update （)** 方法，後者會採用型別資料集、 DataTable、 單一的 DataRow 或陣列中的 Datarow。 (如果您已經圖 9 中的進階屬性此核取方塊未核取產生 Insert、 Update 和 Delete 陳述式 > 」 選項設定不會影響。)讓我們保留選取此核取方塊。


[![從 GetData 中變成 GetProducts 的方法名稱](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**圖 11**： 將方法名稱，從**GetData**要**GetProducts** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image31.png))


按一下 [完成] 來完成精靈。 精靈關閉之後我們會回到 DataSet 設計工具會顯示我們剛才建立的 DataTable。 您可以看到清單中的資料行**產品**DataTable (**ProductID**， **ProductName**等等)，以及方法的**ProductsTableAdapter** (**fill （)** 並**GetProducts()**)。


[![產品 DataTable 和 ProductsTableAdapter 已加入至具類型資料集](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**圖 12**:**產品**DataTable 和**ProductsTableAdapter**已加入至具類型資料集 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image34.png))


現在我們有單一的 datatable 的具類型資料集 (**Northwind.Products**) 和強型別資料配接器類別 (**NorthwindTableAdapters.ProductsTableAdapter**) 與**GetProducts()** 方法。 這些物件可以用來從類似的程式碼存取的所有產品清單中：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

此程式碼不需要我們編寫一個位元的資料存取特定的程式碼。 我們不需要任何 ADO.NET 類別具現化，我們沒有參考任何連接字串，SQL 查詢或預存程序。 相反地，TableAdapter 將為我們提供低層級的資料存取程式碼。

此範例中使用的每個物件也是強型別，讓 Visual Studio 提供 IntelliSense 和編譯時期類型檢查。 和最佳的 TableAdapter 所傳回的所有通用值可以繫結至 ASP.NET 資料 Web 控制項，例如 GridView、 DetailsView、 DropDownList、 CheckBoxList 和其他許多選項。 下列範例說明繫結所傳回 DataTable **GetProducts()** 方法，以在只掃描三行程式碼內的 GridView**頁\_負載**事件處理常式。

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![在 GridView 中顯示產品清單](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**圖 13**: The List of Products GridView 中顯示 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image37.png))


雖然此範例會要求我們在我們的 ASP.NET 網頁中撰寫三行程式碼**網頁\_負載**事件處理常式中，在未來我們將檢驗如何以宣告方式擷取資料的使用 ObjectDataSource 教學課程DAL。 使用 ObjectDataSource 我們將不需要撰寫任何程式碼，並將取得以及分頁和排序支援 ！

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>步驟 3： 加入參數化資料存取層的方法

此時我們**ProductsTableAdapter**類別具有一種方法，但**GetProducts()**，它會先傳回的所有產品，在資料庫中。 時能夠使用 所有產品一定很有用，但也有我們會的想来擷取特定產品或屬於特定類別的所有產品的相關資訊。 若要將這類功能加入我們的資料存取層中，我們可以參數化的方法加入 TableAdapter 中。

讓我們新增**GetProductsByCategoryID (*categoryID*)** 方法。 若要將新方法新增至 DAL，返回 DataSet 設計工具中，以滑鼠右鍵按一下**ProductsTableAdapter**區段，然後選擇 加入查詢。


![TableAdapter 上按一下滑鼠右鍵，然後選擇 新增查詢](creating-a-data-access-layer-cs/_static/image38.png)

**圖 14**: TableAdapter 上按一下滑鼠右鍵，然後選擇 新增查詢


我們先提示您有關我們是否想要存取使用特定 SQL 陳述式或新的或現有的預存程序的資料庫。 讓我們選擇使用特定 SQL 陳述式一次。 接下來，我們會要求我們想要使用的 SQL 查詢的類型。 因為我們想要傳回屬於指定分類的所有產品，我們想要撰寫**選取**陳述式會傳回資料列。


[![選擇建立 SELECT 陳述式會傳回資料列](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**圖 15**： 選擇建立**選取**陳述式的傳回資料列 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image41.png))


下一個步驟是定義用來存取資料的 SQL 查詢。 因為我們想要傳回屬於特定類別目錄的產品，我會使用相同<strong>選取 </strong>陳述式，從<strong>GetProducts()</strong>，但新增下列<strong>其中</strong>子句：<strong>其中 CategoryID = @CategoryID</strong> 。 <strong>@CategoryID</strong>參數表示 TableAdapter 精靈，我們要建立的方法需要對應的型別 （也就是可為 null 的整數） 的輸入的參數。


[![輸入查詢，以便只傳回指定類別中的 產品](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**圖 16**： 輸入的查詢只傳回產品在指定的類別 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image44.png))


最後一個步驟中，我們可以選擇什麼資料存取模式來使用，以及自訂產生的方法的名稱。 填滿模式，讓我們變更名稱以<strong>FillByCategoryID</strong> ，並傳回的 DataTable 傳回模式 (<strong>取得*X</strong>* 方法)，讓我們使用<strong>GetProductsByCategoryID</strong>。


[![選擇 TableAdapter 方法的名稱](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**圖 17**： 選擇 TableAdapter 方法的名稱 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image47.png))


完成精靈之後，DataSet 設計工具會包含新的 TableAdapter 方法。


![產品可以現在查詢依類別目錄](creating-a-data-access-layer-cs/_static/image48.png)

**圖 18**: 產品可以現在查詢依類別目錄


請花一點時間加入**GetProductByProductID (*productID*)** 方法使用相同的技巧。

您可以測試這些參數化的查詢，直接從 DataSet 設計工具。 TableAdapter 中的方法上按一下滑鼠右鍵，然後選擇 預覽資料。 接下來，輸入要使用參數，並按一下 [預覽] 的值。


[![會顯示 「 飲料 」 分類這些產品屬於](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**圖 19**： 會顯示這些產品屬於 「 飲料 」 分類 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image51.png))


具有**GetProductsByCategoryID (*categoryID*)** 方法我們的 DAL 中，我們現在可以建立 ASP.NET 網頁，只會顯示這些產品在指定的分類。 下列範例會顯示所有產品中的飲料類別目錄中，具有**CategoryID**為 1。

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![會顯示 「 飲料 」 分類中的這些產品](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**圖 20**： 會顯示這些產品中的飲料類別目錄 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>步驟 4： 插入、 更新和刪除資料

有兩個常用於插入、 更新和刪除資料的模式。 第一個模式，我將稱之為資料庫直接存取模式，牽涉到建立方法，叫用時，問題**插入**，**更新**，或**刪除**命令單一資料庫記錄運作的資料庫。 一系列對應的純量值 （整數、 字串、 布林值、 日期時間等等） 通常會傳遞這類方法，將值插入、 更新或刪除。 例如，使用此模式**產品**資料表的 delete 方法會採用整數參數，指出**ProductID**要插入方法會需要時刪除的記錄字串，表示**ProductName**、 針對十進位**UnitPrice**，整數**UnitsOnStock**，依此類推。


[![每個插入、 更新和刪除要求傳送至立即資料庫](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**圖 21**： 每個插入、 更新和刪除要求傳送至立即資料庫 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image57.png))


其他模式，我將稱它為批次更新模式，是更新整個資料集、 DataTable 或 Datarow 的一個方法呼叫中的集合。 利用此模式為開發人員會刪除、 插入、 和修改 Datarow 的 DataTable 中並接著將這些資料行或 DataTable 傳遞到更新方法。 然後此方法列舉傳入 Datarow，決定是否它們已被修改、 新增或刪除 (透過 DataRow [RowState 屬性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)值)，以及發出適當的資料庫要求每一筆記錄。


[![在叫用 Update 方法時與資料庫同步的所有變更](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**圖 22**： 所有變更會與同步都處理資料庫時叫用 Update 方法 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter 會根據預設，會使用批次更新模式，但也支援 DB 直接模式。 我們選取 產生 Insert、 Update 和 Delete 陳述式 > 」 選項從 進階內容時建立我們的 TableAdapter，因為**ProductsTableAdapter**包含**update （)** 方法它會實作批次更新模式。 具體來說，包含 TableAdapter **update （)** 可以傳遞具類型資料集、 強型別的 DataTable 或一或多個 Datarow 的方法。 如果您選取當第一次建立 TableAdapter 的 DB 直接模式會也可透過實作 「 GenerateDBDirectMethods 」 核取方塊**insert （)**， **update （)**，和**delete （)** 方法。

這兩個資料修改模式使用的 TableAdapter **InsertCommand**， **UpdateCommand**，並**DeleteCommand**屬性，以發出其**插入**，**更新**，以及**刪除**命令至資料庫。 您可以檢查和修改**InsertCommand**， **UpdateCommand**，並**DeleteCommand** DataSet 設計工具的 TableAdapter 上按一下，然後再將屬性到 [屬性] 視窗中。 (請確定您已選取的 TableAdapter，且**ProductsTableAdapter**物件是在 [屬性] 視窗中的下拉式清單中選取。)


[![TableAdapter 具有 InsertCommand、 UpdateCommand，以及 DeleteCommand 屬性](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**圖 23**: tableadapter **InsertCommand**， **UpdateCommand**，和**DeleteCommand**屬性 ([按一下可觀看完整大小的檢視映像](creating-a-data-access-layer-cs/_static/image63.png))


若要檢查或修改其中任何一個資料庫的命令屬性，按一下**CommandText**子屬性，這會顯示 查詢產生器。


[![設定 查詢產生器中的 INSERT、 UPDATE 和 DELETE 陳述式](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**圖 24**： 設定**插入**，**更新**，和**刪除**查詢產生器中的陳述式 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image66.png))


下列程式碼範例示範如何使用批次更新模式，不會停止，且包含 25 個單位庫存以內的所有產品的價格的兩倍：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

下列程式碼說明如何使用 DB 直接模式以程式設計方式刪除特定的產品，然後更新其中一個，然後再加入 新的：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>建立自訂 Insert、 Update 和 Delete 方法

**Insert （)**， **update （)**，並**delete （)** DB 直接方法所建立的方法可以是有點麻煩，特別是針對包含許多資料行的資料表。 查看先前的程式碼範例中，沒有 IntelliSense 的協助不特別清楚什麼**產品**資料表資料行對應至每個輸入參數**update （)** 和**insert （)** 方法。 有時候我們可能只想要更新單一資料行或兩個，或想要自訂**insert （)** 方法，或許會傳回值的新插入的資料錄**識別**（自動遞增）欄位。

若要建立這種自訂的方法，請返回 DataSet 設計工具。 TableAdapter 上按一下滑鼠右鍵，然後選擇 加入查詢，回到 Tabaleadapter 精靈。 在第二個畫面中，我們可以指示來建立查詢的類型。 讓我們建立的方法加入新的產品，然後傳回新加入的資料錄的值**ProductID**。 因此，若要建立 opt**插入**查詢。


[![建立方法，以將新的資料列加入至 Products 資料表](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**圖 25**： 建立方法，以加入新資料列**產品**資料表 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image69.png))


在下一個畫面**InsertCommand**的**CommandText**隨即出現。 藉由新增增強這項查詢**選取範圍\_IDENTITY()** 查詢的結尾，這會傳回最後插入的 identity 值**識別**相同範圍中的資料行。 (請參閱[技術文件](https://msdn.microsoft.com/library/ms190315.aspx)如需詳細資訊**範圍\_IDENTITY()** 以及為什麼您可能想要[使用範圍\_IDENTITY() 替代 @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).)請確定您最終**插入**以分號的陳述式，然後再加入**選取**陳述式。


[![加強查詢傳回 scope_identity （） 值](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**圖 26**： 增強查詢來傳回**範圍\_IDENTITY()** 值 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image72.png))


最後，名稱的新方法**InsertProduct**。


[![將新的方法名稱設定為 InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**圖 27**： 將新的方法名稱設定為**InsertProduct** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image75.png))


當您返回 DataSet 設計工具您會看到**ProductsTableAdapter**包含一個新方法**InsertProduct**。 如果這個新的方法沒有參數可以用來在每個資料行**產品**資料表，可能會忘記終止**插入**以分號的陳述式。 設定**InsertProduct**方法，並確定您有以分號分隔**插入**並**選取**陳述式。

根據預設，insert 方法問題非查詢方法，這表示它們會傳回受影響的資料列數目。 不過，我們想**InsertProduct**方法傳回查詢，而不是受影響的資料列的數目所傳回的值。 若要達成此目的，調整**InsertProduct**方法的**ExecuteMode**屬性設**純量**。


[![將 ExecuteMode 屬性變更為純量](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**圖 28**： 變更**ExecuteMode**屬性設**純量**([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image78.png))


下列程式碼示範這個新**InsertProduct**作用中的方法：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>步驟 5： 完成資料存取層

請注意， **ProductsTableAdapters**類別會傳回**CategoryID**並**SupplierID**值**產品**資料表，但不包含**CategoryName**資料行**類別**表格或**CompanyName**資料行從**供應商**資料表中，雖然這些可能是我們想要顯示的產品資訊時顯示的資料行。 我們可以擴充 TableAdapter 的第一個方法， **GetProducts()**，以同時包含**CategoryName**並**CompanyName**將更新的資料行值包含這些新資料行以及的強型別的 DataTable。

這可能會造成問題，不過，為 TableAdapter 的方法來插入、 更新和刪除的資料根據這個初始的方法。 幸運的是，自動產生的方法插入、 更新和刪除不會受到中的子查詢**選取**子句。 藉由將我們的查詢，以處理**分類**並**供應商**成子查詢，而非**加入**s，我們將可避免不必重做這些修改資料的方法。 以滑鼠右鍵按一下**GetProducts()** 方法中的**ProductsTableAdapter** ，然後選擇 設定。 然後，調整**選取**子句，使它看起來像：

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![SELECT 陳述式更新 GetProducts() 方法](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**圖 29**： 更新**選取**陳述式**GetProducts()** 方法 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image81.png))


在更新之後**GetProducts()** 方法，以使用此資料表會包含兩個新的資料行的新查詢： **CategoryName**並**供應商名稱**。


![產品 DataTable 有兩個新的資料行](creating-a-data-access-layer-cs/_static/image82.png)

**圖 30**:**產品**DataTable 有兩個新的資料行


請花一點時間更新**選取**中的子句**GetProductsByCategoryID (*categoryID*)** 以及方法。

如果您更新**GetProducts()** **選取**使用**聯結**語法 DataSet 設計工具將無法自動產生插入、 更新和刪除方法使用 DB 直接模式的資料庫資料。 相反地，您必須手動建立它們更像我們一樣**InsertProduct**稍早在本教學課程中的方法。 此外，您會以手動方式必須提供**InsertCommand**， **UpdateCommand**，並**DeleteCommand**屬性值，如果您想要使用批次更新模式。

## <a name="adding-the-remaining-tableadapters"></a>新增其餘的 TableAdapters

目前為止，我們只探討了使用單一資料庫資料表的單一 TableAdapter。 不過，Northwind 資料庫包含數個相關的資料表，我們必須在我們的 web 應用程式中使用。 輸入資料集可以包含多個相關的 Datatable。 因此，若要完成我們的 DAL 我們需要加入 Datatable，我們將使用這些教學課程中的其他資料表。 新增新的 TableAdapter 具類型資料集，請開啟 DataSet 設計工具、 在設計師中，以滑鼠右鍵按一下並選擇 [新增] / TableAdapter。 這會建立新的 DataTable 和 TableAdapter，並逐步引導您完成精靈中稍早在本教學課程中，我們檢查。

需要幾分鐘的時間來建立下列的 TableAdapters 和使用下列查詢的方法。 請注意，在查詢**ProductsTableAdapter**包含子查詢，擷取每個產品類別目錄和供應商的名稱。 此外，如果您已遵循，您已新增**ProductsTableAdapter**類別的**GetProducts()** 並**GetProductsByCategoryID (*categoryID*)** 方法。

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![DataSet 設計工具後已新增四個 TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**圖 31**: 資料集設計工具後四個 Tableadapter 已新增 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>DAL 中加入自訂程式碼

Datatable 加入至具類型資料集與 Tableadapter 會表示為 XML 結構描述定義檔案 (**Northwind.xsd**)。 您可以檢視此結構描述資訊，以滑鼠右鍵按一下**Northwind.xsd**檔案在方案總管] 中，然後選擇 [檢視程式碼。


[![XML 結構描述定義 (XSD) 檔案，如 Northwinds 類型資料集](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**圖 32**: Northwinds 具類型資料集的 XML 結構描述定義 (XSD) 檔案 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image88.png))


此結構描述資訊轉譯為 C# 或 Visual Basic 程式碼在執行階段編譯時，或在執行階段 （如有需要），此時您可以透過它與偵錯工具。 若要檢視此自動產生的程式碼移至類別檢視] 和 [向下切入向下至 TableAdapter 或類型 DataSet 類別。 如果您沒有看到螢幕上的 [類別] 檢視，請移至 [檢視] 功能表並選取從該處，或按 Ctrl + Shift + C。 從 類別檢視中，您可以看到屬性、 方法和型別資料集和 TableAdapter 類別的事件。 若要檢視特定方法的程式碼，連按兩下 類別檢視中的方法名稱或以滑鼠右鍵按一下它，並選擇 移至定義。


![選擇 移至定義，從 類別檢視中檢查自動產生的程式碼](creating-a-data-access-layer-cs/_static/image89.png)

**圖 33**： 選擇 移至定義，從 類別檢視中檢查自動產生的程式碼


雖然自動產生的程式碼可以將大量時間，程式碼通常是非常廣泛，而且需要自訂以符合應用程式的獨特需求。 擴充自動產生的程式碼的風險，是工具產生程式碼可能會決定該是時候 「 重新產生 」，並覆寫您自訂。 使用.NET 2.0 的新的部分類別概念，很容易將類別分割到多個檔案。 這可讓我們不必擔心我們的自訂覆寫的 Visual Studio 中將自己的方法、 屬性和事件新增至自動產生的類別。

為了示範如何自訂 DAL，讓我們新增**GetProducts()** 方法來**SuppliersRow**類別。 **SuppliersRow**類別表示中的單一記錄**供應商**資料表，許多產品，每個供應商可以提供者零因此**GetProducts()** 會傳回那些指定的供應商的產品。 若要完成這會建立新的類別檔案中**應用程式\_程式碼**名為資料夾**SuppliersRow.cs**並加入下列程式碼：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

此部分類別會指示編譯器，當建置**Northwind.SuppliersRow**類別，以包含**GetProducts()** 我們剛才定義的方法。 如果您建置專案，然後返回 類別檢視您會看到**GetProducts()** 現在會列為方法**Northwind.SuppliersRow**。


![GetProducts() 方法屬於現在 Northwind.SuppliersRow 類別](creating-a-data-access-layer-cs/_static/image90.png)

**圖 34**: **GetProducts()** 方法是現在的一部分**Northwind.SuppliersRow**類別


**GetProducts()** 方法現在可用來列舉集合的產品特定供應商，下列程式碼所示：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

這項資料也可以在任何 ASP 中顯示。NET 的資料 Web 控制項。 下列網頁會使用兩個欄位中的 GridView 控制項：

- 顯示名稱的每個供應商、 BoundField 和
- 包含 BulletedList 控制項繫結至所傳回的結果為 TemplateField **GetProducts()** 每個供應商的方法。

我們將檢驗如何顯示在未來的教學課程的這類主版詳細資料報表。 現在，此範例設計來說明使用自訂方法加入至**Northwind.SuppliersRow**類別。

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![供應商的公司名稱會列在左資料行，其在右側的產品](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**圖 35**: 供應商的公司名稱會列在左資料行，其在右側的產品 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>總結

當建置 web 應用程式建立 DAL 應該是您第一個步驟，開始建立展示層之前發生。 使用 Visual Studio，建立具類型資料集為基礎的 DAL 是可以在 10-15 分鐘內完成，而不需要撰寫一行程式碼的工作。 邁向未來的教學課程會以這個 DAL 為基礎。 在 [下一個教學課程](creating-a-business-logic-layer-cs.md)我們會定義數個商務規則，並了解如何在不同的商務邏輯層中實作它們。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建置的 DAL 使用強型別 TableAdapters 和 VS 2005 和 ASP.NET 2.0 中的 Datatable](https://weblogs.asp.net/scottgu/435498)
- [設計資料層元件和傳遞資料層](https://msdn.microsoft.com/library/ms978496.aspx)
- [建置以 Visual Studio 2005 的 DataSet 設計工具的資料存取層](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [ASP.NET 2.0 中的組態資訊加密應用程式](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter 概觀](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [使用具類型資料集](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [使用 Visual Studio 2005 與 ASP.NET 2.0 中的強型別資料存取](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [如何擴充 TableAdapter 方法](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [從預存程序中擷取純量資料](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程中所包含的主題的影片訓練

- [ASP.NET 應用程式中的資料存取層](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [如何以手動方式將資料集繫結到資料格](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [如何使用資料集和篩選從 ASP 應用程式](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者是 Ron 綠色、 Hilton giesenow 將、 Dennis Patterson、 Liz Shulok，Abel Gomez 和 Carlos Santos。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](creating-a-business-logic-layer-cs.md)
