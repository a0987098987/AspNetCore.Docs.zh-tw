---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 實習： 一個 ASP.NET： 將 ASP.NET Web Form、 MVC 和 Web API 的整合 |Microsoft 文件
author: rick-anderson
description: ASP.NET 是用於建置網站、 應用程式和服務使用特定的技術，例如 MVC、 Web API 等的架構。 與 ASP.NET h 延伸...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="1b96f-104">實習： 一個 ASP.NET： 整合 ASP.NET Web Form、 MVC 和 Web API</span><span class="sxs-lookup"><span data-stu-id="1b96f-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="1b96f-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="1b96f-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="1b96f-106">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="1b96f-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="1b96f-107">ASP.NET 是用於建置網站、 應用程式和服務使用特定的技術，例如 MVC、 Web API 等的架構。</span><span class="sxs-lookup"><span data-stu-id="1b96f-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="1b96f-108">展開的 ASP.NET 已經看到它建立之後，表示需要有整合這些技術中已有新的工作優先的工作**One ASP.NET**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="1b96f-109">Visual Studio 2013 導入了新的統一的專案系統可讓您建立應用程式，以及一個專案中使用所有 ASP.NET 技術。</span><span class="sxs-lookup"><span data-stu-id="1b96f-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="1b96f-110">這項功能不需要挑選的專案和搖桿，開頭的一種技術，並改為建議您使用一個專案中的多個 ASP.NET 架構。</span><span class="sxs-lookup"><span data-stu-id="1b96f-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="1b96f-111">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="1b96f-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="1b96f-112">概觀</span><span class="sxs-lookup"><span data-stu-id="1b96f-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="1b96f-113">目標</span><span class="sxs-lookup"><span data-stu-id="1b96f-113">Objectives</span></span>

<span data-ttu-id="1b96f-114">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="1b96f-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="1b96f-115">建立網站，根據**One ASP.NET**專案類型</span><span class="sxs-lookup"><span data-stu-id="1b96f-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="1b96f-116">使用不同**ASP.NET**架構喜歡**MVC**和**Web API**相同專案中</span><span class="sxs-lookup"><span data-stu-id="1b96f-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="1b96f-117">識別的主要元件**ASP.NET**應用程式</span><span class="sxs-lookup"><span data-stu-id="1b96f-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="1b96f-118">利用**ASP.NET Scaffolding** framework 來自動建立控制器和檢視執行 CRUD 作業會根據模型類別</span><span class="sxs-lookup"><span data-stu-id="1b96f-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="1b96f-119">公開 （expose) 相同的電腦和人類看得懂的格式使用適當的工具，為每個工作中的資訊集</span><span class="sxs-lookup"><span data-stu-id="1b96f-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="1b96f-120">必要條件</span><span class="sxs-lookup"><span data-stu-id="1b96f-120">Prerequisites</span></span>

<span data-ttu-id="1b96f-121">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="1b96f-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="1b96f-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="1b96f-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="1b96f-123">Visual Studio 2013 Update 1</span><span class="sxs-lookup"><span data-stu-id="1b96f-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="1b96f-124">安裝程式</span><span class="sxs-lookup"><span data-stu-id="1b96f-124">Setup</span></span>

<span data-ttu-id="1b96f-125">若要執行這個實際操作實驗室練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="1b96f-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="1b96f-126">開啟 Windows 檔案總管並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="1b96f-127">以滑鼠右鍵按一下**Setup.cmd**選取**系統管理員身分執行**啟動安裝程序，將會設定您的環境，並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="1b96f-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="1b96f-128">如果顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="1b96f-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="1b96f-129">請確定執行安裝程式之前已在本實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="1b96f-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="1b96f-130">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="1b96f-130">Using the Code Snippets</span></span>

<span data-ttu-id="1b96f-131">在實驗室文件，您會指示要插入的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="1b96f-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="1b96f-132">為了方便起見，大部分的這段程式碼是依現狀 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，不必以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="1b96f-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="1b96f-133">每個練習會伴隨起始方案位於**開始**練習，可讓您依照每個練習各自的資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="1b96f-134">請注意練習期間加入的程式碼片段遺失從中啟動方案，並可能無法運作，直到您已經完成本練習。</span><span class="sxs-lookup"><span data-stu-id="1b96f-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="1b96f-135">內部練習的原始程式碼，您也可以找到**結束**資料夾包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="1b96f-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="1b96f-136">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。</span><span class="sxs-lookup"><span data-stu-id="1b96f-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="1b96f-137">練習</span><span class="sxs-lookup"><span data-stu-id="1b96f-137">Exercises</span></span>

<span data-ttu-id="1b96f-138">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="1b96f-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="1b96f-139">建立新的 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="1b96f-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="1b96f-140">建立使用 Scaffolding MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="1b96f-141">建立此 Web API 控制器會使用 Scaffolding</span><span class="sxs-lookup"><span data-stu-id="1b96f-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="1b96f-142">完成本實驗室估計時間： **60 分鐘的時間**</span><span class="sxs-lookup"><span data-stu-id="1b96f-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="1b96f-143">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="1b96f-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="1b96f-144">每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="1b96f-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="1b96f-145">在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="1b96f-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="1b96f-146">如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。</span><span class="sxs-lookup"><span data-stu-id="1b96f-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="1b96f-147">練習 1： 建立新的 Web Form 專案</span><span class="sxs-lookup"><span data-stu-id="1b96f-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="1b96f-148">在此練習中，您將建立新的 Web Form 網站，在 Visual Studio 2013 中使用**One ASP.NET**統一專案體驗，可讓您輕鬆地將相同的應用程式中的 Web Form、 MVC 和 Web API 元件的整合。</span><span class="sxs-lookup"><span data-stu-id="1b96f-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="1b96f-149">接著瀏覽產生的方案，並識別其組件，最後您會看到動作中的網站。</span><span class="sxs-lookup"><span data-stu-id="1b96f-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="1b96f-150">工作 1-建立新的站台使用一個 ASP.NET 體驗</span><span class="sxs-lookup"><span data-stu-id="1b96f-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="1b96f-151">在這項工作會啟動 Visual Studio 中建立新的網站根據**One ASP.NET**專案類型。</span><span class="sxs-lookup"><span data-stu-id="1b96f-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="1b96f-152">**一個 ASP.NET**統一所有 ASP.NET 技術，並提供您混搭它們所需的選項。</span><span class="sxs-lookup"><span data-stu-id="1b96f-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="1b96f-153">您再將應用程式中並排顯示辨識即時 Web Form、 MVC 和 Web API 的不同元件。</span><span class="sxs-lookup"><span data-stu-id="1b96f-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="1b96f-154">開啟**Visual Studio Express 2013 for Web**選取**檔案 |新增專案...** 啟動新的解決方案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![建立新專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="1b96f-156">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="1b96f-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="1b96f-157">在**新專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web**索引標籤，並確定 **.NET Framework 4.5**已選取。</span><span class="sxs-lookup"><span data-stu-id="1b96f-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="1b96f-158">將專案命名*MyHybridSite*，選擇**位置**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![新的 ASP.NET Web 應用程式專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="1b96f-160">*建立新的 ASP.NET Web 應用程式專案*</span><span class="sxs-lookup"><span data-stu-id="1b96f-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="1b96f-161">在**新增 ASP.NET 專案**對話方塊中，選取**Web Form**範本，然後選取**MVC**和**Web API**選項。</span><span class="sxs-lookup"><span data-stu-id="1b96f-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="1b96f-162">另外，請確定**驗證**選項設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="1b96f-163">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="1b96f-163">Click **OK** to continue.</span></span>

    ![Web Form 範本，包括 Web API 和 MVC 元件建立新的專案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="1b96f-165">*Web Form 範本，包括 Web API 和 MVC 元件建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="1b96f-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="1b96f-166">您現在可以瀏覽產生方案的結構。</span><span class="sxs-lookup"><span data-stu-id="1b96f-166">You can now explore the structure of the generated solution.</span></span>

    ![探索產生的方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="1b96f-168">*探索產生的方案*</span><span class="sxs-lookup"><span data-stu-id="1b96f-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="1b96f-169">**帳戶：** 此資料夾包含 Web Form 網頁註冊、 登入和管理應用程式的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="1b96f-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="1b96f-170">此資料夾時就會加入**個別使用者帳戶**Web Form 專案範本的組態期間，選取驗證選項。</span><span class="sxs-lookup"><span data-stu-id="1b96f-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="1b96f-171">**模型：** 這個資料夾將包含代表您的應用程式資料的類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="1b96f-172">**控制站**和**檢視**： 這些資料夾所需的**ASP.NET MVC**和**ASP.NET Web API**元件。</span><span class="sxs-lookup"><span data-stu-id="1b96f-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="1b96f-173">您將探索的 MVC 和 Web API 的技術，在下一個練習。</span><span class="sxs-lookup"><span data-stu-id="1b96f-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="1b96f-174">**Default.aspx**， **Contact.aspx**和**about.aspx 的網頁**檔案是預先定義的 Web 表單頁面可讓您做為起點來建立特定頁面您應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b96f-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="1b96f-175">程式設計邏輯，這些檔案位於不同的檔案稱為&quot;程式碼後置&quot;檔案，其有&quot;。 為&quot;或&quot;.aspx.vb&quot;延伸模組 (取決於使用語言）。</span><span class="sxs-lookup"><span data-stu-id="1b96f-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="1b96f-176">程式碼後置邏輯伺服器上執行，並以動態方式產生網頁的 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="1b96f-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="1b96f-177">**Site.Master**和**Site.Mobile.Master**頁面定義應用程式中的外觀與風格和所有頁面的標準行為。</span><span class="sxs-lookup"><span data-stu-id="1b96f-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="1b96f-178">按兩下**Default.aspx**瀏覽網頁的內容檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![瀏覽 Default.aspx 網頁](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="1b96f-180">*瀏覽 Default.aspx 網頁*</span><span class="sxs-lookup"><span data-stu-id="1b96f-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-181">**頁面**指示詞檔案的頂端定義 Web Form 網頁的屬性。</span><span class="sxs-lookup"><span data-stu-id="1b96f-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="1b96f-182">例如， **MasterPageFile**屬性會指定為主要路徑頁面-在此情況下， *Site.Master*頁面和**Inherits**屬性定義程式碼後置頁面，即可繼承的類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="1b96f-183">這個類別位於取決於檔案**程式碼後置**屬性。</span><span class="sxs-lookup"><span data-stu-id="1b96f-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="1b96f-184">**Asp: Content**控制保存的頁面 （文字、 標記和控制項） 的實際內容，且已對應至**asp: ContentPlaceHolder**主版頁面上的控制項。</span><span class="sxs-lookup"><span data-stu-id="1b96f-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="1b96f-185">在此情況下，將呈現之網頁內容內*MainContent*中定義控制項*Site.Master*頁面。</span><span class="sxs-lookup"><span data-stu-id="1b96f-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="1b96f-186">展開**應用程式\_啟動**資料夾，並注意**WebApiConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="1b96f-187">Visual Studio 中產生的方案包含該檔案，因為一個 ASP.NET 範本與設定您的專案時，包含 Web API。</span><span class="sxs-lookup"><span data-stu-id="1b96f-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="1b96f-188">開啟**WebApiConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="1b96f-189">在*到 WebApiConfig*類別，您會發現與 Web 應用程式開發介面，相關聯的對應 HTTP 組態將路由傳送至**Web API 控制器**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="1b96f-190">開啟**RouteConfig.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="1b96f-191">內部*RegisterRoutes*方法，您會發現與 MVC，對應到 HTTP 路由相關聯的組態**MVC 控制器**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="1b96f-192">工作 2-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="1b96f-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="1b96f-193">在這項工作中，您將執行產生的方案，瀏覽應用程式和其功能，例如 URL 重寫和內建的驗證部分。</span><span class="sxs-lookup"><span data-stu-id="1b96f-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="1b96f-194">若要執行此方案，請按**F5**或按一下**啟動**按鈕位於工具列上。</span><span class="sxs-lookup"><span data-stu-id="1b96f-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="1b96f-195">瀏覽器中開啟應用程式首頁。</span><span class="sxs-lookup"><span data-stu-id="1b96f-195">The application home page should open in the browser.</span></span>

    ![執行解決方案](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="1b96f-197">請確認 Web Form 頁面會被叫用。</span><span class="sxs-lookup"><span data-stu-id="1b96f-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="1b96f-198">若要這樣做，請附加 **/contact.aspx**在位址列中按的 url **Enter**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![易記的 Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="1b96f-200">*易記的 Url*</span><span class="sxs-lookup"><span data-stu-id="1b96f-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-201">如您所見，URL 變更為 **/連絡**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="1b96f-202">從開始**ASP.NET 4**、 URL 路由的功能已加入至 Web Form、 Url，您可以撰寫類似*[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* 而不是*[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*。</span><span class="sxs-lookup"><span data-stu-id="1b96f-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="1b96f-203">如需詳細資訊請參閱[URL 路由](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)。</span><span class="sxs-lookup"><span data-stu-id="1b96f-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="1b96f-204">現在，您將探索整合到應用程式的驗證流程。</span><span class="sxs-lookup"><span data-stu-id="1b96f-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="1b96f-205">若要這樣做，請按一下**註冊**頁面的右上角。</span><span class="sxs-lookup"><span data-stu-id="1b96f-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![註冊新的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="1b96f-207">*註冊新的使用者*</span><span class="sxs-lookup"><span data-stu-id="1b96f-207">*Registering a new user*</span></span>
4. <span data-ttu-id="1b96f-208">在**註冊**頁面上，輸入**使用者名**和**密碼**，然後按一下 **註冊**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![註冊頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="1b96f-210">*註冊頁面*</span><span class="sxs-lookup"><span data-stu-id="1b96f-210">*Register page*</span></span>
5. <span data-ttu-id="1b96f-211">應用程式註冊新帳戶，並在驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1b96f-211">The application registers the new account, and the user is authenticated.</span></span>

    ![已驗證的使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="1b96f-213">*已驗證的使用者*</span><span class="sxs-lookup"><span data-stu-id="1b96f-213">*User authenticated*</span></span>
6. <span data-ttu-id="1b96f-214">返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="1b96f-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="1b96f-215">練習 2： 建立使用 Scaffolding MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="1b96f-216">在這個練習中您會利用 ASP.NET Scaffolding 架構來建立 ASP.NET MVC 5 控制器與動作及 Razor 檢視，以執行 CRUD 作業，而不需要撰寫一行程式碼的 Visual Studio 所提供的。</span><span class="sxs-lookup"><span data-stu-id="1b96f-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="1b96f-217">Scaffolding 程序將使用 Entity Framework Code First 來產生 SQL 資料庫中的資料內容和資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="1b96f-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="1b96f-218">**有關 Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="1b96f-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="1b96f-219">Entity Framework (EF) 是可讓您建立資料存取應用程式的程式設計概念應用程式模型，而不是直接使用關聯式儲存結構描述的程式設計物件關聯式對應 (ORM)。</span><span class="sxs-lookup"><span data-stu-id="1b96f-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="1b96f-220">Entity Framework Code First 模型工作流程可讓您使用您自己的網域類別來表示 EF 時執行查詢時，依賴的模型變更追蹤和更新函式。</span><span class="sxs-lookup"><span data-stu-id="1b96f-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="1b96f-221">使用 Code First 開發工作流程，您不需要來開始您的應用程式所建立資料庫，或指定的結構描述。</span><span class="sxs-lookup"><span data-stu-id="1b96f-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="1b96f-222">相反地，您可以撰寫標準的.NET 類別可定義您的應用程式，最適當的網域模型物件和 Entity Framework 會為您建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="1b96f-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="1b96f-223">您可以進一步了解 Entity Framework[這裡](../../../entity-framework.md)。</span><span class="sxs-lookup"><span data-stu-id="1b96f-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="1b96f-224">工作 1-建立新模型</span><span class="sxs-lookup"><span data-stu-id="1b96f-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="1b96f-225">您將定義**人員**類別，將會使用 scaffolding 程序來建立 MVC 控制器和檢視的模型。</span><span class="sxs-lookup"><span data-stu-id="1b96f-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="1b96f-226">一開始會建立**人員**模型類別，控制器中的 CRUD 作業將會自動建立和使用 scaffolding 功能。</span><span class="sxs-lookup"><span data-stu-id="1b96f-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="1b96f-227">開啟**Visual Studio Express 2013 for Web**和**MyHybridSite.sln**方案位於**來源/Ex2 MvcScaffolding/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="1b96f-228">或者，您可以繼續使用解決方案您在上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="1b96f-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="1b96f-229">在**方案總管 中**，以滑鼠右鍵按一下**模型**資料夾**MyHybridSite**專案，然後選取**新增 |類別...**.</span><span class="sxs-lookup"><span data-stu-id="1b96f-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![新增人員模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="1b96f-231">*新增人員模型類別*</span><span class="sxs-lookup"><span data-stu-id="1b96f-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="1b96f-232">在**加入新項目**對話方塊中，將檔案命名*Person.cs*按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![建立個人模型類別](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="1b96f-234">*建立個人模型類別*</span><span class="sxs-lookup"><span data-stu-id="1b96f-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="1b96f-235">取代的內容**Person.cs**以下列程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="1b96f-236">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="1b96f-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="1b96f-237">(程式碼片段- *BringingTogetherOneAspNet-Ex2-PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="1b96f-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="1b96f-238">在**方案總管 中**，以滑鼠右鍵按一下**MyHybridSite**專案，然後選取**建置**，或按**CTRL + SHIFT + B**建置專案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="1b96f-239">工作 2 – 建立 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="1b96f-240">既然**人員**建立模型時，會建立執行 CRUD 動作，控制器和檢視 Entity Framework 搭配使用 ASP.NET MVC scaffolding**人員**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="1b96f-241">在**方案總管 中**，以滑鼠右鍵按一下**控制器**資料夾**MyHybridSite**專案，然後選取**新增 |新的 Scaffold 項目...**.</span><span class="sxs-lookup"><span data-stu-id="1b96f-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="1b96f-243">*建立新的 Scaffold 控制器*</span><span class="sxs-lookup"><span data-stu-id="1b96f-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="1b96f-244">在**新增 Scaffold**對話方塊中，選取**的 MVC 5 控制器與檢視，使用 Entity Framework** ，然後按一下 **新增。**</span><span class="sxs-lookup"><span data-stu-id="1b96f-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![選取的檢視和 Entity Framework 的 MVC 5 控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="1b96f-246">*選取的檢視和 Entity Framework 的 MVC 5 控制器*</span><span class="sxs-lookup"><span data-stu-id="1b96f-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="1b96f-247">設定*MvcPersonController*為**控制器名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 為**模型類別**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![新增此 MVC 控制器具有 scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="1b96f-249">*新增此 MVC 控制器具有 scaffolding*</span><span class="sxs-lookup"><span data-stu-id="1b96f-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="1b96f-250">在下**資料內容類別**，按一下 **新的資料內容...**.</span><span class="sxs-lookup"><span data-stu-id="1b96f-250">Under **Data context class**, click **New data context...**.</span></span>

    ![建立新的資料內容](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="1b96f-252">*建立新的資料內容*</span><span class="sxs-lookup"><span data-stu-id="1b96f-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="1b96f-253">在**新的資料內容**對話方塊中，命名新的資料內容*PersonContext*按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![建立新的 PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="1b96f-255">*建立新的 PersonContext 類型*</span><span class="sxs-lookup"><span data-stu-id="1b96f-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="1b96f-256">按一下**新增**來建立新的控制站的**人員**使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="1b96f-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="1b96f-257">Visual Studio 會隨後產生控制器動作、 個人資料內容和 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="1b96f-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![建立 MVC 控制器的 scaffolding 之後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="1b96f-259">*建立 MVC 控制器的 scaffolding 之後*</span><span class="sxs-lookup"><span data-stu-id="1b96f-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="1b96f-260">開啟**MvcPersonController.cs**檔案**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="1b96f-261">請注意 CRUD 動作方法已自動產生。</span><span class="sxs-lookup"><span data-stu-id="1b96f-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="1b96f-262">藉由選取**使用非同步控制器動作**scaffolding 核取方塊選項，在先前步驟中，Visual Studio 會產生涉及到個人資料內容的存取權的所有動作的非同步動作方法。</span><span class="sxs-lookup"><span data-stu-id="1b96f-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="1b96f-263">建議的非同步動作方法用於長時間執行，不受限於 CPU 以避免封鎖，網頁伺服器無法處理要求時執行工作的要求。</span><span class="sxs-lookup"><span data-stu-id="1b96f-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="1b96f-264">工作 3-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="1b96f-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="1b96f-265">在這個工作中，您將執行一次以確認方案的檢視**人員**是否依照預期方式運作。</span><span class="sxs-lookup"><span data-stu-id="1b96f-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="1b96f-266">您將加入新的對方，以確認它已成功儲存資料庫。</span><span class="sxs-lookup"><span data-stu-id="1b96f-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="1b96f-267">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="1b96f-268">瀏覽至 **/MvcPerson**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="1b96f-269">顯示的人員清單的 scaffold 的檢視應該會出現。</span><span class="sxs-lookup"><span data-stu-id="1b96f-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="1b96f-270">按一下**新建**来加入新的人員。</span><span class="sxs-lookup"><span data-stu-id="1b96f-270">Click **Create New** to add a new person.</span></span>

    ![巡覽至 scaffold MVC 檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="1b96f-272">*巡覽至 scaffold MVC 檢視*</span><span class="sxs-lookup"><span data-stu-id="1b96f-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="1b96f-273">在**建立**檢視中，提供**名稱**和**年齡**人員，然後按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![加入新的人員](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="1b96f-275">*加入新的人員*</span><span class="sxs-lookup"><span data-stu-id="1b96f-275">*Adding a new person*</span></span>
5. <span data-ttu-id="1b96f-276">新的人員加入至清單。</span><span class="sxs-lookup"><span data-stu-id="1b96f-276">The new person is added to the list.</span></span> <span data-ttu-id="1b96f-277">在項目清單中，按一下 **詳細資料**顯示人員的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="1b96f-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="1b96f-278">然後，在**詳細資料**檢視中，按一下**返回清單**返回清單檢視。</span><span class="sxs-lookup"><span data-stu-id="1b96f-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![人員的詳細資料檢視](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="1b96f-280">*人員的詳細資料檢視*</span><span class="sxs-lookup"><span data-stu-id="1b96f-280">*Person's details view*</span></span>
6. <span data-ttu-id="1b96f-281">按一下**刪除**若要刪除之人員的連結。</span><span class="sxs-lookup"><span data-stu-id="1b96f-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="1b96f-282">在**刪除**檢視中，按一下**刪除**確認該項作業。</span><span class="sxs-lookup"><span data-stu-id="1b96f-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![刪除使用者](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="1b96f-284">*刪除使用者*</span><span class="sxs-lookup"><span data-stu-id="1b96f-284">*Deleting a person*</span></span>
7. <span data-ttu-id="1b96f-285">返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="1b96f-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="1b96f-286">練習 3： 建立使用 Scaffolding Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="1b96f-287">Web API framework 是 ASP.NET 堆疊的一部分，旨在讓您更輕鬆，通常傳送和接收 JSON 或 XML 格式的資料，透過 rest 式 API 實作 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="1b96f-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="1b96f-288">在此練習中，您將使用 ASP.NET Scaffolding 再次產生的 Web API 控制器。</span><span class="sxs-lookup"><span data-stu-id="1b96f-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="1b96f-289">您將使用相同**人員**和**PersonContext**中的上一個練習中提供相同的個人資料，以 JSON 格式的類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="1b96f-290">您會看到如何公開相同的資源，以及在相同的 ASP.NET 應用程式中不同的方式。</span><span class="sxs-lookup"><span data-stu-id="1b96f-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="1b96f-291">工作 1 – 建立 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="1b96f-292">在這項工作會建立新**Web API 控制器**，將會公開 （expose) 電腦可使用格式類似 JSON 中的個人資料。</span><span class="sxs-lookup"><span data-stu-id="1b96f-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="1b96f-293">如果尚未開啟，請開啟**Visual Studio Express 2013 for Web**並開啟**MyHybridSite.sln**方案位於**來源/Ex3 WebAPI/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="1b96f-294">或者，您可以繼續使用解決方案您在上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="1b96f-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-295">如果您開始使用 Begin 解決方案從練習 3 時，請按**CTRL + SHIFT + B**以建置方案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="1b96f-296">在**方案總管 中**，以滑鼠右鍵按一下**控制器**資料夾**MyHybridSite**專案，然後選取**新增 |新的 Scaffold 項目...**.</span><span class="sxs-lookup"><span data-stu-id="1b96f-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![建立新的 scaffold 的控制器](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="1b96f-298">*建立新的 scaffold 的控制器*</span><span class="sxs-lookup"><span data-stu-id="1b96f-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="1b96f-299">在**新增 Scaffold**對話方塊中，選取**Web API**的左窗格中，然後**Web API 2 控制器動作，使用 Entity Framework**在中間窗格中，然後按一下  **加入。**</span><span class="sxs-lookup"><span data-stu-id="1b96f-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="1b96f-300">![選取 Web API 2 控制器與動作和 Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "選取的 Web API 2 控制器與動作和 Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="1b96f-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="1b96f-301">*選取 Web API 2 控制器與動作和 Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="1b96f-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="1b96f-302">設定*ApiPersonController*為**控制器名稱**，選取**使用非同步控制器動作**選項，然後選取**人員 (MyHybridSite.Models)** 和**PersonContext (MyHybridSite.Models)** 為**模型**和**資料內容**分別類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="1b96f-303">然後按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="1b96f-303">Then click **Add**.</span></span>

    <span data-ttu-id="1b96f-304">![加入 Web API 控制器會 scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "加入 scaffolding Web API 控制器")</span><span class="sxs-lookup"><span data-stu-id="1b96f-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="1b96f-305">*加入 Web API 控制器的 scaffolding*</span><span class="sxs-lookup"><span data-stu-id="1b96f-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="1b96f-306">Visual Studio 會隨後產生**ApiPersonController**四個的 CRUD 動作，以處理資料的類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="1b96f-307">![建立 Web API 控制器的 scaffolding 之後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "之後使用 scaffolding 建立 Web API 控制器")</span><span class="sxs-lookup"><span data-stu-id="1b96f-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="1b96f-308">*建立 Web API 控制器的 scaffolding 之後*</span><span class="sxs-lookup"><span data-stu-id="1b96f-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="1b96f-309">開啟**ApiPersonController.cs**檔案，並檢查*GetPeople*動作方法。</span><span class="sxs-lookup"><span data-stu-id="1b96f-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="1b96f-310">這個方法會查詢的資料庫欄位**PersonContext**以取得使用者的資料類型。</span><span class="sxs-lookup"><span data-stu-id="1b96f-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="1b96f-311">現在請注意上述的方法定義的註解。</span><span class="sxs-lookup"><span data-stu-id="1b96f-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="1b96f-312">它會提供公開此動作，您將在下一項工作中使用的 URI。</span><span class="sxs-lookup"><span data-stu-id="1b96f-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="1b96f-313">根據預設，Web API 設定要攔截查詢 */api*路徑，以避免與 MVC 控制器的衝突。</span><span class="sxs-lookup"><span data-stu-id="1b96f-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="1b96f-314">如果您需要變更此設定，請參閱[中 ASP.NET Web API 的路由](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="1b96f-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="1b96f-315">工作 2-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="1b96f-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="1b96f-316">在這項工作中，您將使用 Internet Explorer **F12 開發人員工具**檢查 Web API 控制器的完整回應。</span><span class="sxs-lookup"><span data-stu-id="1b96f-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="1b96f-317">您會看到您可以擷取網路流量，以取得更多見解應用程式資料的方式。</span><span class="sxs-lookup"><span data-stu-id="1b96f-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="1b96f-318">請確定**Internet Explorer**中選取**啟動**位於 Visual Studio 工具列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="1b96f-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer 選項](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="1b96f-320">**F12 開發人員工具**有一系列豐富的這個實際操作實驗室中未涵蓋的功能。</span><span class="sxs-lookup"><span data-stu-id="1b96f-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="1b96f-321">如果您想要深入了解，請參閱[使用 F12 開發人員工具](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))。</span><span class="sxs-lookup"><span data-stu-id="1b96f-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="1b96f-322">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-323">為了正確遵循這項工作中，您的應用程式必須有資料。</span><span class="sxs-lookup"><span data-stu-id="1b96f-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="1b96f-324">如果您的資料庫是空的您可以回到工作 3 中練習 2，並依照有關如何建立新的人員使用 MVC 檢視。</span><span class="sxs-lookup"><span data-stu-id="1b96f-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="1b96f-325">在瀏覽器中，按**F12**開啟**開發人員工具**面板。</span><span class="sxs-lookup"><span data-stu-id="1b96f-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="1b96f-326">按**CTRL** + **4**或按一下**網路**圖示，，然後按一下綠色箭頭按鈕，開始擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="1b96f-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="1b96f-327">![初始化 Web API 網路擷取](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "起始 Web API 網路擷取")</span><span class="sxs-lookup"><span data-stu-id="1b96f-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="1b96f-328">*初始化 Web API 網路擷取*</span><span class="sxs-lookup"><span data-stu-id="1b96f-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="1b96f-329">附加**api/ApiPerson**至瀏覽器的網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="1b96f-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="1b96f-330">您現在將會檢查回應的詳細資料**ApiPersonController**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="1b96f-331">![擷取人員資料透過 Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "擷取人透過 Web API 的資料")</span><span class="sxs-lookup"><span data-stu-id="1b96f-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="1b96f-332">*擷取人透過 Web API 的資料*</span><span class="sxs-lookup"><span data-stu-id="1b96f-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-333">一旦下載完成時，系統會提示您讓動作與下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="1b96f-334">讓對話方塊保持開啟，才能監看透過開發人員工具視窗的回應內容。</span><span class="sxs-lookup"><span data-stu-id="1b96f-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="1b96f-335">現在您將會檢查回應的主體。</span><span class="sxs-lookup"><span data-stu-id="1b96f-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="1b96f-336">若要這樣做，請按一下**詳細資料**索引標籤，然後按一下 **回應主體**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="1b96f-337">您可以下載的資料是一份物件的屬性來檢查**識別碼**，**名稱**和**年齡**對應到**人員**類別。</span><span class="sxs-lookup"><span data-stu-id="1b96f-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="1b96f-338">![檢視 Web API 回應主體](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "檢視 Web API 回應主體")</span><span class="sxs-lookup"><span data-stu-id="1b96f-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="1b96f-339">*檢視 Web API 回應主體*</span><span class="sxs-lookup"><span data-stu-id="1b96f-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="1b96f-340">工作 3-加入 Web API 說明頁面</span><span class="sxs-lookup"><span data-stu-id="1b96f-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="1b96f-341">當您建立 Web API 時，會很有用來建立一份說明網頁，以便在其他開發人員知道如何呼叫您的 API。</span><span class="sxs-lookup"><span data-stu-id="1b96f-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="1b96f-342">您可以建立及文件頁面手動更新，但最好是自動產生金鑰以避免執行維護工作。</span><span class="sxs-lookup"><span data-stu-id="1b96f-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="1b96f-343">在這項工作會自動使用 Nuget 封裝來產生方案的 Web API 說明頁面。</span><span class="sxs-lookup"><span data-stu-id="1b96f-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="1b96f-344">從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="1b96f-345">在**Package Manager Console**視窗中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1b96f-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="1b96f-346">**Microsoft.AspNet.WebApi.HelpPage**封裝會安裝必要的組件，並且新增 MVC 檢視底下的 [說明] 頁**區域/HelpPage**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="1b96f-347">![HelpPage 區域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 區域")</span><span class="sxs-lookup"><span data-stu-id="1b96f-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="1b96f-348">*HelpPage 區域*</span><span class="sxs-lookup"><span data-stu-id="1b96f-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="1b96f-349">根據預設，說明 頁面會提供文件集的預留位置字串。</span><span class="sxs-lookup"><span data-stu-id="1b96f-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="1b96f-350">若要建立文件，您可以使用 XML 文件註解。</span><span class="sxs-lookup"><span data-stu-id="1b96f-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="1b96f-351">若要啟用這項功能，請開啟**HelpPageConfig.cs**檔案位於**應用程式的區域/HelpPage\_啟動**資料夾並取消註解下列一行：</span><span class="sxs-lookup"><span data-stu-id="1b96f-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="1b96f-352">在**方案總管 中**，以滑鼠右鍵按一下專案**MyHybridSite**，選取**屬性**按一下**建置** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="1b96f-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="1b96f-353">![建置索引標籤](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "建立區段")</span><span class="sxs-lookup"><span data-stu-id="1b96f-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="1b96f-354">*建置索引標籤*</span><span class="sxs-lookup"><span data-stu-id="1b96f-354">*Build tab*</span></span>
5. <span data-ttu-id="1b96f-355">在下**輸出**，選取**XML 文件檔**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="1b96f-356">在編輯方塊中，輸入**應用程式\_Data/XmlDocument.xml**。</span><span class="sxs-lookup"><span data-stu-id="1b96f-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="1b96f-357">![輸出中 [建置] 索引標籤的區段](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "輸出區段中 [建置] 索引標籤")</span><span class="sxs-lookup"><span data-stu-id="1b96f-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="1b96f-358">*輸出區段中 [建置] 索引標籤*</span><span class="sxs-lookup"><span data-stu-id="1b96f-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="1b96f-359">按**CTRL** + **S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="1b96f-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="1b96f-360">開啟**ApiPersonController.cs**檔案從**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="1b96f-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="1b96f-361">輸入新的一行之間*GetPeople*方法簽章和 */ / 取得應用程式開發介面/ApiPerson*註解，然後輸入 三個正斜線。</span><span class="sxs-lookup"><span data-stu-id="1b96f-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-362">Visual Studio 會自動插入的 XML 項目會定義方法的文件。</span><span class="sxs-lookup"><span data-stu-id="1b96f-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="1b96f-363">加入摘要的文字和傳回值*GetPeople*方法。</span><span class="sxs-lookup"><span data-stu-id="1b96f-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="1b96f-364">它看起來應該如下所示。</span><span class="sxs-lookup"><span data-stu-id="1b96f-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="1b96f-365">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="1b96f-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="1b96f-366">附加 **/help**至 URL 網址列中，以瀏覽至 [說明] 頁面。</span><span class="sxs-lookup"><span data-stu-id="1b96f-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="1b96f-367">![ASP.NET Web API 說明頁面](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API 說明頁面")</span><span class="sxs-lookup"><span data-stu-id="1b96f-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="1b96f-368">*ASP.NET Web API 說明頁面*</span><span class="sxs-lookup"><span data-stu-id="1b96f-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="1b96f-369">主頁面的內容是應用程式開發介面，控制站所分組的資料表。</span><span class="sxs-lookup"><span data-stu-id="1b96f-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="1b96f-370">資料表項目使用，以動態方式產生**IApiExplorer**介面。</span><span class="sxs-lookup"><span data-stu-id="1b96f-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="1b96f-371">如果您新增或更新應用程式開發介面控制站時，資料表會自動更新您下次建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="1b96f-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="1b96f-372">**API**欄會列出的 HTTP 方法和相對 URI。</span><span class="sxs-lookup"><span data-stu-id="1b96f-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="1b96f-373">**描述**資料行包含已擷取方法的文件的資訊。</span><span class="sxs-lookup"><span data-stu-id="1b96f-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="1b96f-374">請注意上述的方法定義您新增的描述會顯示描述資料行中。</span><span class="sxs-lookup"><span data-stu-id="1b96f-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="1b96f-375">![API 方法描述](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API 方法的描述")</span><span class="sxs-lookup"><span data-stu-id="1b96f-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="1b96f-376">*應用程式開發介面方法的說明*</span><span class="sxs-lookup"><span data-stu-id="1b96f-376">*API method description*</span></span>
13. <span data-ttu-id="1b96f-377">按一下瀏覽的頁面提供的詳細資訊，包括範例回應主體的應用程式開發介面方法的其中一個。</span><span class="sxs-lookup"><span data-stu-id="1b96f-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="1b96f-378">![詳細資訊 頁面上](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細資訊 頁面")</span><span class="sxs-lookup"><span data-stu-id="1b96f-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="1b96f-379">*詳細的資訊 頁面*</span><span class="sxs-lookup"><span data-stu-id="1b96f-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="1b96f-380">總結</span><span class="sxs-lookup"><span data-stu-id="1b96f-380">Summary</span></span>

<span data-ttu-id="1b96f-381">藉由完成這個實際操作實驗室，您已經學會如何：</span><span class="sxs-lookup"><span data-stu-id="1b96f-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="1b96f-382">建立新的 Web 應用程式，Visual Studio 2013 中使用一個 ASP.NET 體驗</span><span class="sxs-lookup"><span data-stu-id="1b96f-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="1b96f-383">將多個 ASP.NET 技術整合到一個單一的專案</span><span class="sxs-lookup"><span data-stu-id="1b96f-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="1b96f-384">從您使用 ASP.NET Scaffolding 的模型類別產生 MVC 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="1b96f-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="1b96f-385">產生使用功能，例如非同步程式設計，並透過 Entity Framework 資料存取的 Web API 控制器</span><span class="sxs-lookup"><span data-stu-id="1b96f-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="1b96f-386">自動產生 Web 應用程式開發介面的 [說明] 頁面，您的控制站</span><span class="sxs-lookup"><span data-stu-id="1b96f-386">Automatically generate Web API Help Pages for your controllers</span></span>
