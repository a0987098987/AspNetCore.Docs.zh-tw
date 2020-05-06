---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: rick-anderson
description: 了解在 Windows Server 上搭配 IIS 執行 ASP.NET Core 應用程式時，對該應用程式所提供的偵錯支援。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a6719b4f84b1bc60c7c2aea2aa3a97ef79f43e2e
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777016"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="625d3-103">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="625d3-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="625d3-104">由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供</span><span class="sxs-lookup"><span data-stu-id="625d3-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="625d3-105">本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="625d3-105">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="625d3-106">本主題會逐步解說如何啟用此案例及設定專案。</span><span class="sxs-lookup"><span data-stu-id="625d3-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="625d3-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="625d3-107">Prerequisites</span></span>

* [<span data-ttu-id="625d3-108">適用于 Windows 的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="625d3-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="625d3-109">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="625d3-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="625d3-110">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="625d3-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="625d3-111">X.509 安全性憑證 (以取得 HTTPS 支援)</span><span class="sxs-lookup"><span data-stu-id="625d3-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="625d3-112">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="625d3-112">Enable IIS</span></span>

1. <span data-ttu-id="625d3-113">在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > \*\*\*\* (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="625d3-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="625d3-114">選取 [Internet Information Services]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="625d3-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="625d3-115">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="625d3-115">Select **OK**.</span></span>

<span data-ttu-id="625d3-116">安裝 IIS 可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="625d3-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="625d3-117">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="625d3-117">Configure IIS</span></span>

<span data-ttu-id="625d3-118">IIS 的網站必須含有下列設定：</span><span class="sxs-lookup"><span data-stu-id="625d3-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="625d3-119">**主機名稱** &ndash; 通常，[預設網站]\*\*\*\* 會搭配 [主機名稱]\*\*\*\* 或 `localhost` 使用。</span><span class="sxs-lookup"><span data-stu-id="625d3-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="625d3-120">不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。</span><span class="sxs-lookup"><span data-stu-id="625d3-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="625d3-121">**網站繫結**</span><span class="sxs-lookup"><span data-stu-id="625d3-121">**Site Binding**</span></span>
  * <span data-ttu-id="625d3-122">針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="625d3-123">通常會使用 [IIS Express 開發憑證]\*\*\*\*，但可使用任何有效的憑證。</span><span class="sxs-lookup"><span data-stu-id="625d3-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="625d3-124">針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="625d3-125">需針對 HTTP 或 HTTPS 使用單一繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="625d3-126">**不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**</span><span class="sxs-lookup"><span data-stu-id="625d3-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="625d3-127">在 Visual Studio 中啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="625d3-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="625d3-128">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="625d3-129">針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="625d3-130">針對 [ASP.NET 與網頁程式開發]\*\*\*\* 工作負載，尋找並安裝 [開發時間的 IIS 支援]\*\*\*\* 元件。</span><span class="sxs-lookup"><span data-stu-id="625d3-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="625d3-131">該元件會列於工作負載右側 [安裝詳細資料]\*\*\*\* 中 [開發時間的 IIS 支援]\*\*\*\* 底下的 [選擇性]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="625d3-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="625d3-132">此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="625d3-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="625d3-133">設定專案</span><span class="sxs-lookup"><span data-stu-id="625d3-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="625d3-134">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="625d3-134">HTTPS redirection</span></span>

<span data-ttu-id="625d3-135">針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 視窗中，選取 [針對 HTTPS 進行設定]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="625d3-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="625d3-136">選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="625d3-137">針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="625d3-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="625d3-138">如需詳細資訊，請參閱<xref:security/enforcing-ssl>。</span><span class="sxs-lookup"><span data-stu-id="625d3-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="625d3-139">針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="625d3-140">您不需要進行任何應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="625d3-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="625d3-141">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="625d3-141">IIS launch profile</span></span>

<span data-ttu-id="625d3-142">建立新的啟動設定檔，以新增開發階段 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="625d3-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="625d3-143">以滑鼠右鍵按一下 [方案總管]\*\*\*\* 中的專案。</span><span class="sxs-lookup"><span data-stu-id="625d3-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="625d3-144">選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="625d3-144">Select **Properties**.</span></span> <span data-ttu-id="625d3-145">開啟 [**調試**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="625d3-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="625d3-146">針對 [設定檔]\*\*\*\*，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="625d3-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="625d3-147">在快顯示窗中，將設定檔命名為 "IIS"。</span><span class="sxs-lookup"><span data-stu-id="625d3-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="625d3-148">選取 [確定]\*\*\*\* 以建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="625d3-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="625d3-149">針對 [啟動]\*\*\*\* 設定，從清單中選取 [IIS]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="625d3-150">選取 [啟動瀏覽器]\*\*\*\* 的核取方塊並提供端點 URL。</span><span class="sxs-lookup"><span data-stu-id="625d3-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="625d3-151">當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="625d3-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="625d3-152">針對 HTTP，請使用 HTTP (`http://`) 端點。</span><span class="sxs-lookup"><span data-stu-id="625d3-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="625d3-153">提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="625d3-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="625d3-154">在 URL 結尾提供應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="625d3-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="625d3-155">例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。</span><span class="sxs-lookup"><span data-stu-id="625d3-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="625d3-156">在 [環境變數]\*\*\*\* 區段中，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="625d3-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="625d3-157">提供 [名稱]\*\*\*\* 為 `ASPNETCORE_ENVIRONMENT` 且 [值]\*\*\*\* 為 `Development` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="625d3-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="625d3-158">在 [Web 伺服器設定]\*\*\*\* 區域中，將 [應用程式 URL]\*\*\*\* 設定為用於 [啟動瀏覽器]\*\*\*\* 端點 URL 的相同值。</span><span class="sxs-lookup"><span data-stu-id="625d3-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="625d3-159">針對 Visual Studio 2019 或更新版本中的 [裝載模型]\*\*\*\*，請選取 [預設]\*\*\*\* 以使用專案所使用的裝載模型。</span><span class="sxs-lookup"><span data-stu-id="625d3-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="625d3-160">如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="625d3-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="625d3-161">如果該屬性不存在，便會使用應用程式的預設裝載模型，其為內部處理序。</span><span class="sxs-lookup"><span data-stu-id="625d3-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="625d3-162">如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]\*\*\*\* 設定為 `In Process` 或 `Out Of Process`。</span><span class="sxs-lookup"><span data-stu-id="625d3-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="625d3-163">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="625d3-163">Save the profile.</span></span>

<span data-ttu-id="625d3-164">不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。</span><span class="sxs-lookup"><span data-stu-id="625d3-164">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="625d3-165">下列範例會將專案設定為使用 HTTPS 通訊協定：</span><span class="sxs-lookup"><span data-stu-id="625d3-165">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="625d3-166">請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="625d3-166">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="625d3-167">執行專案</span><span class="sxs-lookup"><span data-stu-id="625d3-167">Run the project</span></span>

<span data-ttu-id="625d3-168">以系統管理員身分執行 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="625d3-168">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="625d3-169">確認建置設定下拉式清單已設定為 [偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-169">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="625d3-170">將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-170">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="625d3-171">如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。</span><span class="sxs-lookup"><span data-stu-id="625d3-171">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="625d3-172">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="625d3-172">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="625d3-173">如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。</span><span class="sxs-lookup"><span data-stu-id="625d3-173">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="625d3-174">使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。</span><span class="sxs-lookup"><span data-stu-id="625d3-174">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="625d3-175">例如，不會碰到中斷點。</span><span class="sxs-lookup"><span data-stu-id="625d3-175">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="625d3-176">其他資源</span><span class="sxs-lookup"><span data-stu-id="625d3-176">Additional resources</span></span>

* [<span data-ttu-id="625d3-177">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="625d3-177">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="625d3-178">本文說明針對在 Windows Server 上搭配 IIS 執行的 ASP.NET Core 應用程式所提供的 [Visual Studio](https://visualstudio.microsoft.com) 偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="625d3-178">This article describes [Visual Studio](https://visualstudio.microsoft.com) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="625d3-179">本主題會逐步解說如何啟用此案例及設定專案。</span><span class="sxs-lookup"><span data-stu-id="625d3-179">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="625d3-180">必要條件</span><span class="sxs-lookup"><span data-stu-id="625d3-180">Prerequisites</span></span>

* [<span data-ttu-id="625d3-181">適用于 Windows 的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="625d3-181">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="625d3-182">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="625d3-182">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="625d3-183">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="625d3-183">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="625d3-184">X.509 安全性憑證 (以取得 HTTPS 支援)</span><span class="sxs-lookup"><span data-stu-id="625d3-184">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="625d3-185">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="625d3-185">Enable IIS</span></span>

1. <span data-ttu-id="625d3-186">在 Windows 中，瀏覽至 [控制台]**[程式]** > **[程式和功能]** > **[開啟或關閉 Windows 功能]** > \*\*\*\* (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="625d3-186">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="625d3-187">選取 [Internet Information Services]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="625d3-187">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="625d3-188">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="625d3-188">Select **OK**.</span></span>

<span data-ttu-id="625d3-189">安裝 IIS 可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="625d3-189">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="625d3-190">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="625d3-190">Configure IIS</span></span>

<span data-ttu-id="625d3-191">IIS 的網站必須含有下列設定：</span><span class="sxs-lookup"><span data-stu-id="625d3-191">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="625d3-192">**主機名稱** &ndash; 通常，[預設網站]\*\*\*\* 會搭配 [主機名稱]\*\*\*\* 或 `localhost` 使用。</span><span class="sxs-lookup"><span data-stu-id="625d3-192">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="625d3-193">不過，任何具有唯一主機名稱的有效 IIS 網站皆適用。</span><span class="sxs-lookup"><span data-stu-id="625d3-193">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="625d3-194">**網站繫結**</span><span class="sxs-lookup"><span data-stu-id="625d3-194">**Site Binding**</span></span>
  * <span data-ttu-id="625d3-195">針對需要 HTTPS 的應用程式，請搭配憑證針對連接埠 443 建立繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-195">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="625d3-196">通常會使用 [IIS Express 開發憑證]\*\*\*\*，但可使用任何有效的憑證。</span><span class="sxs-lookup"><span data-stu-id="625d3-196">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="625d3-197">針對使用 HTTP 的應用程式，請確認已存在針對連接埠 80 的繫結，或是為新網站建立針對連接埠 80 的繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-197">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="625d3-198">需針對 HTTP 或 HTTPS 使用單一繫結。</span><span class="sxs-lookup"><span data-stu-id="625d3-198">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="625d3-199">**不支援同時針對 HTTP 和 HTTPS 連接埠的繫結。**</span><span class="sxs-lookup"><span data-stu-id="625d3-199">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="625d3-200">在 Visual Studio 中啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="625d3-200">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="625d3-201">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-201">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="625d3-202">針對您打算用於 IIS 開發階段支援的 Visual Studio 安裝，選取 [修改]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-202">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="625d3-203">針對 [ASP.NET 與網頁程式開發]\*\*\*\* 工作負載，尋找並安裝 [開發時間的 IIS 支援]\*\*\*\* 元件。</span><span class="sxs-lookup"><span data-stu-id="625d3-203">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="625d3-204">該元件會列於工作負載右側 [安裝詳細資料]\*\*\*\* 中 [開發時間的 IIS 支援]\*\*\*\* 底下的 [選擇性]\*\*\*\* 區段中。</span><span class="sxs-lookup"><span data-stu-id="625d3-204">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="625d3-205">此元件會安裝 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="625d3-205">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="625d3-206">設定專案</span><span class="sxs-lookup"><span data-stu-id="625d3-206">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="625d3-207">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="625d3-207">HTTPS redirection</span></span>

<span data-ttu-id="625d3-208">針對需要 HTTPS 的新專案，請在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 視窗中，選取 [針對 HTTPS 進行設定]\*\*\*\* 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="625d3-208">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="625d3-209">選取該核取方塊會在建立應用程式時，將 [HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)加入該應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-209">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="625d3-210">針對需要 HTTPS 的現有專案，請使用 `Startup.Configure`中的 HTTPS 重新導向和 HSTS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="625d3-210">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="625d3-211">如需詳細資訊，請參閱<xref:security/enforcing-ssl>。</span><span class="sxs-lookup"><span data-stu-id="625d3-211">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="625d3-212">針對使用 HTTP 的專案，[HTTPS 重新導向和 HSTS 中介軟體](xref:security/enforcing-ssl)並不會被加入至應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-212">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="625d3-213">您不需要進行任何應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="625d3-213">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="625d3-214">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="625d3-214">IIS launch profile</span></span>

<span data-ttu-id="625d3-215">建立新的啟動設定檔，以新增開發階段 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="625d3-215">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="625d3-216">以滑鼠右鍵按一下 [方案總管]\*\*\*\* 中的專案。</span><span class="sxs-lookup"><span data-stu-id="625d3-216">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="625d3-217">選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="625d3-217">Select **Properties**.</span></span> <span data-ttu-id="625d3-218">開啟 [**調試**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="625d3-218">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="625d3-219">針對 [設定檔]\*\*\*\*，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="625d3-219">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="625d3-220">在快顯示窗中，將設定檔命名為 "IIS"。</span><span class="sxs-lookup"><span data-stu-id="625d3-220">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="625d3-221">選取 [確定]\*\*\*\* 以建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="625d3-221">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="625d3-222">針對 [啟動]\*\*\*\* 設定，從清單中選取 [IIS]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-222">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="625d3-223">選取 [啟動瀏覽器]\*\*\*\* 的核取方塊並提供端點 URL。</span><span class="sxs-lookup"><span data-stu-id="625d3-223">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="625d3-224">當應用程式需要 HTTPS 時，請使用 HTTPS 端點 (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="625d3-224">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="625d3-225">針對 HTTP，請使用 HTTP (`http://`) 端點。</span><span class="sxs-lookup"><span data-stu-id="625d3-225">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="625d3-226">提供相同的主機名稱和連接埠作為 [IIS 設定所指定的早期用途](#configure-iis)，其通常是 `localhost`。</span><span class="sxs-lookup"><span data-stu-id="625d3-226">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="625d3-227">在 URL 結尾提供應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="625d3-227">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="625d3-228">例如，`https://localhost/WebApplication1` (HTTPS) 或 `http://localhost/WebApplication1` (HTTP) 皆為有效的端點 URL。</span><span class="sxs-lookup"><span data-stu-id="625d3-228">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="625d3-229">在 [環境變數]\*\*\*\* 區段中，選取 [新增]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="625d3-229">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="625d3-230">提供 [名稱]\*\*\*\* 為 `ASPNETCORE_ENVIRONMENT` 且 [值]\*\*\*\* 為 `Development` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="625d3-230">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="625d3-231">在 [Web 伺服器設定]\*\*\*\* 區域中，將 [應用程式 URL]\*\*\*\* 設定為用於 [啟動瀏覽器]\*\*\*\* 端點 URL 的相同值。</span><span class="sxs-lookup"><span data-stu-id="625d3-231">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="625d3-232">針對 Visual Studio 2019 或更新版本中的 [裝載模型]\*\*\*\*，請選取 [預設]\*\*\*\* 以使用專案所使用的裝載模型。</span><span class="sxs-lookup"><span data-stu-id="625d3-232">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="625d3-233">如果專案在其專案檔中已設定 `<AspNetCoreHostingModel>` 屬性，則會使用該屬性的值 (`InProcess` 或 `OutOfProcess`)。</span><span class="sxs-lookup"><span data-stu-id="625d3-233">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="625d3-234">如果該屬性不存在，便會使用應用程式的預設裝載模型，其為外部處理序。</span><span class="sxs-lookup"><span data-stu-id="625d3-234">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="625d3-235">如果應用程式需要與應用程式一般裝載模型不同的明確裝載模型設定，請視需要將 [裝載模型]\*\*\*\* 設定為 `In Process` 或 `Out Of Process`。</span><span class="sxs-lookup"><span data-stu-id="625d3-235">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="625d3-236">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="625d3-236">Save the profile.</span></span>

<span data-ttu-id="625d3-237">不使用 Visual Studio 時，請手動將啟動設定檔新增至 *Properties* 資料夾中的 [launchSettings.json](https://json.schemastore.org/launchsettings) \(英文\) 檔案。</span><span class="sxs-lookup"><span data-stu-id="625d3-237">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](https://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="625d3-238">下列範例會將專案設定為使用 HTTPS 通訊協定：</span><span class="sxs-lookup"><span data-stu-id="625d3-238">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="625d3-239">請確認 `applicationUrl` 和 `launchUrl` 端點相符，並使用和 IIS 繫結設定相同的通訊協定 (HTTP 或 HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="625d3-239">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="625d3-240">執行專案</span><span class="sxs-lookup"><span data-stu-id="625d3-240">Run the project</span></span>

<span data-ttu-id="625d3-241">以系統管理員身分執行 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="625d3-241">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="625d3-242">確認建置設定下拉式清單已設定為 [偵錯]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="625d3-242">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="625d3-243">將 [[開始調試](/visualstudio/debugger/debugger-feature-tour)程式] 按鈕設定為**IIS**設定檔，然後選取 [] 按鈕來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="625d3-243">Set the [Start Debugging button](/visualstudio/debugger/debugger-feature-tour) to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="625d3-244">如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。</span><span class="sxs-lookup"><span data-stu-id="625d3-244">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="625d3-245">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="625d3-245">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="625d3-246">如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。</span><span class="sxs-lookup"><span data-stu-id="625d3-246">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="625d3-247">使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。</span><span class="sxs-lookup"><span data-stu-id="625d3-247">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="625d3-248">例如，不會碰到中斷點。</span><span class="sxs-lookup"><span data-stu-id="625d3-248">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="625d3-249">其他資源</span><span class="sxs-lookup"><span data-stu-id="625d3-249">Additional resources</span></span>

* [<span data-ttu-id="625d3-250">IIS 中的 IIS 管理員使用者入門</span><span class="sxs-lookup"><span data-stu-id="625d3-250">Getting Started with the IIS Manager in IIS</span></span>](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)
* <xref:security/enforcing-ssl>

::: moniker-end
