<span data-ttu-id="5fcad-101">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="5fcad-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="5fcad-102">上述命令會新增：</span><span class="sxs-lookup"><span data-stu-id="5fcad-102">The preceding commands add:</span></span>

* <span data-ttu-id="5fcad-103">[阿斯普內特代碼產生器文手架工具](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。</span><span class="sxs-lookup"><span data-stu-id="5fcad-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="5fcad-104">.NET 核心 CLI 的實體框架核心工具。</span><span class="sxs-lookup"><span data-stu-id="5fcad-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="5fcad-105">EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。</span><span class="sxs-lookup"><span data-stu-id="5fcad-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="5fcad-106">Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。</span><span class="sxs-lookup"><span data-stu-id="5fcad-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>

<span data-ttu-id="5fcad-107">有關允許應用按環境設定其資料庫上下文的多個環境配置的指導,請參閱<xref:fundamentals/environments#environment-based-startup-class-and-methods>。</span><span class="sxs-lookup"><span data-stu-id="5fcad-107">For guidance on multiple environment configuration that permits an app to configure its database contexts by environment, see <xref:fundamentals/environments#environment-based-startup-class-and-methods>.</span></span>

[!INCLUDE[](~/includes/scaffoldTFM.md)]