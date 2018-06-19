---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: 建立的連接字串和使用 SQL Server LocalDB |Microsoft 文件
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: edbd46ef8a03670f0cb7527142babe9bd5846c7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867914"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="78fce-102">建立的連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="78fce-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="78fce-103">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="78fce-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="78fce-104">建立的連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="78fce-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="78fce-105">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="78fce-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="78fce-106">您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="78fce-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="78fce-107">您實際上沒有指定要使用哪一個資料庫，Entity Framework 將會預設為使用[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。</span><span class="sxs-lookup"><span data-stu-id="78fce-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="78fce-108">本節中我們會明確地將新增連接字串中的*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="78fce-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="78fce-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="78fce-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="78fce-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb)是輕量版 SQL Server Express Database Engine，視需要啟動並以使用者模式執行。</span><span class="sxs-lookup"><span data-stu-id="78fce-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="78fce-111">LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您能夠使用資料庫，做為 *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="78fce-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="78fce-112">一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="78fce-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="78fce-113">SQL Server Express 不建議用於生產環境 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="78fce-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="78fce-114">LocalDB 特別不應用於生產環境與 web 應用程式因為它不是使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="78fce-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="78fce-115">不過，您可以輕鬆地到 SQL Server 或 SQL Azure 移轉 LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="78fce-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="78fce-116">在 Visual Studio 2017，LocalDB 被安裝預設會隨 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="78fce-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="78fce-117">根據預設，Entity Framework 會尋找名為與物件內容類別相同的連接字串 (`MovieDBContext`這個專案)。</span><span class="sxs-lookup"><span data-stu-id="78fce-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="78fce-118">如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。</span><span class="sxs-lookup"><span data-stu-id="78fce-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="78fce-119">開啟應用程式根目錄*Web.config*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="78fce-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="78fce-120">(不*Web.config*檔案*檢視*資料夾。)</span><span class="sxs-lookup"><span data-stu-id="78fce-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="78fce-121">尋找`<connectionStrings>`項目：</span><span class="sxs-lookup"><span data-stu-id="78fce-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="78fce-122">加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="78fce-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="78fce-123">下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：</span><span class="sxs-lookup"><span data-stu-id="78fce-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="78fce-124">兩個連接字串都很相似。</span><span class="sxs-lookup"><span data-stu-id="78fce-124">The two connection strings are very similar.</span></span> <span data-ttu-id="78fce-125">第一個連接字串名為`DefaultConnection`並用它來控制可以存取應用程式的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="78fce-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="78fce-126">已新增的連接字串會指定名為 LocalDB 資料庫*Movie.mdf*位於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="78fce-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="78fce-127">我們將不會使用成員資格資料庫，在本教學課程，如需成員資格、 驗證和安全性詳細資訊，請參閱我教學課程[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="78fce-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="78fce-128">連接字串的名稱必須符合的名稱[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="78fce-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="78fce-129">您不需要新增`MovieDBContext`連接字串。</span><span class="sxs-lookup"><span data-stu-id="78fce-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="78fce-130">如果您未指定連接字串，Entity Framework 的完整限定名稱的使用者目錄中會建立 LocalDB 資料庫[DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)類別 (在此情況下`MvcMovie.Models.MovieDBContext`)。</span><span class="sxs-lookup"><span data-stu-id="78fce-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="78fce-131">您可以為資料庫命名任何想要的話，因為它具有 *。MDF*後置詞。</span><span class="sxs-lookup"><span data-stu-id="78fce-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="78fce-132">例如，我們無法為資料庫命名*MyFilms.mdf*。</span><span class="sxs-lookup"><span data-stu-id="78fce-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="78fce-133">接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="78fce-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="78fce-134">[上一頁](adding-a-model.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="78fce-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
