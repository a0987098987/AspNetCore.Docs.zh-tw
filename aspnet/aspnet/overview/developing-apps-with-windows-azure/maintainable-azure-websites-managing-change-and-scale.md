---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 實際操作實驗室： 可維護的 Azure 網站： 管理變更和規模 |Microsoft Docs
author: rick-anderson
description: 在此實驗室中，了解 Microsoft Azure 如何讓您輕鬆建置及部署網站至生產環境。
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 05181ae1b2d857eea45983d378b28011c1cd755a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578129"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="53afb-103">實際操作實驗室： 可維護的 Azure 網站： 管理變更和小數位數</span><span class="sxs-lookup"><span data-stu-id="53afb-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="53afb-104">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="53afb-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="53afb-105">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="53afb-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="53afb-106">Microsoft Azure 可讓您輕鬆建置及部署網站至生產環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="53afb-107">但您未完成時即時應用程式時，您剛開始使用 ！</span><span class="sxs-lookup"><span data-stu-id="53afb-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="53afb-108">您必須處理變更的需求、 資料庫更新、 小數位數和更多功能。</span><span class="sxs-lookup"><span data-stu-id="53afb-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="53afb-109">幸運的是，Azure App Service 具有您的後盾，並附上豐富的功能，可協助您保護您的站台順利執行。</span><span class="sxs-lookup"><span data-stu-id="53afb-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
>
> <span data-ttu-id="53afb-110">Azure 提供安全且彈性的開發、 部署和延展選項為任何大小的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="53afb-111">運用現有的工具來建立及部署應用程式無須管理基礎結構。</span><span class="sxs-lookup"><span data-stu-id="53afb-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
>
> <span data-ttu-id="53afb-112">自行佈建生產 web 應用程式以分鐘為單位輕鬆地部署您慣用的開發工具所建立的內容。</span><span class="sxs-lookup"><span data-stu-id="53afb-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="53afb-113">您可以部署現有的網站，直接從支援的原始檔控制**Git**， **GitHub**， **Bitbucket**， **TFS**，和甚至**DropBox**。</span><span class="sxs-lookup"><span data-stu-id="53afb-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="53afb-114">部署直接從您最喜愛的 IDE 或使用指令碼**PowerShell**在 Windows 或**CLI**任何作業系統上執行的工具。</span><span class="sxs-lookup"><span data-stu-id="53afb-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="53afb-115">一旦部署之後，掌握您的網站持續進行連續部署的支援。</span><span class="sxs-lookup"><span data-stu-id="53afb-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
>
> <span data-ttu-id="53afb-116">Azure 提供可調式、 持久的雲端儲存體、 備份和復原解決方案的任何資料，不論巨量或少。</span><span class="sxs-lookup"><span data-stu-id="53afb-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="53afb-117">當部署至生產環境，例如資料表、 Blob 和 SQL 資料庫的儲存體服務的應用程式，協助您調整在雲端中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
>
> <span data-ttu-id="53afb-118">使用 SQL 資料庫時，請務必保持產能的資料庫部署應用程式的新版本時。</span><span class="sxs-lookup"><span data-stu-id="53afb-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="53afb-119">要感謝**Entity Framework Code First Migrations**，開發和部署您的資料模型已簡化，以更新您的環境，以分鐘為單位。</span><span class="sxs-lookup"><span data-stu-id="53afb-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="53afb-120">這個實際操作實驗室會顯示您的 web 應用程式部署到 Microsoft Azure 中的生產環境時，可能會遇到的不同主題。</span><span class="sxs-lookup"><span data-stu-id="53afb-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
>
> <span data-ttu-id="53afb-121">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="53afb-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
>
> <span data-ttu-id="53afb-122">如需本主題的深入報導，請參閱 <<c0> [ 建置真實世界雲端應用程式與 Azure 的電子書](building-real-world-cloud-apps-with-windows-azure/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="53afb-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="53afb-123">總覽</span><span class="sxs-lookup"><span data-stu-id="53afb-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="53afb-124">目標</span><span class="sxs-lookup"><span data-stu-id="53afb-124">Objectives</span></span>

<span data-ttu-id="53afb-125">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="53afb-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="53afb-126">讓 使用現有的模型 Entity Framework 移轉</span><span class="sxs-lookup"><span data-stu-id="53afb-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="53afb-127">更新物件模型並據以使用 Entity Framework 移轉的資料庫</span><span class="sxs-lookup"><span data-stu-id="53afb-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="53afb-128">部署至 Azure App Service 使用 Git</span><span class="sxs-lookup"><span data-stu-id="53afb-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="53afb-129">回復成先前的部署使用 Azure 管理入口網站</span><span class="sxs-lookup"><span data-stu-id="53afb-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="53afb-130">使用 Azure 儲存體來調整 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="53afb-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="53afb-131">設定自動調整 web 應用程式中使用 Azure 管理入口網站</span><span class="sxs-lookup"><span data-stu-id="53afb-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="53afb-132">建立和設定 Visual Studio 中的負載測試專案</span><span class="sxs-lookup"><span data-stu-id="53afb-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="53afb-133">必要條件</span><span class="sxs-lookup"><span data-stu-id="53afb-133">Prerequisites</span></span>

<span data-ttu-id="53afb-134">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="53afb-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="53afb-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="53afb-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="53afb-136">Azure SDK for.NET 2.2</span><span class="sxs-lookup"><span data-stu-id="53afb-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="53afb-137">GIT 版本控制系統</span><span class="sxs-lookup"><span data-stu-id="53afb-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="53afb-138">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="53afb-138">A Microsoft Azure subscription</span></span>

    - <span data-ttu-id="53afb-139">註冊[免費試用版](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="53afb-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="53afb-140">如果您是 Visual Studio Professional、 Test Professional、 Premium 或 Ultimate with MSDN 或 MSDN Platforms 訂閱者時，啟用您[MSDN 權益](http://aka.ms/watk-msdn)現在要開始開發及測試 Azure 上</span><span class="sxs-lookup"><span data-stu-id="53afb-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="53afb-141">[BizSpark](http://aka.ms/watk-bizspark)成員會自動獲得 Azure 權益透過其 Visual Studio Ultimate with MSDN 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="53afb-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="53afb-142">成員[Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials 方案收到免費的每月 Azure 點數</span><span class="sxs-lookup"><span data-stu-id="53afb-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="53afb-143">安裝程式</span><span class="sxs-lookup"><span data-stu-id="53afb-143">Setup</span></span>

<span data-ttu-id="53afb-144">若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="53afb-145">開啟 Windows Explorer 並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="53afb-146">以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="53afb-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="53afb-147">如果會顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="53afb-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="53afb-148">請確定您執行安裝程式之前檢查這個實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="53afb-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="53afb-149">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="53afb-149">Using the Code Snippets</span></span>

<span data-ttu-id="53afb-150">整個實驗室文件中，您將會指示要插入程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="53afb-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="53afb-151">為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="53afb-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="53afb-152">每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="53afb-153">請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。</span><span class="sxs-lookup"><span data-stu-id="53afb-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="53afb-154">在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="53afb-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="53afb-155">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。</span><span class="sxs-lookup"><span data-stu-id="53afb-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="53afb-156">練習</span><span class="sxs-lookup"><span data-stu-id="53afb-156">Exercises</span></span>

<span data-ttu-id="53afb-157">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="53afb-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="53afb-158">使用 Entity Framework 移轉</span><span class="sxs-lookup"><span data-stu-id="53afb-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="53afb-159">Web 應用程式部署至預備環境</span><span class="sxs-lookup"><span data-stu-id="53afb-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="53afb-160">在生產環境中執行部署復原</span><span class="sxs-lookup"><span data-stu-id="53afb-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="53afb-161">使用 Azure 儲存體進行調整</span><span class="sxs-lookup"><span data-stu-id="53afb-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="53afb-162">[使用 Web 應用程式的自動調整規模](#Exercise5)（可選擇性地用於 Visual Studio 2013 Ultimate edition）</span><span class="sxs-lookup"><span data-stu-id="53afb-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="53afb-163">估計的時間才能完成這個實驗室： **75 分鐘**</span><span class="sxs-lookup"><span data-stu-id="53afb-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="53afb-164">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="53afb-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="53afb-165">每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="53afb-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="53afb-166">在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="53afb-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="53afb-167">如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。</span><span class="sxs-lookup"><span data-stu-id="53afb-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="53afb-168">練習 1： 使用 Entity Framework 移轉</span><span class="sxs-lookup"><span data-stu-id="53afb-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="53afb-169">當您開發應用程式時，您的資料模型可能會隨著時間改變。</span><span class="sxs-lookup"><span data-stu-id="53afb-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="53afb-170">這些變更可能會影響您的資料庫中現有的模型，（如果您要建立新的版本），並務必要讓資料庫保持在最新狀態，以避免發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="53afb-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="53afb-171">若要簡化您的模型，這些變更的追蹤**Entity Framework Code First Migrations**自動偵測比較您的模型資料庫結構描述的變更，並產生特定的程式碼，以更新您的資料庫，建立新*版本*您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="53afb-172">這個練習將示範如何啟用**移轉**針對您的應用程式，以及如何輕鬆地偵測和產生的變更來更新您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="53afb-173">工作 1： 啟用移轉</span><span class="sxs-lookup"><span data-stu-id="53afb-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="53afb-174">在這個工作中，您會瀏覽的啟用步驟**Entity Framework Code First Migrations**要**Geek 測驗**資料庫變更的模型，以及了解如何將這些變更反映在資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="53afb-175">開啟 Visual Studio，然後開啟**GeekQuiz.sln**方案檔從**Source\Ex1 UsingEntityFrameworkMigrations\Begin**。</span><span class="sxs-lookup"><span data-stu-id="53afb-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="53afb-176">建置方案，以下載和安裝**NuGet**封裝的相依性。</span><span class="sxs-lookup"><span data-stu-id="53afb-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="53afb-177">若要這樣做，請以滑鼠右鍵按一下方案，然後按一下**建置方案**或按**Ctrl + Shift + B**。</span><span class="sxs-lookup"><span data-stu-id="53afb-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="53afb-178">從**工具**功能表，在 Visual Studio 中，選取**程式庫套件管理員**，然後按一下**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="53afb-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="53afb-179">在  **Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="53afb-180">將建立初始移轉現有的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="53afb-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="53afb-181">![啟用移轉](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "啟用移轉")</span><span class="sxs-lookup"><span data-stu-id="53afb-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="53afb-182">*啟用移轉*</span><span class="sxs-lookup"><span data-stu-id="53afb-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-183">此命令會新增**移轉**Geek 測驗的專案，其中包含檔案的資料夾名**Configuration.cs**。</span><span class="sxs-lookup"><span data-stu-id="53afb-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="53afb-184">**組態**類別可讓您設定移轉您的內容的運作方式。</span><span class="sxs-lookup"><span data-stu-id="53afb-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="53afb-185">使用啟用的移轉，您需要更新**組態**類別，以填入資料庫的初始資料， **Geek 測驗**需要。</span><span class="sxs-lookup"><span data-stu-id="53afb-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="53afb-186">底下**移轉**，取代**Configuration.cs**檔案，與位於**Source\Assets**本實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-187">由於**移轉**稱之為**種子**與每個資料庫更新的方法，您必須確定資料庫中不會複寫記錄。</span><span class="sxs-lookup"><span data-stu-id="53afb-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="53afb-188">**AddOrUpdate**方法將有助於防止重複的資料。</span><span class="sxs-lookup"><span data-stu-id="53afb-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="53afb-189">新增初始移轉，請輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-190">請確定沒有名為資料庫&quot;GeekQuizProd&quot; LocalDB 執行個體中。</span><span class="sxs-lookup"><span data-stu-id="53afb-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="53afb-191">![新增基底結構描述移轉](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "加入基底結構描述移轉")</span><span class="sxs-lookup"><span data-stu-id="53afb-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="53afb-192">*新增基底結構描述移轉*</span><span class="sxs-lookup"><span data-stu-id="53afb-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-193">**新增移轉**會 scaffold 下一步 的移轉，根據您對您的模型建立的最後一個移轉以來的變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="53afb-194">在此情況下，因為它是第一個移轉專案時，它會新增建立中所定義的所有資料表的指令碼**TriviaContext**類別。</span><span class="sxs-lookup"><span data-stu-id="53afb-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="53afb-195">執行移轉至執行下列命令來更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="53afb-196">此命令會指定**Verbose**旗標，以檢視套用到目標資料庫的 SQL 陳述式。</span><span class="sxs-lookup"><span data-stu-id="53afb-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="53afb-197">![建立初始資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "建立初始資料庫")</span><span class="sxs-lookup"><span data-stu-id="53afb-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="53afb-198">*建立初始資料庫*</span><span class="sxs-lookup"><span data-stu-id="53afb-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-199">**更新資料庫**會將任何暫止的移轉套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="53afb-200">在此情況下，它會建立使用您的 web.config 檔案中定義的連接字串的資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="53afb-201">移至**檢視**功能表，然後開啟**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="53afb-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="53afb-202">![在 SQL Server 物件總管 中開啟](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "在 SQL Server 物件總管 中開啟")</span><span class="sxs-lookup"><span data-stu-id="53afb-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="53afb-203">*在 SQL Server 物件總管 中開啟*</span><span class="sxs-lookup"><span data-stu-id="53afb-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="53afb-204">在 [ **SQL Server 物件總管**] 視窗中，連接到 LocalDB 執行個體中，以滑鼠右鍵按一下**SQL Server**節點，然後選取**加入 SQL Server...** 選項。</span><span class="sxs-lookup"><span data-stu-id="53afb-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="53afb-205">![加入 SQL Server 執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "加入 SQL Server 執行個體")</span><span class="sxs-lookup"><span data-stu-id="53afb-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="53afb-206">*加入 SQL Server 物件總管 中的 SQL Server 執行個體*</span><span class="sxs-lookup"><span data-stu-id="53afb-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="53afb-207">設定**伺服器名稱**要 *(localdb) \v11.0* ，並將**Windows 驗證**當做驗證模式。</span><span class="sxs-lookup"><span data-stu-id="53afb-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="53afb-208">按一下  **Connect**以繼續。</span><span class="sxs-lookup"><span data-stu-id="53afb-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="53afb-209">![連接到 LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "連接到 LocalDB")</span><span class="sxs-lookup"><span data-stu-id="53afb-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="53afb-210">*連接到 LocalDB*</span><span class="sxs-lookup"><span data-stu-id="53afb-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="53afb-211">開啟**GeekQuizProd**資料庫中，展開**資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="53afb-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="53afb-212">如您所見， **Update-database**產生的命令中所定義的所有資料表**TriviaContext**類別。</span><span class="sxs-lookup"><span data-stu-id="53afb-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="53afb-213">找出**dbo。TriviaQuestions**資料表，並開啟 [資料行] 節點。</span><span class="sxs-lookup"><span data-stu-id="53afb-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="53afb-214">在下一個工作中，您會將新的資料行新增至此資料表，並更新資料庫使用**移轉**。</span><span class="sxs-lookup"><span data-stu-id="53afb-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="53afb-215">![邏輯問題的資料行](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "邏輯問題的資料行")</span><span class="sxs-lookup"><span data-stu-id="53afb-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="53afb-216">*邏輯問題的資料行*</span><span class="sxs-lookup"><span data-stu-id="53afb-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="53afb-217">工作 2-使用移轉更新資料庫結構描述</span><span class="sxs-lookup"><span data-stu-id="53afb-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="53afb-218">在這個工作中，您將使用**Entity Framework Code First Migrations**偵測您的模型中的變更，並產生必要的程式碼，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="53afb-219">您將會更新**TriviaQuestions**藉由將新屬性的實體。</span><span class="sxs-lookup"><span data-stu-id="53afb-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="53afb-220">然後，您將執行命令來建立新的移轉，將新的資料行包含資料表中。</span><span class="sxs-lookup"><span data-stu-id="53afb-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="53afb-221">在 **方案總管**，按兩下**TriviaQuestion.cs**檔案位於**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="53afb-222">新增名為的新屬性**提示**，如下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="53afb-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="53afb-223">在  **Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="53afb-224">將反映在我們的模型變更會建立新的移轉。</span><span class="sxs-lookup"><span data-stu-id="53afb-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="53afb-225">![Add-migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "新增移轉")</span><span class="sxs-lookup"><span data-stu-id="53afb-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="53afb-226">*新增移轉*</span><span class="sxs-lookup"><span data-stu-id="53afb-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-227">移轉檔案由兩個方法，組成**向上**並**向下**。</span><span class="sxs-lookup"><span data-stu-id="53afb-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    >
    > - <span data-ttu-id="53afb-228">**向上**方法將用來指定哪些變更我們的應用程式需求，要套用至資料庫的目前版本。</span><span class="sxs-lookup"><span data-stu-id="53afb-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="53afb-229">**下移**用來回復的變更，我們已新增至**向上**方法。</span><span class="sxs-lookup"><span data-stu-id="53afb-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    >
    > <span data-ttu-id="53afb-230">當資料庫移轉更新資料庫時，它會執行所有移轉作業中的時間戳記順序，以及只是尚未使用上次更新之後 ( \_MigrationHistory 資料表會追蹤的哪些移轉經套用)。</span><span class="sxs-lookup"><span data-stu-id="53afb-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="53afb-231">**向上**所有移轉的方法會呼叫並將進行的變更我們已指定到資料庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="53afb-232">如果我們要回到上一個移轉時，決定**往下**將重做的變更，以反向順序呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="53afb-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="53afb-233">在  **Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="53afb-234">輸出**Update-database**產生命令**Alter Table**新增至新的資料行的 SQL 陳述式**TriviaQuestions**資料表，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="53afb-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="53afb-235">![新增資料行的 SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "新增資料行的 SQL 陳述式產生")</span><span class="sxs-lookup"><span data-stu-id="53afb-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="53afb-236">*新增資料行的 SQL 陳述式產生*</span><span class="sxs-lookup"><span data-stu-id="53afb-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="53afb-237">在  **SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並確認新**提示**都會顯示資料行。</span><span class="sxs-lookup"><span data-stu-id="53afb-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="53afb-238">![顯示新的 「 提示 」 資料行](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "顯示新的 「 提示 」 資料行")</span><span class="sxs-lookup"><span data-stu-id="53afb-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="53afb-239">*顯示新的 「 提示 」 資料行*</span><span class="sxs-lookup"><span data-stu-id="53afb-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="53afb-240">回到**TriviaQuestion.cs**編輯器中，加入**StringLength**條件約束*提示*屬性，如下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="53afb-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="53afb-241">在  **Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="53afb-242">在  **Package Manager Console**，輸入下列命令，然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="53afb-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="53afb-243">輸出**Update-database**產生命令**Alter Table** SQL 陳述式來更新*提示*類型的資料行**TriviaQuestions**資料表，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="53afb-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="53afb-244">![Alter column SQL 陳述式產生](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter 資料行的 SQL 陳述式產生")</span><span class="sxs-lookup"><span data-stu-id="53afb-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="53afb-245">*Alter column SQL 陳述式產生*</span><span class="sxs-lookup"><span data-stu-id="53afb-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="53afb-246">在  **SQL Server 物件總管**，重新整理**dbo。TriviaQuestions**資料表，並檢查**提示**資料行類型是**nvarchar(150)**。</span><span class="sxs-lookup"><span data-stu-id="53afb-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="53afb-247">![顯示新的條件約束](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "顯示新的條件約束")</span><span class="sxs-lookup"><span data-stu-id="53afb-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="53afb-248">*顯示新的條件約束*</span><span class="sxs-lookup"><span data-stu-id="53afb-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="53afb-249">練習 2： 將 Web 應用程式部署至預備環境</span><span class="sxs-lookup"><span data-stu-id="53afb-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="53afb-250">**Web 應用程式在 Azure App Service 中的**可讓您執行的預備的發行。</span><span class="sxs-lookup"><span data-stu-id="53afb-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="53afb-251">預備的發行會建立一個預備網站位置，為每個預設生產網站，並可讓您交換這些位置不需要停機。</span><span class="sxs-lookup"><span data-stu-id="53afb-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="53afb-252">這是實際用於公開釋放之前，先驗證變更，以累加方式整合網站內容，並回復變更無法如預期運作。</span><span class="sxs-lookup"><span data-stu-id="53afb-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="53afb-253">在這個練習中，您將部署**Geek 測驗**到預備環境使用 Git 原始檔控制的 web 應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="53afb-254">若要這樣做，您會建立 web 應用程式和佈建所需的元件，在管理入口網站，請設定**Git**來源從本機電腦至預備位置的程式碼存放庫和推播應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="53afb-255">您也會更新您的生產環境資料庫**Code First 移轉**您在前一個練習中建立。</span><span class="sxs-lookup"><span data-stu-id="53afb-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="53afb-256">您接著將此測試環境，以驗證其作業中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="53afb-257">當您滿意其運作根據您的預期，您將會升級到生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="53afb-258">若要啟用預備的發行，web 應用程式必須處於**標準模式**。</span><span class="sxs-lookup"><span data-stu-id="53afb-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="53afb-259">請注意，是否您變更為標準模式的 web 應用程式，將會產生額外費用。</span><span class="sxs-lookup"><span data-stu-id="53afb-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="53afb-260">如需有關定價的詳細資訊，請參閱 < [App Service 定價](https://azure.microsoft.com/pricing/details/app-service/)。</span><span class="sxs-lookup"><span data-stu-id="53afb-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="53afb-261">工作 1-在 Azure App Service 中建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="53afb-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="53afb-262">在這個工作中，您將建立的 web 應用程式**Azure App Service**從管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="53afb-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="53afb-263">您也會設定**SQL Database**保存應用程式資料，並設定原始檔控制的本機 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="53afb-264">移至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![登入 Azure 管理入口網站](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="53afb-266">*登入 Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="53afb-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="53afb-267">按一下 **新增**在頁面底部的命令列中。</span><span class="sxs-lookup"><span data-stu-id="53afb-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="53afb-268">![建立新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "建立新的 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="53afb-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="53afb-269">*建立新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="53afb-270">按一下 **計算**，**網站**，然後**自訂建立**。</span><span class="sxs-lookup"><span data-stu-id="53afb-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="53afb-271">![建立新的 web 應用程式，使用 自訂建立](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "建立新的 web 應用程式，使用 自訂建立")</span><span class="sxs-lookup"><span data-stu-id="53afb-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="53afb-272">*建立新的 web 應用程式，使用 自訂建立*</span><span class="sxs-lookup"><span data-stu-id="53afb-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="53afb-273">在**新的網站-自訂建立**對話方塊方塊中，提供可用**URL** (例如*geek 測驗*)，選取中的位置**區域**下拉式清單中，然後選取**建立新的 SQL database**中**資料庫**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="53afb-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="53afb-274">最後，選取**從原始檔控制發行**核取方塊，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="53afb-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![自訂新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="53afb-276">*自訂新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="53afb-277">指定資料庫設定的下列資訊：</span><span class="sxs-lookup"><span data-stu-id="53afb-277">Specify the following information for the database settings:</span></span>

   - <span data-ttu-id="53afb-278">在 **名稱**文字方塊中，輸入資料庫名稱 (例如*geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="53afb-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
   - <span data-ttu-id="53afb-279">在伺服器中**下拉式清單**清單中，選取**新的 SQL database 伺服器**。</span><span class="sxs-lookup"><span data-stu-id="53afb-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="53afb-280">或者，您可以選取現有的伺服器。</span><span class="sxs-lookup"><span data-stu-id="53afb-280">Alternatively, you can select an existing server.</span></span>
   - <span data-ttu-id="53afb-281">在 **資料庫使用者名稱**並**資料庫密碼**方塊中，輸入 SQL database 伺服器的系統管理員使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="53afb-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="53afb-282">如果您選取的伺服器已經建立，您將會提示輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="53afb-282">If you select a server you have already created, you will be prompted for the password.</span></span>

     ![指定資料庫設定](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     <span data-ttu-id="53afb-284">*指定資料庫設定*</span><span class="sxs-lookup"><span data-stu-id="53afb-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="53afb-285">按 [下一步]  以繼續。</span><span class="sxs-lookup"><span data-stu-id="53afb-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="53afb-286">選取 **本機 Git 儲存機制**使用，並按一下 原始檔控制**下一步**。</span><span class="sxs-lookup"><span data-stu-id="53afb-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-287">您可能會提示您的部署認證 （使用者名稱和密碼）。</span><span class="sxs-lookup"><span data-stu-id="53afb-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![建立 Git 存放庫](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="53afb-289">*建立 Git 存放庫*</span><span class="sxs-lookup"><span data-stu-id="53afb-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="53afb-290">等候，直到建立新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-291">根據預設，Azure 會提供網域*azurewebsites.net*同時也提供您設定使用 Azure 管理入口網站的自訂網域的可能性。</span><span class="sxs-lookup"><span data-stu-id="53afb-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="53afb-292">不過，您只可以管理自訂網域，如果您使用特定的 Azure App Service 模式。</span><span class="sxs-lookup"><span data-stu-id="53afb-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    >
    > <span data-ttu-id="53afb-293">Azure App Service 是免費、 共用、 基本、 標準和 Premium edition 提供。</span><span class="sxs-lookup"><span data-stu-id="53afb-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="53afb-294">在免費與共用模式中，所有的 web 應用程式會在多租用戶環境中執行，並有 CPU、 記憶體和網路使用量的配額。</span><span class="sxs-lookup"><span data-stu-id="53afb-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="53afb-295">免費的應用程式的最大數目與您計劃而有所不同。</span><span class="sxs-lookup"><span data-stu-id="53afb-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="53afb-296">在標準模式中，您可以選擇哪些應用程式在對應的專用虛擬機器上執行的標準 azure 計算資源。</span><span class="sxs-lookup"><span data-stu-id="53afb-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="53afb-297">您可以找到中的 web 應用程式模式組態**擴展**web 應用程式的功能表。</span><span class="sxs-lookup"><span data-stu-id="53afb-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    >
    > <span data-ttu-id="53afb-298">![Azure App Service 模式](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service 模式")</span><span class="sxs-lookup"><span data-stu-id="53afb-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    >
    > <span data-ttu-id="53afb-299">如果您使用**Shared**或是**標準**模式中，您將能夠移至您的應用程式中管理 web 應用程式的自訂網域**設定**功能表，然後按一下**管理網域**底下*網域名稱*。</span><span class="sxs-lookup"><span data-stu-id="53afb-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    >
    > <span data-ttu-id="53afb-300">![管理網域](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "管理網域")</span><span class="sxs-lookup"><span data-stu-id="53afb-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    >
    > <span data-ttu-id="53afb-301">![管理自訂網域](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "管理自訂網域")</span><span class="sxs-lookup"><span data-stu-id="53afb-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="53afb-302">Web 應用程式建立之後，請按一下下方的連結**URL**檢查新的 web 應用程式是否正在執行的資料行。</span><span class="sxs-lookup"><span data-stu-id="53afb-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![瀏覽至新的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="53afb-304">*瀏覽至新的 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-304">*Browsing to the new web app*</span></span>

    ![執行 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="53afb-306">*執行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="53afb-307">工作 2-建立生產環境 SQL 資料庫</span><span class="sxs-lookup"><span data-stu-id="53afb-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="53afb-308">在這個工作中，您將使用**Entity Framework Code First Migrations**若要建立的資料庫目標**Azure SQL Database**您在上一個工作中建立的執行個體。</span><span class="sxs-lookup"><span data-stu-id="53afb-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="53afb-309">在管理入口網站中，瀏覽至您在上一個工作中建立 web 應用程式並移至其**儀表板**。</span><span class="sxs-lookup"><span data-stu-id="53afb-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="53afb-310">在 **儀表板**頁面上，按一下**檢視連接字串**連結底下**快速概覽**一節。</span><span class="sxs-lookup"><span data-stu-id="53afb-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="53afb-311">![檢視連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "檢視連接字串")</span><span class="sxs-lookup"><span data-stu-id="53afb-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="53afb-312">*檢視連接字串*</span><span class="sxs-lookup"><span data-stu-id="53afb-312">*View connection strings*</span></span>
3. <span data-ttu-id="53afb-313">複製**連接字串**值，並關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="53afb-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="53afb-314">![在 Azure 管理入口網站的連接字串](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "在 Azure 管理入口網站的連接字串")</span><span class="sxs-lookup"><span data-stu-id="53afb-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="53afb-315">*在 Azure 管理入口網站的連接字串*</span><span class="sxs-lookup"><span data-stu-id="53afb-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="53afb-316">按一下  **SQL Database**若要查看 Azure 中的 SQL 資料庫清單</span><span class="sxs-lookup"><span data-stu-id="53afb-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="53afb-317">![SQL Database 功能表](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database 功能表")</span><span class="sxs-lookup"><span data-stu-id="53afb-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="53afb-318">*SQL Database 功能表*</span><span class="sxs-lookup"><span data-stu-id="53afb-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="53afb-319">找出您在上一個工作中建立的資料庫，並按一下 在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="53afb-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="53afb-320">![SQL Database 伺服器](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database 伺服器")</span><span class="sxs-lookup"><span data-stu-id="53afb-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="53afb-321">*SQL Database 伺服器*</span><span class="sxs-lookup"><span data-stu-id="53afb-321">*SQL Database server*</span></span>
6. <span data-ttu-id="53afb-322">在 **快速入門**頁面上的伺服器上，按一下**設定**。</span><span class="sxs-lookup"><span data-stu-id="53afb-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="53afb-323">![設定 功能表](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "設定功能表")</span><span class="sxs-lookup"><span data-stu-id="53afb-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="53afb-324">*設定功能表*</span><span class="sxs-lookup"><span data-stu-id="53afb-324">*Configure menu*</span></span>
7. <span data-ttu-id="53afb-325">在 **允許的 IP 位址**區段中，按一下**加入至允許的 IP 位址**連結以啟用您的 IP 連線到 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="53afb-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="53afb-326">![允許的 IP 位址](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "允許的 IP 位址")</span><span class="sxs-lookup"><span data-stu-id="53afb-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="53afb-327">*允許的 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="53afb-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="53afb-328">按一下 **儲存**底部的頁面，即可完成的步驟。</span><span class="sxs-lookup"><span data-stu-id="53afb-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="53afb-329">切換回到 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="53afb-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="53afb-330">在  **Package Manager Console**，執行下列命令取代 *[您連接字串]* 預留位置取代為您從 Azure 複製的連接字串</span><span class="sxs-lookup"><span data-stu-id="53afb-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="53afb-331">![更新目標 Windows Azure SQL Database 的資料庫](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "更新目標 Windows Azure SQL Database 的資料庫")</span><span class="sxs-lookup"><span data-stu-id="53afb-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="53afb-332">*更新目標 Azure SQL Database 的資料庫*</span><span class="sxs-lookup"><span data-stu-id="53afb-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="53afb-333">工作 3-部署到預備環境使用 Git 的玩家測驗</span><span class="sxs-lookup"><span data-stu-id="53afb-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="53afb-334">在這個工作中，您將 web 應用程式中啟用預備的發行。</span><span class="sxs-lookup"><span data-stu-id="53afb-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="53afb-335">然後，您將使用 Git 發行 Geek 測驗應用程式直接從您的本機電腦到您的 web 應用程式的預備環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="53afb-336">返回入口網站並按一下底下的 web 應用程式名稱**名稱**資料行來顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![開啟 web 應用程式管理頁面](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="53afb-338">*開啟 web 應用程式管理頁面*</span><span class="sxs-lookup"><span data-stu-id="53afb-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="53afb-339">瀏覽至**擴展**頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="53afb-340">底下**一般**區段中，選取**標準**的設定，然後按一下**儲存**命令列中。</span><span class="sxs-lookup"><span data-stu-id="53afb-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-341">在目前的區域和訂用帳戶中的執行所有的 web 應用程式**標準**模式中，保持**全選**中所選取的核取方塊**選擇站台**組態。</span><span class="sxs-lookup"><span data-stu-id="53afb-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="53afb-342">否則，請清除**全選**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="53afb-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="53afb-343">![升級為標準模式的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "升級為標準模式的 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="53afb-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="53afb-344">*升級為標準模式的 Web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="53afb-345">按一下 **是**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="53afb-346">![確認為標準模式的變更](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "繼續進行變更的 web 應用程式模式")</span><span class="sxs-lookup"><span data-stu-id="53afb-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="53afb-347">*確認為標準模式的變更*</span><span class="sxs-lookup"><span data-stu-id="53afb-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="53afb-348">移至**儀表板**頁面，然後按一下**啟用預備發行**之下**快速概覽**一節。</span><span class="sxs-lookup"><span data-stu-id="53afb-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="53afb-349">![啟用預備的發行](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "啟用預備發行")</span><span class="sxs-lookup"><span data-stu-id="53afb-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="53afb-350">*啟用預備的發行*</span><span class="sxs-lookup"><span data-stu-id="53afb-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="53afb-351">按一下 **是**才能啟用預備的發行。</span><span class="sxs-lookup"><span data-stu-id="53afb-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="53afb-352">![確認預備發行](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "按一下 [是] 啟用預備的發行")</span><span class="sxs-lookup"><span data-stu-id="53afb-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="53afb-353">*確認預備的發行*</span><span class="sxs-lookup"><span data-stu-id="53afb-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="53afb-354">在 web 應用程式清單中，展開左側的 您的 web 應用程式名稱，以顯示預備網站位置的標記。</span><span class="sxs-lookup"><span data-stu-id="53afb-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="53afb-355">它具有的名稱，後面加上的 web 應用程式 ***（預備）***。</span><span class="sxs-lookup"><span data-stu-id="53afb-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="53afb-356">按一下以移至 [管理] 頁面的預備網站。</span><span class="sxs-lookup"><span data-stu-id="53afb-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="53afb-357">![瀏覽至預備 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "瀏覽至預備 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="53afb-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="53afb-358">*瀏覽至預備應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="53afb-359">請注意，他管理頁面看起來像任何其他 web 應用程式的管理頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="53afb-360">瀏覽至**部署**頁面，然後複製**Git URL**值。</span><span class="sxs-lookup"><span data-stu-id="53afb-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="53afb-361">您將會稍後在本練習中使用它。</span><span class="sxs-lookup"><span data-stu-id="53afb-361">You will use it later in this exercise.</span></span>

    ![複製的 Git URL 值](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="53afb-363">*複製的 Git URL 值*</span><span class="sxs-lookup"><span data-stu-id="53afb-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="53afb-364">開啟新**Git Bash**主控台並執行下列命令。</span><span class="sxs-lookup"><span data-stu-id="53afb-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="53afb-365">更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**方案，位於**Source\Ex1 DeployingWebSiteToStaging\Begin**資料夾這個實驗室中。</span><span class="sxs-lookup"><span data-stu-id="53afb-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git 初始化 」 和 「 第一次認可](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="53afb-367">*Git 初始化 」 和 「 第一次認可*</span><span class="sxs-lookup"><span data-stu-id="53afb-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="53afb-368">執行下列命令，以將您的 web 應用程式推送至遠端**Git**存放庫。</span><span class="sxs-lookup"><span data-stu-id="53afb-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="53afb-369">預留位置取代為您從管理入口網站取得的 URL。</span><span class="sxs-lookup"><span data-stu-id="53afb-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="53afb-370">將提示您為您的部署密碼。</span><span class="sxs-lookup"><span data-stu-id="53afb-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![推送至 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="53afb-372">*推送至 Azure*</span><span class="sxs-lookup"><span data-stu-id="53afb-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-373">當您將內容部署至的 FTP 主機或 GIT 儲存機制的 web 應用程式時，您必須使用進行驗證**部署認證**您從 web 應用程式的建立**快速入門**或**儀表板**管理頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="53afb-374">如果您不知道您的部署認證可以輕鬆地重設它們使用管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="53afb-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="53afb-375">開啟 web 應用程式**儀表板**頁面，然後按一下**重設您的部署認證**連結。</span><span class="sxs-lookup"><span data-stu-id="53afb-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="53afb-376">提供新密碼，然後按**確定**。</span><span class="sxs-lookup"><span data-stu-id="53afb-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="53afb-377">是部署認證都適用於使用與您訂用帳戶相關聯的所有 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="53afb-378">若要確認 web 應用程式已成功推送至 Azure，請回到管理入口網站，並按一下**網站**。</span><span class="sxs-lookup"><span data-stu-id="53afb-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="53afb-379">找出您的 web 應用程式，然後展開以顯示預備網站位置的項目。</span><span class="sxs-lookup"><span data-stu-id="53afb-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="53afb-380">按一下它**名稱**移至 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="53afb-381">按一下 **部署**若要查看**部署歷程記錄**。</span><span class="sxs-lookup"><span data-stu-id="53afb-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="53afb-382">確認沒有**現用部署**與您*&quot;初始認可&quot;*。</span><span class="sxs-lookup"><span data-stu-id="53afb-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![作用中的部署](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="53afb-384">*作用中的部署*</span><span class="sxs-lookup"><span data-stu-id="53afb-384">*Active deployment*</span></span>
13. <span data-ttu-id="53afb-385">最後，按一下**瀏覽**移至 web 應用程式的命令列中。</span><span class="sxs-lookup"><span data-stu-id="53afb-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![瀏覽 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="53afb-387">*瀏覽 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-387">*Browse web app*</span></span>
14. <span data-ttu-id="53afb-388">如果已成功部署應用程式，您會看到 Geek 測驗的 [登入] 頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-389">所部署的應用程式的 URL 包含後面接著的 web 應用程式名稱 *-預備*。</span><span class="sxs-lookup"><span data-stu-id="53afb-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![在預備環境中執行的應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="53afb-391">*在預備環境中執行的應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="53afb-392">如果您想要探索應用程式，請按一下**註冊**註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="53afb-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="53afb-393">輸入使用者名稱、 電子郵件地址和密碼，以完成帳戶詳細資料。</span><span class="sxs-lookup"><span data-stu-id="53afb-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="53afb-394">接下來，應用程式會顯示測驗的第一個問題。</span><span class="sxs-lookup"><span data-stu-id="53afb-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="53afb-395">回答一些問題，以確定它正常運作。</span><span class="sxs-lookup"><span data-stu-id="53afb-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![已備妥可用的應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="53afb-397">*已備妥可用的應用程式*</span><span class="sxs-lookup"><span data-stu-id="53afb-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="53afb-398">工作 4-升級至生產環境 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="53afb-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="53afb-399">既然您已驗證的 web 應用程式在預備環境中正常運作，您就將其升階到生產環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="53afb-400">在這個工作中，您將會交換預備網站位置與生產站台位置。</span><span class="sxs-lookup"><span data-stu-id="53afb-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="53afb-401">返回管理入口網站，並選取 預備網站位置。</span><span class="sxs-lookup"><span data-stu-id="53afb-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="53afb-402">按一下 **交換**命令列中。</span><span class="sxs-lookup"><span data-stu-id="53afb-402">Click **Swap** in the command bar.</span></span>

    ![切換至生產環境](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="53afb-404">*切換至生產環境*</span><span class="sxs-lookup"><span data-stu-id="53afb-404">*Swap to production*</span></span>
2. <span data-ttu-id="53afb-405">按一下 **是**在確認對話方塊中，若要繼續進行交換作業。</span><span class="sxs-lookup"><span data-stu-id="53afb-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="53afb-406">Azure 會立即在交換生產網站的預備網站內容的內容。</span><span class="sxs-lookup"><span data-stu-id="53afb-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-407">某些設定從預備的版本將自動複製到實際執行版本 （例如連接字串覆寫處理常式對應、 等），但其他設定不會變更 （例如 DNS 端點、 SSL 繫結等）。</span><span class="sxs-lookup"><span data-stu-id="53afb-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![確認交換作業](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="53afb-409">*確認交換作業*</span><span class="sxs-lookup"><span data-stu-id="53afb-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="53afb-410">交換完成後，選取 生產位置，然後按一下**瀏覽**若要開啟 將生產站台的命令列中。</span><span class="sxs-lookup"><span data-stu-id="53afb-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="53afb-411">請注意網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="53afb-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-412">您可能需要重新整理瀏覽器來清除快取。</span><span class="sxs-lookup"><span data-stu-id="53afb-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="53afb-413">在 Internet Explorer 中，您可以藉由按下**CTRL + R**。</span><span class="sxs-lookup"><span data-stu-id="53afb-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![在生產環境中執行的 web 應用程式](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="53afb-415">在  **GitBash**主控台中，更新本機 Git 存放庫為目標的生產位置的遠端 URL。</span><span class="sxs-lookup"><span data-stu-id="53afb-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="53afb-416">若要這樣做，請執行下列命令預留位置取代為您的部署使用者名稱和 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="53afb-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-417">在下列練習中，您會將變更推送到生產網站，而不是只為了簡單起見實驗室的預備環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="53afb-418">在真實世界案例中，建議將升階到生產環境之前，請先驗證預備環境中的變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="53afb-419">練習 3： 在生產環境中執行部署復原</span><span class="sxs-lookup"><span data-stu-id="53afb-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="53afb-420">其中您沒有預備位置來執行熱插拔之間預備和生產環境，例如，如果您正在使用的情況下**空閒**或是**共用**模式。</span><span class="sxs-lookup"><span data-stu-id="53afb-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="53afb-421">在這些情況下，您應該測試您的應用程式，在測試環境中 – 在本機或遠端站台中 – 之前部署至生產環境。</span><span class="sxs-lookup"><span data-stu-id="53afb-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="53afb-422">不過，就可以在測試階段偵測不到的問題可能會發生生產網站中。</span><span class="sxs-lookup"><span data-stu-id="53afb-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="53afb-423">在此情況下，務必要有機制，以輕鬆地切換至先前且更穩定版本的應用程式儘速。</span><span class="sxs-lookup"><span data-stu-id="53afb-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="53afb-424">在  **Azure App Service**，從原始檔控制的連續部署讓此可能感謝**重新部署**管理入口網站中可用動作。</span><span class="sxs-lookup"><span data-stu-id="53afb-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="53afb-425">Azure 會追蹤與推送至儲存機制的認可相關聯的部署，並提供選項來重新部署您的應用程式使用任何先前的部署中，在任何時間。</span><span class="sxs-lookup"><span data-stu-id="53afb-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="53afb-426">在這個練習中，您將執行中的程式碼變更**Geek 測驗**刻意插入的應用程式*bug*。</span><span class="sxs-lookup"><span data-stu-id="53afb-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="53afb-427">您將部署到生產環境應用程式，以查看錯誤，並接著您會利用重新部署功能，請回到先前的狀態。</span><span class="sxs-lookup"><span data-stu-id="53afb-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="53afb-428">工作 1-Geek 測驗應用程式更新</span><span class="sxs-lookup"><span data-stu-id="53afb-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="53afb-429">在這個工作中，重構程式碼的一小段**TriviaController**類別來擷取從資料庫擷取選取的測驗選項，為新方法的邏輯的一部分。</span><span class="sxs-lookup"><span data-stu-id="53afb-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="53afb-430">切換至 Visual Studio 執行個體**GeekQuiz**從上一個練習的解決方案。</span><span class="sxs-lookup"><span data-stu-id="53afb-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="53afb-431">在 [**方案總管] 中**，開啟**TriviaController.cs**檔案**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="53afb-432">找出**StoreAsync**方法，然後選取程式碼以在下圖中反白顯示。</span><span class="sxs-lookup"><span data-stu-id="53afb-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![選取的程式碼](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="53afb-434">*選取的程式碼*</span><span class="sxs-lookup"><span data-stu-id="53afb-434">*Selecting the code*</span></span>
4. <span data-ttu-id="53afb-435">以滑鼠右鍵按一下選取的程式碼中，展開**重構**功能表，然後選取**擷取方法...**.</span><span class="sxs-lookup"><span data-stu-id="53afb-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![擷取程式碼做為新的方法](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="53afb-437">*選取 擷取方法*</span><span class="sxs-lookup"><span data-stu-id="53afb-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="53afb-438">在 [**擷取方法**] 對話方塊中，名稱的新方法*MatchesOption*然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![指定方法名稱](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="53afb-440">*指定擷取方法的名稱*</span><span class="sxs-lookup"><span data-stu-id="53afb-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="53afb-441">選取的程式碼接著會擷取至**MatchesOption**方法。</span><span class="sxs-lookup"><span data-stu-id="53afb-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="53afb-442">產生的程式碼是以下列程式碼片段所示。</span><span class="sxs-lookup"><span data-stu-id="53afb-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="53afb-443">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="53afb-444">工作 2-重新部署 Geek 測驗應用程式</span><span class="sxs-lookup"><span data-stu-id="53afb-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="53afb-445">您現在會推送至儲存機制，將會觸發新的部署到生產環境所做先前工作中的變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="53afb-446">然後，您將在其中進行問題使用**F12 開發工具**提供 Internet Explorer 中，再回復到先前的部署從 Azure 管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="53afb-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="53afb-447">開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="53afb-448">執行下列命令，以將變更推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="53afb-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="53afb-449">更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**解決方案。</span><span class="sxs-lookup"><span data-stu-id="53afb-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="53afb-450">將提示您為您的部署密碼。</span><span class="sxs-lookup"><span data-stu-id="53afb-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![將重構程式碼推送至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="53afb-452">*將重構程式碼推送至 Azure*</span><span class="sxs-lookup"><span data-stu-id="53afb-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="53afb-453">開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="53afb-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="53afb-454">使用先前建立的認證登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="53afb-455">按下**F12**若要啟動的開發工具，選取**網路**索引標籤，然後按一下**播放**開始錄製 按鈕。</span><span class="sxs-lookup"><span data-stu-id="53afb-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="53afb-456">![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "啟動網路記錄")</span><span class="sxs-lookup"><span data-stu-id="53afb-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="53afb-457">*啟動網路記錄*</span><span class="sxs-lookup"><span data-stu-id="53afb-457">*Starting network recording*</span></span>
5. <span data-ttu-id="53afb-458">選取任何選項的測驗。</span><span class="sxs-lookup"><span data-stu-id="53afb-458">Select any option of the quiz.</span></span> <span data-ttu-id="53afb-459">您會看到沒有任何反應。</span><span class="sxs-lookup"><span data-stu-id="53afb-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="53afb-460">在 [ **F12** ] 視窗中，對應至 POST HTTP 要求的項目會顯示 HTTP **500**結果。</span><span class="sxs-lookup"><span data-stu-id="53afb-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 錯誤](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="53afb-462">*HTTP 500 錯誤*</span><span class="sxs-lookup"><span data-stu-id="53afb-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="53afb-463">選取 [**主控台**] 索引標籤。錯誤會記錄原因的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="53afb-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![記錄的錯誤](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="53afb-465">*記錄的錯誤*</span><span class="sxs-lookup"><span data-stu-id="53afb-465">*Logged error*</span></span>
8. <span data-ttu-id="53afb-466">找出錯誤的詳細資料部分。</span><span class="sxs-lookup"><span data-stu-id="53afb-466">Locate the details part of the error.</span></span> <span data-ttu-id="53afb-467">很明顯地，此錯誤是重構前一個步驟中認可您的程式碼所造成。</span><span class="sxs-lookup"><span data-stu-id="53afb-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="53afb-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="53afb-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="53afb-469">請勿關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="53afb-469">Do not close the browser.</span></span>
10. <span data-ttu-id="53afb-470">在新的瀏覽器執行個體中，瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="53afb-471">選取 **網站**按一下您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="53afb-472">瀏覽至**部署**頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="53afb-473">請注意，執行的所有認可會都列出在部署歷程記錄中。</span><span class="sxs-lookup"><span data-stu-id="53afb-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![現有部署的清單](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="53afb-475">*現有部署的清單*</span><span class="sxs-lookup"><span data-stu-id="53afb-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="53afb-476">選取 上一次認可，然後按一下**重新部署**命令列上。</span><span class="sxs-lookup"><span data-stu-id="53afb-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![重新部署先前的認可](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="53afb-478">*重新部署先前的認可*</span><span class="sxs-lookup"><span data-stu-id="53afb-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="53afb-479">當系統提示您確認時，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="53afb-479">When prompted to confirm, click **Yes**.</span></span>

    ![確認重新部署](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="53afb-481">當部署完成後時，切換回瀏覽器執行個體，與您的 web 應用程式，然後按**CTRL + F5**。</span><span class="sxs-lookup"><span data-stu-id="53afb-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="53afb-482">按一下任何一個選項。</span><span class="sxs-lookup"><span data-stu-id="53afb-482">Click any of the options.</span></span> <span data-ttu-id="53afb-483">翻頁動畫的動畫現在需要的地方和結果 (*正確/不正確*) 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="53afb-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="53afb-484">（選擇性）若要切換**Git Bash**主控台並執行下列命令來還原成先前的認可。</span><span class="sxs-lookup"><span data-stu-id="53afb-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-485">這些命令會建立新的認可，復原 Git 存放庫中的不正確的認可所做的所有變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="53afb-486">Azure 會再重新部署應用程式使用新的認可。</span><span class="sxs-lookup"><span data-stu-id="53afb-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="53afb-487">使用 Azure 儲存體縮放練習 4:</span><span class="sxs-lookup"><span data-stu-id="53afb-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="53afb-488">**Blob**是最簡單的方式來儲存大量非結構化的文字或二進位資料如視訊、 音訊和影像。</span><span class="sxs-lookup"><span data-stu-id="53afb-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="53afb-489">移至儲存體的應用程式的靜態內容，可協助調整應用程式所提供的映像或直接到瀏覽器的文件。</span><span class="sxs-lookup"><span data-stu-id="53afb-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="53afb-490">在此練習中，您會將移至 Blob 容器的應用程式的靜態內容。</span><span class="sxs-lookup"><span data-stu-id="53afb-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="53afb-491">然後您將設定您的應用程式，以新增**ASP.NET URL 重寫規則**中**Web.config**重新導向至 Blob 容器的內容。</span><span class="sxs-lookup"><span data-stu-id="53afb-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="53afb-492">工作 1-建立 Azure 儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="53afb-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="53afb-493">在這項工作中，您將了解如何建立新的儲存體帳戶，使用管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="53afb-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="53afb-494">瀏覽至[Azure 管理入口網站](https://manage.windowsazure.com)並使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="53afb-495">選取**新 |資料服務 |儲存體 |快速建立**開始建立新的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="53afb-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="53afb-496">輸入唯一的名稱，為該帳戶，然後選取**地區**從清單中。</span><span class="sxs-lookup"><span data-stu-id="53afb-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="53afb-497">按一下 **建立儲存體帳戶**以繼續。</span><span class="sxs-lookup"><span data-stu-id="53afb-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="53afb-498">![建立新的儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "建立新的儲存體帳戶")</span><span class="sxs-lookup"><span data-stu-id="53afb-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="53afb-499">*建立新的儲存體帳戶*</span><span class="sxs-lookup"><span data-stu-id="53afb-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="53afb-500">在 **儲存體**區段中，等到新的儲存體帳戶的狀態會變更為*線上*才能繼續進行下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="53afb-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="53afb-501">![建立儲存體帳戶](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "建立儲存體帳戶")</span><span class="sxs-lookup"><span data-stu-id="53afb-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="53afb-502">*建立儲存體帳戶*</span><span class="sxs-lookup"><span data-stu-id="53afb-502">*Storage Account created*</span></span>
4. <span data-ttu-id="53afb-503">按一下儲存體帳戶名稱，然後按一下**儀表板**在頁面頂端的連結。</span><span class="sxs-lookup"><span data-stu-id="53afb-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="53afb-504">**儀表板**頁面可提供您的帳戶，並且可用於您的應用程式的服務端點的狀態相關資訊。</span><span class="sxs-lookup"><span data-stu-id="53afb-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="53afb-505">![顯示儲存體帳戶儀表板](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "顯示儲存體帳戶儀表板")</span><span class="sxs-lookup"><span data-stu-id="53afb-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="53afb-506">*顯示儲存體帳戶儀表板*</span><span class="sxs-lookup"><span data-stu-id="53afb-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="53afb-507">按一下 **管理存取金鑰**導覽列中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="53afb-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="53afb-508">![管理存取金鑰 按鈕](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "管理存取金鑰 按鈕")</span><span class="sxs-lookup"><span data-stu-id="53afb-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="53afb-509">*管理存取金鑰 按鈕*</span><span class="sxs-lookup"><span data-stu-id="53afb-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="53afb-510">在**管理存取金鑰** 對話方塊中，複製**儲存體帳戶名稱**並**主要存取金鑰**當您將需要在下列練習中。</span><span class="sxs-lookup"><span data-stu-id="53afb-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="53afb-511">然後，關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="53afb-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="53afb-512">![管理存取金鑰 對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "管理存取金鑰 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="53afb-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="53afb-513">*管理存取金鑰 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="53afb-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="53afb-514">工作 2-將資產上傳至 Azure Blob 儲存體</span><span class="sxs-lookup"><span data-stu-id="53afb-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="53afb-515">在這個工作中，您將使用從 Visual Studio 的 [伺服器總管] 視窗來連接到您的儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="53afb-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="53afb-516">然後，您會建立 blob 容器，並 Geek 測驗標誌檔案上傳至容器。</span><span class="sxs-lookup"><span data-stu-id="53afb-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="53afb-517">切換至 Visual Studio 執行個體**GeekQuiz**從上一個練習的解決方案。</span><span class="sxs-lookup"><span data-stu-id="53afb-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="53afb-518">從功能表列中，選取**檢視**，然後按一下**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="53afb-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="53afb-519">在 **伺服器總管**，以滑鼠右鍵按一下**Azure**節點，然後選取**連線到 Azure...**.使用您的訂用帳戶相關聯的 Microsoft 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![連接到 Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="53afb-521">*連接到 Azure*</span><span class="sxs-lookup"><span data-stu-id="53afb-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="53afb-522">依序展開**Azure**節點，以滑鼠右鍵按一下**儲存體**，然後選取**附加外部儲存體...**.</span><span class="sxs-lookup"><span data-stu-id="53afb-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="53afb-523">在**加入新的儲存體帳戶**對話方塊方塊中，輸入**帳戶名稱**並**帳戶金鑰**您在上一個工作並按一下 取得**確定**.</span><span class="sxs-lookup"><span data-stu-id="53afb-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![加入新的儲存體帳戶對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="53afb-525">*加入新的儲存體帳戶對話方塊*</span><span class="sxs-lookup"><span data-stu-id="53afb-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="53afb-526">您的儲存體帳戶應該會出現下**儲存體**節點。</span><span class="sxs-lookup"><span data-stu-id="53afb-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="53afb-527">展開您的儲存體帳戶，以滑鼠右鍵按一下**Blob** ，然後選取**建立 Blob 容器...**.</span><span class="sxs-lookup"><span data-stu-id="53afb-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="53afb-528">![建立 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "建立 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="53afb-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="53afb-529">*建立 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="53afb-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="53afb-530">在 **建立 Blob 容器**對話方塊方塊中，輸入 blob 容器的名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="53afb-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="53afb-531">![建立 Blob 容器 對話方塊](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "建立 Blob 容器 對話方塊")</span><span class="sxs-lookup"><span data-stu-id="53afb-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="53afb-532">*建立 Blob 容器 對話方塊*</span><span class="sxs-lookup"><span data-stu-id="53afb-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="53afb-533">應加入新的 blob 容器**Blob**節點。</span><span class="sxs-lookup"><span data-stu-id="53afb-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="53afb-534">變更要將容器設為公用容器中的存取權限。</span><span class="sxs-lookup"><span data-stu-id="53afb-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="53afb-535">若要這樣做，請以滑鼠右鍵按一下**映像**容器，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="53afb-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="53afb-536">![映像容器屬性](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "映像容器屬性")</span><span class="sxs-lookup"><span data-stu-id="53afb-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="53afb-537">*映像容器屬性*</span><span class="sxs-lookup"><span data-stu-id="53afb-537">*Images container properties*</span></span>
9. <span data-ttu-id="53afb-538">在 **屬性**視窗中，將**公用讀取權限**來**容器**。</span><span class="sxs-lookup"><span data-stu-id="53afb-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="53afb-539">![變更公用讀取權限屬性](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "變更公用讀取權限屬性")</span><span class="sxs-lookup"><span data-stu-id="53afb-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="53afb-540">*變更公用讀取權限屬性*</span><span class="sxs-lookup"><span data-stu-id="53afb-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="53afb-541">當系統提示您是否確定要變更的公用存取屬性，請按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="53afb-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="53afb-542">![Microsoft Visual Studio 警告](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio 警告")</span><span class="sxs-lookup"><span data-stu-id="53afb-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="53afb-543">*Microsoft Visual Studio 警告*</span><span class="sxs-lookup"><span data-stu-id="53afb-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="53afb-544">在 **伺服器總管**，以滑鼠右鍵按一下**映像**blob 容器，然後選取**檢視 Blob 容器**。</span><span class="sxs-lookup"><span data-stu-id="53afb-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="53afb-545">![檢視 Blob 容器](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "檢視 Blob 容器")</span><span class="sxs-lookup"><span data-stu-id="53afb-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="53afb-546">*檢視 Blob 容器*</span><span class="sxs-lookup"><span data-stu-id="53afb-546">*View Blob Container*</span></span>
12. <span data-ttu-id="53afb-547">映像容器應該會開啟新視窗中，應該會顯示任何項目的圖例。</span><span class="sxs-lookup"><span data-stu-id="53afb-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="53afb-548">按一下 **上傳**圖示，以將檔案上傳至 blob 容器。</span><span class="sxs-lookup"><span data-stu-id="53afb-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="53afb-549">![無項目的映像容器](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "無項目的映像容器")</span><span class="sxs-lookup"><span data-stu-id="53afb-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="53afb-550">*無項目的映像容器*</span><span class="sxs-lookup"><span data-stu-id="53afb-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="53afb-551">在 **上傳 Blob**對話方塊方塊中，瀏覽至**資產**實驗室的資料夾。</span><span class="sxs-lookup"><span data-stu-id="53afb-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="53afb-552">選取 **標誌 big.png**檔案，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="53afb-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="53afb-553">請等到檔案上傳。</span><span class="sxs-lookup"><span data-stu-id="53afb-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="53afb-554">上傳完成時，檔案應該列在 映像容器。</span><span class="sxs-lookup"><span data-stu-id="53afb-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="53afb-555">以滑鼠右鍵按一下檔案項目，然後選取**複製 URL**。</span><span class="sxs-lookup"><span data-stu-id="53afb-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="53afb-556">![複製 blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "複製 blob 檔案的 URL")</span><span class="sxs-lookup"><span data-stu-id="53afb-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="53afb-557">*複製 blob URL*</span><span class="sxs-lookup"><span data-stu-id="53afb-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="53afb-558">開啟 Internet Explorer，並貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="53afb-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="53afb-559">下列映像應該會顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="53afb-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="53afb-560">![從 Windows Blob 儲存體的標誌 big.png 映像](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "標誌 big.png 映像從儲存體")</span><span class="sxs-lookup"><span data-stu-id="53afb-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="53afb-561">*從 Azure Blob 儲存體的標誌 big.png 映像*</span><span class="sxs-lookup"><span data-stu-id="53afb-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="53afb-562">工作 3-更新方案，以使用來自 Azure Blob 儲存體的靜態內容</span><span class="sxs-lookup"><span data-stu-id="53afb-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="53afb-563">在這個工作中，您會設定**GeekQuiz**解決方案，以使用映像上傳至 Azure Blob 儲存體 （而不是位於 web 應用程式中的映像） 新增中的 ASP.NET URL 重寫規則**web.config**檔案。</span><span class="sxs-lookup"><span data-stu-id="53afb-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="53afb-564">在 Visual Studio 中開啟**Web.config**檔案內**GeekQuiz**專案，然後找出**&lt;system.webServer&gt;** 項目。</span><span class="sxs-lookup"><span data-stu-id="53afb-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="53afb-565">加入下列程式碼，將 URL 重寫規則，更新您的儲存體帳戶名稱的預留位置。</span><span class="sxs-lookup"><span data-stu-id="53afb-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="53afb-566">(程式碼片段- *WebSitesInProduction-Ex4-UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="53afb-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="53afb-567">URL 重寫為攔截傳入的 Web 要求，並將要求重新導向至不同的資源的程序。</span><span class="sxs-lookup"><span data-stu-id="53afb-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="53afb-568">URL 重寫規則會告訴重寫引擎要求時必須重新導向，以及其中應該將他們重新導向。</span><span class="sxs-lookup"><span data-stu-id="53afb-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="53afb-569">重寫規則由兩個字串所組成： 在要求 URL 中尋找的模式 （通常使用規則運算式），並找到要取代的模式，如果字串。</span><span class="sxs-lookup"><span data-stu-id="53afb-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="53afb-570">如需詳細資訊，請參閱 < [URL 以 ASP.NET 重寫](https://msdn.microsoft.com/library/ms972974.aspx)。</span><span class="sxs-lookup"><span data-stu-id="53afb-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="53afb-571">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="53afb-572">開啟新**Git Bash**主控台部署至 Azure App Service 更新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="53afb-573">執行下列命令，以將變更推送至 Azure。</span><span class="sxs-lookup"><span data-stu-id="53afb-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="53afb-574">更新 *[路徑-您的應用程式]* 預留位置取代為路徑**GeekQuiz**解決方案。</span><span class="sxs-lookup"><span data-stu-id="53afb-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="53afb-575">將提示您為您的部署密碼。</span><span class="sxs-lookup"><span data-stu-id="53afb-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![將更新部署至 Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="53afb-577">*將更新部署至 Azure*</span><span class="sxs-lookup"><span data-stu-id="53afb-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="53afb-578">工作 4-驗證</span><span class="sxs-lookup"><span data-stu-id="53afb-578">Task 4 – Verification</span></span>

<span data-ttu-id="53afb-579">在這項工作會使用**Internet Explorer**瀏覽**Geek 測驗**應用程式，並檢查 URL 重寫規則的映像運作，您會被重新導向至映像裝載在**Azure Blob儲存體**。</span><span class="sxs-lookup"><span data-stu-id="53afb-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="53afb-580">開啟 Internet Explorer 並瀏覽至您的 web 應用程式 (例如`http://<your-web-site>.azurewebsites.net`)。</span><span class="sxs-lookup"><span data-stu-id="53afb-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="53afb-581">使用先前建立的認證登入。</span><span class="sxs-lookup"><span data-stu-id="53afb-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="53afb-582">![顯示玩家測驗 web 應用程式與映像](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "顯示玩家測驗 web 應用程式與映像")</span><span class="sxs-lookup"><span data-stu-id="53afb-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="53afb-583">*顯示玩家測驗 web 應用程式與映像*</span><span class="sxs-lookup"><span data-stu-id="53afb-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="53afb-584">按下**F12**若要啟動的開發工具，選取**網路**索引標籤，並開始錄製。</span><span class="sxs-lookup"><span data-stu-id="53afb-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="53afb-585">![啟動網路錄製](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "啟動網路記錄")</span><span class="sxs-lookup"><span data-stu-id="53afb-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="53afb-586">*啟動網路記錄*</span><span class="sxs-lookup"><span data-stu-id="53afb-586">*Starting network recording*</span></span>
3. <span data-ttu-id="53afb-587">按下**CTRL + F5**重新整理網頁。</span><span class="sxs-lookup"><span data-stu-id="53afb-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="53afb-588">網頁完成載入之後，您應該會看到的 HTTP 要求 **/img/logo-big.png** URL 含有 HTTP **301**結果 （重新導向） 和另一個要求`http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png`URL 含有 HTTP **200**結果。</span><span class="sxs-lookup"><span data-stu-id="53afb-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="53afb-589">![正在驗證的 URL 重新導向](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "開發人員工具中顯示重新導向")</span><span class="sxs-lookup"><span data-stu-id="53afb-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="53afb-590">*正在驗證重新導向 URL*</span><span class="sxs-lookup"><span data-stu-id="53afb-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="53afb-591">練習 5： 使用自動調整規模的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="53afb-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="53afb-592">此練習是選擇性的因為它需要支援的 Web 負載&amp;效能測試，僅適用於**Visual Studio 2013 Ultimate Edition**。</span><span class="sxs-lookup"><span data-stu-id="53afb-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="53afb-593">如需有關 Visual Studio 2013 的特定功能的詳細資訊，請比較版本[此處](https://www.microsoft.com/visualstudio/eng/products/compare)。</span><span class="sxs-lookup"><span data-stu-id="53afb-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="53afb-594">**Azure App Service Web Apps**中執行的 web 應用程式提供的自動調整功能**標準模式**。</span><span class="sxs-lookup"><span data-stu-id="53afb-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="53afb-595">自動調整可讓 Azure 自動調整您的 web 應用程式，根據負載的執行個體計數。</span><span class="sxs-lookup"><span data-stu-id="53afb-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="53afb-596">啟用自動調整時，Azure 會每 5 分鐘會檢查您的 web 應用程式的 CPU，並將執行個體，視需要在該時間點的。</span><span class="sxs-lookup"><span data-stu-id="53afb-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="53afb-597">如果 CPU 使用率偏低，Azure 將移除執行個體一次每隔 2 小時以確保您的 web 應用程式的效能不會降低。</span><span class="sxs-lookup"><span data-stu-id="53afb-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="53afb-598">在這個練習中，您會瀏覽設定所需的步驟**自動調整規模**功能**Geek 測驗**web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="53afb-599">您將會執行產生足夠的 CPU 負載，應用程式觸發程序的執行個體升級到 Visual Studio 負載測試來驗證這項功能。</span><span class="sxs-lookup"><span data-stu-id="53afb-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="53afb-600">工作 1-設定 CPU 度量為基礎的自動調整規模</span><span class="sxs-lookup"><span data-stu-id="53afb-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="53afb-601">在這項工作中，您將使用 Azure 管理入口網站來啟用自動調整功能，以便您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="53afb-602">在  [Azure 管理入口網站](https://manage.windowsazure.com/)，選取**網站**按一下您在練習 2 中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="53afb-603">瀏覽至**擴展**頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="53afb-604">底下**容量**區段中，選取**CPU** for**依度量調整規模**組態。</span><span class="sxs-lookup"><span data-stu-id="53afb-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-605">調整時的 CPU，Azure 會動態調整應用程式使用的 CPU 使用量變更時的執行個體數目。</span><span class="sxs-lookup"><span data-stu-id="53afb-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="53afb-606">![選取即可依 CPU 調整規模](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "選取 CPU 計量自動調整")</span><span class="sxs-lookup"><span data-stu-id="53afb-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="53afb-607">*選取即可依 CPU 調整規模*</span><span class="sxs-lookup"><span data-stu-id="53afb-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="53afb-608">變更**目標 CPU**組態**20**-**40**百分比。</span><span class="sxs-lookup"><span data-stu-id="53afb-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-609">這個範圍代表您的 web 應用程式的平均 CPU 使用量。</span><span class="sxs-lookup"><span data-stu-id="53afb-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="53afb-610">Azure 將會新增或移除執行個體，將您的 web 應用程式保留在這個範圍。</span><span class="sxs-lookup"><span data-stu-id="53afb-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="53afb-611">中指定用於調整的執行個體最小和最大數目**執行個體計數**組態。</span><span class="sxs-lookup"><span data-stu-id="53afb-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="53afb-612">Azure 永遠不會高於或超過該限制。</span><span class="sxs-lookup"><span data-stu-id="53afb-612">Azure will never go above or beyond that limit.</span></span>
    >
    > <span data-ttu-id="53afb-613">預設值**目標 CPU**值只會修改這個實驗室的目的。</span><span class="sxs-lookup"><span data-stu-id="53afb-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="53afb-614">藉由設定 CPU 範圍值較小，您會增加以觸發自動調整的機會中, 度的負載放在應用程式時。</span><span class="sxs-lookup"><span data-stu-id="53afb-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="53afb-615">![變更目標會介於 20 到 40%的 CPU](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "變更會介於 20 到 40%的目標 CPU")</span><span class="sxs-lookup"><span data-stu-id="53afb-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="53afb-616">*變更會介於 20 到 40%的目標 CPU*</span><span class="sxs-lookup"><span data-stu-id="53afb-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="53afb-617">按一下 **儲存**在命令列中儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="53afb-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="53afb-618">工作 2-使用 Visual Studio 負載測試</span><span class="sxs-lookup"><span data-stu-id="53afb-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="53afb-619">既然**自動調整規模**已設定，您將建立**Web 效能和負載測試專案**在 Visual Studio 中產生某些 web 應用程式上的 CPU 負載。</span><span class="sxs-lookup"><span data-stu-id="53afb-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="53afb-620">開啟**Visual Studio Ultimate 2013** ，然後選取**檔案 |新 |專案...** 啟動新的解決方案。</span><span class="sxs-lookup"><span data-stu-id="53afb-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="53afb-621">![建立新的專案](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "建立新的專案")</span><span class="sxs-lookup"><span data-stu-id="53afb-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="53afb-622">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="53afb-622">*Creating a new project*</span></span>
2. <span data-ttu-id="53afb-623">在 [**新的專案**對話方塊中，選取**Web 效能和負載測試專案**下**Visual C# |測試**] 索引標籤。請確定 **.NET Framework 4.5**是選取，將專案命名為*WebAndLoadTestProject*，選擇**位置**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="53afb-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="53afb-624">![建立新的 Web 和負載測試專案](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "建立新的 Web 和負載測試專案")</span><span class="sxs-lookup"><span data-stu-id="53afb-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="53afb-625">*建立新的 Web 和負載測試專案*</span><span class="sxs-lookup"><span data-stu-id="53afb-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="53afb-626">在  **WebTest1.webtest**上按一下滑鼠右鍵**WebTest1**節點，然後按一下**加入要求**。</span><span class="sxs-lookup"><span data-stu-id="53afb-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="53afb-627">![將要求加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1 中加入要求")</span><span class="sxs-lookup"><span data-stu-id="53afb-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="53afb-628">*將要求加入至 WebTest1*</span><span class="sxs-lookup"><span data-stu-id="53afb-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="53afb-629">在 [**屬性**] 視窗的 [新要求] 節點中，更新**Url**屬性以指向您的 web 應用程式的 URL (例如*[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="53afb-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="53afb-630">![變更 Url 屬性](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "變更 Url 屬性")</span><span class="sxs-lookup"><span data-stu-id="53afb-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="53afb-631">*變更 Url 屬性*</span><span class="sxs-lookup"><span data-stu-id="53afb-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="53afb-632">在  **WebTest1.webtest**  視窗中，以滑鼠右鍵按一下**WebTest1** ，按一下 **加入迴圈...**.</span><span class="sxs-lookup"><span data-stu-id="53afb-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="53afb-633">![將迴圈加入至 WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "將迴圈加入至 WebTest1")</span><span class="sxs-lookup"><span data-stu-id="53afb-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="53afb-634">*將迴圈加入至 WebTest1*</span><span class="sxs-lookup"><span data-stu-id="53afb-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="53afb-635">在 **新增的條件式規則和項目迴圈**對話方塊中，選取**For 迴圈**規則，並修改下列屬性。</span><span class="sxs-lookup"><span data-stu-id="53afb-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

   1. <span data-ttu-id="53afb-636">**終止值：** 1000年</span><span class="sxs-lookup"><span data-stu-id="53afb-636">**Terminating value:** 1000</span></span>
   2. <span data-ttu-id="53afb-637">**內容參數名稱：** 迭代器</span><span class="sxs-lookup"><span data-stu-id="53afb-637">**Context Parameter Name:** Iterator</span></span>
   3. <span data-ttu-id="53afb-638">**遞增值：** 1</span><span class="sxs-lookup"><span data-stu-id="53afb-638">**Increment Value:** 1</span></span>

      <span data-ttu-id="53afb-639">![選取 For 迴圈規則並更新內容](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "選取 For 迴圈規則並更新內容")</span><span class="sxs-lookup"><span data-stu-id="53afb-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

      <span data-ttu-id="53afb-640">*選取 For 迴圈規則並更新內容*</span><span class="sxs-lookup"><span data-stu-id="53afb-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="53afb-641">底下**迴圈中的項目**區段中，選取您要在迴圈的第一個和最後一個項目先前建立的要求。</span><span class="sxs-lookup"><span data-stu-id="53afb-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="53afb-642">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="53afb-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="53afb-643">![選取此迴圈的第一個和最後一個項目](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "選取迴圈的第一個和最後一個項目")</span><span class="sxs-lookup"><span data-stu-id="53afb-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="53afb-644">*選取此迴圈的第一個和最後一個項目*</span><span class="sxs-lookup"><span data-stu-id="53afb-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="53afb-645">在 [**方案總管] 中**，以滑鼠右鍵按一下**WebAndLoadTestProject**專案中，展開**新增**功能表，然後選取**負載測試...**.</span><span class="sxs-lookup"><span data-stu-id="53afb-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="53afb-646">![WebAndLoadTestProject 專案中加入負載測試](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "WebAndLoadTestProject 專案中加入負載測試")</span><span class="sxs-lookup"><span data-stu-id="53afb-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="53afb-647">*WebAndLoadTestProject 專案中加入負載測試*</span><span class="sxs-lookup"><span data-stu-id="53afb-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="53afb-648">在 **新增負載測試精靈** 對話方塊中，按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="53afb-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="53afb-649">![新增負載測試精靈](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "新增負載測試精靈")</span><span class="sxs-lookup"><span data-stu-id="53afb-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="53afb-650">*新增負載測試精靈*</span><span class="sxs-lookup"><span data-stu-id="53afb-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="53afb-651">在 [**案例**頁面上，選取**不使用考慮時間**然後按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="53afb-652">![選取不想使用考慮時間](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "選取不想使用考慮時間")</span><span class="sxs-lookup"><span data-stu-id="53afb-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="53afb-653">*選擇不使用考慮時間*</span><span class="sxs-lookup"><span data-stu-id="53afb-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="53afb-654">在 **負載模式**頁面上，請確定**常數負載**選項。</span><span class="sxs-lookup"><span data-stu-id="53afb-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="53afb-655">變更**使用者計數**設為**250**使用者，然後按一下 [**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="53afb-656">![變更使用者計數設為 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "變更的使用者計數設為 250")</span><span class="sxs-lookup"><span data-stu-id="53afb-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="53afb-657">*變更為 250 的使用者計數*</span><span class="sxs-lookup"><span data-stu-id="53afb-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="53afb-658">在 [**測試混合模型**頁面上，選取**依據循序測試順序**然後按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="53afb-659">![選取的測試混合模型](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "選取的測試混合模型")</span><span class="sxs-lookup"><span data-stu-id="53afb-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="53afb-660">*選取的測試混合模型*</span><span class="sxs-lookup"><span data-stu-id="53afb-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="53afb-661">在 **測試混合模型**頁面上，按一下**加入...** 將測試混合。</span><span class="sxs-lookup"><span data-stu-id="53afb-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="53afb-662">![將測試加入至測試混合](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "將測試加入至測試混合")</span><span class="sxs-lookup"><span data-stu-id="53afb-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="53afb-663">*將測試加入至測試混合*</span><span class="sxs-lookup"><span data-stu-id="53afb-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="53afb-664">在 [**新增測試**] 對話方塊中，按兩下**WebTest1**若要加入至測試**選取的測試**清單。</span><span class="sxs-lookup"><span data-stu-id="53afb-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="53afb-665">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="53afb-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="53afb-666">![加入 WebTest1 測試](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "加入 WebTest1 測試")</span><span class="sxs-lookup"><span data-stu-id="53afb-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="53afb-667">*加入 WebTest1 測試*</span><span class="sxs-lookup"><span data-stu-id="53afb-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="53afb-668">回到**測試混合**頁面上，按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="53afb-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="53afb-669">![完成測試混合 頁面](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "完成測試混合 頁面")</span><span class="sxs-lookup"><span data-stu-id="53afb-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="53afb-670">*完成測試混合 頁面*</span><span class="sxs-lookup"><span data-stu-id="53afb-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="53afb-671">在 [**網路混合**頁面上，按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="53afb-672">![按一下 [網路混合] 頁面中的下一個](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "按一下網路混合 頁面中的下一個")</span><span class="sxs-lookup"><span data-stu-id="53afb-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="53afb-673">*網路混合 頁面中按一下 下一步*</span><span class="sxs-lookup"><span data-stu-id="53afb-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="53afb-674">在 [**瀏覽器混合**頁面上，選取**Internet Explorer 10.0**作為瀏覽器類型，然後按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="53afb-675">![選取 瀏覽器類型](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "選取瀏覽器類型")</span><span class="sxs-lookup"><span data-stu-id="53afb-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="53afb-676">*選取 瀏覽器類型*</span><span class="sxs-lookup"><span data-stu-id="53afb-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="53afb-677">在 [**計數器集合**頁面上，按一下**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="53afb-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="53afb-678">![在 計數器集合 頁面中按一下 下一步](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "按一下 下一步 在 計數器集合 頁面")</span><span class="sxs-lookup"><span data-stu-id="53afb-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="53afb-679">*在 [計數器集合] 頁面中按一下 [下一步]*</span><span class="sxs-lookup"><span data-stu-id="53afb-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="53afb-680">在 **回合設定**頁面上，將**負載測試持續期間**來**5 分鐘**，按一下 **完成**。</span><span class="sxs-lookup"><span data-stu-id="53afb-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="53afb-681">![設定負載測試持續期間為 5 分鐘](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "設定負載測試持續期間為 5 分鐘")</span><span class="sxs-lookup"><span data-stu-id="53afb-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="53afb-682">*設定負載測試持續期間為 5 分鐘*</span><span class="sxs-lookup"><span data-stu-id="53afb-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="53afb-683">在 [**方案總管] 中**，按兩下**Local.settings**来探索的測試設定檔案。</span><span class="sxs-lookup"><span data-stu-id="53afb-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="53afb-684">根據預設，Visual Studio 會使用您的本機電腦來執行測試。</span><span class="sxs-lookup"><span data-stu-id="53afb-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-685">或者，您可以設定測試專案，以在雲端中使用執行負載測試**測試計劃 Azure**。</span><span class="sxs-lookup"><span data-stu-id="53afb-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Azure Test Plans**.</span></span> <span data-ttu-id="53afb-686">Azure 的測試計劃提供雲端式負載測試會模擬更真實的負載的服務，避免本機環境的條件約束，例如 CPU 容量、 可用的記憶體和網路頻寬。</span><span class="sxs-lookup"><span data-stu-id="53afb-686">Azure Test Plans provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory, and network bandwidth.</span></span> <span data-ttu-id="53afb-687">如需有關如何使用 執行負載測試的 Azure 測試計劃的詳細資訊，請參閱[負載測試案例](/azure/devops/test/load-test/overview?view=vsts)。</span><span class="sxs-lookup"><span data-stu-id="53afb-687">For more information about using Azure Test Plans to run load tests, see [Load testing scenarios](/azure/devops/test/load-test/overview?view=vsts).</span></span>

    ![測試設定](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="53afb-689">工作 3-自動調整規模的驗證</span><span class="sxs-lookup"><span data-stu-id="53afb-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="53afb-690">現在，您會執行您在上一個工作中建立負載測試，看您的 web 應用程式在負載下的運作方式。</span><span class="sxs-lookup"><span data-stu-id="53afb-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="53afb-691">在 **方案總管**，按兩下**LoadTest1.loadtest**開啟負載測試。</span><span class="sxs-lookup"><span data-stu-id="53afb-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="53afb-692">![開啟 LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "開啟 LoadTest1.loadtest")</span><span class="sxs-lookup"><span data-stu-id="53afb-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="53afb-693">*開啟 LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="53afb-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="53afb-694">在 [ **LoadTest1.loadtest** ] 視窗中，按一下 [執行負載測試工具箱] 中的第一個按鈕。</span><span class="sxs-lookup"><span data-stu-id="53afb-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="53afb-695">![執行負載測試](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "執行負載測試")</span><span class="sxs-lookup"><span data-stu-id="53afb-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="53afb-696">*執行負載測試*</span><span class="sxs-lookup"><span data-stu-id="53afb-696">*Running the load test*</span></span>
3. <span data-ttu-id="53afb-697">等候完成的負載測試。</span><span class="sxs-lookup"><span data-stu-id="53afb-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-698">負載測試模擬多位使用者同時將要求傳送至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="53afb-699">當測試執行時，您可以監視可用的計數器，來偵測任何錯誤、 警告或其他與負載測試回合相關的資訊。</span><span class="sxs-lookup"><span data-stu-id="53afb-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="53afb-700">![負載測試執行](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "等待，直到負載測試完成")</span><span class="sxs-lookup"><span data-stu-id="53afb-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="53afb-701">*執行負載測試*</span><span class="sxs-lookup"><span data-stu-id="53afb-701">*Load test running*</span></span>
4. <span data-ttu-id="53afb-702">測試完成後，返回管理入口網站，並瀏覽至**擴展**web 應用程式的頁面。</span><span class="sxs-lookup"><span data-stu-id="53afb-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="53afb-703">底下**容量** 區段中，您應該會看到在圖形中的新執行個體已自動部署。</span><span class="sxs-lookup"><span data-stu-id="53afb-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![自動部署的新執行個體](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="53afb-705">*自動部署的新執行個體*</span><span class="sxs-lookup"><span data-stu-id="53afb-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="53afb-706">可能需要幾分鐘的時間才會出現在圖形中的變更 (請按**CTRL + F5**定期重新整理頁面)。</span><span class="sxs-lookup"><span data-stu-id="53afb-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="53afb-707">如果看不到任何變更，您可以嘗試下列各項：</span><span class="sxs-lookup"><span data-stu-id="53afb-707">If you do not see any changes, you can try the following:</span></span>
    >
    > - <span data-ttu-id="53afb-708">增加的負載測試持續時間 (例如，新增到**10 分鐘**)</span><span class="sxs-lookup"><span data-stu-id="53afb-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="53afb-709">降低的最大和最小值**目標 CPU** web 應用程式的自動調整規模設定中的範圍</span><span class="sxs-lookup"><span data-stu-id="53afb-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="53afb-710">使用在雲端中執行負載測試**測試計劃 Azure**。</span><span class="sxs-lookup"><span data-stu-id="53afb-710">Run the load test in the cloud with **Azure Test Plans**.</span></span> <span data-ttu-id="53afb-711">更多資訊[這裡](/azure/devops/test/load-test/index?view=vsts)</span><span class="sxs-lookup"><span data-stu-id="53afb-711">More information [here](/azure/devops/test/load-test/index?view=vsts)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="53afb-712">總結</span><span class="sxs-lookup"><span data-stu-id="53afb-712">Summary</span></span>

<span data-ttu-id="53afb-713">在這個實際操作實驗室中，您已了解如何設定及部署您的生產 web 應用程式在 Azure 中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="53afb-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="53afb-714">您開始透過偵測並更新您使用的資料庫**Entity Framework Code First Migrations**，然後繼續執行部署的站台使用的新版本**Git**以及執行回復您的站台的最新穩定版本。</span><span class="sxs-lookup"><span data-stu-id="53afb-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="53afb-715">此外，您已了解如何調整您的應用程式使用儲存體，將您的靜態內容移至 Blob 容器。</span><span class="sxs-lookup"><span data-stu-id="53afb-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
