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

* [阿斯普內特代碼產生器文手架工具](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。
* .NET 核心 CLI 的實體框架核心工具。
* EF Core SQLite 提供者，該提供者會將 EF Core 套件作為相依性安裝。
* Scaffolding `Microsoft.VisualStudio.Web.CodeGeneration.Design` 和 `Microsoft.EntityFrameworkCore.SqlServer` 需要的套件。

有關允許應用按環境設定其資料庫上下文的多個環境配置的指導,請參閱<xref:fundamentals/environments#environment-based-startup-class-and-methods>。

[!INCLUDE[](~/includes/scaffoldTFM.md)]