---
title: 疑難排解適用於 ASP.NET Core
author: Rick-Anderson
description: 了解並疑難排解警告和錯誤與 ASP.NET Core 專案。
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 64a353a9bdf0753c63f676f9d07f42ba45acdcab
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "35217527"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="608bf-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="608bf-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="608bf-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="608bf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="608bf-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="608bf-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="608bf-106">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="608bf-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="608bf-107">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="608bf-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="608bf-108">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="608bf-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* <span data-ttu-id="608bf-109">[YouTube: Diagnosing issues in ASP.NET Core Applications](https://www.youtube.com/watch?v=RYI0DHoIVaA) (YouTube：診斷 ASP.NET Core 應用程式中的問題)</span><span class="sxs-lookup"><span data-stu-id="608bf-109">[YouTube: Diagnosing issues in ASP.NET Core Applications](https://www.youtube.com/watch?v=RYI0DHoIVaA)</span></span>

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="608bf-110">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="608bf-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="608bf-111">會安裝 32 位元和 64 位元版本的.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="608bf-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="608bf-112">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="608bf-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="608bf-114">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="608bf-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="608bf-115">這兩個版本可安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="608bf-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="608bf-116">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，然後複製其整個但安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="608bf-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="608bf-117">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="608bf-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="608bf-118">錯誤的版本已下載及安裝。</span><span class="sxs-lookup"><span data-stu-id="608bf-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="608bf-119">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="608bf-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="608bf-120">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="608bf-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="608bf-121">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="608bf-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="608bf-122">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="608bf-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="608bf-123">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="608bf-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="608bf-124">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="608bf-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="608bf-125">只安裝在同時從範本 ' C:\Program Files\dotnet\sdk\'隨即出現。</span><span class="sxs-lookup"><span data-stu-id="608bf-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="608bf-127">您以外的目錄中有至少一個安裝的.NET Core sdk 時，您會看到此訊息 * C:\Program Files\dotnet\sdk\*。</span><span class="sxs-lookup"><span data-stu-id="608bf-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="608bf-128">通常發生這種情況時已使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="608bf-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="608bf-129">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="608bf-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="608bf-130">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="608bf-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="608bf-131">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="608bf-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="608bf-132">偵測到.NET Core Sdk</span><span class="sxs-lookup"><span data-stu-id="608bf-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="608bf-133">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到下列警告：</span><span class="sxs-lookup"><span data-stu-id="608bf-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="608bf-134">**偵測到.NET Core Sdk，請確定它們包含在環境變數 'PATH'**</span><span class="sxs-lookup"><span data-stu-id="608bf-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="608bf-136">時，會出現這個警告的環境變數`PATH`未指向任何.NET Core Sdk，電腦上。</span><span class="sxs-lookup"><span data-stu-id="608bf-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="608bf-137">若要解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="608bf-137">To resolve this problem:</span></span>

* <span data-ttu-id="608bf-138">安裝或檢查已安裝.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="608bf-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="608bf-139">確認`PATH`環境變數指向安裝 SDK 的位置。</span><span class="sxs-lookup"><span data-stu-id="608bf-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="608bf-140">安裝程式通常設定`PATH`。</span><span class="sxs-lookup"><span data-stu-id="608bf-140">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-application-deadlocks"></a><span data-ttu-id="608bf-141">使用 IHtmlHelper.Partial 可能導致應用程式死結</span><span class="sxs-lookup"><span data-stu-id="608bf-141">Use of IHtmlHelper.Partial may result in application deadlocks</span></span>

<span data-ttu-id="608bf-142">在 ASP.NET 核心 2.1 和更新版本，呼叫`Html.Partial`導致分析器警告，因為可能有死結。</span><span class="sxs-lookup"><span data-stu-id="608bf-142">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="608bf-143">警告訊息是：</span><span class="sxs-lookup"><span data-stu-id="608bf-143">The warning message is:</span></span>

<span data-ttu-id="608bf-144">*使用 IHtmlHelper.Partial 可能會導致應用程式死結。請考慮使用`<partial>`標記協助程式或`IHtmlHelper.PartialAsync`。*</span><span class="sxs-lookup"><span data-stu-id="608bf-144">*Use of IHtmlHelper.Partial may result in application deadlocks. Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.*</span></span>

<span data-ttu-id="608bf-145">呼叫`@Html.Partial`應取代為`@await Html.PartialAsync`或部分標記協助程式`<partial name="_Partial" />`。</span><span class="sxs-lookup"><span data-stu-id="608bf-145">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
