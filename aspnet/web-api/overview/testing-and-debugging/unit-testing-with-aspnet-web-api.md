---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "單元測試 ASP.NET Web API 2 |Microsoft 文件"
author: tfitzmac
description: "本指南及應用程式示範如何建立簡單的單元測試您的 Web API 2 應用程式。 本教學課程會示範如何加入單元測試專案..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="2e253-104">單元測試 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2e253-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2e253-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2e253-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="2e253-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="2e253-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="2e253-107">本指南及應用程式示範如何建立簡單的單元測試您的 Web API 2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e253-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="2e253-108">本教學課程會示範如何在您的方案中包含單元測試專案和寫入測試方法，請檢查從控制器方法的傳回的值。</span><span class="sxs-lookup"><span data-stu-id="2e253-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="2e253-109">本教學課程假設您熟悉的 ASP.NET Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="2e253-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="2e253-110">如需入門教學課程，請參閱[開始使用 ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="2e253-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="2e253-111">本主題中的單元測試會刻意限制為簡單資料案例。</span><span class="sxs-lookup"><span data-stu-id="2e253-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="2e253-112">單元測試資料的更進階的案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="2e253-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2e253-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="2e253-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="2e253-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2e253-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="2e253-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2e253-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="2e253-116">本主題內容</span><span class="sxs-lookup"><span data-stu-id="2e253-116">In this topic</span></span>

<span data-ttu-id="2e253-117">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="2e253-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2e253-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="2e253-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="2e253-119">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="2e253-119">Download code</span></span>](#download)
- [<span data-ttu-id="2e253-120">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="2e253-121">建立應用程式時，加入單元測試專案</span><span class="sxs-lookup"><span data-stu-id="2e253-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="2e253-122">將單元測試專案加入至現有的應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="2e253-123">設定 Web API 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="2e253-124">在測試專案中安裝 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="2e253-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="2e253-125">建立測試</span><span class="sxs-lookup"><span data-stu-id="2e253-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="2e253-126">執行測試</span><span class="sxs-lookup"><span data-stu-id="2e253-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="2e253-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="2e253-127">Prerequisites</span></span>

<span data-ttu-id="2e253-128">Visual Studio 2017 Community、 Professional 或 Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="2e253-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="2e253-129">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="2e253-129">Download code</span></span>

<span data-ttu-id="2e253-130">下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。</span><span class="sxs-lookup"><span data-stu-id="2e253-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="2e253-131">可下載專案中包含單元測試程式碼為本主題和[模擬 Entity Framework 時單元測試 ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)主題。</span><span class="sxs-lookup"><span data-stu-id="2e253-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="2e253-132">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-132">Create application with unit test project</span></span>

<span data-ttu-id="2e253-133">您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案加入至現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2e253-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="2e253-134">本教學課程會示範這兩種方法來建立單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="2e253-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="2e253-135">若要依照本教學課程，您可以使用兩種方法。</span><span class="sxs-lookup"><span data-stu-id="2e253-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="2e253-136">建立應用程式時，加入單元測試專案</span><span class="sxs-lookup"><span data-stu-id="2e253-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="2e253-137">建立新 ASP.NET Web 應用程式名為**StoreApp**。</span><span class="sxs-lookup"><span data-stu-id="2e253-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![建立專案](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2e253-139">在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。</span><span class="sxs-lookup"><span data-stu-id="2e253-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="2e253-140">選取**加入單元測試**選項。</span><span class="sxs-lookup"><span data-stu-id="2e253-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="2e253-141">單元測試專案會自動命名**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="2e253-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="2e253-142">您可以保留這個名稱。</span><span class="sxs-lookup"><span data-stu-id="2e253-142">You can keep this name.</span></span>

![建立單元測試專案](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="2e253-144">建立應用程式之後, 您會看到它包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="2e253-144">After creating the application, you will see it contains two projects.</span></span>

![兩個專案](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="2e253-146">將單元測試專案加入至現有的應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="2e253-147">如果您未建立單元測試專案建立您的應用程式時，您可以新增一個在任何時間。</span><span class="sxs-lookup"><span data-stu-id="2e253-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="2e253-148">例如，假設您已經有一個名為 StoreApp，而且您想要加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="2e253-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="2e253-149">若要加入單元測試專案，以滑鼠右鍵按一下您的方案，然後選取**新增**和**新專案**。</span><span class="sxs-lookup"><span data-stu-id="2e253-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![將新專案加入方案](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="2e253-151">選取**測試**左的窗格中，然後選取**單元測試專案**專案類型。</span><span class="sxs-lookup"><span data-stu-id="2e253-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="2e253-152">將專案命名**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="2e253-152">Name the project **StoreApp.Tests**.</span></span>

![新增單元測試專案](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="2e253-154">您會看到 單元測試專案方案中。</span><span class="sxs-lookup"><span data-stu-id="2e253-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="2e253-155">在單元測試專案加入原始專案的專案參考。</span><span class="sxs-lookup"><span data-stu-id="2e253-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="2e253-156">設定 Web API 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="2e253-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="2e253-157">在您 StoreApp 的專案，加入至類別檔案**模型**資料夾名為**Product.cs**。</span><span class="sxs-lookup"><span data-stu-id="2e253-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="2e253-158">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2e253-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2e253-159">建置方案。</span><span class="sxs-lookup"><span data-stu-id="2e253-159">Build the solution.</span></span>

<span data-ttu-id="2e253-160">以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**和**新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="2e253-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="2e253-161">選取**Web API 2 控制器-空白**。</span><span class="sxs-lookup"><span data-stu-id="2e253-161">Select **Web API 2 Controller - Empty**.</span></span>

![加入新的控制器](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="2e253-163">將控制器名稱設定為**SimpleProductController**，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="2e253-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![指定控制器](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="2e253-165">將現有的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e253-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="2e253-166">若要簡化此範例中，資料會儲存在清單中，而不是資料庫。</span><span class="sxs-lookup"><span data-stu-id="2e253-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="2e253-167">此類別中定義之清單中代表實際執行資料。</span><span class="sxs-lookup"><span data-stu-id="2e253-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="2e253-168">請注意，控制器會包含一份產品物件當作參數的建構函式。</span><span class="sxs-lookup"><span data-stu-id="2e253-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="2e253-169">這個建構函式可讓您將測試資料時執行單元測試。</span><span class="sxs-lookup"><span data-stu-id="2e253-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="2e253-170">控制器也包含兩個**非同步**說明單元測試的非同步方法的方法。</span><span class="sxs-lookup"><span data-stu-id="2e253-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="2e253-171">這些非同步方法呼叫實作了**Task.FromResult**若要減少多餘的程式碼，但通常方法會加入資源密集作業。</span><span class="sxs-lookup"><span data-stu-id="2e253-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="2e253-172">GetProduct 方法傳回的執行個體**IHttpActionResult**介面。</span><span class="sxs-lookup"><span data-stu-id="2e253-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="2e253-173">IHttpActionResult 是其中一個在 Web API 2 中，新的功能，並簡化單元測試開發。</span><span class="sxs-lookup"><span data-stu-id="2e253-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="2e253-174">實作 IHttpActionResult 介面的類別位於[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)命名空間。</span><span class="sxs-lookup"><span data-stu-id="2e253-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="2e253-175">這些類別代表與動作的要求中的可能回應，而且它們對應到 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2e253-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="2e253-176">建置方案。</span><span class="sxs-lookup"><span data-stu-id="2e253-176">Build the solution.</span></span>

<span data-ttu-id="2e253-177">現在您已經準備好設定測試專案。</span><span class="sxs-lookup"><span data-stu-id="2e253-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="2e253-178">在測試專案中安裝 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="2e253-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="2e253-179">當您使用空白範本建立的應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="2e253-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="2e253-180">其他範本，例如 Web API 範本，包括單元測試專案中的部分 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="2e253-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="2e253-181">此教學課程中，您必須包含在測試專案的 Microsoft ASP.NET Web API 2 核心封裝。</span><span class="sxs-lookup"><span data-stu-id="2e253-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="2e253-182">StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="2e253-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="2e253-183">您必須選取 StoreApp.Tests 專案加入至該專案的封裝。</span><span class="sxs-lookup"><span data-stu-id="2e253-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![管理封裝](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="2e253-185">尋找並安裝 Microsoft ASP.NET Web API 2 核心封裝。</span><span class="sxs-lookup"><span data-stu-id="2e253-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![安裝 web api core 套件](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="2e253-187">關閉 [管理 NuGet 封裝] 視窗。</span><span class="sxs-lookup"><span data-stu-id="2e253-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="2e253-188">建立測試</span><span class="sxs-lookup"><span data-stu-id="2e253-188">Create tests</span></span>

<span data-ttu-id="2e253-189">根據預設，您的測試專案包含名為 UnitTest1.cs 的空白測試檔案。</span><span class="sxs-lookup"><span data-stu-id="2e253-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="2e253-190">這個檔案會顯示您用於建立測試方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="2e253-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="2e253-191">您的單元測試，您可以使用此檔案，或建立您自己的檔案。</span><span class="sxs-lookup"><span data-stu-id="2e253-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="2e253-193">此教學課程中，您將建立您自己的測試類別。</span><span class="sxs-lookup"><span data-stu-id="2e253-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="2e253-194">您可以刪除的 UnitTest1.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="2e253-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="2e253-195">將類別命名為**TestSimpleProductController.cs**，並以下列程式碼取代程式碼。</span><span class="sxs-lookup"><span data-stu-id="2e253-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="2e253-196">執行測試</span><span class="sxs-lookup"><span data-stu-id="2e253-196">Run tests</span></span>

<span data-ttu-id="2e253-197">現在您已經準備好要執行測試。</span><span class="sxs-lookup"><span data-stu-id="2e253-197">You are now ready to run the tests.</span></span> <span data-ttu-id="2e253-198">所有的方法，以標記**TestMethod**屬性將進行測試。</span><span class="sxs-lookup"><span data-stu-id="2e253-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="2e253-199">從**測試**功能表項目，執行測試。</span><span class="sxs-lookup"><span data-stu-id="2e253-199">From the **Test** menu item, run the tests.</span></span>

![執行測試](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="2e253-201">開啟**測試總管**視窗中，並注意測試的結果。</span><span class="sxs-lookup"><span data-stu-id="2e253-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![測試結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="2e253-203">總結</span><span class="sxs-lookup"><span data-stu-id="2e253-203">Summary</span></span>

<span data-ttu-id="2e253-204">您已經完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="2e253-204">You have completed this tutorial.</span></span> <span data-ttu-id="2e253-205">將重點放在單元測試條件，已刻意簡化本教學課程中的資料。</span><span class="sxs-lookup"><span data-stu-id="2e253-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="2e253-206">單元測試資料的更進階的案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="2e253-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
