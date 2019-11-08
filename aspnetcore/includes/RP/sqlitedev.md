### <a name="use-sqlite-for-development-sql-server-for-production"></a>使用 SQLite 進行開發，SQL Server 用於生產環境

選取 SQLite 時，範本產生的程式碼就已準備好進行開發。 下列程式碼示範如何將 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 插入啟動。 插入 `IWebHostEnvironment`，讓 `ConfigureServices` 可以在開發中使用 SQLite，並在生產環境中 SQL Server。

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
