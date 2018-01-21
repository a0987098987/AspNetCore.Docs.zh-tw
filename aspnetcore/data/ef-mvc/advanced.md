---
title: "使用 EF Core-ASP.NET Core MVC 進階-10-10"
author: tdykstra
description: "此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET web 應用程式的基本概念的主題。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: ea83e5b17df80e5615dda49335247340d1cfb016
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>進階的主題-EF Core 與 ASP.NET Core MVC 教學課程 (10-10)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。 教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。

在上一個教學課程中，您可以實作資料表每個階層繼承。 此教學課程介紹幾個有用，可注意當您超越開發使用 Entity Framework 核心的 ASP.NET Core web 應用程式的基本概念的主題。

## <a name="raw-sql-queries"></a>原始的 SQL 查詢

使用 Entity Framework 的優點之一是它可避免中斷您太接近儲存資料的特定方法的程式碼。 它會產生 SQL 查詢和命令，這也讓您不必自行撰寫。 但有例外狀況，當您需要執行手動建立的特定 SQL 查詢。 針對這些案例，Entity Framework 程式碼的第一個 API 會包含可讓您將直接對資料庫的 SQL 命令的方法。 您在 EF Core 1.0 中有下列選項：

* 使用`DbSet.FromSql`查詢來傳回實體類型的方法。 傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您[關閉追蹤](crud.md#no-tracking-queries)。

* 使用`Database.ExecuteSqlCommand`非查詢命令。

如果您需要執行可傳回類型不是實體的查詢，您可以使用 ADO.NET 與 EF 所提供的資料庫連接。 即使您使用這個方法來擷取實體類型不被追蹤的資料庫內容，傳回的資料。

因為永遠是 true 時您 web 應用程式中執行 SQL 命令，您必須採取一些預防措施以保護您的網站，SQL 資料隱碼攻擊。 方法之一是使用參數化的查詢，以確定網頁所提交的字串，無法解譯為 SQL 命令。 在本教學課程中，您將使用參數化的查詢時將使用者輸入整合到查詢。

## <a name="call-a-query-that-returns-entities"></a>呼叫傳回實體的查詢

`DbSet<TEntity>`類別會提供方法可讓您執行查詢，以傳回型別的實體`TEntity`。 若要查看這對您所做的運作將會在變更程式碼`Details`部門控制站的方法。

在*DepartmentsController.cs*，請在`Details`方法，擷取的部門使用的程式碼取代`FromSql`方法呼叫，如下列反白顯示的程式碼所示：

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

若要確認新的程式碼正常運作，請選取**部門** 索引標籤，然後**詳細資料**其中一個部門。

![部門詳細資料](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>呼叫會傳回其他類型的查詢

先前您建立學生統計資料方格中，以顯示每個註冊日期的學生總數一樣的 「 關於 」 頁面。 從學生實體集取得資料 (`_context.Students`) 並將結果規劃一份使用 LINQ`EnrollmentDateGroup`檢視模型物件。 假設您想要撰寫 SQL 本身而不是使用 LINQ。 若要執行此操作，您必須執行 SQL 查詢會傳回實體物件以外的項目。 在 EF Core 1.0，方法之一是撰寫 ADO.NET 程式碼，並從 EF 取得資料庫連接。

在*HomeController.cs*，取代`About`方法取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

加入 using 陳述式：

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

執行應用程式，並移至 [關於] 頁面。 它會顯示相同的資料以前一樣。

![有關頁面](advanced/_static/about.png)

## <a name="call-an-update-query"></a>呼叫更新查詢

假設 Contoso 大學系統管理員想要在資料庫中，例如變更的每個課程信用額度數量執行全域的變更。 如果該大學有大量的課程，很效率不佳，擷取這些全部都做為實體，並將它們個別變更。 在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您將執行 SQL UPDATE 陳述式來進行變更。 網頁看起來像下圖：

![更新課程信用額度頁面](advanced/_static/update-credits.png)

在*CoursesContoller.cs*，HttpGet 和 HttpPost 加入 UpdateCourseCredits 方法：

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

當控制器處理 HttpGet 要求時，不會傳回在`ViewData["RowsAffected"]`，並檢視顯示空白的文字方塊和送出按鈕，在上圖所示。

當**更新**按鈕、 HttpPost 方法呼叫時，和倍增器已在文字方塊中輸入的值。 然後執行的程式碼會更新課程並傳回受影響的資料列數目至檢視中的 SQL `ViewData`。 當檢視表取得`RowsAffected`值，它會顯示更新的資料列數目。

在**方案總管 中**，以滑鼠右鍵按一下*檢視/課程*資料夾，然後再按一下**新增 > 新的項目**。

在**加入新項目**] 對話方塊中，按一下**ASP.NET**下**已安裝**在左窗格中，按一下 [ **MVC 檢視頁面**，並將新的檢視*UpdateCourseCredits.cshtml*。

在*Views/Courses/UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

執行`UpdateCourseCredits`方法藉由選取**課程** 索引標籤，然後再新增 「 / UpdateCourseCredits"瀏覽器的網址列中的 URL 的結尾 (例如： `http://localhost:5813/Courses/UpdateCourseCredits`)。 在文字方塊中輸入的數字：

![更新課程信用額度頁面](advanced/_static/update-credits.png)

按一下 [更新] 。 您會看到受影響的資料列數目：

![更新課程信用額度頁面上受影響的資料列](advanced/_static/update-credits-rows-affected.png)

按一下**返回清單**若要查看課程信用額度的修訂編號取代清單。

請注意，實際執行程式碼會確定一律更新中有效的資料結果。 簡化如下所示的程式碼無法將信用額度足以導致大於 5 的數字的數目。 (`Credits`屬性具有`[Range(0, 5)]`屬性。)更新查詢可行，但是無效的資料可能會導致系統其他部分假設信用額度的數目是 5 或更少的非預期的結果。

如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://docs.microsoft.com/ef/core/querying/raw-sql)。

## <a name="examine-sql-sent-to-the-database"></a>檢查傳送至資料庫的 SQL

有時候很有幫助能夠看到實際傳送至資料庫的 SQL 查詢。 內建的記錄功能，適用於 ASP.NET Core 自動使用 EF 核心撰寫包含 SQL 查詢和更新的記錄檔。 在本節中，您會看到 SQL 記錄的一些範例。

開啟*StudentsController.cs*和`Details`方法上設定中斷點`if (student == null)`陳述式。

執行應用程式中偵錯模式，並移至詳細資料頁面的學生。

移至**輸出**視窗顯示偵錯輸出，而且您會看到查詢：

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

您會發現這裡喜歡的項目可能會令您吃驚： SQL 選取最多 2 個資料列 (`TOP(2)`) 從 Person 資料表。 `SingleOrDefaultAsync`方法未解析為 1 個資料列的伺服器上。 原因如下：

* 如果查詢會傳回多個資料列，則方法會傳回 null。
* 若要判斷查詢是否會傳回多個資料列，必須檢查是否它會傳回至少 2 EF。

請注意，您不必使用偵錯模式，並取得記錄輸出中的中斷點處停止**輸出**視窗。 它只是便利方式停止您想要查看輸出的點記錄。 如果不這樣做，請繼續記錄，您必須捲動至尋找您感興趣的部分。

## <a name="repository-and-unit-of-work-patterns"></a>儲存機制和單位的工作模式

許多開發人員撰寫程式碼以搭配 Entity Framework 的程式碼周圍的包裝函式實作的儲存機制和工作模式的單位。 這些模式被用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。 實作這些模式可以協助您隔離您的應用程式資料存放區中的變更，以及有助於自動化的單元測試為導向的開發 (TDD)。 不過，撰寫額外的程式碼來實作這些模式做不一定有數種原因使用 EF，應用程式的最佳選擇：

* EF 內容類別本身，以隔離您的程式碼，從資料存放區特有的程式碼。

* EF 內容類別可做為資料庫的工作單位的類別可讓您更新您不要使用 EF。

* EF 包含而不需要撰寫的程式碼儲存機制實作 TDD 的功能。

如需如何實作儲存機制和工作模式單位資訊，請參閱[此教學課程系列的 Entity Framework 5 新版](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)。

Entity Framework Core 實作可用於測試的記憶體中資料庫提供者。 如需詳細資訊，請參閱[測試 InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory)。

## <a name="automatic-change-detection"></a>自動變更偵測

Entity Framework 藉由比較原始值與實體的目前值，決定如何變更實體 （並因此需要哪些更新傳送至資料庫）。 查詢或附加的實體時，會儲存原始值。 會導致變更自動偵測的方法如下所示：

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善`ChangeTracker.AutoDetectChangesEnabled`屬性。 例如: 

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core 來源的程式碼及開發計劃

Entity Framework Core 的原始程式碼位於[https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore)。 除了原始程式碼，您可以取得夜間組建，發出追蹤，功能規格、 設計會議記錄[未來的開發藍圖](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap)，以及其他更多。 您可以檔 bug，而且您可能會造成您自己的 EF 原始碼的增強功能。

雖然開啟原始碼時，Entity Framework Core 則完全支援以 Microsoft 產品。 Microsoft Entity Framework 小組將控制接受哪些比重保留，並測試所有的程式碼變更，以確保每次發行的品質。

## <a name="reverse-engineer-from-existing-database"></a>從現有資料庫的反向工程

若要反向工程資料模型，包括實體類別，從現有的資料庫，使用[scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext)命令。 請參閱[-快速入門教學課程](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db)。

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>使用動態的 LINQ 來簡化排序選取範圍的程式碼

[本系列的第三個教學課程](sort-filter-page.md)示範以硬式編碼的資料行名稱來撰寫 LINQ 程式碼`switch`陳述式。 可從中選擇的兩個資料行，這可正常運作，但如果您有許多資料行的程式碼可以取得詳細資訊。 若要解決這個問題，您可以使用`EF.Property`方法，以指定的屬性名稱做為字串。 若要試用這種方法，取代`Index`方法中的`StudentsController`為下列程式碼。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>後續步驟

如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework Core。

如需 EF Core 的詳細資訊，請參閱[Entity Framework 的核心文件集](https://docs.microsoft.com/ef/core)。 也會提供一本書：[動作中的 Entity Framework Core](https://www.manning.com/books/entity-framework-core-in-action)。

如需如何部署 web 應用程式的資訊，請參閱[主機和部署](xref:host-and-deploy/index)。

如需 ASP.NET Core MVC、 驗證和授權，例如相關的其他主題，請參閱[ASP.NET Core 文件](https://docs.microsoft.com/aspnet/core/)。

## <a name="acknowledgments"></a>通知

Tom Dykstra 和 Rick Anderson (twitter @RickAndMSFT) 撰寫本教學課程。 Rowan Miller、 狄 Vega 和 Entity Framework 小組的其他成員協助程式碼檢閱，並幫助我們已針對教學課程中撰寫程式碼時，會發生的問題進行偵錯。

## <a name="common-errors"></a>常見的錯誤  

### <a name="contosouniversitydll-used-by-another-process"></a>另一個處理序所使用的 ContosoUniversity.dll

錯誤訊息：

> 無法開啟 '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 進行寫入-' 處理序無法存取檔案 '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' 因為另一個處理序正在使用它。

解決方案:

停止 IIS Express 中的站台。 請移至 Windows 系統匣中，尋找 IIS Express 和它的圖示上按一下滑鼠右鍵，選取 Contoso 大學站台，然後按一下**停止站台**。

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>移轉建立結構中向上和向下方法沒有程式碼

可能的原因：

EF CLI 命令不會自動關閉並儲存程式碼檔案。 如果您未儲存變更，當您執行`migrations add`命令、 EF 找不到您的變更。

解決方案:

執行`migrations remove`命令，儲存您的程式碼變更，並重新執行`migrations add`命令。

### <a name="errors-while-running-database-update"></a>執行資料庫更新時發生錯誤

很可能在具有現有資料的資料庫進行結構描述變更時，取得其他錯誤。 如果您移轉時發生錯誤，您無法解決，您可以變更連接字串中的資料庫名稱，或刪除資料庫。 使用新資料庫時，若要移轉，沒有資料並更新資料庫指令更有可能能順利完成。

最簡單的方法是在資料庫重新命名*appsettings.json*。 下次您執行`database update`，就會建立新的資料庫。

若要刪除 SSOX 中的資料庫，以滑鼠右鍵按一下資料庫，請按一下**刪除**，然後在**資料庫刪除**對話方塊中，選取**關閉現有連接**按一下**確定**。

若要使用 CLI 來刪除資料庫，執行`database drop`CLI 命令：

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>尋找 SQL Server 執行個體時發生錯誤

錯誤訊息：

> 建立 SQL Server 的連接時發生網路相關或執行個體特定錯誤。 找不到或無法存取伺服器。 確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線 (提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)

解決方案:

請檢查連接字串。 如果您已經手動刪除資料庫檔案，變更建構字串以使用新的資料庫中資料庫的名稱。

>[!div class="step-by-step"]
[上一步](inheritance.md)
