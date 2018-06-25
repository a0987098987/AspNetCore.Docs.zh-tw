---
title: ASP.NET Core 模組
author: rick-anderson
description: 了解 ASP.NET Core 模組如何讓 Kestrel Web 伺服器將 IIS 或 IIS Express 作為反向 Proxy 伺服器使用。
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 319ab80f20f3917dae921f43566e368b51c29f0b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272331"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="1bc77-103">ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="1bc77-103">ASP.NET Core Module</span></span>

<span data-ttu-id="1bc77-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Rick Strahl](https://github.com/RickStrahl) 和 [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="1bc77-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="1bc77-105">ASP.NET Core 模組可讓 ASP.NET Core 應用程式在反向 Proxy 組態中的 IIS 後方 執行。</span><span class="sxs-lookup"><span data-stu-id="1bc77-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="1bc77-106">IIS 提供進階的 Web 應用程式安全性和管理能力功能。</span><span class="sxs-lookup"><span data-stu-id="1bc77-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="1bc77-107">支援的 Windows 版本：</span><span class="sxs-lookup"><span data-stu-id="1bc77-107">Supported Windows versions:</span></span>

* <span data-ttu-id="1bc77-108">Windows 7 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1bc77-108">Windows 7 or later</span></span>
* <span data-ttu-id="1bc77-109">Windows Server 2008 R2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="1bc77-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1bc77-110">ASP.NET Core 模組只適用於 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="1bc77-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="1bc77-111">該模組與 [HTTP.sys](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不相容。</span><span class="sxs-lookup"><span data-stu-id="1bc77-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="1bc77-112">ASP.NET Core 模組說明</span><span class="sxs-lookup"><span data-stu-id="1bc77-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="1bc77-113">ASP.NET Core 模組是一種原生 IIS 模組，可連到 IIS 管線，將 Web 要求重新導向至後端的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1bc77-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="1bc77-114">許多如 Windows 驗證等原生模組仍在使用中。</span><span class="sxs-lookup"><span data-stu-id="1bc77-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="1bc77-115">若要深入了解搭配此模組的使用中 IIS 模組，請參閱 [IIS 模組](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="1bc77-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="1bc77-116">因為處理序中執行的 ASP.NET Core 應用程式會與 IIS 背景工作處理序分開，所以此模組也會執行處理序管理。</span><span class="sxs-lookup"><span data-stu-id="1bc77-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="1bc77-117">此模組會在第一個要求到達時啟動 ASP.NET Core 應用程式的處理序，並在應用程式損毀時將它重新啟動。</span><span class="sxs-lookup"><span data-stu-id="1bc77-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="1bc77-118">此行為基本上與在 IIS 中執行同處理序，並由 [Windows 處理器啟用服務 (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) 所管理的 ASP.NET 4.x 應用程式相同。</span><span class="sxs-lookup"><span data-stu-id="1bc77-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="1bc77-119">下圖說明 IIS、ASP.NET Core 模組和 ASP.NET Core 應用程式之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="1bc77-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![ASP.NET Core 模組](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="1bc77-121">要求會從 Web 到達核心模式的 HTTP.sys 驅動程式。</span><span class="sxs-lookup"><span data-stu-id="1bc77-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="1bc77-122">驅動程式會在網站設定的通訊埠上將要求路由至 IIS，此通訊埠通常是 80 (HTTP) 或 443 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="1bc77-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="1bc77-123">此模組會在應用程式的隨機通訊埠上將要求轉送至 Kestrel，這並不是通訊埠 80/443。</span><span class="sxs-lookup"><span data-stu-id="1bc77-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="1bc77-124">此模組在啟動時透過環境變數指定通訊埠，而 IIS 整合中介軟體則會設定伺服器來接聽 `http://localhost:{port}`。</span><span class="sxs-lookup"><span data-stu-id="1bc77-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="1bc77-125">將會執行額外檢查，不是源自模組的要求都會遭到拒絕。</span><span class="sxs-lookup"><span data-stu-id="1bc77-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="1bc77-126">此模組不支援 HTTPS 轉送，因此即使由 IIS 透過 HTTPS 接收，要求還是會透過 HTTP 轉送。</span><span class="sxs-lookup"><span data-stu-id="1bc77-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="1bc77-127">Kestrel 收取來自模組的要求之後，要求會被推送至 ASP.NET Core 中介軟體管線。</span><span class="sxs-lookup"><span data-stu-id="1bc77-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="1bc77-128">中介軟體管線會處理要求，並將其作為 `HttpContext` 執行個體傳遞至應用程式的邏輯。</span><span class="sxs-lookup"><span data-stu-id="1bc77-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="1bc77-129">應用程式的回應會傳回 IIS，而 IIS 會將其推送回起始要求的 HTTP 用戶端。</span><span class="sxs-lookup"><span data-stu-id="1bc77-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="1bc77-130">ASP.NET Core 模組具有一些其他功能。</span><span class="sxs-lookup"><span data-stu-id="1bc77-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="1bc77-131">此模組可以：</span><span class="sxs-lookup"><span data-stu-id="1bc77-131">The module can:</span></span>

* <span data-ttu-id="1bc77-132">設定背景工作處理序的環境變數。</span><span class="sxs-lookup"><span data-stu-id="1bc77-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="1bc77-133">將 stdout 輸出記錄到檔案儲存區，以針對啟動問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="1bc77-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="1bc77-134">轉送 Windows 驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="1bc77-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="1bc77-135">如何安裝和使用 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="1bc77-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="1bc77-136">如需如何安裝和使用 ASP.NET Core 模組的詳細指示，請參閱 [Windows 上使用 IIS 的主機](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="1bc77-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="1bc77-137">如需設定模組的詳細資訊，請參閱 [ASP.NET Core 模組的組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="1bc77-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1bc77-138">其他資源</span><span class="sxs-lookup"><span data-stu-id="1bc77-138">Additional resources</span></span>

* [<span data-ttu-id="1bc77-139"> Windows 上使用 IIS 的主機</span><span class="sxs-lookup"><span data-stu-id="1bc77-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="1bc77-140">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="1bc77-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="1bc77-141">ASP.NET Core 模組 GitHub 存放庫 (原始程式碼)</span><span class="sxs-lookup"><span data-stu-id="1bc77-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
