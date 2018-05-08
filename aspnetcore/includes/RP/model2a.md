<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>新增資料庫連線字串

將連線字串新增到 *appsettings.json* 檔案中。

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>登錄資料庫內容

以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。