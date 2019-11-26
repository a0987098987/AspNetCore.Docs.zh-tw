執行下列 .NET Core CLI 命令：

```dotnetcli
dotnet tool install --global dotnet-ef
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

上述命令會新增：

* The [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
* The Entity Framework Core Tools for the .NET Core CLI.
* EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。
* Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。

For guidance on multiple environment configuration that permits an app to configure its database contexts by environment, see <xref:fundamentals/environments#environment-based-startup-class-and-methods>.
