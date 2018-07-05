---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: 使用 Code First 移轉來植入資料庫 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 0d753ea52c57af2cbffff9e1e8741bbe49bc6d7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370522"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="b8904-102">使用 Code First 移轉來植入資料庫</span><span class="sxs-lookup"><span data-stu-id="b8904-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="b8904-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b8904-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b8904-104">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="b8904-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b8904-105">在本節中，您將使用[Code First 移轉](https://msdn.microsoft.com/data/jj591621)EF 來植入測試資料的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="b8904-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="b8904-106">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="b8904-106">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b8904-107">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8904-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="b8904-108">此命令會新增至您的專案，命名移轉資料夾加上名為 Configuration.cs Migrations 資料夾中的程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="b8904-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="b8904-109">開啟 Configuration.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="b8904-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="b8904-110">新增下列**使用**陳述式。</span><span class="sxs-lookup"><span data-stu-id="b8904-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="b8904-111">然後新增下列程式碼**Configuration.Seed**方法：</span><span class="sxs-lookup"><span data-stu-id="b8904-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="b8904-112">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b8904-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="b8904-113">第一個命令會產生程式碼會建立資料庫，和第二個命令會執行該程式碼。</span><span class="sxs-lookup"><span data-stu-id="b8904-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="b8904-114">資料庫使用本機上，建立[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b8904-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="b8904-115">瀏覽 （選擇性） 的 API</span><span class="sxs-lookup"><span data-stu-id="b8904-115">Explore the API (Optional)</span></span>

<span data-ttu-id="b8904-116">按 F5 以偵錯模式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8904-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="b8904-117">Visual Studio 會啟動 IIS Express，並執行您的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8904-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="b8904-118">Visual Studio 接著會啟動瀏覽器，並開啟應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="b8904-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="b8904-119">Visual Studio 執行時 web 專案，它會指派通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="b8904-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="b8904-120">下圖中的連接埠號碼會是 50524。</span><span class="sxs-lookup"><span data-stu-id="b8904-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="b8904-121">當您執行應用程式時，您會看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="b8904-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="b8904-122">使用 ASP.NET MVC 實作首頁。</span><span class="sxs-lookup"><span data-stu-id="b8904-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="b8904-123">在頁面頂端，沒有一個連結會顯示 「 API 」。</span><span class="sxs-lookup"><span data-stu-id="b8904-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="b8904-124">此連結將帶您到自動產生的說明頁面，web API。</span><span class="sxs-lookup"><span data-stu-id="b8904-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="b8904-125">(若要了解如何產生此說明頁面，以及如何將您自己的文件，加入頁面，請參閱[建立 ASP.NET Web api 說明 頁面](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)若要查看詳細資，包括要求和回應格式的頁面連結，您可以按一下 [說明] 中。</span><span class="sxs-lookup"><span data-stu-id="b8904-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="b8904-126">此 API 可讓資料庫上的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="b8904-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="b8904-127">以下摘要說明 API。</span><span class="sxs-lookup"><span data-stu-id="b8904-127">The following summarizes the API.</span></span>

| <span data-ttu-id="b8904-128">作者</span><span class="sxs-lookup"><span data-stu-id="b8904-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="b8904-129">取得 api/作者</span><span class="sxs-lookup"><span data-stu-id="b8904-129">GET api/authors</span></span> | <span data-ttu-id="b8904-130">取得所有的作者。</span><span class="sxs-lookup"><span data-stu-id="b8904-130">Get all authors.</span></span> |
| <span data-ttu-id="b8904-131">取得 api/作者 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-131">GET api/authors/{id}</span></span> | <span data-ttu-id="b8904-132">取得作者的識別碼。</span><span class="sxs-lookup"><span data-stu-id="b8904-132">Get an author by ID.</span></span> |
| <span data-ttu-id="b8904-133">POST/api/作者</span><span class="sxs-lookup"><span data-stu-id="b8904-133">POST /api/authors</span></span> | <span data-ttu-id="b8904-134">建立新的作者。</span><span class="sxs-lookup"><span data-stu-id="b8904-134">Create a new author.</span></span> |
| <span data-ttu-id="b8904-135">PUT /api/作者 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="b8904-136">更新現有的作者。</span><span class="sxs-lookup"><span data-stu-id="b8904-136">Update an existing author.</span></span> |
| <span data-ttu-id="b8904-137">刪除 /api/作者 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="b8904-138">刪除的作者。</span><span class="sxs-lookup"><span data-stu-id="b8904-138">Delete an author.</span></span> |

| <span data-ttu-id="b8904-139">書籍</span><span class="sxs-lookup"><span data-stu-id="b8904-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="b8904-140">取得 /api/books</span><span class="sxs-lookup"><span data-stu-id="b8904-140">GET /api/books</span></span> | <span data-ttu-id="b8904-141">取得所有書籍。</span><span class="sxs-lookup"><span data-stu-id="b8904-141">Get all books.</span></span> |
| <span data-ttu-id="b8904-142">取得 /api/叢書 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-142">GET /api/books/{id}</span></span> | <span data-ttu-id="b8904-143">取得活頁簿的識別碼。</span><span class="sxs-lookup"><span data-stu-id="b8904-143">Get a book by ID.</span></span> |
| <span data-ttu-id="b8904-144">張貼/api/書籍</span><span class="sxs-lookup"><span data-stu-id="b8904-144">POST /api/books</span></span> | <span data-ttu-id="b8904-145">建立新的書籍。</span><span class="sxs-lookup"><span data-stu-id="b8904-145">Create a new book.</span></span> |
| <span data-ttu-id="b8904-146">PUT /api 叢書 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="b8904-147">更新現有的書籍。</span><span class="sxs-lookup"><span data-stu-id="b8904-147">Update an existing book.</span></span> |
| <span data-ttu-id="b8904-148">刪除 /api/叢書 / {id}</span><span class="sxs-lookup"><span data-stu-id="b8904-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="b8904-149">刪除活頁簿。</span><span class="sxs-lookup"><span data-stu-id="b8904-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="b8904-150">檢視資料庫 （選擇性）</span><span class="sxs-lookup"><span data-stu-id="b8904-150">View the Database (Optional)</span></span>

<span data-ttu-id="b8904-151">當您執行 Update-database 命令時，EF 會建立資料庫，並呼叫`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="b8904-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="b8904-152">當您在本機執行應用程式時，會使用 EF [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b8904-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="b8904-153">您可以在 Visual Studio 中檢視的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b8904-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="b8904-154">從**檢視**功能表上，選取**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="b8904-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="b8904-155">在 [**連接到伺服器**] 對話方塊中**伺服器名稱**編輯方塊中，輸入"(localdb) \v11.0"。</span><span class="sxs-lookup"><span data-stu-id="b8904-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="b8904-156">離開**驗證**選項設定為 「 Windows 驗證 」。</span><span class="sxs-lookup"><span data-stu-id="b8904-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="b8904-157">按一下 **[Connect]**(連線)。</span><span class="sxs-lookup"><span data-stu-id="b8904-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="b8904-158">Visual Studio 連接到 LocalDB 和 SQL Server 物件總管 視窗中會顯示您現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b8904-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="b8904-159">您可以展開節點，以查看 EF 所建立的資料表。</span><span class="sxs-lookup"><span data-stu-id="b8904-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="b8904-160">若要檢視資料，以滑鼠右鍵按一下資料表，然後選取**檢視資料**。</span><span class="sxs-lookup"><span data-stu-id="b8904-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="b8904-161">下列螢幕擷取畫面顯示活頁簿資料表的結果。</span><span class="sxs-lookup"><span data-stu-id="b8904-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="b8904-162">請注意，EF 會填入資料庫種子資料，而且資料表包含 Authors 資料表的外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b8904-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b8904-163">[上一頁](part-2.md)
> [下一頁](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="b8904-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
