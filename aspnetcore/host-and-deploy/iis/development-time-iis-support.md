---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: shirhatti
description: 探索 Windows 伺服器上執行 IIS 後方時，偵錯 ASP.NET Core 應用程式的支援。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 218bb2653b92cd7b1cf2c6726b2d4bedbf307a62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="e2e84-103">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="e2e84-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="e2e84-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="e2e84-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="e2e84-105">本文說明[Visual Studio](https://www.visualstudio.com/vs/)支援 Windows Server 上執行後 IIS 的 ASP.NET Core 應用程式偵錯。</span><span class="sxs-lookup"><span data-stu-id="e2e84-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="e2e84-106">本主題引導啟用此功能，以及設定專案。</span><span class="sxs-lookup"><span data-stu-id="e2e84-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2e84-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="e2e84-107">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="e2e84-108">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="e2e84-108">Enable IIS</span></span>

<span data-ttu-id="e2e84-109">啟用 IIS。</span><span class="sxs-lookup"><span data-stu-id="e2e84-109">Enable IIS.</span></span> <span data-ttu-id="e2e84-110">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="e2e84-110">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="e2e84-111">選取 [Internet Information Services] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="e2e84-111">Select the **Internet Information Services** checkbox.</span></span>

![Windows 功能會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="e2e84-113">若 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="e2e84-113">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="e2e84-114">啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="e2e84-114">Enable development-time IIS support</span></span>

<span data-ttu-id="e2e84-115">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e2e84-115">Launch the Visual Studio installer.</span></span> <span data-ttu-id="e2e84-116">選取**支援 IIS 的開發時間**元件。</span><span class="sxs-lookup"><span data-stu-id="e2e84-116">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="e2e84-117">元件會列在為選擇性**摘要** 面板的**ASP.NET 及 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="e2e84-117">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="e2e84-118">這會安裝[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="e2e84-118">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="e2e84-122">設定專案</span><span class="sxs-lookup"><span data-stu-id="e2e84-122">Configure the project</span></span>

<span data-ttu-id="e2e84-123">建立新的啟動設定檔，以新增開發階段 IIS 支援。</span><span class="sxs-lookup"><span data-stu-id="e2e84-123">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="e2e84-124">在 Visual Studio 的**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="e2e84-124">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="e2e84-125">選取 [偵錯] 索引標籤。在 [啟動] 下拉式清單中選取 [IIS]。</span><span class="sxs-lookup"><span data-stu-id="e2e84-125">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="e2e84-126">請確認使用正確的 URL 啟用 [啟動瀏覽器] 功能。</span><span class="sxs-lookup"><span data-stu-id="e2e84-126">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="e2e84-131">或者，手動新增至啟動設定檔[launchSettings.json](http://json.schemastore.org/launchsettings)應用程式中的檔案：</span><span class="sxs-lookup"><span data-stu-id="e2e84-131">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

<span data-ttu-id="e2e84-132">如果未以系統管理員身分執行 visual Studio 可能會提示重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e2e84-132">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="e2e84-133">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e2e84-133">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2e84-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="e2e84-134">Additional resources</span></span>

* [<span data-ttu-id="e2e84-135">使用 IIS 在 Windows 上裝載 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e2e84-135">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e2e84-136">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="e2e84-136">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e2e84-137">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="e2e84-137">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
