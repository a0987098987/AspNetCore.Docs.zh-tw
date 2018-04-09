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
uid: testing/troubleshoot
ms.openlocfilehash: 98077081409949db14b19c7934bc162990ffc302
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="3b1cc-103">疑難排解 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="3b1cc-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="3b1cc-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3b1cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3b1cc-105">下列連結提供疑難排解指導方針：</span><span class="sxs-lookup"><span data-stu-id="3b1cc-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="3b1cc-106">針對 Azure App Service 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="3b1cc-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="3b1cc-107">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="3b1cc-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="3b1cc-108">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="3b1cc-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="3b1cc-109">.NET core SDK 警告</span><span class="sxs-lookup"><span data-stu-id="3b1cc-109">.NET Core SDK warnings</span></span>

### <a name="both-the-32-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="3b1cc-110">已安裝的 32 和 64 位元版本的.NET Core sdk</span><span class="sxs-lookup"><span data-stu-id="3b1cc-110">Both the 32 and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="3b1cc-111">在**新專案**對話方塊適用於 ASP.NET Core，您可能會看到頂端會出現下列警告：</span><span class="sxs-lookup"><span data-stu-id="3b1cc-111">In the **New Project** dialog for ASP.NET Core, you may see the following warning appear at the top:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="3b1cc-113">時，會出現這個警告 (x86) 32 位元和 64 位元 (x64) 版本的[.NET Core SDK](https://www.microsoft.com/net/download/all)安裝。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-113">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="3b1cc-114">這兩個版本可安裝的常見原因包括：</span><span class="sxs-lookup"><span data-stu-id="3b1cc-114">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="3b1cc-115">您原本下載.NET Core SDK 安裝程式使用 32 位元電腦，然後複製其整個但安裝在 64 位元電腦上。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-115">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="3b1cc-116">32 位元.NET Core SDK 已安裝另一個應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-116">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="3b1cc-117">錯誤的版本已下載及安裝。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-117">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="3b1cc-118">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-118">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="3b1cc-119">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-119">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3b1cc-120">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-120">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="3b1cc-121">.NET Core SDK 安裝在多個位置</span><span class="sxs-lookup"><span data-stu-id="3b1cc-121">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="3b1cc-122">在**新專案**ASP.NET Core 頂端會出現下列警告您可能會看到的對話方塊：</span><span class="sxs-lookup"><span data-stu-id="3b1cc-122">In the **New Project** dialog for ASP.NET Core you may see the following warning appear at the top:</span></span> 

 <span data-ttu-id="3b1cc-123">.NET Core SDK 會安裝在多個位置。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-123">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="3b1cc-124">只安裝在同時從範本 ' C:\Program Files\dotnet\sdk\'隨即出現。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-124">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![OneASP.NET 對話方塊顯示警告訊息的螢幕擷取畫面](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="3b1cc-126">您會看到此訊息，因為您以外的目錄中有至少一個安裝的.NET Core sdk * C:\Program Files\dotnet\sdk\*。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-126">You are seeing this message because you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="3b1cc-127">通常發生這種情況時已使用複製/貼上，而不 MSI 安裝程式在機器上部署.NET Core SDK。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-127">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="3b1cc-128">解除安裝 32 位元.NET Core SDK，以避免這個警告。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-128">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="3b1cc-129">從解除安裝**控制台** > **程式和功能** > **解除安裝或變更程式**。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-129">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="3b1cc-130">如果您了解為什麼會發生警告和它的含意，您可以忽略此警告。</span><span class="sxs-lookup"><span data-stu-id="3b1cc-130">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>
