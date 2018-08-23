---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: 建立資料存取層 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: e6ec385c6a4a5507ffae726157f7d52e9c5605da
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833889"
---
<a name="create-the-data-access-layer"></a>建立資料存取層
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


本教學課程說明如何建立、 存取和檢閱使用 ASP.NET Web Form 和 Entity Framework Code First 資料庫中的資料。 此教學課程的上一個教學課程 [建立專案]，並且是 Wingtip 玩具店教學課程系列的一部分。 當您完成本教學課程中時，您就會建置中的資料存取類別的一群*模型*專案的資料夾。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何建立資料模型。
- 如何初始化和植入資料庫。
- 如何更新及設定應用程式，以支援的資料庫。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>在本教學課程中所引進的功能如下：

- Entity Framework Code First
- LocalDB
- 資料註釋

## <a name="creating-the-data-models"></a>建立資料模型

[Entity Framework](https://msdn.microsoft.com/data/aa937723)是一種物件關聯式對應 (ORM) 架構。 它可讓您使用關聯式資料，以消除資料存取程式碼，您通常需要撰寫的大部分的物件。 使用 Entity Framework，您可以發出查詢使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)，然後擷取和操作資料當做強型別物件。 LINQ 提供資料查詢與更新的模式。 使用 Entity Framework，可讓您專注於建立您的應用程式的其餘部分，而不是將焦點放在資料存取的基本概念。 稍後在本教學課程系列，我們會顯示您如何使用資料來填入導覽和產品的查詢。

Entity Framework 支援呼叫開發架構*Code First*。 程式碼第一次可讓您定義資料模型使用的類別。 類別是一種建構，可讓您建立您自己的自訂類型群組在一起的其他型別、 方法和事件變數。 您可以將類別對應至現有的資料庫，或使用它們來產生資料庫。 在本教學課程中，您會撰寫資料模型類別來建立資料模型。 然後，您會讓這些新的類別從作業中建立資料庫的 Entity Framework。

您會開始藉由建立定義資料模型來進行 Web Forms 應用程式的實體類別。 然後，您將建立管理的實體類別，並提供對資料庫的資料存取的內容類別。 您也會建立您將用來填入資料庫的初始設定式類別。

### <a name="entity-framework-and-references"></a>實體架構和參考

根據預設，Entity Framework 時，會包含您建立新**ASP.NET Web 應用程式**使用**Web Form**範本。 Entity Framework 可安裝、 解除安裝，並更新為 NuGet 套件。

此 NuGet 套件包含下列**執行階段**專案內的組件：

- EntityFramework.dll – 所有通用執行階段程式碼使用 Entity Framework
- EntityFramework.SqlServer.dll – Entity Framework 的 Microsoft SQL Server 提供者

### <a name="entity-classes"></a>實體類別

您所建立用以定義資料的結構描述的類別則稱為實體類別。 如果您還不熟悉資料庫設計，將實體類別視為資料庫的資料表定義。 類別中的每個屬性會指定資料庫的資料表中的資料行。 這些類別會提供輕量、 物件關聯的介面之間物件導向的程式碼和資料庫的關聯式資料表結構。

在本教學課程中，您一開始加入簡單的實體類別代表對於產品和分類的結構描述。 產品類別將包含每項產品的定義。 每個產品類別的成員的名稱會是`ProductID`， `ProductName`， `Description`， `ImagePath`， `UnitPrice`， `CategoryID`，和`Category`。 類別目錄類別會包含每個類別目錄的產品可以屬於，例如車輛、 船隻或平面的定義。 每個類別目錄類別的成員的名稱會是`CategoryID`， `CategoryName`， `Description`，和`Products`。 每個產品將屬於其中一個類別。 這些實體類別將會新增至專案之現有*模型*資料夾。

1. 在 **方案總管**，以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。 

    ![建立資料存取層-新的項目功能表](create_the_data_access_layer/_static/image1.png)

   隨即顯示 [ 新增項目] 對話方塊。
2. 底下**Visual C#** 從**已安裝**左邊的窗格，選取**程式碼**。 

    ![建立資料存取層-新的項目功能表](create_the_data_access_layer/_static/image2.png)
3. 選取 **類別**從中間窗格中，這個新類別命名*Product.cs*。
4. 按一下 [加入] 。  
   新的類別檔案會顯示在編輯器中。
5. 取代為下列程式碼中的預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. 建立另一個類別，重複步驟 1 到 4，不過，新類別命名*Category.cs*並以下列程式碼取代預設的程式碼：  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

如先前所述，`Category`類別的代表銷售中設計的應用程式的產品類型 (例如<a id="a"> </a>&quot;汽車&quot;，&quot;潮水&quot;， &quot;火箭&quot;等等)，而`Product`類別代表在資料庫中的個別產品 (toys)。 每個執行個體`Product`物件將會對應到關聯式資料庫資料表中的資料列和 Product 類別的每個屬性會對應到關聯式資料庫資料表中的資料行。 稍後在本教學課程中，您將檢閱包含在資料庫中的產品資料。

### <a name="data-annotations"></a>資料註釋

您可能已經注意到特定成員的類別有屬性指定成員相關的詳細資料，例如`[ScaffoldColumn(false)]`。 這些是*資料註解*。 資料註解屬性可描述如何驗證使用者輸入，該成員，以指定格式，並指定它在建立資料庫時的模型如何。

### <a name="context-class"></a>Context 類別

若要開始使用資料存取的類別，您必須定義的內容類別。 如先前所述，此內容類別管理的實體類別 (例如`Product`類別和`Category`類別)，並提供對資料庫的資料存取。

此程序會加入新 C# 內容類別來*模型*資料夾。

1. 以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取 **類別**從中間窗格中，其命名*ProductContext.cs*然後按一下**新增**。
3. 取代為下列程式碼類別中包含的預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

這個程式碼加入`System.Data.Entity`命名空間，讓您能夠存取所有的核心功能的 Entity Framework，包括查詢的功能、 插入、 更新和刪除資料，藉由使用強型別物件。

`ProductContext`類別代表 Entity Framework 產品的資料庫內容，用來處理擷取、 儲存及更新`Product`類別執行個體在資料庫中的。 `ProductContext`類別衍生自`DbContext`基底 Entity Framework 所提供的類別。

### <a name="initializer-class"></a>初始設定式類別

您必須執行一些自訂邏輯來初始化資料庫第一次內容時。 這可讓資料植入加入至資料庫，以便您可以立即顯示產品和分類。

此程序會加入新 C# 初始設定式類別來*模型*資料夾。

1. 建立另一個`Class`中*模型*資料夾並將它命名*ProductDatabaseInitializer.cs*。
2. 取代為下列程式碼類別中包含的預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

建立和初始化資料庫時，您可以看到上述的程式碼，`Seed`屬性覆寫，且設定。 當`Seed`屬性設定，用來填入資料庫中的類別和產品的值。 如果您嘗試更新種子資料，藉由修改上述的程式碼，在建立資料庫之後，您就不會看到任何更新，當您執行 Web 應用程式。 上述程式碼中使用的實作時，是因為`DropCreateDatabaseIfModelChanges`辨識重設種子資料之前是否已變更的模型 （結構描述） 的類別。 如果沒有變更`Category`和`Product`實體類別，資料庫將不會重新初始化的種子資料。

> [!NOTE] 
> 
> 如果您想要的資料庫重新建立每次執行應用程式，您可以使用`DropCreateDatabaseAlways`類別而不是`DropCreateDatabaseIfModelChanges`類別。 不過本教學課程系列中，使用`DropCreateDatabaseIfModelChanges`類別。


此時在本教學課程中，您必須*模型*資料夾中，使用四個新的類別和一個預設類別：

![建立資料存取層-Models 資料夾](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>設定應用程式使用的資料模型

既然您已建立的類別，代表的資料，您必須設定應用程式使用的類別。 在  *Global.asax*檔案，您將會初始化模型的程式碼。 在  *Web.config*您新增資訊，告訴您資料庫功能的應用程式用來儲存新的資料類別所表示的資料檔案。 *Global.asax*檔案可以用來處理應用程式事件或方法。 *Web.config*檔可讓您控制 ASP.NET web 應用程式的組態。

#### <a name="updating-the-globalasax-file"></a>正在更新 Global.asax 檔案

若要初始化的資料模型，應用程式啟動時，您將會更新`Application_Start`中的處理常式*Global.asax.cs*檔案。

> [!NOTE] 
> 
> 在 [方案總管] 中，您可以選取*Global.asax*檔案或*Global.asax.cs*若要編輯的檔案*Global.asax.cs*檔案。


1. 新增下列程式碼中以黃色反白顯示`Application_Start`方法中的*Global.asax.cs*檔案。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> 您的瀏覽器必須支援 HTML5 檢視瀏覽器中檢視此教學課程系列時，以黃色醒目提示的程式碼。


如上述程式碼所示，當應用程式啟動時，應用程式會指定初始設定式執行期間第一次資料存取。 若要存取的兩個額外的命名空間所`Database`物件和`ProductDatabaseInitializer`物件。

 修改 Web.Config 檔案 

雖然 Entity Framework Code First 會資料庫讓您在預設位置時產生種子資料填入資料庫，請將您自己的連線資訊新增至您的應用程式讓您控制資料庫位置。 指定資料庫連接的應用程式中使用的連接字串*Web.config*專案根目錄的檔案。 藉由新增新的連接字串，您可以直接使用資料庫的位置 (*wingtiptoys.mdf*) 來建置應用程式的資料目錄中 (*應用程式\_資料*)，而不是其預設值位置。 進行這項變更可讓您找出並檢查資料庫檔案，稍後在本教學課程。

1. 在 **方案總管**，尋找並開啟*Web.config*檔案。
2. 新增以黃色反白顯示的連接字串`<connectionStrings>`一節*Web.config*檔案，如下所示：  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

第一次執行應用程式時，它會建立資料庫連接字串所指定的位置。 但是之前執行的應用程式，我們將建置它第一次。

## <a name="building-the-application"></a>建置應用程式

若要確定所有類別和變更您的 Web 應用程式都運作，您應該建置應用程式。

1. 從**偵錯**功能表上，選取**建置 WingtipToys**。  
 **輸出** 視窗會顯示，而且如果一切順利，您會看到*成功*訊息。  

    ![建立資料存取層-輸出 Windows](create_the_data_access_layer/_static/image4.png)

如果您遇到錯誤時，重新檢查上述的步驟。 中的資訊**輸出**視窗會指出哪一個檔案有問題，而檔案中變更需要的地方。 這項資訊可讓您判斷上述步驟的哪個部分要檢閱並修正您的專案。

## <a name="summary"></a>總結

在本教學課程系列的您已建立資料模型，以及，新增將用於初始化，並植入資料庫的程式碼。 您也已設定要執行應用程式時使用的資料模型的應用程式。

在下一個教學課程中，您將會更新 UI、 新增導覽中，和從資料庫擷取資料。 這樣會自動建立根據您在本教學課程中建立的實體類別之資料庫中。

## <a name="additional-resources"></a>其他資源

[Entity Framework 概觀](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity framework 的初級開發人員指南](https://msdn.microsoft.com/data/ee712907)   
[程式碼使用 Entity Framework 的第一個開發](http://www.msteched.com/2010/Europe/DEV212)（影片）   
[程式碼的第一個關聯性 Fluent API](https://msdn.microsoft.com/data/hh134698)   
[Code First 資料註解](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework 的產能改良功能](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [上一頁](create-the-project.md)
> [下一頁](ui_and_navigation.md)
