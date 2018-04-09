---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: 建立資料存取層 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 671d1bbf661dfb3e56c6ccd67ce0d383990918d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-the-data-access-layer"></a>建立資料存取層
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程說明如何建立、 存取，以及檢閱使用 ASP.NET Web Form 和 Entity Framework Code First，從資料庫的資料。 本教學課程上一個教學課程 < 建立專案 > 為基礎，而且是 Wingtip 玩具存放區的教學課程系列的一部分。 當您已經完成本教學課程時，您會建置中的資料存取類別的群組*模型*專案的資料夾。

## <a name="what-youll-learn"></a>您將學習：

- 如何建立資料模型。
- 如何初始化和植入資料庫。
- 如何更新和應用程式設定為支援的資料庫。

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>這些是教學課程中所引進的功能：

- Entity Framework Code First
- LocalDB
- 資料註釋

## <a name="creating-the-data-models"></a>建立資料模型

[Entity Framework](https://msdn.microsoft.com/data/aa937723)是物件關聯式對應 (ORM) 架構。 它可讓您使用關聯式資料，以排除大部分資料存取程式碼，您通常需要撰寫的物件。 使用 Entity Framework，您可以發出查詢使用[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)，然後擷取和操作資料當做強型別物件。 LINQ 提供查詢及更新的資料的模式。 使用 Entity Framework 可讓您專注於建立您的應用程式的其餘部分，而不是將焦點放在資料存取基礎。 稍後在本教學課程的系列，我們將示範如何使用資料來填入瀏覽和產品的查詢。

Entity Framework 支援呼叫開發架構*Code First*。 程式碼第一次可讓您定義資料模型使用類別。 類別是一種建構，可讓您建立您自己的自訂類型，分組在一起的其他型別、 方法和事件變數。 您可以將類別對應至現有的資料庫，或使用它們來產生資料庫。 在本教學課程中，您將撰寫資料模型類別來建立資料模型。 然後，您會讓 Entity Framework 從這些新的類別上建立資料庫。

您會藉由建立實體類別，定義資料模型來進行 Web Forms 應用程式開始。 然後，您將建立管理的實體類別，並提供資料存取資料庫的內容類別。 您也將建立來擴展資料庫，您將使用的初始設定式類別。

### <a name="entity-framework-and-references"></a>實體架構和參考

根據預設，Entity Framework 時，會包含您建立新**ASP.NET Web 應用程式**使用**Web Form**範本。 Entity Framework 可以安裝、 解除安裝，而且以 NuGet 套件更新。

此 NuGet 封裝包含下列**執行階段**您的專案中的組件：

- EntityFramework.dll – 所有通用執行階段程式碼使用 Entity Framework
- EntityFramework.SqlServer.dll – Entity Framework 的 Microsoft SQL Server 提供者

### <a name="entity-classes"></a>實體類別

您所建立用以定義資料的結構描述的類別稱為 「 實體類別 」。 如果您還不熟悉資料庫設計，將實體類別視為資料表定義的資料庫。 類別中的每個屬性會指定資料庫的資料表中資料行。 這些類別提供輕量型、 關聯式物件之間的介面物件導向的程式碼和資料庫的關聯式資料表結構。

在本教學課程中，您將開始加入簡單的實體類別代表產品和分類的結構描述。 產品類別將包含每個產品定義。 每個產品類別的成員名稱會是`ProductID`， `ProductName`， `Description`， `ImagePath`， `UnitPrice`， `CategoryID`，和`Category`。 目錄類別將包含產品可以屬於，例如汽車、 船或平面的每個分類的定義。 每個類別目錄類別的成員名稱會是`CategoryID`， `CategoryName`， `Description`，和`Products`。 每個產品將屬於其中一個類別。 這些實體類別就會加入至專案的現有*模型*資料夾。

1. 在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。 

    ![建立資料存取層中的新項目功能表](create_the_data_access_layer/_static/image1.png)

   隨即顯示 [ 新增項目] 對話方塊。
2. 在下**Visual C#**從**已安裝**左邊的窗格，選取**程式碼**。 

    ![建立資料存取層中的新項目功能表](create_the_data_access_layer/_static/image2.png)
3. 選取**類別**從中間窗格，並命名這個新類別*Product.cs*。
4. 按一下 [加入] 。  
   在編輯器中，會顯示新的類別檔案。
5. 下列程式碼取代預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. 重複步驟 1 到 4，不過，命名為新的類別，以建立另一個類別*Category.cs* ，並以下列程式碼取代預設程式碼：  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

如先前所述，`Category`類別類型的應用程式的產品設計銷售代表 (例如<a id="a"> </a>&quot;汽車&quot;，&quot;船&quot;， &quot;火箭&quot;等等)，而`Product`類別代表在資料庫中的個別產品 (toys)。 每個執行個體`Product`物件將會對應到關聯式資料庫資料表中的資料列和每個產品類別的屬性會對應到關聯式資料庫資料表中的資料行。 稍後在本教學課程中，您將檢閱包含在資料庫中的產品資料。

### <a name="data-annotations"></a>資料註釋

您可能已注意到某些類別的成員具有屬性指定成員相關的詳細資料，例如`[ScaffoldColumn(false)]`。 這些是*資料註解*。 資料附註屬性可描述如何驗證使用者輸入，該成員，以指定格式，並指定如何它模型化資料庫建立時。

### <a name="context-class"></a>Context 類別

若要開始使用資料存取的類別，您必須定義的內容類別。 如先前所述，內容類別管理的實體類別 (例如`Product`類別和`Category`類別)，並提供對資料庫的資料存取。

此程序會加入新 C# 內容類別來*模型*資料夾。

1. 以滑鼠右鍵按一下*模型*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取**類別**從中間窗格中，其命名*ProductContext.cs*按一下**新增**。
3. 取代為下列程式碼類別中包含的預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

這個程式碼加入`System.Data.Entity`命名空間，讓您可以存取所有核心功能的 Entity Framework 中，其中包含查詢的能力，插入、 更新和刪除資料使用強型別的物件。

`ProductContext`類別代表 Entity Framework 產品的資料庫內容，用來處理擷取、 儲存及更新`Product`類別執行個體在資料庫中的。 `ProductContext`類別衍生自`DbContext`基底 Entity Framework 所提供的類別。

### <a name="initializer-class"></a>初始設定式類別

您必須執行一些自訂邏輯，來初始化資料庫第一個時間使用的內容。 這可讓要加入至資料庫，以便您可以立即顯示產品和分類的種子資料。

此程序會加入新 C# 初始設定式的類別來*模型*資料夾。

1. 建立另一個`Class`中*模型*資料夾並將其命名*ProductDatabaseInitializer.cs*。
2. 取代為下列程式碼類別中包含的預設程式碼：   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

建立和初始化，資料庫時，您可以看到上述程式碼，`Seed`屬性覆寫，而且設定。 當`Seed`屬性設定、 分類和產品中的值會用來填入資料庫。 如果您嘗試更新的種子資料，藉由修改上述的程式碼，在建立資料庫之後，將不會在執行 Web 應用程式時看到任何更新。 原因是上述程式碼使用的實作`DropCreateDatabaseIfModelChanges`辨識重設種子資料之前是否已變更模型 （結構描述） 的類別。 如果沒有變更`Category`和`Product`實體類別，資料庫將不會重新初始化的種子資料。

> [!NOTE] 
> 
> 如果您想要的資料庫重新建立每次執行該應用程式，您可以使用`DropCreateDatabaseAlways`類別而不是`DropCreateDatabaseIfModelChanges`類別。 不過對於此教學課程的數列，請使用`DropCreateDatabaseIfModelChanges`類別。


此時在本教學課程中，您將需要*模型*資料夾具有四個新的類別和一個預設類別：

![建立資料存取層-Models 資料夾](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>設定應用程式使用的資料模型

既然您已建立的類別代表的資料，您必須設定應用程式使用的類別。 在*Global.asax*檔案中，您將初始化模型的程式碼。 在*Web.config*檔案，在您加入的資訊，告訴您資料庫功能的應用程式用來儲存新的資料類別所代表的資料。 *Global.asax*檔案可以用來處理應用程式事件或方法。 *Web.config*檔可讓您控制 ASP.NET web 應用程式的設定。

#### <a name="updating-the-globalasax-file"></a>正在更新 Global.asax 檔案

若要初始化的資料模型，應用程式啟動時，您將更新`Application_Start`中的處理常式*Global.asax.cs*檔案。

> [!NOTE] 
> 
> 在 [方案總管] 中，您可以選取*Global.asax*檔案或*Global.asax.cs*檔案進行編輯*Global.asax.cs*檔案。


1. 加入下列程式碼中以黃色反白顯示`Application_Start`方法中的*Global.asax.cs*檔案。   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> 您的瀏覽器必須支援 HTML5 檢視瀏覽器中檢視此教學課程系列時，以黃色醒目提示的程式碼。


如上述程式碼所示，應用程式啟動時，應用程式會指定存取資料期間第一次將執行初始設定式。 兩個額外的命名空間所需存取`Database`物件和`ProductDatabaseInitializer`物件。

 修改 Web.Config 檔案 

Entity Framework Code First 會產生資料庫針對您在預設位置時資料庫已填入種子資料，雖然您自己的連接資訊加入至應用程式會提供您的資料庫位置的控制項。 您指定此應用程式中使用連接字串的資料庫連接*Web.config*專案根目錄的檔案。 您可以藉由加入新的連接字串，指示資料庫的位置 (*wingtiptoys.mdf*) 建置應用程式的資料目錄 (*應用程式\_資料*)，而不是預設值位置。 進行這項變更可讓您找出並檢查資料庫檔案，稍後在本教學課程。

1. 在**方案總管 中**、 尋找和開啟*Web.config*檔案。
2. 加入連接字串中以黃色反白顯示`<connectionStrings>`區段*Web.config*檔案，如下所示：  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

第一次執行應用程式時，它會建立資料庫連接字串所指定的位置。 但是之前執行的應用程式，讓我們來建立該第一次。

## <a name="building-the-application"></a>建置應用程式

若要確定所有類別和變更您的 Web 應用程式都運作，您應該建置應用程式。

1. 從**偵錯**功能表上，選取**建置 WingtipToys**。  
 **輸出**視窗隨即顯示，且如果一切順利，您會看到*成功*訊息。  

    ![建立資料存取層-輸出視窗](create_the_data_access_layer/_static/image4.png)

如果您遇到錯誤時，請重新檢查前面的步驟。 中的資訊**輸出**視窗會指出哪些檔案有問題，而檔案中變更需要的地方。 這項資訊可讓您決定上述步驟的哪個部分要檢閱並修正您的專案中。

## <a name="summary"></a>總結

在本教學課程系列的您有建立資料模型，以及，新增將用來初始化及植入資料庫的程式碼。 您也已經設定應用程式執行應用程式時使用的資料模型。

在下一個教學課程中，您將會更新 UI、 加入導覽，和從資料庫擷取資料。 這會導致資料庫正在自動根據您在本教學課程中建立的實體類別。

## <a name="additional-resources"></a>其他資源

[Entity Framework 概觀](https://msdn.microsoft.com/library/bb399567.aspx)   
[ADO.NET Entity Framework 初學者指南](https://msdn.microsoft.com/data/ee712907)   
[Code First 開發有 Entity Framework](http://www.msteched.com/2010/Europe/DEV212) （影片）   
[程式碼第一個關聯性 Fluent 應用程式開發介面](https://msdn.microsoft.com/data/hh134698)   
[第一個資料註解的程式碼](https://msdn.microsoft.com/data/gg193958)  
[Entity Framework 的產能改善功能](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [上一頁](create-the-project.md)
> [下一頁](ui_and_navigation.md)
