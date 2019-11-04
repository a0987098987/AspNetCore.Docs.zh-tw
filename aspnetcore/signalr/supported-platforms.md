---
title: ASP.NET Core SignalR 支援的平臺
author: bradygaster
description: 瞭解 ASP.NET Core SignalR 的支援平臺。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/01/2019
uid: signalr/supported-platforms
ms.openlocfilehash: 1be7a307710e6e522c0088fd1ca01da11a13eda1
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426977"
---
# <a name="aspnet-core-signalr-supported-platforms"></a><span data-ttu-id="8950c-103">ASP.NET Core SignalR 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="8950c-103">ASP.NET Core SignalR supported platforms</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="8950c-104">伺服器系統需求</span><span class="sxs-lookup"><span data-stu-id="8950c-104">Server system requirements</span></span>

<span data-ttu-id="8950c-105">SignalR for ASP.NET Core 支援 ASP.NET Core 支援的任何伺服器平臺。</span><span class="sxs-lookup"><span data-stu-id="8950c-105">SignalR for ASP.NET Core supports any server platform that ASP.NET Core supports.</span></span>

## <a name="javascript-client"></a><span data-ttu-id="8950c-106">JavaScript 用戶端</span><span class="sxs-lookup"><span data-stu-id="8950c-106">JavaScript client</span></span>

<span data-ttu-id="8950c-107">[JavaScript 用戶端](https://www.npmjs.com/package/@aspnet/signalr)會在 NodeJS 8 和更新版本以及下列瀏覽器上執行：</span><span class="sxs-lookup"><span data-stu-id="8950c-107">The [JavaScript client](https://www.npmjs.com/package/@aspnet/signalr) runs on NodeJS 8 and later versions and the following browsers:</span></span>

| <span data-ttu-id="8950c-108">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="8950c-108">Browser</span></span>                         | <span data-ttu-id="8950c-109">版本</span><span class="sxs-lookup"><span data-stu-id="8950c-109">Version</span></span>         |
| ------------------------------- | --------------- |
| <span data-ttu-id="8950c-110">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="8950c-110">Microsoft Edge</span></span>                  | <span data-ttu-id="8950c-111">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="8950c-111">Current&dagger;</span></span> |
| <span data-ttu-id="8950c-112">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="8950c-112">Mozilla Firefox</span></span>                 | <span data-ttu-id="8950c-113">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="8950c-113">Current&dagger;</span></span> |
| <span data-ttu-id="8950c-114">Google Chrome;包含 Android</span><span class="sxs-lookup"><span data-stu-id="8950c-114">Google Chrome; includes Android</span></span> | <span data-ttu-id="8950c-115">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="8950c-115">Current&dagger;</span></span> |
| <span data-ttu-id="8950c-116">免費包含 iOS</span><span class="sxs-lookup"><span data-stu-id="8950c-116">Safari; includes iOS</span></span>            | <span data-ttu-id="8950c-117">目前的&dagger;</span><span class="sxs-lookup"><span data-stu-id="8950c-117">Current&dagger;</span></span> |
| <span data-ttu-id="8950c-118">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="8950c-118">Microsoft Internet Explorer</span></span>     | <span data-ttu-id="8950c-119">11</span><span class="sxs-lookup"><span data-stu-id="8950c-119">11</span></span>              |

<span data-ttu-id="8950c-120">&dagger;*目前*指的是最新版的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="8950c-120">&dagger;*Current* refers to the latest version of the browser.</span></span>

## <a name="net-client"></a><span data-ttu-id="8950c-121">.NET 用戶端</span><span class="sxs-lookup"><span data-stu-id="8950c-121">.NET client</span></span>

<span data-ttu-id="8950c-122">[.Net 用戶端](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)會在 ASP.NET Core 支援的任何平臺上執行。</span><span class="sxs-lookup"><span data-stu-id="8950c-122">The [.NET client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/) runs on any platform supported by ASP.NET Core.</span></span> <span data-ttu-id="8950c-123">例如， [xamarin 開發人員可以使用 SignalR](https://github.com/aspnet/Announcements/issues/305) ，利用 xamarin 8.4.0.1 和更新版本，以及使用11.14.0.4 和更新版本的 ios 應用程式來建立 android 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8950c-123">For example, [Xamarin developers can use SignalR](https://github.com/aspnet/Announcements/issues/305) for building Android apps using Xamarin.Android 8.4.0.1 and later and iOS apps using Xamarin.iOS 11.14.0.4 and later.</span></span>

<span data-ttu-id="8950c-124">如果伺服器執行 IIS，Websocket 傳輸需要 Windows Server 2012 或更新版本上的 IIS 8.0 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8950c-124">If the server runs IIS, the WebSockets transport requires IIS 8.0 or later on Windows Server 2012 or later.</span></span> <span data-ttu-id="8950c-125">所有平臺都支援其他傳輸。</span><span class="sxs-lookup"><span data-stu-id="8950c-125">Other transports are supported on all platforms.</span></span>

## <a name="java-client"></a><span data-ttu-id="8950c-126">Java 用戶端</span><span class="sxs-lookup"><span data-stu-id="8950c-126">Java client</span></span>

<span data-ttu-id="8950c-127">[JAVA 用戶端](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)支援 java 8 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="8950c-127">The [Java client](https://search.maven.org/artifact/com.microsoft.aspnet/signalr) supports Java 8 and later versions.</span></span>

## <a name="unsupported-clients"></a><span data-ttu-id="8950c-128">不支援的用戶端</span><span class="sxs-lookup"><span data-stu-id="8950c-128">Unsupported clients</span></span>

<span data-ttu-id="8950c-129">下列用戶端可以使用，但為實驗性或非官方。</span><span class="sxs-lookup"><span data-stu-id="8950c-129">The following clients are available but are experimental or unofficial.</span></span> <span data-ttu-id="8950c-130">它們目前不受支援，而且可能永遠不會。</span><span class="sxs-lookup"><span data-stu-id="8950c-130">They aren't currently supported and may never be.</span></span>

* [<span data-ttu-id="8950c-131">C++台</span><span class="sxs-lookup"><span data-stu-id="8950c-131">C++ client</span></span>](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [<span data-ttu-id="8950c-132">Swift 用戶端</span><span class="sxs-lookup"><span data-stu-id="8950c-132">Swift client</span></span>](https://github.com/moozzyk/SignalR-Client-Swift)
