---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Katana 範例 |Microsoft 文件
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034486"
---
<a name="katana-samples"></a>Katana 範例
====================
by [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Katana 範例

**ASP.NET 路由範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)  
在某些應用程式，您要在 Asp.Net 路由表與非 OWIN 元件並存的 OWIN 元件相連結。 這個範例示範如何使用 MapOwinPath 和 MapOwinRoute Microsoft.Owin.Host.SystemWeb 所提供的 RouteCollection 擴充方法。

**分支管線範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)  
OWIN 要求處理管線，不需要是線性，它們可以分支到不同的方式處理要求。 這個範例示範如何建構分支的管線要求路徑或其他要求資料，例如標頭。 這些元件是 Microsoft.Owin.Mapping nuget 套件中可用。

**自訂伺服器範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs)   
示範如何使用自訂的 OWIN 伺服器自我裝載時 OWIN。

**內嵌範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)  
某些 OWIN 伺服器可以執行自己的程序內 (&quot;自我裝載&quot;)。 這個範例示範如何啟動 OWIN 應用程式使用 Microsoft.Owin.Hosting nuget 封裝所提供的工具。

**HelloWorld 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)  
OWIN 是 HTTP 伺服器 API 的抽象概念，設定跨越各種伺服器啟用應用程式可攜性。 這個範例會示範如何撰寫使用部分的 Hello World 應用程式**簡單包裝函式**周圍的未經處理的 OWIN 抽象和執行它的 web 伺服器上要 ASP.NET。

**Hello World 原始 OWIN 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)  
這個範例會示範如何撰寫 Hello World 應用程式使用**原始**OWIN 抽象和 Asp.Net web 伺服器上執行它。

**SignalR 範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)  
示範如何在自我裝載使用 OWIN SignalR / Katana。 如需自我裝載的 SignalR 的詳細資訊，請參閱[教學課程： 自我裝載的 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)。

**靜態檔案範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs)   
示範如何支援使用 OWIN 的靜態檔案的 HTTP 要求 / Katana。

**Web 應用程式開發介面** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt)   
這個範例示範如何裝載在 IIS 中的 OWIN，並將 Web API 新增至 OWIN 管線。

**Web 通訊端範例** | [原始碼](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs)   
示範如何透過 OWIN 中支援 Web 通訊端[System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx)類別。
