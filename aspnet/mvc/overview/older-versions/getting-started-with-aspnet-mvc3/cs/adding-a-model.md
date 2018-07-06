---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: 新增模型 (C#) |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 61ba86e4bfbb0557b34d245555bc56a320335628
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820768"
---
<a name="adding-a-model-c"></a><span data-ttu-id="25325-104">新增模型 (C#)</span><span class="sxs-lookup"><span data-stu-id="25325-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="25325-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="25325-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="25325-106">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="25325-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="25325-107">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="25325-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="25325-108">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="25325-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="25325-109">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="25325-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="25325-110">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="25325-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="25325-111">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="25325-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="25325-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="25325-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="25325-113">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="25325-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="25325-114">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="25325-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="25325-115">[下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="25325-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="25325-116">如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/adding-a-model.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="25325-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="25325-117">新增模型</span><span class="sxs-lookup"><span data-stu-id="25325-117">Adding a Model</span></span>

<span data-ttu-id="25325-118">在本節中，您將新增一些類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="25325-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="25325-119">這些類別是 ASP.NET MVC 應用程式的 「 模型 」 部分。</span><span class="sxs-lookup"><span data-stu-id="25325-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="25325-120">您將使用稱為 Entity Framework 的.NET Framework 資料存取技術，來定義和使用這些模型類別。</span><span class="sxs-lookup"><span data-stu-id="25325-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="25325-121">Entity Framework （通常稱為 EF） 支援，開發架構稱為*Code First*。</span><span class="sxs-lookup"><span data-stu-id="25325-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="25325-122">程式碼第一次可讓您撰寫簡單的類別來建立模型物件。</span><span class="sxs-lookup"><span data-stu-id="25325-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="25325-123">（這些也稱為是 POCO 類別，來自 「 純舊 CLR 物件 」。）然後您可以在即時從您的類別，可讓非常精簡且快速的開發工作流程所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="25325-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="25325-124">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="25325-124">Adding Model Classes</span></span>

<span data-ttu-id="25325-125">在 [**方案總管] 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="25325-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="25325-126">名稱*類別*「 電影 」。</span><span class="sxs-lookup"><span data-stu-id="25325-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="25325-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="25325-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="25325-128">加入下列五個屬性，以`Movie`類別：</span><span class="sxs-lookup"><span data-stu-id="25325-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="25325-129">我們將使用`Movie`類別來代表資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="25325-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="25325-130">每個執行個體`Movie`物件會對應至資料庫資料表，以及每個屬性中的資料列`Movie`類別會對應至資料表的資料行。</span><span class="sxs-lookup"><span data-stu-id="25325-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="25325-131">在相同的檔案中，新增下列`MovieDBContext`類別：</span><span class="sxs-lookup"><span data-stu-id="25325-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="25325-132">`MovieDBContext`類別代表會處理擷取、 儲存及更新的 Entity Framework 電影資料庫內容`Movie`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="25325-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="25325-133">`MovieDBContext`衍生自`DbContext`基底 Entity Framework 所提供的類別。</span><span class="sxs-lookup"><span data-stu-id="25325-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="25325-134">如需詳細資訊`DbContext`並`DbSet`，請參閱[Entity Framework 的產能改進](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)。</span><span class="sxs-lookup"><span data-stu-id="25325-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="25325-135">為了能夠參考`DbContext`並`DbSet`，您需要新增下列`using`陳述式在檔案頂端：</span><span class="sxs-lookup"><span data-stu-id="25325-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="25325-136">完整*Movie.cs*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="25325-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="25325-137">建立連接字串和使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="25325-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="25325-138">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="25325-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="25325-139">您可能會問的一個問題，是如何指定要連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="25325-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="25325-140">您將的做法是將連接資訊*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="25325-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="25325-141">開啟應用程式根目錄*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="25325-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="25325-142">(沒有*Web.config*中的檔案*檢視*資料夾。)下圖顯示兩者*Web.config*檔案; 開啟*Web.config*以紅色圈起的檔案。</span><span class="sxs-lookup"><span data-stu-id="25325-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="25325-143">加入下列連接字串`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="25325-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="25325-144">下列範例示範的一部分*Web.config*檔案以加入新的連接字串：</span><span class="sxs-lookup"><span data-stu-id="25325-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="25325-145">此少量的程式碼和 XML 是您要撰寫以代表，並將電影資料儲存在資料庫中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="25325-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="25325-146">接下來，您將建立新`MoviesController`類別可用來顯示電影資料，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="25325-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="25325-147">[上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="25325-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
