---
title: "開始使用 ASP.NET Core 與 Entity Framework 6"
author: tdykstra
description: "本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 51445b8c110ad618aeb680148ccf4304a45ee16e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="035a8-103">開始使用 ASP.NET Core 與 Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="035a8-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="035a8-104">由[Paweł Grudzień](https://github.com/pgrudzien12)， [Damien Pontifex](https://github.com/DamienPontifex)，和[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="035a8-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="035a8-105">本文示範如何使用 Entity Framework 6 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="035a8-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="035a8-106">總覽</span><span class="sxs-lookup"><span data-stu-id="035a8-106">Overview</span></span>

<span data-ttu-id="035a8-107">若要使用 Entity Framework 6，您的專案具有.NET Framework，根據編譯因為 Entity Framework 6 不支援.NET Core。</span><span class="sxs-lookup"><span data-stu-id="035a8-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="035a8-108">如果您需要跨平台功能必須升級至[Entity Framework Core](https://docs.microsoft.com/ef/)。</span><span class="sxs-lookup"><span data-stu-id="035a8-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="035a8-109">使用 Entity Framework 6 ASP.NET Core 應用程式中的建議的方式是將 EF6 內容和類別庫中的模型類別專案的目標為完整 framework。</span><span class="sxs-lookup"><span data-stu-id="035a8-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="035a8-110">加入 ASP.NET Core 專案中的類別庫的參考。</span><span class="sxs-lookup"><span data-stu-id="035a8-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="035a8-111">請參閱範例[EF6 和 ASP.NET Core 專案與 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)。</span><span class="sxs-lookup"><span data-stu-id="035a8-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="035a8-112">您無法暫停 EF6 內容 ASP.NET Core 專案中，因為.NET Core 專案不支援的所有功能，例如命令 EF6 *Enable-migrations*需要。</span><span class="sxs-lookup"><span data-stu-id="035a8-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="035a8-113">不論您找出 EF6 內容的專案類型、 EF6 命令列工具使用 EF6 內容。</span><span class="sxs-lookup"><span data-stu-id="035a8-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="035a8-114">例如，`Scaffold-DbContext`僅供以 Entity Framework Core。</span><span class="sxs-lookup"><span data-stu-id="035a8-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="035a8-115">如果您需要執行 EF6 模型反向工程的資料庫，請參閱[Code First 到現有的資料庫](https://msdn.microsoft.com/jj200620)。</span><span class="sxs-lookup"><span data-stu-id="035a8-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="035a8-116">參考完整的 framework 和 EF6 ASP.NET Core 專案中</span><span class="sxs-lookup"><span data-stu-id="035a8-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="035a8-117">您的 ASP.NET Core 專案必須參考.NET framework 和 EF6。</span><span class="sxs-lookup"><span data-stu-id="035a8-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="035a8-118">例如， *.csproj* ASP.NET Core 專案檔看起來類似下列範例 （顯示檔案相關的部分）。</span><span class="sxs-lookup"><span data-stu-id="035a8-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="035a8-119">如果您要建立新的專案，使用**ASP.NET Core Web 應用程式 (.NET Framework)**範本。</span><span class="sxs-lookup"><span data-stu-id="035a8-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="035a8-120">處理連接字串</span><span class="sxs-lookup"><span data-stu-id="035a8-120">Handle connection strings</span></span>

<span data-ttu-id="035a8-121">EF6 命令列工具，您將使用 EF6 類別庫專案中需要預設建構函式，因此它們可以具現化內容。</span><span class="sxs-lookup"><span data-stu-id="035a8-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="035a8-122">但是，您可能會想要指定連接字串，在 ASP.NET Core 專案中，在此情況下使用內容的建構函式必須要有可讓您在連接字串中傳遞的參數。</span><span class="sxs-lookup"><span data-stu-id="035a8-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="035a8-123">以下是範例。</span><span class="sxs-lookup"><span data-stu-id="035a8-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="035a8-124">因為 EF6 內容沒有無參數建構函式，必須提供實作 EF6 專案[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)。</span><span class="sxs-lookup"><span data-stu-id="035a8-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="035a8-125">EF6 命令列工具會尋找並使用該實作，因此它們可以具現化內容。</span><span class="sxs-lookup"><span data-stu-id="035a8-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="035a8-126">以下是範例。</span><span class="sxs-lookup"><span data-stu-id="035a8-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="035a8-127">在此範例程式碼，`IDbContextFactory`硬式編碼的連接字串中傳遞實作。</span><span class="sxs-lookup"><span data-stu-id="035a8-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="035a8-128">這是命令列工具將使用的連接字串。</span><span class="sxs-lookup"><span data-stu-id="035a8-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="035a8-129">您要實作以確保類別庫會使用相同的連接字串呼叫的應用程式所使用的策略。</span><span class="sxs-lookup"><span data-stu-id="035a8-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="035a8-130">例如，您可以從這兩個專案中的環境變數取得值。</span><span class="sxs-lookup"><span data-stu-id="035a8-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="035a8-131">設定 ASP.NET Core 專案中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="035a8-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="035a8-132">在核心專案的*Startup.cs*檔案，在中設定相依性插入 (DI) 的 EF6 內容`ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="035a8-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="035a8-133">應該範圍內的每個要求存留期的 EF 內容物件。</span><span class="sxs-lookup"><span data-stu-id="035a8-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="035a8-134">然後就可以內容的執行個體中的控制站使用 DI。</span><span class="sxs-lookup"><span data-stu-id="035a8-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="035a8-135">程式碼會類似於您要撰寫 EF 核心內容：</span><span class="sxs-lookup"><span data-stu-id="035a8-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="035a8-136">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="035a8-136">Sample application</span></span>

<span data-ttu-id="035a8-137">可用的範例應用程式，請參閱[範例 Visual Studio 方案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)，本文章。</span><span class="sxs-lookup"><span data-stu-id="035a8-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="035a8-138">此範例可以從頭開始建立由 Visual Studio 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="035a8-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="035a8-139">建立方案。</span><span class="sxs-lookup"><span data-stu-id="035a8-139">Create a solution.</span></span>

* <span data-ttu-id="035a8-140">**加入新的專案 > 網路 > ASP.NET Core Web 應用程式 (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="035a8-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="035a8-141">**加入新的專案 > 的傳統 Windows 桌面 > 類別庫 (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="035a8-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="035a8-142">在**Package Manager Console** (PMC) 這兩個專案中，執行命令`Install-Package Entityframework`。</span><span class="sxs-lookup"><span data-stu-id="035a8-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="035a8-143">在類別庫專案中，建立資料模型類別和內容類別的實作`IDbContextFactory`。</span><span class="sxs-lookup"><span data-stu-id="035a8-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="035a8-144">在類別庫專案的 PMC，執行命令`Enable-Migrations`和`Add-Migration Initial`。</span><span class="sxs-lookup"><span data-stu-id="035a8-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="035a8-145">如果您已經設定 ASP.NET Core 專案為啟始專案，加入`-StartupProjectName EF6`這些命令。</span><span class="sxs-lookup"><span data-stu-id="035a8-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="035a8-146">在 [核心] 專案中，加入類別庫專案的專案參考。</span><span class="sxs-lookup"><span data-stu-id="035a8-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="035a8-147">在核心專案中，在*Startup.cs*，DI 註冊內容。</span><span class="sxs-lookup"><span data-stu-id="035a8-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="035a8-148">在核心專案中，在*appsettings.json*，加入連接字串。</span><span class="sxs-lookup"><span data-stu-id="035a8-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="035a8-149">在 [核心] 專案中，加入控制器，並確認您可以讀取和寫入資料的檢視表。</span><span class="sxs-lookup"><span data-stu-id="035a8-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="035a8-150">（請注意，ASP.NET Core MVC scaffolding 不會使用 EF6 內容類別庫的參考。）</span><span class="sxs-lookup"><span data-stu-id="035a8-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="035a8-151">總結</span><span class="sxs-lookup"><span data-stu-id="035a8-151">Summary</span></span>

<span data-ttu-id="035a8-152">本文提供的基本指導方針 ASP.NET Core 應用程式中使用 Entity Framework 6。</span><span class="sxs-lookup"><span data-stu-id="035a8-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="035a8-153">其他資源</span><span class="sxs-lookup"><span data-stu-id="035a8-153">Additional Resources</span></span>

* [<span data-ttu-id="035a8-154">Entity Framework 的程式碼為基礎的設定</span><span class="sxs-lookup"><span data-stu-id="035a8-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
