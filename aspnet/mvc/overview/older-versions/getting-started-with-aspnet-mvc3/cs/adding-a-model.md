---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: "加入模型 (C#) |Microsoft 文件"
author: Rick-Anderson
description: "注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 32f2fcdae9d92b84dcd9be8746e416cdf9a14c48
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-c"></a><span data-ttu-id="3cfcd-104">加入模型 (C#)</span><span class="sxs-lookup"><span data-stu-id="3cfcd-104">Adding a Model (C#)</span></span>
====================
<span data-ttu-id="3cfcd-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3cfcd-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3cfcd-106">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="3cfcd-107">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="3cfcd-108">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="3cfcd-109">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="3cfcd-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="3cfcd-110">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="3cfcd-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="3cfcd-111">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="3cfcd-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="3cfcd-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="3cfcd-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="3cfcd-113">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="3cfcd-114">使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="3cfcd-115">[下載的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="3cfcd-116">如果您偏好 Visual Basic，切換至[Visual Basic 版本](../vb/adding-a-model.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="3cfcd-117">加入模型</span><span class="sxs-lookup"><span data-stu-id="3cfcd-117">Adding a Model</span></span>

<span data-ttu-id="3cfcd-118">本節中您要加入一些類別，來管理資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="3cfcd-119">這些類別會在 ASP.NET MVC 應用程式的 「 模型 」 一部分。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="3cfcd-120">若要定義及使用這些模型類別，您將使用稱為 Entity Framework 的.NET Framework 資料存取技術。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="3cfcd-121">呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="3cfcd-122">程式碼第一次可讓您撰寫簡單的類別來建立模型物件。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="3cfcd-123">（也稱為是 POCO 類別，從 「 純舊 CLR 物件 」。）然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="3cfcd-124">加入模型類別</span><span class="sxs-lookup"><span data-stu-id="3cfcd-124">Adding Model Classes</span></span>

<span data-ttu-id="3cfcd-125">在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="3cfcd-126">名稱*類別*"影片"。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="3cfcd-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="3cfcd-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="3cfcd-128">加入下列五個屬性，以`Movie`類別：</span><span class="sxs-lookup"><span data-stu-id="3cfcd-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="3cfcd-129">我們將使用`Movie`類別來代表在資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="3cfcd-130">每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="3cfcd-131">在同一個檔案中，加入下列`MovieDBContext`類別：</span><span class="sxs-lookup"><span data-stu-id="3cfcd-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="3cfcd-132">`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="3cfcd-133">`MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="3cfcd-134">如需有關`DbContext`和`DbSet`，請參閱[Entity Framework 的產能改善功能](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="3cfcd-135">若要能夠參考`DbContext`和`DbSet`，您需要新增下列`using`在檔案最上方的陳述式：</span><span class="sxs-lookup"><span data-stu-id="3cfcd-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="3cfcd-136">完整*Movie.cs*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="3cfcd-137">建立的連接字串和使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="3cfcd-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="3cfcd-138">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3cfcd-139">您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="3cfcd-140">您將會執行，藉由新增中的連接資訊*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="3cfcd-141">開啟應用程式根目錄*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="3cfcd-142">(不*Web.config*檔案*檢視*資料夾。)下圖顯示兩者*Web.config*檔案; 開啟*Web.config*以紅色圈出的檔案。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

### 

<span data-ttu-id="3cfcd-143">加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="3cfcd-144">下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：</span><span class="sxs-lookup"><span data-stu-id="3cfcd-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="3cfcd-145">少量的程式碼和 XML 是您需要撰寫以代表及電影資料儲存在資料庫中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="3cfcd-146">接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="3cfcd-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3cfcd-147">[上一頁](adding-a-view.md)
[下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="3cfcd-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
