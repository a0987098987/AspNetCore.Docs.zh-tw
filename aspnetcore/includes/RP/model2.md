<a name="dc"></a>

### <a name="add-a-database-context-class"></a>新增資料庫內容類別

將下列 `RazorPagesMovieContext` 類別新增至 *ata* 資料夾：

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

上述程式碼會建立實體集的 `DbSet` 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連接字串新增至 *appsettings* 檔案，如下列反白顯示的程式碼所示：

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-required-nuget-packages"></a>新增必要的 NuGet 封裝

執行下列 .NET Core CLI 命令，以將 SQLite、Entity Framework Core 與 CodeGeneration.Design 新增到專案：

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。

<a name="reg"></a>

### <a name="register-the-database-context"></a>登錄資料庫內容

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a>新增必要的 NuGet 封裝

執行下列 .NET Core CLI 命令，以將 SQLite 和 CodeGeneration.Design 新增到專案：

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

需要 `Microsoft.VisualStudio.Web.CodeGeneration.Design` 封裝，才能進行 Scaffolding。

<a name="reg"></a>

### <a name="register-the-database-context"></a>登錄資料庫內容

在 *Startup.cs* 最上方新增下列 `using` 陳述式：

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

使用[相依性插入](xref:fundamentals/dependency-injection)容器，在 `Startup.ConfigureServices` 中註冊資料庫內容。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

建置專案來檢查錯誤。
::: moniker-end