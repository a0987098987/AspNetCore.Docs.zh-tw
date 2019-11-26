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

* [Aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)的「架構」工具。
* .NET Core CLI 的 Entity Framework Core 工具。
* EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。
* Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。

如需允許應用程式依環境設定其資料庫內容的多個環境設定的指引，請參閱 <xref:fundamentals/environments#environment-based-startup-class-and-methods>。
