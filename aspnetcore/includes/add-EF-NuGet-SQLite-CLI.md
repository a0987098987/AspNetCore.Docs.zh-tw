<span data-ttu-id="b4b8f-101">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="b4b8f-101">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="b4b8f-102">上述命令會新增：</span><span class="sxs-lookup"><span data-stu-id="b4b8f-102">The preceding commands add:</span></span>

* <span data-ttu-id="b4b8f-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="b4b8f-103">The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
* <span data-ttu-id="b4b8f-104">The Entity Framework Core Tools for the .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="b4b8f-104">The Entity Framework Core Tools for the .NET Core CLI.</span></span>
* <span data-ttu-id="b4b8f-105">EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。</span><span class="sxs-lookup"><span data-stu-id="b4b8f-105">The EF Core SQLite provider, which installs the EF Core package as a dependency.</span></span>
* <span data-ttu-id="b4b8f-106">Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。</span><span class="sxs-lookup"><span data-stu-id="b4b8f-106">Packages needed for scaffolding: `Microsoft.VisualStudio.Web.CodeGeneration.Design` and `Microsoft.EntityFrameworkCore.SqlServer`.</span></span>

<span data-ttu-id="b4b8f-107">For guidance on multiple environment configuration that permits an app to configure its database contexts by environment, see <xref:fundamentals/environments#environment-based-startup-class-and-methods>.</span><span class="sxs-lookup"><span data-stu-id="b4b8f-107">For guidance on multiple environment configuration that permits an app to configure its database contexts by environment, see <xref:fundamentals/environments#environment-based-startup-class-and-methods>.</span></span>
