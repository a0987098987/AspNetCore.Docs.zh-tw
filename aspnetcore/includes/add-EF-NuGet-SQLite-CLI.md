<span data-ttu-id="855d2-101">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="855d2-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="855d2-102">上述命令會新增：</span><span class="sxs-lookup"><span data-stu-id="855d2-102">The preceding commands add:</span></span>

* <span data-ttu-id="855d2-103">[Aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)的「架構」工具。</span><span class="sxs-lookup"><span data-stu-id="855d2-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="855d2-104">.NET Core CLI 的 Entity Framework Core 工具。</span><span class="sxs-lookup"><span data-stu-id="855d2-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="855d2-105">EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。</span><span class="sxs-lookup"><span data-stu-id="855d2-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="855d2-106">Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。</span><span class="sxs-lookup"><span data-stu-id="855d2-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>
