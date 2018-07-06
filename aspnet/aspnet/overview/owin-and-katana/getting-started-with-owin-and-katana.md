---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: 開始使用 OWIN 和 Katana |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826655"
---
<a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="b2567-102">開始使用 OWIN 和 Katana</span><span class="sxs-lookup"><span data-stu-id="b2567-102">Getting Started with OWIN and Katana</span></span>
====================
<span data-ttu-id="b2567-103">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b2567-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b2567-104">[Open Web Interface for.NET (OWIN)](http://owin.org/)定義.NET web 伺服器和 web 應用程式之間的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="b2567-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b2567-105">藉由將分離應用程式的 web 伺服器，OWIN 可讓您更輕鬆地建立.NET web 程式開發的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b2567-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="b2567-106">此外，OWIN 輕鬆連接至其他主機的 web 應用程式&#8212;，例如自我裝載在 Windows 服務或其他處理序。</span><span class="sxs-lookup"><span data-stu-id="b2567-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="b2567-107">OWIN 是社群所擁有的規格，而不實作。</span><span class="sxs-lookup"><span data-stu-id="b2567-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="b2567-108">Katana 專案是一份由 Microsoft 所開發的開放原始碼 OWIN 元件。</span><span class="sxs-lookup"><span data-stu-id="b2567-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="b2567-109">OWIN 和 Katana 的一般概觀，請參閱 <<c0> [ 概觀的 Katana 專案](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="b2567-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="b2567-110">在本文中，我會直接跳到程式碼，以開始。</span><span class="sxs-lookup"><span data-stu-id="b2567-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="b2567-111">本教學課程會使用[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)，但您也可以使用 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="b2567-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="b2567-112">幾個步驟是不同的 Visual Studio 2012，我說明如下。</span><span class="sxs-lookup"><span data-stu-id="b2567-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="b2567-113">將 OWIN 裝載在 IIS</span><span class="sxs-lookup"><span data-stu-id="b2567-113">Host OWIN in IIS</span></span>

<span data-ttu-id="b2567-114">在本節中，我們將會裝載在 IIS 中的 OWIN。</span><span class="sxs-lookup"><span data-stu-id="b2567-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="b2567-115">此選項可讓您搭配 IIS 的成熟的功能集 OWIN 管線的編寫性與彈性。</span><span class="sxs-lookup"><span data-stu-id="b2567-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="b2567-116">OWIN 應用程式使用此選項，在 ASP.NET 要求管線中執行。</span><span class="sxs-lookup"><span data-stu-id="b2567-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="b2567-117">首先，建立新的 ASP.NET Web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="b2567-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="b2567-118">（在 Visual Studio 2012 中，使用 ASP.NET 空白 Web 應用程式專案類型）。</span><span class="sxs-lookup"><span data-stu-id="b2567-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="b2567-119">在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。</span><span class="sxs-lookup"><span data-stu-id="b2567-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="b2567-120">新增 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="b2567-120">Add NuGet Packages</span></span>

<span data-ttu-id="b2567-121">接下來，新增必要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="b2567-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="b2567-122">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="b2567-122">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b2567-123">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b2567-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="b2567-124">加入 Startup 類別</span><span class="sxs-lookup"><span data-stu-id="b2567-124">Add a Startup Class</span></span>

<span data-ttu-id="b2567-125">接下來，新增 OWIN 啟動類別。</span><span class="sxs-lookup"><span data-stu-id="b2567-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="b2567-126">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增**，然後選取**新項目**。</span><span class="sxs-lookup"><span data-stu-id="b2567-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="b2567-127">在 **加入新項目**對話方塊中，選取**Owin 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="b2567-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="b2567-128">如需設定啟動類別的詳細資訊，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。</span><span class="sxs-lookup"><span data-stu-id="b2567-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="b2567-129">將下列程式碼加入 `Startup1.Configuration` 方法：</span><span class="sxs-lookup"><span data-stu-id="b2567-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="b2567-130">此程式碼會將簡單的一種中介軟體新增至 OWIN 管線中，實作為函式來接收**Microsoft.Owin.IOwinContext**執行個體。</span><span class="sxs-lookup"><span data-stu-id="b2567-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="b2567-131">當伺服器收到 HTTP 要求時，OWIN 管線叫用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b2567-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="b2567-132">中介軟體會設定回應的內容類型，並寫入回應主體。</span><span class="sxs-lookup"><span data-stu-id="b2567-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="b2567-133">Visual Studio 2013 中使用 [OWIN 啟動類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="b2567-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="b2567-134">如果您使用 Visual Studio 2012，只是加入新的空類別，名為`Startup1`，並貼上下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b2567-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="b2567-135">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b2567-135">Run the Application</span></span>

<span data-ttu-id="b2567-136">按 F5 開始偵錯。</span><span class="sxs-lookup"><span data-stu-id="b2567-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="b2567-137">Visual Studio 會開啟瀏覽器視窗，以`http://localhost:*port*/`。</span><span class="sxs-lookup"><span data-stu-id="b2567-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="b2567-138">網頁看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="b2567-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="b2567-139">將 OWIN 自我裝載的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="b2567-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="b2567-140">它很容易從自我裝載的自訂處理序中的 IIS 裝載轉換此應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2567-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="b2567-141">使用 IIS 裝載，IIS 會做為 HTTP 伺服器，以及裝載服務的處理序。</span><span class="sxs-lookup"><span data-stu-id="b2567-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="b2567-142">與自我裝載，您的應用程式建立程序，並使用**HttpListener**與 HTTP 伺服器的類別。</span><span class="sxs-lookup"><span data-stu-id="b2567-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="b2567-143">在 Visual Studio 中建立新的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="b2567-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="b2567-144">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b2567-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="b2567-145">新增`Startup1`本教學課程的第 1 部分的類別加入專案。</span><span class="sxs-lookup"><span data-stu-id="b2567-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="b2567-146">您不需要修改此類別。</span><span class="sxs-lookup"><span data-stu-id="b2567-146">You don't need to modify this class.</span></span>

<span data-ttu-id="b2567-147">實作應用程式的`Main`方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="b2567-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="b2567-148">當您執行主控台應用程式時，便會開始在伺服器接聽`http://localhost:9000`。</span><span class="sxs-lookup"><span data-stu-id="b2567-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="b2567-149">如果您導覽到此位址在網頁瀏覽器中，您會看到"Hello world"頁面。</span><span class="sxs-lookup"><span data-stu-id="b2567-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="b2567-150">新增 OWIN 診斷</span><span class="sxs-lookup"><span data-stu-id="b2567-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="b2567-151">Microsoft.Owin.Diagnostics 封裝包含中介軟體會攔截未處理例外狀況，並會顯示具有錯誤詳細資料的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="b2567-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="b2567-152">此頁面函式十分類似 ASP.NET 錯誤頁面，有時也稱為 「[黃色死亡畫面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)」 (YSOD)。</span><span class="sxs-lookup"><span data-stu-id="b2567-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="b2567-153">如同 YSOD，Katana 錯誤頁面適合用在開發期間，但建議在生產模式中停用它。</span><span class="sxs-lookup"><span data-stu-id="b2567-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="b2567-154">若要安裝在您的專案中的診斷套件，請在 [套件管理員主控台] 視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b2567-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="b2567-155">變更程式碼中的您`Startup1.Configuration`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b2567-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="b2567-156">現在使用 CTRL + F5 執行應用程式，但不偵錯，讓 Visual Studio 不會在例外狀況中斷。</span><span class="sxs-lookup"><span data-stu-id="b2567-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="b2567-157">應用程式的行為相同，直到您瀏覽至`http://localhost/fail`，此時應用程式擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b2567-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="b2567-158">錯誤頁面中介軟體將會攔截例外狀況，並顯示具有錯誤的相關資訊的 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="b2567-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="b2567-159">您可以按一下索引標籤來查看堆疊、 查詢字串、 cookie、 要求標頭和 OWIN 環境變數。</span><span class="sxs-lookup"><span data-stu-id="b2567-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="b2567-160">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b2567-160">Next Steps</span></span>

- [<span data-ttu-id="b2567-161">OWIN 啟動類別偵測</span><span class="sxs-lookup"><span data-stu-id="b2567-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="b2567-162">使用 OWIN 自我裝載 ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b2567-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="b2567-163">使用 OWIN 自我裝載 SignalR</span><span class="sxs-lookup"><span data-stu-id="b2567-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
