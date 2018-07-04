---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: 啟用在 Katana 中的 Windows 驗證 |Microsoft Docs
author: MikeWasson
description: 本文說明如何啟用 Windows 驗證，在 Katana 中。 它涵蓋了兩種案例： 使用 IIS 來裝載 Katana，並使用 HttpListener 自我裝載 Kat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: e7fbe6af323ecdc09b4d79073f506c5ee056f30f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391611"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="4be09-104">在 Katana 中啟用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="4be09-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="4be09-105">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4be09-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="4be09-106">本文說明如何啟用 Windows 驗證，在 Katana 中。</span><span class="sxs-lookup"><span data-stu-id="4be09-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="4be09-107">它涵蓋了兩種案例： 使用 IIS 來裝載 Katana，並使用 HttpListener 自我裝載的自訂處理序中的 Katana。</span><span class="sxs-lookup"><span data-stu-id="4be09-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="4be09-108">感謝您 Barry Dorrans、 David Matson 和 Chris Ross 閱本篇文章。</span><span class="sxs-lookup"><span data-stu-id="4be09-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="4be09-109">Katana 是 Microsoft 的實作[OWIN](http://owin.org/)，Open Web Interface for.NET。</span><span class="sxs-lookup"><span data-stu-id="4be09-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="4be09-110">您可以閱讀簡介 OWIN 和 Katana[此處](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="4be09-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="4be09-111">OWIN 架構有數個層級：</span><span class="sxs-lookup"><span data-stu-id="4be09-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="4be09-112">主機： 管理程序中執行 OWIN 管線。</span><span class="sxs-lookup"><span data-stu-id="4be09-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="4be09-113">伺服器： 會開啟網路通訊端，並接聽要求。</span><span class="sxs-lookup"><span data-stu-id="4be09-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="4be09-114">中介軟體： 處理 HTTP 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="4be09-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="4be09-115">Katana 目前提供兩部伺服器，這兩者都支援 Windows 整合式驗證：</span><span class="sxs-lookup"><span data-stu-id="4be09-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="4be09-116">**Microsoft.Owin.Host.SystemWeb**。</span><span class="sxs-lookup"><span data-stu-id="4be09-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="4be09-117">使用 IIS 與 ASP.NET 管線。</span><span class="sxs-lookup"><span data-stu-id="4be09-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="4be09-118">**Microsoft.Owin.Host.HttpListener**。</span><span class="sxs-lookup"><span data-stu-id="4be09-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="4be09-119">會使用[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4be09-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="4be09-120">此伺服器目前是預設選項自我裝載時 Katana。</span><span class="sxs-lookup"><span data-stu-id="4be09-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="4be09-121">Katana 目前不提供 OWIN 中介軟體進行 Windows 驗證，因為這項功能已在伺服器中可用。</span><span class="sxs-lookup"><span data-stu-id="4be09-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="4be09-122">在 IIS 中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="4be09-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="4be09-123">使用 Microsoft.Owin.Host.SystemWeb，您可以只啟用 IIS 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="4be09-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="4be09-124">現在就開始建立新的 ASP.NET 應用程式，使用 「 ASP.NET 空白 Web 應用程式 」 專案範本。</span><span class="sxs-lookup"><span data-stu-id="4be09-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="4be09-125">接下來，新增 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="4be09-125">Next, add NuGet packages.</span></span> <span data-ttu-id="4be09-126">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="4be09-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4be09-127">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4be09-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="4be09-128">現在加入類別，名為`Startup`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4be09-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="4be09-129">這是您只需要建立 OWIN，在 IIS 上執行"Hello world"應用程式。</span><span class="sxs-lookup"><span data-stu-id="4be09-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="4be09-130">按 F5，進行應用程式偵錯。</span><span class="sxs-lookup"><span data-stu-id="4be09-130">Press F5 to debug the application.</span></span> <span data-ttu-id="4be09-131">您應該會看到"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="4be09-131">You should see "Hello World!"</span></span> <span data-ttu-id="4be09-132">在瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="4be09-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="4be09-133">接下來，我們將啟用 IIS Express 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="4be09-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="4be09-134">從**檢視**功能表上，選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="4be09-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="4be09-135">按一下 [方案總管]，檢視專案屬性中的專案名稱上。</span><span class="sxs-lookup"><span data-stu-id="4be09-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="4be09-136">中**屬性**視窗中，將**匿名驗證**來**已停用**並將**Windows 驗證**到**啟用**。</span><span class="sxs-lookup"><span data-stu-id="4be09-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="4be09-137">當您從 Visual Studio 執行應用程式時，則 IIS Express 會要求使用者的 Windows 認證。</span><span class="sxs-lookup"><span data-stu-id="4be09-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="4be09-138">您可以使用來查看這[Fiddler](http://fiddler2.com/home)或另一個 HTTP 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="4be09-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="4be09-139">以下是範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="4be09-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="4be09-140">Www-authenticate 標頭，此回應指出伺服器支援[交涉](http://www.ietf.org/rfc/rfc4559.txt)通訊協定，它會使用 Kerberos 或 NTLM。</span><span class="sxs-lookup"><span data-stu-id="4be09-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="4be09-141">稍後，當您部署至伺服器應用程式，請遵循[這些步驟](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)該伺服器上啟用 IIS 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="4be09-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="4be09-142">HttpListener 中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="4be09-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="4be09-143">如果您使用 Microsoft.Owin.Host.HttpListener 自我裝載 Katana，您可以直接上啟用 Windows 驗證**HttpListener**執行個體。</span><span class="sxs-lookup"><span data-stu-id="4be09-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="4be09-144">首先，建立新的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="4be09-144">First, create a new console application.</span></span> <span data-ttu-id="4be09-145">接下來，新增 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="4be09-145">Next, add NuGet packages.</span></span> <span data-ttu-id="4be09-146">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="4be09-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="4be09-147">在 [套件管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4be09-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="4be09-148">現在加入類別，名為`Startup`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="4be09-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="4be09-149">這個類別會實作"Hello world"的範例相同，但它也會做為驗證配置設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="4be09-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="4be09-150">內`Main`函式中，啟動 OWIN 管線：</span><span class="sxs-lookup"><span data-stu-id="4be09-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="4be09-151">您可以使用 Fiddler 以確認應用程式會使用 Windows 驗證來傳送要求：</span><span class="sxs-lookup"><span data-stu-id="4be09-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="4be09-152">相關主題</span><span class="sxs-lookup"><span data-stu-id="4be09-152">Related Topics</span></span>

[<span data-ttu-id="4be09-153">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="4be09-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="4be09-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="4be09-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="4be09-155">了解 OWIN MVC 5 中的表單驗證</span><span class="sxs-lookup"><span data-stu-id="4be09-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
