---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: 建置 RESTful 應用程式開發介面使用 ASP.NET Web API |Microsoft 文件
author: rick-anderson
description: 近年來，它已經變成清除不是 HTTP： 只能用來提供 HTML 網頁。 這也是功能強大的平台，來建置 Web Api，使用少數 o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306880"
---
<a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="d77a8-104">建置 RESTful 應用程式開發介面使用 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="d77a8-104">Build RESTful APIs with ASP.NET Web API</span></span>
====================
<span data-ttu-id="d77a8-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d77a8-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="d77a8-106">近年來，它已經變成清除不是 HTTP： 只能用來提供 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="d77a8-106">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="d77a8-107">它也是功能強大的平台，來建置 Web Api，使用少數幾個動詞命令 （GET、 POST 等） 再加上一些簡單的概念，例如*Uri*和*標頭*。</span><span class="sxs-lookup"><span data-stu-id="d77a8-107">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="d77a8-108">ASP.NET Web API 是一組簡化 HTTP 程式設計的元件。</span><span class="sxs-lookup"><span data-stu-id="d77a8-108">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="d77a8-109">因為它內建 ASP.NET MVC 執行階段，Web 應用程式開發介面會自動處理 HTTP 的傳輸低階詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d77a8-109">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="d77a8-110">同時，Web API 自然會公開 HTTP 程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="d77a8-110">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="d77a8-111">事實上，Web API 的其中一個目標是要*不*提取出的 HTTP 狀況。</span><span class="sxs-lookup"><span data-stu-id="d77a8-111">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="d77a8-112">如此一來，Web API 是彈性且易於擴充。</span><span class="sxs-lookup"><span data-stu-id="d77a8-112">As a result, Web API is both flexible and easy to extend.</span></span> <span data-ttu-id="d77a8-113">在這個實際操作實驗室中，您將建置簡單的 REST API 連絡人管理員應用程式使用 Web API。</span><span class="sxs-lookup"><span data-stu-id="d77a8-113">In this hands-on lab, you will use Web API to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="d77a8-114">您也將建置用戶端使用的 API。</span><span class="sxs-lookup"><span data-stu-id="d77a8-114">You will also build a client to consume the API.</span></span> <span data-ttu-id="d77a8-115">Rest 架構已證明是利用 HTTP-有效率的方式，雖然不一定唯一有效的方法，為 HTTP。</span><span class="sxs-lookup"><span data-stu-id="d77a8-115">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="d77a8-116">連絡人的管理員會公開 RESTful 清單、 加入和移除連絡人和其他項目。</span><span class="sxs-lookup"><span data-stu-id="d77a8-116">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> <span data-ttu-id="d77a8-117">這個實驗室需要基本的了解 HTTP，其餘部分，而且假設您有基本的 HTML、 JavaScript 和 jQuery 的實用知識。</span><span class="sxs-lookup"><span data-stu-id="d77a8-117">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="d77a8-118">ASP.NET 網站有一個專門用來在 ASP.NET Web API framework 的區域[ https://asp.net/web-api ](https://asp.net/web-api)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-118">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="d77a8-119">此站台會繼續以提供最新的資訊、 範例和新聞與 Web 應用程式開發介面，因此請經常如果您想要更深入地建立自訂 Web 應用程式開發介面使用幾乎任何裝置或開發架構的封面鑽研。</span><span class="sxs-lookup"><span data-stu-id="d77a8-119">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="d77a8-120">ASP.NET Web API，類似於 ASP.NET MVC 4，已分離可讓您使用數個可用的相依性插入架構相當簡單的控制站的服務層的絕佳彈性。</span><span class="sxs-lookup"><span data-stu-id="d77a8-120">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="d77a8-121">沒有良好的範例，示範如何使用 ASP.NET Web API 專案中，您可以從下載的相依性插入 Ninject 的 MSDN 中[這裡](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-121">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="d77a8-122">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-122">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d77a8-123">目標</span><span class="sxs-lookup"><span data-stu-id="d77a8-123">Objectives</span></span>

<span data-ttu-id="d77a8-124">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="d77a8-124">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d77a8-125">實作 rest 式 Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="d77a8-125">Implement a RESTful Web API</span></span>
- <span data-ttu-id="d77a8-126">從 HTML 用戶端呼叫 API</span><span class="sxs-lookup"><span data-stu-id="d77a8-126">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d77a8-127">必要條件</span><span class="sxs-lookup"><span data-stu-id="d77a8-127">Prerequisites</span></span>

<span data-ttu-id="d77a8-128">需要下列項目才能完成這個實際操作實驗室：</span><span class="sxs-lookup"><span data-stu-id="d77a8-128">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="d77a8-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 B](#AppendixB)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d77a8-130">安裝程式</span><span class="sxs-lookup"><span data-stu-id="d77a8-130">Setup</span></span>

<span data-ttu-id="d77a8-131">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="d77a8-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="d77a8-132">為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d77a8-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="d77a8-133">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="d77a8-134">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。</span><span class="sxs-lookup"><span data-stu-id="d77a8-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d77a8-135">練習</span><span class="sxs-lookup"><span data-stu-id="d77a8-135">Exercises</span></span>

<span data-ttu-id="d77a8-136">這個實際操作實驗室包含下列練習中：</span><span class="sxs-lookup"><span data-stu-id="d77a8-136">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="d77a8-137">練習 1： 建立唯讀 Web API</span><span class="sxs-lookup"><span data-stu-id="d77a8-137">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="d77a8-138">練習 2： 建立讀取/寫入 Web API</span><span class="sxs-lookup"><span data-stu-id="d77a8-138">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="d77a8-139">練習 3： 使用 Web API，從 HTML 用戶端</span><span class="sxs-lookup"><span data-stu-id="d77a8-139">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="d77a8-140">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="d77a8-140">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="d77a8-141">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="d77a8-141">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="d77a8-142">完成本實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-142">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="d77a8-143">練習 1： 建立唯讀 Web API</span><span class="sxs-lookup"><span data-stu-id="d77a8-143">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="d77a8-144">在此練習中，您將連絡人的 manager 實作唯讀 GET 方法。</span><span class="sxs-lookup"><span data-stu-id="d77a8-144">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="d77a8-145">工作 1-建立 API 專案</span><span class="sxs-lookup"><span data-stu-id="d77a8-145">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="d77a8-146">在這項工作，您將使用新的 ASP.NET web 專案範本建立 Web API 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-146">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="d77a8-147">執行**Visual Studio 2012 Express for Web**，請移至要**啟動**和型別**VS Express for Web**然後按下**Enter**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-147">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="d77a8-148">從**檔案**功能表上，選取**新專案**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-148">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="d77a8-149">選取**Visual C# |Web**專案類型，從 專案類型樹狀檢視中，然後選取  **ASP.NET MVC 4 Web 應用程式**專案類型。</span><span class="sxs-lookup"><span data-stu-id="d77a8-149">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="d77a8-150">將專案的**名稱**至*ContactManager*和**方案名稱**至*開始*，然後按一下 **確定**.</span><span class="sxs-lookup"><span data-stu-id="d77a8-150">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="d77a8-151">![建立新的 ASP.NET MVC 4.0 Web 應用程式專案](build-restful-apis-with-aspnet-web-api/_static/image1.png "建立新的 ASP.NET MVC 4.0 Web 應用程式專案")</span><span class="sxs-lookup"><span data-stu-id="d77a8-151">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="d77a8-152">*建立新的 ASP.NET MVC 4.0 Web 應用程式專案*</span><span class="sxs-lookup"><span data-stu-id="d77a8-152">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="d77a8-153">在 ASP.NET MVC 4 專案的 [類型] 對話方塊中，選取**Web API**專案類型。</span><span class="sxs-lookup"><span data-stu-id="d77a8-153">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="d77a8-154">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="d77a8-154">Click **OK**.</span></span>

    <span data-ttu-id="d77a8-155">![指定 Web API 專案類型](build-restful-apis-with-aspnet-web-api/_static/image2.png "指定 Web API 專案類型")</span><span class="sxs-lookup"><span data-stu-id="d77a8-155">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="d77a8-156">*指定 Web API 專案類型*</span><span class="sxs-lookup"><span data-stu-id="d77a8-156">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="d77a8-157">工作 2-建立連絡人管理員應用程式開發介面控制站</span><span class="sxs-lookup"><span data-stu-id="d77a8-157">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="d77a8-158">在這項工作，您將建立應用程式開發介面方法所在的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="d77a8-158">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="d77a8-159">刪除名為檔案**ValuesController.cs**內**控制器**從專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d77a8-159">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="d77a8-160">以滑鼠右鍵按一下**控制器**中專案，然後選取資料夾**新增 |控制器**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="d77a8-160">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="d77a8-161">![專案中加入新的控制站](build-restful-apis-with-aspnet-web-api/_static/image3.png "專案中加入新的控制站")</span><span class="sxs-lookup"><span data-stu-id="d77a8-161">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="d77a8-162">*將新的控制站新增至專案*</span><span class="sxs-lookup"><span data-stu-id="d77a8-162">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="d77a8-163">在**加入控制器**出現對話方塊，選取**空白的 API 控制器**從 [範本] 功能表。</span><span class="sxs-lookup"><span data-stu-id="d77a8-163">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="d77a8-164">將控制器類別**ContactController**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-164">Name the controller class **ContactController**.</span></span> <span data-ttu-id="d77a8-165">然後，按一下 **新增。**</span><span class="sxs-lookup"><span data-stu-id="d77a8-165">Then, click **Add.**</span></span>

    <span data-ttu-id="d77a8-166">![使用 [加入控制器] 對話方塊建立新的 Web API 控制器](build-restful-apis-with-aspnet-web-api/_static/image4.png "使用加入控制器 對話方塊來建立新的 Web API 控制器")</span><span class="sxs-lookup"><span data-stu-id="d77a8-166">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="d77a8-167">*使用 [加入控制器] 對話方塊建立新的 Web API 控制器*</span><span class="sxs-lookup"><span data-stu-id="d77a8-167">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="d77a8-168">將下列程式碼加入**ContactController**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-168">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="d77a8-169">(程式碼片段- *Web API 實驗室-Ex01-取得應用程式開發介面方法*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-169">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="d77a8-170">按**F5**偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-170">Press **F5** to debug the application.</span></span> <span data-ttu-id="d77a8-171">預設的首頁 Web API 專案應該會出現。</span><span class="sxs-lookup"><span data-stu-id="d77a8-171">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="d77a8-172">![ASP.NET Web API 應用程式的預設首頁](build-restful-apis-with-aspnet-web-api/_static/image5.png "ASP.NET Web API 應用程式的預設首頁")</span><span class="sxs-lookup"><span data-stu-id="d77a8-172">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="d77a8-173">*ASP.NET Web API 應用程式的預設首頁*</span><span class="sxs-lookup"><span data-stu-id="d77a8-173">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="d77a8-174">在 Internet Explorer 視窗中，按**F12**鍵開啟**開發人員工具**視窗。</span><span class="sxs-lookup"><span data-stu-id="d77a8-174">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="d77a8-175">按一下**網路**索引標籤，然後再按一下**開始擷取** 按鈕，開始到視窗擷取網路流量。</span><span class="sxs-lookup"><span data-stu-id="d77a8-175">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="d77a8-176">![開啟 [網路] 索引標籤，然後啟動網路擷取](build-restful-apis-with-aspnet-web-api/_static/image6.png "開啟 [網路] 索引標籤，然後啟動網路擷取")</span><span class="sxs-lookup"><span data-stu-id="d77a8-176">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="d77a8-177">*開啟 [網路] 索引標籤，然後啟動網路擷取*</span><span class="sxs-lookup"><span data-stu-id="d77a8-177">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="d77a8-178">附加與瀏覽器的網址列中的 URL **/api/連絡人**然後按 enter 鍵。</span><span class="sxs-lookup"><span data-stu-id="d77a8-178">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="d77a8-179">傳輸的詳細資料會出現在網路的 [擷取] 視窗中。</span><span class="sxs-lookup"><span data-stu-id="d77a8-179">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="d77a8-180">請注意，回應的 MIME 類型為**應用程式 /json**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-180">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="d77a8-181">這將示範如何將預設輸出格式為 JSON。</span><span class="sxs-lookup"><span data-stu-id="d77a8-181">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="d77a8-182">![在 [網路] 檢視中檢視 Web API 要求的輸出](build-restful-apis-with-aspnet-web-api/_static/image7.png "[網路] 檢視中檢視的 Web API 要求的輸出")</span><span class="sxs-lookup"><span data-stu-id="d77a8-182">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="d77a8-183">*在 [網路] 檢視中檢視 Web API 要求的輸出*</span><span class="sxs-lookup"><span data-stu-id="d77a8-183">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-184">此時 Internet Explorer 10 的預設行為將會詢問使用者是否想要儲存或開啟 Web API 呼叫所產生的資料流。</span><span class="sxs-lookup"><span data-stu-id="d77a8-184">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="d77a8-185">輸出會包含 Web API URL 呼叫的 JSON 結果的文字檔。</span><span class="sxs-lookup"><span data-stu-id="d77a8-185">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="d77a8-186">不要取消對話方塊才能觀看回應的內容，透過開發人員工具視窗。</span><span class="sxs-lookup"><span data-stu-id="d77a8-186">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="d77a8-187">按一下**移至 詳細檢視**按鈕可查看有關此應用程式開發介面呼叫的回應詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d77a8-187">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="d77a8-188">![切換至 詳細檢視](build-restful-apis-with-aspnet-web-api/_static/image8.png "切換至 詳細資料檢視")</span><span class="sxs-lookup"><span data-stu-id="d77a8-188">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="d77a8-189">*切換至 詳細檢視*</span><span class="sxs-lookup"><span data-stu-id="d77a8-189">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="d77a8-190">按一下**回應主體**若要檢視實際的 JSON 回應文字 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d77a8-190">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="d77a8-191">![網路監視器中的文字檢視 JSON 輸出](build-restful-apis-with-aspnet-web-api/_static/image9.png "網路監視器中的文字檢視 JSON 輸出")</span><span class="sxs-lookup"><span data-stu-id="d77a8-191">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="d77a8-192">*網路監視器中檢視 JSON 輸出文字*</span><span class="sxs-lookup"><span data-stu-id="d77a8-192">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="d77a8-193">工作 3-建立連絡人的模型，將它擴大連絡控制器</span><span class="sxs-lookup"><span data-stu-id="d77a8-193">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="d77a8-194">在這項工作，您將建立應用程式開發介面方法所在的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="d77a8-194">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="d77a8-195">以滑鼠右鍵按一下**模型**資料夾，然後選取**新增 |類別...** 從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="d77a8-195">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="d77a8-196">![將新模型加入至 web 應用程式](build-restful-apis-with-aspnet-web-api/_static/image10.png "將新模型加入至 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="d77a8-196">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="d77a8-197">*將新模型加入至 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="d77a8-197">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="d77a8-198">在**加入新項目**對話方塊中，將新的檔案命名**Contact.cs**按一下**新增。**</span><span class="sxs-lookup"><span data-stu-id="d77a8-198">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="d77a8-199">![建立新的連絡人類別檔案](build-restful-apis-with-aspnet-web-api/_static/image11.png "建立新的連絡人類別檔案")</span><span class="sxs-lookup"><span data-stu-id="d77a8-199">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="d77a8-200">*建立新的連絡人類別檔案*</span><span class="sxs-lookup"><span data-stu-id="d77a8-200">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="d77a8-201">將下列反白顯示的程式碼加入**連絡人**類別。</span><span class="sxs-lookup"><span data-stu-id="d77a8-201">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="d77a8-202">(程式碼片段- *Web API 實驗室-Ex01-連絡人類別*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-202">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="d77a8-203">在**ContactController**類別中，選取 word**字串**方法定義中**取得**方法，然後輸入這個字*連絡人*。</span><span class="sxs-lookup"><span data-stu-id="d77a8-203">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="d77a8-204">一旦這個字型別中，指標就會出現這個字開頭**連絡人**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-204">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="d77a8-205">請按住**Ctrl**索引鍵並按下句號 （.） 索引鍵或按一下圖示以開啟 [協助] 對話方塊，在程式碼編輯器中，以自動填入使用滑鼠**使用**模型指示詞命名空間。</span><span class="sxs-lookup"><span data-stu-id="d77a8-205">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![命名空間宣告中使用 Intellisense 協助](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="d77a8-207">*命名空間宣告中使用 Intellisense 協助*</span><span class="sxs-lookup"><span data-stu-id="d77a8-207">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="d77a8-208">修改的程式碼**取得**方法，使它傳回連絡人模型執行個體的陣列。</span><span class="sxs-lookup"><span data-stu-id="d77a8-208">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="d77a8-209">(程式碼片段-*傳回的連絡人清單 Web API 實驗室-Ex01-*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-209">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="d77a8-210">按**F5**偵錯 web 應用程式，在瀏覽器中的。</span><span class="sxs-lookup"><span data-stu-id="d77a8-210">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="d77a8-211">若要檢視應用程式開發介面的回應輸出所做的變更，請執行下列步驟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-211">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="d77a8-212">瀏覽器會開啟之後, 按**F12**如果開發人員工具還未開啟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-212">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="d77a8-213">按一下**網路** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d77a8-213">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="d77a8-214">按**開始擷取** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-214">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="d77a8-215">新增 URL 尾碼 **/api/連絡人**在位址列中按的 url **Enter**索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d77a8-215">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="d77a8-216">按**移至 [詳細檢視**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-216">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="d77a8-217">選取**回應主體** 索引標籤。您應該會看到代表序列化的形式的連絡人執行個體陣列的 JSON 字串。</span><span class="sxs-lookup"><span data-stu-id="d77a8-217">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="d77a8-218">![JSON 序列化的複雜的 Web API 方法呼叫輸出](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON 序列化複雜的 Web API 方法呼叫的輸出")</span><span class="sxs-lookup"><span data-stu-id="d77a8-218">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="d77a8-219">*複雜的 Web API 方法呼叫的序列化的 JSON 輸出*</span><span class="sxs-lookup"><span data-stu-id="d77a8-219">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="d77a8-220">工作 4-解壓縮至服務層的功能</span><span class="sxs-lookup"><span data-stu-id="d77a8-220">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="d77a8-221">此工作將會示範如何擷取到服務層，以方便開發人員在其服務功能分開控制器圖層，藉此讓人重複使用之服務的實際執行工作的功能。</span><span class="sxs-lookup"><span data-stu-id="d77a8-221">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="d77a8-222">在方案根目錄中建立新的資料夾並將其命名**服務**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-222">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="d77a8-223">若要這樣做，請以滑鼠右鍵按一下**ContactManager**專案，然後選取**新增** | **新資料夾**，其命名*服務*。</span><span class="sxs-lookup"><span data-stu-id="d77a8-223">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="d77a8-224">![建立服務 資料夾](build-restful-apis-with-aspnet-web-api/_static/image14.png "建立服務資料夾")</span><span class="sxs-lookup"><span data-stu-id="d77a8-224">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="d77a8-225">*建立服務 資料夾*</span><span class="sxs-lookup"><span data-stu-id="d77a8-225">*Creating Services folder*</span></span>
2. <span data-ttu-id="d77a8-226">以滑鼠右鍵按一下**服務**資料夾，然後選取**新增 |類別...** 從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="d77a8-226">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="d77a8-227">![將新類別加入至 [服務] 資料夾](build-restful-apis-with-aspnet-web-api/_static/image15.png "將新類別加入至 [服務] 資料夾")</span><span class="sxs-lookup"><span data-stu-id="d77a8-227">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="d77a8-228">*將新類別加入至 [服務] 資料夾*</span><span class="sxs-lookup"><span data-stu-id="d77a8-228">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="d77a8-229">當**加入新項目**對話方塊隨即出現，將新類別**ContactRepository**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-229">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="d77a8-230">![建立包含連絡人儲存機制的服務層的程式碼的類別檔案](build-restful-apis-with-aspnet-web-api/_static/image16.png "建立要包含連絡人儲存機制的服務層的程式碼的類別檔案")</span><span class="sxs-lookup"><span data-stu-id="d77a8-230">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="d77a8-231">*建立包含連絡人儲存機制的服務層的程式碼的類別檔案*</span><span class="sxs-lookup"><span data-stu-id="d77a8-231">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="d77a8-232">加入 using 指示詞加入**ContactRepository.cs**檔案以包含模型命名空間。</span><span class="sxs-lookup"><span data-stu-id="d77a8-232">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="d77a8-233">將下列反白顯示的程式碼加入**ContactRepository.cs**檔案，以實作 GetAllContacts 方法。</span><span class="sxs-lookup"><span data-stu-id="d77a8-233">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="d77a8-234">(程式碼片段- *Web API 實驗室-Ex01-連絡人儲存機制*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-234">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="d77a8-235">開啟**ContactController.cs**如果還沒有開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-235">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="d77a8-236">加入下列 using 陳述式之檔案的命名空間宣告區段。</span><span class="sxs-lookup"><span data-stu-id="d77a8-236">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="d77a8-237">將下列反白顯示的程式碼加入**ContactController.cs**新增私用欄位來代表執行個體的儲存機制，讓成員可以建立的類別的其餘部分使用的服務實作的類別。</span><span class="sxs-lookup"><span data-stu-id="d77a8-237">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="d77a8-238">(程式碼片段- *Web API 實驗室-Ex01-連絡人控制器*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-238">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="d77a8-239">變更**取得**方法，使它可讓使用連絡人儲存機制的服務。</span><span class="sxs-lookup"><span data-stu-id="d77a8-239">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="d77a8-240">(程式碼片段-*透過儲存機制傳回的連絡人清單 Web API 實驗室-Ex01-*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-240">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="d77a8-241">將中斷點放**ContactController**的**取得**方法定義。</span><span class="sxs-lookup"><span data-stu-id="d77a8-241">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="d77a8-242">![將中斷點加入至連絡人控制器](build-restful-apis-with-aspnet-web-api/_static/image17.png "連絡人的控制站加入中斷點")</span><span class="sxs-lookup"><span data-stu-id="d77a8-242">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="d77a8-243">*連絡人的控制站加入中斷點*</span><span class="sxs-lookup"><span data-stu-id="d77a8-243">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="d77a8-244">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-244">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="d77a8-245">當瀏覽器會開啟時，請按**F12**以開啟 開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="d77a8-245">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="d77a8-246">按一下**網路** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d77a8-246">Click the **Network** tab.</span></span>
14. <span data-ttu-id="d77a8-247">按一下**開始擷取** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-247">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="d77a8-248">附加後置詞，在網址列中的 URL **/api/連絡人**按**Enter**載入 API 控制器。</span><span class="sxs-lookup"><span data-stu-id="d77a8-248">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="d77a8-249">Visual Studio 2012 應該中斷一次**取得**方法就會開始執行。</span><span class="sxs-lookup"><span data-stu-id="d77a8-249">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="d77a8-250">![取得方法內中斷](build-restful-apis-with-aspnet-web-api/_static/image18.png "Get 方法內中斷")</span><span class="sxs-lookup"><span data-stu-id="d77a8-250">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="d77a8-251">*取得方法內中斷*</span><span class="sxs-lookup"><span data-stu-id="d77a8-251">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="d77a8-252">按 **F5** 繼續。</span><span class="sxs-lookup"><span data-stu-id="d77a8-252">Press **F5** to continue.</span></span>
18. <span data-ttu-id="d77a8-253">回到 Internet Explorer 如果還沒有焦點。</span><span class="sxs-lookup"><span data-stu-id="d77a8-253">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="d77a8-254">請注意網路擷取視窗。</span><span class="sxs-lookup"><span data-stu-id="d77a8-254">Note the network capture window.</span></span>

    <span data-ttu-id="d77a8-255">![網路在 Internet Explorer 中顯示 Web 應用程式開發介面呼叫的結果檢視](build-restful-apis-with-aspnet-web-api/_static/image19.png "網路在 Internet Explorer 中顯示 Web 應用程式開發介面呼叫的結果檢視")</span><span class="sxs-lookup"><span data-stu-id="d77a8-255">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="d77a8-256">*在 Internet Explorer 顯示 Web 應用程式開發介面呼叫的結果中的網路檢視*</span><span class="sxs-lookup"><span data-stu-id="d77a8-256">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="d77a8-257">按一下**移至 [詳細檢視**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-257">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="d77a8-258">按一下**回應主體** 索引標籤。請注意 JSON 輸出的 API，以及它如何表示兩個服務層所擷取的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d77a8-258">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="d77a8-259">![在 [開發人員工具] 視窗中檢視 Web API 的 JSON 輸出](build-restful-apis-with-aspnet-web-api/_static/image20.png "開發人員工具 視窗中檢視 Web API 的 JSON 輸出")</span><span class="sxs-lookup"><span data-stu-id="d77a8-259">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="d77a8-260">*在 [開發人員工具] 視窗中檢視 Web API 的 JSON 輸出*</span><span class="sxs-lookup"><span data-stu-id="d77a8-260">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="d77a8-261">練習 2： 建立讀取/寫入 Web API</span><span class="sxs-lookup"><span data-stu-id="d77a8-261">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="d77a8-262">在此練習中，您將實作 POST 和 PUT 方法連絡管理員以啟用資料編輯功能。</span><span class="sxs-lookup"><span data-stu-id="d77a8-262">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="d77a8-263">工作 1-開啟 Web API 專案</span><span class="sxs-lookup"><span data-stu-id="d77a8-263">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="d77a8-264">在這項工作，您將準備增強，因此可接受使用者輸入，練習 1 中建立的 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-264">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="d77a8-265">執行**Visual Studio 2012 Express for Web**，請移至要**啟動**和型別**VS Express for Web**然後按下**Enter**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-265">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="d77a8-266">開啟**開始**方案位於**來源/Ex02ReadWriteWebAPI/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d77a8-266">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="d77a8-267">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="d77a8-267">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d77a8-268">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="d77a8-268">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d77a8-269">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-269">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d77a8-270">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="d77a8-270">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d77a8-271">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-271">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d77a8-272">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="d77a8-272">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d77a8-273">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-273">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d77a8-274">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-274">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d77a8-275">開啟**Services/ContactRepository.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-275">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="d77a8-276">工作 2-加入連絡人儲存機制實作的資料持續性功能</span><span class="sxs-lookup"><span data-stu-id="d77a8-276">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="d77a8-277">在這項工作，您將會增加 ContactRepository 類別，因此可以保存並接受使用者輸入和新的連絡人執行個體，練習 1 中建立的 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-277">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="d77a8-278">新增下列常數**ContactRepository**類別來代表 web 伺服器快取項目索引鍵名稱稍後在這個練習中的名稱。</span><span class="sxs-lookup"><span data-stu-id="d77a8-278">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="d77a8-279">新增的建構函式**ContactRepository**包含下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="d77a8-279">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="d77a8-280">(程式碼片段- *Web API 實驗室-Ex02-連絡人儲存機制的建構函式*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-280">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="d77a8-281">修改的程式碼**GetAllContacts**依照以下所示的方法。</span><span class="sxs-lookup"><span data-stu-id="d77a8-281">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="d77a8-282">(程式碼片段- *Web API 實驗室-Ex02-取得所有連絡人*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-282">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="d77a8-283">這個範例是為了示範之用，並會為儲存體中使用 web 伺服器的快取，以便值將同時適用於多個用戶端，而不使用工作階段儲存機制或要求儲存體存留期。</span><span class="sxs-lookup"><span data-stu-id="d77a8-283">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="d77a8-284">其中一個可以使用 Entity Framework、 XML 儲存體或任何其他各種取代 web 伺服器快取。</span><span class="sxs-lookup"><span data-stu-id="d77a8-284">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="d77a8-285">實作新方法，名為**SaveContact**至**ContactRepository**類別來儲存連絡人的工作。</span><span class="sxs-lookup"><span data-stu-id="d77a8-285">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="d77a8-286">**SaveContact**方法應該採用單一**連絡人**參數和傳回布林值表示成功或失敗。</span><span class="sxs-lookup"><span data-stu-id="d77a8-286">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="d77a8-287">(程式碼片段- *Web API 實驗室-Ex02-實作 SaveContact 方法*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-287">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="d77a8-288">練習 3： 使用 Web API，從 HTML 用戶端</span><span class="sxs-lookup"><span data-stu-id="d77a8-288">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="d77a8-289">在此練習中，您將建立 HTML 用戶端呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="d77a8-289">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="d77a8-290">此用戶端會協助使用 JavaScript 的 Web api 的資料交換，並會使用 HTML 標記的 web 瀏覽器中顯示的結果。</span><span class="sxs-lookup"><span data-stu-id="d77a8-290">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="d77a8-291">工作 1-修改索引檢視，以提供 GUI 顯示連絡人</span><span class="sxs-lookup"><span data-stu-id="d77a8-291">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="d77a8-292">在這個工作中，您將修改成可支援在 HTML 瀏覽器中顯示現有的連絡人清單需求的 web 應用程式的預設索引檢視。</span><span class="sxs-lookup"><span data-stu-id="d77a8-292">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="d77a8-293">開啟**Visual Studio 2012 Express for Web**如果它尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-293">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="d77a8-294">開啟**開始**方案位於**來源/Ex03ConsumingWebAPI/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d77a8-294">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="d77a8-295">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="d77a8-295">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="d77a8-296">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="d77a8-296">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="d77a8-297">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-297">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="d77a8-298">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="d77a8-298">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="d77a8-299">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-299">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d77a8-300">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="d77a8-300">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="d77a8-301">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-301">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="d77a8-302">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-302">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="d77a8-303">開啟**Index.cshtml**檔案位於**Views/Home**資料夾。</span><span class="sxs-lookup"><span data-stu-id="d77a8-303">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="d77a8-304">Div 項目內的 HTML 程式碼取代為 id**主體**，讓它看起來像下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="d77a8-304">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="d77a8-305">加入下列 Javascript 程式碼檔案來執行 Web API 的 HTTP 要求的底部。</span><span class="sxs-lookup"><span data-stu-id="d77a8-305">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="d77a8-306">開啟**ContactController.cs**如果還沒有開啟檔案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-306">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="d77a8-307">將中斷點放在**取得**方法**ContactController**類別。</span><span class="sxs-lookup"><span data-stu-id="d77a8-307">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="d77a8-308">![Get 方法的 API 控制器上放置中斷點](build-restful-apis-with-aspnet-web-api/_static/image21.png "將中斷點放置在 Get 方法的 API 控制器")</span><span class="sxs-lookup"><span data-stu-id="d77a8-308">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="d77a8-309">*將中斷點放置在 Get 方法的 API 控制器*</span><span class="sxs-lookup"><span data-stu-id="d77a8-309">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="d77a8-310">按**F5**執行專案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-310">Press **F5** to run the project.</span></span> <span data-ttu-id="d77a8-311">瀏覽器將會載入 HTML 文件。</span><span class="sxs-lookup"><span data-stu-id="d77a8-311">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-312">請確定您在瀏覽至您的應用程式的根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="d77a8-312">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="d77a8-313">一旦載入頁面，而且 JavaScript 執行，會叫用中斷點，並執行程式碼將會暫停控制器中。</span><span class="sxs-lookup"><span data-stu-id="d77a8-313">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="d77a8-314">![使用 VS Express for Web 的 Web API 呼叫偵錯](build-restful-apis-with-aspnet-web-api/_static/image22.png "偵錯使用 VS Express for Web 的 Web API 呼叫")</span><span class="sxs-lookup"><span data-stu-id="d77a8-314">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="d77a8-315">*使用 Visual Studio 2012 Express for Web 的 Web 應用程式開發介面呼叫偵錯*</span><span class="sxs-lookup"><span data-stu-id="d77a8-315">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="d77a8-316">移除中斷點，並按**F5**或偵錯 工具列的**繼續**按鈕以繼續載入瀏覽器中的檢視。</span><span class="sxs-lookup"><span data-stu-id="d77a8-316">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="d77a8-317">Web 應用程式開發介面呼叫完成之後應該會看到呼叫為清單項目顯示在瀏覽器從 Web API 傳回的連絡人。</span><span class="sxs-lookup"><span data-stu-id="d77a8-317">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="d77a8-318">![在瀏覽器中顯示為清單項目 API 呼叫的結果](build-restful-apis-with-aspnet-web-api/_static/image23.png "瀏覽器中顯示為清單項目 API 呼叫的結果")</span><span class="sxs-lookup"><span data-stu-id="d77a8-318">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="d77a8-319">*在瀏覽器中顯示為清單項目 API 呼叫的結果*</span><span class="sxs-lookup"><span data-stu-id="d77a8-319">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="d77a8-320">停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="d77a8-320">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="d77a8-321">工作 2-修改索引檢視，以提供用於建立連絡人的 GUI</span><span class="sxs-lookup"><span data-stu-id="d77a8-321">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="d77a8-322">在這項工作，您將會繼續修改索引檢視的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-322">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="d77a8-323">表單將會加入至 HTML 頁面，會擷取使用者輸入，並將其傳送至 Web API 來建立新的連絡人，並將建立新的 Web API 控制器方法，以收集從 GUI 的日期。</span><span class="sxs-lookup"><span data-stu-id="d77a8-323">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="d77a8-324">開啟**ContactController.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-324">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="d77a8-325">將新方法加入至名為的控制器類別**Post**如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="d77a8-325">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="d77a8-326">(程式碼片段- *Web API 實驗室-Ex03-Post 方法*)</span><span class="sxs-lookup"><span data-stu-id="d77a8-326">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="d77a8-327">開啟**Index.cshtml**檔案在 Visual Studio 中，如果它尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-327">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="d77a8-328">將下列 HTML 程式碼加入檔案之後您加入先前工作中的未排序清單。</span><span class="sxs-lookup"><span data-stu-id="d77a8-328">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="d77a8-329">在底部的 文件的指令碼項目，加入下列反白顯示的程式碼，以處理按鈕 click 事件，將會張貼資料到 Web API 使用 HTTP POST 呼叫。</span><span class="sxs-lookup"><span data-stu-id="d77a8-329">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="d77a8-330">在**ContactController.cs**，將中斷點放在**Post**方法。</span><span class="sxs-lookup"><span data-stu-id="d77a8-330">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="d77a8-331">按**F5**瀏覽器中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-331">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="d77a8-332">一旦載入網頁瀏覽器中，輸入新的連絡人名稱和識別碼和按一下**儲存** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-332">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="d77a8-333">![用戶端 HTML 文件載入瀏覽器](build-restful-apis-with-aspnet-web-api/_static/image24.png "用戶端 HTML 文件載入瀏覽器")</span><span class="sxs-lookup"><span data-stu-id="d77a8-333">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="d77a8-334">*用戶端 HTML 文件載入瀏覽器*</span><span class="sxs-lookup"><span data-stu-id="d77a8-334">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="d77a8-335">偵錯工具視窗中的分行符號當**Post**方法的內容查看**連絡**參數。</span><span class="sxs-lookup"><span data-stu-id="d77a8-335">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="d77a8-336">值應該符合您在表單中輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="d77a8-336">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="d77a8-337">![從用戶端傳送至 Web API 的連絡人物件](build-restful-apis-with-aspnet-web-api/_static/image25.png "連絡人物件從用戶端傳送至 Web API")</span><span class="sxs-lookup"><span data-stu-id="d77a8-337">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="d77a8-338">*從用戶端傳送至 Web API 的連絡人物件*</span><span class="sxs-lookup"><span data-stu-id="d77a8-338">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="d77a8-339">逐步執行直到偵錯工具中的方法**回應**已經建立變數。</span><span class="sxs-lookup"><span data-stu-id="d77a8-339">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="d77a8-340">中的檢查時**區域變數**偵錯工具視窗中的，您會看到已設定的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="d77a8-340">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="d77a8-341">![下列偵錯工具中的建立的回應](build-restful-apis-with-aspnet-web-api/_static/image26.png "遵循偵錯工具中的建立的回應")</span><span class="sxs-lookup"><span data-stu-id="d77a8-341">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="d77a8-342">*下列偵錯工具中的建立的回應*</span><span class="sxs-lookup"><span data-stu-id="d77a8-342">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="d77a8-343">如果您按下**F5**或按一下**繼續**偵錯工具要求將會完成。</span><span class="sxs-lookup"><span data-stu-id="d77a8-343">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="d77a8-344">一旦您切換回瀏覽器，新的連絡人具有所儲存的連絡人清單中加入**ContactRepository**實作。</span><span class="sxs-lookup"><span data-stu-id="d77a8-344">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="d77a8-345">![瀏覽器會反映在成功建立新連絡人的執行個體](build-restful-apis-with-aspnet-web-api/_static/image27.png "瀏覽器會反映在成功建立新連絡人的執行個體")</span><span class="sxs-lookup"><span data-stu-id="d77a8-345">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="d77a8-346">*瀏覽器會反映在成功建立新連絡人的執行個體*</span><span class="sxs-lookup"><span data-stu-id="d77a8-346">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="d77a8-347">此外，您可以部署此應用程式以 Azure 下列[附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixC)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-347">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d77a8-348">總結</span><span class="sxs-lookup"><span data-stu-id="d77a8-348">Summary</span></span>

<span data-ttu-id="d77a8-349">這個實驗室已引進新的 ASP.NET Web API framework，並使用 framework RESTful Web Api 的實作。</span><span class="sxs-lookup"><span data-stu-id="d77a8-349">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="d77a8-350">從這裡開始，可以建立新的儲存機制，可促進使用任意數目的機制的資料持續性，而不是提供做為範例在這個實驗室中的簡單一個連接該服務。</span><span class="sxs-lookup"><span data-stu-id="d77a8-350">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="d77a8-351">Web 應用程式開發介面支援許多其他功能，例如啟用非 HTML 用戶端以支援 HTTP 和 JSON 或 XML 的任何語言撰寫的通訊。</span><span class="sxs-lookup"><span data-stu-id="d77a8-351">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="d77a8-352">裝載 Web API 一般 web 應用程式之外的能力也是可行的以及已建立您自己的序列化格式的能力。</span><span class="sxs-lookup"><span data-stu-id="d77a8-352">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="d77a8-353">ASP.NET 網站有一個專門用來在 ASP.NET Web API framework 的區域[ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-353">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="d77a8-354">此站台會繼續以提供最新的資訊、 範例和新聞與 Web 應用程式開發介面，因此請經常如果您想要更深入地建立自訂 Web 應用程式開發介面使用幾乎任何裝置或開發架構的封面鑽研。</span><span class="sxs-lookup"><span data-stu-id="d77a8-354">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="d77a8-355">附錄 a： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="d77a8-355">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="d77a8-356">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="d77a8-356">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="d77a8-357">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="d77a8-357">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="d77a8-358">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image28.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="d77a8-358">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="d77a8-359">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="d77a8-359">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="d77a8-360">若要加入的程式碼片段，使用鍵盤 （僅限 C#)</span><span class="sxs-lookup"><span data-stu-id="d77a8-360">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="d77a8-361">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d77a8-361">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="d77a8-362">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="d77a8-362">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="d77a8-363">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="d77a8-363">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="d77a8-364">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="d77a8-364">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="d77a8-365">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d77a8-365">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="d77a8-366">![開始輸入程式碼片段名稱](build-restful-apis-with-aspnet-web-api/_static/image29.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="d77a8-366">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="d77a8-367">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="d77a8-367">*Start typing the snippet name*</span></span>

    <span data-ttu-id="d77a8-368">![按 Tab 鍵選取反白顯示的程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image30.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="d77a8-368">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="d77a8-369">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="d77a8-369">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="d77a8-370">![按 Tab 鍵一次程式碼片段就會擴展](build-restful-apis-with-aspnet-web-api/_static/image31.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="d77a8-370">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="d77a8-371">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="d77a8-371">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="d77a8-372">若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）</span><span class="sxs-lookup"><span data-stu-id="d77a8-372">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="d77a8-373">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d77a8-373">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="d77a8-374">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-374">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="d77a8-375">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d77a8-375">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="d77a8-376">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image32.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="d77a8-376">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="d77a8-377">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="d77a8-377">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="d77a8-378">![透過在按一下挑選清單中，從相關的程式碼片段](build-restful-apis-with-aspnet-web-api/_static/image33.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="d77a8-378">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="d77a8-379">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="d77a8-379">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="d77a8-380">附錄 b： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="d77a8-380">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="d77a8-381">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="d77a8-381">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="d77a8-382">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="d77a8-382">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="d77a8-383">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-383">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="d77a8-384">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="d77a8-384">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="d77a8-385">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-385">Click on **Install Now**.</span></span> <span data-ttu-id="d77a8-386">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="d77a8-386">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="d77a8-387">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-387">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="d77a8-388">![安裝 Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="d77a8-388">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="d77a8-389">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="d77a8-389">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="d77a8-390">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="d77a8-390">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="d77a8-392">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="d77a8-392">*Accepting the license terms*</span></span>
5. <span data-ttu-id="d77a8-393">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="d77a8-393">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="d77a8-395">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="d77a8-395">*Installation progress*</span></span>
6. <span data-ttu-id="d77a8-396">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-396">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="d77a8-398">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="d77a8-398">*Installation completed*</span></span>
7. <span data-ttu-id="d77a8-399">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="d77a8-399">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="d77a8-400">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="d77a8-400">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="d77a8-402">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="d77a8-402">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d77a8-403">附錄 c： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d77a8-403">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="d77a8-404">此附錄將說明如何從 Azure 入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-404">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="d77a8-405">工作 1-從 Azure 入口網站建立新的網站</span><span class="sxs-lookup"><span data-stu-id="d77a8-405">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="d77a8-406">移至[Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="d77a8-406">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-407">使用 Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。</span><span class="sxs-lookup"><span data-stu-id="d77a8-407">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="d77a8-408">您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-408">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="d77a8-409">![登入 Windows Azure 入口網站](build-restful-apis-with-aspnet-web-api/_static/image39.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="d77a8-409">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="d77a8-410">*登入入口網站*</span><span class="sxs-lookup"><span data-stu-id="d77a8-410">*Log on to Portal*</span></span>
2. <span data-ttu-id="d77a8-411">按一下**新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="d77a8-411">Click **New** on the command bar.</span></span>

    <span data-ttu-id="d77a8-412">![建立新的網站](build-restful-apis-with-aspnet-web-api/_static/image40.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="d77a8-412">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="d77a8-413">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="d77a8-413">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="d77a8-414">按一下**計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-414">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="d77a8-415">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="d77a8-415">Then select **Quick Create** option.</span></span> <span data-ttu-id="d77a8-416">新的網站上提供可用的 URL，然後按一下**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-416">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-417">Azure 是您可以控制和管理在雲端中執行 web 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="d77a8-417">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="d77a8-418">快速建立 選項可讓您部署已完成的 web 應用程式，從 azure 入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="d77a8-418">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="d77a8-419">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="d77a8-419">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="d77a8-420">![建立新的網站使用 快速建立](build-restful-apis-with-aspnet-web-api/_static/image41.png "建立新的網站使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="d77a8-420">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="d77a8-421">*建立新的網站使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="d77a8-421">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="d77a8-422">等候新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="d77a8-422">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="d77a8-423">建立網站之後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="d77a8-423">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="d77a8-424">請檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="d77a8-424">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="d77a8-425">![瀏覽至新的網站](build-restful-apis-with-aspnet-web-api/_static/image42.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="d77a8-425">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="d77a8-426">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="d77a8-426">*Browsing to the new web site*</span></span>

    <span data-ttu-id="d77a8-427">![執行網站](build-restful-apis-with-aspnet-web-api/_static/image43.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="d77a8-427">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="d77a8-428">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="d77a8-428">*Web site running*</span></span>
6. <span data-ttu-id="d77a8-429">返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d77a8-429">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="d77a8-430">![開啟網站管理頁面](build-restful-apis-with-aspnet-web-api/_static/image44.png "開啟網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="d77a8-430">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="d77a8-431">*開啟網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="d77a8-431">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="d77a8-432">在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="d77a8-432">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-433">*發行設定檔*包含所有 web 應用程式發行至 Azure 的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="d77a8-433">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="d77a8-434">發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。</span><span class="sxs-lookup"><span data-stu-id="d77a8-434">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="d77a8-435">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="d77a8-435">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="d77a8-436">![下載網站發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image45.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="d77a8-436">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="d77a8-437">*下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="d77a8-437">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="d77a8-438">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="d77a8-438">Download the publish profile file to a known location.</span></span> <span data-ttu-id="d77a8-439">進一步在這個練習中，您會看到如何使用此檔案發行至 Azure 從 Visual Studio web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d77a8-439">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="d77a8-440">![儲存發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image46.png "儲存發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="d77a8-440">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="d77a8-441">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="d77a8-441">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="d77a8-442">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="d77a8-442">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="d77a8-443">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d77a8-443">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="d77a8-444">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="d77a8-444">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="d77a8-445">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="d77a8-445">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="d77a8-446">您可以從您的訂用帳戶，在 Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器的儀表板**.</span><span class="sxs-lookup"><span data-stu-id="d77a8-446">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="d77a8-447">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-447">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="d77a8-448">請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="d77a8-448">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="d77a8-449">並未建立資料庫，因為它會在後續階段中建立。</span><span class="sxs-lookup"><span data-stu-id="d77a8-449">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="d77a8-450">![SQL Database 伺服器儀表板](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="d77a8-450">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="d77a8-451">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="d77a8-451">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="d77a8-452">下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-452">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="d77a8-453">若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d77a8-453">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![加入用戶端 IP 位址](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="d77a8-455">*加入用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="d77a8-455">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="d77a8-456">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="d77a8-456">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="d77a8-458">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="d77a8-458">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="d77a8-459">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d77a8-459">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="d77a8-460">返回至 ASP.NET MVC 4 方案。</span><span class="sxs-lookup"><span data-stu-id="d77a8-460">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="d77a8-461">在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-461">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="d77a8-462">![發行應用程式](build-restful-apis-with-aspnet-web-api/_static/image51.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="d77a8-462">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="d77a8-463">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="d77a8-463">*Publishing the web site*</span></span>
2. <span data-ttu-id="d77a8-464">匯入發行設定檔儲存在第一項工作中。</span><span class="sxs-lookup"><span data-stu-id="d77a8-464">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="d77a8-465">![匯入發行設定檔](build-restful-apis-with-aspnet-web-api/_static/image52.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="d77a8-465">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="d77a8-466">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="d77a8-466">*Importing publish profile*</span></span>
3. <span data-ttu-id="d77a8-467">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-467">Click **Validate Connection**.</span></span> <span data-ttu-id="d77a8-468">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-468">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d77a8-469">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="d77a8-469">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="d77a8-470">![驗證連線](build-restful-apis-with-aspnet-web-api/_static/image53.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="d77a8-470">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="d77a8-471">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="d77a8-471">*Validating connection*</span></span>
4. <span data-ttu-id="d77a8-472">在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="d77a8-472">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="d77a8-473">![Web 部署設定](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="d77a8-473">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="d77a8-474">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="d77a8-474">*Web deploy configuration*</span></span>
5. <span data-ttu-id="d77a8-475">設定資料庫連接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d77a8-475">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="d77a8-476">在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="d77a8-476">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="d77a8-477">在**使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="d77a8-477">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="d77a8-478">在**密碼**輸入伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="d77a8-478">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="d77a8-479">輸入新的資料庫名稱，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="d77a8-479">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="d77a8-480">![設定目的地連接字串](build-restful-apis-with-aspnet-web-api/_static/image55.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="d77a8-480">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="d77a8-481">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="d77a8-481">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="d77a8-482">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="d77a8-482">Then click **OK**.</span></span> <span data-ttu-id="d77a8-483">當系統提示您建立資料庫依序按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-483">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="d77a8-484">![建立資料庫](build-restful-apis-with-aspnet-web-api/_static/image56.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="d77a8-484">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="d77a8-485">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="d77a8-485">*Creating the database*</span></span>
7. <span data-ttu-id="d77a8-486">您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="d77a8-486">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="d77a8-487">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="d77a8-487">Then click **Next**.</span></span>

    <span data-ttu-id="d77a8-488">![連接字串指向 SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "連接字串指向 SQL 資料庫")</span><span class="sxs-lookup"><span data-stu-id="d77a8-488">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="d77a8-489">*連接字串指向 SQL 資料庫*</span><span class="sxs-lookup"><span data-stu-id="d77a8-489">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="d77a8-490">在**預覽**頁面上，按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="d77a8-490">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="d77a8-491">![Web 應用程式發行](build-restful-apis-with-aspnet-web-api/_static/image58.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="d77a8-491">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="d77a8-492">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="d77a8-492">*Publishing the web application*</span></span>
9. <span data-ttu-id="d77a8-493">一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="d77a8-493">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="d77a8-494">![應用程式發行至 Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "應用程式發行至 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="d77a8-494">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="d77a8-495">*應用程式發行至 Azure*</span><span class="sxs-lookup"><span data-stu-id="d77a8-495">*Application published to Azure*</span></span>
