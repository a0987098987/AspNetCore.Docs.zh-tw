---
uid: webhooks/diagnostics/logging
title: "記錄的 ASP.NET Webhook |Microsoft 文件"
author: rick-anderson
description: "如何登入 ASP.NET Webhook。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="743d7-103">記錄的 ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="743d7-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="743d7-104">Microsoft ASP.NET Webhook 會使用這種報告所發生的問題記錄。</span><span class="sxs-lookup"><span data-stu-id="743d7-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="743d7-105">根據預設記錄檔會寫入使用[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)其中，它們可以能夠使用[追蹤接聽項](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)像任何其他記錄檔資料流。</span><span class="sxs-lookup"><span data-stu-id="743d7-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="743d7-106">在部署 Web 應用程式當做 Azure Web 應用程式時，記錄檔自動挑選，並可管理以及任何其他[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)記錄。</span><span class="sxs-lookup"><span data-stu-id="743d7-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="743d7-107">如需詳細資訊，請參閱[啟用 Azure App Service 中的 web 應用程式的診斷記錄](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="743d7-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="743d7-108">此外，取得記錄檔，直接從 Visual Studio 中所述內部[疑難排解 web 應用程式中使用 Visual Studio 的 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。</span><span class="sxs-lookup"><span data-stu-id="743d7-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="743d7-109">重新導向記錄檔</span><span class="sxs-lookup"><span data-stu-id="743d7-109">Redirecting Logs</span></span>

<span data-ttu-id="743d7-110">而不是寫入記錄檔以[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)，可提供替代的記錄的實作，例如可以記錄到記錄檔管理員直接[Log4Net](http://logging.apache.org/log4net/)和[NLog](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="743d7-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="743d7-111">只需提供的實作[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)和向您選擇的相依性插入引擎，它會取得收取的 Microsoft ASP.NET Webhook。</span><span class="sxs-lookup"><span data-stu-id="743d7-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="743d7-112">請參閱[ASP.NET Web API 2 中的相依性插入](https://www.asp.net/web-api/overview/advanced/dependency-injection)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="743d7-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
