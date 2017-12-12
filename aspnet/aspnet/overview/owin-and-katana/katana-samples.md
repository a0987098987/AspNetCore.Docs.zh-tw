---
uid: aspnet/overview/owin-and-katana/katana-samples
title: "Katana 範例 |Microsoft 文件"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 5540164bda8db31c4e78b49ecb7f7c573acca013
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="katana-samples"></a><span data-ttu-id="bc010-102">Katana 範例</span><span class="sxs-lookup"><span data-stu-id="bc010-102">Katana Samples</span></span>
====================
<span data-ttu-id="bc010-103">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bc010-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="bc010-104">Katana 範例</span><span class="sxs-lookup"><span data-stu-id="bc010-104">Katana Samples</span></span>

<span data-ttu-id="bc010-105">**ASP.NET 路由範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="bc010-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="bc010-106">在某些應用程式，您要在 Asp.Net 路由表與非 OWIN 元件並存的 OWIN 元件相連結。</span><span class="sxs-lookup"><span data-stu-id="bc010-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="bc010-107">這個範例示範如何使用 MapOwinPath 和 MapOwinRoute Microsoft.Owin.Host.SystemWeb 所提供的 RouteCollection 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="bc010-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="bc010-108">**分支管線範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="bc010-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="bc010-109">OWIN 要求處理管線，不需要是線性，它們可以分支到不同的方式處理要求。</span><span class="sxs-lookup"><span data-stu-id="bc010-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="bc010-110">這個範例示範如何建構分支的管線要求路徑或其他要求資料，例如標頭。</span><span class="sxs-lookup"><span data-stu-id="bc010-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="bc010-111">這些元件是 Microsoft.Owin.Mapping nuget 套件中可用。</span><span class="sxs-lookup"><span data-stu-id="bc010-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="bc010-112">**自訂伺服器範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="bc010-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="bc010-113">示範如何使用自訂的 OWIN 伺服器自我裝載時 OWIN。</span><span class="sxs-lookup"><span data-stu-id="bc010-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="bc010-114">**內嵌範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="bc010-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="bc010-115">某些 OWIN 伺服器可以執行自己的程序內 (&quot;自我裝載&quot;)。</span><span class="sxs-lookup"><span data-stu-id="bc010-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="bc010-116">這個範例示範如何啟動 OWIN 應用程式使用 Microsoft.Owin.Hosting nuget 封裝所提供的工具。</span><span class="sxs-lookup"><span data-stu-id="bc010-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="bc010-117">**HelloWorld 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="bc010-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="bc010-118">OWIN 是 HTTP 伺服器 API 的抽象概念，設定跨越各種伺服器啟用應用程式可攜性。</span><span class="sxs-lookup"><span data-stu-id="bc010-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="bc010-119">這個範例會示範如何撰寫使用部分的 Hello World 應用程式**簡單包裝函式**周圍的未經處理的 OWIN 抽象和執行它的 web 伺服器上要 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="bc010-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="bc010-120">**Hello World 原始 OWIN 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="bc010-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="bc010-121">這個範例會示範如何撰寫 Hello World 應用程式使用**原始**OWIN 抽象和 Asp.Net web 伺服器上執行它。</span><span class="sxs-lookup"><span data-stu-id="bc010-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="bc010-122">**SignalR 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="bc010-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="bc010-123">示範如何在自我裝載使用 OWIN SignalR / Katana。</span><span class="sxs-lookup"><span data-stu-id="bc010-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="bc010-124">如需自我裝載的 SignalR 的詳細資訊，請參閱[教學課程： 自我裝載的 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。</span><span class="sxs-lookup"><span data-stu-id="bc010-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="bc010-125">**靜態檔案範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="bc010-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="bc010-126">示範如何支援使用 OWIN 的靜態檔案的 HTTP 要求 / Katana。</span><span class="sxs-lookup"><span data-stu-id="bc010-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="bc010-127">**Web 應用程式開發介面** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="bc010-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="bc010-128">這個範例示範如何裝載在 IIS 中的 OWIN，並將 Web API 新增至 OWIN 管線。</span><span class="sxs-lookup"><span data-stu-id="bc010-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="bc010-129">**Web 通訊端範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="bc010-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="bc010-130">示範如何透過 OWIN 中支援 Web 通訊端[System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="bc010-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
