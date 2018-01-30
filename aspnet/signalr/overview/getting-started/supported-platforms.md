---
uid: signalr/overview/getting-started/supported-platforms
title: "支援的平台 |Microsoft 文件"
author: pfletcher
description: "本文說明 SignalR 支援哪些用戶端和伺服器。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 1379b9fb638f67896d88d7aa4312d95280ef7318
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="supported-platforms"></a><span data-ttu-id="1aafb-103">支援的平台</span><span class="sxs-lookup"><span data-stu-id="1aafb-103">Supported Platforms</span></span>
====================
<span data-ttu-id="1aafb-104">由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1aafb-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="1aafb-105">本文說明 SignalR 支援哪些用戶端和伺服器。</span><span class="sxs-lookup"><span data-stu-id="1aafb-105">This article describes what clients and servers are supported by SignalR.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="1aafb-106">問題和註解</span><span class="sxs-lookup"><span data-stu-id="1aafb-106">Questions and comments</span></span>
> 
> <span data-ttu-id="1aafb-107">請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="1aafb-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1aafb-108">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="1aafb-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="1aafb-109">SignalR 被支援在各種不同的伺服器和用戶端設定。</span><span class="sxs-lookup"><span data-stu-id="1aafb-109">SignalR is supported under a variety of server and client configurations.</span></span> <span data-ttu-id="1aafb-110">此外，每個傳輸選項具有一組自己的; 需求如果無法使用的傳輸的系統需求，則 SignalR 依正常程序會容錯移轉至其他傳輸。</span><span class="sxs-lookup"><span data-stu-id="1aafb-110">In addition, each transport option has a set of requirements of its own; if the system requirements for a transport are not available, SignalR will gracefully failover to other transports.</span></span> <span data-ttu-id="1aafb-111">如需有關支援 SignalR 的傳輸的詳細資訊，請參閱[傳輸和後援](introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="1aafb-111">For more information on the transports that SignalR supports, see [Transports and Fallbacks](introduction-to-signalr.md#transports).</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="1aafb-112">伺服器系統需求</span><span class="sxs-lookup"><span data-stu-id="1aafb-112">Server system requirements</span></span>

<span data-ttu-id="1aafb-113">SignalR 的伺服器元件可裝載於各種不同的伺服器設定。</span><span class="sxs-lookup"><span data-stu-id="1aafb-113">The SignalR server component can be hosted on a variety of server configurations.</span></span> <span data-ttu-id="1aafb-114">本章節描述支援的作業系統、.NET framework、 Internet Information Server，以及其他元件的版本。</span><span class="sxs-lookup"><span data-stu-id="1aafb-114">This section describes the supported versions of operating systems, .NET framework, Internet Information Server, and other components.</span></span>

### <a name="supported-server-operating-systems"></a><span data-ttu-id="1aafb-115">支援的伺服器作業系統</span><span class="sxs-lookup"><span data-stu-id="1aafb-115">Supported server operating systems</span></span>

<span data-ttu-id="1aafb-116">SignalR 的伺服器元件可以裝載於下列伺服器或用戶端作業系統。</span><span class="sxs-lookup"><span data-stu-id="1aafb-116">The SignalR server component can be hosted in the following server or client operating systems.</span></span> <span data-ttu-id="1aafb-117">請注意，適用於 SignalR 使用 WebSockets，Windows Server 2012 或 Windows 8 （WebSocket 可以使用 Windows Azure 網站上，只要設定站台的.NET framework 版本為 4.5，而且站台的組態頁面中已啟用 Web 通訊端）。</span><span class="sxs-lookup"><span data-stu-id="1aafb-117">Note that for SignalR to use WebSockets, Windows Server 2012 or Windows 8 is required (WebSocket can be used on Windows Azure Web Sites, as long as the site's .NET framework version is set to 4.5, and Web Sockets is enabled in the site's Configuration page).</span></span>

- <span data-ttu-id="1aafb-118">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="1aafb-118">Windows Server 2012</span></span>
- <span data-ttu-id="1aafb-119">Windows Server 2008 r2</span><span class="sxs-lookup"><span data-stu-id="1aafb-119">Windows Server 2008 r2</span></span>
- <span data-ttu-id="1aafb-120">Windows 10</span><span class="sxs-lookup"><span data-stu-id="1aafb-120">Windows 10</span></span>
- <span data-ttu-id="1aafb-121">Windows 8</span><span class="sxs-lookup"><span data-stu-id="1aafb-121">Windows 8</span></span>
- <span data-ttu-id="1aafb-122">Windows 7</span><span class="sxs-lookup"><span data-stu-id="1aafb-122">Windows 7</span></span>
- <span data-ttu-id="1aafb-123">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="1aafb-123">Windows Azure</span></span>

### <a name="supported-server-net-framework-version"></a><span data-ttu-id="1aafb-124">支援的伺服器.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="1aafb-124">Supported server .NET Framework version</span></span>

<span data-ttu-id="1aafb-125">SignalR 2 才支援在.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="1aafb-125">SignalR 2 is only supported on .NET Framework 4.5.</span></span> <span data-ttu-id="1aafb-126">請參閱[建議更新](#updates)加強可靠性、 相容性、 穩定性和效能的更新 > 一節。</span><span class="sxs-lookup"><span data-stu-id="1aafb-126">See the [Recommended Updates](#updates) section for updates that enhance reliability, compatibility, stability, and performance.</span></span>

### <a name="supported-server-iis-versions"></a><span data-ttu-id="1aafb-127">支援的伺服器的 IIS 版本</span><span class="sxs-lookup"><span data-stu-id="1aafb-127">Supported server IIS versions</span></span>

<span data-ttu-id="1aafb-128">當 SignalR 裝載在 IIS 中時，支援下列版本。</span><span class="sxs-lookup"><span data-stu-id="1aafb-128">When SignalR is hosted in IIS, the following versions are supported.</span></span> <span data-ttu-id="1aafb-129">請注意，是否使用用戶端作業系統，例如開發 （Windows 8 或 Windows 7），完整版本的 IIS 或 Cassini 不應，因為會有 10 個同時連線數加諸，這會非常快速地到達後連線的限制暫時性，經常重新建立，而且不會處置立即時不再使用。</span><span class="sxs-lookup"><span data-stu-id="1aafb-129">Note that if a client operating system is used, such as for development (Windows 8 or Windows 7), full versions of IIS or Cassini should not be used, since there will be a limit of 10 simultaneous connections imposed, which will be reached very quickly since connections are transient, frequently re-established, and are not disposed immediately upon no longer being used.</span></span> <span data-ttu-id="1aafb-130">應該使用 IIS Express，用戶端作業系統上。</span><span class="sxs-lookup"><span data-stu-id="1aafb-130">IIS Express should be used on client operating systems.</span></span>

<span data-ttu-id="1aafb-131">也請注意，SignalR 使用 WebSocket，必須使用 IIS 8 或 IIS 8 Express，伺服器必須使用 Windows 8、 Windows Server 2012 或更新版本，而且必須在 IIS 中啟用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="1aafb-131">Also note that for SignalR to use WebSocket, IIS 8 or IIS 8 Express must be used, the server must be using Windows 8, Windows Server 2012, or later, and WebSocket must be enabled in IIS.</span></span> <span data-ttu-id="1aafb-132">如需如何啟用 WebSocket，在 IIS 中的資訊，請參閱[IIS 8.0 WebSocket 通訊協定支援](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。</span><span class="sxs-lookup"><span data-stu-id="1aafb-132">For information on how to enable WebSocket in IIS, see [IIS 8.0 WebSocket Protocol Support](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

- <span data-ttu-id="1aafb-133">IIS 8 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="1aafb-133">IIS 8 or IIS 8 Express.</span></span>
- <span data-ttu-id="1aafb-134">IIS 7 和 7.5。</span><span class="sxs-lookup"><span data-stu-id="1aafb-134">IIS 7 and 7.5.</span></span> <span data-ttu-id="1aafb-135">支援[無副檔名 Url](https://support.microsoft.com/kb/980368)需要。</span><span class="sxs-lookup"><span data-stu-id="1aafb-135">Support for [extensionless URLs](https://support.microsoft.com/kb/980368) is required.</span></span>
- <span data-ttu-id="1aafb-136">IIS 必須執行整合式模式，不支援傳統模式。</span><span class="sxs-lookup"><span data-stu-id="1aafb-136">IIS must be running in integrated mode; classic mode is not supported.</span></span> <span data-ttu-id="1aafb-137">如果 IIS 在使用 Server-Sent 事件傳輸的傳統模式中執行時可能會遇到的最多 30 秒的訊息延遲。</span><span class="sxs-lookup"><span data-stu-id="1aafb-137">Message delays of up to 30 seconds may be experienced if IIS is run in classic mode using the Server-Sent Events transport.</span></span>
- <span data-ttu-id="1aafb-138">裝載的應用程式必須在完全信任模式下執行。</span><span class="sxs-lookup"><span data-stu-id="1aafb-138">The hosting application must be running in full trust mode.</span></span>

## <a name="client-system-requirements"></a><span data-ttu-id="1aafb-139">用戶端系統需求</span><span class="sxs-lookup"><span data-stu-id="1aafb-139">Client system requirements</span></span>

<span data-ttu-id="1aafb-140">SignalR 可用各種不同的用戶端平台。</span><span class="sxs-lookup"><span data-stu-id="1aafb-140">SignalR can be used in a variety of client platforms.</span></span> <span data-ttu-id="1aafb-141">本章節描述使用 SignalR 在網頁瀏覽器、 Windows 桌面應用程式、 Silverlight 應用程式和行動裝置的系統需求。</span><span class="sxs-lookup"><span data-stu-id="1aafb-141">This section describes the system requirements for using SignalR in web browsers, Windows desktop applications, Silverlight applications, and mobile devices.</span></span>

### <a name="web-browsers"></a><span data-ttu-id="1aafb-142">網頁瀏覽器</span><span class="sxs-lookup"><span data-stu-id="1aafb-142">Web browsers</span></span>

<span data-ttu-id="1aafb-143">SignalR 可用於各種不同的網頁瀏覽器，但一般而言，只有最新的兩個版本支援。</span><span class="sxs-lookup"><span data-stu-id="1aafb-143">SignalR can be used in a variety of web browsers, but typically, only the latest two versions are supported.</span></span>

<span data-ttu-id="1aafb-144">使用瀏覽器中的 SignalR 應用程式必須使用 jQuery 版本 1.6.4 或重大更新的版本 （例如 1.7.2、 1.8.2 或 1.9.1）。</span><span class="sxs-lookup"><span data-stu-id="1aafb-144">Applications that use SignalR in browsers must use jQuery version 1.6.4 or major later versions (such as 1.7.2, 1.8.2, or 1.9.1).</span></span>

<span data-ttu-id="1aafb-145">SignalR 可以用於下列瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="1aafb-145">SignalR can be used in the following browsers:</span></span>

- <span data-ttu-id="1aafb-146">Microsoft Internet Explorer 版本 8、 9、 10 和 11。</span><span class="sxs-lookup"><span data-stu-id="1aafb-146">Microsoft Internet Explorer versions 8, 9, 10, and 11.</span></span> <span data-ttu-id="1aafb-147">現代化、 桌面和行動裝置的版本支援。</span><span class="sxs-lookup"><span data-stu-id="1aafb-147">Modern, Desktop, and Mobile versions are supported.</span></span>
- <span data-ttu-id="1aafb-148">Mozilla Firefox： 目前版本-1，Windows 和 Mac 的版本。</span><span class="sxs-lookup"><span data-stu-id="1aafb-148">Mozilla Firefox: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="1aafb-149">Google Chrome： 目前版本-1，Windows 和 Mac 的版本。</span><span class="sxs-lookup"><span data-stu-id="1aafb-149">Google Chrome: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="1aafb-150">Safari： 目前版本-1，iOS 和 Mac 的版本。</span><span class="sxs-lookup"><span data-stu-id="1aafb-150">Safari: current version - 1, both Mac and iOS versions.</span></span>
- <span data-ttu-id="1aafb-151">Opera： 目前版本-1，僅限 Windows。</span><span class="sxs-lookup"><span data-stu-id="1aafb-151">Opera: current version - 1, Windows only.</span></span>
- <span data-ttu-id="1aafb-152">Android 瀏覽器</span><span class="sxs-lookup"><span data-stu-id="1aafb-152">Android browser</span></span>

<span data-ttu-id="1aafb-153">除了需要某些瀏覽器，SignalR 使用各種傳輸會具有自己的需求。</span><span class="sxs-lookup"><span data-stu-id="1aafb-153">In addition to requiring certain browsers, the various transports that SignalR uses have requirements of their own.</span></span> <span data-ttu-id="1aafb-154">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="1aafb-154">The following transports are supported under the following configurations:</span></span>

<a id="browser"></a>

<span data-ttu-id="1aafb-155">**網頁瀏覽器傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="1aafb-155">**Web Browser Transport Requirements**</span></span>

| <span data-ttu-id="1aafb-156">Transport</span><span class="sxs-lookup"><span data-stu-id="1aafb-156">Transport</span></span> | <span data-ttu-id="1aafb-157">Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="1aafb-157">Internet Explorer</span></span> | <span data-ttu-id="1aafb-158">Chrome （Windows 或 iOS）</span><span class="sxs-lookup"><span data-stu-id="1aafb-158">Chrome (Windows or iOS)</span></span> | <span data-ttu-id="1aafb-159">Firefox</span><span class="sxs-lookup"><span data-stu-id="1aafb-159">Firefox</span></span> | <span data-ttu-id="1aafb-160">Safari （OSX 或 iOS）</span><span class="sxs-lookup"><span data-stu-id="1aafb-160">Safari (OSX or iOS)</span></span> | <span data-ttu-id="1aafb-161">Android</span><span class="sxs-lookup"><span data-stu-id="1aafb-161">Android</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="1aafb-162">WebSockets</span><span class="sxs-lookup"><span data-stu-id="1aafb-162">WebSockets</span></span> | <span data-ttu-id="1aafb-163">10+</span><span class="sxs-lookup"><span data-stu-id="1aafb-163">10+</span></span> | <span data-ttu-id="1aafb-164">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-164">current - 1</span></span> | <span data-ttu-id="1aafb-165">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-165">current - 1</span></span> | <span data-ttu-id="1aafb-166">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-166">current - 1</span></span> | <span data-ttu-id="1aafb-167">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-167">N/A</span></span> |
| <span data-ttu-id="1aafb-168">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="1aafb-168">Server-Sent Events</span></span> | <span data-ttu-id="1aafb-169">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-169">N/A</span></span> | <span data-ttu-id="1aafb-170">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-170">current - 1</span></span> | <span data-ttu-id="1aafb-171">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-171">current - 1</span></span> | <span data-ttu-id="1aafb-172">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-172">current - 1</span></span> | <span data-ttu-id="1aafb-173">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-173">N/A</span></span> |
| <span data-ttu-id="1aafb-174">ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="1aafb-174">ForeverFrame</span></span> | <span data-ttu-id="1aafb-175">8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-175">8+</span></span> | <span data-ttu-id="1aafb-176">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-176">N/A</span></span> | <span data-ttu-id="1aafb-177">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-177">N/A</span></span> | <span data-ttu-id="1aafb-178">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-178">N/A</span></span> | <span data-ttu-id="1aafb-179">4.1</span><span class="sxs-lookup"><span data-stu-id="1aafb-179">4.1</span></span> |
| <span data-ttu-id="1aafb-180">長輪詢</span><span class="sxs-lookup"><span data-stu-id="1aafb-180">Long Polling</span></span> | <span data-ttu-id="1aafb-181">8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-181">8+</span></span> | <span data-ttu-id="1aafb-182">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-182">current - 1</span></span> | <span data-ttu-id="1aafb-183">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-183">current - 1</span></span> | <span data-ttu-id="1aafb-184">current-1</span><span class="sxs-lookup"><span data-stu-id="1aafb-184">current - 1</span></span> | <span data-ttu-id="1aafb-185">4.1</span><span class="sxs-lookup"><span data-stu-id="1aafb-185">4.1</span></span> |

<span data-ttu-id="1aafb-186">\*: 6 + 所需的完整功能。</span><span class="sxs-lookup"><span data-stu-id="1aafb-186">\*: 6+ required for full functionality.</span></span>

#### <a name="unsupported-browsers"></a><span data-ttu-id="1aafb-187">不支援的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="1aafb-187">Unsupported Browsers</span></span>

<span data-ttu-id="1aafb-188">雖然 SignalR*可能*舊版瀏覽器中的主要問題未執行，我們不要主動測試 SignalR 的通常不會修正可能會出現在它們的 bug。</span><span class="sxs-lookup"><span data-stu-id="1aafb-188">While SignalR *may* run without major issues in older browser versions, we do not actively test SignalR in them and generally do not fix bugs that may appear in them.</span></span>

### <a name="windows-desktop-and-silverlight-applications"></a><span data-ttu-id="1aafb-189">Windows 桌面和 Silverlight 應用程式</span><span class="sxs-lookup"><span data-stu-id="1aafb-189">Windows Desktop and Silverlight Applications</span></span>

<span data-ttu-id="1aafb-190">除了在網頁瀏覽器中執行，SignalR 可以裝載於獨立的 Windows 用戶端或 Silverlight 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1aafb-190">In addition to running in a web browser, SignalR can be hosted in standalone Windows client or Silverlight applications.</span></span> <span data-ttu-id="1aafb-191">Windows 桌面和 Silverlight SignalR 應用程式有下列的系統需求。</span><span class="sxs-lookup"><span data-stu-id="1aafb-191">Windows Desktop and Silverlight SignalR applications have the following system requirements.</span></span>

- <span data-ttu-id="1aafb-192">在 Windows XP SP3 或更新版本支援使用.NET 4 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1aafb-192">Applications using .NET 4 are supported on Windows XP SP3 or later.</span></span>
- <span data-ttu-id="1aafb-193">在 Windows Vista 或更新版本支援使用.NET Framework 4.5 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="1aafb-193">Applications using .NET Framework 4.5 are supported on Windows Vista or later.</span></span>

<span data-ttu-id="1aafb-194">除了作業系統和.NET framework 需求，SignalR 使用的傳輸會有自己的需求。</span><span class="sxs-lookup"><span data-stu-id="1aafb-194">In addition to operating system and .NET framework requirements, the transports available to SignalR have requirements of their own.</span></span> <span data-ttu-id="1aafb-195">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="1aafb-195">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="1aafb-196">**Windows 桌面和 Silverlight 傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="1aafb-196">**Windows Desktop and Silverlight Transport Requirements**</span></span>

| <span data-ttu-id="1aafb-197">Transport</span><span class="sxs-lookup"><span data-stu-id="1aafb-197">Transport</span></span> | <span data-ttu-id="1aafb-198">.NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="1aafb-198">.NET application</span></span> | <span data-ttu-id="1aafb-199">Silverlight</span><span class="sxs-lookup"><span data-stu-id="1aafb-199">Silverlight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1aafb-200">Web 通訊端</span><span class="sxs-lookup"><span data-stu-id="1aafb-200">Web Sockets</span></span> | <span data-ttu-id="1aafb-201">Windows 8 + 和.NET 4.5 +</span><span class="sxs-lookup"><span data-stu-id="1aafb-201">Windows 8+ and .NET 4.5+</span></span> | <span data-ttu-id="1aafb-202">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-202">N/A</span></span> |
| <span data-ttu-id="1aafb-203">永久框架</span><span class="sxs-lookup"><span data-stu-id="1aafb-203">Forever Frame</span></span> | <span data-ttu-id="1aafb-204">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-204">N/A</span></span> | <span data-ttu-id="1aafb-205">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-205">N/A</span></span> |
| <span data-ttu-id="1aafb-206">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="1aafb-206">Server-Sent Events</span></span> | <span data-ttu-id="1aafb-207">.NET 4+</span><span class="sxs-lookup"><span data-stu-id="1aafb-207">.NET 4+</span></span> | <span data-ttu-id="1aafb-208">5+</span><span class="sxs-lookup"><span data-stu-id="1aafb-208">5+</span></span> |
| <span data-ttu-id="1aafb-209">長輪詢</span><span class="sxs-lookup"><span data-stu-id="1aafb-209">Long Polling</span></span> | <span data-ttu-id="1aafb-210">.NET 4+</span><span class="sxs-lookup"><span data-stu-id="1aafb-210">.NET 4+</span></span> | <span data-ttu-id="1aafb-211">5+</span><span class="sxs-lookup"><span data-stu-id="1aafb-211">5+</span></span> |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a><span data-ttu-id="1aafb-212">Windows 市集和 Windows Phone 應用程式</span><span class="sxs-lookup"><span data-stu-id="1aafb-212">Windows Store and Windows Phone Applications</span></span>

<span data-ttu-id="1aafb-213">SignalR 可以用於 Windows 市集應用程式和 Windows Phone 8 應用程式。</span><span class="sxs-lookup"><span data-stu-id="1aafb-213">SignalR can be used in Windows Store applications and Windows Phone 8 applications.</span></span> <span data-ttu-id="1aafb-214">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="1aafb-214">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="1aafb-215">**Windows 市集和 Windows Phone 傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="1aafb-215">**Windows Store and Windows Phone Transport Requirements**</span></span>

| <span data-ttu-id="1aafb-216">Transport</span><span class="sxs-lookup"><span data-stu-id="1aafb-216">Transport</span></span> | <span data-ttu-id="1aafb-217">Windows 市集 /.NET</span><span class="sxs-lookup"><span data-stu-id="1aafb-217">Windows Store/ .NET</span></span> | <span data-ttu-id="1aafb-218">Windows 市集 / JavaScript</span><span class="sxs-lookup"><span data-stu-id="1aafb-218">Windows Store/ JavaScript</span></span> | <span data-ttu-id="1aafb-219">Windows Phone/ IE</span><span class="sxs-lookup"><span data-stu-id="1aafb-219">Windows Phone/ IE</span></span> | <span data-ttu-id="1aafb-220">Windows Phone/ .NET</span><span class="sxs-lookup"><span data-stu-id="1aafb-220">Windows Phone/ .NET</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="1aafb-221">WebSockets</span><span class="sxs-lookup"><span data-stu-id="1aafb-221">WebSockets</span></span> | <span data-ttu-id="1aafb-222">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-222">N/A</span></span> | <span data-ttu-id="1aafb-223">Win8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-223">Win8+</span></span> | <span data-ttu-id="1aafb-224">8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-224">8+</span></span> | <span data-ttu-id="1aafb-225">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-225">N/A</span></span> |
| <span data-ttu-id="1aafb-226">永久框架</span><span class="sxs-lookup"><span data-stu-id="1aafb-226">Forever Frame</span></span> | <span data-ttu-id="1aafb-227">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-227">N/A</span></span> | <span data-ttu-id="1aafb-228">Win8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-228">Win8+</span></span> | <span data-ttu-id="1aafb-229">7.5+</span><span class="sxs-lookup"><span data-stu-id="1aafb-229">7.5+</span></span> | <span data-ttu-id="1aafb-230">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-230">N/A</span></span> |
| <span data-ttu-id="1aafb-231">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="1aafb-231">Server-Sent Events</span></span> | <span data-ttu-id="1aafb-232">Win8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-232">Win8+</span></span> | <span data-ttu-id="1aafb-233">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-233">N/A</span></span> | <span data-ttu-id="1aafb-234">N/A</span><span class="sxs-lookup"><span data-stu-id="1aafb-234">N/A</span></span> | <span data-ttu-id="1aafb-235">8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-235">8+</span></span> |
| <span data-ttu-id="1aafb-236">長輪詢</span><span class="sxs-lookup"><span data-stu-id="1aafb-236">Long Polling</span></span> | <span data-ttu-id="1aafb-237">Win8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-237">Win8+</span></span> | <span data-ttu-id="1aafb-238">Win8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-238">Win8+</span></span> | <span data-ttu-id="1aafb-239">7.5+</span><span class="sxs-lookup"><span data-stu-id="1aafb-239">7.5+</span></span> | <span data-ttu-id="1aafb-240">8+</span><span class="sxs-lookup"><span data-stu-id="1aafb-240">8+</span></span> |

<a id="updates"></a>

## <a name="recommended-updates"></a><span data-ttu-id="1aafb-241">建議的更新</span><span class="sxs-lookup"><span data-stu-id="1aafb-241">Recommended Updates</span></span>

<span data-ttu-id="1aafb-242">對於 SignalR 伺服器建議下列更新：</span><span class="sxs-lookup"><span data-stu-id="1aafb-242">The following updates are recommended for SignalR servers:</span></span>

- <span data-ttu-id="1aafb-243">可更新的.NET Framework 4.5 功能[這裡](https://support.microsoft.com/kb/2750149)。</span><span class="sxs-lookup"><span data-stu-id="1aafb-243">An update for .NET Framework 4.5 is available [here](https://support.microsoft.com/kb/2750149).</span></span>
- <span data-ttu-id="1aafb-244">Microsoft asp.net，會定期發行 Qfe。</span><span class="sxs-lookup"><span data-stu-id="1aafb-244">Microsoft will periodically release QFEs for ASP.NET.</span></span> <span data-ttu-id="1aafb-245">這些應該套用為可用。</span><span class="sxs-lookup"><span data-stu-id="1aafb-245">These should be applied as available.</span></span>
