---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 實習實驗室： 建置單一頁面應用程式 (SPA) 使用 ASP.NET Web API 和 Angular.js |Microsoft Docs
author: rick-anderson
description: 在傳統的 web 應用程式，用戶端 （瀏覽器） 會起始與伺服器通訊，藉由要求的頁面。 伺服器接著會處理要求...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912250"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a><span data-ttu-id="65fb7-104">實習實驗室： 建置單一頁面應用程式 (SPA) 使用 ASP.NET Web API 和 Angular.js</span><span class="sxs-lookup"><span data-stu-id="65fb7-104">Hands On Lab: Build a Single Page Application (SPA) with ASP.NET Web API and Angular.js</span></span>
====================
<span data-ttu-id="65fb7-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="65fb7-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="65fb7-106">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="65fb7-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="65fb7-107">在傳統的 web 應用程式，用戶端 （瀏覽器） 會起始與伺服器通訊，藉由要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="65fb7-107">In traditional web applications, the client (browser) initiates the communication with the server by requesting a page.</span></span> <span data-ttu-id="65fb7-108">伺服器接著會處理要求，並傳送給用戶端網頁的 HTML。</span><span class="sxs-lookup"><span data-stu-id="65fb7-108">The server then processes the request and sends the HTML of the page to the client.</span></span> <span data-ttu-id="65fb7-109">在後續的互動與頁面 – 例如使用者瀏覽至連結或送出表單資料 – 新的要求會傳送到伺服器，並重新啟動流程： 伺服器處理要求，並將新的頁面傳送至新的動作要求回應中的瀏覽器ed 用戶端。</span><span class="sxs-lookup"><span data-stu-id="65fb7-109">In subsequent interactions with the page –e.g. the user navigates to a link or submits a form with data– a new request is sent to the server, and the flow starts again: the server processes the request and sends a new page to the browser in response to the new action requested by the client.</span></span>
> 
> <span data-ttu-id="65fb7-110">在單一頁面應用程式 (Spa) 整頁瀏覽器中之後載入初始要求中，但後續的互動進行透過 Ajax 要求。</span><span class="sxs-lookup"><span data-stu-id="65fb7-110">In Single-Page Applications (SPAs) the entire page is loaded in the browser after the initial request, but subsequent interactions take place through Ajax requests.</span></span> <span data-ttu-id="65fb7-111">這表示瀏覽器具有更新的頁面已變更; 部份沒有需要重新載入整個頁面。</span><span class="sxs-lookup"><span data-stu-id="65fb7-111">This means that the browser has to update only the portion of the page that has changed; there is no need to reload the entire page.</span></span> <span data-ttu-id="65fb7-112">SPA 方法可減少應用程式以回應使用者動作，導致更流暢的經驗所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="65fb7-112">The SPA approach reduces the time taken by the application to respond to user actions, resulting in a more fluid experience.</span></span>
> 
> <span data-ttu-id="65fb7-113">SPA 架構牽涉到某些不會出現在 傳統 web 應用程式的挑戰。</span><span class="sxs-lookup"><span data-stu-id="65fb7-113">The architecture of a SPA involves certain challenges that are not present in traditional web applications.</span></span> <span data-ttu-id="65fb7-114">不過，新興技術，例如 ASP.NET Web API，例如 AngularJS JavaScript 架構，並提供 CSS3 的新樣式功能會使它很容易設計和建置 Spa。</span><span class="sxs-lookup"><span data-stu-id="65fb7-114">However, emerging technologies like ASP.NET Web API, JavaScript frameworks like AngularJS and new styling features provided by CSS3 make it really easy to design and build SPAs.</span></span>
> 
> <span data-ttu-id="65fb7-115">在此手動在實驗室中，您會利用這些技術來實作 Geek 測驗，SPA 概念為基礎的邏輯網站。</span><span class="sxs-lookup"><span data-stu-id="65fb7-115">In this hand-on lab, you will take advantage of those technologies to implement Geek Quiz, a trivia website based on the SPA concept.</span></span> <span data-ttu-id="65fb7-116">您第一次會實作 ASP.NET Web API，可擷取測驗問題，並儲存回應所需的端點公開 （expose） 服務層。</span><span class="sxs-lookup"><span data-stu-id="65fb7-116">You will first implement the service layer with ASP.NET Web API to expose the required endpoints to retrieve the quiz questions and store the answers.</span></span> <span data-ttu-id="65fb7-117">然後，您將建置豐富且回應迅速的 UI，使用 AngularJS 和 CSS3 轉換效果。</span><span class="sxs-lookup"><span data-stu-id="65fb7-117">Then, you will build a rich and responsive UI using AngularJS and CSS3 transformation effects.</span></span>
> 
> <span data-ttu-id="65fb7-118">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="65fb7-118">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


## <a name="overview"></a><span data-ttu-id="65fb7-119">總覽</span><span class="sxs-lookup"><span data-stu-id="65fb7-119">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="65fb7-120">目標</span><span class="sxs-lookup"><span data-stu-id="65fb7-120">Objectives</span></span>

<span data-ttu-id="65fb7-121">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="65fb7-121">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="65fb7-122">建立 ASP.NET Web API 服務，以傳送和接收 JSON 資料</span><span class="sxs-lookup"><span data-stu-id="65fb7-122">Create an ASP.NET Web API service to send and receive JSON data</span></span>
- <span data-ttu-id="65fb7-123">建立回應式 UI，使用 AngularJS</span><span class="sxs-lookup"><span data-stu-id="65fb7-123">Create a responsive UI using AngularJS</span></span>
- <span data-ttu-id="65fb7-124">增強 CSS3 轉換的 UI 體驗</span><span class="sxs-lookup"><span data-stu-id="65fb7-124">Enhance the UI experience with CSS3 transformations</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="65fb7-125">必要條件</span><span class="sxs-lookup"><span data-stu-id="65fb7-125">Prerequisites</span></span>

<span data-ttu-id="65fb7-126">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="65fb7-126">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="65fb7-127">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="65fb7-127">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="65fb7-128">安裝程式</span><span class="sxs-lookup"><span data-stu-id="65fb7-128">Setup</span></span>

<span data-ttu-id="65fb7-129">若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="65fb7-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="65fb7-130">開啟 Windows Explorer 並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="65fb7-130">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="65fb7-131">以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="65fb7-131">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="65fb7-132">如果會顯示 [使用者帳戶控制] 對話方塊中，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="65fb7-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="65fb7-133">請確定您執行安裝程式之前檢查這個實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="65fb7-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="65fb7-134">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="65fb7-134">Using the Code Snippets</span></span>

<span data-ttu-id="65fb7-135">整個實驗室文件中，您將會指示要插入程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="65fb7-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="65fb7-136">為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="65fb7-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="65fb7-137">每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。</span><span class="sxs-lookup"><span data-stu-id="65fb7-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="65fb7-138">請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。</span><span class="sxs-lookup"><span data-stu-id="65fb7-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="65fb7-139">在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="65fb7-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="65fb7-140">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。</span><span class="sxs-lookup"><span data-stu-id="65fb7-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="65fb7-141">練習</span><span class="sxs-lookup"><span data-stu-id="65fb7-141">Exercises</span></span>

<span data-ttu-id="65fb7-142">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="65fb7-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="65fb7-143">建立 Web API</span><span class="sxs-lookup"><span data-stu-id="65fb7-143">Creating a Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="65fb7-144">建立 SPA 介面</span><span class="sxs-lookup"><span data-stu-id="65fb7-144">Creating a SPA Interface</span></span>](#Exercise2)

<span data-ttu-id="65fb7-145">估計的時間才能完成這個實驗室： **60 分鐘**</span><span class="sxs-lookup"><span data-stu-id="65fb7-145">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="65fb7-146">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="65fb7-146">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="65fb7-147">每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="65fb7-147">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="65fb7-148">在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="65fb7-148">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="65fb7-149">如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。</span><span class="sxs-lookup"><span data-stu-id="65fb7-149">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a><span data-ttu-id="65fb7-150">練習 1： 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="65fb7-150">Exercise 1: Creating a Web API</span></span>

<span data-ttu-id="65fb7-151">SPA 的主要部分是服務層。</span><span class="sxs-lookup"><span data-stu-id="65fb7-151">One of the key parts of a SPA is the service layer.</span></span> <span data-ttu-id="65fb7-152">它會負責處理 UI 和該呼叫回應中的傳回資料所傳送的 Ajax 呼叫。</span><span class="sxs-lookup"><span data-stu-id="65fb7-152">It is responsible for processing the Ajax calls sent by the UI and returning data in response to that call.</span></span> <span data-ttu-id="65fb7-153">擷取的資料應該會看到，才能進行剖析與耗用用戶端電腦可讀取的格式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-153">The data retrieved should be presented in a machine-readable format in order to be parsed and consumed by the client.</span></span>

<span data-ttu-id="65fb7-154">Web API 架構是 ASP.NET 堆疊的一部分，旨在讓您輕鬆地實作 HTTP 服務，通常傳送和接收 JSON 或 XML 格式的資料，透過 RESTful API。</span><span class="sxs-lookup"><span data-stu-id="65fb7-154">The Web API framework is part of the ASP.NET Stack and is designed to make it easy to implement HTTP services, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span> <span data-ttu-id="65fb7-155">在這個練習中，您將建立網站，以裝載 Geek 測驗應用程式，然後再實作後端服務公開 （expose），並使用 ASP.NET Web API 的測驗資料保存。</span><span class="sxs-lookup"><span data-stu-id="65fb7-155">In this exercise you will create the Web site to host the Geek Quiz application and then implement the back-end service to expose and persist the quiz data using ASP.NET Web API.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a><span data-ttu-id="65fb7-156">工作 1-Geek 測驗建立初始專案</span><span class="sxs-lookup"><span data-stu-id="65fb7-156">Task 1 – Creating the Initial Project for Geek Quiz</span></span>

<span data-ttu-id="65fb7-157">在這項工作會開始使用支援建立新的 ASP.NET MVC 專案，ASP.NET Web API 是依據**One ASP.NET**專案類型隨附於 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="65fb7-157">In this task you will start creating a new ASP.NET MVC project with support for ASP.NET Web API based on the **One ASP.NET** project type that comes with Visual Studio.</span></span> <span data-ttu-id="65fb7-158">**One ASP.NET**統一所有 ASP.NET 技術，並可讓您混合及比對它們所需的選項。</span><span class="sxs-lookup"><span data-stu-id="65fb7-158">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="65fb7-159">您接著會新增 Entity Framework 的模型類別和資料庫 initializator，若要插入的測驗問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-159">You will then add the Entity Framework's model classes and the database initializator to insert the quiz questions.</span></span>

1. <span data-ttu-id="65fb7-160">開啟**Visual Studio Express 2013 for Web** ，然後選取**檔案 |新增專案...** 啟動新的解決方案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-160">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    <span data-ttu-id="65fb7-161">![建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "建立新的專案")</span><span class="sxs-lookup"><span data-stu-id="65fb7-161">![Creating a New Project](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Creating a New Project")</span></span>

    <span data-ttu-id="65fb7-162">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-162">*Creating a New Project*</span></span>
2. <span data-ttu-id="65fb7-163">在 [**新的專案**對話方塊中，選取**ASP.NET Web 應用程式**下**Visual C# |Web** ] 索引標籤。請確定 **.NET Framework 4.5**是選取，其命名*GeekQuiz*，選擇**位置**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-163">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab. Make sure **.NET Framework 4.5** is selected, name it *GeekQuiz*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="65fb7-164">![建立新的 ASP.NET Web 應用程式專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "建立新的 ASP.NET Web 應用程式專案")</span><span class="sxs-lookup"><span data-stu-id="65fb7-164">![Creating a new ASP.NET Web Application project](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Creating a new ASP.NET Web Application project")</span></span>

    <span data-ttu-id="65fb7-165">*建立新的 ASP.NET Web 應用程式專案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-165">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="65fb7-166">在 **新的 ASP.NET 專案**對話方塊中，選取**MVC**範本，然後選取**Web API**選項。</span><span class="sxs-lookup"><span data-stu-id="65fb7-166">In the **New ASP.NET Project** dialog box, select the **MVC** template and select the **Web API** option.</span></span> <span data-ttu-id="65fb7-167">此外，請確定**驗證**選項設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-167">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="65fb7-168">按一下 [確定]  繼續操作。</span><span class="sxs-lookup"><span data-stu-id="65fb7-168">Click **OK** to continue.</span></span>

    ![使用 MVC 範本，包括 Web API 元件建立新的專案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    <span data-ttu-id="65fb7-170">*使用 MVC 範本，包括 Web API 元件建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-170">*Creating a new project with the MVC template, including Web API components*</span></span>
4. <span data-ttu-id="65fb7-171">在 [**方案總管] 中**，以滑鼠右鍵按一下**模型**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.</span><span class="sxs-lookup"><span data-stu-id="65fb7-171">In **Solution Explorer**, right-click the **Models** folder of the **GeekQuiz** project and select **Add | Existing Item...**.</span></span>

    <span data-ttu-id="65fb7-172">![加入現有項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "加入現有項目")</span><span class="sxs-lookup"><span data-stu-id="65fb7-172">![Adding an existing item](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Adding an existing item")</span></span>

    <span data-ttu-id="65fb7-173">*加入現有項目*</span><span class="sxs-lookup"><span data-stu-id="65fb7-173">*Adding an existing item*</span></span>
5. <span data-ttu-id="65fb7-174">在 **加入現有項目**對話方塊方塊中，瀏覽至**來源/資產/模型**資料夾，然後選取所有檔案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-174">In the **Add Existing Item** dialog box, navigate to the **Source/Assets/Models** folder and select all the files.</span></span> <span data-ttu-id="65fb7-175">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="65fb7-175">Click **Add**.</span></span>

    <span data-ttu-id="65fb7-176">![新增模型資產](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "加入模型資產")</span><span class="sxs-lookup"><span data-stu-id="65fb7-176">![Adding the model assets](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Adding the model assets")</span></span>

    <span data-ttu-id="65fb7-177">*新增模型資產*</span><span class="sxs-lookup"><span data-stu-id="65fb7-177">*Adding the model assets*</span></span>

    > [!NOTE]
    > <span data-ttu-id="65fb7-178">藉由新增這些檔案，您會加入資料模型、 Entity Framework 資料庫內容和玩家測驗應用程式的資料庫初始設定式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-178">By adding these files, you are adding the data model, the Entity Framework's database context and the database initializer for the Geek Quiz application.</span></span>
    > 
    > <span data-ttu-id="65fb7-179">**Entity Framework (EF)** 是物件關聯式對應程式 (ORM)，可讓您使用而不是直接使用關聯式儲存結構描述的程式設計概念應用程式模型進行程式設計來建立資料存取應用程式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-179">**Entity Framework (EF)** is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span> <span data-ttu-id="65fb7-180">您可以深入了解 Entity Framework[此處](../../../entity-framework.md)。</span><span class="sxs-lookup"><span data-stu-id="65fb7-180">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>
    > 
    > <span data-ttu-id="65fb7-181">以下是您剛才新增的類別的描述：</span><span class="sxs-lookup"><span data-stu-id="65fb7-181">The following is a description of the classes you just added:</span></span>
    > 
    > - <span data-ttu-id="65fb7-182">**TriviaOption:** 表示測驗問題相關聯的單一選項</span><span class="sxs-lookup"><span data-stu-id="65fb7-182">**TriviaOption:** represents a single option associated with a quiz question</span></span>
    > - <span data-ttu-id="65fb7-183">**TriviaQuestion:** 代表測驗問題，並公開 （expose） 相關聯的選項，透過**選項**屬性</span><span class="sxs-lookup"><span data-stu-id="65fb7-183">**TriviaQuestion:** represents a quiz question and exposes the associated options through the **Options** property</span></span>
    > - <span data-ttu-id="65fb7-184">**TriviaAnswer:** 表示測驗問題的回應中的使用者所選取的選項</span><span class="sxs-lookup"><span data-stu-id="65fb7-184">**TriviaAnswer:** represents the option selected by the user in response to a quiz question</span></span>
    > - <span data-ttu-id="65fb7-185">**TriviaContext:** 代表玩家測驗應用程式的 Entity Framework 資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="65fb7-185">**TriviaContext:** represents the Entity Framework's database context of the Geek Quiz application.</span></span> <span data-ttu-id="65fb7-186">這個類別衍生自**DContext** ，並公開**DbSet**代表上面所述的實體集合的屬性。</span><span class="sxs-lookup"><span data-stu-id="65fb7-186">This class derives from **DContext** and exposes **DbSet** properties that represent collections of the entities described above.</span></span>
    > - <span data-ttu-id="65fb7-187">**TriviaDatabaseInitializer:** Entity Framework 的初始設定式的實作**TriviaContext**類別繼承自**CreateDatabaseIfNotExists**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-187">**TriviaDatabaseInitializer:** the implementation of the Entity Framework initializer for the **TriviaContext** class which inherits from **CreateDatabaseIfNotExists**.</span></span> <span data-ttu-id="65fb7-188">這個類別的預設行為是建立資料庫，它不存在時，才，插入的實體中指定**種子**方法。</span><span class="sxs-lookup"><span data-stu-id="65fb7-188">The default behavior of this class is to create the database only if it does not exist, inserting the entities specified in the **Seed** method.</span></span>
6. <span data-ttu-id="65fb7-189">開啟**Global.asax.cs**檔案，並新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-189">Open the **Global.asax.cs** file and add the following using statement.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. <span data-ttu-id="65fb7-190">新增下列程式碼的開頭**應用程式\_開始**方法來設定**TriviaDatabaseInitializer**為資料庫初始設定式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-190">Add the following code at the beginning of the **Application\_Start** method to set the **TriviaDatabaseInitializer** as the database initializer.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. <span data-ttu-id="65fb7-191">修改**首頁**来限制存取的控制器已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="65fb7-191">Modify the **Home** controller to restrict access to authenticated users.</span></span> <span data-ttu-id="65fb7-192">若要這樣做，請開啟**HomeController.cs**檔案內**控制站**資料夾，並新增**授權**屬性設定為**HomeController**類別定義。</span><span class="sxs-lookup"><span data-stu-id="65fb7-192">To do this, open the **HomeController.cs** file inside the **Controllers** folder and add the **Authorize** attribute to the **HomeController** class definition.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-193">**授權**篩選檢查，以查看是否已驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="65fb7-193">The **Authorize** filter checks to see if the user is authenticated.</span></span> <span data-ttu-id="65fb7-194">如果使用者未經過驗證，它會傳回 HTTP 狀態碼 401 （未經授權），而不叫用動作。</span><span class="sxs-lookup"><span data-stu-id="65fb7-194">If the user is not authenticated, it returns HTTP status code 401 (Unauthorized) without invoking the action.</span></span> <span data-ttu-id="65fb7-195">您可以套用全域，在控制器層級，或個別動作的層級的篩選器。</span><span class="sxs-lookup"><span data-stu-id="65fb7-195">You can apply the filter globally, at the controller level, or at the level of individual actions.</span></span>
9. <span data-ttu-id="65fb7-196">現在，您將自訂網頁和商標版面的配置。</span><span class="sxs-lookup"><span data-stu-id="65fb7-196">You will now customize the layout of the web pages and the branding.</span></span> <span data-ttu-id="65fb7-197">若要這樣做，請開啟 **\_Layout.cshtml**檔案**檢視 |共用**資料夾，並更新的內容**&lt;標題&gt;** 取代的項目*My ASP.NET Application*與*Geek 測驗*.</span><span class="sxs-lookup"><span data-stu-id="65fb7-197">To do this, open the **\_Layout.cshtml** file inside the **Views | Shared** folder and update the content of the **&lt;title&gt;** element by replacing *My ASP.NET Application* with *Geek Quiz*.</span></span>

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. <span data-ttu-id="65fb7-198">在相同的檔案中，更新導覽列藉由移除*關於*並*連絡人*連結和重新命名*首頁*連結至*播放*。</span><span class="sxs-lookup"><span data-stu-id="65fb7-198">In the same file, update the navigation bar by removing the *About* and *Contact* links and renaming the *Home* link to *Play*.</span></span> <span data-ttu-id="65fb7-199">此外，重新命名*應用程式名稱*連結至*Geek 測驗*。</span><span class="sxs-lookup"><span data-stu-id="65fb7-199">Additionally, rename the *Application name* link to *Geek Quiz*.</span></span> <span data-ttu-id="65fb7-200">在導覽列 HTML 看起來應該類似下列的程式碼。</span><span class="sxs-lookup"><span data-stu-id="65fb7-200">The HTML for the navigation bar should look like the following code.</span></span>

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. <span data-ttu-id="65fb7-201">藉由取代更新的版面配置頁的頁尾*My ASP.NET Application*具有*Geek 測驗*。</span><span class="sxs-lookup"><span data-stu-id="65fb7-201">Update the footer of the layout page by replacing *My ASP.NET Application* with *Geek Quiz*.</span></span> <span data-ttu-id="65fb7-202">若要這樣做，請將取代的內容**&lt;頁尾&gt;** 具有下列醒目提示的程式碼項目。</span><span class="sxs-lookup"><span data-stu-id="65fb7-202">To do this, replace the content of the **&lt;footer&gt;** element with the following highlighted code.</span></span>

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a><span data-ttu-id="65fb7-203">工作 2-建立 TriviaController Web API</span><span class="sxs-lookup"><span data-stu-id="65fb7-203">Task 2 – Creating the TriviaController Web API</span></span>

<span data-ttu-id="65fb7-204">在先前工作中，您會建立初始結構的玩家測驗的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-204">In the previous task, you created the initial structure of the Geek Quiz web application.</span></span> <span data-ttu-id="65fb7-205">現在，您將建置一個簡單的 Web API 服務，與測驗資料模型互動，而且會公開下列動作：</span><span class="sxs-lookup"><span data-stu-id="65fb7-205">You will now build a simple Web API service that interacts with the quiz data model and exposes the following actions:</span></span>

- <span data-ttu-id="65fb7-206">**GET/api/邏輯**： 測驗清單來驗證的使用者回應中擷取下一個問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-206">**GET /api/trivia**: Retrieves the next question from the quiz list to be answered by the authenticated user.</span></span>
- <span data-ttu-id="65fb7-207">**POST/api/邏輯**： 儲存已驗證的使用者所指定的測驗解答。</span><span class="sxs-lookup"><span data-stu-id="65fb7-207">**POST /api/trivia**: Stores the quiz answer specified by the authenticated user.</span></span>

<span data-ttu-id="65fb7-208">您將使用 Visual Studio 所提供的 ASP.NET Scaffolding 工具來建立 Web API 控制器類別的基準。</span><span class="sxs-lookup"><span data-stu-id="65fb7-208">You will use the ASP.NET Scaffolding tools provided by Visual Studio to create the baseline for the Web API controller class.</span></span>

1. <span data-ttu-id="65fb7-209">開啟**WebApiConfig.cs**檔案內**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="65fb7-209">Open the **WebApiConfig.cs** file inside the **App\_Start** folder.</span></span> <span data-ttu-id="65fb7-210">此檔案會定義 Web API 服務，例如路由如何對應至 Web API 控制器動作的設定。</span><span class="sxs-lookup"><span data-stu-id="65fb7-210">This file defines the configuration of the Web API service, like how routes are mapped to Web API controller actions.</span></span>
2. <span data-ttu-id="65fb7-211">新增下列 using 陳述式在檔案開頭。</span><span class="sxs-lookup"><span data-stu-id="65fb7-211">Add the following using statement at the beginning of the file.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. <span data-ttu-id="65fb7-212">將下列反白顯示的程式碼，加入**註冊**全域設定 Web API 動作方法所擷取的 JSON 資料的格式器的方法。</span><span class="sxs-lookup"><span data-stu-id="65fb7-212">Add the following highlighted code to the **Register** method to globally configure the formatter for the JSON data retrieved by the Web API action methods.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-213">**CamelCasePropertyNamesContractResolver**會自動轉換到的屬性名稱*依照 camel 命名法*情況下，也就是在 JavaScript 中的屬性名稱的一般慣例。</span><span class="sxs-lookup"><span data-stu-id="65fb7-213">The **CamelCasePropertyNamesContractResolver** automatically converts property names to *camel* case, which is the general convention for property names in JavaScript.</span></span>
4. <span data-ttu-id="65fb7-214">在 [**方案總管] 中**，以滑鼠右鍵按一下**控制站**資料夾**GeekQuiz**專案，然後選取**新增 |新增 Scaffold 項目...**.</span><span class="sxs-lookup"><span data-stu-id="65fb7-214">In **Solution Explorer**, right-click the **Controllers** folder of the **GeekQuiz** project and select **Add | New Scaffolded Item...**.</span></span>

    <span data-ttu-id="65fb7-215">![建立新的 scaffold 項目](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "建立新的 scaffold 項目")</span><span class="sxs-lookup"><span data-stu-id="65fb7-215">![Creating a new scaffolded item](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Creating a new scaffolded item")</span></span>

    <span data-ttu-id="65fb7-216">*建立新的 scaffold 項目*</span><span class="sxs-lookup"><span data-stu-id="65fb7-216">*Creating a new scaffolded item*</span></span>
5. <span data-ttu-id="65fb7-217">在 **新增 Scaffold**對話方塊方塊中，請確定**常見**的左窗格中選取節點。</span><span class="sxs-lookup"><span data-stu-id="65fb7-217">In the **Add Scaffold** dialog box, make sure that the **Common** node is selected in the left pane.</span></span> <span data-ttu-id="65fb7-218">然後，選取**Web API 2 控制器-空白**中央窗格中按一下範本**新增**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-218">Then, select the **Web API 2 Controller - Empty** template in the center pane and click **Add**.</span></span>

    <span data-ttu-id="65fb7-219">![選取 Web API 2 空白控制器樣板](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "選取 Web API 2 空白控制器的範本")</span><span class="sxs-lookup"><span data-stu-id="65fb7-219">![Selecting the Web API 2 Controller Empty template](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Selecting the Web API 2 Controller Empty template")</span></span>

    <span data-ttu-id="65fb7-220">*選取 Web API 2 空白控制器的範本*</span><span class="sxs-lookup"><span data-stu-id="65fb7-220">*Selecting the Web API 2 Controller Empty template*</span></span>

    > [!NOTE]
    > <span data-ttu-id="65fb7-221">**ASP.NET Scaffolding**是 ASP.NET Web 應用程式的程式碼產生架構。</span><span class="sxs-lookup"><span data-stu-id="65fb7-221">**ASP.NET Scaffolding** is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="65fb7-222">Visual Studio 2013 包含預先安裝的程式碼產生器，適用於 MVC 和 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-222">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="65fb7-223">當您想要快速地加入開發標準的資料作業所需的程式碼與資料模型互動，以降低的時間量，您應該在專案中使用 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="65fb7-223">You should use scaffolding in your project when you want to quickly add code that interacts with data models in order to reduce the amount of time required to develop standard data operations.</span></span>
    > 
    > <span data-ttu-id="65fb7-224">Scaffolding 程序也可確保在專案中安裝的所有必要的相依性。</span><span class="sxs-lookup"><span data-stu-id="65fb7-224">The scaffolding process also ensures that all the required dependencies are installed in the project.</span></span> <span data-ttu-id="65fb7-225">比方說，如果您開始 空白的 ASP.NET 專案，然後使用 scaffolding Web API 控制器，將必要的 Web API NuGet 套件和參考會加入至您的專案自動。</span><span class="sxs-lookup"><span data-stu-id="65fb7-225">For example, if you start with an empty ASP.NET project and then use scaffolding to add a Web API controller, the required Web API NuGet packages and references are added to your project automatically.</span></span>
6. <span data-ttu-id="65fb7-226">在 [**新增控制器**] 對話方塊中，輸入*TriviaController*中**控制器名稱**文字方塊中，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-226">In the **Add Controller** dialog box, type *TriviaController* in the **Controller name** text box and click **Add**.</span></span>

    <span data-ttu-id="65fb7-227">![新增邏輯控制站](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "新增邏輯控制站")</span><span class="sxs-lookup"><span data-stu-id="65fb7-227">![Adding the Trivia Controller](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Adding the Trivia Controller")</span></span>

    <span data-ttu-id="65fb7-228">*新增邏輯控制站*</span><span class="sxs-lookup"><span data-stu-id="65fb7-228">*Adding the Trivia Controller*</span></span>
7. <span data-ttu-id="65fb7-229">**TriviaController.cs**檔案接著會新增至**控制站**資料夾**GeekQuiz**包含空的專案**TriviaController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-229">The **TriviaController.cs** file is then added to the **Controllers** folder of the **GeekQuiz** project, containing an empty **TriviaController** class.</span></span> <span data-ttu-id="65fb7-230">新增下列 using 陳述式在檔案開頭。</span><span class="sxs-lookup"><span data-stu-id="65fb7-230">Add the following using statements at the beginning of the file.</span></span>

    <span data-ttu-id="65fb7-231">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerUsings*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-231">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. <span data-ttu-id="65fb7-232">新增下列程式碼的開頭**TriviaController**類別來定義、 初始化和處置**TriviaContext**控制器中的執行個體。</span><span class="sxs-lookup"><span data-stu-id="65fb7-232">Add the following code at the beginning of the **TriviaController** class to define, initialize and dispose the **TriviaContext** instance in the controller.</span></span>

    <span data-ttu-id="65fb7-233">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerContext*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-233">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-234">**處置**方法**TriviaController**叫用**處置**方法**TriviaContext**執行個體，這可確保所有發行的內容物件所使用的資源時**TriviaContext**執行個體已遭處置或記憶體回收。</span><span class="sxs-lookup"><span data-stu-id="65fb7-234">The **Dispose** method of **TriviaController** invokes the **Dispose** method of the **TriviaContext** instance, which ensures that all the resources used by the context object are released when the **TriviaContext** instance is disposed or garbage-collected.</span></span> <span data-ttu-id="65fb7-235">這包括關閉所有開啟的 Entity Framework 的資料庫連線。</span><span class="sxs-lookup"><span data-stu-id="65fb7-235">This includes closing all database connections opened by Entity Framework.</span></span>
9. <span data-ttu-id="65fb7-236">新增下列 helper 方法的結尾**TriviaController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-236">Add the following helper method at the end of the **TriviaController** class.</span></span> <span data-ttu-id="65fb7-237">這個方法會擷取下列測驗問題，從指定的使用者來回應的資料庫。</span><span class="sxs-lookup"><span data-stu-id="65fb7-237">This method retrieves the following quiz question from the database to be answered by the specified user.</span></span>

    <span data-ttu-id="65fb7-238">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerNextQuestion*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-238">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. <span data-ttu-id="65fb7-239">新增下列**取得**動作方法，來**TriviaController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-239">Add the following **Get** action method to the **TriviaController** class.</span></span> <span data-ttu-id="65fb7-240">這個動作方法會呼叫**NextQuestionAsync** helper 方法來擷取下一個問題，已驗證之使用者的上一個步驟中所定義。</span><span class="sxs-lookup"><span data-stu-id="65fb7-240">This action method calls the **NextQuestionAsync** helper method defined in the previous step to retrieve the next question for the authenticated user.</span></span>

    <span data-ttu-id="65fb7-241">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerGetAction*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-241">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. <span data-ttu-id="65fb7-242">新增下列 helper 方法的結尾**TriviaController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-242">Add the following helper method at the end of the **TriviaController** class.</span></span> <span data-ttu-id="65fb7-243">這個方法會在資料庫中儲存指定的回應，並傳回布林值，指出是正確解答。</span><span class="sxs-lookup"><span data-stu-id="65fb7-243">This method stores the specified answer in the database and returns a Boolean value indicating whether or not the answer is correct.</span></span>

    <span data-ttu-id="65fb7-244">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerStoreAsync*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-244">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. <span data-ttu-id="65fb7-245">新增下列**Post**動作方法，來**TriviaController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-245">Add the following **Post** action method to the **TriviaController** class.</span></span> <span data-ttu-id="65fb7-246">這個動作方法產生關聯的已驗證的使用者和呼叫的答案**StoreAsync** helper 方法。</span><span class="sxs-lookup"><span data-stu-id="65fb7-246">This action method associates the answer to the authenticated user and calls the **StoreAsync** helper method.</span></span> <span data-ttu-id="65fb7-247">然後，它會傳送回應與協助程式方法所傳回的布林值。</span><span class="sxs-lookup"><span data-stu-id="65fb7-247">Then, it sends a response with the Boolean value returned by the helper method.</span></span>

    <span data-ttu-id="65fb7-248">(程式碼片段- *AspNetWebApiSpa-Ex1-TriviaControllerPostAction*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-248">(Code Snippet - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. <span data-ttu-id="65fb7-249">修改 Web API 控制器來限制存取已驗證的使用者，藉由新增**Authorize**屬性設定為**TriviaController**類別定義。</span><span class="sxs-lookup"><span data-stu-id="65fb7-249">Modify the Web API controller to restrict access to authenticated users by adding the **Authorize** attribute to the **TriviaController** class definition.</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="65fb7-250">工作 3-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="65fb7-250">Task 3 – Running the Solution</span></span>

<span data-ttu-id="65fb7-251">在這項工作中，您將會驗證您在上一個工作中建立的 Web API 服務運作正常。</span><span class="sxs-lookup"><span data-stu-id="65fb7-251">In this task you will verify that the Web API service you built in the previous task is working as expected.</span></span> <span data-ttu-id="65fb7-252">您將使用 Internet Explorer **F12 開發人員工具**擷取網路流量，並檢查 Web API 服務的完整回應。</span><span class="sxs-lookup"><span data-stu-id="65fb7-252">You will use the Internet Explorer **F12 Developer Tools** to capture the network traffic and inspect the full response from the Web API service.</span></span>

> [!NOTE]
> <span data-ttu-id="65fb7-253">請確定**Internet Explorer**中選取**開始**位於 Visual Studio 工具列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="65fb7-253">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer 選項](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. <span data-ttu-id="65fb7-255">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-255">Press **F5** to run the solution.</span></span> <span data-ttu-id="65fb7-256">**登入**頁面應該會出現在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="65fb7-256">The **Log in** page should appear in the browser.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65fb7-257">當應用程式啟動時，預設 MVC 路由觸發時，預設會對應至**Index**動作**HomeController**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-257">When the application starts, the default MVC route is triggered, which by default is mapped to the **Index** action of the **HomeController** class.</span></span> <span data-ttu-id="65fb7-258">由於**HomeController**限制為已驗證的使用者 (請記住您裝飾具有該類別**授權**在練習 1 中的屬性)，而且沒有任何已驗證的使用者，應用程式將原始的要求重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="65fb7-258">Since **HomeController** is restricted to authenticated users (remember that you decorated that class with the **Authorize** attribute in Exercise 1) and there is no user authenticated yet, the application redirects the original request to the log in page.</span></span>

    <span data-ttu-id="65fb7-259">![執行解決方案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "執行解決方案")</span><span class="sxs-lookup"><span data-stu-id="65fb7-259">![Running the solution](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Running the solution")</span></span>

    <span data-ttu-id="65fb7-260">*執行解決方案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-260">*Running the solution*</span></span>
2. <span data-ttu-id="65fb7-261">按一下 **註冊**來建立新的使用者。</span><span class="sxs-lookup"><span data-stu-id="65fb7-261">Click **Register** to create a new user.</span></span>

    <span data-ttu-id="65fb7-262">![註冊新的使用者](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "註冊新的使用者")</span><span class="sxs-lookup"><span data-stu-id="65fb7-262">![Registering a new user](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registering a new user")</span></span>

    <span data-ttu-id="65fb7-263">*註冊新的使用者*</span><span class="sxs-lookup"><span data-stu-id="65fb7-263">*Registering a new user*</span></span>
3. <span data-ttu-id="65fb7-264">在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-264">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    <span data-ttu-id="65fb7-265">![註冊頁面](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "註冊 頁面")</span><span class="sxs-lookup"><span data-stu-id="65fb7-265">![Register page](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Register page")</span></span>

    <span data-ttu-id="65fb7-266">*註冊頁面*</span><span class="sxs-lookup"><span data-stu-id="65fb7-266">*Register page*</span></span>
4. <span data-ttu-id="65fb7-267">應用程式註冊新的帳戶，並驗證使用者並將其重新導向回到首頁。</span><span class="sxs-lookup"><span data-stu-id="65fb7-267">The application registers the new account and the user is authenticated and redirected back to the home page.</span></span>

    <span data-ttu-id="65fb7-268">![使用者驗證](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "已驗證的使用者")</span><span class="sxs-lookup"><span data-stu-id="65fb7-268">![User is authenticated](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "User authenticated")</span></span>

    <span data-ttu-id="65fb7-269">*使用者驗證*</span><span class="sxs-lookup"><span data-stu-id="65fb7-269">*User is authenticated*</span></span>
5. <span data-ttu-id="65fb7-270">在瀏覽器中，按下**F12**來開啟**開發人員工具**面板。</span><span class="sxs-lookup"><span data-stu-id="65fb7-270">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="65fb7-271">按下**CTRL + 4**或按**網路**圖示，然後按一下綠色箭號按鈕以開始擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="65fb7-271">Press **CTRL + 4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="65fb7-272">![起始 Web API 網路擷取](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "起始 Web API 網路擷取")</span><span class="sxs-lookup"><span data-stu-id="65fb7-272">![Initiating Web API network capture](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="65fb7-273">*起始的 Web API 網路擷取*</span><span class="sxs-lookup"><span data-stu-id="65fb7-273">*Initiating Web API network capture*</span></span>
6. <span data-ttu-id="65fb7-274">附加**api/邏輯**至瀏覽器的網址列中的 URL。</span><span class="sxs-lookup"><span data-stu-id="65fb7-274">Append **api/trivia** to the URL in the browser's address bar.</span></span> <span data-ttu-id="65fb7-275">您現在會檢查來自回應的詳細資料**取得**中的動作方法**TriviaController**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-275">You will now inspect the details of the response from the **Get** action method in **TriviaController**.</span></span>

    <span data-ttu-id="65fb7-276">![擷取下一個問題資料透過 Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "擷取下一個問題資料透過 Web API")</span><span class="sxs-lookup"><span data-stu-id="65fb7-276">![Retrieving the next question data through Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Retrieving the next question data through Web API")</span></span>

    <span data-ttu-id="65fb7-277">*擷取下一個問題資料透過 Web API*</span><span class="sxs-lookup"><span data-stu-id="65fb7-277">*Retrieving the next question data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="65fb7-278">下載完成後，系統會提示您使用下載的檔案進行的動作。</span><span class="sxs-lookup"><span data-stu-id="65fb7-278">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="65fb7-279">讓對話方塊保持開啟，才能夠觀賞回應內容，透過開發人員工具視窗。</span><span class="sxs-lookup"><span data-stu-id="65fb7-279">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
7. <span data-ttu-id="65fb7-280">現在，您將會檢查回應的主體。</span><span class="sxs-lookup"><span data-stu-id="65fb7-280">Now you will inspect the body of the response.</span></span> <span data-ttu-id="65fb7-281">若要這樣做，請按一下**詳細資料**索引標籤，然後按一下**回應主體**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-281">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="65fb7-282">您可以檢查已下載的資料是否具有屬性的物件**選項**(這是一份**TriviaOption**物件)，**識別碼**並**標題**對應到**TriviaQuestion**類別。</span><span class="sxs-lookup"><span data-stu-id="65fb7-282">You can check that the downloaded data is an object with the properties **options** (which is a list of **TriviaOption** objects), **id** and **title** that correspond to the **TriviaQuestion** class.</span></span>

    <span data-ttu-id="65fb7-283">![檢視 Web API 回應內文](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "檢視 Web API 回應主體")</span><span class="sxs-lookup"><span data-stu-id="65fb7-283">![Viewing the Web API Response Body](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Viewing the Web API Response Body")</span></span>

    <span data-ttu-id="65fb7-284">*檢視 Web API 回應主體*</span><span class="sxs-lookup"><span data-stu-id="65fb7-284">*Viewing Web API Response Body*</span></span>
8. <span data-ttu-id="65fb7-285">請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="65fb7-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a><span data-ttu-id="65fb7-286">練習 2： 建立 SPA 介面</span><span class="sxs-lookup"><span data-stu-id="65fb7-286">Exercise 2: Creating the SPA Interface</span></span>

<span data-ttu-id="65fb7-287">在這個練習中您需要先建立的 web 前端部分 Geek 測驗，將焦點放在單一頁面應用程式互動使用**AngularJS**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-287">In this exercise you will first build the web front-end portion of Geek Quiz, focusing on the Single-Page Application interaction using **AngularJS**.</span></span> <span data-ttu-id="65fb7-288">然後，您將會增強 CSS3 執行豐富的動畫，並提供內容切換時從一個問題轉換到下一個視覺效果的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="65fb7-288">You will then enhance the user experience with CSS3 to perform rich animations and provide a visual effect of context switching when transitioning from one question to the next.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a><span data-ttu-id="65fb7-289">工作 1-建立使用 AngularJS SPA 介面</span><span class="sxs-lookup"><span data-stu-id="65fb7-289">Task 1 – Creating the SPA Interface Using AngularJS</span></span>

<span data-ttu-id="65fb7-290">在這項工作會使用**AngularJS**實作 Geek 測驗應用程式的用戶端。</span><span class="sxs-lookup"><span data-stu-id="65fb7-290">In this task you will use **AngularJS** to implement the client side of the Geek Quiz application.</span></span> <span data-ttu-id="65fb7-291">**AngularJS**是一種開放原始碼 JavaScript 架構，可增強使用瀏覽器為基礎的應用程式*模型-檢視-控制器*(MVC) 功能，可促進開發和測試。</span><span class="sxs-lookup"><span data-stu-id="65fb7-291">**AngularJS** is an open-source JavaScript framework that augments browser-based applications with *Model-View-Controller* (MVC) capability, facilitating both development and testing.</span></span>

<span data-ttu-id="65fb7-292">您會藉由從 Visual Studio Package Manager Console 安裝 AngularJS 來開始。</span><span class="sxs-lookup"><span data-stu-id="65fb7-292">You will start by installing AngularJS from Visual Studio's Package Manager Console.</span></span> <span data-ttu-id="65fb7-293">然後，您將建立的控制站，以提供玩家測驗應用程式和檢視來呈現的測驗問題與解答使用 AngularJS 範本引擎的行為。</span><span class="sxs-lookup"><span data-stu-id="65fb7-293">Then, you will create the controller to provide the behavior of the Geek Quiz app and the view to render the quiz questions and answers using the AngularJS template engine.</span></span>

> [!NOTE]
> <span data-ttu-id="65fb7-294">如需 AngularJS 的詳細資訊，請參閱[ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/)。</span><span class="sxs-lookup"><span data-stu-id="65fb7-294">For more information about AngularJS, refer to [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).</span></span>


1. <span data-ttu-id="65fb7-295">開啟**Visual Studio Express 2013 for Web** ，然後開啟**GeekQuiz.sln**解決方案位於**來源/Ex2-CreatingASPAInterface/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="65fb7-295">Open **Visual Studio Express 2013 for Web** and open the **GeekQuiz.sln** solution located in the **Source/Ex2-CreatingASPAInterface/Begin** folder.</span></span> <span data-ttu-id="65fb7-296">或者，您可以繼續使用解決方案您在前一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="65fb7-296">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="65fb7-297">開啟**Package Manager Console**從**工具** > **NuGet 套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-297">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="65fb7-298">輸入下列命令以安裝**AngularJS.Core** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-298">Type the following command to install the **AngularJS.Core** NuGet package.</span></span>

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. <span data-ttu-id="65fb7-299">在 [**方案總管] 中**，以滑鼠右鍵按一下**指令碼**資料夾**GeekQuiz**專案，然後選取**新增 |新的資料夾**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-299">In **Solution Explorer**, right-click the **Scripts** folder of the **GeekQuiz** project and select **Add | New Folder**.</span></span> <span data-ttu-id="65fb7-300">將資料夾命名**應用程式**然後按**Enter**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-300">Name the folder **app** and press **Enter**.</span></span>
4. <span data-ttu-id="65fb7-301">以滑鼠右鍵按一下**應用程式**資料夾，您剛建立，並選取**新增 |JavaScript 檔案**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-301">Right-click the **app** folder you just created and select **Add | JavaScript File**.</span></span>

    ![建立新的 JavaScript 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    <span data-ttu-id="65fb7-303">*建立新的 JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-303">*Creating a new JavaScript file*</span></span>
5. <span data-ttu-id="65fb7-304">在 [**指定項目的名稱**] 對話方塊中，輸入*測驗控制器*中**項目名稱**文字方塊中，然後按一下 **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-304">In the **Specify Name for Item** dialog box, type *quiz-controller* in the **Item name** text box and click **OK**.</span></span>

    ![命名新的 JavaScript 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    <span data-ttu-id="65fb7-306">*命名新的 JavaScript 檔案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-306">*Naming the new JavaScript file*</span></span>
6. <span data-ttu-id="65fb7-307">在 **測驗 controller.js**檔案中，新增下列程式碼來宣告和初始化 AngularJS **QuizCtrl**控制站。</span><span class="sxs-lookup"><span data-stu-id="65fb7-307">In the **quiz-controller.js** file, add the following code to declare and initialize the AngularJS **QuizCtrl** controller.</span></span>

    <span data-ttu-id="65fb7-308">(程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizController*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-308">(Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizController*)</span></span>

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-309">建構函式的**QuizCtrl**控制器需要名為 injectable 參數 **$scope**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-309">The constructor function of the **QuizCtrl** controller expects an injectable parameter named **$scope**.</span></span> <span data-ttu-id="65fb7-310">範圍的初始狀態應該設定在建構函式藉由附加屬性，以 **$scope**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-310">The initial state of the scope should be set up in the constructor function by attaching properties to the **$scope** object.</span></span> <span data-ttu-id="65fb7-311">屬性包含**檢視模型**，並可讓範本註冊控制器時。</span><span class="sxs-lookup"><span data-stu-id="65fb7-311">The properties contain the **view model**, and will be accessible to the template when the controller is registered.</span></span>
    > 
    > <span data-ttu-id="65fb7-312">**QuizCtrl**名為模組內定義控制器**QuizApp**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-312">The **QuizCtrl** controller is defined inside a module named **QuizApp**.</span></span> <span data-ttu-id="65fb7-313">模組是工作的可讓您單位會將您的應用程式分成個別的元件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-313">Modules are units of work that let you break your application into separate components.</span></span> <span data-ttu-id="65fb7-314">使用模組的主要優點是容易了解的程式碼，並有助於單元測試、 可重複使用性和可維護性。</span><span class="sxs-lookup"><span data-stu-id="65fb7-314">The main advantages of using modules is that the code is easier to understand and facilitates unit testing, reusability and maintainability.</span></span>
7. <span data-ttu-id="65fb7-315">您現在會新增至範圍的行為，以回應事件觸發從檢視。</span><span class="sxs-lookup"><span data-stu-id="65fb7-315">You will now add behavior to the scope in order to react to events triggered from the view.</span></span> <span data-ttu-id="65fb7-316">在結尾新增下列程式碼**QuizCtrl**定義的控制站**nextQuestion**函式中 **$scope**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-316">Add the following code at the end of the **QuizCtrl** controller to define the **nextQuestion** function in the **$scope** object.</span></span>

    <span data-ttu-id="65fb7-317">(程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-317">(Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)</span></span>

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-318">此函式會擷取下一個問題，從**邏輯**Web API 在前一個練習中建立並附加到的問題資料 **$scope**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-318">This function retrieves the next question from the **Trivia** Web API created in the previous exercise and attaches the question data to the **$scope** object.</span></span>
8. <span data-ttu-id="65fb7-319">插入下列程式碼結尾處**QuizCtrl**定義的控制站**sendAnswer**函式中 **$scope**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-319">Insert the following code at the end of the **QuizCtrl** controller to define the **sendAnswer** function in the **$scope** object.</span></span>

    <span data-ttu-id="65fb7-320">(程式碼片段- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-320">(Code Snippet - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)</span></span>

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-321">此函式會將傳送到使用者所選取的答案**邏輯**Web API，並將結果 – 也就是如果答案是否正確 – 在 **$scope**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-321">This function sends the answer selected by the user to the **Trivia** Web API and stores the result –i.e. if the answer is correct or not– in the **$scope** object.</span></span>
    > 
    > <span data-ttu-id="65fb7-322">**NextQuestion**並**sendAnswer**上述的函式會使用 AngularJS **$http**抽象化的通訊使用透過 XMLHttpRequest 的 Web API 的物件從瀏覽器的 JavaScript 物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-322">The **nextQuestion** and **sendAnswer** functions from above use the AngularJS **$http** object to abstract the communication with the Web API via the XMLHttpRequest JavaScript object from the browser.</span></span> <span data-ttu-id="65fb7-323">AngularJS 支援另一項服務，讓較高層級的抽象概念，來執行 CRUD 作業，針對透過 RESTful Api 的資源。</span><span class="sxs-lookup"><span data-stu-id="65fb7-323">AngularJS supports another service that brings a higher level of abstraction to perform CRUD operations against a resource through RESTful APIs.</span></span> <span data-ttu-id="65fb7-324">AngularJS **$resource**物件具有動作方法提供高階的行為，而不需要互動 **$http**物件。</span><span class="sxs-lookup"><span data-stu-id="65fb7-324">The AngularJS **$resource** object has action methods which provide high-level behaviors without the need to interact with the **$http** object.</span></span> <span data-ttu-id="65fb7-325">請考慮使用 **$resource**案例中需要 CRUD 模型的物件 (前景的詳細資訊，請參閱[$resource 文件](https://docs.angularjs.org/api/ngResource/service/$resource))。</span><span class="sxs-lookup"><span data-stu-id="65fb7-325">Consider using the **$resource** object in scenarios that requires the CRUD model (fore information, see the [$resource documentation](https://docs.angularjs.org/api/ngResource/service/$resource)).</span></span>
9. <span data-ttu-id="65fb7-326">下一個步驟是建立定義的檢視測驗的 AngularJS 範本。</span><span class="sxs-lookup"><span data-stu-id="65fb7-326">The next step is to create the AngularJS template that defines the view for the quiz.</span></span> <span data-ttu-id="65fb7-327">若要這樣做，請開啟**Index.cshtml**檔案內**檢視 |首頁**資料夾，並將下列程式碼的內容。</span><span class="sxs-lookup"><span data-stu-id="65fb7-327">To do this, open the **Index.cshtml** file inside the **Views | Home** folder and replace the content with the following code.</span></span>

    <span data-ttu-id="65fb7-328">(程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizView*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-328">(Code Snippet - *AspNetWebApiSpa - Ex2 - GeekQuizView*)</span></span>

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="65fb7-329">AngularJS 範本是宣告式的規格，會使用從模型和控制器的資訊，將靜態標記轉換成瀏覽器中的使用者會看到動態檢視。</span><span class="sxs-lookup"><span data-stu-id="65fb7-329">The AngularJS template is a declarative specification that uses information from the model and the controller to transform static markup into the dynamic view that the user sees in the browser.</span></span> <span data-ttu-id="65fb7-330">AngularJS 元素和可用範本中的項目屬性的範例如下︰</span><span class="sxs-lookup"><span data-stu-id="65fb7-330">The following are examples of AngularJS elements and element attributes that can be used in a template:</span></span>
    > 
    > - <span data-ttu-id="65fb7-331">**Ng 應用程式**指示詞會告訴 AngularJS DOM 項目，表示應用程式的根項目。</span><span class="sxs-lookup"><span data-stu-id="65fb7-331">The **ng-app** directive tells AngularJS the DOM element that represents the root element of the application.</span></span>
    > - <span data-ttu-id="65fb7-332">**Ng 控制器**指示詞會將控制器附加至 DOM 的點指示詞宣告的位置。</span><span class="sxs-lookup"><span data-stu-id="65fb7-332">The **ng-controller** directive attaches a controller to the DOM at the point where the directive is declared.</span></span>
    > - <span data-ttu-id="65fb7-333">大括號標記法 **{{}}** 表示控制器中定義的領域屬性的繫結。</span><span class="sxs-lookup"><span data-stu-id="65fb7-333">The curly brace notation **{{ }}** denotes bindings to the scope properties defined in the controller.</span></span>
    > - <span data-ttu-id="65fb7-334">**Ng 按一下**指示詞用來叫用在以回應使用者按一下範圍中定義的函式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-334">The **ng-click** directive is used to invoke the functions defined in the scope in response to user clicks.</span></span>
10. <span data-ttu-id="65fb7-335">開啟**Site.css**檔案內**內容**資料夾和檔案，以提供測驗檢視的外觀與風格的結尾加入下列反白顯示的樣式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-335">Open the **Site.css** file inside the **Content** folder and add the following highlighted styles at the end of the file to provide a look and feel for the quiz view.</span></span>

    <span data-ttu-id="65fb7-336">(程式碼片段- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)</span><span class="sxs-lookup"><span data-stu-id="65fb7-336">(Code Snippet - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)</span></span>

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="65fb7-337">工作 2-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="65fb7-337">Task 2 – Running the Solution</span></span>

<span data-ttu-id="65fb7-338">在您將執行此工作中使用新的使用者方案介面您建置使用 AngularJS 來回答的一些測驗問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-338">In this task you will execute the solution using the new user interface you built with AngularJS to answer some of the quiz questions.</span></span>

1. <span data-ttu-id="65fb7-339">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-339">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="65fb7-340">註冊新的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="65fb7-340">Register a new user account.</span></span> <span data-ttu-id="65fb7-341">若要這樣做，請依照下列練習 1 中，工作 3 中所述的註冊步驟。</span><span class="sxs-lookup"><span data-stu-id="65fb7-341">To do this, follow the registration steps described in Exercise 1, Task 3.</span></span>

    > [!NOTE]
    > <span data-ttu-id="65fb7-342">如果您使用的解決方案，從上一個練習，您可以登入您在之前建立的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="65fb7-342">If you are using the solution from the previous exercise, you can log in with the user account you created before.</span></span>
3. <span data-ttu-id="65fb7-343">**首頁**頁面應該會出現，顯示測驗的第一個問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-343">The **Home** page should appear, showing the first question of the quiz.</span></span> <span data-ttu-id="65fb7-344">按一下其中一個選項，以回答問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-344">Answer the question by clicking one of the options.</span></span> <span data-ttu-id="65fb7-345">這會觸發**sendAnswer**稍早定義函式會將傳送至選取的選項**邏輯**Web API。</span><span class="sxs-lookup"><span data-stu-id="65fb7-345">This will trigger the **sendAnswer** function defined earlier, which sends the selected option to the **Trivia** Web API.</span></span>

    <span data-ttu-id="65fb7-346">![回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "回答的問題")</span><span class="sxs-lookup"><span data-stu-id="65fb7-346">![Answering a question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Answering a question")</span></span>

    <span data-ttu-id="65fb7-347">*回答的問題*</span><span class="sxs-lookup"><span data-stu-id="65fb7-347">*Answering a question*</span></span>
4. <span data-ttu-id="65fb7-348">按一下其中一個按鈕之後, 應該會出現答案。</span><span class="sxs-lookup"><span data-stu-id="65fb7-348">After clicking one of the buttons, the answer should appear.</span></span> <span data-ttu-id="65fb7-349">按一下 **下一個問題**以顯示下列問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-349">Click **Next Question** to show the following question.</span></span> <span data-ttu-id="65fb7-350">這會觸發**nextQuestion**此控制器中定義的函式。</span><span class="sxs-lookup"><span data-stu-id="65fb7-350">This will trigger the **nextQuestion** function defined in the controller.</span></span>

    <span data-ttu-id="65fb7-351">![要求的下一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "要求下一個問題")</span><span class="sxs-lookup"><span data-stu-id="65fb7-351">![Requesting the next question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Requesting the next question")</span></span>

    <span data-ttu-id="65fb7-352">*要求的下一個問題*</span><span class="sxs-lookup"><span data-stu-id="65fb7-352">*Requesting the next question*</span></span>
5. <span data-ttu-id="65fb7-353">下一個問題應該會出現。</span><span class="sxs-lookup"><span data-stu-id="65fb7-353">The next question should appear.</span></span> <span data-ttu-id="65fb7-354">繼續執行視需要多次回答的問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-354">Continue answering questions as many times as you want.</span></span> <span data-ttu-id="65fb7-355">完成所有問題之後，您應該傳回第一個問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-355">After completing all the questions you should return to the first question.</span></span>

    <span data-ttu-id="65fb7-356">![另一個問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "另一個問題")</span><span class="sxs-lookup"><span data-stu-id="65fb7-356">![Another question](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Another question")</span></span>

    <span data-ttu-id="65fb7-357">*下一個問題*</span><span class="sxs-lookup"><span data-stu-id="65fb7-357">*Next question*</span></span>
6. <span data-ttu-id="65fb7-358">請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="65fb7-358">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a><span data-ttu-id="65fb7-359">工作 3-建立翻頁動畫的動畫使用 CSS3</span><span class="sxs-lookup"><span data-stu-id="65fb7-359">Task 3 – Creating a Flip Animation Using CSS3</span></span>

<span data-ttu-id="65fb7-360">在這項工作中，您會使用 CSS3 屬性來加入翻頁動畫的效果，以及擷取下一個問題時在解答的問題，以執行豐富的動畫。</span><span class="sxs-lookup"><span data-stu-id="65fb7-360">In this task you will use CSS3 properties to perform rich animations by adding a flip effect when a question is answered and when the next question is retrieved.</span></span>

1. <span data-ttu-id="65fb7-361">在 [**方案總管] 中**，以滑鼠右鍵按一下**內容**資料夾**GeekQuiz**專案，然後選取**新增 |現有的項目...**.</span><span class="sxs-lookup"><span data-stu-id="65fb7-361">In **Solution Explorer**, right-click the **Content** folder of the **GeekQuiz** project and select **Add | Existing Item...**.</span></span>

    <span data-ttu-id="65fb7-362">![將現有的項目新增至內容的資料夾](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Content 資料夾加入現有項目")</span><span class="sxs-lookup"><span data-stu-id="65fb7-362">![Adding an existing item to the Content folder](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Adding an existing item to the Content folder")</span></span>

    <span data-ttu-id="65fb7-363">*將現有的項目新增至 [Content] 資料夾*</span><span class="sxs-lookup"><span data-stu-id="65fb7-363">*Adding an existing item to the Content folder*</span></span>
2. <span data-ttu-id="65fb7-364">在 **加入現有項目**對話方塊方塊中，瀏覽至**來源/資產**資料夾，然後選取**Flip.css**。</span><span class="sxs-lookup"><span data-stu-id="65fb7-364">In the **Add Existing Item** dialog box, navigate to the **Source/Assets** folder and select **Flip.css**.</span></span> <span data-ttu-id="65fb7-365">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="65fb7-365">Click **Add**.</span></span>

    <span data-ttu-id="65fb7-366">![從資產新增 Flip.css 檔案](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "從資產新增 Flip.css 檔案")</span><span class="sxs-lookup"><span data-stu-id="65fb7-366">![Adding the Flip.css file from Assets](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Adding the Flip.css file from Assets")</span></span>

    <span data-ttu-id="65fb7-367">*從資產新增 Flip.css 檔案*</span><span class="sxs-lookup"><span data-stu-id="65fb7-367">*Adding the Flip.css file from Assets*</span></span>
3. <span data-ttu-id="65fb7-368">開啟**Flip.css**您剛才新增的檔案，並檢查其內容。</span><span class="sxs-lookup"><span data-stu-id="65fb7-368">Open the **Flip.css** file you just added and inspect its content.</span></span>
4. <span data-ttu-id="65fb7-369">找出**翻轉轉換**註解。</span><span class="sxs-lookup"><span data-stu-id="65fb7-369">Locate the **flip transformation** comment.</span></span> <span data-ttu-id="65fb7-370">下方的註解樣式使用 CSS**觀點來看**並**rotateY**轉換，以產生&quot;卡片 flip&quot;效果。</span><span class="sxs-lookup"><span data-stu-id="65fb7-370">The styles below that comment use the CSS **perspective** and **rotateY** transformations to generate a &quot;card flip&quot; effect.</span></span>

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. <span data-ttu-id="65fb7-371">找出**flip 期間隱藏窗格背面**註解。</span><span class="sxs-lookup"><span data-stu-id="65fb7-371">Locate the **hide back of pane during flip** comment.</span></span> <span data-ttu-id="65fb7-372">當面臨遠離檢視器藉由設定時，下方的註解的樣式會隱藏臉部在後端**背面可視性**CSS 屬性*隱藏*。</span><span class="sxs-lookup"><span data-stu-id="65fb7-372">The style below that comment hides the back-side of the faces when they are facing away from the viewer by setting the **backface-visibility** CSS property to *hidden*.</span></span>

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. <span data-ttu-id="65fb7-373">開啟**BundleConfig.cs**檔案內**應用程式\_開始**資料夾並將參考加入**Flip.css**檔案**&quot;~/Content/css&quot;** 樣式套件組合</span><span class="sxs-lookup"><span data-stu-id="65fb7-373">Open the **BundleConfig.cs** file inside the **App\_Start** folder and add the reference to the **Flip.css** file in the **&quot;~/Content/css&quot;** style bundle</span></span>

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. <span data-ttu-id="65fb7-374">按下**F5**執行方案，然後登入您的認證。</span><span class="sxs-lookup"><span data-stu-id="65fb7-374">Press **F5** to run the solution and log in with your credentials.</span></span>
8. <span data-ttu-id="65fb7-375">按一下其中一個選項回答的問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-375">Answer a question by clicking one of the options.</span></span> <span data-ttu-id="65fb7-376">檢視之間轉換時，請注意翻頁動畫的效果。</span><span class="sxs-lookup"><span data-stu-id="65fb7-376">Notice the flip effect when transitioning between views.</span></span>

    <span data-ttu-id="65fb7-377">![翻頁動畫效果的回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "回答翻頁動畫的效果")</span><span class="sxs-lookup"><span data-stu-id="65fb7-377">![Answering a question with the flip effect](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Answering a question with the flip effect")</span></span>

    <span data-ttu-id="65fb7-378">*回答翻頁動畫的效果*</span><span class="sxs-lookup"><span data-stu-id="65fb7-378">*Answering a question with the flip effect*</span></span>
9. <span data-ttu-id="65fb7-379">按一下 **下一個問題**擷取下列問題。</span><span class="sxs-lookup"><span data-stu-id="65fb7-379">Click **Next Question** to retrieve the following question.</span></span> <span data-ttu-id="65fb7-380">翻頁動畫的效果應會再次出現。</span><span class="sxs-lookup"><span data-stu-id="65fb7-380">The flip effect should appear again.</span></span>

    <span data-ttu-id="65fb7-381">![正在擷取下列翻頁動畫效果的問題](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "擷取下列翻頁動畫效果的問題")</span><span class="sxs-lookup"><span data-stu-id="65fb7-381">![Retrieving the following question with the flip effect](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Retrieving the following question with the flip effect")</span></span>

    <span data-ttu-id="65fb7-382">*正在擷取下列翻頁動畫效果的問題*</span><span class="sxs-lookup"><span data-stu-id="65fb7-382">*Retrieving the following question with the flip effect*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="65fb7-383">總結</span><span class="sxs-lookup"><span data-stu-id="65fb7-383">Summary</span></span>

<span data-ttu-id="65fb7-384">藉由完成這個實際操作實驗室，您已經學會如何：</span><span class="sxs-lookup"><span data-stu-id="65fb7-384">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="65fb7-385">建立 ASP.NET Web API 控制器使用的 ASP.NET Scaffold</span><span class="sxs-lookup"><span data-stu-id="65fb7-385">Create an ASP.NET Web API controller using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="65fb7-386">實作 Web API 取得動作來擷取下一個測驗問題</span><span class="sxs-lookup"><span data-stu-id="65fb7-386">Implement a Web API Get action to retrieve the next quiz question</span></span>
- <span data-ttu-id="65fb7-387">實作 Web API 後動作來儲存測驗的答案</span><span class="sxs-lookup"><span data-stu-id="65fb7-387">Implement a Web API Post action to store the quiz answers</span></span>
- <span data-ttu-id="65fb7-388">從 Visual Studio 套件管理員主控台安裝 AngularJS</span><span class="sxs-lookup"><span data-stu-id="65fb7-388">Install AngularJS from the Visual Studio Package Manager Console</span></span>
- <span data-ttu-id="65fb7-389">實作的 AngularJS 範本及控制器</span><span class="sxs-lookup"><span data-stu-id="65fb7-389">Implement AngularJS templates and controllers</span></span>
- <span data-ttu-id="65fb7-390">使用 CSS3 轉換執行動畫效果</span><span class="sxs-lookup"><span data-stu-id="65fb7-390">Use CSS3 transitions to perform animation effects</span></span>
