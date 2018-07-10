# <a name="work-with-sqlite-in-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="59e7d-101">在 ASP.NET Core Razor Pages 應用程式中使用 SQLit</span><span class="sxs-lookup"><span data-stu-id="59e7d-101">Work with SQLite in an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="59e7d-102">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="59e7d-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="59e7d-103">`MovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="59e7d-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="59e7d-104">在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="59e7d-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="59e7d-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="59e7d-105">SQLite</span></span>

<span data-ttu-id="59e7d-106">[SQLite](https://www.sqlite.org/) 網站指出：</span><span class="sxs-lookup"><span data-stu-id="59e7d-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="59e7d-107">SQLite 是獨立、高可靠性、內嵌式、全功能、公用領域的 SQL 資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="59e7d-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="59e7d-108">SQLite 是世界上最常使用的資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="59e7d-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="59e7d-109">您可以下載許多協力廠商工具來管理和檢視 SQLite 資料庫。</span><span class="sxs-lookup"><span data-stu-id="59e7d-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="59e7d-110">下圖是來自 [SQLite 的資料庫瀏覽器](http://sqlitebrowser.org/)。</span><span class="sxs-lookup"><span data-stu-id="59e7d-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="59e7d-111">如果有慣用的 SQLite 工具，請留下您喜歡哪一點的評論。</span><span class="sxs-lookup"><span data-stu-id="59e7d-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![顯示電影資料庫的 SQLite 的資料庫瀏覽器](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="59e7d-113">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="59e7d-113">Seed the database</span></span>

<span data-ttu-id="59e7d-114">在 *Models* 資料夾中建立名為 `SeedData` 的新類別。</span><span class="sxs-lookup"><span data-stu-id="59e7d-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="59e7d-115">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="59e7d-115">Replace the generated code with the following:</span></span>

[!code-csharp[](code/Models/SeedData.cs)]

<span data-ttu-id="59e7d-116">如果資料庫中有任何電影，則種子初始設定式會返回。</span><span class="sxs-lookup"><span data-stu-id="59e7d-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="59e7d-117">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="59e7d-117">Add the seed initializer</span></span>

<span data-ttu-id="59e7d-118">在 *Program.cs* 檔案中，將種子初始設定式新增至 `Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="59e7d-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Program.cs)]

### <a name="test-the-app"></a><span data-ttu-id="59e7d-119">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="59e7d-119">Test the app</span></span>

<span data-ttu-id="59e7d-120">請刪除資料庫中的所有記錄 (這樣會執行 seed 方法)。</span><span class="sxs-lookup"><span data-stu-id="59e7d-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="59e7d-121">停止並啟動應用程式來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="59e7d-121">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="59e7d-122">應用程式會顯示植入的資料。</span><span class="sxs-lookup"><span data-stu-id="59e7d-122">The app shows the seeded data.</span></span>