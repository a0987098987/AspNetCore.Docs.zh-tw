---
uid: webhooks/diagnostics/logging
title: ASP.NET Webhook 記錄 |Microsoft Docs
author: rick-anderson
description: 如何登入 ASP.NET Webhook。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 2e86d519c24da102075b4da0a32787c90deb0f6b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825513"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET Webhook 記錄

Microsoft ASP.NET Webhook 會使用這種報告所發生的問題記錄。 根據預設記錄檔會寫入使用[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)受管理的它們可以使用[追蹤接聽項](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx)像任何其他的記錄資料流。

在部署您的 Web 應用程式，為 Azure Web 應用程式時，記錄檔會自動挑選，且可管理以及任何其他[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)記錄。 如需詳細資訊，請參閱[針對 Azure App Service 中的 web 應用程式啟用診斷記錄](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

此外，取得記錄檔，直接從 Visual studio 中所述[疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="redirecting-logs"></a>重新導向的記錄檔

而不是寫入記錄檔，以[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)，就能夠提供其他記錄的實作，這類可以記錄到記錄檔管理員直接[Log4Net](http://logging.apache.org/log4net/)和[NLog](http://nlog-project.org/). 只需提供的實作[ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs)向您選擇的相依性插入引擎和它將會由 Microsoft ASP.NET Webhook。 請參閱[ASP.NET Web API 2 中的相依性插入](https://www.asp.net/web-api/overview/advanced/dependency-injection)如需詳細資訊。
