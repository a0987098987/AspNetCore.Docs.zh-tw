---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: 開始使用 OWIN 與 Katana |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="09d9c-102">OWIN 和 Katana 入門</span><span class="sxs-lookup"><span data-stu-id="09d9c-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="09d9c-103">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="09d9c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="09d9c-104">[開啟 Web 介面的.NET (OWIN)](http://owin.org/)定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="09d9c-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="09d9c-105">透過分離應用程式的 web 伺服器，OWIN 可讓您更輕鬆地建立適用於.NET web 開發的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="09d9c-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="09d9c-106">OWIN 此外，可以更輕鬆地連接埠的 web 應用程式的其他主機&#8212;，例如 Windows 服務或其他處理程序中自我裝載。</span><span class="sxs-lookup"><span data-stu-id="09d9c-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="09d9c-107">OWIN 是社群所擁有的規格，而不實作。</span><span class="sxs-lookup"><span data-stu-id="09d9c-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="09d9c-108">Katana 專案是由 Microsoft 開發的開放原始碼 OWIN 元件的一組。</span><span class="sxs-lookup"><span data-stu-id="09d9c-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="09d9c-109">OWIN 和 Katana 的一般概觀，請參閱[的專案 Katana 概觀](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="09d9c-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="09d9c-110">在本文中，我將直接跳至程式碼以開始。</span><span class="sxs-lookup"><span data-stu-id="09d9c-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="09d9c-111">本教學課程使用[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)，但您也可以使用 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="09d9c-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="09d9c-112">在 Visual Studio 2012，我請注意以下幾個步驟截然不同。</span><span class="sxs-lookup"><span data-stu-id="09d9c-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="09d9c-113">主機在 IIS 中的 OWIN</span><span class="sxs-lookup"><span data-stu-id="09d9c-113">Host OWIN in IIS</span></span>

<span data-ttu-id="09d9c-114">在本節中，我們將會裝載在 IIS 中的 OWIN。</span><span class="sxs-lookup"><span data-stu-id="09d9c-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="09d9c-115">此選項可讓您彈性與撰寫性 OWIN 管線以及 IIS 的完整功能集。</span><span class="sxs-lookup"><span data-stu-id="09d9c-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="09d9c-116">OWIN 應用程式使用這個選項，在 ASP.NET 要求管線中執行。</span><span class="sxs-lookup"><span data-stu-id="09d9c-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="09d9c-117">首先，建立新的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="09d9c-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="09d9c-118">（在 Visual Studio 2012 中，使用 ASP.NET 空 Web 應用程式專案類型）。</span><span class="sxs-lookup"><span data-stu-id="09d9c-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="09d9c-119">在**新增 ASP.NET 專案**對話方塊中，選取**空**範本。</span><span class="sxs-lookup"><span data-stu-id="09d9c-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="09d9c-120">新增 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="09d9c-120">Add NuGet Packages</span></span>

<span data-ttu-id="09d9c-121">接下來，加入必要的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="09d9c-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="09d9c-122">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="09d9c-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="09d9c-123">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="09d9c-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="09d9c-124">加入啟動類別</span><span class="sxs-lookup"><span data-stu-id="09d9c-124">Add a Startup Class</span></span>

<span data-ttu-id="09d9c-125">接下來，將 OWIN 啟動類別加入。</span><span class="sxs-lookup"><span data-stu-id="09d9c-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="09d9c-126">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增**，然後選取**新項目**。</span><span class="sxs-lookup"><span data-stu-id="09d9c-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="09d9c-127">在**加入新項目**對話方塊中，選取**Owin 啟動 「 類別**。</span><span class="sxs-lookup"><span data-stu-id="09d9c-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="09d9c-128">如需設定啟動類別的詳細資訊，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。</span><span class="sxs-lookup"><span data-stu-id="09d9c-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="09d9c-129">將下列程式碼加入 `Startup1.Configuration` 方法：</span><span class="sxs-lookup"><span data-stu-id="09d9c-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="09d9c-130">這個程式碼加入至 OWIN 管線，接收的函式以實作簡單的中介軟體片段**Microsoft.Owin.IOwinContext**執行個體。</span><span class="sxs-lookup"><span data-stu-id="09d9c-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="09d9c-131">當伺服器收到的 HTTP 要求時，OWIN 管線叫用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="09d9c-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="09d9c-132">中介軟體設定回應的內容類型，並將寫入回應主體。</span><span class="sxs-lookup"><span data-stu-id="09d9c-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="09d9c-133">Visual Studio 2013 中使用 OWIN 啟動 「 類別樣板。</span><span class="sxs-lookup"><span data-stu-id="09d9c-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="09d9c-134">如果您使用 Visual Studio 2012，只要加入新的空類別，名為`Startup1`，並貼上下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="09d9c-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="09d9c-135">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="09d9c-135">Run the Application</span></span>

<span data-ttu-id="09d9c-136">按 f5 鍵開始偵錯。</span><span class="sxs-lookup"><span data-stu-id="09d9c-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="09d9c-137">Visual Studio 會開啟瀏覽器視窗，以`http://localhost:*port*/`。</span><span class="sxs-lookup"><span data-stu-id="09d9c-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="09d9c-138">網頁看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="09d9c-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="09d9c-139">主控台應用程式中自我裝載的 OWIN</span><span class="sxs-lookup"><span data-stu-id="09d9c-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="09d9c-140">所以可以輕鬆地將此應用程式的自訂處理序中自我裝載的 IIS 裝載。</span><span class="sxs-lookup"><span data-stu-id="09d9c-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="09d9c-141">以 IIS 裝載時，IIS 會做為 HTTP 伺服器，以及裝載服務的處理序。</span><span class="sxs-lookup"><span data-stu-id="09d9c-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="09d9c-142">具有自我裝載，您的應用程式會建立處理程序並使用**HttpListener**與 HTTP 伺服器的類別。</span><span class="sxs-lookup"><span data-stu-id="09d9c-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="09d9c-143">在 Visual Studio 中建立新的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="09d9c-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="09d9c-144">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="09d9c-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="09d9c-145">新增`Startup1`本教學課程的第 1 部分專案的類別。</span><span class="sxs-lookup"><span data-stu-id="09d9c-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="09d9c-146">您不需要修改此類別。</span><span class="sxs-lookup"><span data-stu-id="09d9c-146">You don't need to modify this class.</span></span>

<span data-ttu-id="09d9c-147">實作應用程式的`Main`方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="09d9c-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="09d9c-148">當您執行主控台應用程式時，伺服器會啟動接聽`http://localhost:9000`。</span><span class="sxs-lookup"><span data-stu-id="09d9c-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="09d9c-149">如果您導覽至這個網頁瀏覽器的位址，您會看到"Hello world"頁面。</span><span class="sxs-lookup"><span data-stu-id="09d9c-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="09d9c-150">新增 OWIN Diagnostics</span><span class="sxs-lookup"><span data-stu-id="09d9c-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="09d9c-151">Microsoft.Owin.Diagnostics 套件包含中介軟體會攔截未處理例外狀況，並會顯示具有錯誤的詳細資料的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="09d9c-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="09d9c-152">此頁面函式一樣，ASP.NET 錯誤網頁，有時也稱為 「[黃色死亡畫面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)」 (YSOD)。</span><span class="sxs-lookup"><span data-stu-id="09d9c-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="09d9c-153">像 YSOD Katana 錯誤頁面很有用在開發期間，但它是最好的作法是在生產模式中停用它。</span><span class="sxs-lookup"><span data-stu-id="09d9c-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="09d9c-154">若要安裝在您的專案中的診斷封裝，請在 [封裝管理員主控台] 視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="09d9c-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="09d9c-155">在變更程式碼程式`Startup1.Configuration`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="09d9c-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="09d9c-156">現在，讓 Visual Studio 不會在例外狀況中斷，請執行應用程式，但不偵錯，使用 CTRL + F5。</span><span class="sxs-lookup"><span data-stu-id="09d9c-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="09d9c-157">應用程式的行為相同如往常一般，直到您瀏覽至`http://localhost/fail`，此時應用程式會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="09d9c-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="09d9c-158">錯誤頁面中介軟體將會攔截例外狀況，並顯示具有錯誤的相關資訊的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="09d9c-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="09d9c-159">您可以按一下索引標籤來查看堆疊、 查詢字串、 cookie、 要求標頭，以及 OWIN 環境變數。</span><span class="sxs-lookup"><span data-stu-id="09d9c-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="09d9c-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="09d9c-160">Next Steps</span></span>

- [<span data-ttu-id="09d9c-161">OWIN 啟動類別偵測</span><span class="sxs-lookup"><span data-stu-id="09d9c-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="09d9c-162">使用 OWIN 自我裝載 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="09d9c-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="09d9c-163">使用 OWIN 自我裝載 SignalR</span><span class="sxs-lookup"><span data-stu-id="09d9c-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
