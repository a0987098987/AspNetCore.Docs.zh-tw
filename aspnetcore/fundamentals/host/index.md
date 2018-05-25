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
# <a name="host-in-aspnet-core"></a><span data-ttu-id="271b0-103">在 ASP.NET Core 中代管</span><span class="sxs-lookup"><span data-stu-id="271b0-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="271b0-104">.NET 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="271b0-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="271b0-105">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="271b0-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="271b0-106">提供兩種 API 可供使用：</span><span class="sxs-lookup"><span data-stu-id="271b0-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="271b0-107">[Web 代管](xref:fundamentals/host/web-host) &ndash; 適合代管 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="271b0-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="271b0-108">[一般代管](xref:fundamentals/host/generic-host) (ASP .NET Core 2.1 或更新版本) &ndash; 適合代管非 Web 應用程式 (例如，執行背景工作的應用程式)。</span><span class="sxs-lookup"><span data-stu-id="271b0-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="271b0-109">在後續版本中，一般代管將適用於代管任何種類的應用程式，包括 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="271b0-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="271b0-110">一般代管最後將會取代 Web 代管。</span><span class="sxs-lookup"><span data-stu-id="271b0-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="271b0-111">開發人員於此階段應使用 [Web 代管](xref:fundamentals/host/web-host) (以代管 ASP.NET Core 應用程式的 [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 為基礎)。</span><span class="sxs-lookup"><span data-stu-id="271b0-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
