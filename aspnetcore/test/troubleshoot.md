---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090107"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="7c776-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="7c776-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="7c776-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c776-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c776-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="7c776-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="7c776-106">NDC 會議 （倫敦 2018年）： 在 ASP.NET Core 應用程式診斷問題</span><span class="sxs-lookup"><span data-stu-id="7c776-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="7c776-107">ASP.NET 部落格： ASP.NET Core 效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="7c776-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="7c776-108">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="7c776-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="7c776-109">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="7c776-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="7c776-110">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7c776-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7c776-111">會安裝 32 和 64 位元版本的.NET Core sdk。</span><span class="sxs-lookup"><span data-stu-id="7c776-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="7c776-112">只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="7c776-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="7c776-114">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="7c776-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="7c776-115">這兩個版本可能會安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="7c776-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="7c776-116">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="7c776-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="7c776-117">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="7c776-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="7c776-118">版本錯誤下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="7c776-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="7c776-119">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="7c776-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7c776-120">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="7c776-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7c776-121">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="7c776-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="7c776-122">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="7c776-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="7c776-123">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7c776-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7c776-124">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="7c776-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="7c776-125">只有在已安裝的 SDK 範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="7c776-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="7c776-127">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。</span><span class="sxs-lookup"><span data-stu-id="7c776-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="7c776-128">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="7c776-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="7c776-129">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="7c776-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7c776-130">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="7c776-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7c776-131">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="7c776-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="7c776-132">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="7c776-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="7c776-133">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7c776-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7c776-134">偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'。</span><span class="sxs-lookup"><span data-stu-id="7c776-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="7c776-136">時，會出現這個警告的環境變數`PATH`未指向任何.NET Core Sdk，電腦上。</span><span class="sxs-lookup"><span data-stu-id="7c776-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="7c776-137">若要解決此問題：</span><span class="sxs-lookup"><span data-stu-id="7c776-137">To resolve this problem:</span></span>

* <span data-ttu-id="7c776-138">安裝或檢查已安裝.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="7c776-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="7c776-139">確認`PATH`環境變數指向安裝 SDK 的位置。</span><span class="sxs-lookup"><span data-stu-id="7c776-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="7c776-140">安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="7c776-140">The installer normally sets the `PATH`.</span></span>
