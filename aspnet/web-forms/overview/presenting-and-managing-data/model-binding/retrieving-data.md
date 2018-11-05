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
ms.openlocfilehash: b05f3780d7c4e4734b35c0d9377a89d6f3edb0f8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021335"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>擷取和顯示資料與模型繫結和 web form
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本系列教學課程示範使用 ASP.NET Web Form 專案中的模型繫結的基本層面。 模型繫結進行更簡單的比處理資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此系列簡介資料的開頭，並在稍後的教學課程中將移至更進階的概念。
> 
> 模型繫結模式適用於任何資料存取技術。 在本教學課程中，您將使用 Entity Framework，但您可以使用最熟悉的資料存取技術。 從資料繫結的伺服器控制項，例如 GridView、 ListView、 DetailsView 或 FormView 控制項，您可以指定要用於選取、 更新、 刪除和建立資料方法的名稱。 在本教學課程中，您會指定 SelectMethod 值。
> 
> 在該方法中，您提供的邏輯擷取資料。 在下一個教學課程中，您將 UpdateMethod、 DeleteMethod 和 InsertMethod 設定值。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)完整的專案，以 C# 或 vb。 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，也就是本教學課程中的 Visual Studio 2013 範本稍有不同。
> 
> 在本教學課程中，您可以執行應用程式在 Visual Studio 中。 您也可以提供應用程式透過網際網路將它部署至主機服務提供者。 Microsoft 提供免費的 web 裝載中的最多 10 個 web sites  
>  [免費 Azure 試用帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。 如需有關如何將 Visual Studio web 專案部署至 Azure App Service Web Apps 的資訊，請參閱[使用 Visual Studio 的 ASP.NET Web 部署](../../deployment/visual-studio-web-deployment/introduction.md)系列。 該教學課程也會示範如何使用 Entity Framework Code First Migrations，將 SQL Server 資料庫部署到 Azure SQL Database。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Microsoft Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web
>   
> 
> 本教學課程也適用於使用 Visual Studio 2012，但會在使用者介面和專案範本中的一些差異。


## <a name="what-youll-build"></a>您將建置

在本教學課程中，您將會：

1. 建立與註冊課程的學生反映一所大學的資料物件
2. 建置從物件的資料庫資料表
3. 填入測試資料的資料庫
4. 在 web 表單中顯示資料

## <a name="set-up-project"></a>設定專案

在 Visual Studio 2013 中，建立新**ASP.NET Web 應用程式**稱為**ContosoUniversityModelBinding**。

![建立專案](retrieving-data/_static/image2.png)

選取 Web Form 範本，並保留其他預設選項。 按一下 [確定] 以安裝專案。

![選取 web form](retrieving-data/_static/image3.png)

首先，您將幾個小變更，以自訂網站的外觀。 開啟**Site.Master**檔案，並將標題變更為包含 Contoso 大學，而不是我的 ASP.NET 應用程式。

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

然後，將變更從標頭文字**應用程式名稱**要**Contoso 大學**。

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

也在 Site.Master，變更會出現在標頭可反映與此站台相關的頁面導覽連結。 您不需要其中一個**有關**頁面或**連絡人**頁面上，因此可以移除那些連結。 相反地，您必須呼叫頁面的連結**學生**。 尚未建立此頁面。

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

儲存並關閉 Site.Master。

現在，您將建立顯示學生資料的 web 表單。 以滑鼠右鍵按一下您的專案，並**新增****新項目**。 選取 **使用主版頁面的 Web Form**範本，並將它命名**Students.aspx**。

![建立頁面](retrieving-data/_static/image5.png)

選取  **Site.Master**為新的 web form 主版頁面。

## <a name="create-the-data-models-and-database"></a>建立資料模型和資料庫

您將使用 Code First 移轉來建立物件和對應的資料庫資料表。 這些資料表會儲存有關學生和其課程的資訊。

在 [模型] 資料夾中，新增名為的新類別**UniversityModels.cs**。

![建立模型類別](retrieving-data/_static/image7.png)

在此檔案中，定義 SchoolContext、 學生、 註冊和課程類別，如下所示：

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

SchoolContext 類別衍生自 DbContext，管理資料庫連接和資料變更。

在學生類別中，請注意已套用至的屬性**FirstName**， **LastName**，並**年**屬性。 這些屬性會用於本教學課程中的資料驗證。 若要簡化此示範專案的程式碼，只有這些屬性同時標示為資料驗證屬性。 在實際的專案中，您會將驗證屬性套用至需要驗證的資料，例如註冊和課程類別中屬性的所有屬性。

儲存 UniversityModels.cs。

若要設定資料庫，根據這些類別，您將使用 Code First 移轉的工具。

在  **Package Manager Console**，執行命令：  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

如果命令成功完成，您就會收到一則訊息指出已啟用移轉，

![啟用移轉](retrieving-data/_static/image8.png)

請注意，新的檔案命名為**Configuration.cs**已建立。 在 Visual Studio 中，此檔案會自動開啟它建立之後。 設定類別包含**種子**方法可讓您預先填入測試資料的資料庫資料表。

種子方法中加入下列程式碼。 您必須新增**使用**陳述式**ContosoUniversityModelBinding.Models**命名空間。

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

儲存 Configuration.cs。

在 [套件管理員] 主控台中，執行命令`add-migration initial`。

然後，執行命令`update-database`。

如果執行此命令時，您會收到例外狀況，，就可以 StudentID 和 CourseID 的值具有不同的種子方法中的值。 開啟資料庫中的這些資料表和 StudentID 和 CourseID 尋找現有的值。 將這些值加入程式碼植入的註冊項目資料表中。

已加入的資料庫檔案，但它目前隱藏在專案中。 按一下 **顯示所有檔案**顯示的檔案。

![顯示所有檔案](retrieving-data/_static/image10.png)

請注意.mdf 檔案現在會出現在應用程式\_Data 資料夾。

![資料庫檔案](retrieving-data/_static/image12.png)

按兩下.mdf 檔案，然後開啟 [伺服器總管] 中。 資料表現在會存在，並會填入資料。

![資料庫資料表](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>顯示學生和相關的資料表的資料

在資料庫中的資料，您現在已準備好擷取該資料，並顯示在網頁中。 您將使用**GridView**控制項來顯示資料行和資料列中的資料。

開啟 Students.aspx，並找出**MainContent**預留位置。 在該預留位置中，新增**GridView**控制，以包含下列程式碼。

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

有幾個重要的概念，您會注意到這個標記程式碼中。 首先，請注意值會設定為**SelectMethod** GridView 項目中的屬性。 這個值會指定用於擷取資料的 GridView 方法的名稱。 您將在下一個步驟中建立這個方法。 其次，注意**ItemType**屬性設定為您稍早建立的 Student 類別。 藉由設定此值，您可以參考該類別標記程式碼中的屬性。 比方說，Student 類別會包含名為註冊的集合。 您可以使用**Item.Enrollments**來擷取該集合，然後使用 LINQ 語法擷取每個學生的已註冊的點數總和。

在程式碼後置檔案中，您要新增指定的方法**SelectMethod**值。 開啟**Students.aspx.cs**，並新增**使用**陳述式**ContosoUniversityModelBinding.Models**並**System.Data.Entity**命名空間。

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

然後，新增下列方法。 請注意，這個方法的名稱會符合 SelectMethod 您提供的名稱。

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include**子句可以改善此查詢的效能，但不是不可或缺的查詢運作。 不使用 Include 子句中，擷取的資料會使用消極式載入，這牽涉到將傳送到資料庫的個別查詢每次擷取資料的相關。 藉由提供的 Include 子句，會使用積極式載入，這表示透過單一查詢的資料庫擷取的所有相關資料的擷取資料。 在大部分的相關資料則不會的情況下，積極式載入可能會比較沒有效率，因為擷取詳細資料。 不過，在此情況下，積極式載入可提供最佳效能因為相關的資料會顯示每一筆記錄。

如需載入時的效能考量的詳細資訊的相關資料，請參閱節 **「 延遲 」、 「 Eager 和 「 明確載入相關資料的**中[與 Entity Framework 在 ASP 中的讀取相關資料.NET MVC 應用程式](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)主題。

根據預設，資料會依照標示為索引鍵屬性的值而定。 您可以新增的 OrderBy 子句，以指定不同的值進行排序。 在此範例中，預設 StudentID 屬性用於排序。 在 [排序、 分頁和篩選資料](sorting-paging-and-filtering-data.md)主題，可讓使用者選取排序的資料行。

執行您的 web 應用程式，並巡覽至 Students 頁面。 學生頁面會顯示下列的學生資訊。

![顯示資料](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>自動產生的模型繫結方法

在逐步進行本教學課程系列，只要到您的專案複製程式碼從本教學課程。 不過，這種方法的缺點是功能的，您可能不會察覺到自動產生的模型繫結方法的程式碼的 Visual Studio 所提供。 您自己的專案上工作時，自動產生程式碼可以節省時間，並且有助於您深入了解如何實作的作業。 本章節描述的自動程式碼產生功能。 本章節僅供參考，並不包含任何您需要在您的專案中實作的程式碼。

設定的值時**SelectMethod**， **UpdateMethod**， **InsertMethod**，或**DeleteMethod**標記程式碼中的屬性您可以選取**建立新的方法**選項。

![建立新的方法](retrieving-data/_static/image18.png)

Visual Studio 不僅會建立具有適當簽章中，程式碼後置中的方法，也會產生可協助您執行作業的實作程式碼。 如果您先設定**ItemType**屬性，才能使用自動程式碼產生功能，產生的程式碼會使用該類型的作業。 例如，設定 UpdateMethod 屬性時，下列程式碼就會自動產生：

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

同樣地，上述程式碼不需要加入至專案。 在下一個教學課程中，您將實作更新、 刪除和加入新資料的方法。

## <a name="conclusion"></a>結論

在本教學課程中，您可以建立資料模型類別，並從那些類別產生資料庫。 您會將資料庫資料表中填入測試資料。 您用來從資料庫擷取資料的模型繫結，然後 GridView 中顯示資料。

在接下來[教學課程](updating-deleting-and-creating-data.md)在本系列中，您將啟用更新、 刪除和建立資料。

> [!div class="step-by-step"]
> [下一步](updating-deleting-and-creating-data.md)
