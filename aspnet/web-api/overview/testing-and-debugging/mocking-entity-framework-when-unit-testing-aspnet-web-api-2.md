---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "模擬 Entity Framework 時單元測試 ASP.NET Web API 2 |Microsoft 文件"
author: tfitzmac
description: "本指南及應用程式示範如何建立單元測試，您使用 Entity Framework 的 Web API 2 應用程式。 它會顯示如何修改..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2d8a3df94c91d2fac79006916375764c2b90dc85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="30a44-104">模擬 Entity Framework 時單元測試 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="30a44-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="30a44-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="30a44-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="30a44-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="30a44-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="30a44-107">本指南及應用程式示範如何建立單元測試，您使用 Entity Framework 的 Web API 2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="30a44-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="30a44-108">它會顯示如何修改 scaffold 的控制器啟用傳遞內容物件來進行測試，以及如何建立測試物件可搭配 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="30a44-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="30a44-109">如需單元測試的 ASP.NET Web API 的簡介，請參閱[單元測試的 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="30a44-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="30a44-110">本教學課程假設您熟悉的 ASP.NET Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="30a44-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="30a44-111">如需入門教學課程，請參閱[開始使用 ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="30a44-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="30a44-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="30a44-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="30a44-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="30a44-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="30a44-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="30a44-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="30a44-115">本主題內容</span><span class="sxs-lookup"><span data-stu-id="30a44-115">In this topic</span></span>

<span data-ttu-id="30a44-116">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="30a44-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="30a44-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="30a44-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="30a44-118">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="30a44-118">Download code</span></span>](#download)
- [<span data-ttu-id="30a44-119">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="30a44-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="30a44-120">建立模型類別</span><span class="sxs-lookup"><span data-stu-id="30a44-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="30a44-121">加入控制器</span><span class="sxs-lookup"><span data-stu-id="30a44-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="30a44-122">新增相依性插入</span><span class="sxs-lookup"><span data-stu-id="30a44-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="30a44-123">在測試專案中安裝 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="30a44-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="30a44-124">建立測試內容</span><span class="sxs-lookup"><span data-stu-id="30a44-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="30a44-125">建立測試</span><span class="sxs-lookup"><span data-stu-id="30a44-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="30a44-126">執行測試</span><span class="sxs-lookup"><span data-stu-id="30a44-126">Run tests</span></span>](#runtests)

<span data-ttu-id="30a44-127">如果您已經完成中的步驟[單元測試的 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，您可以略過區段[加入控制器](#controller)。</span><span class="sxs-lookup"><span data-stu-id="30a44-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="30a44-128">必要條件</span><span class="sxs-lookup"><span data-stu-id="30a44-128">Prerequisites</span></span>

<span data-ttu-id="30a44-129">Visual Studio 2017 Community、 Professional 或 Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="30a44-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="30a44-130">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="30a44-130">Download code</span></span>

<span data-ttu-id="30a44-131">下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。</span><span class="sxs-lookup"><span data-stu-id="30a44-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="30a44-132">可下載專案中包含單元測試程式碼為本主題和[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)主題。</span><span class="sxs-lookup"><span data-stu-id="30a44-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="30a44-133">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="30a44-133">Create application with unit test project</span></span>

<span data-ttu-id="30a44-134">您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案加入至現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="30a44-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="30a44-135">本教學課程會示範建立單元測試專案，建立應用程式時。</span><span class="sxs-lookup"><span data-stu-id="30a44-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="30a44-136">建立新 ASP.NET Web 應用程式名為**StoreApp**。</span><span class="sxs-lookup"><span data-stu-id="30a44-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="30a44-137">在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。</span><span class="sxs-lookup"><span data-stu-id="30a44-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="30a44-138">選取**加入單元測試**選項。</span><span class="sxs-lookup"><span data-stu-id="30a44-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="30a44-139">單元測試專案會自動命名**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="30a44-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="30a44-140">您可以保留這個名稱。</span><span class="sxs-lookup"><span data-stu-id="30a44-140">You can keep this name.</span></span>

![建立單元測試專案](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="30a44-142">建立應用程式之後, 您會看到它包含兩個專案- **StoreApp**和**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="30a44-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="30a44-143">建立模型類別</span><span class="sxs-lookup"><span data-stu-id="30a44-143">Create the model class</span></span>

<span data-ttu-id="30a44-144">在您 StoreApp 的專案，加入至類別檔案**模型**資料夾名為**Product.cs**。</span><span class="sxs-lookup"><span data-stu-id="30a44-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="30a44-145">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="30a44-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="30a44-146">建置方案。</span><span class="sxs-lookup"><span data-stu-id="30a44-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="30a44-147">加入控制器</span><span class="sxs-lookup"><span data-stu-id="30a44-147">Add the controller</span></span>

<span data-ttu-id="30a44-148">以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**和**新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="30a44-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="30a44-149">選取 Web API 2 控制器使用 Entity Framework 執行動作。</span><span class="sxs-lookup"><span data-stu-id="30a44-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![加入新的控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="30a44-151">設定下列的值：</span><span class="sxs-lookup"><span data-stu-id="30a44-151">Set the following values:</span></span>

- <span data-ttu-id="30a44-152">控制器名稱： **ProductController**</span><span class="sxs-lookup"><span data-stu-id="30a44-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="30a44-153">模型類別：**產品**</span><span class="sxs-lookup"><span data-stu-id="30a44-153">Model class: **Product**</span></span>
- <span data-ttu-id="30a44-154">資料內容類別: [選取**新的資料內容**按鈕，如下所示的值填入]</span><span class="sxs-lookup"><span data-stu-id="30a44-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![指定控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="30a44-156">按一下**新增**以自動產生的程式碼建立控制器。</span><span class="sxs-lookup"><span data-stu-id="30a44-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="30a44-157">程式碼包含了建立、 擷取、 更新及刪除產品類別的執行個體方法。</span><span class="sxs-lookup"><span data-stu-id="30a44-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="30a44-158">下列程式碼示範的方法為將產品。</span><span class="sxs-lookup"><span data-stu-id="30a44-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="30a44-159">請注意，方法會傳回的執行個體**IHttpActionResult**。</span><span class="sxs-lookup"><span data-stu-id="30a44-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="30a44-160">IHttpActionResult 是其中一個在 Web API 2 中，新的功能，並簡化單元測試開發。</span><span class="sxs-lookup"><span data-stu-id="30a44-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="30a44-161">在下一步 區段中，您將自訂產生的程式碼，以便將測試物件傳遞至控制器。</span><span class="sxs-lookup"><span data-stu-id="30a44-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="30a44-162">新增相依性插入</span><span class="sxs-lookup"><span data-stu-id="30a44-162">Add dependency injection</span></span>

<span data-ttu-id="30a44-163">目前，ProductController 類別是硬式編碼使用 StoreAppContext 類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="30a44-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="30a44-164">若要修改您的應用程式，並移除該硬式編碼相依性，您將使用模式，呼叫相依性插入。</span><span class="sxs-lookup"><span data-stu-id="30a44-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="30a44-165">中斷此相依性之後，您可以測試時傳遞模擬物件中。</span><span class="sxs-lookup"><span data-stu-id="30a44-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="30a44-166">以滑鼠右鍵按一下**模型**資料夾，並加入新的介面，名為**IStoreAppContext**。</span><span class="sxs-lookup"><span data-stu-id="30a44-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="30a44-167">使用下列程式碼取代程式碼。</span><span class="sxs-lookup"><span data-stu-id="30a44-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="30a44-168">開啟 StoreAppContext.cs 檔案並進行下列反白顯示的變更。</span><span class="sxs-lookup"><span data-stu-id="30a44-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="30a44-169">要注意的重要變更會：</span><span class="sxs-lookup"><span data-stu-id="30a44-169">The important changes to note are:</span></span>

- <span data-ttu-id="30a44-170">StoreAppContext 類別會實作 IStoreAppContext 介面</span><span class="sxs-lookup"><span data-stu-id="30a44-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="30a44-171">實作 MarkAsModified 方法</span><span class="sxs-lookup"><span data-stu-id="30a44-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="30a44-172">開啟 ProductController.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="30a44-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="30a44-173">變更現有的程式碼，以符合反白顯示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="30a44-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="30a44-174">這些變更在 StoreAppContext 中斷相依性，讓其他類別，不同物件中傳遞的內容類別。</span><span class="sxs-lookup"><span data-stu-id="30a44-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="30a44-175">這項變更可讓您在單元測試期間在測試內容中傳遞。</span><span class="sxs-lookup"><span data-stu-id="30a44-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="30a44-176">沒有一個詳細的變更，您必須在 ProductController。</span><span class="sxs-lookup"><span data-stu-id="30a44-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="30a44-177">在**PutProduct**方法中，實體狀態設定為的程式修改 MarkAsModified 方法呼叫取代。</span><span class="sxs-lookup"><span data-stu-id="30a44-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="30a44-178">建置方案。</span><span class="sxs-lookup"><span data-stu-id="30a44-178">Build the solution.</span></span>

<span data-ttu-id="30a44-179">現在您已經準備好設定測試專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="30a44-180">在測試專案中安裝 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="30a44-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="30a44-181">當您使用空白範本建立的應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="30a44-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="30a44-182">其他範本，例如 Web API 範本，包括單元測試專案中的部分 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="30a44-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="30a44-183">此教學課程中，您必須包含 Entity Framework 封裝時，發生與 Microsoft ASP.NET Web API 2 核心封裝至測試專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="30a44-184">StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="30a44-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="30a44-185">您必須選取 StoreApp.Tests 專案加入至該專案的封裝。</span><span class="sxs-lookup"><span data-stu-id="30a44-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![管理封裝](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="30a44-187">從線上套件中，尋找並安裝 EntityFramework 封裝 （6.0 版或更新版本）。</span><span class="sxs-lookup"><span data-stu-id="30a44-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="30a44-188">如果似乎已經安裝了 EntityFramework 封裝時，您可能選取了 StoreApp 專案，而不是 StoreApp.Tests 專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the the StoreApp.Tests project.</span></span>

![加入實體架構](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="30a44-190">尋找並安裝 Microsoft ASP.NET Web API 2 核心封裝。</span><span class="sxs-lookup"><span data-stu-id="30a44-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![安裝 web api core 套件](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="30a44-192">關閉 [管理 NuGet 封裝] 視窗。</span><span class="sxs-lookup"><span data-stu-id="30a44-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="30a44-193">建立測試內容</span><span class="sxs-lookup"><span data-stu-id="30a44-193">Create test context</span></span>

<span data-ttu-id="30a44-194">將類別命名為**TestDbSet**至測試專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="30a44-195">這個類別可做為您的測試資料集的基底類別。</span><span class="sxs-lookup"><span data-stu-id="30a44-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="30a44-196">使用下列程式碼取代程式碼。</span><span class="sxs-lookup"><span data-stu-id="30a44-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="30a44-197">將類別命名為**TestProductDbSet**含有下列程式碼測試專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="30a44-198">將類別命名為**TestStoreAppContext** ，並以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="30a44-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="30a44-199">建立測試</span><span class="sxs-lookup"><span data-stu-id="30a44-199">Create tests</span></span>

<span data-ttu-id="30a44-200">根據預設，您的測試專案包含名為的空白測試檔案**UnitTest1.cs**。</span><span class="sxs-lookup"><span data-stu-id="30a44-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="30a44-201">這個檔案會顯示您用於建立測試方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="30a44-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="30a44-202">此教學課程中，您可以刪除此檔案，因為您會新增新的測試類別。</span><span class="sxs-lookup"><span data-stu-id="30a44-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="30a44-203">將類別命名為**TestProductController**至測試專案。</span><span class="sxs-lookup"><span data-stu-id="30a44-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="30a44-204">使用下列程式碼取代程式碼。</span><span class="sxs-lookup"><span data-stu-id="30a44-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="30a44-205">執行測試</span><span class="sxs-lookup"><span data-stu-id="30a44-205">Run tests</span></span>

<span data-ttu-id="30a44-206">現在您已經準備好要執行測試。</span><span class="sxs-lookup"><span data-stu-id="30a44-206">You are now ready to run the tests.</span></span> <span data-ttu-id="30a44-207">所有的方法，以標記**TestMethod**屬性將進行測試。</span><span class="sxs-lookup"><span data-stu-id="30a44-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="30a44-208">從**測試**功能表項目，執行測試。</span><span class="sxs-lookup"><span data-stu-id="30a44-208">From the **Test** menu item, run the tests.</span></span>

![執行測試](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="30a44-210">開啟**測試總管**視窗中，並注意測試的結果。</span><span class="sxs-lookup"><span data-stu-id="30a44-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![測試結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
