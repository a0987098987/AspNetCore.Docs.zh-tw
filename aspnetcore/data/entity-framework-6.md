---
title: "開始使用 ASP.NET Core 與 Entity Framework 6"
author: tdykstra
description: "本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。"
keywords: "ASP.NET Core，Entity Framework EF 6"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="117c5-104">開始使用 ASP.NET Core 與 Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="117c5-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="117c5-105">由[Paweł Grudzień](https://github.com/pgrudzien12)， [Damien Pontifex](https://github.com/DamienPontifex)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="117c5-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="117c5-106">本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="117c5-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="117c5-107">概觀</span><span class="sxs-lookup"><span data-stu-id="117c5-107">Overview</span></span>

<span data-ttu-id="117c5-108">若要使用 Entity Framework 6，您的專案具有.NET Framework，根據編譯因為 Entity Framework 6 不支援.NET Core。</span><span class="sxs-lookup"><span data-stu-id="117c5-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="117c5-109">如果您需要跨平台功能必須升級至[Entity Framework Core](https://docs.microsoft.com/ef/)。</span><span class="sxs-lookup"><span data-stu-id="117c5-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="117c5-110">使用 Entity Framework 6 ASP.NET Core 應用程式中的建議的方式是將 EF6 內容和類別庫中的模型類別專案的目標為完整 framework。</span><span class="sxs-lookup"><span data-stu-id="117c5-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="117c5-111">加入 ASP.NET Core 專案中的類別庫的參考。</span><span class="sxs-lookup"><span data-stu-id="117c5-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="117c5-112">請參閱範例[EF6 和 ASP.NET Core 專案與 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)。</span><span class="sxs-lookup"><span data-stu-id="117c5-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="117c5-113">您無法暫停 EF6 內容 ASP.NET Core 專案中，因為.NET Core 專案不支援的所有功能，例如命令 EF6 *Enable-migrations*需要。</span><span class="sxs-lookup"><span data-stu-id="117c5-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="117c5-114">不論您找出 EF6 內容的專案類型、 EF6 命令列工具使用 EF6 內容。</span><span class="sxs-lookup"><span data-stu-id="117c5-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="117c5-115">例如，`Scaffold-DbContext`僅供以 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="117c5-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="117c5-116">如果您需要執行 EF6 模型反向工程的資料庫，請參閱[Code First 到現有的資料庫](https://msdn.microsoft.com/jj200620)。</span><span class="sxs-lookup"><span data-stu-id="117c5-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="117c5-117">參考完整的 framework 和 EF6 ASP.NET Core 專案中</span><span class="sxs-lookup"><span data-stu-id="117c5-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="117c5-118">您的 ASP.NET Core 專案必須參考.NET framework 和 EF6。</span><span class="sxs-lookup"><span data-stu-id="117c5-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="117c5-119">例如， *.csproj* ASP.NET Core 專案檔看起來類似下列範例 （顯示檔案相關的部分）。</span><span class="sxs-lookup"><span data-stu-id="117c5-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="117c5-120">如果您要建立新的專案，使用**ASP.NET Core Web 應用程式 (.NET Framework)**範本。</span><span class="sxs-lookup"><span data-stu-id="117c5-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="117c5-121">處理連接字串</span><span class="sxs-lookup"><span data-stu-id="117c5-121">Handle connection strings</span></span>

<span data-ttu-id="117c5-122">EF6 命令列工具，您將使用 EF6 類別庫專案中需要預設建構函式，因此它們可以具現化內容。</span><span class="sxs-lookup"><span data-stu-id="117c5-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="117c5-123">但是，您可能會想要指定連接字串，在 ASP.NET Core 專案中，在此情況下使用內容的建構函式必須要有可讓您在連接字串中傳遞的參數。</span><span class="sxs-lookup"><span data-stu-id="117c5-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="117c5-124">以下是範例。</span><span class="sxs-lookup"><span data-stu-id="117c5-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="117c5-125">因為 EF6 內容沒有無參數建構函式，必須提供實作 EF6 專案[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)。</span><span class="sxs-lookup"><span data-stu-id="117c5-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="117c5-126">EF6 命令列工具會尋找並使用該實作，因此它們可以具現化內容。</span><span class="sxs-lookup"><span data-stu-id="117c5-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="117c5-127">以下是範例。</span><span class="sxs-lookup"><span data-stu-id="117c5-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="117c5-128">在此範例程式碼，`IDbContextFactory`硬式編碼的連接字串中傳遞實作。</span><span class="sxs-lookup"><span data-stu-id="117c5-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="117c5-129">這是命令列工具將使用的連接字串。</span><span class="sxs-lookup"><span data-stu-id="117c5-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="117c5-130">您要實作以確保類別庫會使用相同的連接字串呼叫的應用程式所使用的策略。</span><span class="sxs-lookup"><span data-stu-id="117c5-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="117c5-131">例如，您可以從這兩個專案中的環境變數取得值。</span><span class="sxs-lookup"><span data-stu-id="117c5-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="117c5-132">設定 ASP.NET Core 專案中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="117c5-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="117c5-133">在核心專案的*Startup.cs*檔案，在中設定相依性插入 (DI) 的 EF6 內容`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="117c5-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="117c5-134">應該範圍內的每個要求存留期的 EF 內容物件。</span><span class="sxs-lookup"><span data-stu-id="117c5-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="117c5-135">然後就可以內容的執行個體中的控制站使用 DI。</span><span class="sxs-lookup"><span data-stu-id="117c5-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="117c5-136">程式碼會類似於您要撰寫 EF 核心內容：</span><span class="sxs-lookup"><span data-stu-id="117c5-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="117c5-137">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="117c5-137">Sample application</span></span>

<span data-ttu-id="117c5-138">可用的範例應用程式，請參閱[範例 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)，本文章。</span><span class="sxs-lookup"><span data-stu-id="117c5-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="117c5-139">此範例可以從頭開始建立由 Visual Studio 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="117c5-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="117c5-140">建立方案。</span><span class="sxs-lookup"><span data-stu-id="117c5-140">Create a solution.</span></span>

* <span data-ttu-id="117c5-141">**加入新的專案 > 網路 > ASP.NET Core Web 應用程式 (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="117c5-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="117c5-142">**加入新的專案 > 的傳統 Windows 桌面 > 類別庫 (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="117c5-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="117c5-143">在**Package Manager Console** (PMC) 這兩個專案中，執行命令`Install-Package Entityframework`。</span><span class="sxs-lookup"><span data-stu-id="117c5-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="117c5-144">在類別庫專案中，建立資料模型類別和內容類別的實作`IDbContextFactory`。</span><span class="sxs-lookup"><span data-stu-id="117c5-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="117c5-145">在類別庫專案的 PMC，執行命令`Enable-Migrations`和`Add-Migration Initial`。</span><span class="sxs-lookup"><span data-stu-id="117c5-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="117c5-146">如果您已經設定 ASP.NET Core 專案為啟始專案，加入`-StartupProjectName EF6`這些命令。</span><span class="sxs-lookup"><span data-stu-id="117c5-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="117c5-147">在 [核心] 專案中，加入類別庫專案的專案參考。</span><span class="sxs-lookup"><span data-stu-id="117c5-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="117c5-148">在核心專案中，在*Startup.cs*，DI 註冊內容。</span><span class="sxs-lookup"><span data-stu-id="117c5-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="117c5-149">在核心專案中，在*appsettings.json*，加入連接字串。</span><span class="sxs-lookup"><span data-stu-id="117c5-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="117c5-150">在 [核心] 專案中，加入控制器，並確認您可以讀取和寫入資料的檢視表。</span><span class="sxs-lookup"><span data-stu-id="117c5-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="117c5-151">（請注意，ASP.NET Core MVC scaffolding 不會使用 EF6 內容類別庫的參考。）</span><span class="sxs-lookup"><span data-stu-id="117c5-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="117c5-152">總結</span><span class="sxs-lookup"><span data-stu-id="117c5-152">Summary</span></span>

<span data-ttu-id="117c5-153">本文提供的基本指導方針 ASP.NET Core 應用程式中使用 Entity Framework 6。</span><span class="sxs-lookup"><span data-stu-id="117c5-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="117c5-154">其他資源</span><span class="sxs-lookup"><span data-stu-id="117c5-154">Additional Resources</span></span>

* [<span data-ttu-id="117c5-155">Entity Framework 的程式碼為基礎的設定</span><span class="sxs-lookup"><span data-stu-id="117c5-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
