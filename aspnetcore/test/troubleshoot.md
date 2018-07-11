---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938390"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="17b76-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="17b76-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="17b76-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="17b76-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="17b76-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="17b76-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="17b76-106">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="17b76-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="17b76-107">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="17b76-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="17b76-108">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="17b76-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="17b76-109">NDC 會議 （倫敦 2018年）： 在 ASP.NET Core 應用程式診斷問題</span><span class="sxs-lookup"><span data-stu-id="17b76-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="17b76-110">ASP.NET 部落格： ASP.NET Core 效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="17b76-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="17b76-111">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="17b76-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="17b76-112">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="17b76-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="17b76-113">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="17b76-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="17b76-114">會安裝 32 和 64 位元版本的.NET Core sdk。</span><span class="sxs-lookup"><span data-stu-id="17b76-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="17b76-115">只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="17b76-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="17b76-117">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="17b76-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="17b76-118">這兩個版本可能會安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="17b76-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="17b76-119">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="17b76-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="17b76-120">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="17b76-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="17b76-121">版本錯誤下載並安裝。</span><span class="sxs-lookup"><span data-stu-id="17b76-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="17b76-122">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="17b76-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="17b76-123">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="17b76-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="17b76-124">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="17b76-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="17b76-125">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="17b76-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="17b76-126">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="17b76-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="17b76-127">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="17b76-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="17b76-128">只有在已安裝的 SDK 範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="17b76-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="17b76-130">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。</span><span class="sxs-lookup"><span data-stu-id="17b76-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="17b76-131">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="17b76-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="17b76-132">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="17b76-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="17b76-133">從解除安裝**控制台中** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="17b76-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="17b76-134">如果您了解警告的發生原因和其隱含意義，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="17b76-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="17b76-135">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="17b76-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="17b76-136">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="17b76-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="17b76-137">偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'。</span><span class="sxs-lookup"><span data-stu-id="17b76-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![顯示警告訊息 OneASP.NET 對話方塊的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="17b76-139">時，會出現這個警告的環境變數`PATH`未指向任何.NET Core Sdk，電腦上。</span><span class="sxs-lookup"><span data-stu-id="17b76-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="17b76-140">若要解決此問題：</span><span class="sxs-lookup"><span data-stu-id="17b76-140">To resolve this problem:</span></span>

* <span data-ttu-id="17b76-141">安裝或檢查已安裝.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="17b76-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="17b76-142">確認`PATH`環境變數指向安裝 SDK 的位置。</span><span class="sxs-lookup"><span data-stu-id="17b76-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="17b76-143">安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="17b76-143">The installer normally sets the `PATH`.</span></span>
