---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 實習實驗室： 即時 Web 應用程式與 SignalR |Microsoft Docs
author: rick-anderson
description: 即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 適用於 ASP.NET 開發人員，ASP...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a3f6174049ffddae4bb2a1819e3684bcdec1b55f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831828"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a><span data-ttu-id="476a8-104">實習實驗室： 即時 Web 應用程式與 SignalR</span><span class="sxs-lookup"><span data-stu-id="476a8-104">Hands On Lab: Real-Time Web Applications with SignalR</span></span>
====================
<span data-ttu-id="476a8-105">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="476a8-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="476a8-106">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="476a8-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="476a8-107">即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。</span><span class="sxs-lookup"><span data-stu-id="476a8-107">Real-time Web applications feature the ability to push server-side content to the connected clients as it happens, in real-time.</span></span> <span data-ttu-id="476a8-108">ASP.NET 開發人員，如**ASP.NET SignalR**是將即時 web 功能新增至他們的應用程式的程式庫。</span><span class="sxs-lookup"><span data-stu-id="476a8-108">For ASP.NET developers, **ASP.NET SignalR** is a library to add real-time web functionality to their applications.</span></span> <span data-ttu-id="476a8-109">它會利用數個傳輸，會自動選取最佳可用的傳輸用戶端和伺服器的最佳可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="476a8-109">It takes advantage of several transports, automatically selecting the best available transport given the client and server's best available transport.</span></span> <span data-ttu-id="476a8-110">它會善用**WebSocket**，HTML5 API，可讓瀏覽器與伺服器之間的雙向通訊。</span><span class="sxs-lookup"><span data-stu-id="476a8-110">It takes advantage of **WebSocket**, an HTML5 API that enables bi-directional communication between the browser and server.</span></span>
> 
> <span data-ttu-id="476a8-111">**SignalR**也提供簡單、 高階 API，以進行用戶端 RPC 的伺服器 （在用戶端的瀏覽器，從伺服器端.NET 程式碼中呼叫 JavaScript 函式） 在您的 ASP.NET 應用程式，以及新增實用的攔截程序進行連線管理例如連接/中斷連接事件、 分組連線和授權。</span><span class="sxs-lookup"><span data-stu-id="476a8-111">**SignalR** also provides a simple, high-level API for doing server to client RPC (call JavaScript functions in your clients' browsers from server-side .NET code) in your ASP.NET application, as well as adding useful hooks for connection management, such as connect/disconnect events, grouping connections, and authorization.</span></span>
> 
> <span data-ttu-id="476a8-112">**SignalR**是一些需要執行用戶端與伺服器之間的即時工作傳輸的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="476a8-112">**SignalR** is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="476a8-113">A **SignalR**連接一開始為 HTTP，並再升級至**WebSocket**連接，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="476a8-113">A **SignalR** connection starts as HTTP, and is then promoted to a **WebSocket** connection if available.</span></span> <span data-ttu-id="476a8-114">**WebSocket**是的理想傳輸**SignalR**，因為這樣會讓伺服器記憶體最有效率地使用具有最低的延遲，而且具有最基礎的功能 (例如完整的雙工用戶端之間的通訊和伺服器），但它也會有最嚴苛的需求： **WebSocket**需要使用伺服器**Windows Server 2012**或**Windows 8**，以及 **.NET framework 4.5**。</span><span class="sxs-lookup"><span data-stu-id="476a8-114">**WebSocket** is the ideal transport for **SignalR**, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: **WebSocket** requires the server to be using **Windows Server 2012** or **Windows 8**, along with **.NET Framework 4.5**.</span></span> <span data-ttu-id="476a8-115">如果不符合這些需求， **SignalR**會嘗試使用其他傳輸進行其連線 (例如*Ajax 長時間輪詢*)。</span><span class="sxs-lookup"><span data-stu-id="476a8-115">If these requirements are not met, **SignalR** will attempt to use other transports to make its connections (like *Ajax long polling*).</span></span>
> 
> <span data-ttu-id="476a8-116">**SignalR** API 包含用戶端和伺服器之間進行通訊的兩個模型：**持續連線**並**中樞**。</span><span class="sxs-lookup"><span data-stu-id="476a8-116">The **SignalR** API contains two models for communicating between clients and servers: **Persistent Connections** and **Hubs**.</span></span> <span data-ttu-id="476a8-117">A**連線**代表簡單的端點用來傳送單一位收件者，分組，或廣播訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-117">A **Connection** represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="476a8-118">A**中樞**連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法為基礎建置更高層級的管線。</span><span class="sxs-lookup"><span data-stu-id="476a8-118">A **Hub** is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span>
> 
> ![SignalR 架構](real-time-web-applications-with-signalr/_static/image1.png)
> 
> <span data-ttu-id="476a8-120">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="476a8-120">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="476a8-121">總覽</span><span class="sxs-lookup"><span data-stu-id="476a8-121">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="476a8-122">目標</span><span class="sxs-lookup"><span data-stu-id="476a8-122">Objectives</span></span>

<span data-ttu-id="476a8-123">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="476a8-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="476a8-124">若要使用 SignalR 用戶端，從伺服器傳送通知。</span><span class="sxs-lookup"><span data-stu-id="476a8-124">Send notifications from server to client using SignalR.</span></span>
- <span data-ttu-id="476a8-125">向外延展您 SignalR 應用程式使用**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="476a8-125">Scale Out your SignalR application using **SQL Server**.</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="476a8-126">必要條件</span><span class="sxs-lookup"><span data-stu-id="476a8-126">Prerequisites</span></span>

<span data-ttu-id="476a8-127">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="476a8-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="476a8-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="476a8-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="476a8-129">安裝程式</span><span class="sxs-lookup"><span data-stu-id="476a8-129">Setup</span></span>

<span data-ttu-id="476a8-130">若要在這個實際操作實驗室中執行的練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="476a8-130">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="476a8-131">開啟 Windows 檔案總管視窗並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="476a8-131">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="476a8-132">以滑鼠右鍵按一下**Setup.cmd** ，然後選取**系統管理員身分執行**來啟動安裝程序，將會設定您的環境並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="476a8-132">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="476a8-133">如果會顯示 [使用者帳戶控制] 對話方塊中，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="476a8-133">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="476a8-134">請確定您執行安裝程式之前檢查這個實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="476a8-134">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="476a8-135">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="476a8-135">Using the Code Snippets</span></span>

<span data-ttu-id="476a8-136">整個實驗室文件中，您將會指示要插入程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="476a8-136">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="476a8-137">為了方便起見，大部分的這段程式碼提供 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，若要避免以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="476a8-137">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="476a8-138">每個練習會伴隨起始方案，位於**開始**練習，可讓您依照每個練習，獨立於其他的資料夾。</span><span class="sxs-lookup"><span data-stu-id="476a8-138">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="476a8-139">請留意練習期間新增的程式碼片段缺少這些啟動解決方案，並可能無法運作，直到您已完成練習。</span><span class="sxs-lookup"><span data-stu-id="476a8-139">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="476a8-140">在練習的原始程式碼，您也可以找到**結束**資料夾包含 Visual Studio 方案，以程式碼所產生的相對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="476a8-140">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="476a8-141">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案與指引。</span><span class="sxs-lookup"><span data-stu-id="476a8-141">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="476a8-142">練習</span><span class="sxs-lookup"><span data-stu-id="476a8-142">Exercises</span></span>

<span data-ttu-id="476a8-143">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="476a8-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="476a8-144">有關使用 SignalR 即時資料</span><span class="sxs-lookup"><span data-stu-id="476a8-144">Working with Real-Time Data Using SignalR</span></span>](#Exercise1)
2. [<span data-ttu-id="476a8-145">向外延展使用 SQL Server</span><span class="sxs-lookup"><span data-stu-id="476a8-145">Scaling Out Using SQL Server</span></span>](#Exercise2)

<span data-ttu-id="476a8-146">估計的時間才能完成這個實驗室： **60 分鐘**</span><span class="sxs-lookup"><span data-stu-id="476a8-146">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="476a8-147">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="476a8-147">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="476a8-148">每個預先定義的集合以符合特定的開發樣式設計，並決定視窗版面配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="476a8-148">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="476a8-149">在這個實驗室中的程序說明完成指定的工作，在 Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="476a8-149">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="476a8-150">如果您選擇不同的設定集合，您的開發環境，可能會有在步驟中，您應該考慮到的差異。</span><span class="sxs-lookup"><span data-stu-id="476a8-150">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a><span data-ttu-id="476a8-151">練習 1： 使用使用 SignalR 的即時資料</span><span class="sxs-lookup"><span data-stu-id="476a8-151">Exercise 1: Working with Real-Time Data Using SignalR</span></span>

<span data-ttu-id="476a8-152">交談通常是用做為範例，您可以執行整個即時 Web 功能更大。</span><span class="sxs-lookup"><span data-stu-id="476a8-152">While chat is often used as an example, you can do a whole lot more with real-time Web functionality.</span></span> <span data-ttu-id="476a8-153">每當使用者重新整理網頁，以查看新的資料或此頁面會實作 Ajax 長時間輪詢來擷取新的資料，您可以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="476a8-153">Any time a user refreshes a web page to see new data or the page implements Ajax long polling to retrieve new data, you can use SignalR.</span></span>

<span data-ttu-id="476a8-154">支援 SignalR**伺服器推播**或是**廣播**功能; 它會自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="476a8-154">SignalR supports **server push** or **broadcasting** functionality; it handles connection management automatically.</span></span> <span data-ttu-id="476a8-155">在 用戶端與伺服器通訊的傳統 HTTP 連線，針對每個要求，重新建立連接時，但 SignalR 提供用戶端與伺服器之間的持續連線。</span><span class="sxs-lookup"><span data-stu-id="476a8-155">In classic HTTP connections for client-server communication, connection is re-established for each request, but SignalR provides persistent connection between the client and server.</span></span> <span data-ttu-id="476a8-156">Signalr 伺服器程式碼呼叫中使用遠端程序呼叫 (RPC) 的瀏覽器用戶端程式碼，而不是要求-回應模型我們知道今天。</span><span class="sxs-lookup"><span data-stu-id="476a8-156">In SignalR the server code calls out to a client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model we know today.</span></span>

<span data-ttu-id="476a8-157">在這個練習中，您會設定**Geek 測驗**使用 SignalR 來顯示更新的計量，而不需要重新整理整個頁面的統計資料儀表板的應用程式。</span><span class="sxs-lookup"><span data-stu-id="476a8-157">In this exercise, you will configure the **Geek Quiz** application to use SignalR to display the Statistics dashboard with the updated metrics without the need to refresh the entire page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a><span data-ttu-id="476a8-158">工作 1 – 瀏覽 Geek 測驗統計資料 頁面</span><span class="sxs-lookup"><span data-stu-id="476a8-158">Task 1 – Exploring the Geek Quiz Statistics Page</span></span>

<span data-ttu-id="476a8-159">在這個工作中，您會瀏覽應用程式，並確認 [統計資料] 頁面的顯示方式及如何改善資訊的方式更新。</span><span class="sxs-lookup"><span data-stu-id="476a8-159">In this task, you will go through the application and verify how the statistics page is shown and how you can improve the way the information is updated.</span></span>

1. <span data-ttu-id="476a8-160">開啟**Visual Studio Express 2013 for Web** ，然後開啟**GeekQuiz.sln**解決方案位於**Source\Ex1 WorkingWithRealTimeData\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="476a8-160">Open **Visual Studio Express 2013 for Web** and open the **GeekQuiz.sln** solution located in the **Source\Ex1-WorkingWithRealTimeData\Begin** folder.</span></span>
2. <span data-ttu-id="476a8-161">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="476a8-161">Press **F5** to run the solution.</span></span> <span data-ttu-id="476a8-162">**登入**頁面應該會出現在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="476a8-162">The **Log in** page should appear in the browser.</span></span>

    <span data-ttu-id="476a8-163">![執行解決方案](real-time-web-applications-with-signalr/_static/image2.png "執行解決方案")</span><span class="sxs-lookup"><span data-stu-id="476a8-163">![Running the solution](real-time-web-applications-with-signalr/_static/image2.png "Running the solution")</span></span>

    <span data-ttu-id="476a8-164">*執行解決方案*</span><span class="sxs-lookup"><span data-stu-id="476a8-164">*Running the solution*</span></span>
3. <span data-ttu-id="476a8-165">按一下 **註冊**應用程式中建立新的使用者頁面右上角。</span><span class="sxs-lookup"><span data-stu-id="476a8-165">Click **Register** in the upper-right corner of the page to create a new user in the application.</span></span>

    <span data-ttu-id="476a8-166">![註冊連結](real-time-web-applications-with-signalr/_static/image3.png "註冊連結")</span><span class="sxs-lookup"><span data-stu-id="476a8-166">![Register link](real-time-web-applications-with-signalr/_static/image3.png "Register link")</span></span>

    <span data-ttu-id="476a8-167">*註冊連結*</span><span class="sxs-lookup"><span data-stu-id="476a8-167">*Register link*</span></span>
4. <span data-ttu-id="476a8-168">在**註冊**頁面上，輸入**使用者名稱**並**密碼**，然後按一下**註冊**。</span><span class="sxs-lookup"><span data-stu-id="476a8-168">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    <span data-ttu-id="476a8-169">![註冊使用者](real-time-web-applications-with-signalr/_static/image4.png "註冊使用者")</span><span class="sxs-lookup"><span data-stu-id="476a8-169">![Registering a user](real-time-web-applications-with-signalr/_static/image4.png "Registering a user")</span></span>

    <span data-ttu-id="476a8-170">*註冊使用者*</span><span class="sxs-lookup"><span data-stu-id="476a8-170">*Registering a user*</span></span>
5. <span data-ttu-id="476a8-171">應用程式註冊新的帳戶，並驗證使用者並將其重新導向回到首頁上顯示的第一個測驗問題。</span><span class="sxs-lookup"><span data-stu-id="476a8-171">The application registers the new account and the user is authenticated and redirected back to the home page showing the first quiz question.</span></span>
6. <span data-ttu-id="476a8-172">開啟**統計資料**頁面上，在新視窗中，並將放**首頁**頁面並**統計資料**頁面上並排顯示。</span><span class="sxs-lookup"><span data-stu-id="476a8-172">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side.</span></span>

    <span data-ttu-id="476a8-173">![並排顯示視窗](real-time-web-applications-with-signalr/_static/image5.png "側邊 windows")</span><span class="sxs-lookup"><span data-stu-id="476a8-173">![Side-by-side windows](real-time-web-applications-with-signalr/_static/image5.png "Side by side windows")</span></span>

    <span data-ttu-id="476a8-174">*並排顯示視窗*</span><span class="sxs-lookup"><span data-stu-id="476a8-174">*Side-by-side windows*</span></span>
7. <span data-ttu-id="476a8-175">在 **首頁**頁面上，按一下其中一個選項，以回答問題。</span><span class="sxs-lookup"><span data-stu-id="476a8-175">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="476a8-176">![回答](real-time-web-applications-with-signalr/_static/image6.png "回答的問題")</span><span class="sxs-lookup"><span data-stu-id="476a8-176">![Answering a question](real-time-web-applications-with-signalr/_static/image6.png "Answering a question")</span></span>

    <span data-ttu-id="476a8-177">*回答的問題*</span><span class="sxs-lookup"><span data-stu-id="476a8-177">*Answering a question*</span></span>
8. <span data-ttu-id="476a8-178">按一下其中一個按鈕之後, 應該會出現答案。</span><span class="sxs-lookup"><span data-stu-id="476a8-178">After clicking one of the buttons, the answer should appear.</span></span>

    <span data-ttu-id="476a8-179">![回答問題，正確](real-time-web-applications-with-signalr/_static/image7.png "解答正確的問題")</span><span class="sxs-lookup"><span data-stu-id="476a8-179">![Question answered correct](real-time-web-applications-with-signalr/_static/image7.png "Question answered correct")</span></span>

    <span data-ttu-id="476a8-180">*正確回答的問題*</span><span class="sxs-lookup"><span data-stu-id="476a8-180">*Question answered correctly*</span></span>
9. <span data-ttu-id="476a8-181">請注意 [統計資料] 頁面中提供的資訊已過期。</span><span class="sxs-lookup"><span data-stu-id="476a8-181">Notice that the information provided in the Statistics page is outdated.</span></span> <span data-ttu-id="476a8-182">重新整理頁面以查看更新的結果。</span><span class="sxs-lookup"><span data-stu-id="476a8-182">Refresh the page in order to see the updated results.</span></span>

    <span data-ttu-id="476a8-183">![統計資料 頁面](real-time-web-applications-with-signalr/_static/image8.png "統計資料 頁面")</span><span class="sxs-lookup"><span data-stu-id="476a8-183">![Statistics page](real-time-web-applications-with-signalr/_static/image8.png "Statistics page")</span></span>

    <span data-ttu-id="476a8-184">*統計資料 頁面*</span><span class="sxs-lookup"><span data-stu-id="476a8-184">*Statistics page*</span></span>
10. <span data-ttu-id="476a8-185">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="476a8-185">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a><span data-ttu-id="476a8-186">工作 2-新增 SignalR 來 Geek 測驗，以顯示線上的圖表</span><span class="sxs-lookup"><span data-stu-id="476a8-186">Task 2 – Adding SignalR to Geek Quiz to Show Online Charts</span></span>

<span data-ttu-id="476a8-187">在這個工作中，您將加入方案中的 SignalR，並將更新傳送至用戶端會自動新增回應傳送至伺服器時。</span><span class="sxs-lookup"><span data-stu-id="476a8-187">In this task, you will add SignalR to the solution and send updates to the clients automatically when a new answer is sent to the server.</span></span>

1. <span data-ttu-id="476a8-188">從**工具**功能表，在 Visual Studio 中，選取**程式庫套件管理員**，然後按一下**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="476a8-188">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="476a8-189">在 [ **Package Manager Console** ] 視窗中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="476a8-189">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="476a8-190">![SignalR 套件安裝](real-time-web-applications-with-signalr/_static/image9.png "SignalR 套件安裝")</span><span class="sxs-lookup"><span data-stu-id="476a8-190">![SignalR package installation](real-time-web-applications-with-signalr/_static/image9.png "SignalR package installation")</span></span>

    <span data-ttu-id="476a8-191">*SignalR 套件安裝*</span><span class="sxs-lookup"><span data-stu-id="476a8-191">*SignalR package installation*</span></span>

   > [!NOTE]
   > <span data-ttu-id="476a8-192">安裝時**SignalR** NuGet 套件版本 2.0.2 從全新的 MVC 5 應用程式，您必須手動更新**OWIN**封裝以版本 2.0.1 （或更新版本） 安裝 SignalR 之前。</span><span class="sxs-lookup"><span data-stu-id="476a8-192">When installing **SignalR** NuGet packages version 2.0.2 from a brand new MVC 5 application, you will need to manually update **OWIN** packages to version 2.0.1 (or higher) before installing SignalR.</span></span> <span data-ttu-id="476a8-193">若要這樣做，您可以執行下列指令碼**Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="476a8-193">To do this, you can execute the following script in the **Package Manager Console**:</span></span>
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > <span data-ttu-id="476a8-194">中的 SignalR 未來版本中，將會自動更新 OWIN 相依性。</span><span class="sxs-lookup"><span data-stu-id="476a8-194">In a future release of SignalR, OWIN dependencies will be automatically updated.</span></span>
3. <span data-ttu-id="476a8-195">在 [**方案總管] 中**，展開**指令碼**資料夾，請注意，SignalR *js*檔案已加入至方案。</span><span class="sxs-lookup"><span data-stu-id="476a8-195">In **Solution Explorer**, expand the **Scripts** folder and notice that the SignalR *js* files were added to the solution.</span></span>

    <span data-ttu-id="476a8-196">![SignalR JavaScript 參考](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 參考")</span><span class="sxs-lookup"><span data-stu-id="476a8-196">![SignalR JavaScript references](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript references")</span></span>

    <span data-ttu-id="476a8-197">*SignalR JavaScript 參考*</span><span class="sxs-lookup"><span data-stu-id="476a8-197">*SignalR JavaScript references*</span></span>
4. <span data-ttu-id="476a8-198">在 [**方案總管] 中**，以滑鼠右鍵按一下**GeekQuiz**專案，然後選取**新增** | **新資料夾**，並將它命名**中樞**。</span><span class="sxs-lookup"><span data-stu-id="476a8-198">In **Solution Explorer**, right-click the **GeekQuiz** project, select **Add** | **New Folder**, and name it **Hubs**.</span></span>
5. <span data-ttu-id="476a8-199">以滑鼠右鍵按一下**集線器**資料夾，然後選取**新增 |新的項目**。</span><span class="sxs-lookup"><span data-stu-id="476a8-199">Right-click the **Hubs** folder and select **Add | New Item**.</span></span>

    <span data-ttu-id="476a8-200">![加入新項目](real-time-web-applications-with-signalr/_static/image11.png "加入新項目")</span><span class="sxs-lookup"><span data-stu-id="476a8-200">![Add new item](real-time-web-applications-with-signalr/_static/image11.png "Add new item")</span></span>

    <span data-ttu-id="476a8-201">*加入新項目*</span><span class="sxs-lookup"><span data-stu-id="476a8-201">*Add new item*</span></span>
6. <span data-ttu-id="476a8-202">在 **加入新項目**對話方塊中，選取**Visual C# |Web |SignalR**的左窗格中，選取的節點**SignalR Hub 類別 (v2)** 從中間窗格中，將檔案命名**StatisticsHub.cs** ，按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="476a8-202">In the **Add New Item** dialog box, select the **Visual C# | Web | SignalR** node in the left pane, select **SignalR Hub Class (v2)** from the center pane, name the file **StatisticsHub.cs** and click **Add**.</span></span>

    <span data-ttu-id="476a8-203">![加入新項目 對話方塊](real-time-web-applications-with-signalr/_static/image12.png "加入新項目對話方塊")</span><span class="sxs-lookup"><span data-stu-id="476a8-203">![Add new item dialog box](real-time-web-applications-with-signalr/_static/image12.png "Add new item dialog box")</span></span>

    <span data-ttu-id="476a8-204">*加入新項目對話方塊*</span><span class="sxs-lookup"><span data-stu-id="476a8-204">*Add new item dialog box*</span></span>
7. <span data-ttu-id="476a8-205">中的程式碼取代**StatisticsHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="476a8-205">Replace the code in the **StatisticsHub** class with the following code.</span></span>

    <span data-ttu-id="476a8-206">(程式碼片段- *RealTimeSignalR-Ex1-StatisticsHubClass*)</span><span class="sxs-lookup"><span data-stu-id="476a8-206">(Code Snippet - *RealTimeSignalR - Ex1 - StatisticsHubClass*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. <span data-ttu-id="476a8-207">開啟**Startup.cs** ，並在結尾新增下面這一行**組態**方法。</span><span class="sxs-lookup"><span data-stu-id="476a8-207">Open **Startup.cs** and add the following line at the end of the **Configuration** method.</span></span>

    <span data-ttu-id="476a8-208">(程式碼片段- *RealTimeSignalR-Ex1-MapSignalR*)</span><span class="sxs-lookup"><span data-stu-id="476a8-208">(Code Snippet - *RealTimeSignalR - Ex1 - MapSignalR*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. <span data-ttu-id="476a8-209">開啟**StatisticsService.cs**頁面內**服務**資料夾，並新增下列 using 指示詞。</span><span class="sxs-lookup"><span data-stu-id="476a8-209">Open the **StatisticsService.cs** page inside the **Services** folder and add the following using directives.</span></span>

    <span data-ttu-id="476a8-210">(程式碼片段- *RealTimeSignalR-Ex1-UsingDirectives*)</span><span class="sxs-lookup"><span data-stu-id="476a8-210">(Code Snippet - *RealTimeSignalR - Ex1 - UsingDirectives*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. <span data-ttu-id="476a8-211">若要通知連線的用戶端的更新，您先擷取**內容**目前連接的物件。</span><span class="sxs-lookup"><span data-stu-id="476a8-211">To notify connected clients of updates, you first retrieve a **Context** object for the current connection.</span></span> <span data-ttu-id="476a8-212">**中樞**物件包含方法，將訊息傳送至單一用戶端或廣播到所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="476a8-212">The **Hub** object contains methods to send messages to a single client or broadcast to all connected clients.</span></span> <span data-ttu-id="476a8-213">將下列方法加入**StatisticsService**廣播統計資料的類別。</span><span class="sxs-lookup"><span data-stu-id="476a8-213">Add the following method to the **StatisticsService** class to broadcast the statistics data.</span></span>

    <span data-ttu-id="476a8-214">(程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)</span><span class="sxs-lookup"><span data-stu-id="476a8-214">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="476a8-215">上面的程式碼，您會使用任意的方法名稱在用戶端上呼叫的函式 (也就是： *updateStatistics*)。</span><span class="sxs-lookup"><span data-stu-id="476a8-215">In the code above, you are using an arbitrary method name to call a function on the client (i.e.: *updateStatistics*).</span></span> <span data-ttu-id="476a8-216">您指定的方法名稱會解譯為動態物件，這表示沒有任何 IntelliSense 或它的編譯時間驗證。</span><span class="sxs-lookup"><span data-stu-id="476a8-216">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="476a8-217">在執行階段會評估運算式。</span><span class="sxs-lookup"><span data-stu-id="476a8-217">The expression is evaluated at run time.</span></span> <span data-ttu-id="476a8-218">當方法呼叫執行時，SignalR 就會傳送給用戶端的方法名稱和參數值。</span><span class="sxs-lookup"><span data-stu-id="476a8-218">When the method call executes, SignalR sends the method name and the parameter values to the client.</span></span> <span data-ttu-id="476a8-219">如果用戶端有符合的名稱，就會呼叫方法的參數值傳遞給它的方法。</span><span class="sxs-lookup"><span data-stu-id="476a8-219">If the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="476a8-220">如果用戶端上找不到任何相符的方法，不會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="476a8-220">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="476a8-221">如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南](../guide-to-the-api/hubs-api-guide-server.md)。</span><span class="sxs-lookup"><span data-stu-id="476a8-221">For more information, refer to [ASP.NET SignalR Hubs API Guide](../guide-to-the-api/hubs-api-guide-server.md).</span></span>
11. <span data-ttu-id="476a8-222">開啟**TriviaController.cs**頁面內**控制站**資料夾，並新增下列 using 指示詞。</span><span class="sxs-lookup"><span data-stu-id="476a8-222">Open the **TriviaController.cs** page inside the **Controllers** folder and add the following using directives.</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. <span data-ttu-id="476a8-223">將下列反白顯示的程式碼，加入**Post**動作方法。</span><span class="sxs-lookup"><span data-stu-id="476a8-223">Add the following highlighted code to the **Post** action method.</span></span>

    <span data-ttu-id="476a8-224">(程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)</span><span class="sxs-lookup"><span data-stu-id="476a8-224">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. <span data-ttu-id="476a8-225">開啟**Statistics.cshtml**頁面內**檢視 |首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="476a8-225">Open the **Statistics.cshtml** page inside the **Views | Home** folder.</span></span> <span data-ttu-id="476a8-226">找出**指令碼**區段，並在區段的開頭新增下列指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="476a8-226">Locate the **Scripts** section and add the following script references at the beginning of the section.</span></span>

    <span data-ttu-id="476a8-227">(程式碼片段- *RealTimeSignalR-Ex1-SignalRScriptReferences*)</span><span class="sxs-lookup"><span data-stu-id="476a8-227">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="476a8-228">當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，套件管理員可能會安裝的版本比本主題中所顯示的版本還新的 SignalR 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="476a8-228">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="476a8-229">請確定您的程式碼中的指令碼參考符合安裝在您專案的指令碼程式庫的版本。</span><span class="sxs-lookup"><span data-stu-id="476a8-229">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>
14. <span data-ttu-id="476a8-230">新增下列醒目提示的程式碼，以連線到 SignalR 中樞的用戶端，並從中樞收到新訊息時，更新統計資料。</span><span class="sxs-lookup"><span data-stu-id="476a8-230">Add the following highlighted code to connect the client to the SignalR hub and update the statistics data when a new message is received from the hub.</span></span>

    <span data-ttu-id="476a8-231">(程式碼片段- *RealTimeSignalR-Ex1-SignalRClientCode*)</span><span class="sxs-lookup"><span data-stu-id="476a8-231">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRClientCode*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    <span data-ttu-id="476a8-232">在此程式碼中，您建立中樞 Proxy 和註冊事件處理常式來接聽的伺服器所傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-232">In this code, you are creating a Hub Proxy and registering an event handler to listen for messages sent by the server.</span></span> <span data-ttu-id="476a8-233">您在此情況下，接聽訊息透過傳送*updateStatistics*方法。</span><span class="sxs-lookup"><span data-stu-id="476a8-233">In this case, you listen for messages sent through the *updateStatistics* method.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="476a8-234">工作 3-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="476a8-234">Task 3 – Running the Solution</span></span>

<span data-ttu-id="476a8-235">在這個工作中，您將執行解決方案，以驗證使用 SignalR 接聽新的問題之後自動更新統計資料檢視。</span><span class="sxs-lookup"><span data-stu-id="476a8-235">In this task, you will run the solution to verify that the statistics view is updated automatically using SignalR after answering a new question.</span></span>

1. <span data-ttu-id="476a8-236">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="476a8-236">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="476a8-237">如果您尚未登入應用程式，進行登入您在工作 1 中建立的使用者。</span><span class="sxs-lookup"><span data-stu-id="476a8-237">If not already logged in to the application, log in with the user you created in Task 1.</span></span>
2. <span data-ttu-id="476a8-238">開啟**統計資料**頁面上，在新視窗中，並將放**首頁**頁面並**統計資料**頁面上並排顯示，如同在工作 1。</span><span class="sxs-lookup"><span data-stu-id="476a8-238">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side as you did in Task 1.</span></span>
3. <span data-ttu-id="476a8-239">在 **首頁**頁面上，按一下其中一個選項，以回答問題。</span><span class="sxs-lookup"><span data-stu-id="476a8-239">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="476a8-240">![另一個問題的回答](real-time-web-applications-with-signalr/_static/image13.png "回答其他問題")</span><span class="sxs-lookup"><span data-stu-id="476a8-240">![Answering another question](real-time-web-applications-with-signalr/_static/image13.png "Answering another question")</span></span>

    <span data-ttu-id="476a8-241">*回答其他問題*</span><span class="sxs-lookup"><span data-stu-id="476a8-241">*Answering another question*</span></span>
4. <span data-ttu-id="476a8-242">按一下其中一個按鈕之後, 應該會出現答案。</span><span class="sxs-lookup"><span data-stu-id="476a8-242">After clicking one of the buttons, the answer should appear.</span></span> <span data-ttu-id="476a8-243">請注意頁面上的統計資料資訊會自動更新後回答的問題，以更新的資訊，而不需要重新整理整個頁面。</span><span class="sxs-lookup"><span data-stu-id="476a8-243">Notice that the Statistics information on the page is updated automatically after answering the question with the updated information without the need to refresh the entire page.</span></span>

    <span data-ttu-id="476a8-244">![回應後的統計資料 頁面重新整理](real-time-web-applications-with-signalr/_static/image14.png "回應之後重新整理統計資料頁面")</span><span class="sxs-lookup"><span data-stu-id="476a8-244">![Statistics page refreshed after answer](real-time-web-applications-with-signalr/_static/image14.png "Statistics page refreshed after answer")</span></span>

    <span data-ttu-id="476a8-245">*統計資料重新整理之後回應 頁面*</span><span class="sxs-lookup"><span data-stu-id="476a8-245">*Statistics page refreshed after answer*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a><span data-ttu-id="476a8-246">練習 2： 向外延展使用 SQL Server</span><span class="sxs-lookup"><span data-stu-id="476a8-246">Exercise 2: Scaling Out Using SQL Server</span></span>

<span data-ttu-id="476a8-247">當調整 web 應用程式，您通常可以選擇之間*向上*並*相應放大*選項。</span><span class="sxs-lookup"><span data-stu-id="476a8-247">When scaling a web application, you can generally choose between *scaling up* and *scaling out* options.</span></span> <span data-ttu-id="476a8-248">*相應增加*表示使用更大的伺服器，使用更多資源 （CPU、 RAM 等），同時*相應放大*表示新增更多伺服器來處理負載。</span><span class="sxs-lookup"><span data-stu-id="476a8-248">*Scale up* means using a larger server, with more resources (CPU, RAM, etc.) while *scale out* means adding more servers to handle the load.</span></span> <span data-ttu-id="476a8-249">後者的問題在於用戶端可以取得路由傳送至不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="476a8-249">The problem with the latter is that the clients can get routed to different servers.</span></span> <span data-ttu-id="476a8-250">連線到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-250">A client that is connected to one server will not receive messages sent from another server.</span></span>

<span data-ttu-id="476a8-251">您可以使用稱為的元件，以解決這些問題*後擋板*、 轉送給伺服器之間的訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-251">You can solve these issues by using a component called *backplane*, to forward messages between servers.</span></span> <span data-ttu-id="476a8-252">後擋板，已啟用，與每個應用程式執行個體傳送訊息至後擋板，並後擋板將它們轉送到其他應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-252">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span>

<span data-ttu-id="476a8-253">目前有三種類型的 SignalR 的背板：</span><span class="sxs-lookup"><span data-stu-id="476a8-253">There are currently three types of backplanes for SignalR:</span></span>

- <span data-ttu-id="476a8-254">**Windows Azure 服務匯流排**。</span><span class="sxs-lookup"><span data-stu-id="476a8-254">**Windows Azure Service Bus**.</span></span> <span data-ttu-id="476a8-255">服務匯流排是可讓元件，以將鬆散結合的訊息傳送的訊息基礎結構。</span><span class="sxs-lookup"><span data-stu-id="476a8-255">Service Bus is a messaging infrastructure that allows components to send loosely coupled messages.</span></span>
- <span data-ttu-id="476a8-256">**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="476a8-256">**SQL Server**.</span></span> <span data-ttu-id="476a8-257">SQL Server 後擋板會將訊息寫入 SQL 資料表。</span><span class="sxs-lookup"><span data-stu-id="476a8-257">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="476a8-258">後擋板會使用 Service Broker 有效率之傳訊。</span><span class="sxs-lookup"><span data-stu-id="476a8-258">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="476a8-259">不過，它也適用於如果未啟用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="476a8-259">However, it also works if Service Broker is not enabled.</span></span>
- <span data-ttu-id="476a8-260">**Redis**。</span><span class="sxs-lookup"><span data-stu-id="476a8-260">**Redis**.</span></span> <span data-ttu-id="476a8-261">Redis 是記憶體中索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="476a8-261">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="476a8-262">Redis 支援發行/訂閱 ("pub/sub") 的模式來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-262">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>

<span data-ttu-id="476a8-263">每個訊息會透過訊息匯流排傳送。</span><span class="sxs-lookup"><span data-stu-id="476a8-263">Every message is sent through a message bus.</span></span> <span data-ttu-id="476a8-264">訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象概念。</span><span class="sxs-lookup"><span data-stu-id="476a8-264">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="476a8-265">藉由取代預設的背板運作**IMessageBus**與專為該後擋板匯流排。</span><span class="sxs-lookup"><span data-stu-id="476a8-265">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span>

<span data-ttu-id="476a8-266">每個伺服器執行個體連線到透過匯流排後擋板。</span><span class="sxs-lookup"><span data-stu-id="476a8-266">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="476a8-267">當訊息傳送時，會先移至後擋板，並後擋板將它傳送至每一部伺服器。</span><span class="sxs-lookup"><span data-stu-id="476a8-267">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="476a8-268">當伺服器收到一則訊息，從後擋板時，它會在其本機快取中儲存訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-268">When a server receives a message from the backplane, it stores the message in its local cache.</span></span> <span data-ttu-id="476a8-269">伺服器再將訊息傳遞至用戶端從其本機快取。</span><span class="sxs-lookup"><span data-stu-id="476a8-269">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="476a8-270">如需有關 SignalR 後擋板的運作方式，請閱讀本[文章](../performance/scaleout-in-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="476a8-270">For more information about how the SignalR backplane works, read this [article](../performance/scaleout-in-signalr.md).</span></span>

> [!NOTE]
> <span data-ttu-id="476a8-271">有一些案例，其中的後擋板可能成為瓶頸。</span><span class="sxs-lookup"><span data-stu-id="476a8-271">There are some scenarios where a backplane can become a bottleneck.</span></span> <span data-ttu-id="476a8-272">以下是一些典型的 SignalR 案例：</span><span class="sxs-lookup"><span data-stu-id="476a8-272">Here are some typical SignalR scenarios:</span></span>
> 
> - <span data-ttu-id="476a8-273">[伺服器廣播](tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）： 背板適用於此案例中，因為伺服器控制傳送訊息的速率。</span><span class="sxs-lookup"><span data-stu-id="476a8-273">[Server broadcast](tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
> - <span data-ttu-id="476a8-274">[用戶端到用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板瓶頸的訊息數目隨著用戶端數目; 也就是說，如果訊息的速率成長按比例越多的用戶端加入。</span><span class="sxs-lookup"><span data-stu-id="476a8-274">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
> - <span data-ttu-id="476a8-275">[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 這種情況下不建議後擋板。</span><span class="sxs-lookup"><span data-stu-id="476a8-275">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>


<span data-ttu-id="476a8-276">在這個練習中，您將使用**SQL Server**散發郵件，跨越**Geek 測驗**應用程式。</span><span class="sxs-lookup"><span data-stu-id="476a8-276">In this exercise, you will use **SQL Server** to distribute messages across the **Geek Quiz** application.</span></span> <span data-ttu-id="476a8-277">您將這些工作的電腦上執行單一測試以了解如何設定組態，但是為了取得完整的效果，您必須部署兩個或多部伺服器的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="476a8-277">You will run these tasks on a single test machine to learn how to set up the configuration, but in order to get the full effect, you will need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="476a8-278">其中一部伺服器，或個別的專用伺服器上，您也必須安裝 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="476a8-278">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span>

![向外延展您可以使用 SQL Server 的圖表](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a><span data-ttu-id="476a8-280">工作 1-了解案例</span><span class="sxs-lookup"><span data-stu-id="476a8-280">Task 1 - Understanding the Scenario</span></span>

<span data-ttu-id="476a8-281">在這個工作中，您將執行 2 個執行個體**Geek 測驗**本機電腦上模擬多個 IIS 執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-281">In this task, you will run 2 instances of **Geek Quiz** simulating multiple IIS instances on your local machine.</span></span> <span data-ttu-id="476a8-282">在此案例中，回答某一個應用程式邏輯問題時，更新將不會收到通知的第二個執行個體的統計資料 頁面上。</span><span class="sxs-lookup"><span data-stu-id="476a8-282">In this scenario, when answering trivia questions on one application, update won't be notified on the statistics page of the second instance.</span></span> <span data-ttu-id="476a8-283">這個模擬類似於在多個執行個體部署您的應用程式所在的環境和使用負載平衡器與他們溝通。</span><span class="sxs-lookup"><span data-stu-id="476a8-283">This simulation resembles an environment where your application is deployed on multiple instances and using a load balancer to communicate with them.</span></span>

1. <span data-ttu-id="476a8-284">開啟**Begin.sln**解決方案位於**來源/Ex2-ScalingOutWithSQLServer/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="476a8-284">Open the **Begin.sln** solution located in the **Source/Ex2-ScalingOutWithSQLServer/Begin** folder.</span></span> <span data-ttu-id="476a8-285">載入之後，您會注意到在**伺服器總管**解決方案有兩個專案，具有相同結構但不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="476a8-285">Once loaded, you will notice on the **Server Explorer** that the solution has two projects with identical structures but different names.</span></span> <span data-ttu-id="476a8-286">這會模擬您的本機電腦上執行相同的應用程式的兩個執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-286">This will simulate running two instances of the same application on your local machine.</span></span>

    <span data-ttu-id="476a8-287">![開始模擬 Geek 測驗的 2 個執行個體的解決方案](real-time-web-applications-with-signalr/_static/image16.png "開始模擬 Geek 測驗的 2 個執行個體的解決方案")</span><span class="sxs-lookup"><span data-stu-id="476a8-287">![Begin Solution Simulating 2 Instances of Geek Quiz](real-time-web-applications-with-signalr/_static/image16.png "Begin Solution Simulating 2 Instances of Geek Quiz")</span></span>

    <span data-ttu-id="476a8-288">*開始模擬 Geek 測驗的 2 個執行個體的解決方案*</span><span class="sxs-lookup"><span data-stu-id="476a8-288">*Begin Solution Simulating 2 Instances of Geek Quiz*</span></span>
2. <span data-ttu-id="476a8-289">開啟方案的 [屬性] 頁面，以滑鼠右鍵按一下方案節點，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="476a8-289">Open the properties page of the solution by right-clicking the solution node and selecting **Properties**.</span></span> <span data-ttu-id="476a8-290">底下**啟始專案**，選取**多個啟始專案**並變更**動作**這兩個專案到值*啟動*。</span><span class="sxs-lookup"><span data-stu-id="476a8-290">Under **Startup Project**, select **Multiple startup projects** and change the **Action** value for both projects to *Start*.</span></span>

    <span data-ttu-id="476a8-291">![啟動多個專案](real-time-web-applications-with-signalr/_static/image17.png "啟動多個專案")</span><span class="sxs-lookup"><span data-stu-id="476a8-291">![Starting Multiple Projects](real-time-web-applications-with-signalr/_static/image17.png "Starting Multiple Projects")</span></span>

    <span data-ttu-id="476a8-292">*啟動多個專案*</span><span class="sxs-lookup"><span data-stu-id="476a8-292">*Starting Multiple Projects*</span></span>
3. <span data-ttu-id="476a8-293">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="476a8-293">Press **F5** to run the solution.</span></span> <span data-ttu-id="476a8-294">應用程式將會啟動兩個執行個體**Geek 測驗**在不同的連接埠，來模擬多個相同的應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-294">The application will launch two instances of **Geek Quiz** in different ports, simulating multiple instances of the same application.</span></span> <span data-ttu-id="476a8-295">在左邊和右邊的螢幕上其他將其中一個瀏覽器釘選。</span><span class="sxs-lookup"><span data-stu-id="476a8-295">Pin one of the browsers on left and the other on the right of your screen.</span></span> <span data-ttu-id="476a8-296">您的認證登入或註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="476a8-296">Log in with your credentials or register a new user.</span></span> <span data-ttu-id="476a8-297">登入之後，保留在左側的邏輯頁面，並移至**統計資料**右邊的瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="476a8-297">Once logged in, keep the Trivia page on the left and go to the **Statistics** page in the browser on the right.</span></span>

    ![玩家測驗並排顯示](real-time-web-applications-with-signalr/_static/image18.png)

    <span data-ttu-id="476a8-299">*玩家測驗並排顯示*</span><span class="sxs-lookup"><span data-stu-id="476a8-299">*Geek Quiz Side by Side*</span></span>

    ![在不同的連接埠的玩家測驗](real-time-web-applications-with-signalr/_static/image19.png)

    <span data-ttu-id="476a8-301">*在不同的連接埠的玩家測驗*</span><span class="sxs-lookup"><span data-stu-id="476a8-301">*Geek Quiz in Different Ports*</span></span>
4. <span data-ttu-id="476a8-302">左側的瀏覽器中啟動的問題，您將會發現**統計資料**未更新的右邊的瀏覽器頁面。</span><span class="sxs-lookup"><span data-stu-id="476a8-302">Start answering questions in the left browser and you will notice that the **Statistics** page in the right browser is not being updated.</span></span> <span data-ttu-id="476a8-303">這是因為**SignalR**使用本機快取將訊息散發到其用戶端和此案例中模擬多個執行個體，因此快取不會共用它們之間。</span><span class="sxs-lookup"><span data-stu-id="476a8-303">This is because **SignalR** uses a local cache to distribute messages across their clients and this scenario is simulating multiple instances, therefore the cache is not shared between them.</span></span> <span data-ttu-id="476a8-304">您可以確認**SignalR**測試相同的步驟，但使用單一的應用程式正常運作。</span><span class="sxs-lookup"><span data-stu-id="476a8-304">You can verify that **SignalR** is working by testing the same steps but using a single app.</span></span> <span data-ttu-id="476a8-305">在下列工作中，您將設定的後擋板以複寫執行個體之間的訊息。</span><span class="sxs-lookup"><span data-stu-id="476a8-305">In the following tasks you will configure a backplane to replicate the messages across instances.</span></span>
5. <span data-ttu-id="476a8-306">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="476a8-306">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a><span data-ttu-id="476a8-307">工作 2-建立 SQL Server 後擋板</span><span class="sxs-lookup"><span data-stu-id="476a8-307">Task 2 – Creating the SQL Server Backplane</span></span>

<span data-ttu-id="476a8-308">在這個工作中，您將建立的資料庫，將做為的後擋板**Geek 測驗**應用程式。</span><span class="sxs-lookup"><span data-stu-id="476a8-308">In this task, you will create a database that will serve as a backplane for the **Geek Quiz** application.</span></span> <span data-ttu-id="476a8-309">您將使用**SQL Server 物件總管**瀏覽您的伺服器，並初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="476a8-309">You will use **SQL Server Object Explorer** to browse your server and initialize the database.</span></span> <span data-ttu-id="476a8-310">此外，您將會啟用**Service Broker**。</span><span class="sxs-lookup"><span data-stu-id="476a8-310">Additionally, you will enable the **Service Broker**.</span></span>

1. <span data-ttu-id="476a8-311">在  **Visual Studio**，開啟功能表**檢視**，然後選取**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="476a8-311">In **Visual Studio**, open menu **View** and select **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="476a8-312">以滑鼠右鍵按一下連接到您的 LocalDB 執行個體**SQL Server**節點，然後選取**加入 SQL Server...** 選項。</span><span class="sxs-lookup"><span data-stu-id="476a8-312">Connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="476a8-313">![加入 SQL Server 執行個體](real-time-web-applications-with-signalr/_static/image20.png "加入 SQL Server 執行個體")</span><span class="sxs-lookup"><span data-stu-id="476a8-313">![Adding a SQL Server Instance](real-time-web-applications-with-signalr/_static/image20.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="476a8-314">*加入 SQL Server 物件總管 中的 SQL Server 執行個體*</span><span class="sxs-lookup"><span data-stu-id="476a8-314">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
3. <span data-ttu-id="476a8-315">設定**伺服器名稱**要 *(localdb) \v11.0* ，並將**Windows 驗證**當做驗證模式。</span><span class="sxs-lookup"><span data-stu-id="476a8-315">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="476a8-316">按一下  **Connect**以繼續。</span><span class="sxs-lookup"><span data-stu-id="476a8-316">Click **Connect** to continue.</span></span>

    <span data-ttu-id="476a8-317">![連接到 LocalDB](real-time-web-applications-with-signalr/_static/image21.png "連接到 LocalDB")</span><span class="sxs-lookup"><span data-stu-id="476a8-317">![Connecting to LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="476a8-318">*連接到 LocalDB*</span><span class="sxs-lookup"><span data-stu-id="476a8-318">*Connecting to LocalDB*</span></span>
4. <span data-ttu-id="476a8-319">現在，您會連接到 LocalDB 執行個體，您必須建立將代表 signalr 的 SQL Server 後擋板的資料庫。</span><span class="sxs-lookup"><span data-stu-id="476a8-319">Now that you are connected to your LocalDB instance, you will need to create a database that will represent the SQL Server backplane for SignalR.</span></span> <span data-ttu-id="476a8-320">若要這樣做，請以滑鼠右鍵按一下**資料庫**節點，然後選取**加入新的資料庫**。</span><span class="sxs-lookup"><span data-stu-id="476a8-320">To do this, right-click the **Databases** node and select **Add New Database**.</span></span>

    <span data-ttu-id="476a8-321">![加入新的資料庫](real-time-web-applications-with-signalr/_static/image22.png "中加入新的資料庫")</span><span class="sxs-lookup"><span data-stu-id="476a8-321">![Adding a new database](real-time-web-applications-with-signalr/_static/image22.png "Adding a new database")</span></span>

    <span data-ttu-id="476a8-322">*加入新的資料庫*</span><span class="sxs-lookup"><span data-stu-id="476a8-322">*Adding a new database*</span></span>
5. <span data-ttu-id="476a8-323">若要設定的資料庫名稱*SignalR* ，按一下  **確定**來建立它。</span><span class="sxs-lookup"><span data-stu-id="476a8-323">Set the database name to *SignalR* and click **OK** to create it.</span></span>

    <span data-ttu-id="476a8-324">![建立 SignalR 資料庫](real-time-web-applications-with-signalr/_static/image23.png "建立 SignalR 資料庫")</span><span class="sxs-lookup"><span data-stu-id="476a8-324">![Creating the SignalR database](real-time-web-applications-with-signalr/_static/image23.png "Creating the SignalR database")</span></span>

    <span data-ttu-id="476a8-325">*建立 SignalR 資料庫*</span><span class="sxs-lookup"><span data-stu-id="476a8-325">*Creating the SignalR database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="476a8-326">您可以選擇任何資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="476a8-326">You can choose any name for the database.</span></span>
6. <span data-ttu-id="476a8-327">若要從後擋板，更有效率地接收更新，建議啟用 Service Broker 資料庫。</span><span class="sxs-lookup"><span data-stu-id="476a8-327">To receive updates more efficiently from the backplane, it is recommended to enable Service Broker for the database.</span></span> <span data-ttu-id="476a8-328">Service Broker 提供傳訊和佇列在 SQL Server 原生支援。</span><span class="sxs-lookup"><span data-stu-id="476a8-328">Service Broker provides native support for messaging and queuing in SQL Server.</span></span> <span data-ttu-id="476a8-329">後擋板也適用於沒有 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="476a8-329">The backplane also works without Service Broker.</span></span> <span data-ttu-id="476a8-330">開啟新的查詢，以滑鼠右鍵按一下資料庫，然後選取**新的查詢**。</span><span class="sxs-lookup"><span data-stu-id="476a8-330">Open a new query by right-clicking the database and select **New Query**.</span></span>

    <span data-ttu-id="476a8-331">![開啟新的查詢](real-time-web-applications-with-signalr/_static/image24.png "開啟新的查詢")</span><span class="sxs-lookup"><span data-stu-id="476a8-331">![Opening a New Query](real-time-web-applications-with-signalr/_static/image24.png "Opening a New Query")</span></span>

    <span data-ttu-id="476a8-332">*開啟新的查詢*</span><span class="sxs-lookup"><span data-stu-id="476a8-332">*Opening a New Query*</span></span>
7. <span data-ttu-id="476a8-333">若要檢查是否啟用 Service Broker 時，請查詢**已\_broker\_啟用**中的資料行**sys.databases**目錄檢視。</span><span class="sxs-lookup"><span data-stu-id="476a8-333">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span> <span data-ttu-id="476a8-334">在最近開啟的查詢視窗中執行下列指令碼。</span><span class="sxs-lookup"><span data-stu-id="476a8-334">Execute the following script in the recently opened query window.</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    <span data-ttu-id="476a8-335">![查詢服務訊息代理程式狀態](real-time-web-applications-with-signalr/_static/image25.png "查詢服務訊息代理程式狀態")</span><span class="sxs-lookup"><span data-stu-id="476a8-335">![Querying the Service Broker Status](real-time-web-applications-with-signalr/_static/image25.png "Querying the Service Broker Status")</span></span>

    <span data-ttu-id="476a8-336">*查詢服務訊息代理程式狀態*</span><span class="sxs-lookup"><span data-stu-id="476a8-336">*Querying the Service Broker Status*</span></span>
8. <span data-ttu-id="476a8-337">如果值**已\_broker\_啟用**資料庫中的資料行是&quot;0&quot;，使用下列命令來啟用它。</span><span class="sxs-lookup"><span data-stu-id="476a8-337">If the value of the **is\_broker\_enabled** column in your database is &quot;0&quot;, use the following command to enable it.</span></span> <span data-ttu-id="476a8-338">取代**&lt;您資料庫&gt;** 建立資料庫時，您設定的名稱 (例如： SignalR)。</span><span class="sxs-lookup"><span data-stu-id="476a8-338">Replace **&lt;YOUR-DATABASE&gt;** with the name you set when creating the database (e.g.: SignalR).</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    <span data-ttu-id="476a8-339">![啟用 Service Broker](real-time-web-applications-with-signalr/_static/image26.png "啟用 Service Broker")</span><span class="sxs-lookup"><span data-stu-id="476a8-339">![Enabling Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Enabling Service Broker")</span></span>

    <span data-ttu-id="476a8-340">*啟用 Service Broker*</span><span class="sxs-lookup"><span data-stu-id="476a8-340">*Enabling Service Broker*</span></span>

    > [!NOTE]
    > <span data-ttu-id="476a8-341">如果此查詢似乎發生死結，請確定沒有連線到資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="476a8-341">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a><span data-ttu-id="476a8-342">工作 3-設定 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="476a8-342">Task 3 – Configuring the SignalR Application</span></span>

<span data-ttu-id="476a8-343">在這個工作中，您會設定**Geek 測驗**連接到 SQL Server 後擋板。</span><span class="sxs-lookup"><span data-stu-id="476a8-343">In this task, you will configure **Geek Quiz** to connect to the SQL Server backplane.</span></span> <span data-ttu-id="476a8-344">您會先新增**SignalR.SqlServer** NuGet 套件並設定連線到您的後擋板資料庫的字串。</span><span class="sxs-lookup"><span data-stu-id="476a8-344">You will first add the **SignalR.SqlServer** NuGet package and set the connection string to your backplane database.</span></span>

1. <span data-ttu-id="476a8-345">開啟**Package Manager Console**從**工具** | **程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="476a8-345">Open the **Package Manager Console** from **Tools** | **Library Package Manager**.</span></span> <span data-ttu-id="476a8-346">請確定**GeekQuiz**中選取專案**預設專案**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="476a8-346">Make sure that **GeekQuiz** project is selected in the **Default project** drop-down list.</span></span> <span data-ttu-id="476a8-347">輸入下列命令以安裝**Microsoft.AspNet.SignalR.SqlServer** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="476a8-347">Type the following command to install the **Microsoft.AspNet.SignalR.SqlServer** NuGet package.</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. <span data-ttu-id="476a8-348">針對專案重複上一個步驟，但這次**GeekQuiz2**。</span><span class="sxs-lookup"><span data-stu-id="476a8-348">Repeat the previous step but this time for project **GeekQuiz2**.</span></span>
3. <span data-ttu-id="476a8-349">若要設定 SQL Server 後擋板，開啟**Startup.cs**的檔案**GeekQuiz**專案，並新增下列程式碼**設定**方法。</span><span class="sxs-lookup"><span data-stu-id="476a8-349">To configure the SQL Server backplane, open the **Startup.cs** file of the **GeekQuiz** project and add the following code to the **Configure** method.</span></span> <span data-ttu-id="476a8-350">取代**&lt;您資料庫&gt;** 與您建立 SQL Server 後擋板時使用的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="476a8-350">Replace **&lt;YOUR-DATABASE&gt;** with your database name you used when creating the SQL Server backplane.</span></span> <span data-ttu-id="476a8-351">重複這個步驟**GeekQuiz2**專案。</span><span class="sxs-lookup"><span data-stu-id="476a8-351">Repeat this step for the **GeekQuiz2** project.</span></span>

    <span data-ttu-id="476a8-352">(程式碼片段- *RealTimeSignalR-Ex2-StartupConfiguration*)</span><span class="sxs-lookup"><span data-stu-id="476a8-352">(Code Snippet - *RealTimeSignalR - Ex2 - StartupConfiguration*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. <span data-ttu-id="476a8-353">現在，這兩個專案都設定為使用 SQL Server 後擋板，按**F5**同時執行它們。</span><span class="sxs-lookup"><span data-stu-id="476a8-353">Now that both projects are configured to use the SQL Server backplane, press **F5** to run them simultaneously.</span></span>
5. <span data-ttu-id="476a8-354">同樣地， **Visual Studio**將會啟動兩個執行個體**Geek 測驗**在不同的連接埠。</span><span class="sxs-lookup"><span data-stu-id="476a8-354">Again, **Visual Studio** will launch two instances of **Geek Quiz** in different ports.</span></span> <span data-ttu-id="476a8-355">將其中一個瀏覽器釘選在左側及右側螢幕上的其他和您的認證登入。</span><span class="sxs-lookup"><span data-stu-id="476a8-355">Pin one of the browsers on the left and the other on the right of your screen and log in with your credentials.</span></span> <span data-ttu-id="476a8-356">保留在左側的邏輯頁面，並移至**統計資料**pagein 右邊的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="476a8-356">Keep the Trivia page on the left and go to **Statistics** pagein the right browser.</span></span>
6. <span data-ttu-id="476a8-357">開始回答問題，在左邊的瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="476a8-357">Start answering questions in the left browser.</span></span> <span data-ttu-id="476a8-358">此時**統計資料**感謝後擋板更新頁面。</span><span class="sxs-lookup"><span data-stu-id="476a8-358">This time, the **Statistics** page is updated thanks to the backplane.</span></span> <span data-ttu-id="476a8-359">應用程式之間切換 (**統計資料**現在位於左邊，和**邏輯**位於右邊)，並重複進行測試以驗證它是否運作的兩個執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-359">Switch between applications (**Statistics** is now on the left, and **Trivia** is on the right) and repeat the test to validate that it is working for both instances.</span></span> <span data-ttu-id="476a8-360">後擋板做*共用快取*訊息的每個連接的伺服器，且每個伺服器會將訊息儲存在他們自己的本機快取，以散發給連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="476a8-360">The backplane serves as a *shared cache* of messages for each connected server, and each server will store the messages in their own local cache to distribute to connected clients.</span></span>
7. <span data-ttu-id="476a8-361">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="476a8-361">Go back to Visual Studio and stop debugging.</span></span>
8. <span data-ttu-id="476a8-362">SQL Server 後擋板元件會自動產生所需的資料表上指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="476a8-362">The SQL Server backplane component automatically generates the necessary tables on the specified database.</span></span> <span data-ttu-id="476a8-363">在 [ **SQL Server 物件總管**] 面板中，開啟您建立的後擋板的資料庫 (例如： SignalR) 展開一個資料表。</span><span class="sxs-lookup"><span data-stu-id="476a8-363">In the **SQL Server Object Explorer** panel, open the database you created for the backplane (e.g.: SignalR) and expand its tables.</span></span> <span data-ttu-id="476a8-364">您應該會看到下列資料表：</span><span class="sxs-lookup"><span data-stu-id="476a8-364">You should see the following tables:</span></span>

    ![後擋板產生資料表](real-time-web-applications-with-signalr/_static/image27.png)

    <span data-ttu-id="476a8-366">*後擋板產生資料表*</span><span class="sxs-lookup"><span data-stu-id="476a8-366">*Backplane Generated Tables*</span></span>
9. <span data-ttu-id="476a8-367">以滑鼠右鍵按一下**SignalR.Messages\_0**資料表，然後選取**檢視資料**。</span><span class="sxs-lookup"><span data-stu-id="476a8-367">Right-click the **SignalR.Messages\_0** table and select **View Data**.</span></span>

    ![檢視 SignalR 後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image28.png)

    <span data-ttu-id="476a8-369">*檢視 SignalR 後擋板訊息資料表*</span><span class="sxs-lookup"><span data-stu-id="476a8-369">*View SignalR Backplane Messages Table*</span></span>
10. <span data-ttu-id="476a8-370">您可以看到不同的訊息傳送至**中樞**時回答邏輯問題。</span><span class="sxs-lookup"><span data-stu-id="476a8-370">You can see the different messages sent to the **Hub** when answering the trivia questions.</span></span> <span data-ttu-id="476a8-371">後擋板會將這些訊息，任何連接的執行個體。</span><span class="sxs-lookup"><span data-stu-id="476a8-371">The backplane distributes these messages to any connected instance.</span></span>

    ![後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image29.png)

    <span data-ttu-id="476a8-373">*後擋板訊息資料表*</span><span class="sxs-lookup"><span data-stu-id="476a8-373">*Backplane Messages Table*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="476a8-374">總結</span><span class="sxs-lookup"><span data-stu-id="476a8-374">Summary</span></span>

<span data-ttu-id="476a8-375">在這個實際操作實驗室中，您已了解如何新增**SignalR**您的應用程式和傳送通知從伺服器以使用您已連線的用戶端**中樞**。</span><span class="sxs-lookup"><span data-stu-id="476a8-375">In this hands-on lab, you have learned how to add **SignalR** to your application and send notifications from the server to your connected clients using **Hubs**.</span></span> <span data-ttu-id="476a8-376">此外，您已了解如何透過使用相應放大您的應用程式*後擋板*元件多個 IIS 執行個體中部署您的應用程式時。</span><span class="sxs-lookup"><span data-stu-id="476a8-376">Additionally, you learned how to scale out your application by using a *backplane* component when your application is deployed in multiple IIS instances.</span></span>
