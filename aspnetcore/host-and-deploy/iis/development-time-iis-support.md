---
title: "Visual Studio for ASP.NET Core 中的開發階段 IIS 支援"
author: shirhatti
description: "探索 Windows 伺服器上執行 IIS 後方時，偵錯 ASP.NET Core 應用程式的支援。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="8749b-103">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="8749b-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="8749b-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="8749b-104">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="8749b-105">本文說明[Visual Studio](https://www.visualstudio.com/vs/)支援 Windows Server 上執行後 IIS 的 ASP.NET Core 應用程式偵錯。</span><span class="sxs-lookup"><span data-stu-id="8749b-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="8749b-106">本主題引導啟用此功能，以及設定專案。</span><span class="sxs-lookup"><span data-stu-id="8749b-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8749b-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="8749b-107">Prerequisites</span></span>

* <span data-ttu-id="8749b-108">Visual Studio (2017/版本 15.3 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="8749b-108">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="8749b-109">ASP.NET 與網頁程式開發工作負載「或」.NET Core 跨平台開發工作負載</span><span class="sxs-lookup"><span data-stu-id="8749b-109">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="8749b-110">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="8749b-110">Enable IIS</span></span>

<span data-ttu-id="8749b-111">啟用 IIS。</span><span class="sxs-lookup"><span data-stu-id="8749b-111">Enable IIS.</span></span> <span data-ttu-id="8749b-112">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="8749b-112">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="8749b-113">選取 [Internet Information Services] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="8749b-113">Select the **Internet Information Services** checkbox.</span></span>

![Windows 功能會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="8749b-115">如果 IIS 安裝需要重新啟動，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="8749b-115">If the IIS installation requires a restart, restart the system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="8749b-116">啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="8749b-116">Enable development-time IIS support</span></span>

<span data-ttu-id="8749b-117">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="8749b-117">Launch the Visual Studio installer.</span></span> <span data-ttu-id="8749b-118">選取**支援 IIS 的開發時間**元件。</span><span class="sxs-lookup"><span data-stu-id="8749b-118">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="8749b-119">元件會列在為選擇性**摘要** 面板的**ASP.NET 及 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="8749b-119">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="8749b-120">這會安裝[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="8749b-120">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps.</span></span>

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="8749b-124">設定專案</span><span class="sxs-lookup"><span data-stu-id="8749b-124">Configure the project</span></span>

<span data-ttu-id="8749b-125">建立新的啟動設定檔，以新增開發階段 IIS 支援。</span><span class="sxs-lookup"><span data-stu-id="8749b-125">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="8749b-126">在 Visual Studio 的**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="8749b-126">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="8749b-127">選取 [偵錯] 索引標籤。在 [啟動] 下拉式清單中選取 [IIS]。</span><span class="sxs-lookup"><span data-stu-id="8749b-127">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="8749b-128">請確認使用正確的 URL 啟用 [啟動瀏覽器] 功能。</span><span class="sxs-lookup"><span data-stu-id="8749b-128">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="8749b-133">或者，手動新增至啟動設定檔[launchSettings.json](http://json.schemastore.org/launchsettings)應用程式中的檔案：</span><span class="sxs-lookup"><span data-stu-id="8749b-133">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="8749b-134">如果未以系統管理員身分執行 visual Studio 可能會提示重新啟動。</span><span class="sxs-lookup"><span data-stu-id="8749b-134">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="8749b-135">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="8749b-135">If prompted, restart Visual Studio.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8749b-136">其他資源</span><span class="sxs-lookup"><span data-stu-id="8749b-136">Additional resources</span></span>

* [<span data-ttu-id="8749b-137">使用 IIS 在 Windows 上裝載 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8749b-137">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="8749b-138">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="8749b-138">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="8749b-139">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="8749b-139">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
