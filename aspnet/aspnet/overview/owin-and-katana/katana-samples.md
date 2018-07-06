---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana 範例 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: a26cdf144d02f0b429994df9d7863163807ac7b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827268"
---
<a name="katana-samples"></a><span data-ttu-id="f93ef-102">Katana 範例</span><span class="sxs-lookup"><span data-stu-id="f93ef-102">Katana Samples</span></span>
====================
<span data-ttu-id="f93ef-103">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f93ef-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="f93ef-104">Katana 範例</span><span class="sxs-lookup"><span data-stu-id="f93ef-104">Katana Samples</span></span>

<span data-ttu-id="f93ef-105">**ASP.NET 路由範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="f93ef-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="f93ef-106">在某些應用程式，您會想要連結與非 OWIN 元件並存的 Asp.Net 路由表中的 OWIN 元件。</span><span class="sxs-lookup"><span data-stu-id="f93ef-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="f93ef-107">此範例示範如何使用 MapOwinPath 和 MapOwinRoute Microsoft.Owin.Host.SystemWeb 所提供的 RouteCollection 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="f93ef-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="f93ef-108">**分支管線範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="f93ef-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="f93ef-109">OWIN 要求處理管線就不需要是線性，它們可以分支到不同的方式處理要求。</span><span class="sxs-lookup"><span data-stu-id="f93ef-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="f93ef-110">此範例示範如何建構要求路徑或其他要求資料，例如標頭為基礎的分支管線。</span><span class="sxs-lookup"><span data-stu-id="f93ef-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="f93ef-111">這些元件是 Microsoft.Owin.Mapping nuget 套件中可用的。</span><span class="sxs-lookup"><span data-stu-id="f93ef-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="f93ef-112">**自訂伺服器範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="f93ef-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="f93ef-113">示範如何使用自訂的 OWIN 伺服器自我裝載時 OWIN。</span><span class="sxs-lookup"><span data-stu-id="f93ef-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="f93ef-114">**內嵌範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="f93ef-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="f93ef-115">某些 OWIN 伺服器可以在自己的程序內執行 (&quot;自我裝載&quot;)。</span><span class="sxs-lookup"><span data-stu-id="f93ef-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="f93ef-116">此範例示範如何開始使用 Microsoft.Owin.Hosting nuget 套件所提供的工具的 OWIN 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f93ef-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="f93ef-117">**HelloWorld 範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="f93ef-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="f93ef-118">OWIN 會是 HTTP 伺服器 API 抽象概念，可讓應用程式可攜性，設定跨越各種伺服器。</span><span class="sxs-lookup"><span data-stu-id="f93ef-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="f93ef-119">這個範例會示範如何撰寫 Hello World 應用程式使用某些**簡單的包裝函式**未經處理的 OWIN 抽象概念和執行它的 web 伺服器上像是 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="f93ef-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="f93ef-120">**Hello World 未經處理的 OWIN 範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="f93ef-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="f93ef-121">這個範例會示範如何撰寫 Hello World 應用程式，使用**原始**OWIN 抽象概念，例如 Asp.Net web 伺服器上加以執行。</span><span class="sxs-lookup"><span data-stu-id="f93ef-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="f93ef-122">**SignalR 範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="f93ef-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="f93ef-123">示範如何在自我裝載使用 OWIN SignalR / Katana。</span><span class="sxs-lookup"><span data-stu-id="f93ef-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="f93ef-124">如需自我裝載的 SignalR 相關的詳細資訊，請參閱[教學課程： SignalR 自我裝載](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。</span><span class="sxs-lookup"><span data-stu-id="f93ef-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="f93ef-125">**靜態檔案範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="f93ef-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="f93ef-126">示範如何使用 OWIN 的靜態檔案的支援 HTTP 要求 / Katana。</span><span class="sxs-lookup"><span data-stu-id="f93ef-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="f93ef-127">**Web API** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="f93ef-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="f93ef-128">此範例示範如何裝載在 IIS 中的 OWIN，以及將 Web API 新增至 OWIN 管線。</span><span class="sxs-lookup"><span data-stu-id="f93ef-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="f93ef-129">**Web 通訊端範例** | [原始程式碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="f93ef-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="f93ef-130">示範如何使用 OWIN 支援 Web 通訊端[System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="f93ef-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
