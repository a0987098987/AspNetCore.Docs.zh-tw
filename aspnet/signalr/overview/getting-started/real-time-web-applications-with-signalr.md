---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 實習： 與 SignalR 即時 Web 應用程式 |Microsoft 文件
author: rick-anderson
description: 即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。 ASP.NET 開發人員，ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a><span data-ttu-id="f9a8e-104">與 SignalR 實習： 即時 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f9a8e-104">Hands On Lab: Real-Time Web Applications with SignalR</span></span>
====================
<span data-ttu-id="f9a8e-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f9a8e-106">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="f9a8e-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="f9a8e-107">即時 Web 應用程式功能的伺服器端將內容推至連線的用戶端時，即時的能力。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-107">Real-time Web applications feature the ability to push server-side content to the connected clients as it happens, in real-time.</span></span> <span data-ttu-id="f9a8e-108">ASP.NET 開發人員， **ASP.NET SignalR**是將即時 web 功能新增至其應用程式的程式庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-108">For ASP.NET developers, **ASP.NET SignalR** is a library to add real-time web functionality to their applications.</span></span> <span data-ttu-id="f9a8e-109">它會利用數種傳輸，會自動選取最佳可用的傳輸用戶端和伺服器的最佳可用的傳輸。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-109">It takes advantage of several transports, automatically selecting the best available transport given the client and server's best available transport.</span></span> <span data-ttu-id="f9a8e-110">它會利用**WebSocket**，HTML5 API，可讓瀏覽器和伺服器之間的雙向通訊。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-110">It takes advantage of **WebSocket**, an HTML5 API that enables bi-directional communication between the browser and server.</span></span>
> 
> <span data-ttu-id="f9a8e-111">**SignalR**也提供簡單、 高層級 API，以進行用戶端 RPC 伺服器 （在用戶端的瀏覽器中從伺服器端.NET 程式碼會呼叫 JavaScript 函式） 中 ASP.NET 應用程式，以及新增有用的攔截程序，用於連接管理例如連接/中斷連接事件、 分組連線及授權。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-111">**SignalR** also provides a simple, high-level API for doing server to client RPC (call JavaScript functions in your clients' browsers from server-side .NET code) in your ASP.NET application, as well as adding useful hooks for connection management, such as connect/disconnect events, grouping connections, and authorization.</span></span>
> 
> <span data-ttu-id="f9a8e-112">**SignalR**部分所使用的傳輸所需執行用戶端與伺服器之間的即時工作是一個抽象概念。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-112">**SignalR** is an abstraction over some of the transports that are required to do real-time work between client and server.</span></span> <span data-ttu-id="f9a8e-113">A **SignalR**連接啟動為 HTTP，並再提升為**WebSocket**連接，如果有的話。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-113">A **SignalR** connection starts as HTTP, and is then promoted to a **WebSocket** connection if available.</span></span> <span data-ttu-id="f9a8e-114">**WebSocket**是理想的傳輸**SignalR**，因為它會有效率地使用伺服器記憶體的最低的延遲，而且具有最基本的功能 (例如完整雙工用戶端之間的通訊和伺服器），但它也會有最嚴苛的需求： **WebSocket**需要使用伺服器**Windows Server 2012**或**Windows 8**，連同**.NET framework 4.5**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-114">**WebSocket** is the ideal transport for **SignalR**, since it makes the most efficient use of server memory, has the lowest latency, and has the most underlying features (such as full duplex communication between client and server), but it also has the most stringent requirements: **WebSocket** requires the server to be using **Windows Server 2012** or **Windows 8**, along with **.NET Framework 4.5**.</span></span> <span data-ttu-id="f9a8e-115">如果不符合這些需求， **SignalR**會嘗試使用其他傳輸進行其連線 (例如*Ajax 長期輪詢*)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-115">If these requirements are not met, **SignalR** will attempt to use other transports to make its connections (like *Ajax long polling*).</span></span>
> 
> <span data-ttu-id="f9a8e-116">**SignalR** API 包含用戶端和伺服器之間進行通訊的兩個模型：**持續連線**和**集線器**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-116">The **SignalR** API contains two models for communicating between clients and servers: **Persistent Connections** and **Hubs**.</span></span> <span data-ttu-id="f9a8e-117">A**連接**代表簡單端點傳送單一收件者，分組，或廣播訊息。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-117">A **Connection** represents a simple endpoint for sending single-recipient, grouped, or broadcast messages.</span></span> <span data-ttu-id="f9a8e-118">A**中樞**更高層級管線基礎連接 API，可讓您的用戶端與伺服器彼此直接呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-118">A **Hub** is a more high-level pipeline built upon the Connection API that allows your client and server to call methods on each other directly.</span></span>
> 
> ![SignalR 架構](real-time-web-applications-with-signalr/_static/image1.png)
> 
> <span data-ttu-id="f9a8e-120">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-120">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="f9a8e-121">總覽</span><span class="sxs-lookup"><span data-stu-id="f9a8e-121">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f9a8e-122">目標</span><span class="sxs-lookup"><span data-stu-id="f9a8e-122">Objectives</span></span>

<span data-ttu-id="f9a8e-123">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f9a8e-124">使用 SignalR 用戶端，從伺服器傳送通知。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-124">Send notifications from server to client using SignalR.</span></span>
- <span data-ttu-id="f9a8e-125">向外延展 SignalR 應用程式使用**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-125">Scale Out your SignalR application using **SQL Server**.</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f9a8e-126">必要條件</span><span class="sxs-lookup"><span data-stu-id="f9a8e-126">Prerequisites</span></span>

<span data-ttu-id="f9a8e-127">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f9a8e-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)或更新版本</span><span class="sxs-lookup"><span data-stu-id="f9a8e-128">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f9a8e-129">安裝程式</span><span class="sxs-lookup"><span data-stu-id="f9a8e-129">Setup</span></span>

<span data-ttu-id="f9a8e-130">若要執行這個實際操作實驗室練習，您必須先設定您的環境。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-130">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="f9a8e-131">開啟 Windows 檔案總管 視窗並瀏覽至實驗室**來源**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-131">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="f9a8e-132">以滑鼠右鍵按一下**Setup.cmd**選取**系統管理員身分執行**啟動安裝程序，將會設定您的環境，並安裝這個實驗室的 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-132">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="f9a8e-133">如果顯示 [使用者帳戶控制] 對話方塊，確認要繼續進行的動作。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-133">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="f9a8e-134">請確定執行安裝程式之前已在本實驗室的所有相依性。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-134">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="f9a8e-135">使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="f9a8e-135">Using the Code Snippets</span></span>

<span data-ttu-id="f9a8e-136">在實驗室文件，您會指示要插入的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-136">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="f9a8e-137">為了方便起見，大部分的這段程式碼是依現狀 Visual Studio 程式碼片段，您可以從 Visual Studio 2013，不必以手動方式新增內存取。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-137">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="f9a8e-138">每個練習會伴隨起始方案位於**開始**練習，可讓您依照每個練習各自的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-138">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="f9a8e-139">請注意練習期間加入的程式碼片段遺失從中啟動方案，並可能無法運作，直到您已經完成本練習。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-139">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="f9a8e-140">內部練習的原始程式碼，您也可以找到**結束**資料夾包含在 Visual Studio 方案中使用的程式碼所產生對應的練習中的步驟。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-140">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="f9a8e-141">如果您需要其他說明，當您完成這個實際操作實驗室，您可以使用這些解決方案做為指導。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-141">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f9a8e-142">練習</span><span class="sxs-lookup"><span data-stu-id="f9a8e-142">Exercises</span></span>

<span data-ttu-id="f9a8e-143">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-143">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="f9a8e-144">使用 SignalR 的即時資料搭配使用</span><span class="sxs-lookup"><span data-stu-id="f9a8e-144">Working with Real-Time Data Using SignalR</span></span>](#Exercise1)
2. [<span data-ttu-id="f9a8e-145">向外延展使用 SQL Server</span><span class="sxs-lookup"><span data-stu-id="f9a8e-145">Scaling Out Using SQL Server</span></span>](#Exercise2)

<span data-ttu-id="f9a8e-146">完成本實驗室估計時間： **60 分鐘的時間**</span><span class="sxs-lookup"><span data-stu-id="f9a8e-146">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="f9a8e-147">當您第一次啟動 Visual Studio 時，您必須選取其中一個預先定義的設定集合。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-147">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="f9a8e-148">每個預先定義的集合設計為比對特定開發樣式，並判斷視窗配置、 編輯器的行為、 IntelliSense 程式碼片段和對話方塊選項。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-148">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="f9a8e-149">在這個實驗室的程序說明完成指定的工作，Visual Studio 中使用時所需的動作**一般開發設定**集合。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-149">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="f9a8e-150">如果您選擇不同的設定集合的開發環境，可能是您應該考慮的步驟中的差異。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-150">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a><span data-ttu-id="f9a8e-151">練習 1： 使用使用 SignalR 的即時資料</span><span class="sxs-lookup"><span data-stu-id="f9a8e-151">Exercise 1: Working with Real-Time Data Using SignalR</span></span>

<span data-ttu-id="f9a8e-152">雖然交談通常會使用做為範例中，您可以執行整個更多即時 Web 功能。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-152">While chat is often used as an example, you can do a whole lot more with real-time Web functionality.</span></span> <span data-ttu-id="f9a8e-153">每當使用者重新整理網頁即可看到新的資料或頁面實作 Ajax 長期輪詢來擷取新的資料，您可以使用 SignalR。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-153">Any time a user refreshes a web page to see new data or the page implements Ajax long polling to retrieve new data, you can use SignalR.</span></span>

<span data-ttu-id="f9a8e-154">支援 SignalR**伺服器發送**或**廣播**功能; 它會自動處理連接管理。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-154">SignalR supports **server push** or **broadcasting** functionality; it handles connection management automatically.</span></span> <span data-ttu-id="f9a8e-155">在 傳統 HTTP 連線用戶端與伺服器通訊的每個要求，重新建立連接時，但 SignalR 提供用戶端與伺服器之間的持續連線。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-155">In classic HTTP connections for client-server communication, connection is re-established for each request, but SignalR provides persistent connection between the client and server.</span></span> <span data-ttu-id="f9a8e-156">SignalR 的伺服器程式碼呼叫的用戶端程式碼中使用遠端程序呼叫 (RPC) 的瀏覽器，而不是要求-回應模型我們知道在今天。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-156">In SignalR the server code calls out to a client code in the browser using Remote Procedure Calls (RPC), rather than the request-response model we know today.</span></span>

<span data-ttu-id="f9a8e-157">在此練習中，您將設定**玩家測驗**用來顯示包含更新的度量，而不需要重新整理整個頁面的統計資料儀表板的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-157">In this exercise, you will configure the **Geek Quiz** application to use SignalR to display the Statistics dashboard with the updated metrics without the need to refresh the entire page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a><span data-ttu-id="f9a8e-158">工作 1 – 瀏覽一些測驗統計資料頁面</span><span class="sxs-lookup"><span data-stu-id="f9a8e-158">Task 1 – Exploring the Geek Quiz Statistics Page</span></span>

<span data-ttu-id="f9a8e-159">在這項工作，您會瀏覽應用程式，並確認統計資料 頁面會顯示如何以及如何改善資訊的方式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-159">In this task, you will go through the application and verify how the statistics page is shown and how you can improve the way the information is updated.</span></span>

1. <span data-ttu-id="f9a8e-160">開啟**Visual Studio Express 2013 for Web**並開啟**GeekQuiz.sln**方案位於**Source\Ex1 WorkingWithRealTimeData\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-160">Open **Visual Studio Express 2013 for Web** and open the **GeekQuiz.sln** solution located in the **Source\Ex1-WorkingWithRealTimeData\Begin** folder.</span></span>
2. <span data-ttu-id="f9a8e-161">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-161">Press **F5** to run the solution.</span></span> <span data-ttu-id="f9a8e-162">**登入**頁面應該會出現在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-162">The **Log in** page should appear in the browser.</span></span>

    <span data-ttu-id="f9a8e-163">![執行解決方案](real-time-web-applications-with-signalr/_static/image2.png "執行解決方案")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-163">![Running the solution](real-time-web-applications-with-signalr/_static/image2.png "Running the solution")</span></span>

    <span data-ttu-id="f9a8e-164">*執行解決方案*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-164">*Running the solution*</span></span>
3. <span data-ttu-id="f9a8e-165">按一下**註冊**頁面以建立新的使用者在應用程式右上角。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-165">Click **Register** in the upper-right corner of the page to create a new user in the application.</span></span>

    <span data-ttu-id="f9a8e-166">![註冊連結](real-time-web-applications-with-signalr/_static/image3.png "註冊連結")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-166">![Register link](real-time-web-applications-with-signalr/_static/image3.png "Register link")</span></span>

    <span data-ttu-id="f9a8e-167">*註冊連結*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-167">*Register link*</span></span>
4. <span data-ttu-id="f9a8e-168">在**註冊**頁面上，輸入**使用者名**和**密碼**，然後按一下 **註冊**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-168">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    <span data-ttu-id="f9a8e-169">![註冊使用者](real-time-web-applications-with-signalr/_static/image4.png "註冊使用者")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-169">![Registering a user](real-time-web-applications-with-signalr/_static/image4.png "Registering a user")</span></span>

    <span data-ttu-id="f9a8e-170">*註冊使用者*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-170">*Registering a user*</span></span>
5. <span data-ttu-id="f9a8e-171">應用程式註冊新帳戶，並驗證使用者，並且重新導向至首頁上顯示的第一個測驗問題。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-171">The application registers the new account and the user is authenticated and redirected back to the home page showing the first quiz question.</span></span>
6. <span data-ttu-id="f9a8e-172">開啟**統計資料**頁面上，在新視窗中，並放**首頁**頁面和**統計資料**頁面上的並行。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-172">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side.</span></span>

    <span data-ttu-id="f9a8e-173">![並存 windows](real-time-web-applications-with-signalr/_static/image5.png "側邊 windows")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-173">![Side-by-side windows](real-time-web-applications-with-signalr/_static/image5.png "Side by side windows")</span></span>

    <span data-ttu-id="f9a8e-174">*並存的 windows*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-174">*Side-by-side windows*</span></span>
7. <span data-ttu-id="f9a8e-175">在**首頁**頁面上，按一下其中一個選項回答的問題。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-175">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="f9a8e-176">![回答](real-time-web-applications-with-signalr/_static/image6.png "回答的問題。")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-176">![Answering a question](real-time-web-applications-with-signalr/_static/image6.png "Answering a question")</span></span>

    <span data-ttu-id="f9a8e-177">*回答問題。*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-177">*Answering a question*</span></span>
8. <span data-ttu-id="f9a8e-178">按一下其中一個按鈕之後, 應該會出現答案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-178">After clicking one of the buttons, the answer should appear.</span></span>

    <span data-ttu-id="f9a8e-179">![正確回答的問題](real-time-web-applications-with-signalr/_static/image7.png "正確回答的問題")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-179">![Question answered correct](real-time-web-applications-with-signalr/_static/image7.png "Question answered correct")</span></span>

    <span data-ttu-id="f9a8e-180">*正確解答的問題*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-180">*Question answered correctly*</span></span>
9. <span data-ttu-id="f9a8e-181">請注意統計資料 頁面中提供的資訊已過期。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-181">Notice that the information provided in the Statistics page is outdated.</span></span> <span data-ttu-id="f9a8e-182">重新整理頁面，以查看更新的結果。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-182">Refresh the page in order to see the updated results.</span></span>

    <span data-ttu-id="f9a8e-183">![統計資料 頁面](real-time-web-applications-with-signalr/_static/image8.png "統計資料 頁面")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-183">![Statistics page](real-time-web-applications-with-signalr/_static/image8.png "Statistics page")</span></span>

    <span data-ttu-id="f9a8e-184">*統計資料 頁面*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-184">*Statistics page*</span></span>
10. <span data-ttu-id="f9a8e-185">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-185">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a><span data-ttu-id="f9a8e-186">工作 2-加入至顯示線上圖表玩家測驗 SignalR</span><span class="sxs-lookup"><span data-stu-id="f9a8e-186">Task 2 – Adding SignalR to Geek Quiz to Show Online Charts</span></span>

<span data-ttu-id="f9a8e-187">在這項工作，您將 SignalR 加入至方案，並將更新傳送至用戶端自動當新的回應傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-187">In this task, you will add SignalR to the solution and send updates to the clients automatically when a new answer is sent to the server.</span></span>

1. <span data-ttu-id="f9a8e-188">從**工具**在 Visual Studio 中，選取功能表**程式庫套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-188">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="f9a8e-189">在**Package Manager Console**視窗中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-189">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    <span data-ttu-id="f9a8e-190">![SignalR 套件安裝](real-time-web-applications-with-signalr/_static/image9.png "SignalR 套件安裝")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-190">![SignalR package installation](real-time-web-applications-with-signalr/_static/image9.png "SignalR package installation")</span></span>

    <span data-ttu-id="f9a8e-191">*SignalR 套件安裝*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-191">*SignalR package installation*</span></span>

   > [!NOTE]
   > <span data-ttu-id="f9a8e-192">安裝時**SignalR** NuGet 封裝版本 2.0.2 從全新的 MVC 5 應用程式，您必須手動更新**OWIN** 2.0.1 版的封裝 （或更新版本） 安裝 SignalR 之前。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-192">When installing **SignalR** NuGet packages version 2.0.2 from a brand new MVC 5 application, you will need to manually update **OWIN** packages to version 2.0.1 (or higher) before installing SignalR.</span></span> <span data-ttu-id="f9a8e-193">若要這樣做，您可以執行下列指令碼中的**Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="f9a8e-193">To do this, you can execute the following script in the **Package Manager Console**:</span></span>
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > <span data-ttu-id="f9a8e-194">在 SignalR 的未來版本中，將會自動更新 OWIN 相依性。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-194">In a future release of SignalR, OWIN dependencies will be automatically updated.</span></span>
3. <span data-ttu-id="f9a8e-195">在**方案總管 中**，依序展開**指令碼**資料夾，請注意，SignalR *js*檔案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-195">In **Solution Explorer**, expand the **Scripts** folder and notice that the SignalR *js* files were added to the solution.</span></span>

    <span data-ttu-id="f9a8e-196">![SignalR JavaScript 參考](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript 參考")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-196">![SignalR JavaScript references](real-time-web-applications-with-signalr/_static/image10.png "SignalR JavaScript references")</span></span>

    <span data-ttu-id="f9a8e-197">*SignalR JavaScript 參考*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-197">*SignalR JavaScript references*</span></span>
4. <span data-ttu-id="f9a8e-198">在**方案總管 中**，以滑鼠右鍵按一下**GeekQuiz**專案，然後選取**新增** | **新資料夾**，並將它命名**集線器**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-198">In **Solution Explorer**, right-click the **GeekQuiz** project, select **Add** | **New Folder**, and name it **Hubs**.</span></span>
5. <span data-ttu-id="f9a8e-199">以滑鼠右鍵按一下**集線器**資料夾，然後選取**新增 |新項目**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-199">Right-click the **Hubs** folder and select **Add | New Item**.</span></span>

    <span data-ttu-id="f9a8e-200">![加入新項目](real-time-web-applications-with-signalr/_static/image11.png "加入新項目")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-200">![Add new item](real-time-web-applications-with-signalr/_static/image11.png "Add new item")</span></span>

    <span data-ttu-id="f9a8e-201">*加入新項目*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-201">*Add new item*</span></span>
6. <span data-ttu-id="f9a8e-202">在**加入新項目**對話方塊中，選取**Visual C# |Web |SignalR**的左窗格中，選取的節點**SignalR 中樞類別 (v2)**從中間窗格中，將檔案命名**StatisticsHub.cs**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-202">In the **Add New Item** dialog box, select the **Visual C# | Web | SignalR** node in the left pane, select **SignalR Hub Class (v2)** from the center pane, name the file **StatisticsHub.cs** and click **Add**.</span></span>

    <span data-ttu-id="f9a8e-203">![加入新項目對話方塊](real-time-web-applications-with-signalr/_static/image12.png "加入新項目對話方塊")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-203">![Add new item dialog box](real-time-web-applications-with-signalr/_static/image12.png "Add new item dialog box")</span></span>

    <span data-ttu-id="f9a8e-204">*加入新項目對話方塊*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-204">*Add new item dialog box*</span></span>
7. <span data-ttu-id="f9a8e-205">中的程式碼取代**StatisticsHub**為下列程式碼的類別。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-205">Replace the code in the **StatisticsHub** class with the following code.</span></span>

    <span data-ttu-id="f9a8e-206">(程式碼片段- *RealTimeSignalR-Ex1-StatisticsHubClass*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-206">(Code Snippet - *RealTimeSignalR - Ex1 - StatisticsHubClass*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. <span data-ttu-id="f9a8e-207">開啟**Startup.cs**和結尾加入下列行**組態**方法。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-207">Open **Startup.cs** and add the following line at the end of the **Configuration** method.</span></span>

    <span data-ttu-id="f9a8e-208">(程式碼片段- *RealTimeSignalR-Ex1-MapSignalR*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-208">(Code Snippet - *RealTimeSignalR - Ex1 - MapSignalR*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. <span data-ttu-id="f9a8e-209">開啟**StatisticsService.cs**頁面內**服務**資料夾並加入下列 using 指示詞。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-209">Open the **StatisticsService.cs** page inside the **Services** folder and add the following using directives.</span></span>

    <span data-ttu-id="f9a8e-210">(程式碼片段- *RealTimeSignalR-Ex1-UsingDirectives*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-210">(Code Snippet - *RealTimeSignalR - Ex1 - UsingDirectives*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. <span data-ttu-id="f9a8e-211">若要通知連線的用戶端的更新，請您第一次擷取**內容**目前連接的物件。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-211">To notify connected clients of updates, you first retrieve a **Context** object for the current connection.</span></span> <span data-ttu-id="f9a8e-212">**中樞**物件包含方法，將訊息傳送至單一用戶端或廣播到所有已連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-212">The **Hub** object contains methods to send messages to a single client or broadcast to all connected clients.</span></span> <span data-ttu-id="f9a8e-213">將下列方法加入**StatisticsService**廣播統計資料的類別。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-213">Add the following method to the **StatisticsService** class to broadcast the statistics data.</span></span>

    <span data-ttu-id="f9a8e-214">(程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesMethod*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-214">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="f9a8e-215">上述程式碼，您正在使用任意的方法名稱，用戶端上呼叫的函式 (也就是： *updateStatistics*)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-215">In the code above, you are using an arbitrary method name to call a function on the client (i.e.: *updateStatistics*).</span></span> <span data-ttu-id="f9a8e-216">您指定的方法名稱會解譯為動態的物件，表示沒有任何 IntelliSense 或它的編譯時間驗證。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-216">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="f9a8e-217">在執行階段會評估運算式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-217">The expression is evaluated at run time.</span></span> <span data-ttu-id="f9a8e-218">當執行方法呼叫時，SignalR 就會傳送給用戶端的方法名稱和參數值。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-218">When the method call executes, SignalR sends the method name and the parameter values to the client.</span></span> <span data-ttu-id="f9a8e-219">如果用戶端具有相符名稱的方法，會呼叫該方法，且參數值會傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-219">If the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="f9a8e-220">如果用戶端上找到沒有對應的方法，就不會引發錯誤。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-220">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="f9a8e-221">如需詳細資訊，請參閱[ASP.NET SignalR 中樞 API 指南](../guide-to-the-api/hubs-api-guide-server.md)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-221">For more information, refer to [ASP.NET SignalR Hubs API Guide](../guide-to-the-api/hubs-api-guide-server.md).</span></span>
11. <span data-ttu-id="f9a8e-222">開啟**TriviaController.cs**頁面內**控制器**資料夾並加入下列 using 指示詞。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-222">Open the **TriviaController.cs** page inside the **Controllers** folder and add the following using directives.</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. <span data-ttu-id="f9a8e-223">將下列反白顯示的程式碼加入**Post**動作方法。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-223">Add the following highlighted code to the **Post** action method.</span></span>

    <span data-ttu-id="f9a8e-224">(程式碼片段- *RealTimeSignalR-Ex1-NotifyUpdatesCall*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-224">(Code Snippet - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. <span data-ttu-id="f9a8e-225">開啟**Statistics.cshtml**頁面內**檢視 |首頁**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-225">Open the **Statistics.cshtml** page inside the **Views | Home** folder.</span></span> <span data-ttu-id="f9a8e-226">找出**指令碼**區段，區段的開頭加入下列指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-226">Locate the **Scripts** section and add the following script references at the beginning of the section.</span></span>

    <span data-ttu-id="f9a8e-227">(程式碼片段- *RealTimeSignalR-Ex1-SignalRScriptReferences*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-227">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="f9a8e-228">當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，封裝管理員可能會安裝的版本比本主題所顯示的版本還新的 SignalR 指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-228">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="f9a8e-229">請確定您的程式碼中的指令碼參考符合安裝在您的專案中的指令碼程式庫的版本。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-229">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>
14. <span data-ttu-id="f9a8e-230">加入下列反白顯示的程式碼，來連接到 SignalR 中樞的用戶端從中樞接收新訊息時，更新統計資料。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-230">Add the following highlighted code to connect the client to the SignalR hub and update the statistics data when a new message is received from the hub.</span></span>

    <span data-ttu-id="f9a8e-231">(程式碼片段- *RealTimeSignalR-Ex1-SignalRClientCode*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-231">(Code Snippet - *RealTimeSignalR - Ex1 - SignalRClientCode*)</span></span>

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    <span data-ttu-id="f9a8e-232">在此程式碼中，您建立的中樞 Proxy 和註冊事件處理常式來接聽伺服器所傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-232">In this code, you are creating a Hub Proxy and registering an event handler to listen for messages sent by the server.</span></span> <span data-ttu-id="f9a8e-233">您在此情況下，接聽訊息透過傳送*updateStatistics*方法。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-233">In this case, you listen for messages sent through the *updateStatistics* method.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="f9a8e-234">工作 3-執行解決方案</span><span class="sxs-lookup"><span data-stu-id="f9a8e-234">Task 3 – Running the Solution</span></span>

<span data-ttu-id="f9a8e-235">在這個工作中，您將執行方案來驗證新問題要回答之後使用 SignalR 自動更新統計資料檢視。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-235">In this task, you will run the solution to verify that the statistics view is updated automatically using SignalR after answering a new question.</span></span>

1. <span data-ttu-id="f9a8e-236">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-236">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9a8e-237">如果尚未登入應用程式，登入您在工作 1 中建立的使用者。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-237">If not already logged in to the application, log in with the user you created in Task 1.</span></span>
2. <span data-ttu-id="f9a8e-238">開啟**統計資料**頁面上，在新視窗中，並放**首頁**頁面和**統計資料**頁面來並行工作 1 中一樣。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-238">Open the **Statistics** page in a new window and put the **Home** page and **Statistics** page side-by-side as you did in Task 1.</span></span>
3. <span data-ttu-id="f9a8e-239">在**首頁**頁面上，按一下其中一個選項回答的問題。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-239">In the **Home** page, answer the question by clicking one of the options.</span></span>

    <span data-ttu-id="f9a8e-240">![另一個問題要回答](real-time-web-applications-with-signalr/_static/image13.png "回答另一個問題。")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-240">![Answering another question](real-time-web-applications-with-signalr/_static/image13.png "Answering another question")</span></span>

    <span data-ttu-id="f9a8e-241">*另一個問題要回答*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-241">*Answering another question*</span></span>
4. <span data-ttu-id="f9a8e-242">按一下其中一個按鈕之後, 應該會出現答案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-242">After clicking one of the buttons, the answer should appear.</span></span> <span data-ttu-id="f9a8e-243">請注意頁面上的統計資料資訊會自動更新後回答的問題。 更新的資訊，而不需要重新整理整個頁面。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-243">Notice that the Statistics information on the page is updated automatically after answering the question with the updated information without the need to refresh the entire page.</span></span>

    <span data-ttu-id="f9a8e-244">![統計資料 頁面重新整理回應之後](real-time-web-applications-with-signalr/_static/image14.png "回應之後重新整理統計資料頁面")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-244">![Statistics page refreshed after answer](real-time-web-applications-with-signalr/_static/image14.png "Statistics page refreshed after answer")</span></span>

    <span data-ttu-id="f9a8e-245">*統計資料重新整理之後回應 頁面*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-245">*Statistics page refreshed after answer*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a><span data-ttu-id="f9a8e-246">練習 2： 向外延展使用 SQL Server</span><span class="sxs-lookup"><span data-stu-id="f9a8e-246">Exercise 2: Scaling Out Using SQL Server</span></span>

<span data-ttu-id="f9a8e-247">當調整 web 應用程式，您通常可以選擇之間*向上擴充*和*向外延展*選項。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-247">When scaling a web application, you can generally choose between *scaling up* and *scaling out* options.</span></span> <span data-ttu-id="f9a8e-248">*向上延展*表示較多資源 （CPU、 RAM 等），同時使用更大的伺服器，*向外延展*表示加入更多伺服器來處理負載。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-248">*Scale up* means using a larger server, with more resources (CPU, RAM, etc.) while *scale out* means adding more servers to handle the load.</span></span> <span data-ttu-id="f9a8e-249">後者的問題在於，用戶端可以取得路由傳送到不同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-249">The problem with the latter is that the clients can get routed to different servers.</span></span> <span data-ttu-id="f9a8e-250">連接到一部伺服器的用戶端不會接收從另一部伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-250">A client that is connected to one server will not receive messages sent from another server.</span></span>

<span data-ttu-id="f9a8e-251">您可以使用名為的元件，以解決這些問題*後擋板*、 將伺服器之間的訊息轉送。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-251">You can solve these issues by using a component called *backplane*, to forward messages between servers.</span></span> <span data-ttu-id="f9a8e-252">後擋板，已啟用，與每個應用程式執行個體將訊息傳送至後擋板，並後擋板，已轉送至其他應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-252">With a backplane enabled, each application instance sends messages to the backplane, and the backplane forwards them to the other application instances.</span></span>

<span data-ttu-id="f9a8e-253">目前有三種類型的 SignalR 的背板：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-253">There are currently three types of backplanes for SignalR:</span></span>

- <span data-ttu-id="f9a8e-254">**Windows Azure 服務匯流排**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-254">**Windows Azure Service Bus**.</span></span> <span data-ttu-id="f9a8e-255">Service Bus 是訊息的基礎結構可讓鬆散偶合的訊息傳送的元件。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-255">Service Bus is a messaging infrastructure that allows components to send loosely coupled messages.</span></span>
- <span data-ttu-id="f9a8e-256">**SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-256">**SQL Server**.</span></span> <span data-ttu-id="f9a8e-257">SQL Server 後擋板訊息寫入 SQL 資料表。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-257">The SQL Server backplane writes messages to SQL tables.</span></span> <span data-ttu-id="f9a8e-258">後擋板有效率的通訊使用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-258">The backplane uses Service Broker for efficient messaging.</span></span> <span data-ttu-id="f9a8e-259">不過，它也適用於未啟用 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-259">However, it also works if Service Broker is not enabled.</span></span>
- <span data-ttu-id="f9a8e-260">**Redis**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-260">**Redis**.</span></span> <span data-ttu-id="f9a8e-261">Redis 是記憶體中索引鍵-值存放區。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-261">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="f9a8e-262">Redis 支援發行/訂閱 (「 pub/sub") 的模式來傳送訊息。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-262">Redis supports a publish/subscribe ("pub/sub") pattern for sending messages.</span></span>

<span data-ttu-id="f9a8e-263">每個訊息是透過訊息匯流排傳送。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-263">Every message is sent through a message bus.</span></span> <span data-ttu-id="f9a8e-264">訊息匯流排實作[IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)介面，可提供發佈/訂閱抽象的。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-264">A message bus implements the [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interface, which provides a publish/subscribe abstraction.</span></span> <span data-ttu-id="f9a8e-265">運作方式是取代預設的背板**IMessageBus**與針對該後擋板匯流排。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-265">The backplanes work by replacing the default **IMessageBus** with a bus designed for that backplane.</span></span>

<span data-ttu-id="f9a8e-266">每個伺服器執行個體連接到後擋板，已透過匯流排。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-266">Each server instance connects to the backplane through the bus.</span></span> <span data-ttu-id="f9a8e-267">當訊息傳送時，它會移至後擋板，並後擋板，已將它傳送至每一部伺服器。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-267">When a message is sent, it goes to the backplane, and the backplane sends it to every server.</span></span> <span data-ttu-id="f9a8e-268">當伺服器從後擋板接收訊息時，它會在其本機快取中儲存訊息。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-268">When a server receives a message from the backplane, it stores the message in its local cache.</span></span> <span data-ttu-id="f9a8e-269">伺服器再將訊息傳遞至用戶端從其本機快取。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-269">The server then delivers messages to clients from its local cache.</span></span>

<span data-ttu-id="f9a8e-270">如需有關 SignalR 後擋板的運作方式，請閱讀本[文章](../performance/scaleout-in-signalr.md)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-270">For more information about how the SignalR backplane works, read this [article](../performance/scaleout-in-signalr.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f9a8e-271">有一些案例後擋板可能會變成瓶頸。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-271">There are some scenarios where a backplane can become a bottleneck.</span></span> <span data-ttu-id="f9a8e-272">以下是一些典型的 SignalR 案例：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-272">Here are some typical SignalR scenarios:</span></span>
> 
> - <span data-ttu-id="f9a8e-273">[伺服器廣播](tutorial-server-broadcast-with-signalr.md)（例如，股票行情指示器）： 背板很適合此案例中，因為伺服器控制傳送訊息的速率。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-273">[Server broadcast](tutorial-server-broadcast-with-signalr.md) (e.g., stock ticker): Backplanes work well for this scenario, because the server controls the rate at which messages are sent.</span></span>
> - <span data-ttu-id="f9a8e-274">[用戶端](tutorial-getting-started-with-signalr.md)（例如聊天）： 在此案例中後, 擋板可能是效能瓶頸如果用戶端數目的訊息數目縮放比例; 也就是說，如果訊息的速率增加按比例越多的用戶端加入。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-274">[Client-to-client](tutorial-getting-started-with-signalr.md) (e.g., chat): In this scenario, the backplane might be a bottleneck if the number of messages scales with the number of clients; that is, if the rate of messages grows proportionally as more clients join.</span></span>
> - <span data-ttu-id="f9a8e-275">[高頻率即時](tutorial-high-frequency-realtime-with-signalr.md)（例如，即時遊戲）： 後擋板建議您不要在此案例。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-275">[High-frequency realtime](tutorial-high-frequency-realtime-with-signalr.md) (e.g., real-time games): A backplane is not recommended for this scenario.</span></span>


<span data-ttu-id="f9a8e-276">在此練習中，您將使用**SQL Server**發佈訊息**玩家測驗**應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-276">In this exercise, you will use **SQL Server** to distribute messages across the **Geek Quiz** application.</span></span> <span data-ttu-id="f9a8e-277">您要了解如何設定組態，但若要取得完整的效果的單一測試機器上執行這些工作，您將需要部署兩個或多部伺服器的 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-277">You will run these tasks on a single test machine to learn how to set up the configuration, but in order to get the full effect, you will need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="f9a8e-278">您也必須在一部伺服器上或不同的專用伺服器上安裝 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-278">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span>

![向外延展使用 SQL Server 的圖表](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a><span data-ttu-id="f9a8e-280">工作 1-了解案例</span><span class="sxs-lookup"><span data-stu-id="f9a8e-280">Task 1 - Understanding the Scenario</span></span>

<span data-ttu-id="f9a8e-281">在這個工作中，您將執行 2 個執行個體的**玩家測驗**模擬多個 IIS 在本機電腦上執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-281">In this task, you will run 2 instances of **Geek Quiz** simulating multiple IIS instances on your local machine.</span></span> <span data-ttu-id="f9a8e-282">在此案例中，回應瑣事某一個應用程式的問題時，更新將不會在統計資料 頁面上的第二個執行個體收到通知。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-282">In this scenario, when answering trivia questions on one application, update won't be notified on the statistics page of the second instance.</span></span> <span data-ttu-id="f9a8e-283">這個模擬類似於您的應用程式部署在多個執行個體所在的環境和使用負載平衡器與它們通訊。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-283">This simulation resembles an environment where your application is deployed on multiple instances and using a load balancer to communicate with them.</span></span>

1. <span data-ttu-id="f9a8e-284">開啟**Begin.sln**方案位於**來源/Ex2 ScalingOutWithSQLServer/開始**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-284">Open the **Begin.sln** solution located in the **Source/Ex2-ScalingOutWithSQLServer/Begin** folder.</span></span> <span data-ttu-id="f9a8e-285">載入之後，您會注意到**伺服器總管**方案中有兩個專案具有相同結構但不同的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-285">Once loaded, you will notice on the **Server Explorer** that the solution has two projects with identical structures but different names.</span></span> <span data-ttu-id="f9a8e-286">這會模擬在本機電腦上執行相同的應用程式的兩個執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-286">This will simulate running two instances of the same application on your local machine.</span></span>

    <span data-ttu-id="f9a8e-287">![開始模擬玩家測驗的 2 個執行個體的方案](real-time-web-applications-with-signalr/_static/image16.png "開始方案模擬玩家測驗的 2 個執行個體")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-287">![Begin Solution Simulating 2 Instances of Geek Quiz](real-time-web-applications-with-signalr/_static/image16.png "Begin Solution Simulating 2 Instances of Geek Quiz")</span></span>

    <span data-ttu-id="f9a8e-288">*開始模擬玩家測驗的 2 個執行個體的方案*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-288">*Begin Solution Simulating 2 Instances of Geek Quiz*</span></span>
2. <span data-ttu-id="f9a8e-289">開啟方案的屬性頁面，以滑鼠右鍵按一下方案節點，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-289">Open the properties page of the solution by right-clicking the solution node and selecting **Properties**.</span></span> <span data-ttu-id="f9a8e-290">下**啟始專案**，選取**多個啟始專案**並變更**動作**值這兩個專案到*啟動*。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-290">Under **Startup Project**, select **Multiple startup projects** and change the **Action** value for both projects to *Start*.</span></span>

    <span data-ttu-id="f9a8e-291">![啟動多個專案](real-time-web-applications-with-signalr/_static/image17.png "啟動多個專案")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-291">![Starting Multiple Projects](real-time-web-applications-with-signalr/_static/image17.png "Starting Multiple Projects")</span></span>

    <span data-ttu-id="f9a8e-292">*啟動多個專案*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-292">*Starting Multiple Projects*</span></span>
3. <span data-ttu-id="f9a8e-293">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-293">Press **F5** to run the solution.</span></span> <span data-ttu-id="f9a8e-294">應用程式會啟動兩個執行個體**玩家測驗**在不同的連接埠，並模擬多個相同的應用程式執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-294">The application will launch two instances of **Geek Quiz** in different ports, simulating multiple instances of the same application.</span></span> <span data-ttu-id="f9a8e-295">在左方和其他螢幕的右側釘選的瀏覽器上。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-295">Pin one of the browsers on left and the other on the right of your screen.</span></span> <span data-ttu-id="f9a8e-296">您的認證登入或註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-296">Log in with your credentials or register a new user.</span></span> <span data-ttu-id="f9a8e-297">登入之後，保留瑣事頁面左側，並移至**統計資料**右邊的瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-297">Once logged in, keep the Trivia page on the left and go to the **Statistics** page in the browser on the right.</span></span>

    ![玩家測驗並存](real-time-web-applications-with-signalr/_static/image18.png)

    <span data-ttu-id="f9a8e-299">*玩家測驗並存*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-299">*Geek Quiz Side by Side*</span></span>

    ![在不同的連接埠的玩家測驗](real-time-web-applications-with-signalr/_static/image19.png)

    <span data-ttu-id="f9a8e-301">*在不同的連接埠的玩家測驗*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-301">*Geek Quiz in Different Ports*</span></span>
4. <span data-ttu-id="f9a8e-302">在左邊的瀏覽器中啟動回應問題，您會注意到**統計資料**右邊的瀏覽器頁面仍未更新。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-302">Start answering questions in the left browser and you will notice that the **Statistics** page in the right browser is not being updated.</span></span> <span data-ttu-id="f9a8e-303">這是因為**SignalR**使用本機快取可以分散其用戶端的訊息和此案例會模擬多個執行個體，因此快取不它們之間會共用。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-303">This is because **SignalR** uses a local cache to distribute messages across their clients and this scenario is simulating multiple instances, therefore the cache is not shared between them.</span></span> <span data-ttu-id="f9a8e-304">您可以確認**SignalR**測試相同的步驟，但是使用單一的應用程式是否正常運作。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-304">You can verify that **SignalR** is working by testing the same steps but using a single app.</span></span> <span data-ttu-id="f9a8e-305">在下列工作中，您將設定複寫執行個體之間的訊息後擋板。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-305">In the following tasks you will configure a backplane to replicate the messages across instances.</span></span>
5. <span data-ttu-id="f9a8e-306">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-306">Go back to Visual Studio and stop debugging.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a><span data-ttu-id="f9a8e-307">工作 2 – 建立 SQL Server 後擋板</span><span class="sxs-lookup"><span data-stu-id="f9a8e-307">Task 2 – Creating the SQL Server Backplane</span></span>

<span data-ttu-id="f9a8e-308">在這項工作，您將建立的資料庫，將做為的後擋板**玩家測驗**應用程式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-308">In this task, you will create a database that will serve as a backplane for the **Geek Quiz** application.</span></span> <span data-ttu-id="f9a8e-309">您將使用**SQL Server 物件總管**瀏覽您的伺服器，並初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-309">You will use **SQL Server Object Explorer** to browse your server and initialize the database.</span></span> <span data-ttu-id="f9a8e-310">此外，您將會啟用**Service Broker**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-310">Additionally, you will enable the **Service Broker**.</span></span>

1. <span data-ttu-id="f9a8e-311">在**Visual Studio**，開啟功能表**檢視**選取**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-311">In **Visual Studio**, open menu **View** and select **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="f9a8e-312">連接到 LocalDB 執行個體，以滑鼠右鍵按一下**SQL Server**節點，然後選取**加入 SQL Server...**選項。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-312">Connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="f9a8e-313">![加入 SQL Server 執行個體](real-time-web-applications-with-signalr/_static/image20.png "加入 SQL Server 執行個體")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-313">![Adding a SQL Server Instance](real-time-web-applications-with-signalr/_static/image20.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="f9a8e-314">*將 SQL Server 執行個體加入至 SQL Server 物件總管*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-314">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
3. <span data-ttu-id="f9a8e-315">設定**伺服器名稱**至*(localdb) \v11.0*留下**Windows 驗證**當做驗證模式。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-315">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="f9a8e-316">按一下**連接**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-316">Click **Connect** to continue.</span></span>

    <span data-ttu-id="f9a8e-317">![連接到 LocalDB](real-time-web-applications-with-signalr/_static/image21.png "連接到 LocalDB")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-317">![Connecting to LocalDB](real-time-web-applications-with-signalr/_static/image21.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="f9a8e-318">*連接到 LocalDB*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-318">*Connecting to LocalDB*</span></span>
4. <span data-ttu-id="f9a8e-319">現在，您會連接到 LocalDB 執行個體，您必須建立用來表示 SQL Server 後擋板 SignalR 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-319">Now that you are connected to your LocalDB instance, you will need to create a database that will represent the SQL Server backplane for SignalR.</span></span> <span data-ttu-id="f9a8e-320">若要這樣做，請以滑鼠右鍵按一下**資料庫**節點，然後選取**加入新的資料庫**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-320">To do this, right-click the **Databases** node and select **Add New Database**.</span></span>

    <span data-ttu-id="f9a8e-321">![將新的資料庫](real-time-web-applications-with-signalr/_static/image22.png "加入新的資料庫")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-321">![Adding a new database](real-time-web-applications-with-signalr/_static/image22.png "Adding a new database")</span></span>

    <span data-ttu-id="f9a8e-322">*將新的資料庫*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-322">*Adding a new database*</span></span>
5. <span data-ttu-id="f9a8e-323">資料庫名稱設定為*SignalR*按一下**確定**加以建立。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-323">Set the database name to *SignalR* and click **OK** to create it.</span></span>

    <span data-ttu-id="f9a8e-324">![建立 SignalR 資料庫](real-time-web-applications-with-signalr/_static/image23.png "建立 SignalR 資料庫")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-324">![Creating the SignalR database](real-time-web-applications-with-signalr/_static/image23.png "Creating the SignalR database")</span></span>

    <span data-ttu-id="f9a8e-325">*建立 SignalR 資料庫*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-325">*Creating the SignalR database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9a8e-326">您可以選擇任何資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-326">You can choose any name for the database.</span></span>
6. <span data-ttu-id="f9a8e-327">若要從後擋板更有效率地接收更新，建議啟用 Service Broker 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-327">To receive updates more efficiently from the backplane, it is recommended to enable Service Broker for the database.</span></span> <span data-ttu-id="f9a8e-328">Service Broker 提供傳訊和佇列在 SQL Server 中的原生支援。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-328">Service Broker provides native support for messaging and queuing in SQL Server.</span></span> <span data-ttu-id="f9a8e-329">後擋板也就不會 Service Broker。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-329">The backplane also works without Service Broker.</span></span> <span data-ttu-id="f9a8e-330">開啟新的查詢，以滑鼠右鍵按一下資料庫，然後選取**新查詢**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-330">Open a new query by right-clicking the database and select **New Query**.</span></span>

    <span data-ttu-id="f9a8e-331">![開啟新查詢](real-time-web-applications-with-signalr/_static/image24.png "開啟新查詢")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-331">![Opening a New Query](real-time-web-applications-with-signalr/_static/image24.png "Opening a New Query")</span></span>

    <span data-ttu-id="f9a8e-332">*開啟新查詢*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-332">*Opening a New Query*</span></span>
7. <span data-ttu-id="f9a8e-333">若要檢查是否啟用 Service Broker，查詢**是\_broker\_啟用**中的資料行**sys.databases**目錄檢視。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-333">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span> <span data-ttu-id="f9a8e-334">在最近開啟的查詢視窗中執行下列指令碼。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-334">Execute the following script in the recently opened query window.</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    <span data-ttu-id="f9a8e-335">![查詢 Service Broker 狀態](real-time-web-applications-with-signalr/_static/image25.png "查詢服務 Broker 狀態")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-335">![Querying the Service Broker Status](real-time-web-applications-with-signalr/_static/image25.png "Querying the Service Broker Status")</span></span>

    <span data-ttu-id="f9a8e-336">*查詢 Service Broker 狀態*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-336">*Querying the Service Broker Status*</span></span>
8. <span data-ttu-id="f9a8e-337">如果值**是\_broker\_啟用**資料庫中的資料行是&quot;0&quot;，使用下列命令來啟用它。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-337">If the value of the **is\_broker\_enabled** column in your database is &quot;0&quot;, use the following command to enable it.</span></span> <span data-ttu-id="f9a8e-338">取代**&lt;您資料庫&gt;**建立資料庫時，您設定的名稱 (例如： SignalR)。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-338">Replace **&lt;YOUR-DATABASE&gt;** with the name you set when creating the database (e.g.: SignalR).</span></span>

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    <span data-ttu-id="f9a8e-339">![啟用 Service Broker](real-time-web-applications-with-signalr/_static/image26.png "啟用 Service Broker")</span><span class="sxs-lookup"><span data-stu-id="f9a8e-339">![Enabling Service Broker](real-time-web-applications-with-signalr/_static/image26.png "Enabling Service Broker")</span></span>

    <span data-ttu-id="f9a8e-340">*啟用 Service Broker*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-340">*Enabling Service Broker*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f9a8e-341">如果此查詢似乎發生死結，請確定沒有應用程式連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-341">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a><span data-ttu-id="f9a8e-342">工作 3-設定 SignalR 應用程式</span><span class="sxs-lookup"><span data-stu-id="f9a8e-342">Task 3 – Configuring the SignalR Application</span></span>

<span data-ttu-id="f9a8e-343">在這個工作中，您將設定**玩家測驗**連接到 SQL Server 後擋板。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-343">In this task, you will configure **Geek Quiz** to connect to the SQL Server backplane.</span></span> <span data-ttu-id="f9a8e-344">您會先加入**SignalR.SqlServer** NuGet 套件和設定的連接字串至後擋板資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-344">You will first add the **SignalR.SqlServer** NuGet package and set the connection string to your backplane database.</span></span>

1. <span data-ttu-id="f9a8e-345">開啟**Package Manager Console**從**工具** | **程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-345">Open the **Package Manager Console** from **Tools** | **Library Package Manager**.</span></span> <span data-ttu-id="f9a8e-346">請確定**GeekQuiz**中選取專案**預設專案**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-346">Make sure that **GeekQuiz** project is selected in the **Default project** drop-down list.</span></span> <span data-ttu-id="f9a8e-347">輸入下列命令以安裝**Microsoft.AspNet.SignalR.SqlServer** NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-347">Type the following command to install the **Microsoft.AspNet.SignalR.SqlServer** NuGet package.</span></span>

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. <span data-ttu-id="f9a8e-348">重複上一個步驟，但這次專案**GeekQuiz2**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-348">Repeat the previous step but this time for project **GeekQuiz2**.</span></span>
3. <span data-ttu-id="f9a8e-349">若要設定 SQL Server 後擋板，開啟**Startup.cs**檔案**GeekQuiz**專案，並將下列程式碼加入**設定**方法。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-349">To configure the SQL Server backplane, open the **Startup.cs** file of the **GeekQuiz** project and add the following code to the **Configure** method.</span></span> <span data-ttu-id="f9a8e-350">取代**&lt;您資料庫&gt;**與您建立 SQL Server 後擋板時所使用的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-350">Replace **&lt;YOUR-DATABASE&gt;** with your database name you used when creating the SQL Server backplane.</span></span> <span data-ttu-id="f9a8e-351">重複這個步驟**GeekQuiz2**專案。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-351">Repeat this step for the **GeekQuiz2** project.</span></span>

    <span data-ttu-id="f9a8e-352">(程式碼片段- *RealTimeSignalR-Ex2-StartupConfiguration*)</span><span class="sxs-lookup"><span data-stu-id="f9a8e-352">(Code Snippet - *RealTimeSignalR - Ex2 - StartupConfiguration*)</span></span>

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. <span data-ttu-id="f9a8e-353">現在，這兩個專案都設定為使用 SQL Server 後擋板，按**F5**同時執行它們。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-353">Now that both projects are configured to use the SQL Server backplane, press **F5** to run them simultaneously.</span></span>
5. <span data-ttu-id="f9a8e-354">同樣地， **Visual Studio**將會啟動兩個執行個體**玩家測驗**在不同的連接埠。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-354">Again, **Visual Studio** will launch two instances of **Geek Quiz** in different ports.</span></span> <span data-ttu-id="f9a8e-355">釘選的瀏覽器，在左方和其他螢幕的右側，並登入您的認證。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-355">Pin one of the browsers on the left and the other on the right of your screen and log in with your credentials.</span></span> <span data-ttu-id="f9a8e-356">保留瑣事頁面左側，並移至**統計資料**pagein 右邊的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-356">Keep the Trivia page on the left and go to **Statistics** pagein the right browser.</span></span>
6. <span data-ttu-id="f9a8e-357">啟動回答左邊的瀏覽器中的問題。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-357">Start answering questions in the left browser.</span></span> <span data-ttu-id="f9a8e-358">這次**統計資料**這點受惠後擋板，已更新頁面。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-358">This time, the **Statistics** page is updated thanks to the backplane.</span></span> <span data-ttu-id="f9a8e-359">應用程式之間切換 (**統計資料**現在位於左邊，和**瑣事**位於右邊)，並重複測試，以驗證它是否運作的兩個執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-359">Switch between applications (**Statistics** is now on the left, and **Trivia** is on the right) and repeat the test to validate that it is working for both instances.</span></span> <span data-ttu-id="f9a8e-360">後擋板做為*共用快取*訊息的每個連接的伺服器，且每個伺服器會將訊息儲存在他們自己的本機快取，以發佈至連線的用戶端。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-360">The backplane serves as a *shared cache* of messages for each connected server, and each server will store the messages in their own local cache to distribute to connected clients.</span></span>
7. <span data-ttu-id="f9a8e-361">返回 Visual Studio，並停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-361">Go back to Visual Studio and stop debugging.</span></span>
8. <span data-ttu-id="f9a8e-362">SQL Server 後擋板元件會自動產生所需的資料表上指定的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-362">The SQL Server backplane component automatically generates the necessary tables on the specified database.</span></span> <span data-ttu-id="f9a8e-363">在**SQL Server 物件總管** 面板中，開啟您的後擋板，已建立的資料庫 (例如： SignalR) 並展開其資料表。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-363">In the **SQL Server Object Explorer** panel, open the database you created for the backplane (e.g.: SignalR) and expand its tables.</span></span> <span data-ttu-id="f9a8e-364">您應該會看到下列資料表：</span><span class="sxs-lookup"><span data-stu-id="f9a8e-364">You should see the following tables:</span></span>

    ![後擋板，已產生的資料表](real-time-web-applications-with-signalr/_static/image27.png)

    <span data-ttu-id="f9a8e-366">*後擋板，已產生的資料表*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-366">*Backplane Generated Tables*</span></span>
9. <span data-ttu-id="f9a8e-367">以滑鼠右鍵按一下**SignalR.Messages\_0**資料表，然後選取**檢視資料**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-367">Right-click the **SignalR.Messages\_0** table and select **View Data**.</span></span>

    ![檢視 SignalR 後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image28.png)

    <span data-ttu-id="f9a8e-369">*檢視 SignalR 後擋板訊息資料表*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-369">*View SignalR Backplane Messages Table*</span></span>
10. <span data-ttu-id="f9a8e-370">您可以看到不同的訊息傳送至**中樞**回答瑣事問題時。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-370">You can see the different messages sent to the **Hub** when answering the trivia questions.</span></span> <span data-ttu-id="f9a8e-371">後擋板會將這些訊息，任何連接的執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-371">The backplane distributes these messages to any connected instance.</span></span>

    ![後擋板訊息資料表](real-time-web-applications-with-signalr/_static/image29.png)

    <span data-ttu-id="f9a8e-373">*後擋板訊息資料表*</span><span class="sxs-lookup"><span data-stu-id="f9a8e-373">*Backplane Messages Table*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f9a8e-374">總結</span><span class="sxs-lookup"><span data-stu-id="f9a8e-374">Summary</span></span>

<span data-ttu-id="f9a8e-375">在這個實際操作實驗室中，您已經學會如何新增**SignalR**您應用程式，並將傳送通知，從伺服器以使用您已連線的用戶端**集線器**。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-375">In this hands-on lab, you have learned how to add **SignalR** to your application and send notifications from the server to your connected clients using **Hubs**.</span></span> <span data-ttu-id="f9a8e-376">此外，您學會了如何以擴充您的應用程式使用*後擋板*元件時，您的應用程式部署在多個 IIS 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f9a8e-376">Additionally, you learned how to scale out your application by using a *backplane* component when your application is deployed in multiple IIS instances.</span></span>
