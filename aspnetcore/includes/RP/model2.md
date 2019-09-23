<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="22e60-101">新增資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="22e60-101">Add a database context class</span></span>

<span data-ttu-id="22e60-102">在 RazorPagesMovie 專案中，建立名為 *Data* 的新資料夾。</span><span class="sxs-lookup"><span data-stu-id="22e60-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="22e60-103">將下列 `RazorPagesMovieContext` 類別新增至 *ata* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="22e60-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="22e60-104">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="22e60-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="22e60-105">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="22e60-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="22e60-106">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="22e60-106">Add a database connection string</span></span>

<span data-ttu-id="22e60-107">將連接字串新增至 *appsettings* 檔案，如下列反白顯示的程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="22e60-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="22e60-108">新增 NuGet 套件和 EF 工具</span><span class="sxs-lookup"><span data-stu-id="22e60-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="22e60-109">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="22e60-109">Register the database context</span></span>

<span data-ttu-id="22e60-110">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="22e60-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="22e60-111">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="22e60-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="22e60-112">新增必要的 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="22e60-112">Add required NuGet packages</span></span>

<span data-ttu-id="22e60-113">執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：</span><span class="sxs-lookup"><span data-stu-id="22e60-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="22e60-114">需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="22e60-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="22e60-115">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="22e60-115">Register the database context</span></span>

<span data-ttu-id="22e60-116">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="22e60-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="22e60-117">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="22e60-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="22e60-118">建置專案來檢查錯誤。</span><span class="sxs-lookup"><span data-stu-id="22e60-118">Build the project as a check for errors.</span></span>
::: moniker-end
