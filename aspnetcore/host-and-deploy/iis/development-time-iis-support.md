---
title: Visual Studio for ASP.NET Core 中的開發階段 IIS 支援
author: shirhatti
description: 探索 Windows 伺服器上執行 IIS 後方時，偵錯 ASP.NET Core 應用程式的支援。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="0cd5a-103">Visual Studio for ASP.NET Core 中的開發階段 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="0cd5a-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="0cd5a-104">由[Sourabh Shirhatti](https://twitter.com/sshirhatti)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0cd5a-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0cd5a-105">本文說明[Visual Studio](https://www.visualstudio.com/vs/)支援 Windows Server 上執行後 IIS 的 ASP.NET Core 應用程式偵錯。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="0cd5a-106">本主題引導啟用此功能，以及設定專案。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0cd5a-107">必要條件</span><span class="sxs-lookup"><span data-stu-id="0cd5a-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="0cd5a-108">啟用 IIS</span><span class="sxs-lookup"><span data-stu-id="0cd5a-108">Enable IIS</span></span>

1. <span data-ttu-id="0cd5a-109">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="0cd5a-110">選取**Internet Information Services**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-110">Select the **Internet Information Services** check box.</span></span>

![檢查變成黑色方塊 （未勾選記號） 表示的部分 IIS 功能已啟用 Windows 功能顯示 Internet Information Services 核取方塊](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="0cd5a-112">IIS 安裝可能需要重新啟動系統。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="0cd5a-113">設定 IIS</span><span class="sxs-lookup"><span data-stu-id="0cd5a-113">Configure IIS</span></span>

<span data-ttu-id="0cd5a-114">IIS 必須設定下列網站：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="0cd5a-115">主機名稱符合應用程式的啟動設定檔 URL 主機名稱。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="0cd5a-116">指派的憑證與連接埠 443 繫結。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="0cd5a-117">例如，**主機名稱**新增的網站設定為"localhost"（啟動設定檔也會使用"localhost"本主題稍後的）。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="0cd5a-118">連接埠會設定為"443"(HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="0cd5a-119">**IIS Express 開發憑證**指派給網站，但任何有效的憑證適用於：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![在 IIS 連接埠 443 上顯示為本機主機設定的繫結，以指派憑證中加入網站視窗。](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="0cd5a-121">如果 IIS 已經安裝具有**Default Web Site**符合應用程式的啟動設定檔 URL 主機名稱的主機名稱：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="0cd5a-122">加入連接埠 443 (HTTPS) 連接埠繫結。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="0cd5a-123">為網站指派有效的憑證。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="0cd5a-124">啟用 Visual Studio 中的開發時間 IIS 支援</span><span class="sxs-lookup"><span data-stu-id="0cd5a-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="0cd5a-125">啟動 Visual Studio 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="0cd5a-126">選取**支援 IIS 的開發時間**元件。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="0cd5a-127">元件會列在為選擇性**摘要** 面板的**ASP.NET 及 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="0cd5a-128">元件會安裝[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)，這是執行 ASP.NET Core 背後 IIS 應用程式中的反向 proxy 設定所需的原生 IIS 模組。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![修改 Visual Studio 功能：選取 [工作負載] 索引標籤。](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="0cd5a-132">設定專案</span><span class="sxs-lookup"><span data-stu-id="0cd5a-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="0cd5a-133">HTTPS 的重新導向</span><span class="sxs-lookup"><span data-stu-id="0cd5a-133">HTTPS redirection</span></span>

<span data-ttu-id="0cd5a-134">對於新的專案中，選取核取方塊，以**設定以進行 HTTPS**中**新 ASP.NET Core Web 應用程式**視窗：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![具有 HTTPS 核取方塊，選取 設定的新 ASP.NET Core Web 應用程式視窗。](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="0cd5a-136">在現有的專案中，使用 HTTPS 重新導向中介軟體中`Startup.Configure`藉由呼叫[UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)擴充方法：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="0cd5a-137">IIS 啟動設定檔</span><span class="sxs-lookup"><span data-stu-id="0cd5a-137">IIS launch profile</span></span>

<span data-ttu-id="0cd5a-138">建立新的啟動設定檔，以加入開發時間 IIS 支援：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="0cd5a-139">如**設定檔**，選取**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="0cd5a-140">名稱"IIS"設定檔快顯視窗中。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="0cd5a-141">選取**確定**建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="0cd5a-142">如**啟動**設定中，選取**IIS**從清單中。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="0cd5a-143">選取核取方塊**啟動瀏覽器**和提供的端點 URL。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="0cd5a-144">使用 HTTPS 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="0cd5a-145">這個範例會使用 `https://localhost/WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="0cd5a-146">在**環境變數**區段中，選取**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="0cd5a-147">提供索引鍵為環境變數`ASPNETCORE_ENVIRONMENT`以及值`Development`。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="0cd5a-148">在**網頁伺服器設定**區域中，設定**應用程式 URL**。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="0cd5a-149">這個範例會使用 `https://localhost/WebApplication1`。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="0cd5a-150">儲存設定檔。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-150">Save the profile.</span></span>

![在 [專案屬性] 視窗中選取 [偵錯] 索引標籤。](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="0cd5a-155">或者，手動新增至啟動設定檔[launchSettings.json](http://json.schemastore.org/launchsettings)應用程式中的檔案：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="0cd5a-156">執行專案</span><span class="sxs-lookup"><span data-stu-id="0cd5a-156">Run the project</span></span>

<span data-ttu-id="0cd5a-157">在 VS UI 中，設定為 [執行] 按鈕**IIS**剖析，並選取 [啟動應用程式] 按鈕：</span><span class="sxs-lookup"><span data-stu-id="0cd5a-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![執行與工具列設定為"IIS"設定檔中的按鈕。](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="0cd5a-159">如果未以系統管理員身分執行 visual Studio 可能會提示重新啟動。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="0cd5a-160">若收到提示，請重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="0cd5a-161">如果使用不受信任的程式開發憑證瀏覽器可能會需要您建立受信任的憑證的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0cd5a-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0cd5a-162">其他資源</span><span class="sxs-lookup"><span data-stu-id="0cd5a-162">Additional resources</span></span>

* [<span data-ttu-id="0cd5a-163">使用 IIS 在 Windows 上裝載 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0cd5a-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="0cd5a-164">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="0cd5a-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="0cd5a-165">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="0cd5a-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="0cd5a-166">強制使用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="0cd5a-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
