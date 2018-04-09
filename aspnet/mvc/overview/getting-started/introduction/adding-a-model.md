---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: 將模型加入 |Microsoft 文件
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a><span data-ttu-id="eb0e6-102">加入模型</span><span class="sxs-lookup"><span data-stu-id="eb0e6-102">Adding a Model</span></span>
====================
<span data-ttu-id="eb0e6-103">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="eb0e6-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="eb0e6-104">本節中您要加入一些類別，來管理資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="eb0e6-105">這些類別會&quot;模型&quot;ASP.NET MVC 應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="eb0e6-106">您將使用的.NET Framework 資料存取技術稱為[Entity Framework](https://docs.microsoft.com/ef/)來定義及使用這些模型類別。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="eb0e6-107">呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="eb0e6-108">程式碼第一次可讓您撰寫簡單的類別來建立模型物件。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="eb0e6-109">(這些是也稱為 POCO 類別，從&quot;純舊 CLR 物件。&quot;)然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="eb0e6-110">如果您需要先建立資料庫時，您仍然可以遵循本教學課程來了解 MVC 和 EF 應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="eb0e6-111">您可以遵循 Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview)教學課程中，其中涵蓋了資料庫的第一種方法。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="eb0e6-112">加入模型類別</span><span class="sxs-lookup"><span data-stu-id="eb0e6-112">Adding Model Classes</span></span>

<span data-ttu-id="eb0e6-113">在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="eb0e6-114">輸入*類別*名稱&quot;影片&quot;。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="eb0e6-115">加入下列五個屬性，以`Movie`類別：</span><span class="sxs-lookup"><span data-stu-id="eb0e6-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="eb0e6-116">我們將使用`Movie`類別來代表在資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="eb0e6-117">每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="eb0e6-118">注意： 若要使用 System.Data.Entity 和相關的類別，您必須安裝[Entity Framework 的 NuGet 套件](https://www.nuget.org/packages/EntityFramework/)。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="eb0e6-119">請以取得進一步指示的連結。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="eb0e6-120">在同一個檔案中，加入下列`MovieDBContext`類別：</span><span class="sxs-lookup"><span data-stu-id="eb0e6-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="eb0e6-121">`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="eb0e6-122">`MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="eb0e6-123">若要能夠參考`DbContext`和`DbSet`，您需要新增下列`using`在檔案最上方的陳述式：</span><span class="sxs-lookup"><span data-stu-id="eb0e6-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="eb0e6-124">您可以透過手動方式新增 using 陳述式，或您可以將滑鼠停留在紅色曲線，請按一下`Show potential fixes`按一下 `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="eb0e6-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="eb0e6-125">注意： 幾個未使用`using`陳述式已被移除。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="eb0e6-126">Visual Studio 會顯示為灰色的未使用的相依性。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="eb0e6-127">您可以移除未使用的相依性，將滑鼠游標停留於灰色的相依性，請按一下`Show potential fixes`按一下**移除未使用的 using。**</span><span class="sxs-lookup"><span data-stu-id="eb0e6-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="eb0e6-128">最後，我們已加入模型 (MVC 中 M)。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="eb0e6-129">下一節將使用的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="eb0e6-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eb0e6-130">[上一頁](adding-a-view.md)
> [下一頁](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="eb0e6-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
