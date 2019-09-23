::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

建立 *Data* 資料夾。

將下列 `MvcMovieContext` 類別新增至 *ata* 資料夾：  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

上述程式碼會建立實體集的 `DbSet` 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連接字串新增到 *appsettings.json* 檔案：

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a>新增 NuGet 套件和 EF 工具

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a>登錄資料庫內容

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

建置專案以檢查編譯器錯誤。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

將下列 `MvcMovieContext` 類別新增至 *Models* 資料夾：  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

上述程式碼會建立實體集的 `DbSet` 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連接字串新增到 *appsettings.json* 檔案：

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>新增必要的 NuGet 封裝

執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。

<a name="reg"></a>

### <a name="register-the-database-context"></a>登錄資料庫內容

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

建置專案來檢查錯誤。
::: moniker-end