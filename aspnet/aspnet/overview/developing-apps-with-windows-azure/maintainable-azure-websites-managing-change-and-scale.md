---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "在實驗室交給： 容易維護的 Azure 網站： 管理變更和小數位數 |Microsoft 文件"
author: rick-anderson
description: "Microsoft Azure 可讓您輕鬆地建置和部署網站至生產環境。 但不是完成即時應用程式時，您剛開始使用 ！ 您..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3d24c633368abc14efcd9fcf200a4d05c5b182c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="998fa-105">在實驗室交給： 容易維護的 Azure 網站： 管理變更和小數位數</span><span class="sxs-lookup"><span data-stu-id="998fa-105">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="998fa-106">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="998fa-106">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="998fa-107">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="998fa-107">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="998fa-108">Microsoft Azure 可讓您輕鬆地建置和部署網站至生產環境。</span><span class="sxs-lookup"><span data-stu-id="998fa-108">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="998fa-109">但不是完成即時應用程式時，您剛開始使用 ！</span><span class="sxs-lookup"><span data-stu-id="998fa-109">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="998fa-110">您必須處理變更要求、 資料庫更新、 小數位數，等等。</span><span class="sxs-lookup"><span data-stu-id="998fa-110">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="998fa-111">幸運的是，Azure 應用程式服務，您已涵蓋、 具有足夠的功能，可協助您保持順暢執行您的站台。</span><span class="sxs-lookup"><span data-stu-id="998fa-111">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="998fa-112">Azure 提供安全又靈活的開發、 部署和擴充選項的任何大小的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-112">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="998fa-113">運用您現有的工具來建立及部署應用程式，省去管理基礎結構。</span><span class="sxs-lookup"><span data-stu-id="998fa-113">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="998fa-114">佈建的生產環境 web 應用程式自行以分鐘為單位輕鬆部署使用您最愛的開發工具建立的內容。</span><span class="sxs-lookup"><span data-stu-id="998fa-114">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="998fa-115">您可以部署現有的站台直接從原始檔控制，可以支援**Git**， **GitHub**， **Bitbucket**， **TFS**，甚至和**DropBox**。</span><span class="sxs-lookup"><span data-stu-id="998fa-115">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="998fa-116">直接從您喜愛的 IDE 或使用指令碼部署**PowerShell**在 Windows 或**CLI**任何作業系統上執行的工具。</span><span class="sxs-lookup"><span data-stu-id="998fa-116">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="998fa-117">一旦部署之後，追蹤您的站台的不斷地最新支援連續部署。</span><span class="sxs-lookup"><span data-stu-id="998fa-117">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="998fa-118">Azure 提供可擴充且持久的雲端儲存體、 備份及復原方案的任何資料，大或較小。</span><span class="sxs-lookup"><span data-stu-id="998fa-118">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="998fa-119">當部署到實際執行環境，例如資料表、 Blob 和 SQL 資料庫的儲存體服務的應用程式，讓您調整應用程式在雲端中。</span><span class="sxs-lookup"><span data-stu-id="998fa-119">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="998fa-120">SQL 資料庫是應用的您生產力資料庫保持最新部署程式的新版本時。</span><span class="sxs-lookup"><span data-stu-id="998fa-120">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="998fa-121">要感謝**Entity Framework Code First 移轉**，開發和部署您的資料模型已簡化，以更新您的環境，以分鐘為單位。</span><span class="sxs-lookup"><span data-stu-id="998fa-121">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="998fa-122">這個實際操作實驗室會顯示您的 web 應用程式部署至 Microsoft Azure 中的實際執行環境時，可能會遇到的不同主題。</span><span class="sxs-lookup"><span data-stu-id="998fa-122">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="998fa-123">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="998fa-123">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="998fa-124">如需本主題的深入涵蓋範圍，請參閱[建置真實世界雲端應用程式與 Azure 的電子書](building-real-world-cloud-apps-with-windows-azure/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="998fa-124">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="998fa-125">總覽</span><span class="sxs-lookup"><span data-stu-id="998fa-125">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="998fa-126">目標</span><span class="sxs-lookup"><span data-stu-id="998fa-126">Objectives</span></span>

<span data-ttu-id="998fa-127">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="998fa-127">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="998fa-128">啟用與現有模型的實體架構移轉</span><span class="sxs-lookup"><span data-stu-id="998fa-128">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="998fa-129">更新的物件模型和據以使用 Entity Framework 移轉資料庫</span><span class="sxs-lookup"><span data-stu-id="998fa-129">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="998fa-130">部署至 Azure App Service 使用 Git</span><span class="sxs-lookup"><span data-stu-id="998fa-130">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="998fa-131">回復成先前的部署使用 Azure 管理入口網站</span><span class="sxs-lookup"><span data-stu-id="998fa-131">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="998fa-132">使用 Azure 儲存體來調整 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-132">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="998fa-133">設定 web 應用程式中使用 Azure 管理入口網站的自動調整</span><span class="sxs-lookup"><span data-stu-id="998fa-133">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="998fa-134">建立和設定 Visual Studio 中的負載測試專案</span><span class="sxs-lookup"><span data-stu-id="998fa-134">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="998fa-135">必要條件</span><span class="sxs-lookup"><span data-stu-id="998fa-135">Prerequisites</span></span>

<span data-ttu-id="998fa-136">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="998fa-136">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="998fa-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="998fa-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="998fa-138">Azure SDK for .NET 2.2</span><span class="sxs-lookup"><span data-stu-id="998fa-138">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="998fa-139">GIT 版本控制系統</span><span class="sxs-lookup"><span data-stu-id="998fa-139">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="998fa-140">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="998fa-140">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="998fa-141">申請[免費試用版](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="998fa-141">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="998fa-142">如果您是 Visual Studio Professional、 Test Professional、 Premium 或 Ultimate 的 MSDN 或 MSDN 平台的訂閱者 」 時，啟用您[MSDN 權益](http://aka.ms/watk-msdn)現在要啟動開發和測試 Azure 上</span><span class="sxs-lookup"><span data-stu-id="998fa-142">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="998fa-143">[BizSpark](http://aka.ms/watk-bizspark)成員會自動收到 Azure 透過其 Visual Studio Ultimate with MSDN 訂用帳戶權益</span><span class="sxs-lookup"><span data-stu-id="998fa-143">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="998fa-144">成員[Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials 程式收到的免費每月 Azure 信用額度</span><span class="sxs-lookup"><span data-stu-id="998fa-144">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="998fa-145">安裝程式</span><span class="sxs-lookup"><span data-stu-id="998fa-145">Setup</span></span>

<span data-ttu-id="998fa-146">若要執行這個實際操作實驗室練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="998fa-146">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="998fa-147">開啟 Windows 檔案總管並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-147">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="998fa-148">以滑鼠右鍵按一下**Setup.cmd**選取**系統管理員身分執行**啟動安裝程序，將會設定您的環境，並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="998fa-148">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="998fa-149">如果顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="998fa-149">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="998fa-150">請確定執行安裝程式之前已在本實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="998fa-150">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="998fa-151">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="998fa-151">Using the Code Snippets</span></span>

<span data-ttu-id="998fa-152">在實驗室文件，您會指示要插入的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="998fa-152">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="998fa-153">為了方便起見，大部分的這段程式碼是依現狀 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，不必以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="998fa-153">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="998fa-154">每個練習會伴隨起始方案位於**開始**練習，可讓您依照每個練習各自的資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-154">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="998fa-155">請注意練習期間加入的程式碼片段遺失從中啟動方案，並可能無法運作，直到您已經完成本練習。</span><span class="sxs-lookup"><span data-stu-id="998fa-155">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="998fa-156">內部練習的原始程式碼，您也可以找到**結束**資料夾包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="998fa-156">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="998fa-157">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。</span><span class="sxs-lookup"><span data-stu-id="998fa-157">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="998fa-158">練習</span><span class="sxs-lookup"><span data-stu-id="998fa-158">Exercises</span></span>

<span data-ttu-id="998fa-159">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="998fa-159">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="998fa-160">使用 Entity Framework 移轉</span><span class="sxs-lookup"><span data-stu-id="998fa-160">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="998fa-161">Web 應用程式部署到預備環境</span><span class="sxs-lookup"><span data-stu-id="998fa-161">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="998fa-162">在生產環境中執行部署復原</span><span class="sxs-lookup"><span data-stu-id="998fa-162">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="998fa-163">使用 Azure 儲存體的縮放比例</span><span class="sxs-lookup"><span data-stu-id="998fa-163">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="998fa-164">[使用 Web 應用程式的自動調整規模](#Exercise5)Visual 的 Studio 2013 Ultimate edition （可省略）</span><span class="sxs-lookup"><span data-stu-id="998fa-164">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="998fa-165">完成本實驗室估計時間： **75 分鐘**</span><span class="sxs-lookup"><span data-stu-id="998fa-165">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="998fa-166">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="998fa-166">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="998fa-167">每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="998fa-167">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="998fa-168">在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="998fa-168">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="998fa-169">如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。</span><span class="sxs-lookup"><span data-stu-id="998fa-169">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="998fa-170">練習 1： 使用 Entity Framework 移轉</span><span class="sxs-lookup"><span data-stu-id="998fa-170">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="998fa-171">當您開發應用程式時，您的資料模型可能會隨時間變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-171">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="998fa-172">這些變更可能會影響您的資料庫中現有的模型，（如果您要建立新的版本），請務必讓資料庫保持在最新狀態，以避免發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="998fa-172">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="998fa-173">若要簡化在模型中，這些變更的追蹤**Entity Framework Code First 移轉**自動偵測比較您的模型資料庫結構描述變更，並會產生特定的程式碼以更新您的資料庫，建立新的*版本*您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-173">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="998fa-174">這個練習將示範如何啟用**移轉**有關您的應用程式，以及如何輕鬆地偵測和產生的變更來更新您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-174">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="998fa-175">工作 1 – 啟用移轉</span><span class="sxs-lookup"><span data-stu-id="998fa-175">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="998fa-176">在這項工作，您會瀏覽的步驟啟用**Entity Framework Code First 移轉**至**玩家測驗**資料庫變更的模型，以及了解如何將這些變更反映在資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-176">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="998fa-177">開啟 Visual Studio，並開啟**GeekQuiz.sln**方案檔從**Source\Ex1 UsingEntityFrameworkMigrations\Begin**。</span><span class="sxs-lookup"><span data-stu-id="998fa-177">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="998fa-178">建置此方案，以下載和安裝**NuGet**封裝的相依性。</span><span class="sxs-lookup"><span data-stu-id="998fa-178">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="998fa-179">若要這樣做，請以滑鼠右鍵按一下方案，然後按一下**建置方案**或按**Ctrl + Shift + B**。</span><span class="sxs-lookup"><span data-stu-id="998fa-179">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="998fa-180">從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="998fa-180">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="998fa-181">在**Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-181">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="998fa-182">將建立初始移轉現有的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="998fa-182">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="998fa-183">![啟用移轉](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "啟用移轉")</span><span class="sxs-lookup"><span data-stu-id="998fa-183">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="998fa-184">*啟用移轉*</span><span class="sxs-lookup"><span data-stu-id="998fa-184">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-185">此命令會新增**移轉**玩家測驗的專案包含檔案的資料夾稱為**configuration.cs 中**。</span><span class="sxs-lookup"><span data-stu-id="998fa-185">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="998fa-186">**組態**類別可讓您設定您的內容移轉的運作方式。</span><span class="sxs-lookup"><span data-stu-id="998fa-186">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="998fa-187">啟用移轉，您需要更新**組態**類別，以填入資料庫與初始資料，**玩家測驗**需要。</span><span class="sxs-lookup"><span data-stu-id="998fa-187">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="998fa-188">在下**移轉**，取代**configuration.cs 中**與一個檔案位於**Source\Assets**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-188">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-189">因為**移轉**會呼叫**種子**與每個資料庫的 update 方法，您必須確定記錄不會在資料庫中有重複。</span><span class="sxs-lookup"><span data-stu-id="998fa-189">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="998fa-190">**AddOrUpdate**方法可協助防止資料重複。</span><span class="sxs-lookup"><span data-stu-id="998fa-190">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="998fa-191">若要加入初始的移轉，輸入下列命令，然後按下**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-191">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-192">請確定沒有資料庫名為&quot;GeekQuizProd&quot; LocalDB 執行個體中。</span><span class="sxs-lookup"><span data-stu-id="998fa-192">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="998fa-193">![加入基底結構描述移轉](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "加入基底結構描述移轉")</span><span class="sxs-lookup"><span data-stu-id="998fa-193">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="998fa-194">*加入基底結構描述移轉*</span><span class="sxs-lookup"><span data-stu-id="998fa-194">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-195">**新增移轉**會為根據您的模型建立最後的移轉之後所做的變更的下一個移轉建立結構。</span><span class="sxs-lookup"><span data-stu-id="998fa-195">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="998fa-196">在此情況下，因為它是第一個專案的移轉，它會新增建立中定義的所有資料表的指令碼**TriviaContext**類別。</span><span class="sxs-lookup"><span data-stu-id="998fa-196">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="998fa-197">執行移轉至執行下列命令來更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-197">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="998fa-198">此命令會指定**Verbose**檢視要套用至目標資料庫的 SQL 陳述式的旗標。</span><span class="sxs-lookup"><span data-stu-id="998fa-198">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="998fa-199">![建立初始資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "建立初始資料庫")</span><span class="sxs-lookup"><span data-stu-id="998fa-199">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="998fa-200">*建立初始資料庫*</span><span class="sxs-lookup"><span data-stu-id="998fa-200">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-201">**更新資料庫**會將任何擱置中移轉套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-201">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="998fa-202">在此情況下，它會建立使用您的 web.config 檔案中定義的連接字串的資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-202">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="998fa-203">移至**檢視**功能表，並開啟**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="998fa-203">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="998fa-204">![在 SQL Server 物件總管 中開啟](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "在 SQL Server 物件總管 中開啟")</span><span class="sxs-lookup"><span data-stu-id="998fa-204">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="998fa-205">*在 SQL Server 物件總管 中開啟*</span><span class="sxs-lookup"><span data-stu-id="998fa-205">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="998fa-206">在**SQL Server 物件總管**視窗中，連接到 LocalDB 執行個體，以滑鼠右鍵按一下**SQL Server**節點，然後選取**加入 SQL Server...**選項。</span><span class="sxs-lookup"><span data-stu-id="998fa-206">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="998fa-207">![加入 SQL Server 執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "加入 SQL Server 執行個體")</span><span class="sxs-lookup"><span data-stu-id="998fa-207">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="998fa-208">*將 SQL Server 執行個體加入至 SQL Server 物件總管*</span><span class="sxs-lookup"><span data-stu-id="998fa-208">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="998fa-209">設定**伺服器名稱**至*(localdb) \v11.0*留下**Windows 驗證**當做驗證模式。</span><span class="sxs-lookup"><span data-stu-id="998fa-209">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="998fa-210">按一下**連接**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="998fa-210">Click **Connect** to continue.</span></span>

    <span data-ttu-id="998fa-211">![連接到 LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "連接到 LocalDB")</span><span class="sxs-lookup"><span data-stu-id="998fa-211">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="998fa-212">*連接到 LocalDB*</span><span class="sxs-lookup"><span data-stu-id="998fa-212">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="998fa-213">開啟**GeekQuizProd**資料庫中，展開 **資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="998fa-213">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="998fa-214">如您所見，**更新資料庫**命令產生中定義的所有資料表**TriviaContext**類別。</span><span class="sxs-lookup"><span data-stu-id="998fa-214">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="998fa-215">找出**dbo。TriviaQuestions**資料表，並開啟 [資料行] 節點。</span><span class="sxs-lookup"><span data-stu-id="998fa-215">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="998fa-216">在下一個工作中，您會將新的資料行加入至這個資料表，並更新資料庫使用**移轉**。</span><span class="sxs-lookup"><span data-stu-id="998fa-216">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="998fa-217">![瑣事問題的資料行](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "瑣事問題的資料行")</span><span class="sxs-lookup"><span data-stu-id="998fa-217">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="998fa-218">*瑣事問題的資料行*</span><span class="sxs-lookup"><span data-stu-id="998fa-218">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="998fa-219">工作 2 – 使用移轉更新的資料庫結構描述</span><span class="sxs-lookup"><span data-stu-id="998fa-219">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="998fa-220">在這個工作中，您將使用**Entity Framework Code First 移轉**偵測您的模型中的變更，並產生必要的程式碼，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="998fa-220">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="998fa-221">您將更新**TriviaQuestions**實體加入新的屬性。</span><span class="sxs-lookup"><span data-stu-id="998fa-221">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="998fa-222">然後，您將執行命令，以建立新的移轉，將新的資料行包含資料表中。</span><span class="sxs-lookup"><span data-stu-id="998fa-222">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="998fa-223">在**方案總管 中**，連按兩下**TriviaQuestion.cs**檔案位於**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-223">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="998fa-224">加入新的屬性，名為**提示**，如下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="998fa-224">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="998fa-225">在**Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-225">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="998fa-226">將反映在我們的模型變更會建立新的移轉。</span><span class="sxs-lookup"><span data-stu-id="998fa-226">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="998fa-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span><span class="sxs-lookup"><span data-stu-id="998fa-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="998fa-228">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="998fa-228">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-229">移轉檔案由兩個方法，組成**向上**和**向**。</span><span class="sxs-lookup"><span data-stu-id="998fa-229">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="998fa-230">**向上**方法將會用來指定哪些變更我們應用程式需要在套用到資料庫的目前版本。</span><span class="sxs-lookup"><span data-stu-id="998fa-230">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="998fa-231">**向**用來回復的變更，我們加入**向上**方法。</span><span class="sxs-lookup"><span data-stu-id="998fa-231">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="998fa-232">當資料庫移轉更新的資料庫時，它會執行所有移轉的時間戳記順序，以及只包括上次更新之後尚未使用中 ( \_MigrationHistory 資料表會追蹤的哪些移轉已經套用)。</span><span class="sxs-lookup"><span data-stu-id="998fa-232">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="998fa-233">**向上**將呼叫的所有移轉的方法，它會讓我們指定資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-233">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="998fa-234">如果我們決定要回到先前的移轉，**向**將重做的變更，以反向順序呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="998fa-234">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="998fa-235">在**Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-235">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="998fa-236">輸出**更新資料庫**命令產生**Alter Table** SQL 陳述式來加入新的資料行， **TriviaQuestions**資料表，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="998fa-236">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="998fa-237">![加入資料行的 SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "加入資料行產生的 SQL 陳述式")</span><span class="sxs-lookup"><span data-stu-id="998fa-237">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="998fa-238">*加入資料行產生的 SQL 陳述式*</span><span class="sxs-lookup"><span data-stu-id="998fa-238">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="998fa-239">在**SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並檢查新**提示**都會顯示資料行。</span><span class="sxs-lookup"><span data-stu-id="998fa-239">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="998fa-240">![顯示新的資料行提示](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "顯示新的提示資料行")</span><span class="sxs-lookup"><span data-stu-id="998fa-240">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="998fa-241">*顯示新的提示資料行*</span><span class="sxs-lookup"><span data-stu-id="998fa-241">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="998fa-242">回到**TriviaQuestion.cs**編輯器中，加入**StringLength**條件約束*提示*屬性，如下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="998fa-242">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="998fa-243">在**Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-243">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="998fa-244">在**Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="998fa-244">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="998fa-245">輸出**更新資料庫**命令產生**Alter Table** SQL 陳述式來更新*提示*類型的資料行**TriviaQuestions**資料表，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="998fa-245">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="998fa-246">![Alter column SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter 資料行產生的 SQL 陳述式")</span><span class="sxs-lookup"><span data-stu-id="998fa-246">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="998fa-247">*Alter column SQL 陳述式產生*</span><span class="sxs-lookup"><span data-stu-id="998fa-247">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="998fa-248">在**SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並檢查**提示**資料行類型是**nvarchar(150)**。</span><span class="sxs-lookup"><span data-stu-id="998fa-248">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="998fa-249">![顯示新的條件約束](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "顯示新的條件約束")</span><span class="sxs-lookup"><span data-stu-id="998fa-249">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="998fa-250">*顯示新的條件約束*</span><span class="sxs-lookup"><span data-stu-id="998fa-250">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="998fa-251">練習 2： 將 Web 應用程式部署至預備環境</span><span class="sxs-lookup"><span data-stu-id="998fa-251">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="998fa-252">**Web 應用程式在 Azure App Service 中的**可讓您執行預備的發行。</span><span class="sxs-lookup"><span data-stu-id="998fa-252">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="998fa-253">預備的發行建立預備網站位置的每個預設生產網站，並可讓您交換這些位置不需要停機。</span><span class="sxs-lookup"><span data-stu-id="998fa-253">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="998fa-254">這是真的適用於公開釋放之前，先驗證變更，以累加方式整合網站內容和回復的變更都無法如預期運作。</span><span class="sxs-lookup"><span data-stu-id="998fa-254">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="998fa-255">在此練習中，您將部署**玩家測驗**到預備環境使用 Git 原始檔控制的 web 應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-255">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="998fa-256">若要這樣做，您會建立 web 應用程式和佈建在管理入口網站的必要的元件，請設定**Git**儲存機制並推送應用程式原始程式碼，從本機電腦以預備位置。</span><span class="sxs-lookup"><span data-stu-id="998fa-256">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="998fa-257">您也會更新您的生產資料庫與**Code First 移轉**您在上一個練習中建立。</span><span class="sxs-lookup"><span data-stu-id="998fa-257">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="998fa-258">您再將此測試環境，以驗證其作業中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-258">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="998fa-259">一旦您滿意其根據您的預期運作，您將會升級到生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-259">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="998fa-260">若要啟用預備的發行，web 應用程式必須處於**標準模式**。</span><span class="sxs-lookup"><span data-stu-id="998fa-260">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="998fa-261">請注意，是否您變更為標準模式的 web 應用程式，將會產生額外費用。</span><span class="sxs-lookup"><span data-stu-id="998fa-261">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="998fa-262">如需有關定價的詳細資訊，請參閱[應用程式服務定價](https://azure.microsoft.com/pricing/details/app-service/)。</span><span class="sxs-lookup"><span data-stu-id="998fa-262">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="998fa-263">工作 1-在 Azure App Service 中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-263">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="998fa-264">在這個工作中，您將建立 web 應用程式中的**Azure App Service**從管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-264">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="998fa-265">您也會設定**SQL Database**可持續的應用程式資料，以及設定原始檔控制的本機 Git 儲存機制。</span><span class="sxs-lookup"><span data-stu-id="998fa-265">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="998fa-266">移至[Azure 管理入口網站](https://manage.windowsazure.com)並使用與您訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-266">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![登入 Azure 管理入口網站](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="998fa-268">*登入 Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="998fa-268">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="998fa-269">按一下**新增**在頁面底部命令列中。</span><span class="sxs-lookup"><span data-stu-id="998fa-269">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="998fa-270">![建立新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "建立新的 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="998fa-270">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="998fa-271">*建立新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-271">*Creating a new web app*</span></span>
3. <span data-ttu-id="998fa-272">按一下**計算**，**網站**然後**自訂建立**。</span><span class="sxs-lookup"><span data-stu-id="998fa-272">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="998fa-273">![建立新的 web 應用程式，使用 自訂建立](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "建立新的 web 應用程式，使用 自訂建立")</span><span class="sxs-lookup"><span data-stu-id="998fa-273">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="998fa-274">*建立新的 web 應用程式，使用 自訂建立*</span><span class="sxs-lookup"><span data-stu-id="998fa-274">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="998fa-275">在**新增網站-自訂建立**對話方塊方塊中，提供可用**URL** (例如*玩家測驗*)，選取的位置**區域**下拉式清單並選取**建立新的 SQL database**中**資料庫**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="998fa-275">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="998fa-276">最後，選取**從原始檔控制發行**核取方塊，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-276">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![自訂新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="998fa-278">*自訂新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-278">*Customizing the new web app*</span></span>
5. <span data-ttu-id="998fa-279">指定的資料庫設定的下列資訊：</span><span class="sxs-lookup"><span data-stu-id="998fa-279">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="998fa-280">在**名稱**文字中，輸入資料庫名稱 (例如*geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="998fa-280">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="998fa-281">在伺服器中**下拉式**清單中，選取**新 SQL database 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="998fa-281">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="998fa-282">或者，您可以選取現有的伺服器。</span><span class="sxs-lookup"><span data-stu-id="998fa-282">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="998fa-283">在**資料庫使用者名稱**和**資料庫密碼**方塊中，輸入 SQL 資料庫伺服器的系統管理員使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="998fa-283">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="998fa-284">如果您選取的伺服器已經建立，系統將提示您輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="998fa-284">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![指定的資料庫設定](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="998fa-286">*指定的資料庫設定*</span><span class="sxs-lookup"><span data-stu-id="998fa-286">*Specifying the database settings*</span></span>
6. <span data-ttu-id="998fa-287">按 [下一步]  以繼續。</span><span class="sxs-lookup"><span data-stu-id="998fa-287">Click **Next** to continue.</span></span>
7. <span data-ttu-id="998fa-288">選取**本機 Git 儲存機制**原始檔控制，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-288">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-289">您可能會提示您的部署認證 （使用者名稱和密碼）。</span><span class="sxs-lookup"><span data-stu-id="998fa-289">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![建立 Git 儲存機制](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="998fa-291">*建立 Git 儲存機制*</span><span class="sxs-lookup"><span data-stu-id="998fa-291">*Creating the Git repository*</span></span>
8. <span data-ttu-id="998fa-292">請等到建立新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-292">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-293">根據預設，Azure 會提供在網域*.azurewebsites.net*但也讓您設定使用 Azure 管理入口網站的自訂網域的可能性。</span><span class="sxs-lookup"><span data-stu-id="998fa-293">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="998fa-294">不過，您只可以管理自訂網域，如果您使用特定的 Azure 應用程式服務模式。</span><span class="sxs-lookup"><span data-stu-id="998fa-294">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="998fa-295">Azure App Service 是免費、 共用、 Basic、 Standard 和 Premium edition 提供。</span><span class="sxs-lookup"><span data-stu-id="998fa-295">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="998fa-296">在免費和共用模式中，所有的 web 應用程式在多租用戶環境中執行，並可 CPU、 記憶體和網路使用量配額。</span><span class="sxs-lookup"><span data-stu-id="998fa-296">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="998fa-297">可用的應用程式的最大數目可能會隨您的計劃。</span><span class="sxs-lookup"><span data-stu-id="998fa-297">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="998fa-298">在標準模式中，您可以選擇哪些應用程式對應的專用虛擬機器上執行標準的 azure 計算資源。</span><span class="sxs-lookup"><span data-stu-id="998fa-298">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="998fa-299">您可以找到中的 web 應用程式模式組態**標尺**web 應用程式的功能表。</span><span class="sxs-lookup"><span data-stu-id="998fa-299">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="998fa-300">![Azure App Service 模式](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service 模式")</span><span class="sxs-lookup"><span data-stu-id="998fa-300">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="998fa-301">如果您使用**共用**或**標準**模式中，您將能夠管理 web 應用程式的自訂網域，請前往您的應用程式**設定**功能表，按一下**管理網域**下*網域名稱*。</span><span class="sxs-lookup"><span data-stu-id="998fa-301">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="998fa-302">![管理網域](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "管理網域")</span><span class="sxs-lookup"><span data-stu-id="998fa-302">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="998fa-303">![管理自訂網域](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "管理自訂網域")</span><span class="sxs-lookup"><span data-stu-id="998fa-303">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="998fa-304">Web 應用程式建立之後，請按一下下方的連結**URL**檢查新的 web 應用程式是否正在執行的資料行。</span><span class="sxs-lookup"><span data-stu-id="998fa-304">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![瀏覽至新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="998fa-306">*瀏覽至新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-306">*Browsing to the new web app*</span></span>

    ![執行 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="998fa-308">*執行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-308">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="998fa-309">工作 2 – 建立實際執行的 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="998fa-309">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="998fa-310">在這個工作中，您將使用**Entity Framework Code First 移轉**來建立資料庫的目標**Azure SQL Database**您在前一項工作中建立的執行個體。</span><span class="sxs-lookup"><span data-stu-id="998fa-310">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="998fa-311">在管理入口網站中，瀏覽至您在前一項工作中建立 web 應用程式，並移至其**儀表板**。</span><span class="sxs-lookup"><span data-stu-id="998fa-311">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="998fa-312">在**儀表板**頁面上，按一下**檢視連接字串**連結底下**快速概覽**> 一節。</span><span class="sxs-lookup"><span data-stu-id="998fa-312">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="998fa-313">![檢視連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "檢視連接字串")</span><span class="sxs-lookup"><span data-stu-id="998fa-313">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="998fa-314">*檢視連接字串*</span><span class="sxs-lookup"><span data-stu-id="998fa-314">*View connection strings*</span></span>
3. <span data-ttu-id="998fa-315">複製**連接字串**值，並關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="998fa-315">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="998fa-316">![在 Azure 管理入口網站中的連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure 管理入口網站中的連接字串")</span><span class="sxs-lookup"><span data-stu-id="998fa-316">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="998fa-317">*在 Azure 管理入口網站中的連接字串*</span><span class="sxs-lookup"><span data-stu-id="998fa-317">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="998fa-318">按一下**SQL 資料庫**若要查看 Azure 中的 SQL 資料庫的清單</span><span class="sxs-lookup"><span data-stu-id="998fa-318">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="998fa-319">![SQL Database 功能表](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database 功能表")</span><span class="sxs-lookup"><span data-stu-id="998fa-319">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="998fa-320">*SQL Database 功能表*</span><span class="sxs-lookup"><span data-stu-id="998fa-320">*SQL Database menu*</span></span>
5. <span data-ttu-id="998fa-321">找出您在前一項工作中建立的資料庫，並在伺服器上按一下。</span><span class="sxs-lookup"><span data-stu-id="998fa-321">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="998fa-322">![SQL Database 伺服器](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database 伺服器")</span><span class="sxs-lookup"><span data-stu-id="998fa-322">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="998fa-323">*SQL Database 伺服器*</span><span class="sxs-lookup"><span data-stu-id="998fa-323">*SQL Database server*</span></span>
6. <span data-ttu-id="998fa-324">在**快速入門**網頁伺服器上，按一下**設定**。</span><span class="sxs-lookup"><span data-stu-id="998fa-324">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="998fa-325">![設定功能表](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "設定功能表")</span><span class="sxs-lookup"><span data-stu-id="998fa-325">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="998fa-326">*設定功能表*</span><span class="sxs-lookup"><span data-stu-id="998fa-326">*Configure menu*</span></span>
7. <span data-ttu-id="998fa-327">在**允許的 IP 位址**區段中，按一下**加入至允許的 IP 位址**連結讓您連接到 SQL Database 伺服器的 IP。</span><span class="sxs-lookup"><span data-stu-id="998fa-327">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="998fa-328">![允許的 IP 位址](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "允許的 IP 位址")</span><span class="sxs-lookup"><span data-stu-id="998fa-328">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="998fa-329">*允許的 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="998fa-329">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="998fa-330">按一下**儲存**底部的頁面，即可完成步驟。</span><span class="sxs-lookup"><span data-stu-id="998fa-330">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="998fa-331">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="998fa-331">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="998fa-332">在**Package Manager Console**，執行下列命令取代*[您的連接字串的]*預留位置您從 Azure 複製連接字串</span><span class="sxs-lookup"><span data-stu-id="998fa-332">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="998fa-333">![更新目標 Windows Azure SQL Database 的資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "更新目標 Windows Azure SQL Database 的資料庫")</span><span class="sxs-lookup"><span data-stu-id="998fa-333">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="998fa-334">*更新目標 Azure SQL Database 的資料庫*</span><span class="sxs-lookup"><span data-stu-id="998fa-334">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="998fa-335">工作 3-部署到預備環境使用 Git 的玩家測驗</span><span class="sxs-lookup"><span data-stu-id="998fa-335">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="998fa-336">在這項工作，您將 web 應用程式中啟用預備的發行。</span><span class="sxs-lookup"><span data-stu-id="998fa-336">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="998fa-337">接著，您將使用 Git 發行玩家測驗應用程式直接從本機電腦到您的 web 應用程式的預備環境。</span><span class="sxs-lookup"><span data-stu-id="998fa-337">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="998fa-338">返回入口網站並按一下底下的 web 應用程式名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-338">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![開啟 web 應用程式管理頁面](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="998fa-340">*開啟 web 應用程式管理頁面*</span><span class="sxs-lookup"><span data-stu-id="998fa-340">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="998fa-341">瀏覽至**標尺**頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-341">Navigate to the **Scale** page.</span></span> <span data-ttu-id="998fa-342">在下**一般**區段中，選取**標準**的設定，然後按一下**儲存**命令列中。</span><span class="sxs-lookup"><span data-stu-id="998fa-342">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-343">若要執行中的目前地區和訂用帳戶中的所有 web 應用程式**標準**模式中，保留**全選**核取方塊中選取**選擇網站**組態。</span><span class="sxs-lookup"><span data-stu-id="998fa-343">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="998fa-344">否則，請清除**全選**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="998fa-344">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="998fa-345">![Web 應用程式升級為標準模式](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "升級為標準模式的 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="998fa-345">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="998fa-346">*升級為標準模式的 Web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-346">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="998fa-347">按一下**是**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-347">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="998fa-348">![確認變更為標準模式](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "繼續進行變更的 web 應用程式模式")</span><span class="sxs-lookup"><span data-stu-id="998fa-348">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="998fa-349">*確認變更為標準模式*</span><span class="sxs-lookup"><span data-stu-id="998fa-349">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="998fa-350">移至**儀表板**頁面上，按一下 **啟用分段發行**下**快速概覽**> 一節。</span><span class="sxs-lookup"><span data-stu-id="998fa-350">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="998fa-351">![啟用預備的發行](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "啟用預備發行")</span><span class="sxs-lookup"><span data-stu-id="998fa-351">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="998fa-352">*啟用預備的發行*</span><span class="sxs-lookup"><span data-stu-id="998fa-352">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="998fa-353">按一下**是**才能啟用預備的發行。</span><span class="sxs-lookup"><span data-stu-id="998fa-353">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="998fa-354">![確認暫存發行](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "按一下 [是] 才能啟用預備的發行")</span><span class="sxs-lookup"><span data-stu-id="998fa-354">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="998fa-355">*確認預備的發行*</span><span class="sxs-lookup"><span data-stu-id="998fa-355">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="998fa-356">在 web 應用程式清單中，展開左邊的 web 應用程式名稱以顯示預備網站位置的標記。</span><span class="sxs-lookup"><span data-stu-id="998fa-356">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="998fa-357">您的 web 應用程式，後面接著名稱***（預備）***。</span><span class="sxs-lookup"><span data-stu-id="998fa-357">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="998fa-358">按一下以移至 [管理] 頁面的預備網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-358">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="998fa-359">![瀏覽至預備環境的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "瀏覽至預備環境的 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="998fa-359">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="998fa-360">*瀏覽至預備環境的應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-360">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="998fa-361">請注意，他管理頁面看起來像任何其他 web 應用程式的管理頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-361">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="998fa-362">瀏覽至**部署**頁面，並複製**Git URL**值。</span><span class="sxs-lookup"><span data-stu-id="998fa-362">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="998fa-363">您可以將稍後在這個練習中。</span><span class="sxs-lookup"><span data-stu-id="998fa-363">You will use it later in this exercise.</span></span>

    ![複製的 Git URL 值](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="998fa-365">*複製的 Git URL 值*</span><span class="sxs-lookup"><span data-stu-id="998fa-365">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="998fa-366">開啟新**Git Bash**主控台，然後執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="998fa-366">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="998fa-367">更新*[您的應用程式的路徑]*預留位置路徑**GeekQuiz**方案中，位於**Source\Ex1 DeployingWebSiteToStaging\Begin**資料夾這個實驗室中。</span><span class="sxs-lookup"><span data-stu-id="998fa-367">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git 初始化和第一次認可](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="998fa-369">*Git 初始化和第一次認可*</span><span class="sxs-lookup"><span data-stu-id="998fa-369">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="998fa-370">執行下列命令，以將您的 web 應用程式推送至遠端**Git**儲存機制。</span><span class="sxs-lookup"><span data-stu-id="998fa-370">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="998fa-371">預留位置取代為您從管理入口網站取得的 URL。</span><span class="sxs-lookup"><span data-stu-id="998fa-371">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="998fa-372">系統會提示您做為部署密碼。</span><span class="sxs-lookup"><span data-stu-id="998fa-372">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![推入至 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="998fa-374">*推入至 Azure*</span><span class="sxs-lookup"><span data-stu-id="998fa-374">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-375">當您將內容部署到 FTP 主機或 GIT 儲存機制的 web 應用程式時，您必須驗證使用**部署認證**您從 web 應用程式的建立**快速入門**或**儀表板**管理頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-375">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="998fa-376">如果您不知道您的部署認證您可以輕鬆地加以重設使用管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-376">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="998fa-377">開啟 web 應用程式**儀表板**頁面上，按一下 **重設部署認證**連結。</span><span class="sxs-lookup"><span data-stu-id="998fa-377">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="998fa-378">提供新密碼，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="998fa-378">Provide a new password and click **OK**.</span></span> <span data-ttu-id="998fa-379">是部署認證都適用於使用與您訂用帳戶相關聯的所有 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-379">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="998fa-380">若要驗證 web 應用程式已成功推入至 Azure，請前往管理入口網站，然後按一下**網站**。</span><span class="sxs-lookup"><span data-stu-id="998fa-380">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="998fa-381">找出您的 web 應用程式，然後展開以顯示預備網站位置的項目。</span><span class="sxs-lookup"><span data-stu-id="998fa-381">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="998fa-382">按一下其**名稱**移至 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-382">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="998fa-383">按一下**部署**查看**部署歷程記錄**。</span><span class="sxs-lookup"><span data-stu-id="998fa-383">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="998fa-384">請確認有**現用部署**與您*&quot;初始認可&quot;*。</span><span class="sxs-lookup"><span data-stu-id="998fa-384">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![作用中的部署](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="998fa-386">*作用中的部署*</span><span class="sxs-lookup"><span data-stu-id="998fa-386">*Active deployment*</span></span>
13. <span data-ttu-id="998fa-387">最後，按一下 **瀏覽**移至 web 應用程式的命令列中。</span><span class="sxs-lookup"><span data-stu-id="998fa-387">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![瀏覽 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="998fa-389">*瀏覽 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-389">*Browse web app*</span></span>
14. <span data-ttu-id="998fa-390">如果已成功部署應用程式，您會看到一些測驗登入頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-390">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-391">所部署的應用程式的 URL 包含您的 web 應用程式，後面接著名稱*-暫存*。</span><span class="sxs-lookup"><span data-stu-id="998fa-391">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![預備環境中執行應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="998fa-393">*預備環境中執行應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-393">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="998fa-394">如果您想要瀏覽應用程式，按一下**註冊**註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="998fa-394">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="998fa-395">輸入使用者名稱、 電子郵件地址和密碼，以完成帳戶詳細資料。</span><span class="sxs-lookup"><span data-stu-id="998fa-395">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="998fa-396">接下來，應用程式顯示測驗的第一個問題。</span><span class="sxs-lookup"><span data-stu-id="998fa-396">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="998fa-397">回答幾個問題，請確定它正常運作。</span><span class="sxs-lookup"><span data-stu-id="998fa-397">Answer a few questions to make sure it is working as expected.</span></span>

    ![可供使用的應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="998fa-399">*可供使用的應用程式*</span><span class="sxs-lookup"><span data-stu-id="998fa-399">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="998fa-400">工作 4-升級至生產環境 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-400">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="998fa-401">現在您已驗證的 web 應用程式正常運作，預備環境中，您就準備好要將其升階到生產環境。</span><span class="sxs-lookup"><span data-stu-id="998fa-401">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="998fa-402">在這項工作，您將會交換預備與生產網站位置的站台位置。</span><span class="sxs-lookup"><span data-stu-id="998fa-402">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="998fa-403">返回管理入口網站，並選取 預備網站位置。</span><span class="sxs-lookup"><span data-stu-id="998fa-403">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="998fa-404">按一下**交換**命令列中。</span><span class="sxs-lookup"><span data-stu-id="998fa-404">Click **Swap** in the command bar.</span></span>

    ![交換到生產環境](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="998fa-406">*交換到生產環境*</span><span class="sxs-lookup"><span data-stu-id="998fa-406">*Swap to production*</span></span>
2. <span data-ttu-id="998fa-407">按一下**是**在確認對話方塊方塊中，才能繼續執行交換操作。</span><span class="sxs-lookup"><span data-stu-id="998fa-407">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="998fa-408">Azure 會立即在交換預備網站的內容將生產站台的內容。</span><span class="sxs-lookup"><span data-stu-id="998fa-408">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-409">從暫存版本的某些設定將會自動複製 （例如連接字串覆寫、 處理常式對應等） 的實際執行版本，但其他設定不會變更 （例如 DNS 端點、 SSL 繫結等）。</span><span class="sxs-lookup"><span data-stu-id="998fa-409">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![確認交換作業](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="998fa-411">*確認交換作業*</span><span class="sxs-lookup"><span data-stu-id="998fa-411">*Confirming swap operation*</span></span>
3. <span data-ttu-id="998fa-412">交換完成之後，請選取 生產位置，然後按一下**瀏覽**開啟將生產站台的命令列中。</span><span class="sxs-lookup"><span data-stu-id="998fa-412">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="998fa-413">請注意網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="998fa-413">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-414">您可能需要重新整理瀏覽器，以清除快取。</span><span class="sxs-lookup"><span data-stu-id="998fa-414">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="998fa-415">在 Internet Explorer 中，您可以藉由按下**CTRL + R**。</span><span class="sxs-lookup"><span data-stu-id="998fa-415">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![在實際執行環境中執行的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="998fa-417">在**GitBash**主控台中，更新目標生產位置的本機 Git 儲存機制的遠端 URL。</span><span class="sxs-lookup"><span data-stu-id="998fa-417">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="998fa-418">若要這樣做，請執行下列命令預留位置取代為您部署的使用者名稱和您的 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="998fa-418">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-419">在下列練習中，您會將變更推送到生產網站，而不是只能用來簡化實驗室的預備環境。</span><span class="sxs-lookup"><span data-stu-id="998fa-419">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="998fa-420">在真實世界案例中，建議升級至生產環境之前，先驗證預備環境中的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-420">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="998fa-421">練習 3： 在生產環境中執行部署復原</span><span class="sxs-lookup"><span data-stu-id="998fa-421">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="998fa-422">其中您沒有預備位置來執行熱交換之間預備和生產環境，例如，如果您正在使用的情況下**免費**或**共用**模式。</span><span class="sxs-lookup"><span data-stu-id="998fa-422">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="998fa-423">在這些情況下，您應該測試應用程式在測試環境中 （本機或遠端網站中） 部署到生產環境之前。</span><span class="sxs-lookup"><span data-stu-id="998fa-423">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="998fa-424">不過，很可能在測試階段期間未偵測到的問題可能會發生在生產網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-424">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="998fa-425">在此情況下，它是一定要有一個機制，輕鬆地儘快切換至應用程式的上一個且更穩定版本。</span><span class="sxs-lookup"><span data-stu-id="998fa-425">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="998fa-426">在**Azure App Service**，從原始檔控制的連續部署可讓此可能有**重新部署**管理入口網站中可用的動作。</span><span class="sxs-lookup"><span data-stu-id="998fa-426">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="998fa-427">Azure 會追蹤在認可推送到儲存機制相關聯的部署，並提供選項，以重新部署應用程式使用任何先前的部署中，在任何時間。</span><span class="sxs-lookup"><span data-stu-id="998fa-427">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="998fa-428">在這個練習中，您將執行中的程式碼變更**玩家測驗**刻意插入的應用程式*bug*。</span><span class="sxs-lookup"><span data-stu-id="998fa-428">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="998fa-429">您將部署到生產環境以查看錯誤，應用程式，然後您會利用重新部署功能，請回到先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="998fa-429">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="998fa-430">工作 1-更新玩家受測應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-430">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="998fa-431">在這項工作，您將會重構一小段程式碼**TriviaController**類別來擷取從資料庫擷取選取的測驗選項，為新方法的邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="998fa-431">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="998fa-432">切換至 Visual Studio 執行個體與**GeekQuiz**在上一個作業中的方案。</span><span class="sxs-lookup"><span data-stu-id="998fa-432">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="998fa-433">在**方案總管 中**，開啟**TriviaController.cs**檔案內**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-433">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="998fa-434">找出**StoreAsync**方法，然後選取程式碼以在下圖中反白顯示。</span><span class="sxs-lookup"><span data-stu-id="998fa-434">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![選取程式碼](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="998fa-436">*選取程式碼*</span><span class="sxs-lookup"><span data-stu-id="998fa-436">*Selecting the code*</span></span>
4. <span data-ttu-id="998fa-437">以滑鼠右鍵按一下選取的程式碼中，展開**重構**功能表，然後選取**擷取方法...**.</span><span class="sxs-lookup"><span data-stu-id="998fa-437">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![擷取程式碼做為新方法](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="998fa-439">*選取擷取方法*</span><span class="sxs-lookup"><span data-stu-id="998fa-439">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="998fa-440">在**擷取方法**對話方塊中，新的方法名稱*MatchesOption*按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="998fa-440">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![指定方法名稱](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="998fa-442">*指定擷取方法的名稱*</span><span class="sxs-lookup"><span data-stu-id="998fa-442">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="998fa-443">選取的程式碼接著會擷取到**MatchesOption**方法。</span><span class="sxs-lookup"><span data-stu-id="998fa-443">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="998fa-444">產生的程式碼是以下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="998fa-444">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="998fa-445">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-445">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="998fa-446">工作 2 – 重新部署玩家受測應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-446">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="998fa-447">您現在會將推送您對先前工作中的儲存機制將會觸發新的部署到生產環境的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-447">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="998fa-448">然後，您將使用問題使用**F12 開發工具**由 Internet Explorer 中，提供，然後執行回復至先前的部署從 Azure 管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-448">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="998fa-449">開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-449">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="998fa-450">執行下列命令以將變更推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="998fa-450">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="998fa-451">更新*[您的應用程式的路徑]*預留位置路徑**GeekQuiz**方案。</span><span class="sxs-lookup"><span data-stu-id="998fa-451">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="998fa-452">系統會提示您做為部署密碼。</span><span class="sxs-lookup"><span data-stu-id="998fa-452">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![將重構的程式碼推送至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="998fa-454">*將重構的程式碼推送至 Azure*</span><span class="sxs-lookup"><span data-stu-id="998fa-454">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="998fa-455">開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="998fa-455">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="998fa-456">使用先前建立的認證登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-456">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="998fa-457">按**F12**若要啟動開發工具，請選取**網路**索引標籤上，按一下 [**播放**開始錄製] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="998fa-457">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="998fa-458">![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "啟動網路錄製")</span><span class="sxs-lookup"><span data-stu-id="998fa-458">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="998fa-459">*啟動網路錄製*</span><span class="sxs-lookup"><span data-stu-id="998fa-459">*Starting network recording*</span></span>
5. <span data-ttu-id="998fa-460">選取測驗的任何選項。</span><span class="sxs-lookup"><span data-stu-id="998fa-460">Select any option of the quiz.</span></span> <span data-ttu-id="998fa-461">您會看到沒有任何反應。</span><span class="sxs-lookup"><span data-stu-id="998fa-461">You will see that nothing happens.</span></span>
6. <span data-ttu-id="998fa-462">在**F12**視窗中，對應至 POST HTTP 要求的項目會顯示 HTTP **500**結果。</span><span class="sxs-lookup"><span data-stu-id="998fa-462">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 error](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="998fa-464">*HTTP 500 error*</span><span class="sxs-lookup"><span data-stu-id="998fa-464">*HTTP 500 error*</span></span>
7. <span data-ttu-id="998fa-465">選取**主控台** 索引標籤。將錯誤記錄的原因詳細資料。</span><span class="sxs-lookup"><span data-stu-id="998fa-465">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![記錄的錯誤](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="998fa-467">*記錄的錯誤*</span><span class="sxs-lookup"><span data-stu-id="998fa-467">*Logged error*</span></span>
8. <span data-ttu-id="998fa-468">找出錯誤的詳細資料部分。</span><span class="sxs-lookup"><span data-stu-id="998fa-468">Locate the details part of the error.</span></span> <span data-ttu-id="998fa-469">很明顯地，這個錯誤是重構前一個步驟中認可您的程式碼所造成。</span><span class="sxs-lookup"><span data-stu-id="998fa-469">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="998fa-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="998fa-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="998fa-471">請勿關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="998fa-471">Do not close the browser.</span></span>
10. <span data-ttu-id="998fa-472">在新的瀏覽器執行個體中，瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用與您訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-472">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="998fa-473">選取**網站**按一下您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-473">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="998fa-474">瀏覽至**部署**頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-474">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="998fa-475">請注意，執行的所有認可會列在 部署歷程記錄。</span><span class="sxs-lookup"><span data-stu-id="998fa-475">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![現有的部署清單](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="998fa-477">*現有的部署清單*</span><span class="sxs-lookup"><span data-stu-id="998fa-477">*List of existing deployments*</span></span>
13. <span data-ttu-id="998fa-478">選取 上一次認可，然後按一下**重新部署**命令列上。</span><span class="sxs-lookup"><span data-stu-id="998fa-478">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![上一次認可重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="998fa-480">*上一次認可重新部署*</span><span class="sxs-lookup"><span data-stu-id="998fa-480">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="998fa-481">當提示確認，請按一下 **是**。</span><span class="sxs-lookup"><span data-stu-id="998fa-481">When prompted to confirm, click **Yes**.</span></span>

    ![確認重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="998fa-483">完成部署之後，切換回瀏覽器執行個體與您的 web 應用程式按**CTRL + F5**。</span><span class="sxs-lookup"><span data-stu-id="998fa-483">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="998fa-484">按一下任何一個選項。</span><span class="sxs-lookup"><span data-stu-id="998fa-484">Click any of the options.</span></span> <span data-ttu-id="998fa-485">位置和結果，將立即帶翻轉動畫 (*正確/不正確*) 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="998fa-485">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="998fa-486">（選擇性）切換至**Git Bash**主控台，然後執行下列命令，還原成先前的認可。</span><span class="sxs-lookup"><span data-stu-id="998fa-486">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-487">這些命令會建立新的認可，復原將 Git 儲存機制中不正確的認可中所做的所有變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-487">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="998fa-488">Azure 會再重新部署應用程式使用新的認可。</span><span class="sxs-lookup"><span data-stu-id="998fa-488">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="998fa-489">練習 4： 可讓您調整使用 Azure 儲存體</span><span class="sxs-lookup"><span data-stu-id="998fa-489">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="998fa-490">**Blob**是最簡單的方式來儲存大量非結構化的文字或二進位資料，例如視訊、 音訊和影像。</span><span class="sxs-lookup"><span data-stu-id="998fa-490">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="998fa-491">移至儲存體的應用程式的靜態內容，有助於藉由提供影像或文件，以瀏覽器直接調整您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-491">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="998fa-492">在此練習中，您會將移到 Blob 容器的應用程式的靜態內容。</span><span class="sxs-lookup"><span data-stu-id="998fa-492">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="998fa-493">然後您將設定您的應用程式加入**ASP.NET URL 重寫規則**中**Web.config**重新導向至 Blob 容器的內容。</span><span class="sxs-lookup"><span data-stu-id="998fa-493">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="998fa-494">工作 1 – 建立 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="998fa-494">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="998fa-495">在這項工作中，您將學習如何建立新的儲存體帳戶，使用管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="998fa-495">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="998fa-496">瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用與您訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-496">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="998fa-497">選取**新 |資料服務 |儲存體 |快速建立**開始建立新的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="998fa-497">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="998fa-498">輸入唯一的名稱做為帳戶選取**區域**從清單中。</span><span class="sxs-lookup"><span data-stu-id="998fa-498">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="998fa-499">按一下**建立儲存體帳戶**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="998fa-499">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="998fa-500">![建立新的儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "建立新的儲存體帳戶")</span><span class="sxs-lookup"><span data-stu-id="998fa-500">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="998fa-501">*建立新的儲存體帳戶*</span><span class="sxs-lookup"><span data-stu-id="998fa-501">*Creating a new storage account*</span></span>
3. <span data-ttu-id="998fa-502">在**儲存體**區段中，等到新的儲存體帳戶的狀態變更為*線上*才能繼續進行下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="998fa-502">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="998fa-503">![建立儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "建立儲存體帳戶")</span><span class="sxs-lookup"><span data-stu-id="998fa-503">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="998fa-504">*建立儲存體帳戶*</span><span class="sxs-lookup"><span data-stu-id="998fa-504">*Storage Account created*</span></span>
4. <span data-ttu-id="998fa-505">按一下儲存體帳戶名稱，然後按一下**儀表板**在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="998fa-505">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="998fa-506">**儀表板**頁面為您提供的帳戶，可用於您的應用程式的服務端點的狀態資訊。</span><span class="sxs-lookup"><span data-stu-id="998fa-506">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="998fa-507">![顯示儲存體帳戶儀表板](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "顯示儲存體帳戶儀表板")</span><span class="sxs-lookup"><span data-stu-id="998fa-507">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="998fa-508">*顯示儲存體帳戶儀表板*</span><span class="sxs-lookup"><span data-stu-id="998fa-508">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="998fa-509">按一下**管理存取金鑰**導覽列中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="998fa-509">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="998fa-510">![管理存取金鑰 按鈕](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "管理存取金鑰 按鈕")</span><span class="sxs-lookup"><span data-stu-id="998fa-510">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="998fa-511">*管理存取金鑰 按鈕*</span><span class="sxs-lookup"><span data-stu-id="998fa-511">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="998fa-512">在**管理存取金鑰**對話方塊中，複製**儲存體帳戶名稱**和**主要存取金鑰**當您將需要在下列練習中。</span><span class="sxs-lookup"><span data-stu-id="998fa-512">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="998fa-513">然後，關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="998fa-513">Then, close the dialog box.</span></span>

    <span data-ttu-id="998fa-514">![管理存取金鑰對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "管理存取金鑰 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="998fa-514">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="998fa-515">*管理存取金鑰對話方塊*</span><span class="sxs-lookup"><span data-stu-id="998fa-515">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="998fa-516">工作 2-將資產上傳至 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="998fa-516">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="998fa-517">在這個工作中，您將使用從 Visual Studio 的 [伺服器總管] 視窗，來連接至儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="998fa-517">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="998fa-518">接著建立 blob 容器，並有一些測驗標誌檔案上傳至容器。</span><span class="sxs-lookup"><span data-stu-id="998fa-518">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="998fa-519">切換至 Visual Studio 執行個體與**GeekQuiz**在上一個作業中的方案。</span><span class="sxs-lookup"><span data-stu-id="998fa-519">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="998fa-520">從功能表列中，選取**檢視**，然後按一下 **伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="998fa-520">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="998fa-521">在**伺服器總管**，以滑鼠右鍵按一下**Azure**節點，然後選取**連接至 Azure...**.使用與您訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-521">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![連接到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="998fa-523">*連接到 Azure*</span><span class="sxs-lookup"><span data-stu-id="998fa-523">*Connect to Azure*</span></span>
4. <span data-ttu-id="998fa-524">展開**Azure**  節點，以滑鼠右鍵按一下**儲存體**選取**附加外部儲存體...**.</span><span class="sxs-lookup"><span data-stu-id="998fa-524">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="998fa-525">在**加入新的儲存體帳戶**對話方塊方塊中，輸入**帳戶名稱**和**帳戶金鑰**您在上一項工作，然後按一下取得**確定**.</span><span class="sxs-lookup"><span data-stu-id="998fa-525">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![加入新的儲存體帳戶對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="998fa-527">*加入新的儲存體帳戶對話方塊*</span><span class="sxs-lookup"><span data-stu-id="998fa-527">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="998fa-528">儲存體帳戶應該會出現在**儲存體**節點。</span><span class="sxs-lookup"><span data-stu-id="998fa-528">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="998fa-529">展開您的儲存體帳戶，以滑鼠右鍵按一下**Blob**選取**建立 Blob 容器...**.</span><span class="sxs-lookup"><span data-stu-id="998fa-529">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="998fa-530">![建立 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "建立 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="998fa-530">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="998fa-531">*建立 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="998fa-531">*Create Blob Container*</span></span>
7. <span data-ttu-id="998fa-532">在**建立 Blob 容器**對話方塊方塊中，輸入 blob 容器的名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="998fa-532">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="998fa-533">![建立 Blob 容器 對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "建立 Blob 容器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="998fa-533">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="998fa-534">*建立 Blob 容器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="998fa-534">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="998fa-535">應該加入新的 blob 容器**Blob**節點。</span><span class="sxs-lookup"><span data-stu-id="998fa-535">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="998fa-536">變更要將容器設為公用容器中的存取權限。</span><span class="sxs-lookup"><span data-stu-id="998fa-536">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="998fa-537">若要這樣做，請以滑鼠右鍵按一下**映像**容器，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="998fa-537">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="998fa-538">![映像容器屬性](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "映像容器屬性")</span><span class="sxs-lookup"><span data-stu-id="998fa-538">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="998fa-539">*映像容器屬性*</span><span class="sxs-lookup"><span data-stu-id="998fa-539">*Images container properties*</span></span>
9. <span data-ttu-id="998fa-540">在**屬性**視窗中，將**公用讀取權限**至**容器**。</span><span class="sxs-lookup"><span data-stu-id="998fa-540">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="998fa-541">![變更公用讀取權限屬性](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "變更公用讀取權限屬性")</span><span class="sxs-lookup"><span data-stu-id="998fa-541">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="998fa-542">*變更公用讀取權限屬性*</span><span class="sxs-lookup"><span data-stu-id="998fa-542">*Changing public read access property*</span></span>
10. <span data-ttu-id="998fa-543">當系統提示您是否確定要變更的公用存取屬性，請按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="998fa-543">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="998fa-544">![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="998fa-544">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="998fa-545">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="998fa-545">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="998fa-546">在**伺服器總管**，以滑鼠右鍵按一下**映像**blob 容器，然後選取**檢視 Blob 容器**。</span><span class="sxs-lookup"><span data-stu-id="998fa-546">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="998fa-547">![檢視 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "檢視 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="998fa-547">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="998fa-548">*檢視 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="998fa-548">*View Blob Container*</span></span>
12. <span data-ttu-id="998fa-549">容器映像應該會開啟新視窗中，應該會顯示任何項目的圖例。</span><span class="sxs-lookup"><span data-stu-id="998fa-549">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="998fa-550">按一下**上傳**檔案上傳至 blob 容器的圖示。</span><span class="sxs-lookup"><span data-stu-id="998fa-550">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="998fa-551">![無項目的影像容器](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "無項目的影像容器")</span><span class="sxs-lookup"><span data-stu-id="998fa-551">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="998fa-552">*無項目的影像容器*</span><span class="sxs-lookup"><span data-stu-id="998fa-552">*Images container with no entries*</span></span>
13. <span data-ttu-id="998fa-553">在**上傳 Blob**對話方塊方塊中，瀏覽至**資產**實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="998fa-553">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="998fa-554">選取**標誌 big.png**檔案，然後按一下 **開啟**。</span><span class="sxs-lookup"><span data-stu-id="998fa-554">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="998fa-555">請等到檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="998fa-555">Wait until the file is uploaded.</span></span> <span data-ttu-id="998fa-556">完成上傳的檔案應該列映像容器中。</span><span class="sxs-lookup"><span data-stu-id="998fa-556">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="998fa-557">檔案項目上按一下滑鼠右鍵，然後選取**複製 URL**。</span><span class="sxs-lookup"><span data-stu-id="998fa-557">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="998fa-558">![複製 blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "複製 blob 檔案的 URL")</span><span class="sxs-lookup"><span data-stu-id="998fa-558">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="998fa-559">*複製 blob URL*</span><span class="sxs-lookup"><span data-stu-id="998fa-559">*Copy blob URL*</span></span>
15. <span data-ttu-id="998fa-560">開啟 Internet Explorer 並貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="998fa-560">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="998fa-561">下列映像應該會顯示在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="998fa-561">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="998fa-562">![從 Windows Blob 儲存體的標誌 big.png 映像](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "標誌 big.png 映像，從儲存體")</span><span class="sxs-lookup"><span data-stu-id="998fa-562">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="998fa-563">*從 Azure Blob 儲存體的標誌 big.png 映像*</span><span class="sxs-lookup"><span data-stu-id="998fa-563">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="998fa-564">工作 3-更新方案來使用 Azure Blob 儲存體的靜態內容</span><span class="sxs-lookup"><span data-stu-id="998fa-564">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="998fa-565">在這個工作中，您將設定**GeekQuiz**解決方案，以使用映像上傳至 Azure Blob 儲存體 （而不是位於 web 應用程式中的影像） 加入 ASP.NET URL 重寫規則，在**web.config**檔案。</span><span class="sxs-lookup"><span data-stu-id="998fa-565">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="998fa-566">在 Visual Studio 中開啟**Web.config**檔案內**GeekQuiz**專案，並找出 **&lt;system.webServer&gt;** 項目。</span><span class="sxs-lookup"><span data-stu-id="998fa-566">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="998fa-567">加入下列程式碼，將 URL 重寫規則，更新您的儲存體帳戶名稱的預留位置。</span><span class="sxs-lookup"><span data-stu-id="998fa-567">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="998fa-568">(程式碼片段- *WebSitesInProduction-Ex4-UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="998fa-568">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="998fa-569">URL 重寫為攔截傳入的 Web 要求，並將要求重新導向至不同的資源的程序。</span><span class="sxs-lookup"><span data-stu-id="998fa-569">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="998fa-570">重寫規則的 URL 會告訴重寫引擎要求時必須重新導向，而且其中應該他們重新導向。</span><span class="sxs-lookup"><span data-stu-id="998fa-570">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="998fa-571">重寫規則由兩個字串所組成： 尋找所要求 URL 中的模式 （通常會使用規則運算式），並找到要取代的模式，如果字串。</span><span class="sxs-lookup"><span data-stu-id="998fa-571">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="998fa-572">如需詳細資訊，請參閱[ASP.NET 中的 URL 重寫](https://msdn.microsoft.com/library/ms972974.aspx)。</span><span class="sxs-lookup"><span data-stu-id="998fa-572">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="998fa-573">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-573">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="998fa-574">開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-574">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="998fa-575">執行下列命令以將變更推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="998fa-575">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="998fa-576">更新*[您的應用程式的路徑]*預留位置路徑**GeekQuiz**方案。</span><span class="sxs-lookup"><span data-stu-id="998fa-576">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="998fa-577">系統會提示您做為部署密碼。</span><span class="sxs-lookup"><span data-stu-id="998fa-577">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![將更新部署至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="998fa-579">*將更新部署至 Azure*</span><span class="sxs-lookup"><span data-stu-id="998fa-579">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="998fa-580">工作 4-驗證</span><span class="sxs-lookup"><span data-stu-id="998fa-580">Task 4 – Verification</span></span>

<span data-ttu-id="998fa-581">在這項工作會使用**Internet Explorer**瀏覽**玩家測驗**應用程式，並檢查 URL 重寫規則的映像運作，而且您會被重新導向至裝載於映像**Azure Blob儲存體**。</span><span class="sxs-lookup"><span data-stu-id="998fa-581">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="998fa-582">開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="998fa-582">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="998fa-583">使用先前建立的認證登入。</span><span class="sxs-lookup"><span data-stu-id="998fa-583">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="998fa-584">![顯示玩家測驗 web 應用程式與映像](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "顯示玩家測驗 web 應用程式與映像")</span><span class="sxs-lookup"><span data-stu-id="998fa-584">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="998fa-585">*顯示玩家測驗 web 應用程式與映像*</span><span class="sxs-lookup"><span data-stu-id="998fa-585">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="998fa-586">按**F12**若要啟動開發工具，請選取**網路**索引標籤上，並開始錄製。</span><span class="sxs-lookup"><span data-stu-id="998fa-586">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="998fa-587">![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "啟動網路錄製")</span><span class="sxs-lookup"><span data-stu-id="998fa-587">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="998fa-588">*啟動網路錄製*</span><span class="sxs-lookup"><span data-stu-id="998fa-588">*Starting network recording*</span></span>
3. <span data-ttu-id="998fa-589">按**CTRL + F5**重新整理網頁。</span><span class="sxs-lookup"><span data-stu-id="998fa-589">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="998fa-590">在網頁完成載入時，您應該會看到的 HTTP 要求**/img/logo-big.png**含有 HTTP URL **301**結果 （重新導向） 和另一個要求`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL 含有 HTTP **200**結果。</span><span class="sxs-lookup"><span data-stu-id="998fa-590">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="998fa-591">![正在驗證重新導向 URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開發人員工具中顯示重新導向")</span><span class="sxs-lookup"><span data-stu-id="998fa-591">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="998fa-592">*正在驗證重新導向 URL*</span><span class="sxs-lookup"><span data-stu-id="998fa-592">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="998fa-593">練習 5： 使用自動調整規模的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="998fa-593">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="998fa-594">這個練習中是選擇性的因為它要求支援 Web 負載&amp;效能測試時才可供**Visual Studio 2013 Ultimate Edition**。</span><span class="sxs-lookup"><span data-stu-id="998fa-594">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="998fa-595">如需特定的 Visual Studio 2013 功能的詳細資訊，請比較版本[這裡](https://www.microsoft.com/visualstudio/eng/products/compare)。</span><span class="sxs-lookup"><span data-stu-id="998fa-595">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="998fa-596">**Azure App Service Web 應用程式**中執行的 web 應用程式提供的自動調整功能**標準模式**。</span><span class="sxs-lookup"><span data-stu-id="998fa-596">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="998fa-597">自動調整規模可讓 Azure 自動調整您的 web 應用程式，根據負載的執行個體計數。</span><span class="sxs-lookup"><span data-stu-id="998fa-597">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="998fa-598">啟用自動調整時，Azure 會檢查您的 web 應用程式的 CPU 一次每隔五分鐘，並將執行個體，視需要在該點的時間。</span><span class="sxs-lookup"><span data-stu-id="998fa-598">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="998fa-599">如果 CPU 使用量過低時，Azure 將會移除執行個體一次每隔兩小時以確保您的 web 應用程式的效能不會效能降低。</span><span class="sxs-lookup"><span data-stu-id="998fa-599">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="998fa-600">在此練習將逐步執行設定所需的步驟**自動調整規模**之功能**玩家測驗**web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-600">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="998fa-601">您將執行 Visual Studio 負載測試，以便產生足夠的 CPU 負載，以觸發執行個體升級應用程式上，以驗證這項功能。</span><span class="sxs-lookup"><span data-stu-id="998fa-601">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="998fa-602">工作 1-根據 CPU 度量的自動調整設定</span><span class="sxs-lookup"><span data-stu-id="998fa-602">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="998fa-603">在這項工作中，您將使用 Azure 管理入口網站來啟用自動調整功能，您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-603">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="998fa-604">在[Azure 管理入口網站](https://manage.windowsazure.com/)，選取**網站**按一下您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="998fa-604">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="998fa-605">瀏覽至**標尺**頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-605">Navigate to the **Scale** page.</span></span> <span data-ttu-id="998fa-606">在下**容量**區段中，選取**CPU**如**依度量調整**組態。</span><span class="sxs-lookup"><span data-stu-id="998fa-606">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-607">依 CPU 調整，當 Azure 動態調整應用程式中使用的 CPU 使用量變更時的執行個體數目。</span><span class="sxs-lookup"><span data-stu-id="998fa-607">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="998fa-608">![選取依 CPU 調整即可](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "選取自動調整大小的 CPU 計量")</span><span class="sxs-lookup"><span data-stu-id="998fa-608">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="998fa-609">*選取依 CPU 調整即可*</span><span class="sxs-lookup"><span data-stu-id="998fa-609">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="998fa-610">變更**目標 CPU**設定**20**-**40**百分比。</span><span class="sxs-lookup"><span data-stu-id="998fa-610">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-611">這個範圍代表您 web 應用程式的平均 CPU 使用量。</span><span class="sxs-lookup"><span data-stu-id="998fa-611">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="998fa-612">Azure 會加入或移除執行個體，將您的 web 應用程式保留在這個範圍。</span><span class="sxs-lookup"><span data-stu-id="998fa-612">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="998fa-613">用於調整的執行個體最小和最大數目指定在**執行個體計數**組態。</span><span class="sxs-lookup"><span data-stu-id="998fa-613">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="998fa-614">Azure 絕不會高於或超過該限制。</span><span class="sxs-lookup"><span data-stu-id="998fa-614">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="998fa-615">預設值**目標 CPU**只針對這個實驗室的目的修改值。</span><span class="sxs-lookup"><span data-stu-id="998fa-615">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="998fa-616">藉由設定 CPU 範圍值較小，您要在一般負載放在應用程式時增加到觸發程序自動調整的機會。</span><span class="sxs-lookup"><span data-stu-id="998fa-616">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="998fa-617">![變更目標必須介於 20 到 40%的 CPU](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "變更必須介於 20 到 40%的目標 CPU")</span><span class="sxs-lookup"><span data-stu-id="998fa-617">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="998fa-618">*變更必須介於 20 到 40%的目標 CPU*</span><span class="sxs-lookup"><span data-stu-id="998fa-618">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="998fa-619">按一下**儲存**在命令列中儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="998fa-619">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="998fa-620">工作 2： 使用 Visual Studio 測試的負載</span><span class="sxs-lookup"><span data-stu-id="998fa-620">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="998fa-621">既然**自動調整規模**已經過設定，您將建立**Web 效能和負載測試專案**在 Visual Studio 中產生某些 web 應用程式上的 CPU 負載。</span><span class="sxs-lookup"><span data-stu-id="998fa-621">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="998fa-622">開啟**Visual Studio Ultimate 2013**選取**檔案 |新 |專案...**啟動新的解決方案。</span><span class="sxs-lookup"><span data-stu-id="998fa-622">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="998fa-623">![建立新的專案](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "建立新的專案")</span><span class="sxs-lookup"><span data-stu-id="998fa-623">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="998fa-624">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="998fa-624">*Creating a new project*</span></span>
2. <span data-ttu-id="998fa-625">在**新專案**對話方塊中，選取**Web 效能和負載測試專案**下**Visual C# |測試** 索引標籤。請確定**.NET Framework 4.5**是選取，將專案命名為*WebAndLoadTestProject*，選擇**位置**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="998fa-625">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="998fa-626">![建立新的 Web 和負載測試專案](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "建立新的 Web 和負載測試專案")</span><span class="sxs-lookup"><span data-stu-id="998fa-626">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="998fa-627">*建立新的 Web 和負載測試專案*</span><span class="sxs-lookup"><span data-stu-id="998fa-627">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="998fa-628">在**WebTest1.webtest**以滑鼠右鍵按一下**WebTest1**節點，然後按一下**加入要求**。</span><span class="sxs-lookup"><span data-stu-id="998fa-628">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="998fa-629">![將要求加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "將要求加入至 WebTest1")</span><span class="sxs-lookup"><span data-stu-id="998fa-629">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="998fa-630">*將要求加入至 WebTest1*</span><span class="sxs-lookup"><span data-stu-id="998fa-630">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="998fa-631">在**屬性**視窗的 [新要求] 節點中，更新**Url**屬性以指向您的 web 應用程式的 URL (例如 *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="998fa-631">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="998fa-632">![變更 Url 屬性](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "變更 Url 屬性")</span><span class="sxs-lookup"><span data-stu-id="998fa-632">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="998fa-633">*變更 Url 屬性*</span><span class="sxs-lookup"><span data-stu-id="998fa-633">*Changing the Url property*</span></span>
5. <span data-ttu-id="998fa-634">在**WebTest1.webtest**視窗中，以滑鼠右鍵按一下**WebTest1**按一下**新增迴圈...**.</span><span class="sxs-lookup"><span data-stu-id="998fa-634">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="998fa-635">![將迴圈加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "將迴圈加入至 WebTest1")</span><span class="sxs-lookup"><span data-stu-id="998fa-635">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="998fa-636">*將迴圈加入至 WebTest1*</span><span class="sxs-lookup"><span data-stu-id="998fa-636">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="998fa-637">在**加入條件式規則和項目迴圈**對話方塊中，選取**For 迴圈 」**規則，並修改下列屬性。</span><span class="sxs-lookup"><span data-stu-id="998fa-637">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="998fa-638">**結束值：** 1000年</span><span class="sxs-lookup"><span data-stu-id="998fa-638">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="998fa-639">**內容參數名稱：**迭代器</span><span class="sxs-lookup"><span data-stu-id="998fa-639">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="998fa-640">**遞增值：** 1</span><span class="sxs-lookup"><span data-stu-id="998fa-640">**Increment Value:** 1</span></span>

    <span data-ttu-id="998fa-641">![選取 For 迴圈 」 規則，並更新屬性](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "選取 For 迴圈 」 規則，並更新屬性")</span><span class="sxs-lookup"><span data-stu-id="998fa-641">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="998fa-642">*選取 For 迴圈 」 規則，並更新屬性*</span><span class="sxs-lookup"><span data-stu-id="998fa-642">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="998fa-643">在下**迴圈中的項目**區段中，選取您要在迴圈的第一個和最後一個項目先前建立的要求。</span><span class="sxs-lookup"><span data-stu-id="998fa-643">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="998fa-644">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="998fa-644">Click **OK** to continue.</span></span>

    <span data-ttu-id="998fa-645">![選取此迴圈的第一個和最後一個項目](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "選取迴圈的第一個和最後一個項目")</span><span class="sxs-lookup"><span data-stu-id="998fa-645">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="998fa-646">*選取此迴圈的第一個和最後一個項目*</span><span class="sxs-lookup"><span data-stu-id="998fa-646">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="998fa-647">在**方案總管] 中**，以滑鼠右鍵按一下**WebAndLoadTestProject**專案中，展開 [**新增**功能表，然後選取**負載測試...**.</span><span class="sxs-lookup"><span data-stu-id="998fa-647">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="998fa-648">![將負載測試加入至 WebAndLoadTestProject 專案](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject 專案中加入負載測試")</span><span class="sxs-lookup"><span data-stu-id="998fa-648">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="998fa-649">*WebAndLoadTestProject 專案中加入負載測試*</span><span class="sxs-lookup"><span data-stu-id="998fa-649">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="998fa-650">在**新增負載測試精靈**對話方塊中，按一下 **下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-650">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="998fa-651">![新增負載測試精靈](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新增負載測試精靈")</span><span class="sxs-lookup"><span data-stu-id="998fa-651">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="998fa-652">*新增負載測試精靈*</span><span class="sxs-lookup"><span data-stu-id="998fa-652">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="998fa-653">在**案例**頁面上，選取**不使用考慮時間**按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-653">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="998fa-654">![選取不想使用考慮時間](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "選取不想使用考慮時間")</span><span class="sxs-lookup"><span data-stu-id="998fa-654">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="998fa-655">*選擇不使用考慮時間*</span><span class="sxs-lookup"><span data-stu-id="998fa-655">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="998fa-656">在**負載模式**頁面上，請確定**常數負載**選取選項。</span><span class="sxs-lookup"><span data-stu-id="998fa-656">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="998fa-657">變更**使用者計數**設**250**使用者，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-657">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="998fa-658">![使用者計數變更到 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "變更到 250 使用者計數")</span><span class="sxs-lookup"><span data-stu-id="998fa-658">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="998fa-659">*變更到 250 使用者計數*</span><span class="sxs-lookup"><span data-stu-id="998fa-659">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="998fa-660">在**測試混合模型**頁面上，選取**依據循序測試順序**按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-660">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="998fa-661">![選取的測試混合模型](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "選取的測試混合模型")</span><span class="sxs-lookup"><span data-stu-id="998fa-661">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="998fa-662">*選取的測試混合模型*</span><span class="sxs-lookup"><span data-stu-id="998fa-662">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="998fa-663">在**測試混合模型**頁面上，按一下**新增...**若要將測試加入至混合。</span><span class="sxs-lookup"><span data-stu-id="998fa-663">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="998fa-664">![將測試加入至測試混合](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "將測試加入至測試混合")</span><span class="sxs-lookup"><span data-stu-id="998fa-664">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="998fa-665">*將測試加入至測試混合*</span><span class="sxs-lookup"><span data-stu-id="998fa-665">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="998fa-666">在**加入測試**對話方塊中，按兩下**WebTest1**若要加入至測試**選取的測試**清單。</span><span class="sxs-lookup"><span data-stu-id="998fa-666">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="998fa-667">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="998fa-667">Click **OK** to continue.</span></span>

    <span data-ttu-id="998fa-668">![新增 WebTest1 測試](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "新增 WebTest1 測試")</span><span class="sxs-lookup"><span data-stu-id="998fa-668">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="998fa-669">*新增 WebTest1 測試*</span><span class="sxs-lookup"><span data-stu-id="998fa-669">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="998fa-670">回到**測試混合**頁面上，按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-670">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="998fa-671">![正在完成測試混合頁面](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "完成測試混合 頁面")</span><span class="sxs-lookup"><span data-stu-id="998fa-671">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="998fa-672">*完成測試混合 頁面*</span><span class="sxs-lookup"><span data-stu-id="998fa-672">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="998fa-673">在**網路混合**頁面上，按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-673">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="998fa-674">![按一下 [網路混合] 頁面中的下一個](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "按一下網路混合 頁面中的下一個")</span><span class="sxs-lookup"><span data-stu-id="998fa-674">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="998fa-675">*網路混合頁面中按一下 [下一步]*</span><span class="sxs-lookup"><span data-stu-id="998fa-675">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="998fa-676">在**瀏覽器混合**頁面上，選取**Internet Explorer 10.0**作為瀏覽器類型按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-676">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="998fa-677">![選取瀏覽器類型](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "選取瀏覽器類型")</span><span class="sxs-lookup"><span data-stu-id="998fa-677">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="998fa-678">*選取瀏覽器類型*</span><span class="sxs-lookup"><span data-stu-id="998fa-678">*Selecting the browser type*</span></span>
18. <span data-ttu-id="998fa-679">在**計數器集合**頁面上，按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="998fa-679">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="998fa-680">![計數器集合] 頁面中按一下 [下一步](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "按一下中的下一個計數器集合 頁面")</span><span class="sxs-lookup"><span data-stu-id="998fa-680">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="998fa-681">*計數器集合 頁面中按一下 下一步*</span><span class="sxs-lookup"><span data-stu-id="998fa-681">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="998fa-682">在**回合設定**頁面上，設定**負載測試持續期間**至**5 分鐘**按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="998fa-682">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="998fa-683">![設定負載測試持續期間為 5 分鐘](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "設定負載測試持續期間為 5 分鐘")</span><span class="sxs-lookup"><span data-stu-id="998fa-683">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="998fa-684">*設定負載測試持續期間為 5 分鐘*</span><span class="sxs-lookup"><span data-stu-id="998fa-684">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="998fa-685">在**方案總管 中**，連按兩下**Local.settings**瀏覽測試設定檔案。</span><span class="sxs-lookup"><span data-stu-id="998fa-685">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="998fa-686">根據預設，Visual Studio 會使用本機電腦執行的測試。</span><span class="sxs-lookup"><span data-stu-id="998fa-686">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-687">或者，您可以設定您的測試專案來執行負載測試在雲端中使用**Visual Studio Online (VSO)**。</span><span class="sxs-lookup"><span data-stu-id="998fa-687">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="998fa-688">VSO 提供雲端式負載測試會模擬更真實的負載的服務，避免本機環境條件約束，例如 CPU 容量總計、 可用記憶體和網路頻寬。</span><span class="sxs-lookup"><span data-stu-id="998fa-688">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="998fa-689">如需使用 VSO 執行負載測試的詳細資訊，請參閱[本文](https://www.visualstudio.com/get-started/load-test-your-app-vs)。</span><span class="sxs-lookup"><span data-stu-id="998fa-689">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![測試設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="998fa-691">工作 3-自動調整規模驗證</span><span class="sxs-lookup"><span data-stu-id="998fa-691">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="998fa-692">您現在將會執行您在前一項工作中建立負載測試，並查看您的 web 應用程式在負載下的運作方式。</span><span class="sxs-lookup"><span data-stu-id="998fa-692">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="998fa-693">在**方案總管 中**，連按兩下**LoadTest1.loadtest**開啟負載測試。</span><span class="sxs-lookup"><span data-stu-id="998fa-693">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="998fa-694">![開啟 LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "開啟 LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="998fa-694">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="998fa-695">*開啟 LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="998fa-695">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="998fa-696">在**LoadTest1.loadtest**視窗中，按一下要執行負載測試工具箱 中的第一個按鈕。</span><span class="sxs-lookup"><span data-stu-id="998fa-696">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="998fa-697">![執行負載測試](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "執行負載測試")</span><span class="sxs-lookup"><span data-stu-id="998fa-697">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="998fa-698">*執行負載測試*</span><span class="sxs-lookup"><span data-stu-id="998fa-698">*Running the load test*</span></span>
3. <span data-ttu-id="998fa-699">等待直到完成負載測試。</span><span class="sxs-lookup"><span data-stu-id="998fa-699">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-700">負載測試會模擬多個同時將要求傳送至 web 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="998fa-700">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="998fa-701">當執行測試時，您可以監視可用的計數器，來偵測所有錯誤、 警告或其他與您的負載測試回合相關的資訊。</span><span class="sxs-lookup"><span data-stu-id="998fa-701">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="998fa-702">![負載測試執行](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "等待，直到負載測試完成")</span><span class="sxs-lookup"><span data-stu-id="998fa-702">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="998fa-703">*執行負載測試*</span><span class="sxs-lookup"><span data-stu-id="998fa-703">*Load test running*</span></span>
4. <span data-ttu-id="998fa-704">測試完成之後，請返回管理入口網站並瀏覽至**標尺**web 應用程式頁面。</span><span class="sxs-lookup"><span data-stu-id="998fa-704">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="998fa-705">在下**容量** 區段中，您應該會看到圖表中的新執行個體自動部署。</span><span class="sxs-lookup"><span data-stu-id="998fa-705">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![自動部署的新執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="998fa-707">*自動部署的新執行個體*</span><span class="sxs-lookup"><span data-stu-id="998fa-707">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="998fa-708">可能需要幾分鐘的時間才會出現在圖表中的變更 (請按**CTRL + F5**定期重新整理頁面)。</span><span class="sxs-lookup"><span data-stu-id="998fa-708">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="998fa-709">如果看不到任何變更，您可以嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="998fa-709">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="998fa-710">增加負載測試的持續時間 (例如以**10 分鐘**)</span><span class="sxs-lookup"><span data-stu-id="998fa-710">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="998fa-711">減少的最大和最小值**目標 CPU**您 web 應用程式的自動調整規模設定中的範圍</span><span class="sxs-lookup"><span data-stu-id="998fa-711">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="998fa-712">透過在雲端執行負載測試**Visual Studio Online**。</span><span class="sxs-lookup"><span data-stu-id="998fa-712">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="998fa-713">更多資訊[這裡](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="998fa-713">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="998fa-714">總結</span><span class="sxs-lookup"><span data-stu-id="998fa-714">Summary</span></span>

<span data-ttu-id="998fa-715">在這個實際操作實驗室中，您學會如何設定和應用程式部署至生產環境 web 應用程式在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="998fa-715">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="998fa-716">首先會偵測並使用您資料庫更新**Entity Framework Code First 移轉**，然後繼續執行部署新版本的站台使用**Git**以及執行回復您的站台的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="998fa-716">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="998fa-717">此外，您會學到如何調整您的應用程式使用儲存體將靜態內容移至 Blob 容器。</span><span class="sxs-lookup"><span data-stu-id="998fa-717">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
