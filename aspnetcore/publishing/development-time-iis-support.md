---
title: "Visual Studio for ASP.NET Core 中的開發階段 IIS 支援"
author: shirhatti
description: "探索在 Windows Server 上的 IIS 背後執行時，ASP.NET Core 應用程式的偵錯支援。"
keywords: "ASP.NET Core, Internet Information Services, IIS, Windows Server, ASP.NET Core Module, 偵錯"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: f531d90646b9d261c5fbbffcecd6ded9185ae292
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/15/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="05415-104">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="05415-104">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="05415-105">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="05415-105">By: [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="05415-106">本文說明在 Windows Server 上的 IIS 背後執行時，ASP.NET Core 應用程式的 [Visual Studio](https://www.visualstudio.com/vs/) 偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="05415-106">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core applications running behind IIS on Windows Server.</span></span> <span data-ttu-id="05415-107">本主題會逐步引導您啟用這項功能，以及設定專案。</span><span class="sxs-lookup"><span data-stu-id="05415-107">This topic walks you through enabling this feature and setting up your project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05415-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="05415-108">Prerequisites</span></span>

* <span data-ttu-id="05415-109">Visual Studio (2017/版本 15.3 或更新版本)</span><span class="sxs-lookup"><span data-stu-id="05415-109">Visual Studio (2017/version 15.3 or later)</span></span>
* <span data-ttu-id="05415-110">ASP.NET 與網頁程式開發工作負載「或」.NET Core 跨平台開發工作負載</span><span class="sxs-lookup"><span data-stu-id="05415-110">ASP.NET and web development workload *OR* the .NET Core cross-platform development workload</span></span>

## <a name="enable-iis"></a><span data-ttu-id="05415-111">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="05415-111">Enable IIS</span></span>

<span data-ttu-id="05415-112">在系統上啟用 IIS。</span><span class="sxs-lookup"><span data-stu-id="05415-112">Enable IIS on your system.</span></span> <span data-ttu-id="05415-113">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="05415-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="05415-114">選取 [Internet Information Services] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="05415-114">Select the **Internet Information Services** checkbox.</span></span>

![Windows 功能會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="05415-116">若您的 IIS 安裝需要重新開機，請重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="05415-116">If your IIS installation requires a reboot, reboot your system.</span></span>

## <a name="enable-development-time-iis-support"></a><span data-ttu-id="05415-117">啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="05415-117">Enable development-time IIS support</span></span>

<span data-ttu-id="05415-118">安裝 IIS 後，請啟動 Visual Studio 安裝程式來修改現有的 Visual Studio 安裝。</span><span class="sxs-lookup"><span data-stu-id="05415-118">Once you've installed IIS, launch the Visual Studio installer to modify your existing Visual Studio installation.</span></span> <span data-ttu-id="05415-119">在安裝程式中，選取 [開發階段 IIS 支援] 元件。</span><span class="sxs-lookup"><span data-stu-id="05415-119">In the installer, select the **Development time IIS support** component.</span></span> <span data-ttu-id="05415-120">該元件會在 [ASP.NET 與網頁程式開發] 工作負載的 [摘要] 面板中，列為選擇性元件。</span><span class="sxs-lookup"><span data-stu-id="05415-120">The component is listed as an optional component in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="05415-121">這會安裝 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 應用程式的必要原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="05415-121">This installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core applications.</span></span>

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="05415-125">設定專案</span><span class="sxs-lookup"><span data-stu-id="05415-125">Configure the project</span></span>

<span data-ttu-id="05415-126">建立新的啟動設定檔，以新增開發階段 IIS 支援。</span><span class="sxs-lookup"><span data-stu-id="05415-126">Create a new launch profile to add development-time IIS support.</span></span> <span data-ttu-id="05415-127">在 Visual Studio 的**方案總管**中，以滑鼠右鍵按一下專案，然後選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="05415-127">In Visual Studio's **Solution Explorer**, right-click the project and select **Properties**.</span></span> <span data-ttu-id="05415-128">選取 [偵錯] 索引標籤。在 [啟動] 下拉式清單中選取 [IIS]。</span><span class="sxs-lookup"><span data-stu-id="05415-128">Select the **Debug** tab. Select **IIS** from the **Launch** dropdown.</span></span> <span data-ttu-id="05415-129">請確認使用正確的 URL 啟用 [啟動瀏覽器] 功能。</span><span class="sxs-lookup"><span data-stu-id="05415-129">Confirm that the **Launch browser** feature is enabled with the correct URL.</span></span>

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="05415-134">或者，您也可以手動將啟動設定檔新增至應用程式中的 [launchSettings.json](http://json.schemastore.org/launchsettings) 檔案：</span><span class="sxs-lookup"><span data-stu-id="05415-134">Alternatively, you can manually add a launch profile to your [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

<span data-ttu-id="05415-135">若您不是以系統管理員的身分執行，系統可能會提示您重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="05415-135">You may be prompted to restart Visual Studio if you weren't running as an administrator.</span></span> <span data-ttu-id="05415-136">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="05415-136">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="05415-137">恭喜您！</span><span class="sxs-lookup"><span data-stu-id="05415-137">Congratulations!</span></span> <span data-ttu-id="05415-138">您已成功為專案設定開發階段 IIS 支援。</span><span class="sxs-lookup"><span data-stu-id="05415-138">At this point, your project is configured for development-time IIS support.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="05415-139">其他資源</span><span class="sxs-lookup"><span data-stu-id="05415-139">Additional resources</span></span>

* [<span data-ttu-id="05415-140">使用 IIS 在 Windows 上裝載 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="05415-140">Host ASP.NET Core on Windows with IIS</span></span>](xref:publishing/iis)
* [<span data-ttu-id="05415-141">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="05415-141">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="05415-142">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="05415-142">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
