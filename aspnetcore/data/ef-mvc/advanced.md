---
title: 教學課程：瞭解 advanced 案例-使用 EF Core ASP.NET MVC
description: 本教學課程介紹一些實用主題，這些主題超出開發 ASP.NET Core Web 應用程式 (使用 Entity Framework Core ) 的基本概念。
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
ms.topic: tutorial
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: data/ef-mvc/advanced
ms.openlocfilehash: ebeb581cf79f2d2ab60de7df43d042fa3185cd32
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86212737"
---
# <a name="tutorial-learn-about-advanced-scenarios---aspnet-mvc-with-ef-core"></a>教學課程：瞭解 advanced 案例-使用 EF Core ASP.NET MVC

在上一個教學課程中，您實作了單表繼承。 本教學課程介紹幾個實用的主題，在超出開發 ASP.NET Core Web 應用程式 (使用 Entity Framework Core ) 的基本概念時，需要注意這些主題。

在本教學課程中，您：

> [!div class="checklist"]
> * 執行原始 SQL 查詢
> * 呼叫查詢以傳回實體
> * 呼叫查詢以傳回其他類型
> * 呼叫更新查詢
> * 檢查 SQL 查詢
> * 建立抽象層
> * 了解自動變更偵測
> * 了解 EF Core 原始程式碼和開發計劃
> * 了解如何使用動態 LINQ 來簡化程式碼

## <a name="prerequisites"></a>必要條件

* [實作繼承](inheritance.md)

## <a name="perform-raw-sql-queries"></a>執行原始 SQL 查詢

使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。 它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。 但當您需要執行手動建立的特定 SQL 查詢時，會出現例外情況。 針對這些情況，Entity Framework Code 的第一個 API 會包含可讓您將 SQL 命令直接傳遞至資料庫的方法。 EF Core 1.0 中有下列選項可供您選擇：

* 針對傳回實體類型的查詢使用 `DbSet.FromSql` 方法。 傳回的物件必須是 `DbSet` 物件所預期的類型，而且除非您[關閉追蹤](crud.md#no-tracking-queries)，否則資料庫內容將自動追蹤它們。

* 針對非查詢命令使用 `Database.ExecuteSqlCommand`。

如果您需要執行傳回類型不是實體的查詢，則可以搭配使用 ADO.NET 與 EF 所提供的資料庫連線。 即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。

如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。 執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。 在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。

## <a name="call-a-query-to-return-entities"></a>呼叫查詢以傳回實體

`DbSet<TEntity>` 類別會提供一種方法，您可使用它來執行查詢，以傳回類型 `TEntity` 的實體。 若要查看其運作方式，您需要變更 Department 控制器的 `Details` 方法中的程式碼。

在 *DepartmentsController.cs* 中，將 `Details` 方法中擷取部門的程式碼取代為 `FromSql` 方法呼叫，如下列醒目提顯示的程式碼所示：

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10)]

若要確認新的程式碼運作正常，請選取 [部門] **** 索引標籤，然後針對其中一個部門選取 [詳細資料] ****。

![部門詳細資料](advanced/_static/department-details.png)

## <a name="call-a-query-to-return-other-types"></a>呼叫查詢以傳回其他類型

先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。 您已從學生實體集 (`_context.Students`) 取得資料，並使用 LINQ 將結果投射到 `EnrollmentDateGroup` 檢視模型物件清單。 假設您想要撰寫 SQL 本身，而不是使用 LINQ。 若要執行這項操作，您必須執行 SQL 查詢以傳回實體物件以外的某些項目。 在 EF Core 1.0 中，執行這項操作的方法之一是撰寫 ADO.NET 程式碼，並從 EF 取得資料庫連線。

在 *HomeController.cs* 中，以下列程式碼取代 `About` 方法：

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

新增 using 陳述式：

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

執行應用程式並移至 About 頁面。 它會顯示與之前相同的資料。

![About 頁面](advanced/_static/about.png)

## <a name="call-an-update-query"></a>呼叫更新查詢

假設 Contoso 大學的系統管理員想要在資料庫中執行全域變更，例如變更每個課程的學分數。 如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。 在本節中，您將實作網頁，讓使用者能夠指定變更所有課程的學分數所要依據的因素，並將執行 SQL UPDATE 陳述式來進行變更。 網頁看起來將如下圖所示：

![更新課程學分數頁面](advanced/_static/update-credits.png)

在 *CoursesController.cs* 中，為 HttpGet 和 HttpPost 新增 UpdateCourseCredits 方法：

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

當控制器處理 HttpGet 要求時，不會在 `ViewData["RowsAffected"]` 中傳回任何項目，而檢視會顯示空白的文字方塊和提交按鈕，如上圖所示。

按一下 [更新]**** 按鈕後，會呼叫 HttpPost 方法，而乘數具有在文字方塊中輸入的值。 程式碼接著執行的 SQL 會更新課程，並將受影響的資料列數目傳回至 `ViewData` 中的檢視。 當檢視取得 `RowsAffected` 值時，它會顯示更新的資料列數目。

在方案總管**** 中，以滑鼠右鍵按一下 *Views/Courses* 資料夾，然後按一下 [新增] > [新增項目]****。

在 [**加入新專案**] 對話方塊中，按一下左窗格中 [**已安裝**] 底下的 [ **ASP.NET Core** ]，按一下 [ ** Razor View**]，並將新的 view 命名為*UpdateCourseCredits*。

在 *Views/Courses/UpdateCourseCredits.cshtml* 中，以下列程式碼取代範本程式碼：

[!code-cshtml[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

藉由選取 [課程] **** 索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:5813/Courses/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。 在文字方塊中輸入數目：

![更新課程學分數頁面](advanced/_static/update-credits.png)

按一下 [更新]  。 您會看到受影響的資料列數目：

![更新課程學分數頁面之受影響的資料列](advanced/_static/update-credits-rows-affected.png)

按一下 [回到清單]****，以查看課程與已修訂學分數的清單。

請注意，生產環境程式碼可確保更新一律會產生有效的資料。 此處顯示的簡化程式碼會增加足夠的學分數而使其數目大於 5。  (`Credits` 屬性具有 `[Range(0, 5)]` 屬性。 ) 更新查詢會正常執行，但是不正確資料可能會導致系統的其他部分假設點數為5或更少，而造成非預期的結果。

如需原始 SQL 查詢的詳細資訊，請參閱[原始 SQL 查詢](/ef/core/querying/raw-sql)。

## <a name="examine-sql-queries"></a>檢查 SQL 查詢

有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。 EF Core 會自動使用 ASP.NET Core 的內建記錄功能來寫入記錄檔，以包含用於查詢和更新的 SQL。 在本節中，您將看到 SQL 記錄的一些範例。

開啟 *StudentsController.cs*，並在 `Details` 方法中設定 `if (student == null)` 陳述式的中斷點。

以偵錯模式執行應用程式，並移至學生的 [詳細資料] 頁面。

移至顯示偵錯輸出的 [輸出] **** 視窗，您會看到查詢：

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

您在這裡將發現可能會令您吃驚的某些事項：SQL 從 Person 資料表最多選取 2 個資料列 (`TOP(2)`)。 `SingleOrDefaultAsync` 方法不會解析為伺服器上的 1 個資料列。 原因如下：

* 如果查詢會傳回多個資料列，則方法會傳回 null。
* 若要判斷查詢是否會傳回多個資料列，EF 必須檢查它是否會至少傳回 2。

請注意，您不必使用偵錯模式並在中斷點處停止，便能在 [輸出] **** 視窗中取得記錄輸出。 它只是在您想要查看輸出的點上停止記錄的便利方式。 如果不這樣做，記錄將繼續進行，而您必須往回捲動以尋找您感興趣的部分。

## <a name="create-an-abstraction-layer"></a>建立抽象層

許多開發人員撰寫程式碼以實作存放庫和工作單元模式，作為使用 Entity Framework 之程式碼周圍的包裝函式。 這些模式主要用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。 實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。 不過，撰寫額外的程式碼來實作這些模式並非一直是使用 EF 之應用程式的最佳選擇，原因如下：

* EF 內容類別本身會隔離您的程式碼與資料存放區特有的程式碼。

* EF 內容類別可作為您使用 EF 進行之資料庫更新的工作單元類別。

* EF 包含實作 TDD 而不需要撰寫存放庫程式碼的功能。

如需如何實作存放庫和工作單元模式的資訊，請參閱[本教學課程系列的 Entity Framework 第 5 版](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。

Entity Framework Core 實作了可用於測試的記憶體資料庫提供者。 如需詳細資訊，請參閱[使用 InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)。

## <a name="automatic-change-detection"></a>自動變更偵測

Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。 查詢或附加實體時，會儲存原始值。 會導致自動變更偵測的一些方法如下：

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，您可能會透過使用 `ChangeTracker.AutoDetectChangesEnabled` 屬性暫時關閉自動變更偵測，使效能獲得顯著改善。 例如：

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="ef-core-source-code-and-development-plans"></a>EF Core 原始程式碼和開發計劃

Entity Framework Core 來源位於 [https://github.com/dotnet/efcore](https://github.com/dotnet/efcore) 。 EF Core 存放庫包含每夜組建、問題追蹤、功能規格、設計會議記錄和[未來開發藍圖](https://github.com/dotnet/efcore/wiki/Roadmap)。 您可以提交或尋找 Bug，並做出貢獻。

雖然原始程式碼是開放式程式碼，但 Entity Framework Core 也作為 Microsoft 產品完整支援。 Microsoft Entity Framework 小組將控制接受哪些貢獻，並測試所有的程式碼變更以確保每次發行的品質。

## <a name="reverse-engineer-from-existing-database"></a>從現有資料庫進行還原工程

若要從現有資料庫對包括實體類別的資料模型進行還原工程，請使用 [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) 命令。 請參閱[快速入門教學課程](/ef/core/get-started/aspnetcore/existing-db)。

<a id="dynamic-linq"></a>

## <a name="use-dynamic-linq-to-simplify-code"></a>使用動態 LINQ 來簡化程式碼

[本系列的第三個教學課程](sort-filter-page.md)示範如何在 `switch` 陳述式中，以硬式編碼的資料行名稱來撰寫 LINQ 程式碼。 若有兩個資料行可供選擇，這可正常運作；但是如果您有許多資料行，程式碼可能變得冗長。 若要解決該問題，您可以使用 `EF.Property` 方法，以指定屬性的名稱作為字串。 若要試用這種方法，請以下列程式碼取代 `StudentsController` 中的 `Index` 方法。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="acknowledgments"></a>通知

Tom Dykstra 和 Rick Anderson (Twitter @RickAndMSFT) 撰寫了本教學課程。 Rowan Miller、Diego Vega 和其他 Entity Framework 小組成員協助進行程式碼檢閱，並協助對撰寫本教學課程的程式碼時發生的問題進行偵錯。 John Parente 和 Paul Goldman 更新了 ASP.NET Core 2.2 的教學課程。

<a id="common-errors"></a>

## <a name="troubleshoot-common-errors"></a>常見問題疑難排解

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll 已由其他處理序使用

錯誤訊息：

> 無法開啟 '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 進行寫入 -- '處理序無法存取 '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 檔案，因為其他處理序正在使用它。

解決方案：

停止 IIS Express 中的網站。 移至 Windows 系統匣中，尋找 IIS Express 並以滑鼠右鍵按一下其圖示，選取 Contoso 大學網站，然後按一下 [停止網站]****。

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Up 和 Down 方法中沒有程式碼的 Scaffold 移轉

可能的原因：

EF CLI 命令不會自動關閉並儲存程式碼檔案。 如果您有未儲存的變更，當您執行 `migrations add` 命令時，EF 找不到您的變更。

解決方案：

執行 `migrations remove` 命令，儲存您的程式碼變更，並重新執行 `migrations add` 命令。

### <a name="errors-while-running-database-update"></a>執行資料庫更新時發生錯誤

在具有現有資料的資料庫中進行結構描述變更時，可能會收到其他錯誤。 如果收到無法解決的移轉錯誤，您可以變更連接字串中的資料庫名稱，或刪除該資料庫。 使用新資料庫時，沒有可移轉的資料，因此 update-database 命令更可能會在沒有錯誤的情況下完成。

最簡單的方法是在 *appsettings.json* 中重新命名資料庫。 下次您執行 `database update` 時，就會建立新的資料庫。

若要刪除 SSOX 中的資料庫，請以滑鼠右鍵按一下該資料庫，按一下 [刪除]****，然後在 [刪除資料庫]**** 對話方塊中選取 [關閉現有的連線]****，並按一下 [確定]****。

若要使用 CLI 來刪除資料庫，請執行 `database drop` CLI 命令：

```dotnetcli
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>搜尋 SQL Server 執行個體時發生錯誤

錯誤訊息：

> 和 SQL Server 建立連線時，發生與網路相關或執行個體特定的錯誤。 找不到或無法存取伺服器。 檢查執行個體名稱是否正確以及 SQL Server 執行個體是否設定為允許遠端連接。 (提供者：SQL 網路介面，錯誤：26 - 搜尋指定的伺服器/執行個體時發生錯誤)

解決方案：

檢查連接字串。 如果您已手動刪除資料庫檔案，請變更建構字串的資料庫名稱，以重新開始使用新的資料庫。

## <a name="get-the-code"></a>取得程式碼

[下載或檢視已完成的應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="additional-resources"></a>其他資源

如需 EF Core 的詳細資訊，請參閱 [Entity Framework Core 文件](/ef/core)。 另外，還提供了一本書：[Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action) (Entity Framework Core 實戰)。

如需如何部署 Web 應用程式的資訊，請參閱<xref:host-and-deploy/index>。

如需與 ASP.NET Core MVC 相關之其他主題 (例如驗證和授權) 的資訊，請參閱 <xref:index>。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您：

> [!div class="checklist"]
> * 執行原始 SQL 查詢
> * 呼叫查詢以傳回實體
> * 呼叫查詢以傳回其他類型
> * 呼叫更新查詢
> * 檢查 SQL 查詢
> * 建立抽象層
> * 了解自動變更偵測
> * 了解 EF Core 原始程式碼和開發計劃
> * 了解如何使用動態 LINQ 來簡化程式碼

如此即完成本系列中在 ASP.NET Core MVC 應用程式中使用 Entity Framework Core 的教學課程。 這一系列教學課程使用了新的資料庫，您也可以從現有的資料庫反向建構模型。

> [!div class="nextstepaction"]
> [教學課程：使用 MVC、現有的資料庫 EF Core](/ef/core/get-started/aspnetcore/existing-db?toc=/aspnet/core/toc.json&bc=/aspnet/core/breadcrumb/toc.json)
