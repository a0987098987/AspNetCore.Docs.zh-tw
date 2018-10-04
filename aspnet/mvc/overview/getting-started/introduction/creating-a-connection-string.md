---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 建立連接字串和使用 SQL Server LocalDB |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 746101344832793b199d2b3f3dfcfcd4e3b9a8da
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578519"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d8616-102">建立連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d8616-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="d8616-103">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="d8616-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="d8616-104">建立連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d8616-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d8616-105">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="d8616-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d8616-106">您可能會問的一個問題，是如何指定要連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="d8616-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="d8616-107">您實際上沒有指定要使用哪一個資料庫，Entity Framework 會使用預設[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。</span><span class="sxs-lookup"><span data-stu-id="d8616-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="d8616-108">在這一節我們將明確新增中的連接字串*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="d8616-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d8616-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d8616-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d8616-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)是輕量版的 SQL Server Express Database Engine 會視需要啟動，並以使用者模式執行。</span><span class="sxs-lookup"><span data-stu-id="d8616-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="d8616-111">LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8616-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="d8616-112">一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d8616-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="d8616-113">不建議在生產環境 web 應用程式中使用 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="d8616-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="d8616-114">LocalDB 尤其不應用於生產環境的 web 應用程式因為它不是在搭配 IIS 運作。</span><span class="sxs-lookup"><span data-stu-id="d8616-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="d8616-115">不過，LocalDB 資料庫可以輕鬆地移轉至 SQL Server 或 SQL Azure。</span><span class="sxs-lookup"><span data-stu-id="d8616-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="d8616-116">在 Visual Studio 2017 中，使用 Visual Studio 的預設會安裝 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="d8616-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="d8616-117">根據預設，Entity Framework 會尋找名為物件內容類別相同的連接字串 (`MovieDBContext`這個專案)。</span><span class="sxs-lookup"><span data-stu-id="d8616-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="d8616-118">如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d8616-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="d8616-119">開啟應用程式根目錄*Web.config*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="d8616-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="d8616-120">(沒有*Web.config*中的檔案*檢視*資料夾。)</span><span class="sxs-lookup"><span data-stu-id="d8616-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="d8616-121">尋找`<connectionStrings>`項目：</span><span class="sxs-lookup"><span data-stu-id="d8616-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="d8616-122">加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d8616-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="d8616-123">下列範例示範的一部分*Web.config*檔案以加入新的連接字串：</span><span class="sxs-lookup"><span data-stu-id="d8616-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="d8616-124">兩個連接字串都很相似。</span><span class="sxs-lookup"><span data-stu-id="d8616-124">The two connection strings are very similar.</span></span> <span data-ttu-id="d8616-125">第一個連接字串名為`DefaultConnection`並用它來控制可以存取應用程式的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="d8616-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="d8616-126">您已新增連接字串會指定名為 LocalDB 資料庫*Movie.mdf*位於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d8616-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="d8616-127">我們不會使用成員資格資料庫中，請在本教學課程，如需成員資格、 驗證和安全性的詳細資訊，請參閱我的教學課程[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="d8616-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="d8616-128">連接字串的名稱必須符合的名稱[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="d8616-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="d8616-129">您實際上不需要新增`MovieDBContext`連接字串。</span><span class="sxs-lookup"><span data-stu-id="d8616-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="d8616-130">如果您未指定連接字串，Entity Framework 將 LocalDB 資料庫的目錄中建立使用者的完整名稱[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別 (在此情況下`MvcMovie.Models.MovieDBContext`)。</span><span class="sxs-lookup"><span data-stu-id="d8616-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="d8616-131">您可以為資料庫命名任何想要的話，只要它擁有 *。MDF*後置詞。</span><span class="sxs-lookup"><span data-stu-id="d8616-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="d8616-132">比方說，我們無法為資料庫命名*MyFilms.mdf*。</span><span class="sxs-lookup"><span data-stu-id="d8616-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="d8616-133">接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="d8616-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8616-134">[上一頁](adding-a-model.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="d8616-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
