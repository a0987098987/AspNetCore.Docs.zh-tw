---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: shirhatti
description: 了解當 ASP.NET Core 應用程式在 Windows Server 上的 IIS 後方執行時，對其提供的偵錯支援。
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862378"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="e04d0-103">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="e04d0-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="e04d0-104">作者：[Sourabh Shirhatti](https://twitter.com/sshirhatti) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e04d0-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e04d0-105">本文針對在 Windows Server 上 IIS 後方執行的 ASP.NET Core 應用程式，說明 [Visual Studio](https://www.visualstudio.com/vs/) 的偵錯支援。</span><span class="sxs-lookup"><span data-stu-id="e04d0-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="e04d0-106">本主題會逐步解說如何啟用這項功能及設定專案。</span><span class="sxs-lookup"><span data-stu-id="e04d0-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e04d0-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="e04d0-107">Prerequisites</span></span>

* [<span data-ttu-id="e04d0-108">Visual Studio (適用於 Windows)</span><span class="sxs-lookup"><span data-stu-id="e04d0-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="e04d0-109">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="e04d0-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="e04d0-110">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="e04d0-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="e04d0-111">X.509 安全性憑證</span><span class="sxs-lookup"><span data-stu-id="e04d0-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="e04d0-112">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="e04d0-112">Enable IIS</span></span>

1. <span data-ttu-id="e04d0-113">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="e04d0-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="e04d0-114">選取 [Internet Information Services] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="e04d0-114">Select the **Internet Information Services** check box.</span></span>

![[Windows 功能] 會以黑色方塊 (而非勾選記號) 顯示選取的 [Internet Information Services] 核取方塊，表示已啟用部分 IIS 功能](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="e04d0-116">安裝 IIS 可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="e04d0-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="e04d0-117">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="e04d0-117">Configure IIS</span></span>

<span data-ttu-id="e04d0-118">IIS 的網站必須含有下列設定：</span><span class="sxs-lookup"><span data-stu-id="e04d0-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="e04d0-119">與應用程式啟動設定檔 URL 主機名稱相符的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="e04d0-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="e04d0-120">搭配所指派憑證的連接埠 443 繫結。</span><span class="sxs-lookup"><span data-stu-id="e04d0-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="e04d0-121">例如，所新增網站的 [主機名稱] 會設定為 "localhost" (啟動設定檔稍後在本主題中也會使用 "localhost")。</span><span class="sxs-lookup"><span data-stu-id="e04d0-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="e04d0-122">連接埠會設定為 "443" (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="e04d0-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="e04d0-123">已將 [IIS Express 開發憑證] 指派給網站，但可使用任何有效的憑證：</span><span class="sxs-lookup"><span data-stu-id="e04d0-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![IIS 中的 [新增網站] 視窗，其中顯示具有已指派的憑證並在連接埠 443 上 localhost 的繫結集合。](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="e04d0-125">如果 IIS 安裝已經有主機名稱與應用程式啟動設定檔 URL 主機名稱相符的「預設網站」：</span><span class="sxs-lookup"><span data-stu-id="e04d0-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="e04d0-126">為連接埠 443 (HTTPS) 新增連接埠繫結。</span><span class="sxs-lookup"><span data-stu-id="e04d0-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="e04d0-127">指派一個有效的憑證給網站。</span><span class="sxs-lookup"><span data-stu-id="e04d0-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="e04d0-128">在 Visual Studio 中啟用開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="e04d0-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="e04d0-129">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e04d0-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="e04d0-130">選取 [開發階段 IIS 支援] 元件。</span><span class="sxs-lookup"><span data-stu-id="e04d0-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="e04d0-131">此元件在 [ASP.NET 與網頁程式開發] 工作負載的 [摘要] 面板中，會列為選擇性元件。</span><span class="sxs-lookup"><span data-stu-id="e04d0-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="e04d0-132">此元件會安裝 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，這是搭配 IIS 執行 ASP.NET Core 應用程式所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="e04d0-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="e04d0-136">設定專案</span><span class="sxs-lookup"><span data-stu-id="e04d0-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="e04d0-137">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="e04d0-137">HTTPS redirection</span></span>

<span data-ttu-id="e04d0-138">針對新的專案，請在 [新增 ASP.NET Core Web 應用程式] 視窗中，選取 [針對 HTTPS 進行設定] 核取方塊：</span><span class="sxs-lookup"><span data-stu-id="e04d0-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![已選取 [針對 HTTPS 進行設定] 核取方塊的 [新增 ASP.NET Core Web 應用程式] 視窗。](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="e04d0-140">在現有的專案中，請在 `Startup.Configure` 中透過呼叫 [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) 擴充方法來使用「HTTPS 重新導向中介軟體」：</span><span class="sxs-lookup"><span data-stu-id="e04d0-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="e04d0-141">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="e04d0-141">IIS launch profile</span></span>

<span data-ttu-id="e04d0-142">建立新的啟動設定檔，以新增開發階段 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="e04d0-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="e04d0-143">針對 [設定檔]，選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e04d0-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="e04d0-144">在快顯示窗中，將設定檔命名為 "IIS"。</span><span class="sxs-lookup"><span data-stu-id="e04d0-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="e04d0-145">選取 [確定] 以建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="e04d0-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="e04d0-146">針對 [啟動] 設定，從清單中選取 [IIS]。</span><span class="sxs-lookup"><span data-stu-id="e04d0-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="e04d0-147">選取 [啟動瀏覽器] 的核取方塊並提供端點 URL。</span><span class="sxs-lookup"><span data-stu-id="e04d0-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="e04d0-148">使用 HTTPS 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="e04d0-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="e04d0-149">這個範例會使用 `https://localhost/WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="e04d0-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="e04d0-150">在 [環境變數] 區段中，選取 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e04d0-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="e04d0-151">提供一個索引鍵為 `ASPNETCORE_ENVIRONMENT` 且值為 `Development` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="e04d0-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="e04d0-152">在 [Web 伺服器設定] 區域中，設定 [應用程式 URL]。</span><span class="sxs-lookup"><span data-stu-id="e04d0-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="e04d0-153">這個範例會使用 `https://localhost/WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="e04d0-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="e04d0-154">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="e04d0-154">Save the profile.</span></span>

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="e04d0-159">或者，您也可以手動將啟動設定檔新增至應用程式中的 [launchSettings.json](http://json.schemastore.org/launchsettings) 檔案：</span><span class="sxs-lookup"><span data-stu-id="e04d0-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="e04d0-160">執行專案</span><span class="sxs-lookup"><span data-stu-id="e04d0-160">Run the project</span></span>

<span data-ttu-id="e04d0-161">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="e04d0-161">In Visual Studio:</span></span>

* <span data-ttu-id="e04d0-162">確認建置設定下拉式清單已設定為 [偵錯]。</span><span class="sxs-lookup"><span data-stu-id="e04d0-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="e04d0-163">將 [執行] 按鈕設定為 **IIS** 設定檔，然後選取該按鈕來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e04d0-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![VS 工具列中的 [執行] 按鈕已設定為 IIS 設定檔，而且建置設定下拉式清單已設定為 [發行]。](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="e04d0-165">如果不是以系統管理員身分執行，Visual Studio 可能會提示您重新啟動。</span><span class="sxs-lookup"><span data-stu-id="e04d0-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="e04d0-166">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="e04d0-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="e04d0-167">如果使用未受信任的開發憑證，瀏覽器可能會要求您針對未受信任的憑證建立例外。</span><span class="sxs-lookup"><span data-stu-id="e04d0-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="e04d0-168">使用 [Just My Code](/visualstudio/debugger/just-my-code) 與編譯器最佳化針對發行建置設定進行偵錯會導致體驗變差。</span><span class="sxs-lookup"><span data-stu-id="e04d0-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="e04d0-169">例如，不會碰到中斷點。</span><span class="sxs-lookup"><span data-stu-id="e04d0-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e04d0-170">其他資源</span><span class="sxs-lookup"><span data-stu-id="e04d0-170">Additional resources</span></span>

* [<span data-ttu-id="e04d0-171">使用 IIS 在 Windows 上裝載 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e04d0-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="e04d0-172">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="e04d0-172">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="e04d0-173">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="e04d0-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="e04d0-174">強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="e04d0-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
