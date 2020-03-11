::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="9b2da-101">建立 *Data* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9b2da-101">Create a *Data* folder.</span></span>

<span data-ttu-id="9b2da-102">將下列 `MvcMovieContext` 類別新增至 *ata* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9b2da-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="9b2da-103">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="9b2da-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9b2da-104">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="9b2da-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="9b2da-105">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="9b2da-105">Add a database connection string</span></span>

<span data-ttu-id="9b2da-106">將連接字串新增到 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9b2da-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="9b2da-107">新增 NuGet 套件和 EF 工具</span><span class="sxs-lookup"><span data-stu-id="9b2da-107">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9b2da-108">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9b2da-108">Register the database context</span></span>

<span data-ttu-id="9b2da-109">在 `using`Startup.cs*最上方新增下列* 陳述式：</span><span class="sxs-lookup"><span data-stu-id="9b2da-109">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9b2da-110">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9b2da-110">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="9b2da-111">建置專案以檢查編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="9b2da-111">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9b2da-112">將下列 `MvcMovieContext` 類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9b2da-112">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="9b2da-113">上述程式碼會建立實體集的 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="9b2da-113">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9b2da-114">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="9b2da-114">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="9b2da-115">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="9b2da-115">Add a database connection string</span></span>

<span data-ttu-id="9b2da-116">將連接字串新增到 *appsettings.json* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9b2da-116">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="9b2da-117">新增必要的 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="9b2da-117">Add required NuGet packages</span></span>

<span data-ttu-id="9b2da-118">執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：</span><span class="sxs-lookup"><span data-stu-id="9b2da-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="9b2da-119">需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="9b2da-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9b2da-120">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9b2da-120">Register the database context</span></span>

<span data-ttu-id="9b2da-121">在 `using`Startup.cs*最上方新增下列* 陳述式：</span><span class="sxs-lookup"><span data-stu-id="9b2da-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9b2da-122">使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9b2da-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="9b2da-123">建置專案來檢查錯誤。</span><span class="sxs-lookup"><span data-stu-id="9b2da-123">Build the project as a check for errors.</span></span>
::: moniker-end