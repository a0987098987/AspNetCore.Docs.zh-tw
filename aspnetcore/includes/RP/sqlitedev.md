### <a name="use-sqlite-for-development-sql-server-for-production"></a>使用 SQLite 進行開發,SQL 伺服器進行生產

選擇 SQLite 後,範本生成的代碼已準備就緒,可以進行開發。 以下代碼演示如何注入<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>啟動。 `IWebHostEnvironment`注入,以便在`ConfigureServices`開發和生產中使用 SQL Server。

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
