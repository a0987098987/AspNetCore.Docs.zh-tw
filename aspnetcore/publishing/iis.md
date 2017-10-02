---
title: "在使用 IIS 的 Windows 上裝載 ASP.NET Core"
author: guardrex
description: "ASP.NET Core 應用程式的 Windows Server Internet Information Services (IIS) 組態及部署。"
keywords: "ASP.NET Core, internet information services, iis, windows server, 裝載套件組合, asp.net core 模組, web deploy"
ms.author: riande
manager: wpickett
ms.date: 03/13/2017
ms.topic: article
ms.assetid: a4449ad3-5bad-410c-afa7-dc32d832b552
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/iis
ms.openlocfilehash: 75fc1edec9050a4690a39d37307f2f95f5d534a5
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="0c0b0-104">在使用 IIS 的 Windows 上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c0b0-104">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="0c0b0-105">作者：[Luke Latham](https://github.com/guardrex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0c0b0-105">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="0c0b0-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="0c0b0-106">Supported operating systems</span></span>

<span data-ttu-id="0c0b0-107">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="0c0b0-108">Windows 7 和更新版本</span><span class="sxs-lookup"><span data-stu-id="0c0b0-108">Windows 7 and newer</span></span>
* <span data-ttu-id="0c0b0-109">Windows Server 2008 R2 和更新版本&#8224;</span><span class="sxs-lookup"><span data-stu-id="0c0b0-109">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="0c0b0-110">&#8224;就概念而言，本文件中描述的 IIS 組態也適用於在 Nano Server IIS 上裝載 ASP.NET Core 應用程式，但特定指示需參考 [Nano Server 上的 IIS 與 ASP.NET Core](xref:tutorials/nano-server)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-110">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core applications on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="0c0b0-111">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys) (先前稱為 [WebListener](xref:fundamentals/servers/weblistener)) 不適用 IIS 的反向 Proxy 設定。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse-proxy configuration with IIS.</span></span> <span data-ttu-id="0c0b0-112">您必須使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-112">You must use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="0c0b0-113">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="0c0b0-113">IIS configuration</span></span>

<span data-ttu-id="0c0b0-114">啟用**網頁伺服器 (IIS)**角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-114">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="0c0b0-115">Windows 桌面作業系統</span><span class="sxs-lookup"><span data-stu-id="0c0b0-115">Windows desktop operating systems</span></span>

<span data-ttu-id="0c0b0-116">瀏覽至控制台 > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (畫面左側)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-116">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="0c0b0-117">開啟 [Internet Information Services] 和 [Web 管理工具] 群組。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-117">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="0c0b0-118">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-118">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="0c0b0-119">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-119">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="0c0b0-120">接受**全球資訊網服務**的預設功能，或自訂符合需求的 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-120">Accept the default features for **World Wide Web Services** or customize the IIS features to suit your needs.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="0c0b0-122">Windows Server 作業系統</span><span class="sxs-lookup"><span data-stu-id="0c0b0-122">Windows Server operating systems</span></span>

<span data-ttu-id="0c0b0-123">伺服器作業系統，請透過 [伺服器管理員] 中的 [管理] 功能表或連結，使用 [新增角色及功能精靈]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-123">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="0c0b0-124">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-124">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](iis/_static/server-roles-ws2016.png)

<span data-ttu-id="0c0b0-126">在**角色服務**步驟中，選取您想要的 IIS 角色服務或接受提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-126">On the **Role services** step, select the IIS role services you desire or accept the default role services provided.</span></span>

![在選取角色服務步驟中，選取預設的角色服務。](iis/_static/role-services-ws2016.png)

<span data-ttu-id="0c0b0-128">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-128">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="0c0b0-129">安裝網頁伺服器 (IIS) 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-129">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="0c0b0-130">安裝 .NET Core Windows Server 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="0c0b0-130">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="0c0b0-131">在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore.2.0.0-windowshosting)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-131">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting) on the hosting system.</span></span> <span data-ttu-id="0c0b0-132">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-132">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="0c0b0-133">此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-133">The module creates the reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="0c0b0-134">如果系統沒有網際網路連線，請先取得並安裝 [Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)，再安裝 .NET Core Windows Server 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-134">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="0c0b0-135">重新啟動系統或從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，挑選系統 PATH 的變更。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-135">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="0c0b0-136">如果使用 IIS 共用組態，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-136">If you use an IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="0c0b0-137">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0c0b0-137">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="0c0b0-138">如果您想要在 [Visual Studio](https://www.visualstudio.com/vs/) 中使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 部署應用程式，請在主控系統上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-138">If you intend to deploy your applications with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) in [Visual Studio](https://www.visualstudio.com/vs/), install the latest version of Web Deploy on the hosting system.</span></span> <span data-ttu-id="0c0b0-139">若要安裝 Web Deploy，您可以使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從[Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-139">To install Web Deploy, you can use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="0c0b0-140">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-140">The preferred method is to use WebPI.</span></span> <span data-ttu-id="0c0b0-141">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-141">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="0c0b0-142">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="0c0b0-142">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="0c0b0-143">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="0c0b0-143">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0c0b0-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0c0b0-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="0c0b0-145">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-145">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="0c0b0-146">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-146">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0c0b0-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0c0b0-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="0c0b0-148">在應用程式相依性中，納入對 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件的相依性。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-148">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in your app dependencies.</span></span> <span data-ttu-id="0c0b0-149">將 *UseIISIntegration* 擴充方法新增至 *WebHostBuilder*，以在應用程式中併入 IIS 整合中介軟體：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-149">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="0c0b0-150">`UseKestrel` 與 `UseIISIntegration` 都是必要項目。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-150">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="0c0b0-151">呼叫 *UseIISIntegration* 的程式碼並不會影響程式碼的可攜性。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-151">Code calling *UseIISIntegration* doesn't affect code portability.</span></span> <span data-ttu-id="0c0b0-152">若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會生效。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-152">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="0c0b0-153">如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-153">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="0c0b0-154">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="0c0b0-154">IIS options</span></span>

<span data-ttu-id="0c0b0-155">若要設定 *IISIntegration* 服務選項，*ConfigureServices* 中請包括 *IISOptions* 的服務組態：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-155">To configure *IISIntegration* service options, include a service configuration for *IISOptions* in *ConfigureServices*:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="0c0b0-156">選項</span><span class="sxs-lookup"><span data-stu-id="0c0b0-156">Option</span></span>                         | <span data-ttu-id="0c0b0-157">預設</span><span class="sxs-lookup"><span data-stu-id="0c0b0-157">Default</span></span> | <span data-ttu-id="0c0b0-158">設定</span><span class="sxs-lookup"><span data-stu-id="0c0b0-158">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="0c0b0-159">如果為 `true`，則驗證中介軟體設為 `HttpContext.User`並回應泛行挑戰。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-159">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="0c0b0-160">如果為 `false`，則驗證中介軟體僅提供身分識別 (`HttpContext.User`) 並當 `AuthenticationScheme` 提出明確要求時，回應泛型挑戰。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-160">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="0c0b0-161">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-161">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="0c0b0-162">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-162">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="0c0b0-163">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-163">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="0c0b0-164">web.config</span><span class="sxs-lookup"><span data-stu-id="0c0b0-164">web.config</span></span>

<span data-ttu-id="0c0b0-165">*web.config* 檔案會設定 ASP.NET Core 模組，並提供其他 IIS 組態。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-165">The *web.config* file configures the ASP.NET Core Module and provides other IIS configuration.</span></span> <span data-ttu-id="0c0b0-166">*web.config* 的建立、轉換及發佈作業是由 `Microsoft.NET.Sdk.Web` 來處理，當您在專案 (*.csproj*) 檔案 `<Project Sdk="Microsoft.NET.Sdk.Web">` 的上方設定專案 SDK 時，即會包含此項目。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-166">Creating, transforming, and publishing *web.config* is handled by `Microsoft.NET.Sdk.Web`, which is included when you set your project's SDK at the top of your project (*.csproj*) file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="0c0b0-167">為防止 MSBuild 目標轉換您的 *web.config* 檔案，請將 **\<IsTransformWebConfigDisabled>** 屬性新增至設定為 `true` 的專案檔：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-167">To prevent the MSBuild target from transforming your *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to your project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="0c0b0-168">如果專案中沒有 *web.config* 檔案，當您使用 *dotnet publish* 或 Visual Studio publish 進行發佈時，系統就會為您在[已發佈的輸出](xref:hosting/directory-structure)中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-168">If you don't have a *web.config* file in the project when you publish with *dotnet publish* or with Visual Studio publish, the file is created for you in [published output](xref:hosting/directory-structure).</span></span> <span data-ttu-id="0c0b0-169">如果專案中具備 *web.config* 檔案，則系統會使用正確的 *processPath* 和 *arguments* 來進行轉換，並設定 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)，然後將其移至已發佈的輸出。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-169">If you have a *web.config* file in your project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to published output.</span></span> <span data-ttu-id="0c0b0-170">轉換不會觸碰檔案包含的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-170">The transformation doesn't touch IIS configuration settings that you've included in the file.</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="0c0b0-171">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="0c0b0-171">Create the IIS Website</span></span>

1. <span data-ttu-id="0c0b0-172">在目標 IIS 系統上建立資料夾，以包含應用程式的已發佈資料夾和檔案；相關說明請參閱[目錄結構](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-172">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:hosting/directory-structure).</span></span>

2. <span data-ttu-id="0c0b0-173">在您建立的資料夾內，建立 *logs* 資料夾以保存 stdout 記錄檔 (若您打算啟用記錄來針對啟動問題進行疑難排解的話)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-173">Within the folder you created, create a *logs* folder to hold stdout logs (if you plan to enable logging to troubleshoot start-up issues).</span></span> <span data-ttu-id="0c0b0-174">如果您打算以將 *logs* 資料夾放在承載中的方式來部署您的應用程式，您可以略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-174">If you plan to deploy your application with a *logs* folder in the payload, you may skip this step.</span></span> <span data-ttu-id="0c0b0-175">目前有一項[自動建立資料夾方面的開放問題](https://github.com/aspnet/AspNetCoreModule/issues/30)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-175">There's an [open issue to create the folder automatically](https://github.com/aspnet/AspNetCoreModule/issues/30).</span></span> <span data-ttu-id="0c0b0-176">若要讓 MSBuild 為您建立 *log* 資料夾，請在專案檔中新增下列 `Target`：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-176">If you would like MSBuild to create the *log* folder for you, add the following `Target` to your project file:</span></span>

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
     <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
     <MakeDir Directories="$(PublishUrl)logs" Condition="!Exists('$(PublishUrl)logs')" />
   </Target>
   ```

3. <span data-ttu-id="0c0b0-177">在 **IIS 管理員**中建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-177">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="0c0b0-178">提供**網站名稱**，並將**實體路徑**設定為您建立的應用程式部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-178">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="0c0b0-179">提供**繫結**組態並建立網站。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-179">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="0c0b0-180">將應用程式集區設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-180">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="0c0b0-181">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-181">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="0c0b0-182">開啟 [新增網站]視窗。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-182">Open the **Add Website** window.</span></span>

   ![按一下 [網站] 操作功能表的 [新增網站]。](iis/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="0c0b0-184">設定網站。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-184">Configure the website.</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](iis/_static/add-website-ws2016.png)

7. <span data-ttu-id="0c0b0-186">在 [應用程式集區] 面板中，以滑鼠右鍵按一下網站的應用程式集區，並從快顯功能表中選取 [基本設定...]，開啟 [編輯應用程式集區] 窗格。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-186">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![選取 [應用程式集區] 操作功能表的 [基本設定]。](iis/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="0c0b0-188">將 **.NET CLR 版本**設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-188">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![將 .NET CLR 版本設為 [沒有 Managed 程式碼]。](iis/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="0c0b0-190">注意：將 **.NET CLR 版本**設為 [沒有 Managed 程式碼] 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-190">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="0c0b0-191">ASP.NET Core 不依賴載入桌面 CLR。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-191">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="0c0b0-192">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-192">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="0c0b0-193">如果您將應用程式集區的預設身分識別 ([處理序模型] > [身分識別]) 從 **ApplicationPoolIdentity** 變更為其他身分識別，請驗證新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-193">If you change the default identity of the app pool (**Process Model** > **Identity**) from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-application"></a><span data-ttu-id="0c0b0-194">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="0c0b0-194">Deploy the application</span></span>
<span data-ttu-id="0c0b0-195">將應用程式部署至您在目標 IIS 系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-195">Deploy the application to the folder you created on the target IIS system.</span></span> <span data-ttu-id="0c0b0-196">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-196">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span> <span data-ttu-id="0c0b0-197">Web Deploy 的替代項目列示如下。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-197">Alternatives to Web Deploy are listed below.</span></span>

<span data-ttu-id="0c0b0-198">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-198">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="0c0b0-199">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-199">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="0c0b0-200">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-200">Deployment can't occur because locked files can't be copied.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="0c0b0-201">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0c0b0-201">Web Deploy with Visual Studio</span></span>
<span data-ttu-id="0c0b0-202">請參閱[建立 Visual Studio 和 MSBuild 的發行設定檔，以部署 ASP.NET Core 應用程式](xref:publishing/web-publishing-vs#publish-profiles)主題，了解如何建立發行設定檔供 Web Deploy 使用。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-202">See [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="0c0b0-203">如果您的裝載提供者提供或支援建立發行設定檔，請下載並使用 Visual Studio 的 [發佈] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-203">If your hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="0c0b0-205">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="0c0b0-205">Web Deploy outside of Visual Studio</span></span>
<span data-ttu-id="0c0b0-206">您也可以透過命令列在 Visual Studio 外部使用 [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-206">You can also use [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) outside of Visual Studio from the command line.</span></span> <span data-ttu-id="0c0b0-207">如需詳細資訊，請參閱 [Web 部署工具](/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-207">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="0c0b0-208">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-208">Alternatives to Web Deploy</span></span>
<span data-ttu-id="0c0b0-209">如果您不想使用 Web Deploy 或不是使用 Visual Studio，您可以使用 Xcopy、Robocopy 或 PowerShell 等任何一種方法，將應用程式移至主控系統。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-209">If you don't wish to use Web Deploy or are not using Visual Studio, you may use any of several methods to move the application to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="0c0b0-210">Visual Studio 使用者可以使用[發佈範例](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-210">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="0c0b0-211">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="0c0b0-211">Browse the website</span></span>
![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](iis/_static/browsewebsite.png)
   
> [!WARNING]
> <span data-ttu-id="0c0b0-213">.NET Core 應用程式是透過 IIS 和 Kestrel 伺服器之間的反向 Proxy 裝載。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-213">.NET Core apps are hosted via a reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="0c0b0-214">為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)，這是提供給 IIS 的網站實體路徑。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-214">In order to create the reverse-proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed application, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="0c0b0-215">機密檔案位於應用程式的實體路徑，包括 *my_application.runtimeconfig.json*、*my_application.xml* (XML 文件註解) 和 *my_application.deps.json* 等子資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-215">Sensitive files exist on the app's physical path, including subfolders, such as *my_application.runtimeconfig.json*, *my_application.xml* (XML Documentation comments), and *my_application.deps.json*.</span></span> <span data-ttu-id="0c0b0-216">需要有 *web.config* 檔案才能建立 Kestrel 的反向 Proxy，防止 IIS 提供這些和其他機密檔案。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-216">The *web.config* file is required to create the reverse proxy to Kestrel, which prevents IIS from serving these and other sensitive files.</span></span> <span data-ttu-id="0c0b0-217">**因此，請注意避免意外重新命名或從部署移除 *web.config* 檔案。**</span><span class="sxs-lookup"><span data-stu-id="0c0b0-217">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="data-protection"></a><span data-ttu-id="0c0b0-218">資料保護</span><span class="sxs-lookup"><span data-stu-id="0c0b0-218">Data protection</span></span>

<span data-ttu-id="0c0b0-219">下列情況下，ASP.NET Core 應用程式會將 Keyring 儲存在記憶體中：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-219">An ASP.NET Core application stores the keyring in memory under the following conditions:</span></span>

* <span data-ttu-id="0c0b0-220">網站裝載在 IIS 後端。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-220">A website is hosted behind IIS.</span></span>
* <span data-ttu-id="0c0b0-221">尚未將資料保護堆疊設定為要在持續性存放區中儲存 Keyring。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-221">The Data Protection stack hasn't been configured to store the keyring in a persistent store.</span></span>

<span data-ttu-id="0c0b0-222">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-222">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="0c0b0-223">所有表單驗證權杖都會失效。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-223">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="0c0b0-224">當使用者提出下一個要求時，需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-224">Users are required to login again on their next request.</span></span> 
* <span data-ttu-id="0c0b0-225">使用 Keyring 保護的所有資料都不再受到保護。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-225">Any data you protected with the keyring are no longer protected.</span></span>

> [!WARNING]
> <span data-ttu-id="0c0b0-226">有數個 ASP.NET 中介軟體使用資料保護，包括在驗證中使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="0c0b0-227">即使您不從自己的程式碼呼叫任何資料保護 API，也應該使用部署指令碼或在自己的程式碼中設定資料保護。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-227">Even if you don't call Data Protection APIs from your own code, you should configure Data Protection with a deployment script or in your code.</span></span> <span data-ttu-id="0c0b0-228">如果您不設定資料保護，則系統預設會將金鑰保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-228">If you don't configure data protection, by default the keys are held in memory and discarded when your app restarts.</span></span> <span data-ttu-id="0c0b0-229">重新啟動時，會導致 Cookie 驗證中介軟體寫入的所有 Cookie 失效，因此使用者必須再次登入。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-229">Restarting invalidates cookies written by the cookie authentication middleware and users must login again.</span></span>

<span data-ttu-id="0c0b0-230">若要在 IIS 下設定資料保護，您必須使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-230">To configure Data Protection under IIS, you must use one of the following approaches:</span></span>

* <span data-ttu-id="0c0b0-231">執行 [powershell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)以建立適當的登錄項目 (例如 `.\Provision-AutoGenKeys.ps1 DefaultAppPool`)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-231">Run a [powershell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) to create suitable registry entries (For example, `.\Provision-AutoGenKeys.ps1 DefaultAppPool`).</span></span> <span data-ttu-id="0c0b0-232">這會將金鑰存放在登錄中，並使用 DPAPI 與全電腦金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-232">This stores keys in the registry, protected using DPAPI with a machine-wide key.</span></span>
* <span data-ttu-id="0c0b0-233">設定 IIS 應用程式集區載入使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-233">Configure the IIS Application Pool to load the user profile.</span></span> <span data-ttu-id="0c0b0-234">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-234">This setting is in the **Process Model** section under the **Advanced Settings** for the application pool.</span></span> <span data-ttu-id="0c0b0-235">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-235">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="0c0b0-236">這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-236">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>
* <span data-ttu-id="0c0b0-237">調整應用程式程式碼，以[將檔案系統作為 Keyring 存放區](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-237">Adjust your app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="0c0b0-238">使用 X509 憑證保護 Keyring，確保它是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-238">Use an X509 certificate to protect the keyring and ensure it's a trusted certificate.</span></span> <span data-ttu-id="0c0b0-239">如果它是自我簽署的憑證，就必須放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-239">If it's a self signed certificate, you must place it in the Trusted Root store.</span></span>

<span data-ttu-id="0c0b0-240">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-240">When using IIS in a web farm:</span></span>

* <span data-ttu-id="0c0b0-241">請使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-241">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="0c0b0-242">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-242">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="0c0b0-243">設定[程式碼中的資料保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-243">Configure [data protection in code](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).</span></span>

### <a name="1-create-a-data-protection-registry-hive"></a><span data-ttu-id="0c0b0-244">1.建立資料保護登錄 Hive</span><span class="sxs-lookup"><span data-stu-id="0c0b0-244">1. Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="0c0b0-245">ASP.NET 應用程式使用的資料保護金鑰會儲存在應用程式外部的登錄 Hive 中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-245">Data Protection keys used by ASP.NET applications are stored in registry hives external to the applications.</span></span> <span data-ttu-id="0c0b0-246">若要保存指定應用程式的金鑰，您必須為應用程式的應用程式集區建立登錄 Hive。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-246">To persist the keys for a given application, you must create a registry hive for the app's application pool.</span></span>

<span data-ttu-id="0c0b0-247">若為獨立的 IIS 安裝，您可以針對搭配使用 ASP.NET Core 應用程式的每個應用程式集區，使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-247">For standalone IIS installations, you may use the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="0c0b0-248">此指令碼會在 HKLM 登錄中建立特殊的登錄機碼，其只會將背景工作處理序帳戶列入 ACL。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-248">This script creates a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="0c0b0-249">在待用期間使用 DPAPI 加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-249">Keys are encrypted at rest using DPAPI.</span></span>

<span data-ttu-id="0c0b0-250">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-250">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="0c0b0-251">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-251">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="0c0b0-252">您應該確保這類共用的檔案權限僅限於執行應用程式的 Windows 帳戶身分。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-252">You should ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="0c0b0-253">此外，您可以選擇使用 X509 憑證保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-253">In addition, you may choose to protect keys at rest using an X509 certificate.</span></span> <span data-ttu-id="0c0b0-254">您可能考慮下列讓使用者上傳憑證的機制：將憑證放入使用者的受信任憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用這些憑證。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-254">You may wish to consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="0c0b0-255">詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview#data-protection-configuring)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-255">See [Configuring Data Protection](xref:security/data-protection/configuration/overview#data-protection-configuring) for details.</span></span>

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="0c0b0-256">2.設定 IIS 應用程式集區載入使用者設定檔</span><span class="sxs-lookup"><span data-stu-id="0c0b0-256">2. Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="0c0b0-257">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-257">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="0c0b0-258">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-258">Set Load User Profile to `True`.</span></span> <span data-ttu-id="0c0b0-259">這會將金鑰儲存在使用者設定檔目錄下，並使用 DPAPI 與應用程式集區所用的特定使用者帳戶金鑰加以保護。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-259">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="3-machine-wide-policy-for-data-protection"></a><span data-ttu-id="0c0b0-260">3.資料保護的全電腦原則</span><span class="sxs-lookup"><span data-stu-id="0c0b0-260">3. Machine-wide policy for data protection</span></span>

<span data-ttu-id="0c0b0-261">針對取用資料保護 API 的所有應用程式，資料保護系統僅支援有限的預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy)設定。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-261">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="0c0b0-262">如需詳細資訊，請參閱[資料保護](xref:security/data-protection/index)文件。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-262">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="0c0b0-263">子應用程式的組態</span><span class="sxs-lookup"><span data-stu-id="0c0b0-263">Configuration of sub-applications</span></span>

<span data-ttu-id="0c0b0-264">根應用程式下新增的子應用程式不應包含 ASP.NET Core 模組當做處理常式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-264">Sub applications added under the root application shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="0c0b0-265">如果您在子應用程式的 *web.config* 檔案中，將模組新增為處理常式，則當您嘗試瀏覽子應用程式時，會收到參考錯誤組態檔的 500.19 (內部伺服器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-265">If you add the module as a handler in a sub-application's *web.config* file, you receive a 500.19 (Internal Server Error) referencing the faulty config file when you attempt to browse the sub-app.</span></span> <span data-ttu-id="0c0b0-266">下列範例顯示 ASP.NET Core 子應用程式的已發佈 *web.config* 檔案內容：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-266">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0c0b0-267">如果您打算在 ASP.NET Core 應用程式下方裝載非 ASP.NET Core 子應用程式，您必須明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-267">If you intend to host a non-ASP.NET Core sub-app underneath an ASP.NET Core app, you must explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="0c0b0-268">如需使用 *web.config* 檔案設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-268">For more information on configuring the ASP.NET Core Module with the *web.config* file, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="0c0b0-269">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="0c0b0-269">Configuration of IIS with web.config</span></span>

<span data-ttu-id="0c0b0-270">IIS 組態仍然會受到這些 IIS 功能 *web.config* 的 `<system.webServer>` 區段所影響，這些 IIS 功能適用於反向 Proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-270">IIS configuration is still influenced by the `<system.webServer>` section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="0c0b0-271">例如，您可能在系統層級設定 IIS 使用動態壓縮，但可使用應用程式之 *web.config* 檔案中的 `<urlCompression>` 元素，停用應用程式的該項設定。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-271">For example, you may have IIS configured at the system level to use dynamic compression, but you could disable that setting for an app with the `<urlCompression>` element in the app's *web.config* file.</span></span> <span data-ttu-id="0c0b0-272">如需詳細資訊，請參閱 [`<system.webServer>` 的組態參考](https://docs.microsoft.com/iis/configuration/system.webServer/)、[ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)以及[使用 IIS 模組與 ASP.NET Core](xref:hosting/iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-272">For more information, see the [configuration reference for `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:hosting/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:hosting/iis-modules).</span></span> <span data-ttu-id="0c0b0-273">如果您需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-273">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="0c0b0-274">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="0c0b0-274">Configuration sections of web.config</span></span>

<span data-ttu-id="0c0b0-275">有別於使用 *web.config* 中的 `<system.web>`、`<appSettings>`、`<connectionStrings>` 和 `<location>` 元素所設定的 .NET Framework 應用程式，ASP.NET Core 應用程式設定使用其他組態提供者。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-275">Unlike .NET Framework applications that are configured with the `<system.web>`, `<appSettings>`, `<connectionStrings>`, and `<location>` elements in *web.config*, ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="0c0b0-276">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-276">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="application-pools"></a><span data-ttu-id="0c0b0-277">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="0c0b0-277">Application Pools</span></span>

<span data-ttu-id="0c0b0-278">在單一系統上裝載多個網站時，您應該在其各自的應用程式集區中執行每個應用程式，以將應用程式互相隔離。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-278">When hosting multiple websites on a single system, you should isolate the apps from each other by running each app in its own application pool.</span></span> <span data-ttu-id="0c0b0-279">IIS [新增網站] 對話方塊預設執行此行為。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-279">The IIS **Add Website** dialog defaults to this behavior.</span></span> <span data-ttu-id="0c0b0-280">當您提供**站台名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-280">When you provide a **Site name**, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="0c0b0-281">當您新增網站時，即會使用該網站名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-281">A new application pool is created using the site name when you add the website.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="0c0b0-282">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="0c0b0-282">Application Pool Identity</span></span>

<span data-ttu-id="0c0b0-283">應用程式集區身分識別帳戶可讓您在唯一的帳戶下執行應用程式，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-283">An application pool identity account allows you to run an application under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="0c0b0-284">在 IIS 8.0+ 中，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，並預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-284">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new application pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="0c0b0-285">在 IIS 管理主控台中，於應用程式集區的 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-285">In the IIS Management Console under **Advanced Settings** for your app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity** as shown in the image below.</span></span>

![應用程式集區進階設定對話方塊](iis/_static/apppool-identity.png)

<span data-ttu-id="0c0b0-287">IIS 管理程序會在 Windows 安全系統中，以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-287">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="0c0b0-288">使用這個身分識別時，即可保護資源；不過，此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-288">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="0c0b0-289">如果您要將提升的 IIS 背景工作處理序存取權授與應用程式，就必須修改包含應用程式的目錄存取控制清單 (ACL)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-289">If you need to grant the IIS worker process elevated access to your app, you must modify the Access Control List (ACL) for the directory containing your application.</span></span>

1. <span data-ttu-id="0c0b0-290">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-290">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="0c0b0-291">以滑鼠右鍵按一下目錄，然後按一下 [內容]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-291">Right click on the directory and click **Properties**.</span></span>

3. <span data-ttu-id="0c0b0-292">依序按一下 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-292">Under the **Security** tab, click the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="0c0b0-293">按一下 [位置] 按鈕，並確定選取您的系統。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-293">Click the **Locations** button and make sure you select your system.</span></span>

5. <span data-ttu-id="0c0b0-294">在 [輸入要選取的物件名稱] 文字方塊中輸入 **IIS AppPool\DefaultAppPool**。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-294">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="0c0b0-296">按一下 [檢查名稱] 按鈕，再按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-296">Click the **Check Names** button and then click **OK**.</span></span>

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-2.png)

<span data-ttu-id="0c0b0-298">您也可以透過命令提示字元使用 **ICACLS** 工具執行此作業：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-298">You can also do this via a command prompt using **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a><span data-ttu-id="0c0b0-299">疑難排解提示</span><span class="sxs-lookup"><span data-stu-id="0c0b0-299">Troubleshooting tips</span></span>

<span data-ttu-id="0c0b0-300">若要診斷 IIS 部署問題：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-300">To diagnose problems with IIS deployments:</span></span>

* <span data-ttu-id="0c0b0-301">研究瀏覽器輸出。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-301">Study browser output.</span></span>
* <span data-ttu-id="0c0b0-302">透過**事件檢視器**，檢查系統的**應用程式**記錄檔。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-302">Examine the system's **Application** log through **Event Viewer**.</span></span>
* <span data-ttu-id="0c0b0-303">啟用 `stdout` 記錄。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-303">Enable `stdout` logging.</span></span> <span data-ttu-id="0c0b0-304">您可以在 *web.config* 中 `<aspNetCore>` 項目的 *stdoutLogFile* 屬性所提供的路徑上，找到 **ASP.NET Core 模組**記錄檔。部署中必須有屬性值提供之路徑上的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-304">The **ASP.NET Core Module** log is found on the path provided in the *stdoutLogFile* attribute of the `<aspNetCore>` element in *web.config*. Any folders on the path provided in the attribute value must exist in the deployment.</span></span> <span data-ttu-id="0c0b0-305">您也必須設定 *stdoutLogEnabled ="true"*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-305">You must also set *stdoutLogEnabled="true"*.</span></span> <span data-ttu-id="0c0b0-306">如果應用程式是使用 `Microsoft.NET.Sdk.Web` SDK 來建立 *web.config* 檔案，其會將 *stdoutLogEnabled* 設定預設為 *false*，因此您必須手動提供 *web.config* 檔案或修改檔案，以啟用 `stdout` 記錄。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-306">Apps that use the the `Microsoft.NET.Sdk.Web` SDK to create the *web.config* file default the *stdoutLogEnabled* setting to *false*, so you must manually provide the *web.config* file or modify the file in order to enable `stdout` logging.</span></span>

<span data-ttu-id="0c0b0-307">若干常見錯誤要等到模組的 *startupTimeLimit* (預設值：120 秒) 和 *startupRetryCount* (預設值：2) 經過後，才會出現在瀏覽器、應用程式記錄檔和 ASP.NET Core 模組記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-307">Several of the common errors don't appear in the browser, Application Log, and ASP.NET Core Module Log until the module *startupTimeLimit* (default: 120 seconds) and *startupRetryCount* (default: 2) have passed.</span></span> <span data-ttu-id="0c0b0-308">因此，要等候整整六分鐘，才能推算出模組無法啟動應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-308">Therefore, wait a full six minutes before deducing that the module has failed to start a process for the app.</span></span>

<span data-ttu-id="0c0b0-309">若要判斷應用程式是否正常運作，有一個快速的方法是直接在 Kestrel 上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-309">One quick way to determine if the app is working properly is to run the app directly on Kestrel.</span></span> <span data-ttu-id="0c0b0-310">如果您已將應用程式發佈為與 Framework 相依的部署 (FDD)，請執行部署資料夾中的 `dotnet my_application.dll`，這是應用程式的 IIS 實體路徑。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-310">If the app was published as a framework-dependent deployment (FDD), execute `dotnet my_application.dll` in the deployment folder, which is the IIS physical path to the app.</span></span> <span data-ttu-id="0c0b0-311">如果您已將應用程式發佈為獨立部署 (SCD)，請直接從命令提示字元執行部署資料夾中的應用程式可執行檔 `my_application.exe`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-311">If the app was published as a self-contained deployment (SCD), run the app's executable directly from a command prompt, `my_application.exe`, in the deployment folder.</span></span> <span data-ttu-id="0c0b0-312">如果 Kestrel 是接聽預設通訊埠 5000，您應該能夠在 `http://localhost:5000/` 瀏覽應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-312">If Kestrel is listening on default port 5000, you should be able to browse the app at `http://localhost:5000/`.</span></span> <span data-ttu-id="0c0b0-313">如果應用程式如常在 Kestrel 端點位址回應，則 IIS-ASP.NET Core 模組-Kestrel 組態出問題的機率比較大，應用程式本身出問題的機率比較小。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-313">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the IIS-ASP.NET Core Module-Kestrel configuration and less likely within the app itself.</span></span>

<span data-ttu-id="0c0b0-314">若要判斷 Kestrel 伺服器的 IIS 反向 Proxy 是否正常運作，其中一種方式是使用[靜態檔案中介軟體](xref:fundamentals/static-files)，針對樣式表、指令碼或來自 *wwwroot* 應用程式靜態檔案的影像，執行簡單的靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-314">One way to determine if the IIS reverse proxy to the Kestrel server is working properly is to perform a simple static file request for a stylesheet, script, or image from the app's static files in *wwwroot* using [Static File middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="0c0b0-315">如果應用程式可以提供靜態檔案，但 MVC 檢視和其他端點卻失敗，就不太可能是 IIS-ASP.NET Core 模組-Kestrel 組態出問題，更可能是應用程式本身出問題 (例如，MVC 路由或 500 內部伺服器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-315">If the app can serve static files but MVC Views and other endpoints are failing, the problem is less likely related to the IIS-ASP.NET Core Module-Kestrel configuration and more likely within the app itself (for example, MVC routing or 500 Internal Server Error).</span></span>

<span data-ttu-id="0c0b0-316">當 Kestrel 在 IIS 後端正常啟動，但應用程式在本機上成功執行之後卻不能在系統上執行時，您可以暫時將環境變數新增至 *web.config*，將 `ASPNETCORE_ENVIRONMENT` 設成 `Development`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-316">When Kestrel starts normally behind IIS but the app won't run on the system after successfully running locally, you can temporarily add an environment variable to *web.config* to set the `ASPNETCORE_ENVIRONMENT` to `Development`.</span></span> <span data-ttu-id="0c0b0-317">只要您不覆寫應用程式啟動的環境，上述設定即可讓應用程式在系統上執行時出現[開發人員例外狀況頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-317">As long as you don't override the environment in app startup, this allows the [developer exception page](xref:fundamentals/error-handling) to appear when the app is run on the system.</span></span> <span data-ttu-id="0c0b0-318">不過，只有不公開到網際網路的臨時/測試系統，才建議以這種方式設定 `ASPNETCORE_ENVIRONMENT` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-318">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` in this way is only recommended for staging/testing systems that aren't exposed to the Internet.</span></span> <span data-ttu-id="0c0b0-319">完成後，請務必從 *web.config* 檔案移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-319">Be sure you remove the environment variable from the *web.config* file when finished.</span></span> <span data-ttu-id="0c0b0-320">如需透過反向 Proxy 的 *web.config* 設定環境變數的相關資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:hosting/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-320">For information on setting environment variables via *web.config* for the reverse proxy, see [environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).</span></span>

<span data-ttu-id="0c0b0-321">在大部分情況下，啟用應用程式記錄有助於針對應用程式或反向 Proxy 的問題進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-321">In most cases, enabling application logging assists in troubleshooting problems with the app or the reverse proxy.</span></span> <span data-ttu-id="0c0b0-322">如需詳細資訊，請參閱[記錄](xref:fundamentals/logging)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-322">See [Logging](xref:fundamentals/logging) for more information.</span></span>

<span data-ttu-id="0c0b0-323">升級開發電腦的 .NET Core SDK 或應用程式內的套件版本後，最後有關應用程式的疑難排解提示無法執行。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-323">Our last troubleshooting tip pertains to apps that fail to run after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="0c0b0-324">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-324">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="0c0b0-325">這些問題大部分是可以修正的，方法是刪除專案中的 `bin` 和 `obj` 資料夾、清除 `%UserProfile%\.nuget\packages\` 和 `%LocalAppData%\Nuget\v3-cache` 的套件快取、還原專案，然後確認已完全刪除之前在系統上的部署，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-325">You can fix most of these issues by deleting the `bin` and `obj` folders in the project, clearing package caches at `%UserProfile%\.nuget\packages\` and `%LocalAppData%\Nuget\v3-cache`, restoring the project, and confirming that your prior deployment on the system has been completely deleted prior to re-deploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="0c0b0-326">若要清除套件快取，有一個便利的方式是從 [NuGet.org](https://www.nuget.org/) 取得 *NuGet.exe* 工具，將它新增至系統路徑，再從命令提示字元執行 `nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-326">A convenient way to clear package caches is to obtain the *NuGet.exe* tool from [NuGet.org](https://www.nuget.org/), add it to your system PATH, and execute `nuget locals all -clear` from a command prompt.</span></span> <span data-ttu-id="0c0b0-327">您也可以從命令提示字元執行 `dotnet nuget locals all --clear` 命令，即不需取得 *NuGet.exe*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-327">You can also execute the `dotnet nuget locals all --clear` command from a command prompt without obtaining *NuGet.exe*.</span></span>

## <a name="common-errors"></a><span data-ttu-id="0c0b0-328">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="0c0b0-328">Common errors</span></span>

<span data-ttu-id="0c0b0-329">以下的錯誤清單並非完整清單。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-329">The following isn't a complete list of errors.</span></span> <span data-ttu-id="0c0b0-330">假如遇到此處未列出的錯誤，請在下方的 [註解] 區段中留下詳細的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-330">Should you encounter an error not listed here, please leave a detailed error message in the comments section below.</span></span>

### <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="0c0b0-331">安裝程式無法取得 VC++ 可轉散發套件</span><span class="sxs-lookup"><span data-stu-id="0c0b0-331">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="0c0b0-332">**安裝程式的例外狀況：**0x80072efd 或 0x80072f76 - 未指定的錯誤</span><span class="sxs-lookup"><span data-stu-id="0c0b0-332">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="0c0b0-333">**安裝程式記錄例外狀況&#8224;：**錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件</span><span class="sxs-lookup"><span data-stu-id="0c0b0-333">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="0c0b0-334">&#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-334">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="0c0b0-335">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-335">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-336">如果安裝伺服器裝載套件組合時，系統無法存取網際網路，導致安裝程式無法取得 *Microsoft Visual C++ 2015 可轉散發套件*，就會發生這個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-336">If the system doesn't have Internet access while installing the server hosting bundle, this exception occurs when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="0c0b0-337">您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-337">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="0c0b0-338">如果安裝程式失敗，您可能無法收到必要的 .NET Core 執行階段來裝載與 Framework 相依的部署 (FDD)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-338">If the installer fails, you may not receive the .NET Core runtime required to host a framework-dependent deployment (FDD).</span></span> <span data-ttu-id="0c0b0-339">如果您打算裝載 FDD，請確認已在 [程式和功能] 中安裝執行階段。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-339">If you plan to host an FDD, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="0c0b0-340">您可從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-340">You may obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="0c0b0-341">安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-341">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="0c0b0-342">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="0c0b0-342">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="0c0b0-343">**應用程式記錄檔：**無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-343">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="0c0b0-344">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-344">The data is the error.</span></span>

<span data-ttu-id="0c0b0-345">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-345">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-346">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-346">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="0c0b0-347">如果先安裝 ASP.NET Core 模組再升級作業系統，然後嘗試在作業系統升級之後於 32 位元模式中執行任何 AppPool，就會發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-347">If you have the ASP.NET Core Module installed prior to an OS upgrade and then try to run any AppPool in 32-bit mode after an OS upgrade, you encounter this issue.</span></span> <span data-ttu-id="0c0b0-348">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-348">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="0c0b0-349">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-349">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="0c0b0-350">執行安裝程式時，請選取 [修復]。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-350">Select **Repair** when you run the installer.</span></span>

### <a name="platform-conflicts-with-rid"></a><span data-ttu-id="0c0b0-351">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="0c0b0-351">Platform conflicts with RID</span></span>

* <span data-ttu-id="0c0b0-352">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="0c0b0-352">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="0c0b0-353">**應用程式記錄檔：**實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法使用命令行 '"C:\\{PATH}\my_application.{exe|dll}" ' 啟動程序，ErrorCode = '0x80004005 : ff。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-353">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="0c0b0-354">**ASP.NET Core 模組記錄檔：**未處理的例外狀況：System.BadImageFormatException：無法載入檔案或組件 'my_application.dll'。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-354">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'.</span></span> <span data-ttu-id="0c0b0-355">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-355">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="0c0b0-356">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-356">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-357">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-357">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="0c0b0-358">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-358">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="0c0b0-359">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-359">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="0c0b0-360">確認您並未在 *.csproj* 中設定與 RID 發生衝突的 `<PlatformTarget>`。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-360">Confirm that you didn't set a `<PlatformTarget>` in your *.csproj* that conflicts with the RID.</span></span> <span data-ttu-id="0c0b0-361">例如，藉由使用 *dotnet publish -c Release -r win10-x64*，或將 *.csproj* 的 `<RuntimeIdentifiers>` 設定為 `win10-x64`，不指定 `x86` 的 `<PlatformTarget>` 且以 `win10-x64` 的 RID 發佈。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-361">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in your *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="0c0b0-362">專案發佈時沒有警告或錯誤，但因為上述在系統上記錄的例外狀況而失敗。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-362">The project publishes without warning or error but fails with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="0c0b0-363">如果在升級應用程式和部署新的組件時， Azure 應用程式部署發生這個例外狀況，請手動刪除先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-363">If this exception occurs for an Azure Apps deployment when upgrading an application and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="0c0b0-364">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-364">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

### <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="0c0b0-365">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="0c0b0-365">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="0c0b0-366">**瀏覽器：**ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="0c0b0-366">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="0c0b0-367">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-367">**Application Log:** No entry</span></span>

* <span data-ttu-id="0c0b0-368">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="0c0b0-368">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="0c0b0-369">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-369">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-370">確認您為應用程式使用的是正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-370">Confirm you are using the correct URI endpoint for the application.</span></span> <span data-ttu-id="0c0b0-371">請檢查您的繫結。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-371">Check your bindings.</span></span>

* <span data-ttu-id="0c0b0-372">確認 IIS 網站不是「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-372">Confirm that the IIS website is not in the *Stopped* state.</span></span>

### <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="0c0b0-373">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="0c0b0-373">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="0c0b0-374">**作業系統例外狀況：**必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-374">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="0c0b0-375">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-375">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-376">確認您已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-376">Confirm that you have enabled the proper role and features.</span></span> <span data-ttu-id="0c0b0-377">請參閱 [IIS 組態](#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-377">See [IIS Configuration](#iis-configuration).</span></span>

### <a name="incorrect-website-physical-path-or-application-missing"></a><span data-ttu-id="0c0b0-378">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="0c0b0-378">Incorrect website physical path or application missing</span></span>

* <span data-ttu-id="0c0b0-379">**瀏覽器：** 403 禁止 - 存取被拒**--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-379">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="0c0b0-380">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-380">**Application Log:** No entry</span></span>

* <span data-ttu-id="0c0b0-381">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="0c0b0-381">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="0c0b0-382">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-382">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-383">請檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-383">Check the IIS website **Basic Settings** and the physical application folder.</span></span> <span data-ttu-id="0c0b0-384">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-384">Confirm that the application is in the folder at the IIS website **Physical path**.</span></span>

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="0c0b0-385">角色不正確、模組未安裝，或權限不正確。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-385">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="0c0b0-386">**瀏覽器：**500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-386">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="0c0b0-387">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-387">**Application Log:** No entry</span></span>

* <span data-ttu-id="0c0b0-388">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="0c0b0-388">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="0c0b0-389">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-389">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-390">確認您已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-390">Confirm that you've enabled the proper role.</span></span> <span data-ttu-id="0c0b0-391">請參閱 [IIS 組態](#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-391">See [IIS Configuration](#iis-configuration).</span></span>

* <span data-ttu-id="0c0b0-392">請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-392">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="0c0b0-393">如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-393">If the **Microsoft ASP.NET Core Module** isn't present in the list of installed programs, install the module.</span></span> <span data-ttu-id="0c0b0-394">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-394">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="0c0b0-395">請確定 [應用程式集區] > [處理序模型] > [身分識別] 設為 **ApplicationPoolIdentity**，或您的自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-395">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or your custom identity has the correct permissions to access the app's deployment folder.</span></span>

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="0c0b0-396">不正確的 processPath, 遺失 PATH 變數, 未安裝裝載套件組合, 未重新啟動系統/IIS, 未安裝 VC++ 可轉散發套件, 或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="0c0b0-396">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="0c0b0-397">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="0c0b0-397">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="0c0b0-398">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '".\my_application.exe" ' 啟動程序，ErrorCode = '0x80070002 : 0。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-398">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="0c0b0-399">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-399">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="0c0b0-400">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-400">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-401">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-401">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="0c0b0-402">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-402">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="0c0b0-403">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-403">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="0c0b0-404">請檢查 *web.config* 中 `<aspNetCore>` 項目的 *processPath* 屬性，以確認它是 *dotnet* (若是與 Framework 相依的部署 (FDD)) 或 *.\my_application.exe* (若為獨立部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-404">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's *dotnet* for a framework-dependent deployment (FDD) or *.\my_application.exe* for a self-contained deployment (SCD).</span></span>

* <span data-ttu-id="0c0b0-405">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-405">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="0c0b0-406">確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-406">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="0c0b0-407">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-407">For an FDD, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="0c0b0-408">確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-408">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="0c0b0-409">確認 *C:\Program Files\dotnet* 和應用程式目錄上的 AppPool 使用者身分識別未設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-409">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and application directories.</span></span>

* <span data-ttu-id="0c0b0-410">您可能已部署 FDD，並在未重新啟動 IIS 的狀況下安裝了 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-410">You may have deployed an FDD and installed .NET Core without restarting IIS.</span></span> <span data-ttu-id="0c0b0-411">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-411">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="0c0b0-412">您可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-412">You may have deployed an FDD without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="0c0b0-413">如果您嘗試部署 FDD，但尚未安裝 .NET Core 執行階段，請在系統上執行 **.NET Core Windows Server 裝載套件組合安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-413">If you're attempting to deploy a FDD and haven't installed the .NET Core runtime, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="0c0b0-414">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-414">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="0c0b0-415">如果您嘗試在沒有網際網路連線的系統上安裝 .NET Core 執行階段，請從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段，並執行裝載套件組合安裝程式以安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-415">If you're attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="0c0b0-416">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-416">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="0c0b0-417">您可能已部署 FDD，並在未重新啟動系統/IIS 的狀況下安裝了 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-417">You may have deployed an FDD and installed .NET Core without restarting the system/IIS.</span></span> <span data-ttu-id="0c0b0-418">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-418">Either restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="0c0b0-419">您可能已部署 FDD，但系統上未安裝 *Microsoft Visual C++ 2015 可轉散發套件 (x64)*。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-419">You may have deployed an FDD and the *Microsoft Visual C++ 2015 Redistributable (x64)* is not installed on the system.</span></span> <span data-ttu-id="0c0b0-420">您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-420">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

### <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="0c0b0-421">不正確的 \<aspNetCore\> 元素引數</span><span class="sxs-lookup"><span data-stu-id="0c0b0-421">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="0c0b0-422">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="0c0b0-422">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="0c0b0-423">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-423">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="0c0b0-424">**ASP.NET Core 模組記錄檔：**要執行的應用程式不存在：'PATH\my_application.dll'</span><span class="sxs-lookup"><span data-stu-id="0c0b0-424">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\my_application.dll'</span></span>

<span data-ttu-id="0c0b0-425">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-425">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-426">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-426">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="0c0b0-427">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-427">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="0c0b0-428">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-428">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="0c0b0-429">檢查 *web.config* 中 `<aspNetCore>` 項目的 *arguments* 屬性，以確認它是 (a) 與 Framework 相依部署 (FDD) 的 *.\my_application.dll*；或 (b) 不存在的空字串 (*arguments=""*)，或獨立部署 (SCD) 的應用程式引數清單 (*arguments="arg1, arg2, ..."*)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-429">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is either (a) *.\my_application.dll* for a framework-dependent deployment (FDD); or (b) not present, an empty string (*arguments=""*), or a list of your app's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment (SCD).</span></span>

### <a name="missing-net-framework-version"></a><span data-ttu-id="0c0b0-430">遺失 .NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="0c0b0-430">Missing .NET Framework version</span></span>

* <span data-ttu-id="0c0b0-431">**瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-431">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="0c0b0-432">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-432">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="0c0b0-433">**ASP.NET Core 模組記錄檔：**遺失方法、檔案或組件例外狀況。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-433">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="0c0b0-434">例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-434">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="0c0b0-435">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="0c0b0-435">Troubleshooting:</span></span>

* <span data-ttu-id="0c0b0-436">安裝系統中遺失的 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-436">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="0c0b0-437">若為 FDD，請確認已在系統上正確安裝執行階段。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-437">For a framework-dependent deployment (FDD), confirm that you have the correct runtime installed on the system.</span></span> <span data-ttu-id="0c0b0-438">如果您將專案從 1.1 升級到 2.0，再部署到主控系統，然後收到這個例外狀況，請確定主控系統上安裝了 2.0 架構。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-438">If you upgrade a project from 1.1 to 2.0, deploy to the hosting system, and receive this exception, ensure you install the 2.0 framework on the hosting system.</span></span>

### <a name="stopped-application-pool"></a><span data-ttu-id="0c0b0-439">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="0c0b0-439">Stopped Application Pool</span></span>

* <span data-ttu-id="0c0b0-440">**瀏覽器：**503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="0c0b0-440">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="0c0b0-441">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-441">**Application Log:** No entry</span></span>

* <span data-ttu-id="0c0b0-442">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="0c0b0-442">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="0c0b0-443">疑難排解</span><span class="sxs-lookup"><span data-stu-id="0c0b0-443">Troubleshooting</span></span>

* <span data-ttu-id="0c0b0-444">確認應用程式集區不是「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-444">Confirm that the Application Pool is not in the *Stopped* state.</span></span>

### <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="0c0b0-445">未實作 IIS Integration 中介軟體</span><span class="sxs-lookup"><span data-stu-id="0c0b0-445">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="0c0b0-446">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="0c0b0-446">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="0c0b0-447">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-447">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="0c0b0-448">**ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-448">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="0c0b0-449">疑難排解</span><span class="sxs-lookup"><span data-stu-id="0c0b0-449">Troubleshooting</span></span>

* <span data-ttu-id="0c0b0-450">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-450">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="0c0b0-451">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-451">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="0c0b0-452">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-452">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="0c0b0-453">在應用程式的 *WebHostBuilder()* (ASP.NET Core 1.x) 上呼叫 *.UseIISIntegration()* 方法，確認您已正確參考 IIS 整合中介軟體，或已正確使用 `CreateDefaultBuilder` 方法 (ASP.NET Core 2.x)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-453">Confirm that you've correctly referenced the IIS Integration middleware by calling the *.UseIISIntegration()* method on the application's *WebHostBuilder()* (ASP.NET Core 1.x) or used the `CreateDefaultBuilder` method (ASP.NET Core 2.x).</span></span> <span data-ttu-id="0c0b0-454">如需詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-454">See [Hosting in ASP.NET Core](xref:fundamentals/hosting) for details.</span></span>

### <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="0c0b0-455">子應用程式包含\<處理常式\>區段</span><span class="sxs-lookup"><span data-stu-id="0c0b0-455">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="0c0b0-456">**瀏覽器：**HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="0c0b0-456">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="0c0b0-457">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="0c0b0-457">**Application Log:** No entry</span></span>

* <span data-ttu-id="0c0b0-458">**ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示根應用程式的一般作業。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-458">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root application.</span></span> <span data-ttu-id="0c0b0-459">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-459">Log file not created for the sub-application.</span></span>

<span data-ttu-id="0c0b0-460">疑難排解</span><span class="sxs-lookup"><span data-stu-id="0c0b0-460">Troubleshooting</span></span>

* <span data-ttu-id="0c0b0-461">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-461">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

### <a name="application-configuration-general-issue"></a><span data-ttu-id="0c0b0-462">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="0c0b0-462">Application configuration general issue</span></span>

* <span data-ttu-id="0c0b0-463">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="0c0b0-463">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="0c0b0-464">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-464">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="0c0b0-465">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-465">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="0c0b0-466">疑難排解</span><span class="sxs-lookup"><span data-stu-id="0c0b0-466">Troubleshooting</span></span>

* <span data-ttu-id="0c0b0-467">這個一般的例外狀況指出程序無法啟動，最可能的原因是應用程式組態問題。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-467">This general exception indicates that the process failed to start, most likely due to an application configuration issue.</span></span> <span data-ttu-id="0c0b0-468">參考[目錄結構](xref:hosting/directory-structure)，確認應用程式部署的檔案和資料夾都是合適的，且應用程式組態檔都存在，並包含應用程式和環境的正確設定。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-468">Referring to [Directory Structure](xref:hosting/directory-structure), confirm that your app's deployed files and folders are appropriate and that your app's configuration files are present and contain the correct settings for your app and environment.</span></span> <span data-ttu-id="0c0b0-469">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="0c0b0-469">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

## <a name="resources"></a><span data-ttu-id="0c0b0-470">資源</span><span class="sxs-lookup"><span data-stu-id="0c0b0-470">Resources</span></span>

* [<span data-ttu-id="0c0b0-471">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="0c0b0-471">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="0c0b0-472">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="0c0b0-472">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)
* [<span data-ttu-id="0c0b0-473">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c0b0-473">Using IIS Modules with ASP.NET Core</span></span>](xref:hosting/iis-modules)
* [<span data-ttu-id="0c0b0-474">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="0c0b0-474">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="0c0b0-475">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="0c0b0-475">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="0c0b0-476">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="0c0b0-476">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
