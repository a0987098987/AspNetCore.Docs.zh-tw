<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="06966-101">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="06966-101">Add a database connection string</span></span>

<span data-ttu-id="06966-102">將連線字串新增到 *appsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="06966-102">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="06966-103">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="06966-103">[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="06966-104">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="06966-104">Register the database context</span></span>

<span data-ttu-id="06966-105">以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="06966-105">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>