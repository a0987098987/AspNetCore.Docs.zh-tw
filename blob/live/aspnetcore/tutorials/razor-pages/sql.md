---
title: "使用 SQL Server LocalDB 與 ASP.NET Core"
author: rick-anderson
description: "說明如何使用 SQL Server LocalDB 與 ASP.NET Core。"
keywords: "ASP.NET Core, Razor 頁面, Razor,MVC, SQL, LocalDB"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1e35ce49980bf026de35359cdf413961051e3bee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="361b0-104">使用 SQL Server LocalDB 與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="361b0-104">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="361b0-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="361b0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="361b0-106">`MovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="361b0-106">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="361b0-107">在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="361b0-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

<span data-ttu-id="361b0-108">ASP.NET Core [組態](xref:fundamentals/configuration)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="361b0-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="361b0-109">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串：</span><span class="sxs-lookup"><span data-stu-id="361b0-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="361b0-110">當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="361b0-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="361b0-111">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="361b0-111">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="361b0-112">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="361b0-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="361b0-113">LocalDB 是輕量版的 SQL Server Express Database Engine，以程式開發為目標。</span><span class="sxs-lookup"><span data-stu-id="361b0-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="361b0-114">LocalDB 會視需要啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="361b0-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="361b0-115">根據預設，LocalDB 資料庫會在 *C:/Users/*\<使用者\> 目錄中建立 "\*.mdf" 檔案。</span><span class="sxs-lookup"><span data-stu-id="361b0-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="361b0-116">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="361b0-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="361b0-118">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：</span><span class="sxs-lookup"><span data-stu-id="361b0-118">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

<span data-ttu-id="361b0-121">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="361b0-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="361b0-122">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="361b0-122">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="361b0-123">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：</span><span class="sxs-lookup"><span data-stu-id="361b0-123">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="361b0-125">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="361b0-125">Seed the database</span></span>

<span data-ttu-id="361b0-126">在 *Models* 資料夾中建立名為 `SeedData` 的新類別。</span><span class="sxs-lookup"><span data-stu-id="361b0-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="361b0-127">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="361b0-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="361b0-128">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="361b0-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="361b0-129">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="361b0-129">Add the seed initializer</span></span>

<span data-ttu-id="361b0-130">在 *Program.cs* 檔案中，將種子初始設定式新增至 `Main` 方法的結尾處：</span><span class="sxs-lookup"><span data-stu-id="361b0-130">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="361b0-131">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="361b0-131">Test the app</span></span>

* <span data-ttu-id="361b0-132">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="361b0-132">Delete all the records in the DB.</span></span> <span data-ttu-id="361b0-133">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="361b0-133">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="361b0-134">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="361b0-134">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="361b0-135">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="361b0-135">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="361b0-136">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="361b0-136">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="361b0-137">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]：</span><span class="sxs-lookup"><span data-stu-id="361b0-137">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

   * <span data-ttu-id="361b0-140">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="361b0-140">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="361b0-141">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="361b0-141">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="361b0-142">應用程式會顯示植入的資料：</span><span class="sxs-lookup"><span data-stu-id="361b0-142">The app shows the seeded data:</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

<span data-ttu-id="361b0-144">接下來的教學課程將會清除資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="361b0-144">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="361b0-145">[上一步：包含 Scaffold 的 Razor 頁面](xref:tutorials/razor-pages/page)
[下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="361b0-145">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
