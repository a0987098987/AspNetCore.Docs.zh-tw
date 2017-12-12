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
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a>記錄的 ASP.NET Webhook

Microsoft ASP.NET Webhook 會使用這種報告所發生的問題記錄。 根據預設記錄檔會寫入使用[System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace)其中，它們可以能夠使用[追蹤接聽項](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx)像任何其他記錄檔資料流。

在部署 Web 應用程式當做 Azure Web 應用程式時，記錄檔自動挑選，並可管理以及任何其他[System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace)記錄。 如需詳細資訊，請參閱[啟用 Azure App Service 中的 web 應用程式的診斷記錄](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)

此外，取得記錄檔，直接從 Visual Studio 中所述內部[疑難排解 web 應用程式中使用 Visual Studio 的 Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="redirecting-logs"></a>重新導向記錄檔

而不是寫入記錄檔以[System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace)，可提供替代的記錄的實作，例如可以記錄到記錄檔管理員直接[Log4Net](http://logging.apache.org/log4net/)和[NLog](http://nlog-project.org/). 只需提供的實作[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)和向您選擇的相依性插入引擎，它會取得收取的 Microsoft ASP.NET Webhook。 請參閱[ASP.NET Web API 2 中的相依性插入](https://www.asp.net/web-api/overview/advanced/dependency-injection)如需詳細資訊。
