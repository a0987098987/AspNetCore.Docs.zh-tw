---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: 單元測試 ASP.NET Web API 2 |Microsoft Docs
author: Rick-Anderson
description: 本指南及應用程式示範如何建立簡單的單元測試，您的 Web API 2 應用程式。 本教學課程會示範如何包含單元測試專案...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 915610e6646ebe86dd8f16f290ecabd36bf7f48d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020828"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="2b7d0-104">單元測試 ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="2b7d0-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2b7d0-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2b7d0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="2b7d0-106">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="2b7d0-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="2b7d0-107">本指南及應用程式示範如何建立簡單的單元測試，您的 Web API 2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="2b7d0-108">本教學課程會示範如何在您的解決方案，包括單元測試專案和撰寫檢查從控制器方法的傳回的值的測試方法。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="2b7d0-109">本教學課程假設您已熟悉 ASP.NET Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="2b7d0-110">如需入門教學課程，請參閱 < [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="2b7d0-111">本主題中的單元測試是故意局限於簡單的資料案例。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="2b7d0-112">單元測試更進階的資料案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2b7d0-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="2b7d0-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="2b7d0-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2b7d0-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="2b7d0-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="2b7d0-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="2b7d0-116">本主題內容</span><span class="sxs-lookup"><span data-stu-id="2b7d0-116">In this topic</span></span>

<span data-ttu-id="2b7d0-117">此主題包括下列章節：</span><span class="sxs-lookup"><span data-stu-id="2b7d0-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2b7d0-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="2b7d0-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="2b7d0-119">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="2b7d0-119">Download code</span></span>](#download)
- [<span data-ttu-id="2b7d0-120">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="2b7d0-121">建立應用程式時，加入單元測試專案</span><span class="sxs-lookup"><span data-stu-id="2b7d0-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="2b7d0-122">將單元測試專案新增至現有的應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="2b7d0-123">設定 Web API 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="2b7d0-124">在測試專案中安裝 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2b7d0-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="2b7d0-125">建立測試</span><span class="sxs-lookup"><span data-stu-id="2b7d0-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="2b7d0-126">執行測試</span><span class="sxs-lookup"><span data-stu-id="2b7d0-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="2b7d0-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="2b7d0-127">Prerequisites</span></span>

<span data-ttu-id="2b7d0-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、 Professional 或 Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="2b7d0-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="2b7d0-129">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="2b7d0-129">Download code</span></span>

<span data-ttu-id="2b7d0-130">下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="2b7d0-131">可下載的專案包含單元測試程式碼以及本主題中[模擬 Entity Framework 時單元測試 ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)主題。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="2b7d0-132">使用單元測試專案建立應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-132">Create application with unit test project</span></span>

<span data-ttu-id="2b7d0-133">您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案新增至現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="2b7d0-134">本教學課程會示範這兩種方法來建立單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="2b7d0-135">若要依照本教學課程中，您可以使用兩種方法。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="2b7d0-136">建立應用程式時，加入單元測試專案</span><span class="sxs-lookup"><span data-stu-id="2b7d0-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="2b7d0-137">建立名為新 ASP.NET Web 應用程式**StoreApp**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![建立專案](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="2b7d0-139">在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="2b7d0-140">選取 **加入單元測試**選項。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="2b7d0-141">單元測試專案會自動命名**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="2b7d0-142">您可以保留此名稱。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-142">You can keep this name.</span></span>

![建立單元測試專案](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="2b7d0-144">建立應用程式之後, 您會看到它包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-144">After creating the application, you will see it contains two projects.</span></span>

![兩個專案](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="2b7d0-146">將單元測試專案新增至現有的應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="2b7d0-147">如果您在建立您的應用程式時，您未建立單元測試專案，您可以新增一個在任何時間。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="2b7d0-148">例如，假設您已經有名稱為 StoreApp，應用程式，而您想要新增單元測試。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="2b7d0-149">若要新增單元測試專案，以滑鼠右鍵按一下您的方案，然後選取**新增**並**新的專案**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![將新的專案加入方案](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="2b7d0-151">選取 **測試**左的窗格中，然後選取**單元測試專案**專案類型。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="2b7d0-152">將專案命名為**StoreApp.Tests**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-152">Name the project **StoreApp.Tests**.</span></span>

![加入單元測試專案](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="2b7d0-154">在您的方案中，您會看到單元測試專案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="2b7d0-155">在單元測試專案加入原始專案的專案參考。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="2b7d0-156">設定 Web API 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="2b7d0-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="2b7d0-157">在您 StoreApp 專案中加入類別檔案，以**模型**名為資料夾**Product.cs**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="2b7d0-158">取代下列程式碼檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="2b7d0-159">建置方案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-159">Build the solution.</span></span>

<span data-ttu-id="2b7d0-160">以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**並**新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="2b7d0-161">選取  **Web API 2 控制器-空白**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-161">Select **Web API 2 Controller - Empty**.</span></span>

![新增控制器](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="2b7d0-163">將控制器名稱設定為**SimpleProductController**，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![指定控制器](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="2b7d0-165">將現有的程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="2b7d0-166">若要簡化此範例中，資料會儲存在清單中，而不是資料庫。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="2b7d0-167">此類別中定義的清單表示實際執行資料。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="2b7d0-168">請注意，控制器就會包含一份產品物件會作為參數的建構函式。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="2b7d0-169">這個建構函式可讓您將測試資料傳遞時的單元測試。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="2b7d0-170">控制器也包含兩個**非同步**來說明單元測試非同步方法的方法。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="2b7d0-171">藉由呼叫實作這些非同步方法**Task.FromResult**降到最低，多餘的程式碼，但通常方法會包含需要大量資源的作業。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="2b7d0-172">GetProduct 方法傳回的執行個體**IHttpActionResult**介面。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="2b7d0-173">IHttpActionResult 是其中一個新功能，在 Web API 2 中，而且它可簡化單元測試開發。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="2b7d0-174">實作 IHttpActionResult 介面的類別位於[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)命名空間。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="2b7d0-175">這些類別代表可能的回應，從動作要求，以及它們對應到 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="2b7d0-176">建置方案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-176">Build the solution.</span></span>

<span data-ttu-id="2b7d0-177">您現在已準備好要設定測試專案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="2b7d0-178">在測試專案中安裝 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2b7d0-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="2b7d0-179">當您使用空白範本建立應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="2b7d0-180">其他範本，例如 Web API 範本中，包括單元測試專案中的某些 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="2b7d0-181">本教學課程中，您必須包含在測試專案的 Microsoft ASP.NET Web API 2 核心封裝。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="2b7d0-182">StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="2b7d0-183">您必須選取 StoreApp.Tests 專案，將套件新增至該專案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![管理套件](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="2b7d0-185">尋找並安裝 Microsoft ASP.NET Web API 2 核心套件。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![安裝 web api 的核心套件](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="2b7d0-187">關閉 [管理 NuGet 封裝] 視窗。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="2b7d0-188">建立測試</span><span class="sxs-lookup"><span data-stu-id="2b7d0-188">Create tests</span></span>

<span data-ttu-id="2b7d0-189">根據預設，您的測試專案會包含名為 UnitTest1.cs 的空白測試檔案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="2b7d0-190">此檔案會顯示您用來建立測試方法的屬性。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="2b7d0-191">您的單元測試，您可以使用此檔案，或建立您自己的檔案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="2b7d0-193">本教學課程中，您將建立您自己的測試類別。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="2b7d0-194">您可以刪除 UnitTest1.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="2b7d0-195">新增名為類別**TestSimpleProductController.cs**，取代為下列程式碼的程式碼。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="2b7d0-196">執行測試</span><span class="sxs-lookup"><span data-stu-id="2b7d0-196">Run tests</span></span>

<span data-ttu-id="2b7d0-197">您現在已準備好執行測試。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-197">You are now ready to run the tests.</span></span> <span data-ttu-id="2b7d0-198">所有方法標記著**TestMethod**將測試屬性。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="2b7d0-199">從**測試**功能表項目，執行測試。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-199">From the **Test** menu item, run the tests.</span></span>

![執行測試](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="2b7d0-201">開啟**測試總管** 視窗中，並注意測試的結果。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![測試結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="2b7d0-203">總結</span><span class="sxs-lookup"><span data-stu-id="2b7d0-203">Summary</span></span>

<span data-ttu-id="2b7d0-204">您已完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-204">You have completed this tutorial.</span></span> <span data-ttu-id="2b7d0-205">本教學課程中的資料已刻意簡化，以專注於單元測試條件。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="2b7d0-206">單元測試更進階的資料案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。</span><span class="sxs-lookup"><span data-stu-id="2b7d0-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
