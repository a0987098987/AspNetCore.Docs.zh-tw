---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: "建立資料存取層 (C#) |Microsoft 文件"
author: rick-anderson
description: "本教學課程中我們會從一開始啟動並建立資料存取層 (DAL)，使用具類型資料集，來存取資料庫中的資訊。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 927b2490b5c539a79bb9939b88942499b23cc464
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-data-access-layer-c"></a>建立資料存取層 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe)或[下載 PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> 本教學課程中我們會從一開始啟動並建立資料存取層 (DAL)，使用具類型資料集，來存取資料庫中的資訊。


## <a name="introduction"></a>簡介

與 web 開發人員，我們的生活心力都圍繞處理的資料。 我們會建立資料庫來儲存資料時，程式碼來擷取和修改它，並以收集及彙總的網頁。 這是將探索實作這些常見的模式，在 ASP.NET 2.0 技術冗長系列的第一個教學課程。 我們將開始建立[軟體架構](http://en.wikipedia.org/wiki/Software_architecture)組成的資料存取層 (DAL) 使用具類型的資料集，商務邏輯層 (BLL)，會強制執行自訂商務規則，以及展示層由 ASP.NET 的頁面共用通用的頁面配置。 一旦這個後端基礎已配置，我們將會移入報告，顯示如何顯示，請彙總、 收集和驗證 web 應用程式的資料。 這些教學課程被專很簡潔，並提供具有足夠的螢幕擷取畫面來引導您完成程序以視覺化方式的逐步指示。 每一個教學課程使用在 C# 和 Visual Basic 版本，包括使用的完整程式碼的下載。 （本教學課程中第一個是相當冗長，但其餘部分會更容易吸收的區塊 （chunk）。

這些教學課程中我們將使用放在 Northwind 資料庫的 Microsoft SQL Server 2005 Express 的 Edition 版本**應用程式\_資料**目錄。 資料庫檔案，除了**應用程式\_資料**資料夾還包含用於建立資料庫的 SQL 指令碼，如果您想要使用不同的資料庫版本。 這些指令碼可能也很[直接從 Microsoft 下載](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)，如果您想使用。 如果您使用不同的 SQL Server 版本的 Northwind 資料庫，您必須更新**NORTHWNDConnectionString**設定在應用程式的**Web.config**檔案。 使用 Visual Studio 2005 Professional Edition 做為檔案系統為基礎的網站專案建置的 web 應用程式。 不過，所有教學課程將運作地一樣良好的免費版本的 Visual Studio 2005 中[Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/)。  
  
本教學課程中我們會從一開始啟動並建立資料存取層 (DAL)，後面接著第二個教學課程中，以建立商務邏輯層 (BLL)，然後處理的頁面配置與第三個中的導覽。 教學課程之後的第三個是會根據基礎的配置方式在前三個。 我們有很多來涵蓋如何在此教學課程中第一個，所以啟動 Visual Studio，讓我們開始吧 ！

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>步驟 1： 建立 Web 專案，並連接到資料庫

我們可以建立我們的資料存取層 (DAL) 之前，我們先必須建立網站，並設定資料庫。 開始建立新檔案系統以 ASP.NET 網站。 若要這麼做，請移至 檔案 功能表並選擇新的網站，顯示新的網站 對話方塊。 選擇 ASP.NET 網站範本、 設定到檔案系統位置下拉式清單中，選擇要將網站的資料夾和設定的語言為 C#。


[![建立新檔案系統為基礎的網站](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**圖 1**： 建立 New File System-Based 網站 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image3.png))


這會建立新的網站與**Default.aspx** ASP.NET 網頁和**應用程式\_資料**資料夾。

建立網站後下, 一個步驟是在 Visual Studio 伺服器總管中加入資料庫參考。 將資料庫加入至 [伺服器總管] 中，您可以加入資料表、 預存程序、 檢視和等等全部從 Visual Studio 內。 您也可以檢視資料表的資料，或透過查詢產生器以手動方式或以圖形方式建立您自己的查詢。 此外，當我們建立具類型資料集為 DAL 我們必須點 Visual Studio 應該建構輸入資料集的資料庫。 雖然我們可以提供此階段中該時間點的連接資訊，Visual Studio 會自動填入下拉式清單已在 [伺服器總管] 中註冊的資料庫。

將 Northwind 資料庫加入至 [伺服器總管] 中的步驟取決於您是否想要使用的 SQL Server 2005 Express Edition 資料庫**應用程式\_資料**資料夾，或如果您有 Microsoft SQL Server 2000 或 2005年您想要改為使用的資料庫伺服器設定。

## <a name="using-a-database-in-theappdatafolder"></a>使用資料庫中 theApp\_DataFolder

如果您沒有 SQL Server 2000 或 2005年的資料庫伺服器連線，或您只想要避免將資料庫加入至資料庫伺服器，您可以使用位於下載 websit Northwind 資料庫的 SQL Server 2005 Express Edition 版本e's**應用程式\_資料**資料夾 (**NORTHWND。MDF**)。

將資料庫置於**應用程式\_資料**資料夾會自動加入至 [伺服器總管] 中。 假設您有 SQL Server 2005 Express Edition 安裝在您的電腦上應該會看到名為 NORTHWND 的節點。MDF 在伺服器總管中，您可以展開，並瀏覽其資料表、 檢視、 預存程序，以及其他 （請參閱圖 2）。

**應用程式\_資料**資料夾也可以保存 Microsoft Access **.mdb**它像其 SQL Server 的對應，會自動加入至 [伺服器總管] 中的檔案。 如果您不想要使用的任何 SQL Server 選項，您可以隨時[下載 Northwind 資料庫檔案的 Microsoft Access 版本](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN)拖放到**應用程式\_資料**目錄。 請記住，不過，Access 資料庫不是做為與 SQL Server 功能豐富，並且不可用於網站的案例。 此外，幾個 35 + 教學課程會使用不支援存取某些資料庫層級功能。

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>連接到 Microsoft SQL Server 2000 或 2005年的資料庫伺服器中的資料庫

或者，您可能會連接到資料庫伺服器上安裝 Northwind 資料庫。 如果資料庫伺服器還沒有安裝 Northwind 資料庫，您首先必須將它加入至資料庫伺服器執行包含在本教學課程下載或安裝指令碼[下載 Northwind SQL Server 2000 版安裝指令碼](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en)直接從 Microsoft 的網站。

安裝的資料庫之後，請移至 Visual Studio 中的伺服器總管，以滑鼠右鍵按一下資料連接] 節點，然後選擇 [加入連接。 如果您沒有看到 [伺服器總管] 中移至 [檢視 / 伺服器總管] 中或叫用的 Ctrl + Alt + S。 這會顯示加入連接 對話方塊，其中您可以指定要連接到伺服器，驗證資訊，並輸入資料庫名稱。 一旦您已成功設定資料庫連接資訊並按一下 [確定] 按鈕，資料庫將會加入做為資料連接的節點下的節點。 您可以展開 [資料庫] 節點，即可瀏覽資料表、 檢視、 預存程序，等等。


![將連接加入您的資料庫伺服器 Northwind 資料庫](creating-a-data-access-layer-cs/_static/image4.png)

**圖 2**： 將連接加入您的資料庫伺服器 Northwind 資料庫


## <a name="step-2-creating-the-data-access-layer"></a>步驟 2： 建立資料存取層

當使用資料的其中一個選項是直接將展示層的特定資料的邏輯內嵌 （在 web 應用程式，展示層 ASP.NET 網頁組成）。 這可能需要在 ASP.NET 網頁的程式碼部分撰寫 ADO.NET 程式碼或使用 SqlDataSource 控制項標記部分的格式。 在任一情況下，這種方法緊密結合的資料存取邏輯展示層。 不過，建議的方法是分開展示層中的資料存取邏輯。 此個別圖層資料存取層，DAL 簡稱，指，通常會實作為個別的類別庫專案。 這個多層式架構的優點有詳細的記錄 （結尾的本教學課程中的這些優點的詳細資訊，請參閱 「 進一步讀數 」 一節），而且我們會在這一系列中的方法。

基礎資料來源，例如建立資料庫的連接特有的所有程式碼發出**選取**，**插入**，**更新**，和**刪除**DAL 應該位命令，依此類推。 展示層不應該包含這類資料存取程式碼中，任何參考，但應該改進行所有的資料要求的 DAL 呼叫。 資料存取層級通常包含存取基礎資料庫資料的方法。 Northwind 資料庫中，例如，具有**產品**和**類別**記錄的產品銷售和其所屬的類別目錄的資料表。 在我們 DAL 我們將會有類似的方法：

- **GetCategories()，**這會傳回有關所有的分類資訊
- **GetProducts()**，這會傳回所有產品的相關資訊
- **GetProductsByCategoryID (*categoryID*)**，這會傳回屬於指定類別的所有產品
- **GetProductByProductID (*productID*)**，這會傳回特定產品的相關資訊

這些方法，叫用時，將連接到資料庫、 發出適當的查詢，並傳回結果。 我們決定傳回這些結果的方式很重要。 這些方法可能只會傳回資料集或 DataReader 填入資料庫查詢，但在理想情況下這些結果應該傳回使用*強型別物件*。 強型別物件是在編譯時期，嚴格定義其結構描述而相反，是透過鬆散型別的物件，則是其中一個執行階段之前不知道其結構描述。

比方說，DataReader 和 （依預設） 資料集是透過鬆散型別物件，因為其結構描述由用來填入資料庫查詢所傳回的資料行所定義。 若要從鬆散型別的 DataTable，我們需要使用類似下面的語法存取特定資料行: ***DataTable*。資料列 [*索引*] ["*columnName *"]**。 DataTable 的鬆散型別在此範例中會顯示我們需要存取使用字串或序數索引的資料行名稱的事實。 強型別 DataTable，相反地，會有每個實作為屬性，其資料行導致程式碼所示： ***DataTable*。資料列 [*索引*]。*columnName***。

若要傳回強型別物件，開發人員可以建立自己的自訂商務物件或使用型別資料集。 類別，其屬性通常會反映基礎資料庫資料表的商務物件的資料行表示為開發人員所實作的商務物件。 型別資料集是由基礎資料庫結構描述和其成員的強型別根據此結構描述上的 Visual Studio 為您產生的類別。 輸入資料集本身包含的擴充 ADO.NET 資料集、 DataTable 和 DataRow 類別的類別。 除了強型別 Datatable，具類型資料集現在也包含 TableAdapters，也就是具有填入資料集的 Datatable 和傳播回資料庫中的 Datatable 修改方法的類別。

> [!NOTE]
> 如需有關使用型別資料集與自訂商務物件的優缺點的詳細資訊，請參閱[設計的資料層元件和傳遞資料透過層](https://msdn.microsoft.com/library/ms978496.aspx)。


這些教學課程架構，我們會使用強型別資料集。 圖 3 說明使用具類型資料集的應用程式的不同層之間的工作流程。


[![所有的資料存取程式碼被貶 dal](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**圖 3**： 所有資料存取程式碼都貶 dal ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>建立具類型資料集和資料表配接器

若要開始建立 DAL 我們，我們一開始將輸入資料集加入至受測專案。 若要完成此動作，以滑鼠右鍵按一下 方案總管 中的專案節點並選擇 加入新項目。 從範本清單中選取資料集選項並將其命名**Northwind.xsd**。


[![選擇將新的資料集加入至您的專案](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**圖 4**： 選擇將新的資料集加入至您的專案 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image10.png))


當系統提示您加入資料集，按一下 [新增] 之後,**應用程式\_程式碼**資料夾中，選擇 [是]。 型別資料集的設計工具就會顯示，並將啟動 TableAdapter 組態精靈，可讓您將您的第一個 TableAdapter 加入至型別資料集。

型別資料集做為強型別集合的資料;它是由構成強型別 DataTable 執行個體，其中每個則包含強型別 DataRow 執行個體。 我們將建立強型別 DataTable，我們需要使用此教學課程系列的基礎資料庫資料表的每個。 讓我們開始建立 DataTable**產品**資料表。

請注意，強型別 Datatable 不會包含如何存取資料，從其基礎資料庫資料表上的所有資訊。 若要擷取的資料來填入 DataTable，我們使用的 TableAdapter 類別，可當做我們的資料存取層。 針對我們**產品**DataTable，TableAdapter 將包含方法**GetProducts()**， **GetProductByCategoryID (*categoryID*)**，依此類推，我們將會叫用從展示層。 DataTable 的角色是做為用來在各層之間傳遞資料的強型別物件。

TableAdapter 組態精靈一開始會提示您選取要使用哪一個資料庫。 下拉式清單會顯示在 [伺服器總管] 中這些資料庫。 如果您沒有 Northwind 資料庫新增到 [伺服器總管] 中，您可以按一下 [新增連接] 按鈕，此時若要這樣做。


[![從下拉式清單中選擇 Northwind 資料庫](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**圖 5**： 從下拉式清單中選擇 Northwind 資料庫 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image13.png))


之後選取資料庫，然後按一下 下一步，您將需要在想要儲存的連接字串中**Web.config**檔案。 儲存連接字串中，您可避免將它永久的自動程式碼中的 TableAdapter 類別，可簡化作業，如果連接字串資訊在未來變更。 如果您選擇將連接字串儲存在組態檔中將它放在 **&lt;connectionStrings&gt;** 區段中，它可以是[選擇性地加密](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)以改進安全性或修改過，稍後透過在 IIS GUI 管理工具中，這是系統管理員更理想新的 ASP.NET 2.0 屬性頁。


[![將連接字串儲存至 Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**圖 6**： 連接字串儲存到**Web.config** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image16.png))


接下來，我們需要定義結構描述的第一個的強型別資料表，並提供我們 TableAdapter 填入強型別資料集時所要使用的第一個方法。 這兩個步驟是同時完成建立查詢，傳回資料表中的資料行，我們想要在我們的 DataTable 中反映。 在精靈結束我們會提供此查詢的方法名稱。 一旦已達成，可以從我們展示層叫用此方法。 方法會執行已定義的查詢，並填入 DataTable 強型別。

若要開始定義我們必須先指示我們要如何發出查詢，TableAdapter 的 SQL 查詢。 我們可以使用特定 SQL 陳述式，建立新的預存程序，或使用現有的預存程序。 如需這些教學課程中，我們會使用特定 SQL 陳述式。 請參閱[Brian Noyes](http://briannoyes.net/)的發行項，[建置 Visual Studio 2005 DataSet 設計工具與資料存取層](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)針對使用預存程序的範例。


[![查詢的資料，使用特定 SQL 陳述式](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**圖 7**： 查詢的資料，使用特定 SQL 陳述式 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image19.png))


此時我們可以輸入 SQL 查詢中以手動方式。 在 TableAdapter 中建立第一種方法時您通常會想要讓查詢傳回要在對應資料表中以表示這些資料行。 我們可以完成此作業藉由建立可傳回的所有資料列和所有資料行的查詢**產品**資料表：


[![文字方塊中輸入 SQL 查詢](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**圖 8**： 輸入 SQL 查詢至文字方塊中 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image22.png))


或者，使用查詢產生器，並以圖形方式建立查詢，如圖 9 所示。


[![以圖形方式，透過查詢編輯器建立查詢](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**圖 9**: 查詢以圖形方式，透過查詢編輯器中建立 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image25.png))


建立查詢之後但之前移動到下一個畫面上，按一下 [進階選項] 按鈕。 在網站專案中，「 產生 Insert、 Update 和 Delete 陳述式 」 是唯一進階預設; 選取的選項如果您從 類別庫或 Windows 專案執行此精靈也會選取 使用開放式並行存取 的選項。 核取 [使用開放式並行存取] 選項現在。 在未來教學課程中，我們將檢驗開放式並行存取。


[![選取 只產生 Insert、 Update 和 Delete 陳述式選項](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**圖 10**： 選取 只產生 Insert、 Update 和 Delete 陳述式選項 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image28.png))


驗證之後的進階的選項，按一下 [下一步] 繼續進行最後的畫面。 這裡我們必須選取要加入至 TableAdapter 方法。 有兩種模式，用來填入資料：

- **填入 DataTable**透過這種方式建立方法接受 DataTable 做為參數，並於其中填入它根據查詢的結果。 ADO.NET 資料配接器類別，例如，實作此模式與其**fill （)**方法。
- **傳回 DataTable**透過這種方式方法會建立並為您填入 DataTable 並傳回該方法會傳回值。

您可以實作一或兩種模式的 TableAdapter。 您也可以重新命名此處提供的方法。 即使我們只會用在整個這些教學課程的第二種模式，讓我們保留選取此選項，這兩個核取方塊。 此外，請重新命名而不是泛型**GetData**方法**GetProducts**。

如果選取此選項，最後一個核取方塊，「"GenerateDBDirectMethods，會建立**insert**， **update （)**，和**delete （)** TableAdapter 方法。 如果您核取此選項，所有更新都必須透過 TableAdapter 的唯一**update （)**採用型別資料集、 DataTable、 單一資料列中或陣列中的資料行的方法。 (如果您未選取的 「 產生 Insert、 Update 和 Delete 陳述式 」 選項圖 9 中的進階屬性從這個核取方塊設定沒有任何作用。)讓我們保留選取此核取方塊。


[![GetData 方法名稱為 GetProducts 變更](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**圖 11**： 將方法名稱，從**GetData**至**GetProducts** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image31.png))


完成精靈，按一下 [完成]。 關閉精靈之後，我們會傳回 DataSet 設計工具會顯示我們剛才建立的 DataTable。 您可以查看中的資料行清單**產品**DataTable (**ProductID**， **ProductName**等等)，以及方法的**ProductsTableAdapter** (**fill （)**和**GetProducts()**)。


[![產品 DataTable 和 ProductsTableAdapter 已新增至輸入資料集](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**圖 12**:**產品**DataTable 和**ProductsTableAdapter**已新增至輸入資料集 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image34.png))


我們現在有單一的 datatable 的型別資料集 (**Northwind.Products**) 和強型別資料配接器類別 (**NorthwindTableAdapters.ProductsTableAdapter**) 與**GetProducts()**方法。 這些物件可以用來從類似的程式碼存取的所有產品的清單：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

此程式碼不需要我們寫入的資料存取特定的程式碼的一個位元。 我們不需要任何 ADO.NET 類別具現化，我們沒有任何連接字串，SQL 查詢，請參閱或預存程序。 相反地，TableAdapter 會為我們提供低階資料存取程式碼。

此範例中使用每個物件也是強型別，可讓 Visual Studio 提供 IntelliSense 和編譯時間類型檢查。 最棒的是 TableAdapter 所傳回的所有通用值可以繫結至 ASP.NET Web 控制項，例如 GridView、 DetailsView、 DropDownList、 CheckBoxList，和其他的資料。 下列範例會說明繫結所傳回 DataTable **GetProducts()**方法，以只掃描三行程式碼內的 GridView**頁面\_負載**事件處理常式。

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![產品的清單會顯示在 GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**圖 13**： 產品的清單會顯示在 GridView ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image37.png))


雖然這個範例需要在我們的 ASP.NET 頁面我們寫入三行程式碼，但**頁面\_負載**事件處理常式，在未來我們將檢驗如何以宣告方式擷取資料的使用 ObjectDataSource 教學課程DAL。 ObjectDataSource 與我們將不需要撰寫任何程式碼，並會取得分頁和排序支援時，以及 ！

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>步驟 3： 加入參數化資料存取層的方法

此時我們**ProductsTableAdapter**類別具有一種方法，但**GetProducts()**，它會傳回所有產品在資料庫中。 雖然無法使用的所有產品很明確有用的但有些的時候我們會想要擷取特定產品或屬於特定類別的所有產品的相關資訊。 將這類功能加入至我們的資料存取層中，我們可以將參數化的方法加入 TableAdapter。

讓我們加入**GetProductsByCategoryID (*categoryID*)**方法。 若要將新方法新增至 DAL 返回 DataSet 設計工具中，以滑鼠右鍵按一下**ProductsTableAdapter**區段，然後選擇 加入查詢。


![TableAdapter 上按一下滑鼠右鍵，然後選擇 新增查詢](creating-a-data-access-layer-cs/_static/image38.png)

**圖 14**： 在 TableAdapter 上按一下滑鼠右鍵，然後選擇 新增查詢


首先，我們會提示您是否我們要使用特定 SQL 陳述式或新的或現有的預存程序存取資料庫。 讓我們選擇使用特定 SQL 陳述式一次。 接下來，我們會要求何種類型的 SQL 查詢我們想要使用。 我們想要傳回屬於指定類別的所有產品，因為我們想要撰寫**選取**陳述式會傳回資料列。


[![選擇建立 SELECT 陳述式會傳回資料列](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**圖 15**： 選擇建立**選取**陳述式的傳回資料列 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image41.png))


下一個步驟是定義用來存取資料的 SQL 查詢。 由於我們想要傳回屬於特定類別的產品，我會使用相同**選取**陳述式從**GetProducts()**，但加入下列**其中**子句：**其中 CategoryID = @CategoryID** 。 **@CategoryID** 參數表示 TableAdapter 精靈正在建立的方法，將會要求輸入的參數的對應型別 （也就是可為 null 整數）。


[![輸入查詢只傳回指定的類別目錄中的 產品](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**圖 16**： 輸入查詢只傳回產品中指定的類別目錄 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image44.png))


最後一個步驟中，我們可以選擇資料存取模式來使用，以及自訂方法產生的名稱。 填滿模式，讓我們將名稱變更為**FillByCategoryID**和傳回的 DataTable 傳回模式 (**取得 * X*** 方法)，讓我們使用**GetProductsByCategoryID**.


[![選擇的 TableAdapter 方法的名稱](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**圖 17**： 選擇的 TableAdapter 方法的名稱 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image47.png))


完成精靈之後，在 DataSet 設計工具會包含新的 TableAdapter 方法。


![依分類產品可以立即進行查詢](creating-a-data-access-layer-cs/_static/image48.png)

**圖 18**: 產品可以立即查詢依類別目錄


請花一點時間加入**GetProductByProductID (*productID*)**方法使用相同的技巧。

您可以測試這些參數化的查詢，直接從 DataSet 設計工具。 TableAdapter 中的方法上按一下滑鼠右鍵，然後選擇 預覽資料。 接下來，輸入要使用的參數，並按一下 [預覽] 的值。


[![會顯示 「 飲料 」 分類這些產品屬於](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**圖 19**： 會顯示那些產品屬於 「 飲料 」 分類 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image51.png))


與**GetProductsByCategoryID (*categoryID*)**方法我們 DAL 中的，我們現在可以建立 ASP.NET 網頁，只會顯示這些產品指定類別中。 下列範例會顯示所有產品中的飲料類別目錄中，具有**CategoryID**為 1。

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![會顯示 「 飲料 」 分類中的那些產品](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**圖 20**： 會顯示那些產品飲料類別目錄中 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>步驟 4： 插入、 更新和刪除資料

有兩種模式通常用來插入、 更新和刪除資料。 第一個模式，我稱資料庫直接模式，包括建立方法，叫用時，問題**插入**，**更新**，或**刪除**命令對單一資料庫記錄的資料庫。 這類方法通常傳入一系列的對應的純量值 （整數、 字串、 布林值、 Datetime，等等），這些值来插入、 更新或刪除。 例如，使用這個模式**產品**資料表 delete 方法會採用整數參數，指出**ProductID**要插入方法會需要時刪除的記錄字串， **ProductName**、 十進位的**UnitPrice**，如整數**UnitsOnStock**，依此類推。


[![每個插入、 更新和刪除要求傳送至立即資料庫](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**圖 21**： 每個插入、 更新和刪除要求傳送至立即資料庫 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image57.png))


其他模式，它會參照為批次更新模式，是更新整個資料集、 資料表或其中一個方法呼叫中的資料行集合。 此模式與開發人員刪除、 插入、 修改 DataTable 中的資料行以及然後將這些資料行或資料表傳遞轉換成更新的方法。 然後此方法列舉傳入資料行，判斷是否已被修改、 加入，或刪除這些 (透過 DataRow [RowState 屬性](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx)值)，並會發出適當的資料庫要求的每一筆記錄。


[![與資料庫同步的所有變更時叫用 Update 方法](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**圖 22**： 與資料庫同步的所有變更時叫用 Update 方法 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter 會根據預設，會使用批次更新模式，但也支援 DB 直接模式。 因為我們的 「 產生 Insert、 Update 和 Delete 陳述式 」 選項從進階屬性建立時選取我們的 TableAdapter， **ProductsTableAdapter**包含**update （)**方法它會實作批次更新模式。 具體來說，TableAdapter 包含**update （)**類型 DataSet、 DataTable 強型別或一或多個資料行可以傳遞的方法。 如果您保留 「 GenerateDBDirectMethods 」 核取當第一次建立 TableAdapter 的 DB 直接模式將也會實作透過**insert**， **update （)**，和**delete （)**方法。

這兩個資料修改模式使用 TableAdapter 的**InsertCommand**， **UpdateCommand**，和**DeleteCommand**屬性，以發出其**插入**，**更新**，和**刪除**命令至資料庫。 您可以檢查和修改**InsertCommand**， **UpdateCommand**，和**DeleteCommand** DataSet 設計工具的 TableAdapter 上按一下，然後繼續屬性以 [屬性] 視窗中。 (請確定您已選取的 TableAdapter，而且**ProductsTableAdapter**物件是在 [屬性] 視窗中的下拉式清單中選取。)


[![TableAdapter 具有 InsertCommand，UpdateCommand，以及 DeleteCommand 屬性](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**圖 23**: tableadapter **InsertCommand**， **UpdateCommand**，和**DeleteCommand**屬性 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image63.png))


若要檢查或修改任何資料庫命令屬性，請按一下**CommandText**子屬性，將會出現 查詢產生器。


[![在 查詢產生器中設定 INSERT、 UPDATE 和 DELETE 陳述式](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**圖 24**： 設定**插入**，**更新**，和**刪除**查詢產生器中的陳述式 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image66.png))


下列程式碼範例示範如何使用不中斷，且包含 25 個單位庫存或更少的所有產品的價格增加兩倍的批次更新模式：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

下列程式碼說明如何以程式設計方式刪除特定產品，然後更新一個，使用 DB 直接模式，然後再加入一個新：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>建立自訂 Insert、 Update 和 Delete 方法

**Insert**， **update （)**，和**delete** DB 直接方法所建立的方法可以是一個位元很麻煩，特別是針對包含許多資料行的資料表。 查看先前的程式碼範例中，沒有 IntelliSense 的協助它並不特別清楚什麼**產品**資料表資料行對應至每個輸入參數**update （)**和**insert （)**方法。 有時候我們可能只想要更新單一資料行或兩個，或想要自訂**insert**方法可能會傳回新插入的記錄的值**識別**（自動遞增）欄位。

若要建立自訂方法，返回 DataSet 設計工具。 TableAdapter 上按一下滑鼠右鍵，然後選擇 加入查詢，返回 Tabaleadapter 精靈。 在第二個畫面中，我們可以指示來建立查詢的類型。 讓我們來建立的方法加入新的產品，然後傳回新加入的記錄的值**ProductID**。 因此，若要建立選擇**插入**查詢。


[![建立方法以將新的資料列加入至 Products 資料表](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**圖 25**： 建立方法以加入新的資料列至**產品**資料表 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image69.png))


在下一個畫面**InsertCommand**的**CommandText**隨即出現。 藉由新增擴充此查詢**選取範圍\_IDENTITY()**在查詢的結束時，它會傳回插入的最後一個識別值**識別**相同範圍中的資料行。 (請參閱[技術文件](https://msdn.microsoft.com/library/ms190315.aspx)如需有關**範圍\_IDENTITY()**以及為什麼您可能想要[使用範圍\_替代 IDENTITY() @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).)請確定您結束**插入**以分號分隔的陳述式，然後再加入**選取**陳述式。


[![加強查詢傳回 scope_identity （） 值](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**圖 26**： 加強查詢以傳回**範圍\_IDENTITY()**值 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image72.png))


最後，命名為新方法**InsertProduct**。


[![將新的方法名稱設定為 InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**圖 27**： 將新的方法名稱設定為**InsertProduct** ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image75.png))


當您返回 DataSet 設計工具會看到**ProductsTableAdapter**包含新方法， **InsertProduct**。 如果這個新的方法沒有參數可以用來在每個資料行**產品**資料表中，有可能是因為您忘記終止**插入**陳述式以分號分隔。 設定**InsertProduct**方法並確定您有以分號分隔**插入**和**選取**陳述式。

根據預設，insert 方法問題非查詢方法，這表示它們會傳回受影響的資料列數目。 不過，我們想**InsertProduct**方法來傳回查詢中，不受影響的資料列數所傳回的值。 若要完成這項作業，調整**InsertProduct**方法的**ExecuteMode**屬性**純量**。


[![將 ExecuteMode 屬性變更為純量](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**圖 28**： 變更**ExecuteMode**屬性**純量**([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image78.png))


下列程式碼示範這個新**InsertProduct**動作中的方法：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>步驟 5： 完成資料存取層

請注意， **ProductsTableAdapters**類別會傳回**CategoryID**和**SupplierID**值從**產品**資料表，但不包含**CategoryName**資料行從**類別**資料表或**CompanyName**資料行從**供應商**資料表中，雖然這些可能是我們想要顯示產品的相關資訊時顯示的資料行。 我們可以擴充 TableAdapter 的第一個方法， **GetProducts()**，以同時包含**CategoryName**和**CompanyName**資料行值，將會更新強型別來包含這些新資料行以及 DataTable。

這可能會造成問題，不過，為 TableAdapter 的方法來插入、 更新和刪除資料會根據這個初始的方法。 幸運的是，自動產生的方法為插入、 更新及刪除未受到中的子查詢**選取**子句。 所要加入我們的查詢處理**類別**和**供應商**成子查詢，而非**聯結**s，我們將會避免重做這些修改資料的方法。 以滑鼠右鍵按一下**GetProducts()**方法中的**ProductsTableAdapter**並選擇 [設定]。 然後，調整**選取**子句，使它看起來像：

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![SELECT 陳述式更新 GetProducts() 方法](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**圖 29**： 更新**選取**陳述式**GetProducts()**方法 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image81.png))


在更新之後， **GetProducts()**方法，以使用這個資料表會包含兩個新的資料行的新查詢： **CategoryName**和**供應商名稱**。


![產品資料表有兩個新的資料行](creating-a-data-access-layer-cs/_static/image82.png)

**圖 30**:**產品**DataTable 有兩個新的資料行


請花一點時間更新**選取**中的子句**GetProductsByCategoryID (*categoryID*)**以及方法。

如果您更新**GetProducts()** **選取**使用**聯結**語法 DataSet 設計工具將無法自動產生插入、 更新和刪除的方法使用 DB 直接模式的資料庫資料。 相反地，您必須手動建立它們更像我們與**InsertProduct**稍早在本教學課程中的方法。 此外，您以手動方式必須提供**InsertCommand**， **UpdateCommand**，和**DeleteCommand**屬性值，如果您想要使用批次更新模式。

## <a name="adding-the-remaining-tableadapters"></a>加入其餘的 TableAdapters

至今的情況下，我們只探討了使用單一資料庫資料表的單一 TableAdapter。 不過，Northwind 資料庫包含數個相關的資料表，我們需要使用我們的 web 應用程式中。 型別資料集可以包含多個相關的 Datatable。 因此，若要完成我們 DAL 我們需要將 Datatable 加入我們將使用這些教學課程中的其他資料表。 新的 TableAdapter 加入型別資料集，請開啟 DataSet 設計工具、 在設計師中，以滑鼠右鍵按一下並選擇 加入 / TableAdapter。 這會建立新的 DataTable 和 TableAdapter，並逐步引導您完成精靈我們稍早在本教學課程中檢查。

需要幾分鐘才能建立 TableAdapters，下列方法使用下列查詢。 請注意，在查詢**ProductsTableAdapter**包含子查詢，擷取每個產品類別目錄] 和 [供應商的名稱。 此外，如果您已經一路，您已新增**ProductsTableAdapter**類別的**GetProducts()**和**GetProductsByCategoryID (*categoryID*)**方法。

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


[![DataSet 設計工具之後已加入四個 TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**圖 31**: 資料集設計工具後四個 Tableadapter 已加入 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Dal 將自訂程式碼

Datatable 加入輸入資料集與 Tableadapter 會表示為 XML 結構描述定義檔案 (**Northwind.xsd**)。 您可以檢視這個結構描述資訊，以滑鼠右鍵按一下**Northwind.xsd**檔案在 方案總管 中，然後選擇 檢視程式碼。


[![XML 結構描述定義 (XSD) 檔案，如 Northwinds 具類型資料集](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**圖 32**: Northwinds 型別資料集的 XML 結構描述定義 (XSD) 檔案 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image88.png))


這個結構描述資訊會在執行階段帳戶轉譯為 C# 或 Visual Basic 程式碼編譯時，或在執行階段 （如有需要），此時您可以逐步執行它偵錯工具。 若要檢視這個自動產生程式碼移至類別檢視] 和 [向下切入 TableAdapter 或類型 DataSet 類別。 如果您沒有看到您螢幕上的 類別檢視，請移至 檢視 功能表並選取從該處，或按 Ctrl + Shift + C。 從 類別檢視中，您可以查看屬性、 方法和事件的型別資料集和 TableAdapter 類別。 若要檢視特定方法的程式碼，連按兩下 類別檢視 中的方法名稱或以滑鼠右鍵按一下並選擇 移至定義。


![檢查自動產生的程式碼，從 類別檢視中選擇 移至定義](creating-a-data-access-layer-cs/_static/image89.png)

**圖 33**： 檢查自動產生的程式碼，從 類別檢視中選擇 移至定義


雖然自動產生的程式碼可以大量時間，而該程式碼通常是相當廣泛，並且需要進行自訂以符合應用程式的獨特需求。 擴充自動產生程式碼的風險是，產生的程式碼的工具可能會決定它是 「 重新產生 」，並覆寫您的自訂設定的時間。 使用.NET 2.0 的新的部分類別概念，所以可以輕鬆地將類別分割到多個檔案。 這可讓我們來自動產生的類別加入自己的方法、 屬性和事件，而不需要擔心覆寫我們自訂 Visual Studio。

若要示範如何自訂 DAL，讓我們加入**GetProducts()**方法**SuppliersRow**類別。 **SuppliersRow**類別表示中的單一記錄**供應商**資料表; 許多產品，每個供應商可以提供者零讓**GetProducts()**傳回那些指定的供應商的產品。 若要完成這會建立新的類別檔案中**應用程式\_程式碼**資料夾名為**SuppliersRow.cs**並加入下列程式碼：

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

此部分類別指示編譯器，當建置**Northwind.SuppliersRow**類別，以包含**GetProducts()**我們剛才定義的方法。 如果您建置專案，然後返回 類別檢視您會看到**GetProducts()**現在列示的方法為**Northwind.SuppliersRow**。


![GetProducts() 方法屬於現在 Northwind.SuppliersRow 類別](creating-a-data-access-layer-cs/_static/image90.png)

**圖 34**: **GetProducts()**方法是現在的一部分**Northwind.SuppliersRow**類別


**GetProducts()**方法現在可用來列舉組產品特定供應商，如下列程式碼所示：

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

此資料也可以顯示在任何 ASP。Web 控制項的網路的資料。 下列網頁使用 GridView 控制項包含兩個欄位：

- 顯示每個供應商名稱 BoundField 和
- 包含設定 BulletedList 的控制項繫結至所傳回的結果為 TemplateField **GetProducts()**每個供應商的方法。

我們將檢驗如何顯示在未來的教學課程的這類的主版詳細資料報表。 現在，這個範例為了說明如何使用自訂的方法加入至**Northwind.SuppliersRow**類別。

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![供應商的公司名稱會列在左邊資料行，其方產品](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**圖 35**: 供應商的公司名稱會列在左邊資料行，其產品方 ([按一下以檢視完整大小的影像](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>總結

當建置 web 應用程式建立 DAL 應該是您第一個步驟，您之前在您開始建立展示層。 使用 Visual Studio，建立具類型資料集基礎 DAL 是可以在 10-15 分鐘內完成而不需要撰寫一行程式碼的工作。 繼續進行後續步驟的教學課程會建置時這個 DAL。 在[下一個教學課程](creating-a-business-logic-layer-cs.md)我們會定義數個商務規則，請參閱 < 如何在不同的商務邏輯層中實作它們。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [建置 DAL，使用強型別 Tableadapter 和 VS 2005 和 ASP.NET 2.0 中的 Datatable](https://weblogs.asp.net/scottgu/435498)
- [設計資料層元件和傳遞的資料層](https://msdn.microsoft.com/library/ms978496.aspx)
- [建置 Visual Studio 2005 DataSet 設計工具與資料存取層](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [加密組態資訊，在 ASP.NET 2.0 應用程式](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter 概觀](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [使用具類型資料集](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [使用 Visual Studio 2005 和 ASP.NET 2.0 中的強型別資料存取](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [如何擴充 TableAdapter 方法](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [從預存程序擷取純量的資料](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程所包含的主題訓練影片

- [ASP.NET 應用程式中的資料存取層](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [如何以手動方式將資料集繫結到資料格](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [如何從 ASP 應用程式與資料集和篩選的工作](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 會導致此教學課程中的檢閱者已 Ron 綠色、 Hilton Giesenow、 Dennis Patterson、 Liz Shulok、 Abel Gomez 和 Carlos Santos。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[下一步](creating-a-business-logic-layer-cs.md)
