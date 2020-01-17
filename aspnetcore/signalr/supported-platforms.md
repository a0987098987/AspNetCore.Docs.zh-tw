---
title: ASP.NET Core SignalR 支援的平臺
author: bradygaster
description: 瞭解 ASP.NET Core SignalR的支援平臺。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/16/2020
no-loc:
- SignalR
uid: signalr/supported-platforms
ms.openlocfilehash: 054965921c87c1a9be27e5ddaa8a87b0fa1f4113
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146494"
---
# <a name="aspnet-core-opno-locsignalr-supported-platforms"></a><span data-ttu-id="7300b-103">ASP.NET Core SignalR 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="7300b-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="7300b-104">伺服器系統需求</span><span class="sxs-lookup"><span data-stu-id="7300b-104">Server system requirements</span></span>

<span data-ttu-id="7300b-105">ASP.NET Core 的 SignalR 支援 ASP.NET Core 支援的任何伺服器平臺。</span><span class="sxs-lookup"><span data-stu-id="7300b-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="7300b-106">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="7300b-106">JavaScript client</span></span>

<span data-ttu-id="7300b-107">[JavaScript 用戶端](xref:signalr/javascript-client)會在 NodeJS 8 和更新版本以及下列瀏覽器上執行：</span><span class="sxs-lookup"><span data-stu-id="7300b-107">The [JavaScript client](xref:signalr/javascript-client) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="7300b-108">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="7300b-108">Browser</span></span>                         | <span data-ttu-id="7300b-109">{2&gt;版本&lt;2}</span><span class="sxs-lookup"><span data-stu-id="7300b-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="7300b-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="7300b-110">Microsoft Edge</span></span>                  | <span data-ttu-id="7300b-111">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="7300b-111">Current&dagger;</span></span> |
| <span data-ttu-id="7300b-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="7300b-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="7300b-113">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="7300b-113">Current&dagger;</span></span> |
| <span data-ttu-id="7300b-114">Google Chrome;包含 Android</span><span class="sxs-lookup"><span data-stu-id="7300b-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="7300b-115">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="7300b-115">Current&dagger;</span></span> |
| <span data-ttu-id="7300b-116">免費包含 iOS</span><span class="sxs-lookup"><span data-stu-id="7300b-116">Safari; includes iOS</span></span>            | <span data-ttu-id="7300b-117">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="7300b-117">Current&dagger;</span></span> |
| <span data-ttu-id="7300b-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="7300b-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="7300b-119">11</span><span class="sxs-lookup"><span data-stu-id="7300b-119">11</span></span>              |

<span data-ttu-id="7300b-120">&dagger;*目前*指的是最新版的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="7300b-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="7300b-121">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="7300b-121">.NET client</span></span>

<span data-ttu-id="7300b-122">[.Net 用戶端](xref:signalr/dotnet-client)會在 ASP.NET Core 支援的任何平臺上執行。</span><span class="sxs-lookup"><span data-stu-id="7300b-122">The [.NET client](xref:signalr/dotnet-client) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="7300b-123">例如， [Xamarin 開發人員可以使用 SignalR](https://github.com/aspnet/Announcements/issues/305) ，以使用8.4.0.1 和更新版本，以及使用11.14.0.4 和更新版本的 ios 應用程式來建立 android 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7300b-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="7300b-124">如果伺服器執行 IIS，Websocket 傳輸需要 Windows Server 2012 或更新版本上的 IIS 8.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7300b-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="7300b-125">所有平臺都支援其他傳輸。</span><span class="sxs-lookup"><span data-stu-id="7300b-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="7300b-126">Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="7300b-126">Java client</span></span>

<span data-ttu-id="7300b-127">[JAVA 用戶端](xref:signalr/java-client)支援 java 8 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="7300b-127">The [Java client](xref:signalr/java-client) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="7300b-128">不支援的用戶端</span><span class="sxs-lookup"><span data-stu-id="7300b-128">Unsupported clients</span></span>

<span data-ttu-id="7300b-129">下列用戶端可以使用，但為實驗性或非官方。</span><span class="sxs-lookup"><span data-stu-id="7300b-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="7300b-130">它們目前不受支援，而且可能永遠不會。</span><span class="sxs-lookup"><span data-stu-id="7300b-130">They aren't currently supported and may never be.</span></span>

* <span data-ttu-id="7300b-131">[C++台](https://github.com/aspnet/SignalR-Client-Cpp)</span><span class="sxs-lookup"><span data-stu-id="7300b-131">[C++ client](https://github.com/aspnet/SignalR-Client-Cpp)</span></span>

* <span data-ttu-id="7300b-132">[Swift 用戶端](https://github.com/moozzyk/SignalR-Client-Swift)</span><span class="sxs-lookup"><span data-stu-id="7300b-132">[Swift client](https://github.com/moozzyk/SignalR-Client-Swift)</span></span>
