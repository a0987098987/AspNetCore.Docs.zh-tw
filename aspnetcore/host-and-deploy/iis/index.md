---
title: "在使用 IIS 的 Windows 上裝載 ASP.NET Core"
author: guardrex
description: "了解如何在 Windows Server Internet Information Services (IIS) 上裝載 ASP.NET Core 應用程式。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 01cedb4e3abb35670d2908fe8cb4367c3fd58b33
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="c1cfe-103">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1cfe-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="c1cfe-104">作者：[Luke Latham](https://github.com/guardrex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c1cfe-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="c1cfe-105">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="c1cfe-105">Supported operating systems</span></span>

<span data-ttu-id="c1cfe-106">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="c1cfe-107">Windows 7 和更新版本</span><span class="sxs-lookup"><span data-stu-id="c1cfe-107">Windows 7 and newer</span></span>
* <span data-ttu-id="c1cfe-108">Windows Server 2008 R2 和更新版本&#8224;</span><span class="sxs-lookup"><span data-stu-id="c1cfe-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="c1cfe-109">&#8224;就概念而言，本文件中描述的 IIS 組態也適用於在 Nano Server IIS 上裝載 ASP.NET Core 應用程式，但特定指示需參考 [Nano Server 上的 IIS 與 ASP.NET Core](xref:tutorials/nano-server)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="c1cfe-110">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不適用 IIS 的反向 Proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="c1cfe-111">請使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="c1cfe-112">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="c1cfe-112">IIS configuration</span></span>

<span data-ttu-id="c1cfe-113">啟用**網頁伺服器 (IIS)**角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="c1cfe-114">Windows 桌面作業系統</span><span class="sxs-lookup"><span data-stu-id="c1cfe-114">Windows desktop operating systems</span></span>

<span data-ttu-id="c1cfe-115">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="c1cfe-116">開啟 [Internet Information Services] 和 [Web 管理工具] 群組。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="c1cfe-117">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="c1cfe-118">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="c1cfe-119">接受**全球資訊網服務**的預設功能，或自訂 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="c1cfe-121">Windows Server 作業系統</span><span class="sxs-lookup"><span data-stu-id="c1cfe-121">Windows Server operating systems</span></span>

<span data-ttu-id="c1cfe-122">伺服器作業系統，請透過 [伺服器管理員] 中的 [管理] 功能表或連結，使用 [新增角色及功能精靈]。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="c1cfe-123">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](index/_static/server-roles-ws2016.png)

<span data-ttu-id="c1cfe-125">在**角色服務**步驟中，選取所要的 IIS 角色服務或接受所提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![在選取角色服務步驟中，選取預設的角色服務。](index/_static/role-services-ws2016.png)

<span data-ttu-id="c1cfe-127">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="c1cfe-128">安裝網頁伺服器 (IIS) 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="c1cfe-129">安裝 .NET Core Windows Server 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="c1cfe-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="c1cfe-130">在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore-2-windowshosting)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="c1cfe-131">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="c1cfe-132">此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="c1cfe-133">如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core Windows Server 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="c1cfe-134">重新啟動系統或從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，挑選系統 PATH 的變更。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="c1cfe-135">如需 IIS 共用組態的資訊，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="c1cfe-136">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c1cfe-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="c1cfe-137">將應用程式部署到具有 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 的伺服器時，請在伺服器上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="c1cfe-138">若要安裝 Web Deploy，請使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="c1cfe-139">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="c1cfe-140">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="c1cfe-141">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="c1cfe-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="c1cfe-142">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="c1cfe-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c1cfe-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c1cfe-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c1cfe-144">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="c1cfe-145">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c1cfe-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c1cfe-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c1cfe-147">在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="c1cfe-148">將 *UseIISIntegration* 擴充方法新增至 *WebHostBuilder*，以在應用程式中併入 IIS 整合中介軟體：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="c1cfe-149">`UseKestrel` 與 `UseIISIntegration` 都是必要項目。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="c1cfe-150">呼叫 `UseIISIntegration` 的程式碼並不會影響程式碼的可攜性。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="c1cfe-151">若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會生效。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="c1cfe-152">如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="c1cfe-153">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="c1cfe-153">IIS options</span></span>

<span data-ttu-id="c1cfe-154">若要設定 IIS 選項，請在 `ConfigureServices` 中包含 `IISOptions` 的服務組態：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="c1cfe-155">選項</span><span class="sxs-lookup"><span data-stu-id="c1cfe-155">Option</span></span>                         | <span data-ttu-id="c1cfe-156">預設</span><span class="sxs-lookup"><span data-stu-id="c1cfe-156">Default</span></span> | <span data-ttu-id="c1cfe-157">設定</span><span class="sxs-lookup"><span data-stu-id="c1cfe-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="c1cfe-158">如果為 `true`，則驗證中介軟體設為 `HttpContext.User`並回應泛行挑戰。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="c1cfe-159">如果為 `false`，則驗證中介軟體僅提供身分識別 (`HttpContext.User`) 並當 `AuthenticationScheme` 提出明確要求時，回應泛型挑戰。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="c1cfe-160">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="c1cfe-161">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="c1cfe-162">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="c1cfe-163">web.config</span><span class="sxs-lookup"><span data-stu-id="c1cfe-163">web.config</span></span>

<span data-ttu-id="c1cfe-164">*web.config* 檔案的主要目的是設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="c1cfe-165">它能選擇性地提供其他 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="c1cfe-166">建立、轉換及發行 *web.config* 是由 .NET Core Web SDK (`Microsoft.NET.Sdk.Web`) 處理。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="c1cfe-167">SDK 設定在專案檔 `<Project Sdk="Microsoft.NET.Sdk.Web">` 的頂端。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="c1cfe-168">為防止 SDK 轉換 *web.config* 檔案，請將 **\<IsTransformWebConfigDisabled>** 屬性新增至專案檔，並將其設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="c1cfe-169">如果 *web.config* 檔案在專案中，則系統會使用正確的 *processPath* 和 *arguments* 轉換它以設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，然後將它移至[已發行的輸出](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="c1cfe-170">轉換不會修改檔案中的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="c1cfe-171">web.config 位置</span><span class="sxs-lookup"><span data-stu-id="c1cfe-171">web.config location</span></span>

<span data-ttu-id="c1cfe-172">.NET Core 應用程式是透過 IIS 和 Kestrel 伺服器之間的反向 Proxy 裝載。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="c1cfe-173">為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)，這是提供給 IIS 的網站實體路徑。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="c1cfe-174">應用程式的根目錄需有 *web.config* 檔案，才能使用 Web Deploy 發行多個應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="c1cfe-175">機密檔案位於應用程式的實體路徑，包括 \<組件名稱>.runtimeconfig.json、\<組件名稱>.xm*l* (XML 文件註解) 和 *組件名稱>.deps.json\<* 等子資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="c1cfe-176">當 *web.config* 檔案存在且設定網站時，IIS 會禁止使用這些機密檔案。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="c1cfe-177">**因此，請注意避免意外重新命名或從部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="c1cfe-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="c1cfe-178">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="c1cfe-178">Create the IIS Website</span></span>

1. <span data-ttu-id="c1cfe-179">在目標 IIS 系統上建立資料夾，以包含應用程式的已發佈資料夾和檔案；相關說明請參閱[目錄結構](xref:host-and-deploy/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="c1cfe-180">在資料夾內建立 *logs* 資料夾，以在啟用 stdout 記錄時保留 stdout 記錄。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="c1cfe-181">如果應用程式已在承載中部署 *logs* 資料夾，請略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="c1cfe-182">如需如何讓 MSBuild 建立 *logs* 資料夾的指示，請參閱[目錄結構](xref:host-and-deploy/directory-structure)主題。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="c1cfe-183">在 **IIS 管理員**中建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="c1cfe-184">提供**網站名稱**，並將**實體路徑**設定為應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="c1cfe-185">提供**繫結**組態並建立網站。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="c1cfe-186">將應用程式集區設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="c1cfe-187">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="c1cfe-188">開啟 [新增網站]視窗。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-188">Open the **Add Website** window.</span></span>

   ![選取 [網站] 操作功能表的 [新增網站]。](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="c1cfe-190">設定網站。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-190">Configure the website.</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="c1cfe-192">在 [應用程式集區] 面板中，以滑鼠右鍵按一下網站的應用程式集區，並從快顯功能表中選取 [基本設定...]，開啟 [編輯應用程式集區] 窗格。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![選取 [應用程式集區] 操作功能表的 [基本設定]。](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="c1cfe-194">將 **.NET CLR 版本**設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![將 .NET CLR 版本設為 [沒有 Managed 程式碼]。](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="c1cfe-196">注意：將 **.NET CLR 版本**設為 [沒有 Managed 程式碼] 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="c1cfe-197">ASP.NET Core 不依賴載入桌面 CLR。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="c1cfe-198">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="c1cfe-199">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="c1cfe-200">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="c1cfe-200">Deploy the app</span></span>

<span data-ttu-id="c1cfe-201">將應用程式部署至您在目標 IIS 系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="c1cfe-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="c1cfe-203">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="c1cfe-204">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="c1cfe-205">無法覆寫鎖定的檔案。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="c1cfe-206">若要釋放部署內的鎖定檔案，請停止應用程式集區：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="c1cfe-207">伺服器上的 IIS 管理員手動方式。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="c1cfe-208">使用 Web Deploy 和參考專案檔中的 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="c1cfe-209">*app_offline.htm* 檔案是放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="c1cfe-210">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="c1cfe-211">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="c1cfe-212">使用 PowerShell 停止和重新啟動應用程式集區 (需要 PowerShell 5 或更新版本)：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="c1cfe-213">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c1cfe-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="c1cfe-214">請參閱[適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)主題，了解如何建立搭配 Web Deploy 使用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="c1cfe-215">如果裝載提供者提供或支援建立發行設定檔，請下載並使用 Visual Studio 的 [發行] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="c1cfe-217">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="c1cfe-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="c1cfe-218">透過命令列也可以在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="c1cfe-219">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="c1cfe-220">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="c1cfe-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="c1cfe-221">有多種方法可將應用程式移到 Xcopy、Robocopy 或 PowerShell 等主機系統。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="c1cfe-222">Visual Studio 使用者可以使用[發佈範例](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="c1cfe-223">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="c1cfe-223">Browse the website</span></span>

![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="c1cfe-225">資料保護</span><span class="sxs-lookup"><span data-stu-id="c1cfe-225">Data protection</span></span>

<span data-ttu-id="c1cfe-226">有數個 ASP.NET 中介軟體使用資料保護，包括在驗證中使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="c1cfe-227">即使不從使用者程式碼呼叫資料保護 API (DPAPI) ，仍應使用部署指令碼設定資料保護，或以使用者程式碼建立持續性的金鑰存放區。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="c1cfe-228">如不設定資料保護，金鑰會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="c1cfe-229">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="c1cfe-230">所有表單驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="c1cfe-231">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="c1cfe-232">所有以 keyring 保護的資料都無法再解密。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="c1cfe-233">若要在 IIS 下設定資料保護，請使用下列方法**之一**：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="c1cfe-234">建立資料保護登錄 Hive</span><span class="sxs-lookup"><span data-stu-id="c1cfe-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="c1cfe-235">ASP.NET 應用程式使用的資料保護金鑰會儲存在應用程式外部的登錄 Hive 中。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="c1cfe-236">若要保存指定應用程式的金鑰，請為應用程式集區建立登錄 Hive。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="c1cfe-237">若為獨立的非Web 伺服陣列 IIS 安裝，請針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="c1cfe-238">此指令碼會在 HKLM 登錄中建立特殊的登錄機碼，其只會將背景工作處理序帳戶列入 ACL。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-238">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="c1cfe-239">在待用期間使用 DPAPI 和全電腦金鑰加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="c1cfe-240">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="c1cfe-241">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="c1cfe-242">請確保這類共用的檔案權限僅限於執行應用程式的 Windows 帳戶身分。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="c1cfe-243">此外，可以使用 X509 憑證保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="c1cfe-244">請考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="c1cfe-245">詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="c1cfe-246">設定 IIS 應用程式集區載入使用者設定檔</span><span class="sxs-lookup"><span data-stu-id="c1cfe-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="c1cfe-247">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="c1cfe-248">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="c1cfe-249">這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="c1cfe-250">將檔案系統當作 Keyring 存放區使用</span><span class="sxs-lookup"><span data-stu-id="c1cfe-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="c1cfe-251">調整應用程式程式碼，以[將檔案系統當作 Keyring 存放區使用](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="c1cfe-252">使用 X509 憑證保護 Keyring，確保憑證是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="c1cfe-253">如果它是自我簽署的憑證，請將憑證放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="c1cfe-254">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="c1cfe-255">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="c1cfe-256">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="c1cfe-257">設定[程式碼中的資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="c1cfe-258">設定資料保護的全電腦原則</span><span class="sxs-lookup"><span data-stu-id="c1cfe-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="c1cfe-259">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy)設定。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="c1cfe-260">如需詳細資訊，請參閱[資料保護](xref:security/data-protection/index)文件。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="c1cfe-261">子應用程式的組態</span><span class="sxs-lookup"><span data-stu-id="c1cfe-261">Configuration of sub-applications</span></span>

<span data-ttu-id="c1cfe-262">根應用程式下新增的子應用程式不應包含當作處理常式的 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="c1cfe-263">如果您在子應用程式的 *web.config* 檔案中，將模組新增為處理常式，則當您嘗試瀏覽子應用程式時，會收到參考錯誤組態檔的 500.19 (內部伺服器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="c1cfe-264">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案內容：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="c1cfe-265">在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式時，請明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="c1cfe-266">如需設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="c1cfe-267">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="c1cfe-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="c1cfe-268">IIS 組態受到這些適用於反向 Proxy 組態之 IIS 功能 *web.config* 的 **\<system.webServer>** 區段所影響。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="c1cfe-269">如果在系統層級設定 IIS 使用動態壓縮，則在應用程式的 *web.config* 檔案中有 **\<urlCompression>** 項目的應用程式，可停用該項設定。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="c1cfe-270">如需詳細資訊，請參閱 [\<system.webServer> 的組態參考](https://docs.microsoft.com/iis/configuration/system.webServer/)、[ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module)以及[使用 IIS 模組與 ASP.NET Core](xref:host-and-deploy/iis/modules)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="c1cfe-271">如果需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="c1cfe-272">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="c1cfe-272">Configuration sections of web.config</span></span>

<span data-ttu-id="c1cfe-273">ASP.NET Core 應用程式的組態不使用 *web.config* 中的 ASP.NET Framework 應用程式組態區段：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="c1cfe-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="c1cfe-274">**\<system.web>**</span></span>
* <span data-ttu-id="c1cfe-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="c1cfe-275">**\<appSettings>**</span></span>
* <span data-ttu-id="c1cfe-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="c1cfe-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="c1cfe-277">**\<位置>**</span><span class="sxs-lookup"><span data-stu-id="c1cfe-277">**\<location>**</span></span>

<span data-ttu-id="c1cfe-278">使用其他組態提供者設定的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="c1cfe-279">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="c1cfe-280">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="c1cfe-280">Application Pools</span></span>

<span data-ttu-id="c1cfe-281">在單一系統上裝載多個網站時，請在其各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="c1cfe-282">IIS [新增網站] 對話方塊預設成此組態。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="c1cfe-283">當您提供**網站名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="c1cfe-284">新增網站時，會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="c1cfe-285">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="c1cfe-285">Application Pool Identity</span></span>

<span data-ttu-id="c1cfe-286">應用程式集區身分識別帳戶可讓應用程式在唯一的帳戶下執行，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="c1cfe-287">在 IIS 8.0+ 中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="c1cfe-288">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![應用程式集區進階設定對話方塊](index/_static/apppool-identity.png)

<span data-ttu-id="c1cfe-290">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="c1cfe-291">使用這個身分識別時，即可保護資源；不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="c1cfe-292">如果 IIS 背景工作處理序需要提升應用程式的存取權限，請修改包含應用程式的目錄存取控制清單 (ACL)：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="c1cfe-293">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="c1cfe-294">以滑鼠右鍵按一下目錄並選取 [屬性]。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="c1cfe-295">依序選取 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="c1cfe-296">選取 [位置] 按鈕，並確定選取系統。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="c1cfe-297">在 [輸入要選取的物件名稱] 文字方塊中輸入 **IIS AppPool\DefaultAppPool**。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![選取應用程式資料夾的 [使用者或群組] 對話方塊](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="c1cfe-299">選取 [檢查名稱] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-299">Select the **Check Names** button.</span></span> <span data-ttu-id="c1cfe-300">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c1cfe-300">Select **OK**.</span></span>

   ![選取應用程式資料夾的 [使用者或群組] 對話方塊](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="c1cfe-302">也可使用 **ICACLS** 工具透過命令提示字元完成：</span><span class="sxs-lookup"><span data-stu-id="c1cfe-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="c1cfe-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="c1cfe-303">Additional resources</span></span>

* [<span data-ttu-id="c1cfe-304">針對 IIS 上的 ASP.NET Core 進行疑難排解</span><span class="sxs-lookup"><span data-stu-id="c1cfe-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="c1cfe-305">Azure 應用程式服務和 IIS 常見的 ASP.NET Core 錯誤參考</span><span class="sxs-lookup"><span data-stu-id="c1cfe-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="c1cfe-306">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="c1cfe-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="c1cfe-307">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="c1cfe-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="c1cfe-308">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1cfe-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="c1cfe-309">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="c1cfe-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="c1cfe-310">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="c1cfe-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="c1cfe-311">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="c1cfe-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
