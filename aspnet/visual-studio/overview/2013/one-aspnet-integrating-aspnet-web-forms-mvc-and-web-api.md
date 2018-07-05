---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 實習實驗室： 一個 ASP.NET： 整合 ASP.NET Web Form、 MVC 和 Web API |Microsoft Docs
author: rick-anderson
description: ASP.NET 是用於建置網站、 應用程式和服務使用特製化的技術，例如 MVC、 Web API 等的架構。 與延伸 ASP.NET h...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382941"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="e2881-104">實習實驗室： 一個 ASP.NET： 整合 ASP.NET Web Form、 MVC 和 Web API</span><span class="sxs-lookup"><span data-stu-id="e2881-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="e2881-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e2881-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e2881-106">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="e2881-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="e2881-107">ASP.NET 是用於建置網站、 應用程式和服務使用特製化的技術，例如 MVC、 Web API 等的架構。</span><span class="sxs-lookup"><span data-stu-id="e2881-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="e2881-108">與延伸 ASP.NET 已經看到自建立後，表示需要有整合這些技術中已經有最新的工作來運作**One ASP.NET**。</span><span class="sxs-lookup"><span data-stu-id="e2881-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="e2881-109">Visual Studio 2013 導入了新的統一的專案系統可讓您建置應用程式和一個專案中使用所有的 ASP.NET 技術。</span><span class="sxs-lookup"><span data-stu-id="e2881-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="e2881-110">這項功能就不需要在開始專案，並隨身碟，挑選一種技術，並改為鼓勵使用一個專案中的多個 ASP.NET 架構。</span><span class="sxs-lookup"><span data-stu-id="e2881-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="e2881-111">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="e2881-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="e2881-112">總覽</span><span class="sxs-lookup"><span data-stu-id="e2881-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e2881-113">目標</span><span class="sxs-lookup"><span data-stu-id="e2881-113">Objectives</span></span>

<span data-ttu-id="e2881-114">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="e2881-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="e2881-115">建立網站，根據**One ASP.NET**專案類型</span><span class="sxs-lookup"><span data-stu-id="e2881-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="e2881-116">使用不同**ASP.NET**架構喜歡**MVC**並**Web API**相同專案中</span><span class="sxs-lookup"><span data-stu-id="e2881-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="e2881-117">識別的主要元件**ASP.NET**應用程式</span><span class="sxs-lookup"><span data-stu-id="e2881-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="e2881-118">善用**ASP.NET Scaffolding**架構，來自動建立控制器和檢視來執行 CRUD 作業會根據您的模型類別</span><span class="sxs-lookup"><span data-stu-id="e2881-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="e2881-119">公開 （expose) 相同的機器和人類看得懂的格式為每個工作中使用正確的工具中的資訊集</span><span class="sxs-lookup"><span data-stu-id="e2881-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e2881-120">必要條件</span><span class="sxs-lookup"><span data-stu-id="e2881-120">Prerequisites</span></span>

<span data-ttu-id="e2881-121">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="e2881-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="e2881-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="e2881-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="e2881-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="e2881-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e2881-124">安裝程式</span><span class="sxs-lookup"><span data-stu-id="e2881-124">Setup</span></span>

<span data-ttu-id="e2881-125">若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="e2881-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="e2881-126">開啟 Windows Explorer 並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="e2881-127">以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="e2881-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="e2881-128">如果會顯示 [使用者帳戶控制] 對話方塊中，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="e2881-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="e2881-129">請確定您執行安裝程式之前檢查這個實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="e2881-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="e2881-130">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="e2881-130">Using the Code Snippets</span></span>

<span data-ttu-id="e2881-131">整個實驗室文件中，您將會指示要插入程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="e2881-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="e2881-132">為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="e2881-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="e2881-133">每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="e2881-134">請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。</span><span class="sxs-lookup"><span data-stu-id="e2881-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="e2881-135">在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="e2881-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="e2881-136">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。</span><span class="sxs-lookup"><span data-stu-id="e2881-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e2881-137">練習</span><span class="sxs-lookup"><span data-stu-id="e2881-137">Exercises</span></span>

<span data-ttu-id="e2881-138">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="e2881-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e2881-139">建立新的 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="e2881-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="e2881-140">建立使用 Scaffolding MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="e2881-141">建立使用 Scaffolding Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="e2881-142">估計的時間才能完成這個實驗室： **60 分鐘**</span><span class="sxs-lookup"><span data-stu-id="e2881-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="e2881-143">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="e2881-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="e2881-144">每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="e2881-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="e2881-145">在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="e2881-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="e2881-146">如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。</span><span class="sxs-lookup"><span data-stu-id="e2881-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="e2881-147">練習 1： 建立新的 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="e2881-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="e2881-148">在此練習中，您將建立新的 Web Form 網站，在 Visual Studio 2013 中使用**One ASP.NET**統一專案體驗，可讓您輕鬆地將相同的應用程式的 Web Form、 MVC 和 Web API 元件整合。</span><span class="sxs-lookup"><span data-stu-id="e2881-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="e2881-149">您接著會探索產生的方案，並找出其組件，和最後您會看到動作中的網站。</span><span class="sxs-lookup"><span data-stu-id="e2881-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="e2881-150">工作 1-建立新的站台使用一個 ASP.NET 使用經驗</span><span class="sxs-lookup"><span data-stu-id="e2881-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="e2881-151">在這項工作會啟動 Visual Studio 中建立新的網站為基礎**One ASP.NET**專案類型。</span><span class="sxs-lookup"><span data-stu-id="e2881-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="e2881-152">**One ASP.NET**統一所有 ASP.NET 技術，並可讓您混合及比對它們所需的選項。</span><span class="sxs-lookup"><span data-stu-id="e2881-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="e2881-153">然後辨識 live Web Form、 MVC 和 Web API 的不同元件並存應用程式內。</span><span class="sxs-lookup"><span data-stu-id="e2881-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="e2881-154">開啟**Visual Studio Express 2013 for Web** ，然後選取**檔案 |新增專案...** 啟動新的解決方案。</span><span class="sxs-lookup"><span data-stu-id="e2881-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![建立新專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="e2881-156">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="e2881-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="e2881-157">在 **新的專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web**索引標籤，並確定 **.NET Framework 4.5**已選取。</span><span class="sxs-lookup"><span data-stu-id="e2881-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="e2881-158">將專案命名為*MyHybridSite*，選擇**位置**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="e2881-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![新的 ASP.NET Web 應用程式專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="e2881-160">*建立新的 ASP.NET Web 應用程式專案*</span><span class="sxs-lookup"><span data-stu-id="e2881-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="e2881-161">在 **新增 ASP.NET 專案**對話方塊中，選取**Web Form**範本，然後選取**MVC**並**Web API**選項。</span><span class="sxs-lookup"><span data-stu-id="e2881-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="e2881-162">此外，請確定**驗證**選項設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="e2881-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="e2881-163">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="e2881-163">Click **OK** to continue.</span></span>

    ![使用 Web Form 範本，包括 Web API 和 MVC 元件建立新的專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="e2881-165">*使用 Web Form 範本，包括 Web API 和 MVC 元件建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="e2881-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="e2881-166">您現在可以探索產生的方案結構。</span><span class="sxs-lookup"><span data-stu-id="e2881-166">You can now explore the structure of the generated solution.</span></span>

    ![探索產生的方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="e2881-168">*探索產生的方案*</span><span class="sxs-lookup"><span data-stu-id="e2881-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="e2881-169">**帳戶：** 這個資料夾包含 Web Form 網頁，以註冊、 登入，以及管理應用程式的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="e2881-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="e2881-170">此資料夾時就會加入**個別使用者帳戶**Web Form 專案範本在設定期間選取驗證選項。</span><span class="sxs-lookup"><span data-stu-id="e2881-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="e2881-171">**模型：** 這個資料夾將包含代表您的應用程式資料的類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="e2881-172">**控制站**並**檢視**： 這些資料夾所需**ASP.NET MVC**並**ASP.NET Web API**元件。</span><span class="sxs-lookup"><span data-stu-id="e2881-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="e2881-173">您將探索的 MVC 和 Web API 的技術，在下一個練習中。</span><span class="sxs-lookup"><span data-stu-id="e2881-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="e2881-174">**Default.aspx**， **Contact.aspx**並**about.aspx 的網頁**檔案是預先定義的 Web 表單頁面，您可以使用做為起點來建立特定頁面您應用程式。</span><span class="sxs-lookup"><span data-stu-id="e2881-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="e2881-175">這些檔案的程式設計邏輯位於個別的檔案，稱為&quot;程式碼後置&quot;檔案，其有&quot;。 aspx.cs&quot;或&quot;。 aspx.cs&quot;延伸模組 (取決於使用語言）。</span><span class="sxs-lookup"><span data-stu-id="e2881-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="e2881-176">程式碼後置邏輯伺服器上執行，並以動態方式產生您頁面的 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="e2881-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="e2881-177">**Site.Master**並**Site.Mobile.Master**頁面定義應用程式中的外觀與風格及標準行為的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="e2881-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="e2881-178">按兩下**Default.aspx**瀏覽網頁的內容檔案。</span><span class="sxs-lookup"><span data-stu-id="e2881-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![瀏覽 Default.aspx 頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="e2881-180">*瀏覽 Default.aspx 頁面*</span><span class="sxs-lookup"><span data-stu-id="e2881-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-181">**網頁**在檔案頂端的指示詞定義的 Web Form 網頁的屬性。</span><span class="sxs-lookup"><span data-stu-id="e2881-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="e2881-182">例如， **MasterPageFile**屬性會指定為主要路徑頁面-在此情況下， *Site.Master*頁面和**Inherits**屬性定義[繼承] 頁面的程式碼後置類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="e2881-183">此類別位於檔案取決於**程式碼後置**屬性。</span><span class="sxs-lookup"><span data-stu-id="e2881-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="e2881-184">**Asp: Content**控制項包含頁面 （文字、 標記和控制項） 的實際內容，且對應至**asp: ContentPlaceHolder**主版頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="e2881-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="e2881-185">在此情況下，將呈現頁面內容內*MainContent*中所定義的控制項*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="e2881-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="e2881-186">依序展開**應用程式\_開始**資料夾，請注意**WebApiConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="e2881-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e2881-187">Visual Studio 中產生的方案包含該檔案，因為 One ASP.NET 範本與設定您的專案時，包含 Web API。</span><span class="sxs-lookup"><span data-stu-id="e2881-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="e2881-188">開啟**WebApiConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="e2881-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e2881-189">在  *WebApiConfig*類別，您會發現與 Web API，相關聯的對應 HTTP 組態將路由傳送至**Web API 控制器**。</span><span class="sxs-lookup"><span data-stu-id="e2881-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="e2881-190">開啟**RouteConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="e2881-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="e2881-191">內部*RegisterRoutes*方法，您會發現與 MVC，這會對應至 HTTP 路由相關聯的組態**MVC 控制器**。</span><span class="sxs-lookup"><span data-stu-id="e2881-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e2881-192">工作 2-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="e2881-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e2881-193">在這項工作中，您將會執行產生的方案，瀏覽應用程式和其部分功能，例如重新撰寫 URL 和內建的驗證。</span><span class="sxs-lookup"><span data-stu-id="e2881-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="e2881-194">若要執行此方案，請按**F5**或按**開始**按鈕位於工具列上。</span><span class="sxs-lookup"><span data-stu-id="e2881-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="e2881-195">應用程式首頁應該會在瀏覽器中開啟。</span><span class="sxs-lookup"><span data-stu-id="e2881-195">The application home page should open in the browser.</span></span>

    ![執行解決方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="e2881-197">確認 Web Form 網頁，會叫用。</span><span class="sxs-lookup"><span data-stu-id="e2881-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="e2881-198">若要這樣做，請附加 **/contact.aspx** url 網址列，然後按下**Enter**。</span><span class="sxs-lookup"><span data-stu-id="e2881-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![易記 Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="e2881-200">*易記 Url*</span><span class="sxs-lookup"><span data-stu-id="e2881-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-201">如您所見，URL 變更為 **/連絡**。</span><span class="sxs-lookup"><span data-stu-id="e2881-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="e2881-202">從開始**ASP.NET 4**、 URL 路由的功能已新增至 Web Form、 Url，您可以撰寫喜歡*[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* 而不是 *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="e2881-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="e2881-203">如需詳細資訊請參閱[URL 路由](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="e2881-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="e2881-204">現在，您將探索整合到應用程式的驗證流程。</span><span class="sxs-lookup"><span data-stu-id="e2881-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="e2881-205">若要這樣做，請按一下**註冊**頁面右上角。</span><span class="sxs-lookup"><span data-stu-id="e2881-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![註冊新的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="e2881-207">*註冊新的使用者*</span><span class="sxs-lookup"><span data-stu-id="e2881-207">*Registering a new user*</span></span>
4. <span data-ttu-id="e2881-208">在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。</span><span class="sxs-lookup"><span data-stu-id="e2881-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![註冊頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="e2881-210">*註冊頁面*</span><span class="sxs-lookup"><span data-stu-id="e2881-210">*Register page*</span></span>
5. <span data-ttu-id="e2881-211">應用程式註冊新的帳戶，並驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="e2881-211">The application registers the new account, and the user is authenticated.</span></span>

    ![已驗證的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="e2881-213">*已驗證的使用者*</span><span class="sxs-lookup"><span data-stu-id="e2881-213">*User authenticated*</span></span>
6. <span data-ttu-id="e2881-214">請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="e2881-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="e2881-215">練習 2： 建立使用 Scaffolding MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="e2881-216">在這個練習中您會利用 Visual Studio 建立 ASP.NET MVC 5 控制器與動作和 Razor 檢視，以執行 CRUD 作業，而不需要撰寫一行程式碼提供的 ASP.NET Scaffold 架構。</span><span class="sxs-lookup"><span data-stu-id="e2881-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="e2881-217">Scaffolding 程序將使用 Entity Framework Code First 來產生 SQL database 中的 資料內容和資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e2881-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="e2881-218">**有關 Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="e2881-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="e2881-219">Entity Framework (EF) 是物件關聯式對應程式 (ORM)，可讓您使用而不是直接使用關聯式儲存結構描述的程式設計概念應用程式模型進行程式設計來建立資料存取應用程式。</span><span class="sxs-lookup"><span data-stu-id="e2881-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="e2881-220">Entity Framework Code First 模型化工作流程可讓您使用您自己的網域類別代表模型執行查詢時，EF 相依於變更追蹤和更新函式。</span><span class="sxs-lookup"><span data-stu-id="e2881-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="e2881-221">使用 Code First 開發工作流程，您不需要藉由建立資料庫，或指定的結構描述開始您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e2881-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="e2881-222">相反地，您可以撰寫標準的.NET 類別可定義應用程式最適當的網域模型物件和 Entity Framework 會為您建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e2881-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="e2881-223">您可以深入了解 Entity Framework[此處](../../../entity-framework.md)。</span><span class="sxs-lookup"><span data-stu-id="e2881-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="e2881-224">工作 1-建立新模型</span><span class="sxs-lookup"><span data-stu-id="e2881-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="e2881-225">您現在將會定義**人員**類別，將會用來建立 MVC 控制器和檢視的 scaffolding 程序的模型。</span><span class="sxs-lookup"><span data-stu-id="e2881-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="e2881-226">您會先建立**人員**模型類別，在控制器中的 CRUD 作業將會自動建立和使用 scaffolding 功能。</span><span class="sxs-lookup"><span data-stu-id="e2881-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="e2881-227">開啟**Visual Studio Express 2013 for Web**並**MyHybridSite.sln**解決方案位於**來源/Ex2-MvcScaffolding/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="e2881-228">或者，您可以繼續使用解決方案您在前一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="e2881-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="e2881-229">在 [**方案總管] 中**，以滑鼠右鍵按一下**模型**資料夾**MyHybridSite**專案，然後選取**新增 |類別...**.</span><span class="sxs-lookup"><span data-stu-id="e2881-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![新增人員模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="e2881-231">*新增人員模型類別*</span><span class="sxs-lookup"><span data-stu-id="e2881-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="e2881-232">在 [**加入新項目**] 對話方塊中，將檔案命名*Person.cs*然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="e2881-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![建立 Person 模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="e2881-234">*建立 Person 模型類別*</span><span class="sxs-lookup"><span data-stu-id="e2881-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="e2881-235">取代的內容**Person.cs**為下列程式碼的檔案。</span><span class="sxs-lookup"><span data-stu-id="e2881-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="e2881-236">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="e2881-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="e2881-237">(程式碼片段- *BringingTogetherOneAspNet-Ex2-PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="e2881-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="e2881-238">在 [**方案總管] 中**，以滑鼠右鍵按一下**MyHybridSite**專案，然後選取**建置**，或按**CTRL + SHIFT + B**來建置專案。</span><span class="sxs-lookup"><span data-stu-id="e2881-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="e2881-239">工作 2-建立 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="e2881-240">既然**Person**建立模型，您將使用 Entity Framework 使用 ASP.NET MVC scaffolding，若要建立的 CRUD 控制器動作和檢視**人員**。</span><span class="sxs-lookup"><span data-stu-id="e2881-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="e2881-241">在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**MyHybridSite**專案，然後選取**新增 |新增 Scaffold 項目...**.</span><span class="sxs-lookup"><span data-stu-id="e2881-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="e2881-243">*建立新的 Scaffold 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="e2881-244">在 **新增 Scaffold**對話方塊中，選取**使用 MVC 5 控制器與檢視，Entity Framework** ，然後按一下 **新增。**</span><span class="sxs-lookup"><span data-stu-id="e2881-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![選取具有檢視和 Entity Framework 的 MVC 5 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="e2881-246">*選取具有檢視和 Entity Framework 的 MVC 5 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="e2881-247">設定*MvcPersonController*作為**控制站名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 作為**模型類別**。</span><span class="sxs-lookup"><span data-stu-id="e2881-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![新增 scaffolding MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="e2881-249">*新增 scaffolding MVC 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="e2881-250">底下**資料內容類別**，按一下 **新資料內容...**.</span><span class="sxs-lookup"><span data-stu-id="e2881-250">Under **Data context class**, click **New data context...**.</span></span>

    ![建立新的資料內容](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="e2881-252">*建立新的資料內容*</span><span class="sxs-lookup"><span data-stu-id="e2881-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="e2881-253">在 **新的資料內容** 對話方塊中，名稱的新資料內容*PersonContext* ，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="e2881-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![建立新的 PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="e2881-255">*建立新的 PersonContext 類型*</span><span class="sxs-lookup"><span data-stu-id="e2881-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="e2881-256">按一下 **新增**若要建立的新控制器**人員**使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="e2881-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="e2881-257">Visual Studio 接著會產生控制器動作、 個人資料內容和 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="e2881-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![在之後使用 scaffolding 建立 MVC 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="e2881-259">*在之後使用 scaffolding 建立 MVC 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="e2881-260">開啟**MvcPersonController.cs**中的檔案**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="e2881-261">請注意有自動產生的 CRUD 動作方法。</span><span class="sxs-lookup"><span data-stu-id="e2881-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="e2881-262">藉由選取**使用非同步控制器動作**從 scaffolding 的核取方塊選項，在先前步驟中，Visual Studio 產生的所有動作涉及之存取權的個人資料內容的非同步動作方法。</span><span class="sxs-lookup"><span data-stu-id="e2881-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="e2881-263">建議您用於長時間執行非同步動作方法，不受限於 CPU 以避免封鎖在處理要求時執行工作的 Web 伺服器的要求。</span><span class="sxs-lookup"><span data-stu-id="e2881-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="e2881-264">工作 3-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="e2881-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="e2881-265">在這個工作中，您將執行一次以確認解決方案的檢視**人員**會如預期般運作。</span><span class="sxs-lookup"><span data-stu-id="e2881-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="e2881-266">您將加入新的對方，以確認它已成功儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="e2881-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="e2881-267">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="e2881-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="e2881-268">瀏覽至 **/MvcPerson**。</span><span class="sxs-lookup"><span data-stu-id="e2881-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="e2881-269">包含 scaffold 的檢視，其顯示的人員清單應該會出現。</span><span class="sxs-lookup"><span data-stu-id="e2881-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="e2881-270">按一下 **新建**來新增新的人員。</span><span class="sxs-lookup"><span data-stu-id="e2881-270">Click **Create New** to add a new person.</span></span>

    ![瀏覽至包含 scaffold 的 MVC 檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="e2881-272">*瀏覽至包含 scaffold 的 MVC 檢視*</span><span class="sxs-lookup"><span data-stu-id="e2881-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="e2881-273">在**建立**檢視中，提供**名稱**並**年齡**人，然後按一下 **建立**。</span><span class="sxs-lookup"><span data-stu-id="e2881-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![加入新的人員](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="e2881-275">*加入新的人員*</span><span class="sxs-lookup"><span data-stu-id="e2881-275">*Adding a new person*</span></span>
5. <span data-ttu-id="e2881-276">新的人員新增至清單。</span><span class="sxs-lookup"><span data-stu-id="e2881-276">The new person is added to the list.</span></span> <span data-ttu-id="e2881-277">在 [項目] 清單中，按一下**詳細資料**顯示連絡人的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="e2881-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="e2881-278">然後，在**詳細資料**檢視中，按一下**回到清單**返回清單檢視。</span><span class="sxs-lookup"><span data-stu-id="e2881-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![人員詳細資料檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="e2881-280">*人員詳細資料檢視*</span><span class="sxs-lookup"><span data-stu-id="e2881-280">*Person's details view*</span></span>
6. <span data-ttu-id="e2881-281">按一下 **刪除**若要刪除之人員的連結。</span><span class="sxs-lookup"><span data-stu-id="e2881-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="e2881-282">在 **刪除**檢視中，按一下**刪除**以確認此作業。</span><span class="sxs-lookup"><span data-stu-id="e2881-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![刪除使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="e2881-284">*刪除使用者*</span><span class="sxs-lookup"><span data-stu-id="e2881-284">*Deleting a person*</span></span>
7. <span data-ttu-id="e2881-285">請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="e2881-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="e2881-286">練習 3： 建立使用 Scaffolding Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="e2881-287">Web API 架構是 ASP.NET 堆疊的一部分，並可輕鬆實作 HTTP 服務，通常傳送和接收 JSON 或 XML 格式的資料，透過 RESTful API。</span><span class="sxs-lookup"><span data-stu-id="e2881-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="e2881-288">在此練習中，您將使用 ASP.NET Scaffolding 一次產生 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="e2881-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="e2881-289">您將使用相同**Person**並**PersonContext**從先前的練習，以提供相同的個人資料，以 JSON 格式的類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="e2881-290">您會看到如何公開相同的資源，以及在相同的 ASP.NET 應用程式內不同的方式。</span><span class="sxs-lookup"><span data-stu-id="e2881-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="e2881-291">工作 1-建立 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="e2881-292">在這項工作會建立新**Web API 控制器**，將會公開 （expose) 電腦可使用格式 JSON 等個人資料。</span><span class="sxs-lookup"><span data-stu-id="e2881-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="e2881-293">如果尚未開啟，請開啟**Visual Studio Express 2013 for Web** ，然後開啟**MyHybridSite.sln**解決方案位於**來源/Ex3-WebAPI/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="e2881-294">或者，您可以繼續使用解決方案您在前一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="e2881-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-295">如果您開始從練習 3 開始方案時，請按**CTRL + SHIFT + B**建置方案。</span><span class="sxs-lookup"><span data-stu-id="e2881-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="e2881-296">在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**MyHybridSite**專案，然後選取**新增 |新增 Scaffold 項目...**.</span><span class="sxs-lookup"><span data-stu-id="e2881-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="e2881-298">*建立新的 scaffold 的控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="e2881-299">在**新增 Scaffold**對話方塊中，選取**Web API**左窗格中，然後**Web API 2 控制器與動作，使用 Entity Framework**在中間窗格中，然後按一下  **新增。**</span><span class="sxs-lookup"><span data-stu-id="e2881-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="e2881-300">![選取 Web API 2 控制器與動作和 Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Entity Framework 與動作選取 Web API 2 控制器")</span><span class="sxs-lookup"><span data-stu-id="e2881-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="e2881-301">*選取 Web API 2 控制器與動作和 Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="e2881-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="e2881-302">設定*ApiPersonController*作為**控制站名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 並**PersonContext (MyHybridSite.Models)** 作為**模型**並**資料內容**分別為類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="e2881-303">然後按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="e2881-303">Then click **Add**.</span></span>

    <span data-ttu-id="e2881-304">![新增 scaffolding Web API 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "新增 scaffolding Web API 控制器")</span><span class="sxs-lookup"><span data-stu-id="e2881-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e2881-305">*新增 scaffolding Web API 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="e2881-306">Visual Studio 接著會產生**ApiPersonController**執行四個的 CRUD 動作，以處理資料的類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="e2881-307">![使用 scaffolding 建立 Web API 控制器之後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "之後使用 scaffolding 建立 Web API 控制器")</span><span class="sxs-lookup"><span data-stu-id="e2881-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e2881-308">*在之後使用 scaffolding 建立 Web API 控制器*</span><span class="sxs-lookup"><span data-stu-id="e2881-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="e2881-309">開啟**ApiPersonController.cs**檔案，並檢查*GetPeople*動作方法。</span><span class="sxs-lookup"><span data-stu-id="e2881-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="e2881-310">這個方法會查詢的資料庫欄位**PersonContext**以取得使用者的資料類型。</span><span class="sxs-lookup"><span data-stu-id="e2881-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="e2881-311">現在注意到方法定義上方的註解。</span><span class="sxs-lookup"><span data-stu-id="e2881-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="e2881-312">它會提供您將在下一個工作使用此動作公開 （expose) 的 URI。</span><span class="sxs-lookup"><span data-stu-id="e2881-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="e2881-313">根據預設，Web API 已攔截到的查詢 */api*路徑，以避免與 MVC 控制器的衝突。</span><span class="sxs-lookup"><span data-stu-id="e2881-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="e2881-314">如果您需要變更此設定，請參閱[ASP.NET Web API 中的路由](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="e2881-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e2881-315">工作 2-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="e2881-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e2881-316">在這項工作中，您將使用 Internet Explorer **F12 開發人員工具**檢查 Web API 控制器的完整回應。</span><span class="sxs-lookup"><span data-stu-id="e2881-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="e2881-317">您會看到如何您也可以擷取網路流量，以取得您的應用程式資料的更多見解。</span><span class="sxs-lookup"><span data-stu-id="e2881-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="e2881-318">請確定**Internet Explorer**中選取**開始**位於 Visual Studio 工具列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="e2881-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer 選項](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="e2881-320">**F12 開發人員工具**有一組廣泛的並未涵蓋此實際操作實驗室的功能。</span><span class="sxs-lookup"><span data-stu-id="e2881-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="e2881-321">如果您想要深入了解，請參閱[使用 F12 開發人員工具](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))。</span><span class="sxs-lookup"><span data-stu-id="e2881-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="e2881-322">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="e2881-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-323">若要正確遵循這項工作中，您的應用程式需要有資料。</span><span class="sxs-lookup"><span data-stu-id="e2881-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="e2881-324">如果您的資料庫是空的您可以返回練習 2 中的工作 3，並遵循上如何建立使用 MVC 檢視的新使用者的步驟。</span><span class="sxs-lookup"><span data-stu-id="e2881-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="e2881-325">在瀏覽器中，按下**F12**來開啟**開發人員工具**面板。</span><span class="sxs-lookup"><span data-stu-id="e2881-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="e2881-326">按下**CTRL** + **4** ，或按一下**網路**圖示，然後按一下綠色箭號按鈕以開始擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="e2881-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="e2881-327">![起始 Web API 網路擷取](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "起始 Web API 網路擷取")</span><span class="sxs-lookup"><span data-stu-id="e2881-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="e2881-328">*起始的 Web API 網路擷取*</span><span class="sxs-lookup"><span data-stu-id="e2881-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="e2881-329">附加**api/ApiPerson**至瀏覽器的網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="e2881-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="e2881-330">您現在會檢查來自回應的詳細資料**ApiPersonController**。</span><span class="sxs-lookup"><span data-stu-id="e2881-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="e2881-331">![擷取使用者資料透過 Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "擷取透過 Web API 的個人資料")</span><span class="sxs-lookup"><span data-stu-id="e2881-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="e2881-332">*擷取透過 Web API 的個人資料*</span><span class="sxs-lookup"><span data-stu-id="e2881-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-333">下載完成後，系統會提示您使用下載的檔案進行的動作。</span><span class="sxs-lookup"><span data-stu-id="e2881-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="e2881-334">讓對話方塊保持開啟，才能夠觀賞回應內容，透過開發人員工具視窗。</span><span class="sxs-lookup"><span data-stu-id="e2881-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="e2881-335">現在，您將會檢查回應的主體。</span><span class="sxs-lookup"><span data-stu-id="e2881-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="e2881-336">若要這樣做，請按一下**詳細資料**索引標籤，然後按一下**回應主體**。</span><span class="sxs-lookup"><span data-stu-id="e2881-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="e2881-337">您可以檢查已下載的資料是否具有屬性的物件清單**識別碼**，**名稱**並**年齡**對應到**人員**類別。</span><span class="sxs-lookup"><span data-stu-id="e2881-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="e2881-338">![檢視 Web API 回應主體](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "檢視 Web API 回應主體")</span><span class="sxs-lookup"><span data-stu-id="e2881-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="e2881-339">*檢視 Web API 回應主體*</span><span class="sxs-lookup"><span data-stu-id="e2881-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="e2881-340">工作 3-新增 Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="e2881-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="e2881-341">當您建立 Web API 時，最好建立說明頁面，讓其他開發人員都知道如何呼叫您的 API。</span><span class="sxs-lookup"><span data-stu-id="e2881-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="e2881-342">您可以建立並手動更新文件頁面，但最好是自動產生以避免需要進行的維護工作。</span><span class="sxs-lookup"><span data-stu-id="e2881-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="e2881-343">在這項工作中，您將使用的 Nuget 套件來自動產生方案的 Web API 說明頁面。</span><span class="sxs-lookup"><span data-stu-id="e2881-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="e2881-344">從**工具**功能表，在 Visual Studio 中，選取**程式庫套件管理員**，然後按一下**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="e2881-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="e2881-345">在 [ **Package Manager Console** ] 視窗中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e2881-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="e2881-346">**Microsoft.AspNet.WebApi.HelpPage**套件會安裝必要的組件，並且新增 MVC 檢視底下的 [說明] 頁**區域/HelpPage**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="e2881-347">![HelpPage 區域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 區域")</span><span class="sxs-lookup"><span data-stu-id="e2881-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="e2881-348">*HelpPage 區域*</span><span class="sxs-lookup"><span data-stu-id="e2881-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="e2881-349">根據預設，說明 頁面都有文件的預留位置字串。</span><span class="sxs-lookup"><span data-stu-id="e2881-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="e2881-350">若要建立文件，您可以使用 XML 文件註解。</span><span class="sxs-lookup"><span data-stu-id="e2881-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="e2881-351">若要啟用這項功能，請開啟**HelpPageConfig.cs**檔案位於**應用程式的區域/HelpPage\_啟動**資料夾並取消註解下面這一行：</span><span class="sxs-lookup"><span data-stu-id="e2881-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="e2881-352">在 [**方案總管] 中**，以滑鼠右鍵按一下專案**MyHybridSite**，選取**屬性**，按一下 [**建置**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e2881-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="e2881-353">![建置索引標籤](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "建置區段")</span><span class="sxs-lookup"><span data-stu-id="e2881-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="e2881-354">*建置索引標籤*</span><span class="sxs-lookup"><span data-stu-id="e2881-354">*Build tab*</span></span>
5. <span data-ttu-id="e2881-355">底下**輸出**，選取**XML 文件檔案**。</span><span class="sxs-lookup"><span data-stu-id="e2881-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="e2881-356">在 [編輯] 方塊中，鍵入**應用程式\_Data/XmlDocument.xml**。</span><span class="sxs-lookup"><span data-stu-id="e2881-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="e2881-357">![輸出 [建置] 索引標籤中的一節](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "輸出區段中 [建置] 索引標籤")</span><span class="sxs-lookup"><span data-stu-id="e2881-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="e2881-358">*在 [建置] 索引標籤的 [輸出] 區段*</span><span class="sxs-lookup"><span data-stu-id="e2881-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="e2881-359">按下**CTRL** + **S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="e2881-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="e2881-360">開啟**ApiPersonController.cs**檔案**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2881-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="e2881-361">輸入新的一行之間*GetPeople*方法簽章並有 */ / 取得 api/ApiPerson*註解，然後輸入 三個正斜線。</span><span class="sxs-lookup"><span data-stu-id="e2881-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-362">Visual Studio 會自動插入的 XML 項目會定義方法的文件。</span><span class="sxs-lookup"><span data-stu-id="e2881-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="e2881-363">新增摘要的文字和傳回值*GetPeople*方法。</span><span class="sxs-lookup"><span data-stu-id="e2881-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="e2881-364">它看起來應該如下所示。</span><span class="sxs-lookup"><span data-stu-id="e2881-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="e2881-365">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="e2881-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="e2881-366">附加 **/help** [網址] 列中的 url，瀏覽至 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="e2881-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="e2881-367">![ASP.NET Web API 說明頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API 說明頁面")</span><span class="sxs-lookup"><span data-stu-id="e2881-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="e2881-368">*ASP.NET Web API 說明頁面*</span><span class="sxs-lookup"><span data-stu-id="e2881-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e2881-369">主頁面的內容是依控制站的 api 的資料表。</span><span class="sxs-lookup"><span data-stu-id="e2881-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="e2881-370">資料表項目使用，以動態方式產生**IApiExplorer**介面。</span><span class="sxs-lookup"><span data-stu-id="e2881-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="e2881-371">如果您新增或更新 API 控制器，資料表會自動更新您建置應用程式在下一次。</span><span class="sxs-lookup"><span data-stu-id="e2881-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="e2881-372">**API**欄會列出的 HTTP 方法和相對 URI。</span><span class="sxs-lookup"><span data-stu-id="e2881-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="e2881-373">**描述**資料行包含已從方法的文件中擷取的資訊。</span><span class="sxs-lookup"><span data-stu-id="e2881-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="e2881-374">請注意您新增上述的方法定義的描述會顯示在 [描述] 欄中。</span><span class="sxs-lookup"><span data-stu-id="e2881-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="e2881-375">![API 方法描述](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API 方法的描述")</span><span class="sxs-lookup"><span data-stu-id="e2881-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="e2881-376">*API 方法的描述*</span><span class="sxs-lookup"><span data-stu-id="e2881-376">*API method description*</span></span>
13. <span data-ttu-id="e2881-377">按一下其中一個 API 方法，以便瀏覽至含有更多詳細資訊，包括範例回應主體的頁面。</span><span class="sxs-lookup"><span data-stu-id="e2881-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="e2881-378">![詳細資料 頁](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細資訊 頁面")</span><span class="sxs-lookup"><span data-stu-id="e2881-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="e2881-379">*詳細的資訊 頁面*</span><span class="sxs-lookup"><span data-stu-id="e2881-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e2881-380">總結</span><span class="sxs-lookup"><span data-stu-id="e2881-380">Summary</span></span>

<span data-ttu-id="e2881-381">藉由完成這個實際操作實驗室，您已經學會如何：</span><span class="sxs-lookup"><span data-stu-id="e2881-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="e2881-382">建立新的 Web 應用程式，Visual Studio 2013 中使用一個 ASP.NET 使用經驗</span><span class="sxs-lookup"><span data-stu-id="e2881-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="e2881-383">將多個 ASP.NET 技術整合到一個單一專案</span><span class="sxs-lookup"><span data-stu-id="e2881-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="e2881-384">從您的模型類別使用的 ASP.NET Scaffold 產生 MVC 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="e2881-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="e2881-385">產生使用功能，例如非同步程式設計，並透過 Entity Framework 資料存取的 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="e2881-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="e2881-386">自動產生 Web API 說明頁面，您的控制站</span><span class="sxs-lookup"><span data-stu-id="e2881-386">Automatically generate Web API Help Pages for your controllers</span></span>
