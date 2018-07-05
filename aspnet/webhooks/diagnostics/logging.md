---
uid: webhooks/diagnostics/logging
title: ASP.NET Webhook 記錄 |Microsoft Docs
author: rick-anderson
description: 如何登入 ASP.NET Webhook。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 65e4d49474034406be835eb31378c81ba0706da3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828451"
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="1eb9e-103">ASP.NET Webhook 記錄</span><span class="sxs-lookup"><span data-stu-id="1eb9e-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="1eb9e-104">Microsoft ASP.NET Webhook 會使用這種報告所發生的問題記錄。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="1eb9e-105">根據預設記錄檔會寫入使用[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)受管理的它們可以使用[追蹤接聽項](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)像任何其他的記錄資料流。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="1eb9e-106">在部署您的 Web 應用程式，為 Azure Web 應用程式時，記錄檔會自動挑選，且可管理以及任何其他[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)記錄。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="1eb9e-107">如需詳細資訊，請參閱[針對 Azure App Service 中的 web 應用程式啟用診斷記錄](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="1eb9e-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="1eb9e-108">此外，取得記錄檔，直接從 Visual studio 中所述[疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="1eb9e-109">重新導向的記錄檔</span><span class="sxs-lookup"><span data-stu-id="1eb9e-109">Redirecting Logs</span></span>

<span data-ttu-id="1eb9e-110">而不是寫入記錄檔，以[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)，就能夠提供其他記錄的實作，這類可以記錄到記錄檔管理員直接[Log4Net](http://logging.apache.org/log4net/)和[NLog](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="1eb9e-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="1eb9e-111">只需提供的實作[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)向您選擇的相依性插入引擎和它將會由 Microsoft ASP.NET Webhook。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="1eb9e-112">請參閱[ASP.NET Web API 2 中的相依性插入](https://www.asp.net/web-api/overview/advanced/dependency-injection)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1eb9e-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
