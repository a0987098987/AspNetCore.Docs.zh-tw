---
uid: signalr/overview/getting-started/supported-platforms
title: 支援的平台 |Microsoft Docs
author: pfletcher
description: 此文章說明 SignalR 支援哪些用戶端和伺服器。
ms.author: riande
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: e270f9a328f36854fdfb3e23b78e0b40cdda6411
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287353"
---
<a name="supported-platforms"></a><span data-ttu-id="54392-103">支援的平台</span><span class="sxs-lookup"><span data-stu-id="54392-103">Supported Platforms</span></span>
====================
<span data-ttu-id="54392-104">藉由[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="54392-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="54392-105">此文章說明 SignalR 支援哪些用戶端和伺服器。</span><span class="sxs-lookup"><span data-stu-id="54392-105">This article describes what clients and servers are supported by SignalR.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="54392-106">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="54392-106">Questions and comments</span></span>
> 
> <span data-ttu-id="54392-107">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="54392-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="54392-108">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="54392-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="54392-109">SignalR 支援各種不同的伺服器和用戶端設定底下。</span><span class="sxs-lookup"><span data-stu-id="54392-109">SignalR is supported under a variety of server and client configurations.</span></span> <span data-ttu-id="54392-110">此外，每個傳輸選項有一組自己的; 的需求如果無法使用的傳輸的系統需求，則 SignalR 也能流暢地容錯移轉至其他傳輸。</span><span class="sxs-lookup"><span data-stu-id="54392-110">In addition, each transport option has a set of requirements of its own; if the system requirements for a transport are not available, SignalR will gracefully failover to other transports.</span></span> <span data-ttu-id="54392-111">如需有關支援 SignalR 的傳輸的詳細資訊，請參閱[傳輸和後援](introduction-to-signalr.md#transports)。</span><span class="sxs-lookup"><span data-stu-id="54392-111">For more information on the transports that SignalR supports, see [Transports and Fallbacks](introduction-to-signalr.md#transports).</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="54392-112">伺服器系統需求</span><span class="sxs-lookup"><span data-stu-id="54392-112">Server system requirements</span></span>

<span data-ttu-id="54392-113">SignalR 伺服器元件可裝載於各種不同的伺服器設定。</span><span class="sxs-lookup"><span data-stu-id="54392-113">The SignalR server component can be hosted on a variety of server configurations.</span></span> <span data-ttu-id="54392-114">本章節描述支援的作業系統、.NET framework、 Internet Information Server，以及其他元件的版本。</span><span class="sxs-lookup"><span data-stu-id="54392-114">This section describes the supported versions of operating systems, .NET framework, Internet Information Server, and other components.</span></span>

### <a name="supported-server-operating-systems"></a><span data-ttu-id="54392-115">支援的伺服器作業系統</span><span class="sxs-lookup"><span data-stu-id="54392-115">Supported server operating systems</span></span>

<span data-ttu-id="54392-116">SignalR 伺服器元件可裝載於下列伺服器或用戶端作業系統。</span><span class="sxs-lookup"><span data-stu-id="54392-116">The SignalR server component can be hosted in the following server or client operating systems.</span></span> <span data-ttu-id="54392-117">請注意，signalr 使用 WebSockets，Windows Server 2012、 Windows Server 2016 或 Windows 8 需要 （WebSocket 可以使用 Windows Azure 網站上，只要設定站台的.NET framework 版本為 4.5，而且在網站中啟用 Web 通訊端設定頁面）。</span><span class="sxs-lookup"><span data-stu-id="54392-117">Note that for SignalR to use WebSockets, Windows Server 2012, Windows Server 2016, or Windows 8 is required (WebSocket can be used on Windows Azure Web Sites, as long as the site's .NET framework version is set to 4.5, and Web Sockets is enabled in the site's Configuration page).</span></span>

- <span data-ttu-id="54392-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="54392-118">Windows Server 2016</span></span>
- <span data-ttu-id="54392-119">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="54392-119">Windows Server 2012</span></span>
- <span data-ttu-id="54392-120">Windows Server 2008 r2</span><span class="sxs-lookup"><span data-stu-id="54392-120">Windows Server 2008 r2</span></span>
- <span data-ttu-id="54392-121">Windows 10</span><span class="sxs-lookup"><span data-stu-id="54392-121">Windows 10</span></span>
- <span data-ttu-id="54392-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="54392-122">Windows 8</span></span>
- <span data-ttu-id="54392-123">Windows 7</span><span class="sxs-lookup"><span data-stu-id="54392-123">Windows 7</span></span>
- <span data-ttu-id="54392-124">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="54392-124">Windows Azure</span></span>

### <a name="supported-server-net-framework-version"></a><span data-ttu-id="54392-125">支援的伺服器的.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="54392-125">Supported server .NET Framework version</span></span>

<span data-ttu-id="54392-126">.NET Framework 4.5 上才支援 SignalR 2。</span><span class="sxs-lookup"><span data-stu-id="54392-126">SignalR 2 is only supported on .NET Framework 4.5.</span></span> <span data-ttu-id="54392-127">請參閱[建議的更新](#updates)增強可靠性、 相容性、 穩定性和效能的更新 區段。</span><span class="sxs-lookup"><span data-stu-id="54392-127">See the [Recommended Updates](#updates) section for updates that enhance reliability, compatibility, stability, and performance.</span></span>

### <a name="supported-server-iis-versions"></a><span data-ttu-id="54392-128">支援的伺服器的 IIS 版本</span><span class="sxs-lookup"><span data-stu-id="54392-128">Supported server IIS versions</span></span>

<span data-ttu-id="54392-129">SignalR 裝載在 IIS 中，會支援下列版本。</span><span class="sxs-lookup"><span data-stu-id="54392-129">When SignalR is hosted in IIS, the following versions are supported.</span></span> <span data-ttu-id="54392-130">請注意，是否用戶端作業系統，例如適用於開發 （Windows 8 或 Windows 7 中），完整的 IIS 或 Cassini 版本不應，因為會有 10 個同時連線加諸，因為連線將會非常快速地達到限制暫時性，經常重新建立，而且不會處置立即時無法再使用。</span><span class="sxs-lookup"><span data-stu-id="54392-130">Note that if a client operating system is used, such as for development (Windows 8 or Windows 7), full versions of IIS or Cassini should not be used, since there will be a limit of 10 simultaneous connections imposed, which will be reached very quickly since connections are transient, frequently re-established, and are not disposed immediately upon no longer being used.</span></span> <span data-ttu-id="54392-131">用戶端作業系統上時，應該會使用 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="54392-131">IIS Express should be used on client operating systems.</span></span>

<span data-ttu-id="54392-132">也請注意，signalr 使用 WebSocket，必須使用 IIS 8 或 IIS 8 Express，伺服器必須使用 Windows 8，Windows Server 2012 或更新版本，必須在 IIS 中啟用 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="54392-132">Also note that for SignalR to use WebSocket, IIS 8 or IIS 8 Express must be used, the server must be using Windows 8, Windows Server 2012, or later, and WebSocket must be enabled in IIS.</span></span> <span data-ttu-id="54392-133">如需如何啟用 WebSocket，在 IIS 中的資訊，請參閱[IIS 8.0 WebSocket 通訊協定支援](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)。</span><span class="sxs-lookup"><span data-stu-id="54392-133">For information on how to enable WebSocket in IIS, see [IIS 8.0 WebSocket Protocol Support](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

- <span data-ttu-id="54392-134">IIS 8 的 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="54392-134">IIS 8 or IIS 8 Express.</span></span>
- <span data-ttu-id="54392-135">IIS 7 和 7.5。</span><span class="sxs-lookup"><span data-stu-id="54392-135">IIS 7 and 7.5.</span></span> <span data-ttu-id="54392-136">支援[無副檔名 Url](https://support.microsoft.com/kb/980368)需要。</span><span class="sxs-lookup"><span data-stu-id="54392-136">Support for [extensionless URLs](https://support.microsoft.com/kb/980368) is required.</span></span>
- <span data-ttu-id="54392-137">IIS 必須執行整合式模式，不支援傳統模式。</span><span class="sxs-lookup"><span data-stu-id="54392-137">IIS must be running in integrated mode; classic mode is not supported.</span></span> <span data-ttu-id="54392-138">如果使用 Server-Sent 事件傳輸以傳統模式執行 IIS，可能會遇到最多 30 秒的訊息延遲。</span><span class="sxs-lookup"><span data-stu-id="54392-138">Message delays of up to 30 seconds may be experienced if IIS is run in classic mode using the Server-Sent Events transport.</span></span>
- <span data-ttu-id="54392-139">裝載的應用程式必須在完全信任模式下執行。</span><span class="sxs-lookup"><span data-stu-id="54392-139">The hosting application must be running in full trust mode.</span></span>

## <a name="client-system-requirements"></a><span data-ttu-id="54392-140">用戶端系統需求</span><span class="sxs-lookup"><span data-stu-id="54392-140">Client system requirements</span></span>

<span data-ttu-id="54392-141">SignalR 可以用於各種不同的用戶端平台。</span><span class="sxs-lookup"><span data-stu-id="54392-141">SignalR can be used in a variety of client platforms.</span></span> <span data-ttu-id="54392-142">本章節描述使用 SignalR 的網頁瀏覽器、 Windows 桌面應用程式、 Silverlight 應用程式和行動裝置的系統需求。</span><span class="sxs-lookup"><span data-stu-id="54392-142">This section describes the system requirements for using SignalR in web browsers, Windows desktop applications, Silverlight applications, and mobile devices.</span></span>

### <a name="web-browsers"></a><span data-ttu-id="54392-143">網頁瀏覽器</span><span class="sxs-lookup"><span data-stu-id="54392-143">Web browsers</span></span>

<span data-ttu-id="54392-144">SignalR 可以用各種不同的網頁瀏覽器，但一般而言，只有最新的兩個版本支援。</span><span class="sxs-lookup"><span data-stu-id="54392-144">SignalR can be used in a variety of web browsers, but typically, only the latest two versions are supported.</span></span>

<span data-ttu-id="54392-145">在瀏覽器中使用 SignalR 的應用程式必須使用 jQuery 版本 1.6.4 或重大更新的版本 （例如 1.7.2、 1.8.2 或 1.9.1）。</span><span class="sxs-lookup"><span data-stu-id="54392-145">Applications that use SignalR in browsers must use jQuery version 1.6.4 or major later versions (such as 1.7.2, 1.8.2, or 1.9.1).</span></span>

<span data-ttu-id="54392-146">SignalR 可以用於下列瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="54392-146">SignalR can be used in the following browsers:</span></span>

- <span data-ttu-id="54392-147">Microsoft Internet Explorer 版本 8、 9、 10 和 11。</span><span class="sxs-lookup"><span data-stu-id="54392-147">Microsoft Internet Explorer versions 8, 9, 10, and 11.</span></span> <span data-ttu-id="54392-148">現代化、 桌面和行動裝置版支援。</span><span class="sxs-lookup"><span data-stu-id="54392-148">Modern, Desktop, and Mobile versions are supported.</span></span>
- <span data-ttu-id="54392-149">Mozilla Firefox： 目前的版本-1，Windows 和 Mac 的版本。</span><span class="sxs-lookup"><span data-stu-id="54392-149">Mozilla Firefox: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="54392-150">Google Chrome： 目前的版本-1，Windows 和 Mac 的版本。</span><span class="sxs-lookup"><span data-stu-id="54392-150">Google Chrome: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="54392-151">Safari： 目前的版本-1，Mac 和 iOS 版本。</span><span class="sxs-lookup"><span data-stu-id="54392-151">Safari: current version - 1, both Mac and iOS versions.</span></span>
- <span data-ttu-id="54392-152">Opera： 目前的版本-1，只有 Windows。</span><span class="sxs-lookup"><span data-stu-id="54392-152">Opera: current version - 1, Windows only.</span></span>
- <span data-ttu-id="54392-153">Android 的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="54392-153">Android browser</span></span>

<span data-ttu-id="54392-154">除了需要某些瀏覽器，SignalR 使用的各種傳輸會有自己的需求。</span><span class="sxs-lookup"><span data-stu-id="54392-154">In addition to requiring certain browsers, the various transports that SignalR uses have requirements of their own.</span></span> <span data-ttu-id="54392-155">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="54392-155">The following transports are supported under the following configurations:</span></span>

<a id="browser"></a>

<span data-ttu-id="54392-156">**網頁瀏覽器傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="54392-156">**Web Browser Transport Requirements**</span></span>

| <span data-ttu-id="54392-157">Transport</span><span class="sxs-lookup"><span data-stu-id="54392-157">Transport</span></span> | <span data-ttu-id="54392-158">Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="54392-158">Internet Explorer</span></span> | <span data-ttu-id="54392-159">Chrome （Windows 或 iOS）</span><span class="sxs-lookup"><span data-stu-id="54392-159">Chrome (Windows or iOS)</span></span> | <span data-ttu-id="54392-160">Firefox</span><span class="sxs-lookup"><span data-stu-id="54392-160">Firefox</span></span> | <span data-ttu-id="54392-161">Safari （OSX 或 iOS）</span><span class="sxs-lookup"><span data-stu-id="54392-161">Safari (OSX or iOS)</span></span> | <span data-ttu-id="54392-162">Android</span><span class="sxs-lookup"><span data-stu-id="54392-162">Android</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="54392-163">WebSockets</span><span class="sxs-lookup"><span data-stu-id="54392-163">WebSockets</span></span> | <span data-ttu-id="54392-164">10+</span><span class="sxs-lookup"><span data-stu-id="54392-164">10+</span></span> | <span data-ttu-id="54392-165">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-165">current - 1</span></span> | <span data-ttu-id="54392-166">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-166">current - 1</span></span> | <span data-ttu-id="54392-167">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-167">current - 1</span></span> | <span data-ttu-id="54392-168">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-168">N/A</span></span> |
| <span data-ttu-id="54392-169">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="54392-169">Server-Sent Events</span></span> | <span data-ttu-id="54392-170">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-170">N/A</span></span> | <span data-ttu-id="54392-171">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-171">current - 1</span></span> | <span data-ttu-id="54392-172">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-172">current - 1</span></span> | <span data-ttu-id="54392-173">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-173">current - 1</span></span> | <span data-ttu-id="54392-174">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-174">N/A</span></span> |
| <span data-ttu-id="54392-175">ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="54392-175">ForeverFrame</span></span> | <span data-ttu-id="54392-176">8+</span><span class="sxs-lookup"><span data-stu-id="54392-176">8+</span></span> | <span data-ttu-id="54392-177">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-177">N/A</span></span> | <span data-ttu-id="54392-178">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-178">N/A</span></span> | <span data-ttu-id="54392-179">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-179">N/A</span></span> | <span data-ttu-id="54392-180">4.1</span><span class="sxs-lookup"><span data-stu-id="54392-180">4.1</span></span> |
| <span data-ttu-id="54392-181">長輪詢</span><span class="sxs-lookup"><span data-stu-id="54392-181">Long Polling</span></span> | <span data-ttu-id="54392-182">8+</span><span class="sxs-lookup"><span data-stu-id="54392-182">8+</span></span> | <span data-ttu-id="54392-183">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-183">current - 1</span></span> | <span data-ttu-id="54392-184">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-184">current - 1</span></span> | <span data-ttu-id="54392-185">目前-1</span><span class="sxs-lookup"><span data-stu-id="54392-185">current - 1</span></span> | <span data-ttu-id="54392-186">4.1</span><span class="sxs-lookup"><span data-stu-id="54392-186">4.1</span></span> |

<span data-ttu-id="54392-187">\*：6 + 所需的完整功能。</span><span class="sxs-lookup"><span data-stu-id="54392-187">\*: 6+ required for full functionality.</span></span>

#### <a name="unsupported-browsers"></a><span data-ttu-id="54392-188">不支援的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="54392-188">Unsupported Browsers</span></span>

<span data-ttu-id="54392-189">雖然 SignalR*可能*執行而不會在較舊的瀏覽器版本的主要問題，我們不要主動測試 SignalR 中和通常不會修正可能會出現在他們的 bug。</span><span class="sxs-lookup"><span data-stu-id="54392-189">While SignalR *may* run without major issues in older browser versions, we do not actively test SignalR in them and generally do not fix bugs that may appear in them.</span></span>

### <a name="windows-desktop-and-silverlight-applications"></a><span data-ttu-id="54392-190">Windows 桌面和 Silverlight 應用程式</span><span class="sxs-lookup"><span data-stu-id="54392-190">Windows Desktop and Silverlight Applications</span></span>

<span data-ttu-id="54392-191">除了在網頁瀏覽器中執行，SignalR 可以裝載於獨立的 Windows 用戶端或 Silverlight 應用程式。</span><span class="sxs-lookup"><span data-stu-id="54392-191">In addition to running in a web browser, SignalR can be hosted in standalone Windows client or Silverlight applications.</span></span> <span data-ttu-id="54392-192">Windows 桌面和 Silverlight 的 SignalR 應用程式有下列系統需求。</span><span class="sxs-lookup"><span data-stu-id="54392-192">Windows Desktop and Silverlight SignalR applications have the following system requirements.</span></span>

- <span data-ttu-id="54392-193">在 Windows XP SP3 或更新版本支援使用.NET 4 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="54392-193">Applications using .NET 4 are supported on Windows XP SP3 or later.</span></span>
- <span data-ttu-id="54392-194">在 Windows Vista 或更新版本支援使用.NET Framework 4.5 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="54392-194">Applications using .NET Framework 4.5 are supported on Windows Vista or later.</span></span>

<span data-ttu-id="54392-195">除了作業系統和.NET framework 需求，使用 signalr 的傳輸會有自己的需求。</span><span class="sxs-lookup"><span data-stu-id="54392-195">In addition to operating system and .NET framework requirements, the transports available to SignalR have requirements of their own.</span></span> <span data-ttu-id="54392-196">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="54392-196">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="54392-197">**Windows 桌面和 Silverlight 傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="54392-197">**Windows Desktop and Silverlight Transport Requirements**</span></span>

| <span data-ttu-id="54392-198">Transport</span><span class="sxs-lookup"><span data-stu-id="54392-198">Transport</span></span> | <span data-ttu-id="54392-199">.NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="54392-199">.NET application</span></span> | <span data-ttu-id="54392-200">Silverlight</span><span class="sxs-lookup"><span data-stu-id="54392-200">Silverlight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="54392-201">Web 通訊端</span><span class="sxs-lookup"><span data-stu-id="54392-201">Web Sockets</span></span> | <span data-ttu-id="54392-202">Windows 8 及更新版本和.NET 4.5 +</span><span class="sxs-lookup"><span data-stu-id="54392-202">Windows 8+ and .NET 4.5+</span></span> | <span data-ttu-id="54392-203">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-203">N/A</span></span> |
| <span data-ttu-id="54392-204">不限次數的框架</span><span class="sxs-lookup"><span data-stu-id="54392-204">Forever Frame</span></span> | <span data-ttu-id="54392-205">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-205">N/A</span></span> | <span data-ttu-id="54392-206">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-206">N/A</span></span> |
| <span data-ttu-id="54392-207">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="54392-207">Server-Sent Events</span></span> | <span data-ttu-id="54392-208">.NET 4 +</span><span class="sxs-lookup"><span data-stu-id="54392-208">.NET 4+</span></span> | <span data-ttu-id="54392-209">5+</span><span class="sxs-lookup"><span data-stu-id="54392-209">5+</span></span> |
| <span data-ttu-id="54392-210">長輪詢</span><span class="sxs-lookup"><span data-stu-id="54392-210">Long Polling</span></span> | <span data-ttu-id="54392-211">.NET 4 +</span><span class="sxs-lookup"><span data-stu-id="54392-211">.NET 4+</span></span> | <span data-ttu-id="54392-212">5+</span><span class="sxs-lookup"><span data-stu-id="54392-212">5+</span></span> |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a><span data-ttu-id="54392-213">Windows 市集和 Windows Phone 應用程式</span><span class="sxs-lookup"><span data-stu-id="54392-213">Windows Store and Windows Phone Applications</span></span>

<span data-ttu-id="54392-214">SignalR 可以用於 Windows 市集應用程式和 Windows Phone 8 應用程式。</span><span class="sxs-lookup"><span data-stu-id="54392-214">SignalR can be used in Windows Store applications and Windows Phone 8 applications.</span></span> <span data-ttu-id="54392-215">下列傳輸所支援的下列設定：</span><span class="sxs-lookup"><span data-stu-id="54392-215">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="54392-216">**Windows 市集和 Windows Phone 傳輸需求**</span><span class="sxs-lookup"><span data-stu-id="54392-216">**Windows Store and Windows Phone Transport Requirements**</span></span>

| <span data-ttu-id="54392-217">Transport</span><span class="sxs-lookup"><span data-stu-id="54392-217">Transport</span></span> | <span data-ttu-id="54392-218">Windows 市集 /.NET</span><span class="sxs-lookup"><span data-stu-id="54392-218">Windows Store/ .NET</span></span> | <span data-ttu-id="54392-219">Windows 市集 / JavaScript</span><span class="sxs-lookup"><span data-stu-id="54392-219">Windows Store/ JavaScript</span></span> | <span data-ttu-id="54392-220">Windows Phone / IE</span><span class="sxs-lookup"><span data-stu-id="54392-220">Windows Phone/ IE</span></span> | <span data-ttu-id="54392-221">Windows Phone /.NET</span><span class="sxs-lookup"><span data-stu-id="54392-221">Windows Phone/ .NET</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="54392-222">WebSockets</span><span class="sxs-lookup"><span data-stu-id="54392-222">WebSockets</span></span> | <span data-ttu-id="54392-223">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-223">N/A</span></span> | <span data-ttu-id="54392-224">Win8 +</span><span class="sxs-lookup"><span data-stu-id="54392-224">Win8+</span></span> | <span data-ttu-id="54392-225">8+</span><span class="sxs-lookup"><span data-stu-id="54392-225">8+</span></span> | <span data-ttu-id="54392-226">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-226">N/A</span></span> |
| <span data-ttu-id="54392-227">不限次數的框架</span><span class="sxs-lookup"><span data-stu-id="54392-227">Forever Frame</span></span> | <span data-ttu-id="54392-228">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-228">N/A</span></span> | <span data-ttu-id="54392-229">Win8 +</span><span class="sxs-lookup"><span data-stu-id="54392-229">Win8+</span></span> | <span data-ttu-id="54392-230">7.5+</span><span class="sxs-lookup"><span data-stu-id="54392-230">7.5+</span></span> | <span data-ttu-id="54392-231">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-231">N/A</span></span> |
| <span data-ttu-id="54392-232">伺服器傳送事件</span><span class="sxs-lookup"><span data-stu-id="54392-232">Server-Sent Events</span></span> | <span data-ttu-id="54392-233">Win8 +</span><span class="sxs-lookup"><span data-stu-id="54392-233">Win8+</span></span> | <span data-ttu-id="54392-234">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-234">N/A</span></span> | <span data-ttu-id="54392-235">N/A</span><span class="sxs-lookup"><span data-stu-id="54392-235">N/A</span></span> | <span data-ttu-id="54392-236">8+</span><span class="sxs-lookup"><span data-stu-id="54392-236">8+</span></span> |
| <span data-ttu-id="54392-237">長輪詢</span><span class="sxs-lookup"><span data-stu-id="54392-237">Long Polling</span></span> | <span data-ttu-id="54392-238">Win8 +</span><span class="sxs-lookup"><span data-stu-id="54392-238">Win8+</span></span> | <span data-ttu-id="54392-239">Win8 +</span><span class="sxs-lookup"><span data-stu-id="54392-239">Win8+</span></span> | <span data-ttu-id="54392-240">7.5+</span><span class="sxs-lookup"><span data-stu-id="54392-240">7.5+</span></span> | <span data-ttu-id="54392-241">8+</span><span class="sxs-lookup"><span data-stu-id="54392-241">8+</span></span> |

<a id="updates"></a>

## <a name="recommended-updates"></a><span data-ttu-id="54392-242">建議的更新</span><span class="sxs-lookup"><span data-stu-id="54392-242">Recommended Updates</span></span>

<span data-ttu-id="54392-243">SignalR 伺服器建議下列更新：</span><span class="sxs-lookup"><span data-stu-id="54392-243">The following updates are recommended for SignalR servers:</span></span>

- <span data-ttu-id="54392-244">有可用的更新，針對.NET Framework 4.5[此處](https://support.microsoft.com/kb/2750149)。</span><span class="sxs-lookup"><span data-stu-id="54392-244">An update for .NET Framework 4.5 is available [here](https://support.microsoft.com/kb/2750149).</span></span>
- <span data-ttu-id="54392-245">Microsoft 會定期發行 ASP.NET Qfe。</span><span class="sxs-lookup"><span data-stu-id="54392-245">Microsoft will periodically release QFEs for ASP.NET.</span></span> <span data-ttu-id="54392-246">這些應該套用為可用。</span><span class="sxs-lookup"><span data-stu-id="54392-246">These should be applied as available.</span></span>
