---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 擷取並顯示與資料模型繫結和 web form |Microsoft 文件
author: tfitzmac
description: 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行資料互動詳細直線-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Web form 模型繫結與資料擷取和顯示
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 此教學課程系列示範與 ASP.NET Web Form 專案中使用模型繫結的基本層面。 模型繫結進行更直覺比處理的資料來源物件 （例如 ObjectDataSource 或 SqlDataSource） 的資料互動。 此數列開頭簡介資料，並會移到更進階的概念中之後的教學課程。
> 
> 模型繫結模式適用於任何資料存取技術。 在本教學課程中，您會使用 Entity Framework 中，但是您可以使用最熟悉的資料存取技術。 從資料繫結伺服器控制項，例如 GridView、 ListView、 DetailsView 或在 FormView 的控制項，您可以指定要用於選取、 更新、 刪除和建立資料方法的名稱。 在本教學課程中，您會指定 SelectMethod 的值。
> 
> 在該方法中，您提供的邏輯擷取資料。 在下一個教學課程中，您將 UpdateMethod、 DeleteMethod 和 InsertMethod 設定值。
> 
> 您可以[下載](https://go.microsoft.com/fwlink/?LinkId=286116)以 C# 或 VB 完整的專案 可下載的程式碼適用於 Visual Studio 2012 或 Visual Studio 2013。 它會使用 Visual Studio 2012 範本，這可能會比 Visual Studio 2013 範本顯示在本教學課程稍有不同。
> 
> 教學課程中您要在 Visual Studio 執行應用程式。 您也可以讓應用程式可透過網際網路將它部署至主機服務提供者。 Microsoft 提供的免費 web 裝載中的最多 10 個 web sites  
>  [免費試用的 Azure 帳戶](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。 如需如何將 Visual Studio web 專案部署至 Azure App Service Web 應用程式資訊，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)數列。 該教學課程也會示範如何使用 Entity Framework Code First 移轉將 SQL Server 資料庫部署到 Azure SQL Database。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web
>   
> 
> 本教學課程也會搭配 Visual Studio 2012，但會在使用者介面和專案範本中的一些差異。


## <a name="what-youll-build"></a>您將建置

在此教學課程中，您將會：

1. 建立反映大學和學生課程中註冊的資料物件
2. 建立從物件的資料庫資料表
3. 填入資料庫與測試資料
4. 在網頁表單中顯示資料

## <a name="set-up-project"></a>設定專案

在 Visual Studio 2013 中，建立新**ASP.NET Web 應用程式**呼叫**ContosoUniversityModelBinding**。

![建立專案](retrieving-data/_static/image2.png)

選取 Web Form 範本，並保留預設選項。 按一下 [確定] 以安裝專案。

![選取 web form](retrieving-data/_static/image3.png)

首先，您將幾個自訂網站的外觀進行小變更。 開啟**Site.Master**檔案，並將標題變更為包含 Contoso 大學而非 我的 ASP.NET 應用程式。

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

然後，變更標頭文字**應用程式名稱**至**Contoso 大學**。

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

也在 Site.Master，變更會出現在標頭可反映與此站台的網頁瀏覽連結。 您不需要是**有關**頁面或**連絡人**頁面上，所以可以移除那些連結。 相反地，您必須呼叫頁面的連結**學生**。 尚未建立此頁面。

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

儲存並關閉 Site.Master。

現在，您將建立顯示學生資料的 web 表單。 以滑鼠右鍵按一下您的專案，並**新增****新項目**。 選取**使用主版頁面的 Web Form**範本，並將其命名**Students.aspx**。

![建立頁面](retrieving-data/_static/image5.png)

選取**Site.Master**做為新的 web 表單的主版頁面。

## <a name="create-the-data-models-and-database"></a>建立資料模型和資料庫

您將使用 Code First 移轉建立物件和對應的資料庫資料表。 這些資料表會儲存學生以及他們的課程相關資訊。

在 [模型] 資料夾中，加入名為新類別**UniversityModels.cs**。

![建立模型類別](retrieving-data/_static/image7.png)

這個檔案中定義的 SchoolContext、 學生、 註冊和課程類別，如下所示：

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

SchoolContext 類別衍生自 DbContext，管理資料庫連接和資料中的變更。

在 Student 類別中，請注意已套用至屬性**FirstName**， **LastName**，和**年**屬性。 這些屬性會用於本教學課程中的資料驗證。 若要簡化此示範專案的程式碼，只有這些屬性同時標示為資料驗證屬性。 在真實的專案中，您可以將驗證屬性套用至需要驗證的資料，例如註冊和課程類別中屬性的所有屬性。

儲存 UniversityModels.cs。

您將使用 Code First 移轉的工具來設定資料庫，這些類別為基礎。

在**Package Manager Console**，執行命令：  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

如果命令成功完成，您會收到訊息，指出 已啟用移轉，

![啟用移轉](retrieving-data/_static/image8.png)

請注意，新的檔案命名為**configuration.cs 中**已建立。 在 Visual Studio 中開啟此檔案會自動建立後。 設定類別包含**種子**方法可讓您預先填入的測試資料的資料庫資料表。

Seed 方法中加入下列程式碼。 您必須新增**使用**陳述式**ContosoUniversityModelBinding.Models**命名空間。

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

儲存 configuration.cs。

在 Package Manager Console 中，執行命令`add-migration initial`。

然後，執行命令`update-database`。

如果執行此命令時，您會收到例外狀況，，則可能 StudentID 和 CourseID 的值具有不同的種子方法中的值。 開啟資料庫中的這些資料表，並找出 StudentID 和 CourseID 現有的值。 在植入的註冊項目資料表程式碼加入這些值。

已加入的資料庫檔案，但是它目前隱藏在專案中。 按一下**顯示所有檔案**顯示的檔案。

![顯示所有檔案](retrieving-data/_static/image10.png)

請注意.mdf 檔案現在會出現在應用程式\_Data 資料夾。

![資料庫檔案](retrieving-data/_static/image12.png)

按兩下.mdf 檔案，然後開啟 [伺服器總管] 中。 資料表現在存在，而且會填入資料。

![資料庫資料表](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>顯示學生版和相關的資料表的資料

資料庫中的資料，您現在準備擷取該資料，並將其顯示在網頁中。 您將使用**GridView**控制項來顯示資料行和資料列中的資料。

開啟 Students.aspx，並找出**MainContent**預留位置。 預留位置，在加入**GridView**包含下列程式碼的控制項。

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

有幾個重要的概念，您會注意到這個標記程式碼中。 首先，請注意，為設定值， **SelectMethod** GridView 項目中的屬性。 這個值會指定用來擷取資料的 GridView 方法的名稱。 您將在下一個步驟中建立這個方法。 第二，請注意， **ItemType**屬性設定為您稍早建立的學生類別。 藉由設定此值，您可以參考該類別的標記程式碼中的屬性。 例如，Student 類別包含名為註冊的集合。 您可以使用**Item.Enrollments**來擷取該集合，然後使用 LINQ 語法擷取每個學生的已註冊的點數總和。

在程式碼後置檔案中，您需要加入指定的方法**SelectMethod**值。 開啟**Students.aspx.cs**，並加入**使用**陳述式**ContosoUniversityModelBinding.Models**和**System.Data.Entity**命名空間。

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

然後，加入下列方法。 請注意此方法的名稱符合您提供為 SelectMethod 的名稱。

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include**子句可改善此查詢的效能，但不是不可或缺的查詢工作。 不使用 Include 子句中，擷取的資料會使用消極式載入，需要傳送另一個資料庫查詢每次擷取資料的相關。 藉由提供的 Include 子句，會使用積極式載入，這表示透過資料庫的單一查詢擷取的所有相關資料的擷取資料。 在案例中的大部分相關資料不能使用的積極式載入可能會比較沒有效率，因為擷取詳細資料。 不過，在此情況下，積極式載入提供最佳效能因為相關的資料會顯示每一筆記錄。

載入時的效能考量的詳細資訊的相關資料，請參閱節**Lazy、 Eager 和明確載入相關資料的**中[讀取與 Entity Framework ASP 的相關資料.NET MVC 應用程式](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)主題。

根據預設，資料會依照標示為索引鍵屬性的值。 您可以加入 OrderBy 子句，以指定不同的值進行排序。 在此範例中，預設 StudentID 屬性用來排序。 在[排序、 分頁和篩選資料](sorting-paging-and-filtering-data.md)主題，可讓使用者選取的資料行進行排序。

執行 web 應用程式，並瀏覽至學生頁面。 學生頁面會顯示下列的學生資訊。

![顯示資料](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>自動產生的模型繫結方法

當使用透過本教學課程系列時，只要您的專案複製程式碼從教學課程。 不過，這種方法的缺點是功能的，您可能不會察覺到自動產生的模型繫結方法的程式碼的 Visual Studio 所提供。 在處理您的專案，自動產生程式碼可以節省您時間和說明，您就可以了解如何實作作業。 本章節描述自動程式碼產生功能。 本章節僅供參考，並不包含您要在您的專案中實作的任何程式碼。

設定的值時**SelectMethod**， **UpdateMethod**， **InsertMethod**，或**DeleteMethod**標記程式碼中的屬性您可以選取**建立新的方法**選項。

![建立新的方法](retrieving-data/_static/image18.png)

Visual Studio 不僅會在程式碼後置具有適當簽章，建立方法，也會產生實作程式碼，以協助您執行此作業。 如果您先設定**ItemType**屬性，才能使用自動程式碼產生功能，產生的程式碼會使用該類型的作業。 例如，設定 UpdateMethod 屬性時，下列程式碼就會自動產生：

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

同樣地，上述程式碼不需要加入至專案。 在下一個教學課程中，您將實作更新、 刪除和加入新資料的方法。

## <a name="conclusion"></a>結論

在本教學課程中，您可以建立資料模型類別，並從這些類別中產生資料庫。 您的測試資料填入資料庫資料表。 您用來從資料庫擷取資料的模型繫結，並接著會顯示資料在 GridView。

在接下來[教學課程](updating-deleting-and-creating-data.md)在此數列，您將啟用更新、 刪除和建立的資料。

> [!div class="step-by-step"]
> [下一步](updating-deleting-and-creating-data.md)
