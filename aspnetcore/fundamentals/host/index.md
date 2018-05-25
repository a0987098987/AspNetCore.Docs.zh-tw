---
title: 在 ASP.NET Core 中代管
author: guardrex
description: 深入了解 ASP.NET Core 的 Web 代管以及 .NET 的一般代管，其負責啟動應用程式以及進行存留期間的管理。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>在 ASP.NET Core 中代管

.NET 應用程式會設定並啟動「主機」。 主機負責應用程式啟動和存留期管理。 提供兩種 API 可供使用：

* [Web 代管](xref:fundamentals/host/web-host) &ndash; 適合代管 Web 應用程式。
* [一般代管](xref:fundamentals/host/generic-host) (ASP .NET Core 2.1 或更新版本) &ndash; 適合代管非 Web 應用程式 (例如，執行背景工作的應用程式)。 在後續版本中，一般代管將適用於代管任何種類的應用程式，包括 Web 應用程式。 一般代管最後將會取代 Web 代管。

開發人員於此階段應使用 [Web 代管](xref:fundamentals/host/web-host) (以代管 ASP.NET Core 應用程式的 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 為基礎)。
