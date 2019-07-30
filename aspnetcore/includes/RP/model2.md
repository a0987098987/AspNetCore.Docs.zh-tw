<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="4c85d-101">新增資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="4c85d-101">Add a database context class</span></span>

<span data-ttu-id="4c85d-102">將下列 `RazorPagesMovieContext` 類別新增至 *ata* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="4c85d-102">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="4c85d-103">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4c85d-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="4c85d-104">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="4c85d-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="4c85d-105">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="4c85d-105">Add a database connection string</span></span>

<span data-ttu-id="4c85d-106">將連接字串新增至 *appsettings* 檔案，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="4c85d-106">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="4c85d-107">新增必要的 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="4c85d-107">Add required NuGet packages</span></span>

<span data-ttu-id="4c85d-108">執行下列 .NET Core CLI 命令，以將 SQLite、Entity Framework Core 與 CodeGeneration.Design 新增到專案：</span><span class="sxs-lookup"><span data-stu-id="4c85d-108">Run the following .NET Core CLI commands to add SQLite, Entity Framework Core, and  CodeGeneration.Design to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="4c85d-109">需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="4c85d-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="4c85d-110">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="4c85d-110">Register the database context</span></span>

<span data-ttu-id="4c85d-111">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="4c85d-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="4c85d-112">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="4c85d-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="4c85d-113">新增必要的 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="4c85d-113">Add required NuGet packages</span></span>

<span data-ttu-id="4c85d-114">執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：</span><span class="sxs-lookup"><span data-stu-id="4c85d-114">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="4c85d-115">需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="4c85d-115">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="4c85d-116">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="4c85d-116">Register the database context</span></span>

<span data-ttu-id="4c85d-117">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="4c85d-117">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="4c85d-118">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="4c85d-118">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="4c85d-119">建置專案來檢查錯誤。</span><span class="sxs-lookup"><span data-stu-id="4c85d-119">Build the project as a check for errors.</span></span>
::: moniker-end