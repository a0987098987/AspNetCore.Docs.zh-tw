---
title: 使用 SQL Server LocalDB 與 ASP.NET Core
author: rick-anderson
description: 說明如何使用 SQL Server LocalDB 與 ASP.NET Core。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 255faf12064aa424d51fb6faa801884c474bd288
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889479"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="86184-103">使用 SQL Server LocalDB 與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86184-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="86184-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="86184-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="86184-105">`MovieContext` 物件會處理連線到資料庫和將 `Movie` 物件對應至資料庫記錄的工作。</span><span class="sxs-lookup"><span data-stu-id="86184-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="86184-106">在 *Startup.cs* 檔案的 `ConfigureServices` 方法中，以[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="86184-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="86184-107">如需 `ConfigureServices` 中所用之方法的詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="86184-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="86184-108">適用於 `CookiePolicyOptions` 的 [ASP.NET Core 中的 EU 一般資料保護規定 (GDPR) 支援](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="86184-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="86184-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="86184-109">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="86184-110">ASP.NET Core [組態](xref:fundamentals/configuration/index)系統會讀取 `ConnectionString`。</span><span class="sxs-lookup"><span data-stu-id="86184-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="86184-111">對於本機開發，它會從 *appsettings.json* 檔案取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="86184-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="86184-112">您的產生程式碼會有不同的資料庫 (`Database={Database name}`) 名稱值。</span><span class="sxs-lookup"><span data-stu-id="86184-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="86184-113">名稱值為任意值。</span><span class="sxs-lookup"><span data-stu-id="86184-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="86184-114">當您將應用程式部署到測試或生產環境伺服器時，可以使用環境變數或另一個方法來設定實際 SQL Server 的連接字串。</span><span class="sxs-lookup"><span data-stu-id="86184-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="86184-115">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="86184-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="86184-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="86184-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="86184-117">LocalDB 為輕量版的 SQL Server Express Database Engine，鎖定程式開發為其目標。</span><span class="sxs-lookup"><span data-stu-id="86184-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="86184-118">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="86184-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="86184-119">根據預設，LocalDB 資料庫會在 *C:/Users/\<使用者\>* 目錄中建立 "\*.mdf" 檔案。</span><span class="sxs-lookup"><span data-stu-id="86184-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="86184-120">從 [檢視] 功能表中，開啟 [SQL Server 物件總管] (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="86184-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![檢視功能表](sql/_static/ssox.png)

* <span data-ttu-id="86184-122">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視表設計工具]：</span><span class="sxs-lookup"><span data-stu-id="86184-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![在電影資料表上開啟操作功能表](sql/_static/design.png)

  ![在設計工具中開啟電影資料表](sql/_static/dv.png)

<span data-ttu-id="86184-125">請注意 `ID` 旁的索引鍵圖示。</span><span class="sxs-lookup"><span data-stu-id="86184-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="86184-126">根據預設，EF 會為主索引鍵建立名為 `ID` 的屬性。</span><span class="sxs-lookup"><span data-stu-id="86184-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="86184-127">以滑鼠右鍵按一下 `Movie` 資料表，並選取 [檢視資料]：</span><span class="sxs-lookup"><span data-stu-id="86184-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開啟的電影資料表顯示資料表資料](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="86184-129">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="86184-129">Seed the database</span></span>

<span data-ttu-id="86184-130">在 *Models* 資料夾中建立名為 `SeedData` 的新類別。</span><span class="sxs-lookup"><span data-stu-id="86184-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="86184-131">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="86184-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="86184-132">如果資料庫中有任何電影，則種子初始設定式會返回，而且不會新增任何電影。</span><span class="sxs-lookup"><span data-stu-id="86184-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="86184-133">新增種子初始設定式</span><span class="sxs-lookup"><span data-stu-id="86184-133">Add the seed initializer</span></span>

<span data-ttu-id="86184-134">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="86184-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="86184-135">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="86184-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="86184-136">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="86184-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="86184-137">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="86184-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="86184-138">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="86184-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="86184-139">生產環境應用程式不會呼叫 `Database.Migrate`。</span><span class="sxs-lookup"><span data-stu-id="86184-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="86184-140">它會新增至前面的程式碼以免在尚未執行 `Update-Database` 時發生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="86184-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="86184-141">SqlException：無法開啟登入要求的 "RazorPagesMovieContext-21" 資料庫。</span><span class="sxs-lookup"><span data-stu-id="86184-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="86184-142">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="86184-142">The login failed.</span></span>
<span data-ttu-id="86184-143">使用者 'user name' 登入失敗。</span><span class="sxs-lookup"><span data-stu-id="86184-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="86184-144">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="86184-144">Test the app</span></span>

* <span data-ttu-id="86184-145">刪除資料庫中的所有記錄。</span><span class="sxs-lookup"><span data-stu-id="86184-145">Delete all the records in the DB.</span></span> <span data-ttu-id="86184-146">您可以使用瀏覽器或 [SSOX](xref:tutorials/razor-pages/new-field#ssox) 的刪除連結來執行這項操作</span><span class="sxs-lookup"><span data-stu-id="86184-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="86184-147">強制應用程式初始化 (呼叫 `Startup` 類別中的方法)，以執行植入方法。</span><span class="sxs-lookup"><span data-stu-id="86184-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="86184-148">若要強制初始化，IIS Express 必須停止並重新啟動。</span><span class="sxs-lookup"><span data-stu-id="86184-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="86184-149">您可以使用下列其中一個方法來執行此工作：</span><span class="sxs-lookup"><span data-stu-id="86184-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="86184-150">以滑鼠右鍵按一下通知區域中的 IIS Express 系統匣圖示，然後點選 [結束] 或 [停止網站]：</span><span class="sxs-lookup"><span data-stu-id="86184-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express 系統匣圖示](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![操作功能表](sql/_static/stopIIS.png)

    * <span data-ttu-id="86184-153">如果您已在非偵錯模式中執行 VS，請按 F5 以偵錯模式執行。</span><span class="sxs-lookup"><span data-stu-id="86184-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="86184-154">如果您已在偵錯模式中執行 VS，請停止偵錯工具，然後按 F5。</span><span class="sxs-lookup"><span data-stu-id="86184-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="86184-155">應用程式會顯示植入的資料：</span><span class="sxs-lookup"><span data-stu-id="86184-155">The app shows the seeded data:</span></span>

![在 Chrome 中開啟的電影應用程式顯示電影資料](sql/_static/m55.png)

<span data-ttu-id="86184-157">接下來的教學課程將會清除資料的呈現。</span><span class="sxs-lookup"><span data-stu-id="86184-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86184-158">[上一步：包含 Scaffold 的 Razor Pages](xref:tutorials/razor-pages/page)
> [下一步：更新頁面](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="86184-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
