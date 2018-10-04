---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: 將模型新增 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 5308d2d1d11f954db8a4502adb42223f69e0c675
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578499"
---
<a name="adding-a-model"></a><span data-ttu-id="459d2-104">新增模型</span><span class="sxs-lookup"><span data-stu-id="459d2-104">Adding a Model</span></span>
====================
<span data-ttu-id="459d2-105">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="459d2-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="459d2-106">本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="459d2-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="459d2-107">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="459d2-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="459d2-108">在本節中，您將新增一些類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="459d2-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="459d2-109">這些類別是&quot;模型&quot;ASP.NET MVC 應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="459d2-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="459d2-110">您將使用的.NET Framework 資料存取技術，稱為[Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)來定義和使用這些模型類別。</span><span class="sxs-lookup"><span data-stu-id="459d2-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="459d2-111">Entity Framework （通常稱為 EF） 支援，開發架構稱為*Code First*。</span><span class="sxs-lookup"><span data-stu-id="459d2-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="459d2-112">程式碼第一次可讓您撰寫簡單的類別來建立模型物件。</span><span class="sxs-lookup"><span data-stu-id="459d2-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="459d2-113">(這些是也稱為 POCO 類別，來自&quot;純舊 CLR 物件。&quot;)然後您可以在即時從您的類別，可讓非常精簡且快速的開發工作流程所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="459d2-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="459d2-114">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="459d2-114">Adding Model Classes</span></span>

<span data-ttu-id="459d2-115">在 [**方案總管] 中**，以滑鼠右鍵按一下 *模型* 資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="459d2-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="459d2-116">請輸入*類別*名稱&quot;電影&quot;。</span><span class="sxs-lookup"><span data-stu-id="459d2-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="459d2-117">加入下列五個屬性，以`Movie`類別：</span><span class="sxs-lookup"><span data-stu-id="459d2-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="459d2-118">我們將使用`Movie`類別來代表資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="459d2-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="459d2-119">每個執行個體`Movie`物件會對應至資料庫資料表，以及每個屬性中的資料列`Movie`類別會對應至資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="459d2-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="459d2-120">在相同的檔案中，新增下列`MovieDBContext`類別：</span><span class="sxs-lookup"><span data-stu-id="459d2-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="459d2-121">`MovieDBContext`類別代表會處理擷取、 儲存及更新的 Entity Framework 電影資料庫內容`Movie`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="459d2-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="459d2-122">`MovieDBContext`衍生自`DbContext`基底 Entity Framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="459d2-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="459d2-123">為了能夠參考`DbContext`並`DbSet`，您需要新增下列`using`陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="459d2-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="459d2-124">完整*Movie.cs*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="459d2-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="459d2-125">(數個 using 陳述式不需要已移除。)</span><span class="sxs-lookup"><span data-stu-id="459d2-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="459d2-126">建立連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="459d2-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="459d2-127">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="459d2-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="459d2-128">您可能會問的一個問題，是如何指定要連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="459d2-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="459d2-129">您將的做法是將連接資訊*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="459d2-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="459d2-130">開啟應用程式根目錄*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="459d2-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="459d2-131">(沒有*Web.config*中的檔案*檢視*資料夾。)開啟*Web.config*紅色外框的檔案。</span><span class="sxs-lookup"><span data-stu-id="459d2-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="459d2-132">加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="459d2-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="459d2-133">下列範例示範的一部分*Web.config*檔案以加入新的連接字串：</span><span class="sxs-lookup"><span data-stu-id="459d2-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="459d2-134">此少量的程式碼和 XML 是您要撰寫以代表，並將電影資料儲存在資料庫中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="459d2-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="459d2-135">接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="459d2-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="459d2-136">[上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="459d2-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
