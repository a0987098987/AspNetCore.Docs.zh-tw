---
title: 在 ASP.NET Core 中代管
author: guardrex
description: 深入了解 ASP.NET Core 的 Web 代管以及 .NET 的一般代管，其負責啟動應用程式以及進行存留期間的管理。
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276613"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="85ec2-103">在 ASP.NET Core 中代管</span><span class="sxs-lookup"><span data-stu-id="85ec2-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="85ec2-104">.NET 應用程式會設定並啟動「主機」。</span><span class="sxs-lookup"><span data-stu-id="85ec2-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="85ec2-105">主機負責應用程式啟動和存留期管理。</span><span class="sxs-lookup"><span data-stu-id="85ec2-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="85ec2-106">提供兩種 API 可供使用：</span><span class="sxs-lookup"><span data-stu-id="85ec2-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="85ec2-107">[Web 代管](xref:fundamentals/host/web-host) &ndash; 適合代管 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="85ec2-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="85ec2-108">[一般代管](xref:fundamentals/host/generic-host) (ASP .NET Core 2.1 或更新版本) &ndash; 適合代管非 Web 應用程式 (例如，執行背景工作的應用程式)。</span><span class="sxs-lookup"><span data-stu-id="85ec2-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="85ec2-109">在後續版本中，一般代管將適用於代管任何種類的應用程式，包括 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="85ec2-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="85ec2-110">一般代管最後將會取代 Web 代管。</span><span class="sxs-lookup"><span data-stu-id="85ec2-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="85ec2-111">若要裝載 ASP.NET Core「Web 應用程式」，開發人員應使用以 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 為基礎的 Web 主機。</span><span class="sxs-lookup"><span data-stu-id="85ec2-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="85ec2-112">若要裝載 ASP.NET Core「非 Web 應用程式」，開發人員應使用以 [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) 為基礎的一般主機。</span><span class="sxs-lookup"><span data-stu-id="85ec2-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
