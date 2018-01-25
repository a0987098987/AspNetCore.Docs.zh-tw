---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "啟用 Windows 驗證中 Katana |Microsoft 文件"
author: MikeWasson
description: "本文將說明如何啟用 Katana 中的 Windows 驗證。 它涵蓋了兩個案例： 使用 IIS 來裝載 Katana，以及使用 HttpListener 自我裝載 Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="94b7f-104">Katana 中啟用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="94b7f-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="94b7f-105">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="94b7f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="94b7f-106">本文將說明如何啟用 Katana 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="94b7f-107">它涵蓋了兩個案例： 使用 IIS 來裝載 Katana，以及使用 HttpListener 自我裝載 Katana 自訂程序中。</span><span class="sxs-lookup"><span data-stu-id="94b7f-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="94b7f-108">感謝您 Barry Dorrans、 David Matson 和 Chris Ross 檢閱這份文件。</span><span class="sxs-lookup"><span data-stu-id="94b7f-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="94b7f-109">Katana 是 Microsoft 實作[OWIN](http://owin.org/)，開啟 Web 介面 for.NET。</span><span class="sxs-lookup"><span data-stu-id="94b7f-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="94b7f-110">您可以閱讀簡介 OWIN 和 Katana[這裡](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="94b7f-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="94b7f-111">OWIN 架構分為數個層級：</span><span class="sxs-lookup"><span data-stu-id="94b7f-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="94b7f-112">主機： 管理 OWIN 管線執行所在的程序。</span><span class="sxs-lookup"><span data-stu-id="94b7f-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="94b7f-113">伺服器： 會開啟網路通訊端，並接聽要求。</span><span class="sxs-lookup"><span data-stu-id="94b7f-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="94b7f-114">中介軟體： 處理 HTTP 要求和回應。</span><span class="sxs-lookup"><span data-stu-id="94b7f-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="94b7f-115">Katana 目前提供兩個伺服器，這兩者都支援 Windows 整合式驗證：</span><span class="sxs-lookup"><span data-stu-id="94b7f-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="94b7f-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="94b7f-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="94b7f-117">使用 IIS 的 ASP.NET 管線。</span><span class="sxs-lookup"><span data-stu-id="94b7f-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="94b7f-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="94b7f-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="94b7f-119">使用[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)。</span><span class="sxs-lookup"><span data-stu-id="94b7f-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="94b7f-120">此伺服器目前是預設選項自我裝載時 Katana。</span><span class="sxs-lookup"><span data-stu-id="94b7f-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="94b7f-121">Katana 目前未提供 OWIN 中介軟體的 Windows 驗證，因為這項功能已在伺服器可用。</span><span class="sxs-lookup"><span data-stu-id="94b7f-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="94b7f-122">在 IIS 中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="94b7f-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="94b7f-123">使用 Microsoft.Owin.Host.SystemWeb，您可以只啟用 IIS 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="94b7f-124">讓我們開始建立新的 ASP.NET 應用程式，使用 「 ASP.NET 空 Web 應用程式 」 專案範本。</span><span class="sxs-lookup"><span data-stu-id="94b7f-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="94b7f-125">接下來，新增 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="94b7f-125">Next, add NuGet packages.</span></span> <span data-ttu-id="94b7f-126">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="94b7f-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="94b7f-127">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="94b7f-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="94b7f-128">現在加入類別，名為`Startup`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="94b7f-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="94b7f-129">這是您只需要建立 owin，在 IIS 上執行"Hello world"應用程式。</span><span class="sxs-lookup"><span data-stu-id="94b7f-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="94b7f-130">按 F5，進行應用程式偵錯。</span><span class="sxs-lookup"><span data-stu-id="94b7f-130">Press F5 to debug the application.</span></span> <span data-ttu-id="94b7f-131">您應該會看到"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="94b7f-131">You should see "Hello World!"</span></span> <span data-ttu-id="94b7f-132">在瀏覽器視窗中。</span><span class="sxs-lookup"><span data-stu-id="94b7f-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="94b7f-133">接下來，我們會啟用 IIS Express 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="94b7f-134">從**檢視**功能表上，選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="94b7f-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="94b7f-135">按一下 [方案總管]，檢視專案屬性中的專案名稱上。</span><span class="sxs-lookup"><span data-stu-id="94b7f-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="94b7f-136">在**屬性**視窗中，將**匿名驗證**至**已停用**並設定**Windows 驗證**至**啟用**。</span><span class="sxs-lookup"><span data-stu-id="94b7f-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="94b7f-137">當您從 Visual Studio 執行應用程式時，則 IIS Express 會要求使用者的 Windows 認證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="94b7f-138">您可以看到此使用[Fiddler](http://fiddler2.com/home)或另一個 HTTP 偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="94b7f-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="94b7f-139">以下是範例 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="94b7f-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="94b7f-140">Www-authenticate 標頭，在此回應指出伺服器支援[交涉](http://www.ietf.org/rfc/rfc4559.txt)使用 Kerberos 或 NTLM 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="94b7f-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="94b7f-141">稍後，當您部署至伺服器應用程式，請遵循[步驟](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)該伺服器上啟用 IIS 中的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="94b7f-142">HttpListener 中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="94b7f-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="94b7f-143">如果您使用來自我裝載 Katana Microsoft.Owin.Host.HttpListener，您可以直接上啟用 Windows 驗證**HttpListener**執行個體。</span><span class="sxs-lookup"><span data-stu-id="94b7f-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="94b7f-144">首先，建立新的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="94b7f-144">First, create a new console application.</span></span> <span data-ttu-id="94b7f-145">接下來，新增 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="94b7f-145">Next, add NuGet packages.</span></span> <span data-ttu-id="94b7f-146">從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="94b7f-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="94b7f-147">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="94b7f-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="94b7f-148">現在加入類別，名為`Startup`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="94b7f-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="94b7f-149">這個類別會實作"Hello world"的範例相同，但它也會做為驗證配置設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="94b7f-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="94b7f-150">內部`Main`函式中，啟動 OWIN 管線：</span><span class="sxs-lookup"><span data-stu-id="94b7f-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="94b7f-151">您可以使用 Fiddler，以確認應用程式使用 Windows 驗證來傳送要求：</span><span class="sxs-lookup"><span data-stu-id="94b7f-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="94b7f-152">相關主題</span><span class="sxs-lookup"><span data-stu-id="94b7f-152">Related Topics</span></span>

[<span data-ttu-id="94b7f-153">Katana 專案概觀</span><span class="sxs-lookup"><span data-stu-id="94b7f-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="94b7f-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="94b7f-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="94b7f-155">了解 OWIN MVC 5 中的表單驗證</span><span class="sxs-lookup"><span data-stu-id="94b7f-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
