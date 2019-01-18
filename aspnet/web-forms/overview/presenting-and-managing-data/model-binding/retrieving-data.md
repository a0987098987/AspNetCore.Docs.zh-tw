---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 擷取和顯示資料與模型繫結和 web form |Microsoft Docs
author: Rick-Anderson
description: 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行資料互動更多簡單-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396281"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>擷取和顯示資料與模型繫結和 web form
====================

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
>  模型繫結模式適用於任何資料存取技術。 在本教學課程中，您將使用 Entity Framework，但您可以使用最熟悉的資料存取技術。 從資料繫結的伺服器控制項，例如 GridView、 ListView、 DetailsView 或 FormView 控制項，您可以指定要用於選取、 更新、 刪除和建立資料方法的名稱。 在本教學課程中，您會指定 SelectMethod 值。 
> 
> 在該方法中，您提供的邏輯擷取資料。 在下一個教學課程中，您將 UpdateMethod、 DeleteMethod 和 InsertMethod 設定值。
>
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，在C#或 Visual Basic。 可下載的程式碼適用於使用 Visual Studio 2012 和更新版本。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2017 範本稍有不同。
> 
> 在本教學課程中，您可以執行應用程式在 Visual Studio 中。 您也可以部署到裝載提供者應用程式，並使其可透過網際網路使用。 Microsoft 提供免費的 web 裝載中的最多 10 個 web sites  
>  [免費 Azure 試用帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。 如需有關如何將 Visual Studio web 專案部署至 Azure App Service Web Apps 的資訊，請參閱[使用 Visual Studio 的 ASP.NET Web 部署](../../deployment/visual-studio-web-deployment/introduction.md)系列。 該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Azure SQL Database。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> - Microsoft Visual Studio 2017 或 Microsoft Visual Studio Community 2017
>   
> 本教學課程也適用於 Visual Studio 2012 和 Visual Studio 2013，但有一些差異，在使用者介面和專案範本。


## <a name="what-youll-build"></a>您將建置

在本教學課程中，您將會：

* 建立與註冊課程的學生反映一所大學的資料物件
* 建置從物件的資料庫資料表
* 填入測試資料的資料庫
* 在 web 表單中顯示資料

## <a name="create-the-project"></a>建立專案

1. 在 Visual Studio 2017 中，建立**ASP.NET Web 應用程式 (.NET Framework)** 專案，稱為**ContosoUniversityModelBinding**。

   ![建立專案](retrieving-data/_static/image19.png)

2. 選取 [確定]。 若要選取範本的對話方塊隨即出現。

   ![選取 web form](retrieving-data/_static/image3.png)

3. 選取  **Web Form**範本。 

4. 如有必要，驗證變更為**個別使用者帳戶**。 

5. 選取 [確定] 建立專案。

## <a name="modify-site-appearance"></a>修改網站的外觀

   變更幾個自訂網站的外觀。 
   
   1. 開啟 Site.Master 檔。
   
   2. 變更要顯示的標題**Contoso 大學**而非**My ASP.NET Application**。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. 變更中的標頭文字**應用程式名稱**要**Contoso 大學**。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. 變更導覽標頭連結至適當的站台。 
   
      移除的連結**關於**並**連絡人**，相反地，連結到**學生**頁面上，您將建立。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. 儲存 Site.Master。

## <a name="add-a-web-form-to-display-student-data"></a>新增 web 表單，以顯示學生資料

   1. 在 [**方案總管] 中**，以滑鼠右鍵按一下您的專案，然後選取**新增**，然後**新項目**。 
   
   2. 在 **加入新項目**對話方塊中，選取**使用主版頁面的 Web Form**範本並將它命名**Students.aspx**。

      ![建立頁面](retrieving-data/_static/image5.png)

   3. 選取 [新增]。
   
   4. Web form 主版頁面，選取**Site.Master**。
   
   5. 選取 [確定]。
   

## <a name="add-the-data-model"></a>加入資料模型

在 **模型**資料夾中，加入名為類別**UniversityModels.cs**。

   1. 以滑鼠右鍵按一下**模型**，選取**新增**，然後**新項目**。 [新增項目] 對話方塊隨即出現。

   2. 從左側的導覽功能表中，選取**程式碼**，然後**類別**。

      ![建立模型類別](retrieving-data/_static/image20.png)

   3. 將類別命名為**UniversityModels.cs** ，然後選取**新增**。

      這個檔案中定義`SchoolContext`， `Student`， `Enrollment`，和`Course`類別，如下所示：

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      `SchoolContext`類別衍生自`DbContext`，其管理資料庫連接，並變更資料中。

      在 `Student`類別，您會發現要套用的屬性`FirstName`， `LastName`，和`Year`屬性。 本教學課程會使用這些屬性進行資料驗證。 若要簡化程式碼，只有這些屬性會標示具有資料驗證屬性。 在實際的專案中，您會將驗證屬性套用至需要驗證的所有屬性。

   4. 儲存 UniversityModels.cs。

## <a name="set-up-the-database-based-on-classes"></a>將類別為基礎的資料庫設定

本教學課程會使用[Code First 移轉](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/)建立物件和資料庫資料表。 這些資料表會儲存資訊的學生和他們的課程。

   1. 選取 **工具** > **NuGet 套件管理員** > **Package Manager Console**。

   2. 在  **Package Manager Console**，執行下列命令：  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      如果命令成功完成時，會出現一則訊息指出已啟用移轉。

      ![啟用移轉](retrieving-data/_static/image8.png)

      請注意，名為的檔案*Configuration.cs*已建立。 `Configuration`類別具有`Seed`方法，它可以預先填入測試資料的資料庫資料表。

## <a name="pre-populate-the-database"></a>在資料庫中預先填入

   1. 開啟 Configuration.cs。
   
   2. 將下列程式碼加入至 `Seed` 方法。 此外，新增`using`陳述式`ContosoUniversityModelBinding. Models`命名空間。

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. 儲存 Configuration.cs。

   4. 在 [套件管理員] 主控台中，執行命令**新增移轉初始**。

   5. 執行命令**更新資料庫**。

      如果執行此命令中時，您會收到例外狀況`StudentID`並`CourseID`值可能不同於`Seed`方法值。 開啟這些資料庫資料表，並尋找現有的值，如`StudentID`和`CourseID`。 將這些值加入的程式碼植入`Enrollments`資料表。

## <a name="add-a-gridview-control"></a>新增 GridView 控制項

填入的資料庫的資料，您現在準備好擷取該資料並加以顯示。 

1. 開啟 Students.aspx。

2. 找出`MainContent`預留位置。 在該預留位置中，新增**GridView**包括此程式碼的控制項。

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   要注意的事項：
   * 請注意，設定的值`SelectMethod`GridView 項目中的屬性。 這個值會指定用來擷取您在下一個步驟中建立的 GridView 資料的方法。 
   
   * `ItemType`屬性設定為`Student`稍早建立的類別。 此設定可讓您參考的標記中的類別屬性。 例如，`Student`類別具有名為`Enrollments`。 您可以使用`Item.Enrollments`來擷取該集合，然後使用[LINQ 語法](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq)擷取每個學生的註冊點數總和。
   
3. 儲存 Students.aspx。

## <a name="add-code-to-retrieve-data"></a>加入程式碼來擷取資料

   在 Students.aspx 程式碼後置檔案中，新增為指定的方法`SelectMethod`值。 
   
   1. 開啟 Students.aspx.cs。
   
   2. 新增`using`陳述式`ContosoUniversityModelBinding. Models`和`System.Data.Entity`命名空間。

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. 新增您指定的方法`SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      `Include`子句可改善查詢效能，但並非必要。 不含`Include`子句，資料會使用擷取[*消極式載入*](https://en.wikipedia.org/wiki/Lazy_loading)，其中包含每次傳送至資料庫的個別查詢相關的擷取資料。 具有`Include`子句，資料會使用擷取*積極式載入*，這表示單一資料庫查詢會擷取所有相關的資料。 如果不使用相關的資料，積極式載入是比較沒有效率因為擷取詳細資料。 不過，在此情況下，積極式載入可讓您獲得最佳效能因為相關的資料會顯示每一筆記錄。

      如需載入時的效能考量的詳細資訊的相關資料，請參閱 < **「 延遲 」、 「 Eager 和 「 明確載入相關資料的**一節中[讀取相關資料，與 ASP.NET 中的 Entity FrameworkMVC 應用程式](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)文章。

      根據預設，資料會依照標示為索引鍵屬性的值而定。 您可以新增`OrderBy`子句來指定不同的排序值。 在此範例中，預設值`StudentID`屬性用來排序。 在 [排序、 分頁和篩選資料](sorting-paging-and-filtering-data.md)文章中，使用者可選取的資料行排序。
 
   4. 儲存 Students.aspx.cs。

## <a name="run-your-application"></a>執行您的應用程式 

執行您的 web 應用程式 (**F5**) 並瀏覽至**學生**網頁，當中會顯示下列：

   ![顯示資料](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>自動產生的模型繫結方法

在逐步進行本教學課程系列，只要到您的專案複製程式碼從本教學課程。 不過，這種方法的缺點是功能的，您可能不會察覺到自動產生的模型繫結方法的程式碼的 Visual Studio 所提供。 您自己的專案上工作時，自動產生程式碼可以節省時間，並且有助於您深入了解如何實作的作業。 本章節描述的自動程式碼產生功能。 本章節僅供參考，並不包含任何您需要在您的專案中實作的程式碼。 

設定的值時`SelectMethod`， `UpdateMethod`， `InsertMethod`，或`DeleteMethod`屬性的標記程式碼中，您可以選取**建立的新方法**選項。

![建立方法](retrieving-data/_static/image18.png)

Visual Studio 不僅會建立具有適當簽章中，程式碼後置中的方法，也會產生執行作業的實作程式碼。 如果您先設定`ItemType`屬性，才能使用自動程式碼產生功能，為作業輸入產生的程式碼使用。 例如，當設定`UpdateMethod`屬性，下列程式碼會自動產生：

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

同樣地，此程式碼不需要加入至專案。 在下一個教學課程中，您將實作更新、 刪除和加入新資料的方法。

## <a name="summary"></a>總結

在本教學課程中，您可以建立資料模型類別，並從那些類別產生資料庫。 您會將資料庫資料表中填入測試資料。 您用來從資料庫擷取資料的模型繫結，然後 GridView 中顯示資料。

在接下來[教學課程](updating-deleting-and-creating-data.md)在本系列中，您將會讓更新、 刪除和建立資料。

> [!div class="step-by-step"]
> [下一步](updating-deleting-and-creating-data.md)
