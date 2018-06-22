---
title: 疑難排解 ASP.NET Core 專案
author: Rick-Anderson
description: 了解 ASP.NET Core 專案的相關警告和錯誤，並為其進行疑難排解。
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274589"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="7d10e-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="7d10e-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="7d10e-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7d10e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7d10e-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="7d10e-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="7d10e-106">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7d10e-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="7d10e-107">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="7d10e-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="7d10e-108">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="7d10e-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="7d10e-109">NDC 會議 （倫敦 2018年）： 在 ASP.NET Core 應用程式診斷問題</span><span class="sxs-lookup"><span data-stu-id="7d10e-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="7d10e-110">ASP.NET 部落格： ASP.NET Core 效能問題的疑難排解</span><span class="sxs-lookup"><span data-stu-id="7d10e-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="7d10e-111">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="7d10e-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="7d10e-112">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="7d10e-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="7d10e-113">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7d10e-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7d10e-114">會安裝 32 和 64 位元版本的.NET Core sdk。</span><span class="sxs-lookup"><span data-stu-id="7d10e-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="7d10e-115">只有從安裝在 64 位元版本的範本 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="7d10e-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="7d10e-117">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="7d10e-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="7d10e-118">這兩個版本可能會安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="7d10e-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="7d10e-119">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，但然後複製其整個並安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="7d10e-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="7d10e-120">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d10e-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="7d10e-121">錯誤的版本已下載及安裝。</span><span class="sxs-lookup"><span data-stu-id="7d10e-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="7d10e-122">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="7d10e-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7d10e-123">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="7d10e-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7d10e-124">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="7d10e-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="7d10e-125">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="7d10e-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="7d10e-126">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7d10e-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7d10e-127">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="7d10e-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="7d10e-128">只有安裝在同時從樣板 'c:\\Program Files\\dotnet\\sdk\\' 將會顯示。</span><span class="sxs-lookup"><span data-stu-id="7d10e-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="7d10e-130">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息*c:\\Program Files\\dotnet\\sdk\\*。</span><span class="sxs-lookup"><span data-stu-id="7d10e-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="7d10e-131">通常這會使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="7d10e-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="7d10e-132">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="7d10e-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="7d10e-133">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="7d10e-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="7d10e-134">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="7d10e-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="7d10e-135">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="7d10e-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="7d10e-136">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="7d10e-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="7d10e-137">偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'。</span><span class="sxs-lookup"><span data-stu-id="7d10e-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="7d10e-139">時，會出現這個警告的環境變數`PATH`未指向任何.NET Core Sdk，電腦上。</span><span class="sxs-lookup"><span data-stu-id="7d10e-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="7d10e-140">若要解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="7d10e-140">To resolve this problem:</span></span>

* <span data-ttu-id="7d10e-141">安裝或檢查已安裝.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="7d10e-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="7d10e-142">確認`PATH`環境變數指向安裝 SDK 的位置。</span><span class="sxs-lookup"><span data-stu-id="7d10e-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="7d10e-143">安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="7d10e-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="7d10e-144">使用 IHtmlHelper.Partial 可能導致應用程式死結</span><span class="sxs-lookup"><span data-stu-id="7d10e-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="7d10e-145">在 ASP.NET 核心 2.1 和更新版本，呼叫`Html.Partial`導致分析器警告，因為可能有死結。</span><span class="sxs-lookup"><span data-stu-id="7d10e-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="7d10e-146">警告訊息是：</span><span class="sxs-lookup"><span data-stu-id="7d10e-146">The warning message is:</span></span>

> <span data-ttu-id="7d10e-147">使用 IHtmlHelper.Partial 可能會導致應用程式死結。</span><span class="sxs-lookup"><span data-stu-id="7d10e-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="7d10e-148">請考慮使用`<partial>`標記協助程式或`IHtmlHelper.PartialAsync`。</span><span class="sxs-lookup"><span data-stu-id="7d10e-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="7d10e-149">呼叫`@Html.Partial`應取代為`@await Html.PartialAsync`或部分標記協助程式`<partial name="_Partial" />`。</span><span class="sxs-lookup"><span data-stu-id="7d10e-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
