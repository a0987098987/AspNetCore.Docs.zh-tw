---
title: ASP.NET Core 中的 Web 主機與泛型主機
author: guardrex
description: 深入了解 ASP.NET Core 的 Web 代管以及 .NET 的一般代管，其負責啟動應用程式以及進行存留期間的管理。
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121514"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="02698-103">ASP.NET Core 中的 Web 主機與泛型主機</span><span class="sxs-lookup"><span data-stu-id="02698-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="02698-104">.NET 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="02698-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="02698-105">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="02698-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="02698-106">提供兩種 API 可供使用：</span><span class="sxs-lookup"><span data-stu-id="02698-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="02698-107">[Web 代管](xref:fundamentals/host/web-host) &ndash; 適合代管 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="02698-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="02698-108">[一般代管](xref:fundamentals/host/generic-host) (ASP .NET Core 2.1 或更新版本) &ndash; 適合代管非 Web 應用程式 (例如，執行背景工作的應用程式)。</span><span class="sxs-lookup"><span data-stu-id="02698-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="02698-109">在後續版本中，一般代管將適用於代管任何種類的應用程式，包括 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="02698-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="02698-110">一般代管最後將會取代 Web 代管。</span><span class="sxs-lookup"><span data-stu-id="02698-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="02698-111">若要裝載 ASP.NET Core「Web 應用程式」，開發人員應該使用以 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 為基礎的 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="02698-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="02698-112">若要裝載 ASP.NET Core「非 Web 應用程式」，開發人員應使用以 <xref:Microsoft.Extensions.Hosting.HostBuilder> 為基礎的一般主機。</span><span class="sxs-lookup"><span data-stu-id="02698-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="02698-113">了解如何在 ASP.NET Core 中使用託管服務實作背景工作。</span><span class="sxs-lookup"><span data-stu-id="02698-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="02698-114">了解如何使用 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 實作，從參考或非參考的組件增強 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="02698-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
