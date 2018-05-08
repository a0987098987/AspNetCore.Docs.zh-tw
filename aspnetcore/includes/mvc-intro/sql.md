# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a>在 ASP.NET Core MVC 專案中使用 SQLit

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。 在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：

[!code-csharp[](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

[SQLite](https://www.sqlite.org/) 網站指出：

> SQLite 是獨立、高可靠性、內嵌式、全功能、公用領域的 SQL 資料庫引擎。 SQLite 是世界上最常使用的資料庫引擎。

您可以下載許多協力廠商工具來管理和檢視 SQLite 資料庫。 下圖是來自 [SQLite 的資料庫瀏覽器](http://sqlitebrowser.org/)。 如果有慣用的 SQLite 工具，請留下您喜歡哪一點的評論。

![顯示電影資料庫的 SQLite 的資料庫瀏覽器](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>植入資料庫

在 *Models* 資料夾中建立名為 `SeedData` 的新類別。 使用下列程式碼取代產生的程式碼：

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

如果資料庫中有任何電影，則種子初始設定式會返回。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>新增種子初始設定式

在 *Program.cs* 檔案中，將種子初始設定式新增至 `Main` 方法：

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a>測試應用程式

請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。 停止並啟動應用程式來植入資料庫。
   
應用程式會顯示植入的資料。

![在瀏覽器中開啟的 MVC 電影應用程式顯示電影資料](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
