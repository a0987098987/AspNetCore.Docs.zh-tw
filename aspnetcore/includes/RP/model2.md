<a name="dc"></a>
### 

<span data-ttu-id="3f779-101">將下列 `RazorPagesMovieContext` 類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="3f779-101">Add the following `RazorPagesMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="3f779-102">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3f779-102">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="3f779-103">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="3f779-103">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="3f779-104">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="3f779-104">Add a database connection string</span></span>

<span data-ttu-id="3f779-105">將連接字串新增到 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3f779-105">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="3f779-106">新增必要的 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="3f779-106">Add required NuGet packages</span></span>

<span data-ttu-id="3f779-107">執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：</span><span class="sxs-lookup"><span data-stu-id="3f779-107">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="3f779-108">需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="3f779-108">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="3f779-109">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="3f779-109">Register the database context</span></span>

<span data-ttu-id="3f779-110">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="3f779-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="3f779-111">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="3f779-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="3f779-112">建置專案來檢查錯誤。</span><span class="sxs-lookup"><span data-stu-id="3f779-112">Build the project as a check for errors.</span></span>
