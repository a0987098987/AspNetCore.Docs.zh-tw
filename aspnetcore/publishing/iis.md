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
ms.openlocfilehash: 8ffadc1dede4053faa129a3b224aace901e70e14
ms.sourcegitcommit: ad01283f299d346cf757c4f4744c48634dc27e73
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-windows-with-iis-and-deploy-to-it"></a><span data-ttu-id="ef0c1-104">在 Windows 上使用 IIS 設定並部署到 ASP.NET Core 裝載環境</span><span class="sxs-lookup"><span data-stu-id="ef0c1-104">Set up a hosting environment for ASP.NET Core on Windows with IIS, and deploy to it</span></span>

<span data-ttu-id="ef0c1-105">作者：[Luke Latham](https://github.com/GuardRex) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ef0c1-105">By [Luke Latham](https://github.com/GuardRex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="ef0c1-106">支援的作業系統</span><span class="sxs-lookup"><span data-stu-id="ef0c1-106">Supported operating systems</span></span>

<span data-ttu-id="ef0c1-107">支援下列作業系統：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="ef0c1-108">Windows 7 和更新版本</span><span class="sxs-lookup"><span data-stu-id="ef0c1-108">Windows 7 and newer</span></span>

* <span data-ttu-id="ef0c1-109">Windows Server 2008 R2 和更新版本&#8224;</span><span class="sxs-lookup"><span data-stu-id="ef0c1-109">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="ef0c1-110">&#8224;就概念而言，本文件中描述的 IIS 組態也適用於在 Nano Server IIS 上裝載 ASP.NET Core 應用程式，但特定指示需參考 [Nano Server 上的 IIS 與 ASP.NET Core](xref:tutorials/nano-server)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-110">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core applications on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="ef0c1-111">[WebListener 伺服器](xref:fundamentals/servers/weblistener)在使用 IIS 的反向 Proxy 組態中無法運作。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-111">[WebListener server](xref:fundamentals/servers/weblistener) will not work in a reverse-proxy configuration with IIS.</span></span> <span data-ttu-id="ef0c1-112">您必須使用 [Kestrel 伺服器](xref:fundamentals/servers/kestrel)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-112">You must use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="ef0c1-113">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ef0c1-113">IIS configuration</span></span>

<span data-ttu-id="ef0c1-114">啟用**網頁伺服器 (IIS)**角色，並建立角色服務。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-114">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="ef0c1-115">Windows 桌面作業系統</span><span class="sxs-lookup"><span data-stu-id="ef0c1-115">Windows desktop operating systems</span></span>

<span data-ttu-id="ef0c1-116">巡覽至 [控制台] > [程式] > [程式和功能] > [開啟或關閉 Windows 功能] (螢幕左側)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-116">Navigate to **Control Panel > Programs > Programs and Features > Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="ef0c1-117">開啟 [Internet Information Services] 和 [Web 管理工具] 群組。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-117">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="ef0c1-118">核取 [IIS 管理主控台] 方塊。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-118">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="ef0c1-119">[World Wide Web Services] (全球資訊網服務) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-119">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="ef0c1-120">接受**全球資訊網服務**的預設功能，或自訂符合需求的 IIS 功能。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-120">Accept the default features for **World Wide Web Services** or customize the IIS features to suit your needs.</span></span>

![選取 [Windows 功能] 中的 [IIS 管理主控台] 和 [World Wide Web Services] (全球資訊網服務)。](iis/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="ef0c1-122">Windows Server 作業系統</span><span class="sxs-lookup"><span data-stu-id="ef0c1-122">Windows Server operating systems</span></span>

<span data-ttu-id="ef0c1-123">伺服器作業系統，請透過 [伺服器管理員] 中的 [管理] 功能表或連結，使用 [新增角色及功能精靈]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-123">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="ef0c1-124">在**伺服器角色**步驟中，核取 [網頁伺服器 (IIS)] 方塊。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-124">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![在選取伺服器角色步驟中選取網頁伺服器 IIS 角色。](iis/_static/server-roles-ws2016.png)

<span data-ttu-id="ef0c1-126">在**角色服務**步驟中，選取您想要的 IIS 角色服務或接受提供的預設角色服務。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-126">On the **Role services** step, select the IIS role services you desire or accept the default role services provided.</span></span>

![在選取角色服務步驟中，選取預設的角色服務。](iis/_static/role-services-ws2016.png)

<span data-ttu-id="ef0c1-128">透過**確認**步驟繼續作業，安裝網頁伺服器角色和服務。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-128">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="ef0c1-129">安裝網頁伺服器 (IIS) 角色之後，不需要重新啟動伺服器/IIS。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-129">A server/IIS restart is not required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="ef0c1-130">安裝 .NET Core Windows Server 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="ef0c1-130">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="ef0c1-131">在主控系統上安裝 [.NET Core Windows Server 裝載套件組合](https://aka.ms/dotnetcore.2.0.0-windowshosting)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-131">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore.2.0.0-windowshosting) on the hosting system.</span></span> <span data-ttu-id="ef0c1-132">套件組合會安裝 .NET Core 執行階段、.NET Core 程式庫和 [ASP.NET Core 模組](xref:fundamentals/servers/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-132">The bundle will install the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="ef0c1-133">此模組會在 IIS 和 Kestrel 伺服器之間建立反向 Proxy。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-133">The module creates the reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="ef0c1-134">注意：如果系統沒有網際網路連線，請先取得並安裝 *[Microsoft Visual C++ 2015 可轉散發套件](https://www.microsoft.com/download/details.aspx?id=53840)*，再安裝 .NET Core Windows Server 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-134">Note: If the system doesn't have an Internet connection, obtain and install the *[Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840)* before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="ef0c1-135">重新啟動系統或從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，挑選系統 PATH 的變更。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-135">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="ef0c1-136">如果使用 IIS 共用組態，請參閱[使用 IIS 共用組態的 ASP.NET Core 模組](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-136">If you use an IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:hosting/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="ef0c1-137">使用 Visual Studio 發佈時安裝 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ef0c1-137">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="ef0c1-138">如果您想要在 Visual Studio 中使用 Web Deploy 部署應用程式，請在主控系統上安裝最新版的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-138">If you intend to deploy your applications with Web Deploy in Visual Studio, install the latest version of Web Deploy on the hosting system.</span></span> <span data-ttu-id="ef0c1-139">若要安裝 Web Deploy，您可以使用 [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) 或從[Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=43717)直接取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-139">To install Web Deploy, you can use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="ef0c1-140">慣用的方法是使用 WebPI。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-140">The preferred method is to use WebPI.</span></span> <span data-ttu-id="ef0c1-141">WebPI 提供獨立的安裝程式和組態以裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-141">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ef0c1-142">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="ef0c1-142">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="ef0c1-143">啟用 IISIntegration 元件</span><span class="sxs-lookup"><span data-stu-id="ef0c1-143">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef0c1-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef0c1-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef0c1-145">一般 *Program.cs* 會呼叫 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) 以開始設定主機。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-145">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="ef0c1-146">`CreateDefaultBuilder` 會將 [Kestrel](xref:fundamentals/servers/kestrel) 設為網頁伺服器，並設定 [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) 的基底路徑與連接埠來啟用 IIS 整合：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-146">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef0c1-147">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef0c1-147">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef0c1-148">在應用程式相依性的 [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) 套件中包含相依性。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-148">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the application dependencies.</span></span> <span data-ttu-id="ef0c1-149">在 *WebHostBuilder* 新增 *UseIISIntegration* 擴充方法，將 IIS 整合中介程式併入應用程式中：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-149">Incorporate IIS Integration middleware into the application by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="ef0c1-150">`UseKestrel` 與 `UseIISIntegration` 都是必要項目。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-150">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="ef0c1-151">呼叫 *UseIISIntegration* 的程式碼並不會影響程式碼的可攜性。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-151">Code calling *UseIISIntegration* doesn't affect code portability.</span></span> <span data-ttu-id="ef0c1-152">若應用程式不在 IIS 背後執行 (例如，應用程式直接在 Kestrel 上執行)，`UseIISIntegration` 就不會生效。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-152">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="ef0c1-153">如需裝載的詳細資訊，請參閱[在 ASP.NET Core 中裝載](xref:fundamentals/hosting)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-153">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="ef0c1-154">IIS 選項</span><span class="sxs-lookup"><span data-stu-id="ef0c1-154">IIS options</span></span>

<span data-ttu-id="ef0c1-155">若要設定 *IISIntegration* 服務選項，*ConfigureServices* 中請包括 *IISOptions* 的服務組態：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-155">To configure *IISIntegration* service options, include a service configuration for *IISOptions* in *ConfigureServices*:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="ef0c1-156">選項</span><span class="sxs-lookup"><span data-stu-id="ef0c1-156">Option</span></span>                         | <span data-ttu-id="ef0c1-157">預設</span><span class="sxs-lookup"><span data-stu-id="ef0c1-157">Default</span></span> | <span data-ttu-id="ef0c1-158">設定</span><span class="sxs-lookup"><span data-stu-id="ef0c1-158">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="ef0c1-159">如果為 `true`，則驗證中介軟體設為 `HttpContext.User`並回應泛行挑戰。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-159">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="ef0c1-160">如果為 `false`，則驗證中介軟體僅提供身分識別 (`HttpContext.User`) 並當 `AuthenticationScheme` 提出明確要求時，回應泛型挑戰。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-160">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="ef0c1-161">必須在 IIS 中啟用 Windows 驗證以讓 `AutomaticAuthentication` 作用。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-161">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="ef0c1-162">設定使用者在登入頁面上看到的顯示名稱。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-162">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="ef0c1-163">如果為 `true` 且 `MS-ASPNETCORE-CLIENTCERT` 要求標頭已存在，則會填入 `HttpContext.Connection.ClientCertificate`。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-163">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="ef0c1-164">web.config</span><span class="sxs-lookup"><span data-stu-id="ef0c1-164">web.config</span></span>

<span data-ttu-id="ef0c1-165">*web.config* 檔案會設定 ASP.NET Core 模組，並提供其他 IIS 組態。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-165">The *web.config* file configures the ASP.NET Core Module and provides other IIS configuration.</span></span> <span data-ttu-id="ef0c1-166">`Microsoft.NET.Sdk.Web` 處理 *web.config* 的建立、轉換及發佈，這都包含在當您將專案的 SDK 設定在 *.csproj* 檔案 `<Project Sdk="Microsoft.NET.Sdk.Web">` 的上方時。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-166">Creating, transforming, and publishing *web.config* is handled by `Microsoft.NET.Sdk.Web`, which is included when you set your project's SDK at the top of your *.csproj* file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="ef0c1-167">為防止 MSBuild 目標轉換您的 *web.config* 檔案，請將 **\<IsTransformWebConfigDisabled>** 屬性新增至設定為 `true` 的專案檔：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-167">To prevent the MSBuild target from transforming your *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to your project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="ef0c1-168">如果專案中沒有 *web.config* 檔案，當您使用 *dotnet publish* 或 Visual Studio publish 發佈時，就會為您在發佈的輸出中建立檔案。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-168">If you don't have a *web.config* file in the project when you publish with *dotnet publish* or with Visual Studio publish, the file is created for you in published output.</span></span> <span data-ttu-id="ef0c1-169">如果專案中有該檔案，它會使用正確的 *processPath* 和「引數」轉換以設定 ASP.NET Core 模組，並移至已發佈的輸出。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-169">If you have the file in your project, it's transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="ef0c1-170">轉換不會觸碰檔案包含的 IIS 組態設定。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-170">The transformation doesn't touch IIS configuration settings that you've included in the file.</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="ef0c1-171">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="ef0c1-171">Create the IIS Website</span></span>

1. <span data-ttu-id="ef0c1-172">在目標 IIS 系統上建立資料夾，以包含應用程式的已發佈資料夾和檔案，說明請參閱[目錄結構](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-172">On the target IIS system, create a folder to contain the application's published folders and files, which are described in [Directory Structure](xref:hosting/directory-structure).</span></span>

2. <span data-ttu-id="ef0c1-173">在您建立的資料夾內，建立 *logs* 資料夾來保存應用程式記錄檔 (如果您打算啟用記錄)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-173">Within the folder you created, create a *logs* folder to hold application logs (if you plan to enable logging).</span></span> <span data-ttu-id="ef0c1-174">如果您打算以將 *logs* 資料夾放在承載中的方式來部署您的應用程式，您可以略過此步驟。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-174">If you plan to deploy your application with a *logs* folder in the payload, you may skip this step.</span></span>

3. <span data-ttu-id="ef0c1-175">在 **IIS 管理員**中建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-175">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="ef0c1-176">提供**網站名稱**並將**實體路徑**設定為您建立的應用程式部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-176">Provide a **Site name** and set the **Physical path** to the application's deployment folder that you created.</span></span> <span data-ttu-id="ef0c1-177">提供**繫結**組態並建立網站。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-177">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="ef0c1-178">將應用程式集區設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-178">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="ef0c1-179">ASP.NET Core 會在不同的處理序中執行，並管理執行階段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-179">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="ef0c1-180">開啟 [新增網站]視窗。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-180">Open the **Add Website** window.</span></span>

   ![按一下 [網站] 操作功能表的 [新增網站]。](iis/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="ef0c1-182">設定網站。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-182">Configure the website.</span></span>

   ![在新增網站步驟中提供站台名稱、實體路徑和主機名稱。](iis/_static/add-website-ws2016.png)

7. <span data-ttu-id="ef0c1-184">在 [應用程式集區] 面板中，以滑鼠右鍵按一下網站的應用程式集區，並從快顯功能表中選取 [基本設定...]，開啟 [編輯應用程式集區] 視窗。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-184">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's application pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![選取 [應用程式集區] 操作功能表的 [基本設定]。](iis/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="ef0c1-186">將 **.NET CLR 版本**設為 [沒有 Managed 程式碼]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-186">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![將 .NET CLR 版本設為 [沒有 Managed 程式碼]。](iis/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="ef0c1-188">注意：將 **.NET CLR 版本**設為 [沒有 Managed 程式碼] 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-188">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="ef0c1-189">ASP.NET Core 不依賴載入桌面 CLR。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-189">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="ef0c1-190">確認處理序模型身分識別具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-190">Confirm the process model identity has the proper permissions.</span></span>

    <span data-ttu-id="ef0c1-191">如果您將應用程式集區的預設身分識別從 **ApplicationPoolIdentity** 變更為其他身分識別 ([處理序模型] > [身分識別])，確認新的身分識別具有必要權限，可存取應用程式的資料夾、資料庫和其他必要的資源。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-191">If you change the default identity of the application pool (**Process Model** > **Identity**) from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the application's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-application"></a><span data-ttu-id="ef0c1-192">部署應用程式</span><span class="sxs-lookup"><span data-stu-id="ef0c1-192">Deploy the application</span></span>
<span data-ttu-id="ef0c1-193">將應用程式部署至您在目標 IIS 系統上建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-193">Deploy the application to the folder you created on the target IIS system.</span></span> <span data-ttu-id="ef0c1-194">Web Deploy 是建議的部署機制。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-194">Web Deploy is the recommended mechanism for deployment.</span></span> <span data-ttu-id="ef0c1-195">Web Deploy 的替代項目列示如下。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-195">Alternatives to Web Deploy are listed below.</span></span>

<span data-ttu-id="ef0c1-196">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-196">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="ef0c1-197">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-197">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="ef0c1-198">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-198">Deployment can't occur because locked files can't be copied.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="ef0c1-199">使用 Visual Studio 的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ef0c1-199">Web Deploy with Visual Studio</span></span>
<span data-ttu-id="ef0c1-200">請參閱[建立 Visual Studio 和 MSBuild 的發行設定檔，以部署 ASP.NET Core 應用程式](xref:publishing/web-publishing-vs#publish-profiles)主題，了解如何建立發行設定檔供 Web Deploy 使用。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-200">See [Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps](xref:publishing/web-publishing-vs#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="ef0c1-201">如果您的裝載提供者提供或支援建立發行設定檔，請下載並使用 Visual Studio 的 [發佈] 對話方塊匯入其設定檔。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-201">If your hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![[發佈] 對話方塊頁](iis/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="ef0c1-203">Visual Studio 外部的 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ef0c1-203">Web Deploy outside of Visual Studio</span></span>
<span data-ttu-id="ef0c1-204">您也可以從命令列使用 Visual Studio 外部的 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-204">You can also use Web Deploy outside of Visual Studio from the command line.</span></span> <span data-ttu-id="ef0c1-205">如需詳細資訊，請參閱 [Web 部署工具](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-205">For more information, see [Web Deployment Tool](https://docs.microsoft.com/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="ef0c1-206">Web Deploy 的替代項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-206">Alternatives to Web Deploy</span></span>
<span data-ttu-id="ef0c1-207">如果您不想使用 Web Deploy 或不是使用 Visual Studio，您可以使用 Xcopy、Robocopy 或 PowerShell 等任何一種方法，將應用程式移至主控系統。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-207">If you don't wish to use Web Deploy or are not using Visual Studio, you may use any of several methods to move the application to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="ef0c1-208">Visual Studio 使用者可以使用[發佈範例](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-208">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="ef0c1-209">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="ef0c1-209">Browse the website</span></span>
![Microsoft Edge 瀏覽器已載入 IIS 啟動頁面。](iis/_static/browsewebsite.png)
   
>[!WARNING]
> <span data-ttu-id="ef0c1-211">.NET Core 應用程式是透過 IIS 和 Kestrel 伺服器之間的反向 Proxy 裝載。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-211">.NET Core applications are hosted via a reverse-proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="ef0c1-212">為了建立反向 Proxy，*web.config* 檔案必須存在於已部署應用程式的內容根路徑 (通常是應用程式基底路徑)，這是提供給 IIS 的網站實體路徑。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-212">In order to create the reverse-proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed application, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="ef0c1-213">機密檔案位於應用程式的實體路徑，包括 *my_application.runtimeconfig.json*、*my_application.xml* (XML 文件註解) 和 *my_application.deps.json* 等子資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-213">Sensitive files exist on the app's physical path, including subfolders, such as *my_application.runtimeconfig.json*, *my_application.xml* (XML Documentation comments), and *my_application.deps.json*.</span></span> <span data-ttu-id="ef0c1-214">需要有 *web.config* 檔案才能建立 Kestrel 的反向 Proxy，防止 IIS 提供這些和其他機密檔案。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-214">The *web.config* file is required to create the reverse proxy to Kestrel, which prevents IIS from serving these and other sensitive files.</span></span> <span data-ttu-id="ef0c1-215">**因此，絕對不要意外重新命名或從部署移除 *web.config* 檔案，這一點十分重要。**</span><span class="sxs-lookup"><span data-stu-id="ef0c1-215">**Therefore, it is important that the *web.config* file is never accidently renamed or removed from the deployment.**</span></span>

## <a name="data-protection"></a><span data-ttu-id="ef0c1-216">資料保護</span><span class="sxs-lookup"><span data-stu-id="ef0c1-216">Data protection</span></span>

<span data-ttu-id="ef0c1-217">下列情況下，ASP.NET Core 應用程式會將 Keyring 儲存在記憶體中：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-217">An ASP.NET Core application will store the keyring in memory under the following condition:</span></span>

* <span data-ttu-id="ef0c1-218">網站裝載在 IIS 後端。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-218">A website is hosted behind IIS.</span></span>
* <span data-ttu-id="ef0c1-219">尚未設定資料保護堆疊將 Keyring 儲存在持續性存放區中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-219">The Data Protection stack has not been configured to store the keyring in a persistent store.</span></span>

<span data-ttu-id="ef0c1-220">如果 Keyring 儲存在記憶體中，則當應用程式重新啟動時：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-220">If the keyring is stored in memory, when the app restarts:</span></span>

* <span data-ttu-id="ef0c1-221">所有表單驗證權杖都將無效。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-221">All forms authentication tokens will be invalid.</span></span> 
* <span data-ttu-id="ef0c1-222">下一次要求時使用者需要再次登入。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-222">Users will need to login again on their next request.</span></span> 
* <span data-ttu-id="ef0c1-223">使用 Keyring 保護的所有資料都會受到保護。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-223">Any data you protected with the keyring will no longer be unprotected.</span></span>

> [!WARNING]
> <span data-ttu-id="ef0c1-224">有數個 ASP.NET 中介軟體使用資料保護，包括在驗證中使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-224">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="ef0c1-225">即使不特別從自己的程式碼呼叫任何資料保護 API，您也應該使用部署指令碼或在您自己的程式碼中設定資料保護。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-225">Even if you do not specifically call any Data Protection APIs from your own code you should configure Data Protection with a deployment script or in your own code.</span></span> <span data-ttu-id="ef0c1-226">如果您不設定資料保護，索引鍵預設會保留在記憶體中，並於應用程式重新啟動時捨棄。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-226">If you do not configure data protection, by default the keys will be held in memory and discarded when your app restarts.</span></span> <span data-ttu-id="ef0c1-227">重新啟動會讓 Cookie 驗證寫入的所有 Cookie 失效，使用者必須再次登入。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-227">Restarting will invalidate any cookies written by the cookie authentication and users will have to login again.</span></span>

<span data-ttu-id="ef0c1-228">若要在 IIS 下設定資料保護，您必須使用下列方法之一：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-228">To configure Data Protection under IIS you must use one of the following approaches:</span></span>

* <span data-ttu-id="ef0c1-229">執行 [powershell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) 建立適當的登錄項目 (例如，`.\Provision-AutoGenKeys.ps1 DefaultAppPool`)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-229">Run a [powershell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) to create suitable registry entries (For example,  `.\Provision-AutoGenKeys.ps1 DefaultAppPool`).</span></span> <span data-ttu-id="ef0c1-230">這會將金鑰存放在登錄中，使用具有全電腦金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-230">This will store keys in the registry, protected using DPAPI with a machine wide key.</span></span>
* <span data-ttu-id="ef0c1-231">設定 IIS 應用程式集區載入使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-231">Configure the IIS Application Pool to load the user profile.</span></span> <span data-ttu-id="ef0c1-232">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-232">This setting is in the **Process Model** section under the **Advanced Settings** for the application pool.</span></span> <span data-ttu-id="ef0c1-233">將 [載入使用者設定檔] 設為 `True`。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-233">Set **Load User Profile** to `True`.</span></span> <span data-ttu-id="ef0c1-234">這會將索引鍵儲存在使用者設定檔目錄下，使用具有應用程式集區使用的使用者帳戶特定金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-234">This will store keys under the user profile directory, and protected using DPAPI with a key specific to the user account used for the app pool.</span></span>
* <span data-ttu-id="ef0c1-235">調整應用程式程式碼[將檔案系統用為 Keyring 存放區](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-235">Adjust your application code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="ef0c1-236">使用 X509 憑證保護 Keyring，確保它是受信任的憑證。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-236">Use an X509 certificate to protect the key ring and ensure it is a trusted certificate.</span></span> <span data-ttu-id="ef0c1-237">例如，如果它是自我簽署的憑證，就必須放在受信任的根存放區。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-237">For example, if it is a self signed certificate you must place it in the Trusted Root store.</span></span>

<span data-ttu-id="ef0c1-238">在 Web 伺服陣列中使用 IIS 時：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-238">When using IIS in a web farm:</span></span>

* <span data-ttu-id="ef0c1-239">使用所有電腦都可以存取的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-239">Use a file share all machines can access.</span></span>
* <span data-ttu-id="ef0c1-240">將 X509 憑證部署到每一部電腦。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-240">Deploy an X509 certificate to each machine.</span></span>  <span data-ttu-id="ef0c1-241">設定[程式碼中的資料保護](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-241">Configure [data protection in code](https://docs.microsoft.com/aspnet/core/security/data-protection/configuration/overview).</span></span>

### <a name="1-create-a-data-protection-registry-hive"></a><span data-ttu-id="ef0c1-242">1.建立資料保護登錄 Hive</span><span class="sxs-lookup"><span data-stu-id="ef0c1-242">1. Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="ef0c1-243">ASP.NET 應用程式使用的資料保護金鑰會儲存在應用程式外部的登錄 Hive 中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-243">Data Protection keys used by ASP.NET applications are stored in registry hives external to the applications.</span></span> <span data-ttu-id="ef0c1-244">若要保存指定應用程式的金鑰，您必須建立應用程式的應用程式集區登錄 Hive。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-244">To persist the keys for a given application, you must create a registry hive for the application's application pool.</span></span>

<span data-ttu-id="ef0c1-245">若為獨立的 IIS 安裝，您可以針對和 ASP.NET Core 應用程式一起使用的每個應用程式集區使用[資料保護 Provision-AutoGenKeys.ps1 PowerShell 指令碼](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-245">For standalone IIS installations, you may use the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) for each application pool used with an ASP.NET Core application.</span></span> <span data-ttu-id="ef0c1-246">此指令碼會在 HKLM 登錄中建立特殊的登錄機碼，HKLM 登錄只有碰到背景工作處理序帳戶才會是 ACL。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-246">This script will create a special registry key in the HKLM registry that is ACLed only to the worker process account.</span></span> <span data-ttu-id="ef0c1-247">在待用期間使用 DPAPI 加密金鑰。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-247">Keys are encrypted at rest using DPAPI.</span></span>

<span data-ttu-id="ef0c1-248">在 Web 伺服陣列案例中，應用程式可以設定成使用 UNC 路徑來儲存其資料保護 Keyring。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-248">In web farm scenarios, an application can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="ef0c1-249">根據預設，資料保護金鑰不予加密。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-249">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="ef0c1-250">您應該確保這類共用的檔案權限僅限於執行應用程式的 Windows 帳戶身分。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-250">You should ensure that the file permissions for such a share are limited to the Windows account the application runs as.</span></span> <span data-ttu-id="ef0c1-251">此外，您可以選擇使用 X509 憑證保護待用的金鑰。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-251">In addition you may choose to protect keys at rest using an X509 certificate.</span></span> <span data-ttu-id="ef0c1-252">您可能要考慮這樣的機制：讓使用者上傳憑證、將它們放入使用者的受信任的憑證存放區，並確保在執行使用者應用程式的所有電腦上都能使用它們。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-252">You may wish to consider a mechanism to allow users to upload certificates, place them into the user's trusted certificate store, and ensure they are available on all machines the user's application will run on.</span></span> <span data-ttu-id="ef0c1-253">詳細資訊請參閱[設定資料保護](xref:security/data-protection/configuration/overview#data-protection-configuring)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-253">See [Configuring Data Protection](xref:security/data-protection/configuration/overview#data-protection-configuring) for details.</span></span>

### <a name="2-configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="ef0c1-254">2.設定 IIS 應用程式集區載入使用者設定檔</span><span class="sxs-lookup"><span data-stu-id="ef0c1-254">2. Configure the IIS Application Pool to load the user profile</span></span>
<span data-ttu-id="ef0c1-255">此設定位在應用程式集區 [進階設定] 下的 [處理序模型] 區段中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-255">This setting is in the Process Model section under the Advanced Settings for the application pool.</span></span> <span data-ttu-id="ef0c1-256">將 [載入使用者設定檔] 設為 True。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-256">Set Load User Profile to True.</span></span> <span data-ttu-id="ef0c1-257">這會將索引鍵儲存在使用者設定檔目錄下，使用具有應用程式集區使用的使用者帳戶特定金鑰的 DPAPI 保護。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-257">This will store keys under the user profile directory, and protected using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="3-machine-wide-policy-for-data-protection"></a><span data-ttu-id="ef0c1-258">3.資料保護的全電腦原則</span><span class="sxs-lookup"><span data-stu-id="ef0c1-258">3. Machine-wide policy for data protection</span></span>

<span data-ttu-id="ef0c1-259">資料保護系統對針對取用資料保護 API 的所有應用程式設定預設[全電腦原則](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy)的支援有限。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-259">The data protection system has limited support for setting default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy#data-protection-configuration-machinewidepolicy) for all applications that consume the data protection APIs.</span></span> <span data-ttu-id="ef0c1-260">如需詳細資訊，請參閱[資料保護](xref:security/data-protection/index)文件。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="ef0c1-261">子應用程式的組態</span><span class="sxs-lookup"><span data-stu-id="ef0c1-261">Configuration of sub-applications</span></span>

<span data-ttu-id="ef0c1-262">根應用程式下新增的子應用程式不應包含 ASP.NET Core 模組當做處理常式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-262">Sub applications added under the root application shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="ef0c1-263">如果您在子應用程式的 *web.config* 檔案中將模組新增為處理常式，當您嘗試瀏覽子應用程式時，您會收到參考錯誤組態檔的 500.19 (內部伺服器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-263">If you add the module as a handler in a sub-application's *web.config* file, you receive a 500.19 (Internal Server Error) referencing the faulty config file when you attempt to browse the sub app.</span></span> <span data-ttu-id="ef0c1-264">下例顯示已發佈之 ASP.NET Core 子應用程式的 *web.config* 檔案內容：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub app:</span></span>

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

<span data-ttu-id="ef0c1-265">如果您打算在 ASP.NET Core 應用程式下裝載非 ASP.NET Core 子應用程式，您必須明確移除子應用程式 *web.config* 檔案中已繼承的處理常式：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-265">If you intend to host a non-ASP.NET Core sub app underneath an ASP.NET Core app, you must explicitly remove the inherited handler in the sub app *web.config* file:</span></span>

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

<span data-ttu-id="ef0c1-266">如需使用 *web.config* 檔案設定 ASP.NET Core 模組的詳細資訊，請參閱 [ASP.NET Core 模組簡介](xref:fundamentals/servers/aspnet-core-module)主題和 [ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-266">For more information on configuring the ASP.NET Core Module with the *web.config* file, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="ef0c1-267">使用 web.config 的 IIS 組態</span><span class="sxs-lookup"><span data-stu-id="ef0c1-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="ef0c1-268">IIS 組態仍然會受到這些 IIS 功能 *web.config* 的 `<system.webServer>` 區段所影響，這些 IIS 功能適用於反向 Proxy 組態。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-268">IIS configuration is still influenced by the `<system.webServer>` section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="ef0c1-269">例如，您可能在系統層級設定 IIS 使用動態壓縮，但可使用應用程式之 *web.config* 檔案中的 `<urlCompression>` 元素，停用應用程式的該項設定。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-269">For example, you may have IIS configured at the system level to use dynamic compression, but you could disable that setting for an app with the `<urlCompression>` element in the app's *web.config* file.</span></span> <span data-ttu-id="ef0c1-270">如需詳細資訊，請參閱 [`<system.webServer>` 的組態參考](https://docs.microsoft.com/iis/configuration/system.webServer/)、[ASP.NET Core 模組組態參考](xref:hosting/aspnet-core-module)，和[使用 IIS 模組與 ASP.NET Core](xref:hosting/iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-270">For more information, see the [configuration reference for `<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:hosting/aspnet-core-module), and [Using IIS Modules with ASP.NET Core](xref:hosting/iis-modules).</span></span> <span data-ttu-id="ef0c1-271">如果您需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-271">If you need to set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="ef0c1-272">web.config 的組態區段</span><span class="sxs-lookup"><span data-stu-id="ef0c1-272">Configuration sections of web.config</span></span>

<span data-ttu-id="ef0c1-273">有別於使用 *web.config* 中的 `<system.web>`、`<appSettings>`、`<connectionStrings>` 和 `<location>` 元素所設定的 .NET Framework 應用程式，ASP.NET Core 應用程式設定使用其他組態提供者。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-273">Unlike .NET Framework applications that are configured with the `<system.web>`, `<appSettings>`, `<connectionStrings>`, and `<location>` elements in *web.config*, ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="ef0c1-274">如需詳細資訊，請參閱[組態](xref:fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-274">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="application-pools"></a><span data-ttu-id="ef0c1-275">應用程式集區</span><span class="sxs-lookup"><span data-stu-id="ef0c1-275">Application Pools</span></span>

<span data-ttu-id="ef0c1-276">當多個網站裝載在單一系統上時，您應該在其各自的應用程式集區中執行每個應用程式，以隔開所有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-276">When hosting multiple websites on a single system, you should isolate the applications from each other by running each app in its own application pool.</span></span> <span data-ttu-id="ef0c1-277">IIS [新增網站] 對話方塊預設執行此行為。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-277">The IIS **Add Website** dialog defaults to this behavior.</span></span> <span data-ttu-id="ef0c1-278">當您提供**站台名稱**時，文字會自動轉移至 [應用程式集區] 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-278">When you provide a **Site name**, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="ef0c1-279">當您新增網站時，即使用該站台名稱建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-279">A new application pool will be created using the site name when you add the website.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="ef0c1-280">應用程式集區身分識別</span><span class="sxs-lookup"><span data-stu-id="ef0c1-280">Application Pool Identity</span></span>

<span data-ttu-id="ef0c1-281">應用程式集區身分識別帳戶可讓您在唯一的帳戶下執行應用程式，不必建立及管理網域或本機帳戶。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-281">An application pool identity account allows you to run an application under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="ef0c1-282">在 IIS 8.0+ 上，IIS 管理背景工作處理序 (WAS) 會使用新的應用程式集區名稱建立虛擬帳戶，預設在此帳戶下執行應用程式集區的背景工作處理序。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-282">On IIS 8.0+, the IIS Admin Worker Process (WAS) will create a virtual account with the name of the new application pool and run the application pool's worker processes under this account by default.</span></span> <span data-ttu-id="ef0c1-283">在 IIS 管理主控台中，於您的應用程式集區 [進階設定] 下，確定 [身分識別] 設定為使用 **ApplicationPoolIdentity**，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-283">In the IIS Management Console, under Advanced Settings for your application pool, ensure that the Identity is set to use **ApplicationPoolIdentity** as shown in the image below.</span></span>

![應用程式集區進階設定對話方塊](iis/_static/apppool-identity.png)

<span data-ttu-id="ef0c1-285">IIS 管理程序會在 Windows 安全系統中以應用程式集區的名稱建立安全識別碼。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-285">The IIS management process creates a secure identifier with the name of the application pool in the Windows Security System.</span></span> <span data-ttu-id="ef0c1-286">使用這個身分識別可以保護資源，但此身分識別不是真正的使用者帳戶，也不會顯示在 Windows 使用者管理主控台中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-286">Resources can be secured by using this identity; however, this identity is not a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="ef0c1-287">如果您需要將提升的 IIS 背景工作處理序存取權授與您的應用程式，就需要修改包含您應用程式的目錄存取控制清單 (ACL)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-287">If you need to grant the IIS worker process elevated access to your application, you will need to modify the Access Control List (ACL) for the directory containing your application.</span></span>

1. <span data-ttu-id="ef0c1-288">開啟 Windows 檔案總管，巡覽至目錄。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-288">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="ef0c1-289">以滑鼠右鍵按一下目錄，然後按一下 [內容]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-289">Right click on the directory and click **Properties**.</span></span>

3. <span data-ttu-id="ef0c1-290">依序按一下 [安全性] 索引標籤下的 [編輯] 按鈕和 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-290">Under the **Security** tab, click the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="ef0c1-291">按一下 [位置] 按鈕，並確定選取您的系統。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-291">Click the **Locations** button and make sure you select your system.</span></span>

5. <span data-ttu-id="ef0c1-292">在 [輸入要選取的物件名稱] 文字方塊中輸入 **IIS AppPool\DefaultAppPool**。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-292">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="ef0c1-294">按一下 [檢查名稱] 按鈕，再按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-294">Click the **Check Names** button and then click **OK**.</span></span>

  ![選取應用程式資料夾的 [使用者或群組] 對話方塊](iis/_static/select-users-or-groups-2.png)

<span data-ttu-id="ef0c1-296">您也可以透過命令提示字元使用 **ICACLS** 工具執行此作業：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-296">You can also do this via a command prompt using **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="troubleshooting-tips"></a><span data-ttu-id="ef0c1-297">疑難排解提示</span><span class="sxs-lookup"><span data-stu-id="ef0c1-297">Troubleshooting tips</span></span>

<span data-ttu-id="ef0c1-298">若要診斷 IIS 部署的問題，請研究瀏覽器輸出、透過 [事件檢視器] 檢查系統的**應用程式**記錄檔，並啟用 `stdout` 記錄。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-298">To diagnose problems with IIS deployments, study browser output, examine the system's **Application** log through **Event Viewer**, and enable `stdout` logging.</span></span> <span data-ttu-id="ef0c1-299">在 *web.config* 的 `<aspNetCore>` 元素的 *stdoutLogFile* 屬性提供的路徑上，會找到 **ASP.NET Core 模組**記錄。部署中必須有屬性值提供之路徑上的所有資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-299">The **ASP.NET Core Module** log will be found on the path provided in the *stdoutLogFile* attribute of the `<aspNetCore>` element in *web.config*. Any folders on the path provided in the attribute value must exist in the deployment.</span></span> <span data-ttu-id="ef0c1-300">您也必須設定 *stdoutLogEnabled ="true"*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-300">You must also set *stdoutLogEnabled="true"*.</span></span> <span data-ttu-id="ef0c1-301">使用 `Microsoft.NET.Sdk.Web` SDK 建立 *web.config* 檔案的應用程式，其 *stdoutLogEnabled* 設定預設值為 *false*，因此您必須手動提供 *web.config* 檔案或修改檔案，以啟用 `stdout` 記錄。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-301">Applications that use the `Microsoft.NET.Sdk.Web` SDK to create the *web.config* file will default the *stdoutLogEnabled* setting to *false*, so you must manually provide the *web.config* file or modify the file in order to enable `stdout` logging.</span></span>

<span data-ttu-id="ef0c1-302">要等到模組 *startupTimeLimit* (預設值：120 秒) 和 *startupRetryCount* (預設值：2) 通過後，數個常見錯誤才會出現在瀏覽器、應用程式記錄檔和 ASP.NET Core 模組記錄檔中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-302">Several of the common errors do not appear in the browser, Application Log, and ASP.NET Core Module Log until the module *startupTimeLimit* (default: 120 seconds) and *startupRetryCount* (default: 2) have passed.</span></span> <span data-ttu-id="ef0c1-303">因此，要等候整整六分鐘，才能推算出模組無法啟動應用程式的程序。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-303">Therefore, wait a full six minutes before deducing that the module has failed to start a process for the application.</span></span>

<span data-ttu-id="ef0c1-304">有一個快速方法可判斷應用程式是否正常運作，即直接在 Kestrel 上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-304">One quick way to determine if the application is working properly is to run the application directly on Kestrel.</span></span> <span data-ttu-id="ef0c1-305">應用程式如已發佈為與 Framework 相依的部署，請執行部署資料夾中的 **dotnet my_application.dll**，這是應用程式的 IIS 實體路徑。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-305">If the application was published as a framework-dependent deployment, execute **dotnet my_application.dll** in the deployment folder, which is the IIS physical path to the application.</span></span> <span data-ttu-id="ef0c1-306">如果應用程式已發佈為獨立部署，請直接從命令提示字元執行部署資料夾中的應用程式可執行檔 **my_application.exe**。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-306">If the application was published as a self-contained deployment, run the application's executable directly from a command prompt, **my_application.exe**, in the deployment folder.</span></span> <span data-ttu-id="ef0c1-307">如果 Kestrel 接聽預設通訊埠 5000，您應該能夠瀏覽在 `http://localhost:5000/` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-307">If Kestrel is listening on default port 5000, you should be able to browse the application at `http://localhost:5000/`.</span></span> <span data-ttu-id="ef0c1-308">如果應用程式通常在 Kestrel 端點位址回應，IIS-ASP.NET Core 模組-Kestrel 組態出問題的機率比較大，應用程式本身出問題的機率比較小。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-308">If the application responds normally at the Kestrel endpoint address, the problem is more likely related to the IIS-ASP.NET Core Module-Kestrel configuration and less likely within the application itself.</span></span>

<span data-ttu-id="ef0c1-309">判定 Kestrel 伺服器的 IIS 反向 Proxy 是否正常運作的一種方式，是使用[靜態檔案中介軟體](xref:fundamentals/static-files)針對樣式表、指令碼或來自 *wwwroot* 應用程式靜態檔案的影像，執行簡單的靜態檔案要求。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-309">One way to determine if the IIS reverse proxy to the Kestrel server is working properly is to perform a simple static file request for a stylesheet, script, or image from the application's static files in *wwwroot* using [Static File middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="ef0c1-310">如果應用程式可以提供靜態檔案，但 MVC 檢視和其他端點卻失敗，就不太可能是 IIS-ASP.NET Core 模組-Kestrel 組態出問題，更可能是應用程式本身出問題 (例如，MVC 路由或 500 內部伺服器錯誤)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-310">If the application can serve static files but MVC Views and other endpoints are failing, the problem is less likely related to the IIS-ASP.NET Core Module-Kestrel configuration and more likely within the application itself (for example, MVC routing or 500 Internal Server Error).</span></span>

<span data-ttu-id="ef0c1-311">當 Kestrel 在 IIS 後端正常啟動，但應用程式在本機上成功執行之後卻不能在系統上執行時，您可以暫時將環境變數新增至 *web.config*，將 `ASPNETCORE_ENVIRONMENT` 設成 `Development`。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-311">When Kestrel starts normally behind IIS but the app won't run on the system after successfully running locally, you can temporarily add an environment variable to *web.config* to set the `ASPNETCORE_ENVIRONMENT` to `Development`.</span></span> <span data-ttu-id="ef0c1-312">只要您不覆寫應用程式啟動的環境，應用程式在系統上執行時，[開發人員例外狀況頁面](xref:fundamentals/error-handling)就會出現。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-312">As long as you don't override the environment in app startup, this will allow the [developer exception page](xref:fundamentals/error-handling) to appear when the app is run on the system.</span></span> <span data-ttu-id="ef0c1-313">只有不公開到網際網路的臨時/測試系統，才建議以這種方式設定 `ASPNETCORE_ENVIRONMENT` 的環境變數。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-313">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` in this way is only recommended for staging/testing systems that are not exposed to the Internet.</span></span> <span data-ttu-id="ef0c1-314">完成後，請務必從 *web.config* 檔案移除環境變數。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-314">Be sure you remove the environment variable from the *web.config* file when finished.</span></span> <span data-ttu-id="ef0c1-315">如需透過反向 Proxy 的 *web.config* 設定環境變數的相關資訊，請參閱 [aspNetCore 的 environmentVariables 子元素](xref:hosting/aspnet-core-module#setting-environment-variables)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-315">For information on setting environment variables via *web.config* for the reverse proxy, see [environmentVariables child element of aspNetCore](xref:hosting/aspnet-core-module#setting-environment-variables).</span></span>

<span data-ttu-id="ef0c1-316">在大部分情況下，啟用應用程式記錄會協助疑難排解應用程式或反向 Proxy 的問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-316">In most cases, enabling application logging will assist in troubleshooting problems with application or the reverse proxy.</span></span> <span data-ttu-id="ef0c1-317">如需詳細資訊，請參閱[記錄](xref:fundamentals/logging)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-317">See [Logging](xref:fundamentals/logging) for more information.</span></span>

<span data-ttu-id="ef0c1-318">升級開發電腦的 .NET Core SDK 或應用程式內的套件版本後，最後有關應用程式的疑難排解提示無法執行。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-318">Our last troubleshooting tip pertains to apps that fail to run after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="ef0c1-319">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-319">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="ef0c1-320">這些問題大部分是可以修正的，方法是刪除專案中的 `bin` 和 `obj` 資料夾、清除 `%UserProfile%\.nuget\packages\` 和 `%LocalAppData%\Nuget\v3-cache` 的套件快取、還原專案，然後確認已完全刪除之前在系統上的部署，再重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-320">You can fix most of these issues by deleting the `bin` and `obj` folders in the project, clearing package caches at `%UserProfile%\.nuget\packages\` and `%LocalAppData%\Nuget\v3-cache`, restoring the project, and confirming that your prior deployment on the system has been completely deleted prior to re-deploying the app.</span></span>

>[!TIP]
> <span data-ttu-id="ef0c1-321">有一個便利的方式可以清除套件快取，即從 [NuGet.org](https://www.nuget.org/) 取得 `NuGet.exe` 工具，將它新增系統的 PATH，再從命令提示字元執行 `nuget locals all -clear`。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-321">A convenient way to clear package caches is to obtain the `NuGet.exe` tool from [NuGet.org](https://www.nuget.org/), add it to your system PATH, and execute `nuget locals all -clear` from a command prompt.</span></span>

## <a name="common-errors"></a><span data-ttu-id="ef0c1-322">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="ef0c1-322">Common errors</span></span>

<span data-ttu-id="ef0c1-323">以下是不完整的錯誤清單。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-323">The following is not a complete list of errors.</span></span> <span data-ttu-id="ef0c1-324">假如遇到此處未列出的錯誤，請在下方的 [註解] 區段中留下詳細的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-324">Should you encounter an error not listed here, please leave a detailed error message in the comments section below.</span></span>

### <a name="installer-unable-to-obtain-vc-redistributable"></a><span data-ttu-id="ef0c1-325">安裝程式無法取得 VC++ 可轉散發套件</span><span class="sxs-lookup"><span data-stu-id="ef0c1-325">Installer unable to obtain VC++ Redistributable</span></span>

* <span data-ttu-id="ef0c1-326">**安裝程式的例外狀況：**0x80072efd 或 0x80072f76 - 未指定的錯誤</span><span class="sxs-lookup"><span data-stu-id="ef0c1-326">**Installer Exception:** 0x80072efd or 0x80072f76 - Unspecified error</span></span>

* <span data-ttu-id="ef0c1-327">**安裝程式記錄例外狀況&#8224;：**錯誤 0x80072efd 或 0x80072f76：無法執行 EXE 套件</span><span class="sxs-lookup"><span data-stu-id="ef0c1-327">**Installer Log Exception&#8224;:** Error 0x80072efd or 0x80072f76: Failed to execute EXE package</span></span>

  <span data-ttu-id="ef0c1-328">&#8224;記錄位於 C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-328">&#8224;The log is located at C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.</span></span>

<span data-ttu-id="ef0c1-329">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-329">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-330">如果安裝伺服器裝載套件組合時，系統不能存取網際網路，當安裝程式受阻無法取得 *Microsoft Visual C++ 2015 可轉散發套件*時，就會發生這個例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-330">If the system does not have Internet access while installing the server hosting bundle, this exception will occur when the installer is prevented from obtaining the *Microsoft Visual C++ 2015 Redistributable*.</span></span> <span data-ttu-id="ef0c1-331">您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-331">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span> <span data-ttu-id="ef0c1-332">如果安裝程式失敗，您可能不會收到裝載與 Framework 相依部署的必要 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-332">If the installer fails, you may not receive the .NET Core runtime required to host framework-dependent deployments.</span></span> <span data-ttu-id="ef0c1-333">如果您打算裝載與 Framework 相依的部署，請確認已在 [程式和功能] 中安裝執行階段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-333">If you plan to host framework-dependent deployments, confirm that the runtime is installed in Programs &amp; Features.</span></span> <span data-ttu-id="ef0c1-334">您可從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-334">You may obtain a runtime installer from [.NET Downloads](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="ef0c1-335">安裝執行階段之後，從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-335">After installing the runtime, restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

### <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="ef0c1-336">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="ef0c1-336">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

* <span data-ttu-id="ef0c1-337">**應用程式記錄檔：**無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-337">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="ef0c1-338">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-338">The data is the error.</span></span>

<span data-ttu-id="ef0c1-339">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-339">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-340">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-340">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory are not preserved during an OS upgrade.</span></span> <span data-ttu-id="ef0c1-341">如果先安裝 ASP.NET Core 模組再升級作業系統，然後嘗試在作業系統升級之後在 32 位元模式中執行任何 AppPool，就會發生這個問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-341">If you have the ASP.NET Core Module installed prior to an OS upgrade and then try to run any AppPool in 32-bit mode after an OS upgrade, you will encounter this issue.</span></span> <span data-ttu-id="ef0c1-342">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-342">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="ef0c1-343">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-343">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="ef0c1-344">執行安裝程式時，請選取 [修復]。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-344">Select **Repair** when you run the installer.</span></span>

### <a name="platform-conflicts-with-rid"></a><span data-ttu-id="ef0c1-345">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="ef0c1-345">Platform conflicts with RID</span></span>

* <span data-ttu-id="ef0c1-346">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="ef0c1-346">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ef0c1-347">**應用程式記錄檔：**實體根目錄為 'C:\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法使用命令行 '"C:\\{PATH}\my_application.{exe|dll}" ' 啟動程序，ErrorCode = '0x80004005 : ff。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-347">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="ef0c1-348">**ASP.NET Core 模組記錄檔：**未處理的例外狀況：System.BadImageFormatException：無法載入檔案或組件 'my_application.dll'。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-348">**ASP.NET Core Module Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly 'my_application.dll'.</span></span> <span data-ttu-id="ef0c1-349">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-349">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="ef0c1-350">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-350">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-351">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-351">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="ef0c1-352">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-352">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="ef0c1-353">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-353">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="ef0c1-354">確認您並未在 *.csproj* 中設定與 RID 發生衝突的 `<PlatformTarget>`。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-354">Confirm that you didn't set a `<PlatformTarget>` in your *.csproj* that conflicts with the RID.</span></span> <span data-ttu-id="ef0c1-355">例如，藉由使用 *dotnet publish -c Release -r win10-x64*，或將 *.csproj* 的 `<RuntimeIdentifiers>` 設定為 `win10-x64`，不指定 `x86` 的 `<PlatformTarget>` 且以 `win10-x64` 的 RID 發佈。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-355">For example, don't specify a `<PlatformTarget>` of `x86` and publish with an RID of `win10-x64`, either by using *dotnet publish -c Release -r win10-x64* or by setting the `<RuntimeIdentifiers>` in your *.csproj* to `win10-x64`.</span></span> <span data-ttu-id="ef0c1-356">專案發佈且沒有警告或錯誤，但會失敗，因為上述記錄在系統上的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-356">The project will publish without warning or error but fail with the above logged exceptions on the system.</span></span>

* <span data-ttu-id="ef0c1-357">如果在升級應用程式和部署新的組件時， Azure 應用程式部署發生這個例外狀況，請手動刪除先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-357">If this exception occurs for an Azure Apps deployment when upgrading an application and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="ef0c1-358">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-358">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

### <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="ef0c1-359">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="ef0c1-359">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="ef0c1-360">**瀏覽器：**ERR_CONNECTION_REFUSED</span><span class="sxs-lookup"><span data-stu-id="ef0c1-360">**Browser:** ERR_CONNECTION_REFUSED</span></span>

* <span data-ttu-id="ef0c1-361">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-361">**Application Log:** No entry</span></span>

* <span data-ttu-id="ef0c1-362">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="ef0c1-362">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="ef0c1-363">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-363">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-364">確認您為應用程式使用的是正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-364">Confirm you are using the correct URI endpoint for the application.</span></span> <span data-ttu-id="ef0c1-365">請檢查您的繫結。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-365">Check your bindings.</span></span>

* <span data-ttu-id="ef0c1-366">確認 IIS 網站不是「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-366">Confirm that the IIS website is not in the *Stopped* state.</span></span>

### <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="ef0c1-367">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="ef0c1-367">CoreWebEngine or W3SVC server features disabled</span></span>

* <span data-ttu-id="ef0c1-368">**作業系統例外狀況：**必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-368">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="ef0c1-369">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-369">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-370">確認您已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-370">Confirm that you have enabled the proper role and features.</span></span> <span data-ttu-id="ef0c1-371">請參閱 [IIS 組態](#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-371">See [IIS Configuration](#iis-configuration).</span></span>

### <a name="incorrect-website-physical-path-or-application-missing"></a><span data-ttu-id="ef0c1-372">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="ef0c1-372">Incorrect website physical path or application missing</span></span>

* <span data-ttu-id="ef0c1-373">**瀏覽器：** 403 禁止 - 存取被拒**--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-373">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="ef0c1-374">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-374">**Application Log:** No entry</span></span>

* <span data-ttu-id="ef0c1-375">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="ef0c1-375">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="ef0c1-376">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-376">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-377">請檢查 IIS 網站的 [基本設定] 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-377">Check the IIS website **Basic Settings** and the physical application folder.</span></span> <span data-ttu-id="ef0c1-378">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-378">Confirm that the application is in the folder at the IIS website **Physical path**.</span></span>

### <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="ef0c1-379">角色不正確、模組未安裝，或權限不正確。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-379">Incorrect role, module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="ef0c1-380">**瀏覽器：**500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-380">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span>

* <span data-ttu-id="ef0c1-381">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-381">**Application Log:** No entry</span></span>

* <span data-ttu-id="ef0c1-382">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="ef0c1-382">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="ef0c1-383">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-383">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-384">確認您已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-384">Confirm that you have enabled the proper role.</span></span> <span data-ttu-id="ef0c1-385">請參閱 [IIS 組態](#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-385">See [IIS Configuration](#iis-configuration).</span></span>

* <span data-ttu-id="ef0c1-386">請檢查 [程式和功能] 並確認 **Microsoft ASP.NET Core 模組** 已安裝。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-386">Check **Programs &amp; Features** and confirm that the **Microsoft ASP.NET Core Module** has been installed.</span></span> <span data-ttu-id="ef0c1-387">如果已安裝的程式清單中沒有 **Microsoft ASP.NET Core 模組**，請安裝此模組。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-387">If the **Microsoft ASP.NET Core Module** is not present in the list of installed programs, install the module.</span></span> <span data-ttu-id="ef0c1-388">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-388">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span>

* <span data-ttu-id="ef0c1-389">請確定 [應用程式集區] > [處理序模型] > [身分識別] 設為 [ApplicationPoolIdentity]，或您的自訂身分識別具有正確的權限，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-389">Make sure that the **Application Pool > Process Model > Identity** is set to **ApplicationPoolIdentity** or your custom identity has the correct permissions to access the application's deployment folder.</span></span>

### <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="ef0c1-390">不正確的 processPath, 遺失 PATH 變數, 未安裝裝載套件組合, 未重新啟動系統/IIS, 未安裝 VC++ 可轉散發套件, 或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="ef0c1-390">Incorrect processPath, missing PATH variable, hosting bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="ef0c1-391">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="ef0c1-391">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ef0c1-392">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '".\my_application.exe" ' 啟動程序，ErrorCode = '0x80070002 : 0。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-392">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '".\my_application.exe" ', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="ef0c1-393">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-393">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="ef0c1-394">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-394">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-395">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-395">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="ef0c1-396">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-396">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="ef0c1-397">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-397">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="ef0c1-398">請檢查 *web.config* 中 `<aspNetCore>` 元素的 *processPath* 屬性，以確認它是與 Framework 相依部署的 *dotnet* 或獨立部署的 *.\my_application.exe*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-398">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is *dotnet* for a framework-dependent deployment or *.\my_application.exe* for a self-contained deployment.</span></span>

* <span data-ttu-id="ef0c1-399">若為與 Framework 相依的部署，可能無法透過 PATH 設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-399">For a framework-dependent deployment, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="ef0c1-400">確認系統 PATH 設定中有 *C:\Program Files\dotnet\*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-400">Confirm that *C:\Program Files\dotnet\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="ef0c1-401">若為與 Framework 相依的部署，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-401">For a framework-dependent deployment, *dotnet.exe* might not be accessible for the user identity of the Application Pool.</span></span> <span data-ttu-id="ef0c1-402">確認 AppPool 使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-402">Confirm that the AppPool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="ef0c1-403">確認 *C:\Program Files\dotnet* 和應用程式目錄上的 AppPool 使用者身分識別未設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-403">Confirm that there are no deny rules configured for the AppPool user identity on the *C:\Program Files\dotnet* and application directories.</span></span>

* <span data-ttu-id="ef0c1-404">您可能已部署與 Framework 相依的部署，並在不重新啟動 IIS 的狀況下安裝了 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-404">You may have deployed a framework-dependent deployment and installed .NET Core without restarting IIS.</span></span> <span data-ttu-id="ef0c1-405">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-405">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ef0c1-406">您可能已部署與 Framework 相依的部署，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-406">You may have deployed a framework-dependent deployment without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="ef0c1-407">如果您要嘗試部署與 Framework 相依的部署，但尚未安裝 .NET Core 執行階段，請在系統上執行 **.NET Core Windows Server 裝載套件組合安裝程式**。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-407">If you're attempting to deploy a framework-dependent deployment and have not installed the .NET Core runtime, run the **.NET Core Windows Server Hosting bundle installer** on the system.</span></span> <span data-ttu-id="ef0c1-408">請參閱[安裝 .NET Core Windows Server 裝載套件組合](#install-the-net-core-windows-server-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-408">See [Install the .NET Core Windows Server Hosting bundle](#install-the-net-core-windows-server-hosting-bundle).</span></span> <span data-ttu-id="ef0c1-409">如果您要嘗試在沒有網際網路連線的系統上安裝 .NET Core 執行階段，請從 [.NET 下載](https://www.microsoft.com/net/download/core)取得執行階段，執行裝載套件組合安裝程式以安裝 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-409">If you are attempting to install the .NET Core runtime on a system without an Internet connection, obtain the runtime from [.NET Downloads](https://www.microsoft.com/net/download/core) and run the hosting bundle installer to install the ASP.NET Core Module.</span></span> <span data-ttu-id="ef0c1-410">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-410">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ef0c1-411">您可能已部署與 Framework 相依的部署，並在不重新啟動系統/IIS 的情況下安裝 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-411">You may have deployed a framework-dependent deployment and installed .NET Core without restarting the system/IIS.</span></span> <span data-ttu-id="ef0c1-412">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動系統或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-412">Either restart the system or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="ef0c1-413">您可能已部署與 Framework 相依的部署，但系統上未安裝 *Microsoft Visual C++ 2015 可轉散發套件 (x64)*。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-413">You may have deployed a framework-dependent deployment and the *Microsoft Visual C++ 2015 Redistributable (x64)* is not installed on the system.</span></span> <span data-ttu-id="ef0c1-414">您可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53840)取得安裝程式。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-414">You may obtain an installer from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53840).</span></span>

### <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="ef0c1-415">不正確的 \<aspNetCore\> 元素引數</span><span class="sxs-lookup"><span data-stu-id="ef0c1-415">Incorrect arguments of \<aspNetCore\> element</span></span>

* <span data-ttu-id="ef0c1-416">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="ef0c1-416">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ef0c1-417">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-417">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="ef0c1-418">**ASP.NET Core 模組記錄檔：**要執行的應用程式不存在：'PATH\my_application.dll'</span><span class="sxs-lookup"><span data-stu-id="ef0c1-418">**ASP.NET Core Module Log:** The application to execute does not exist: 'PATH\my_application.dll'</span></span>

<span data-ttu-id="ef0c1-419">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-419">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-420">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-420">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="ef0c1-421">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-421">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="ef0c1-422">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-422">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="ef0c1-423">檢查 *web.config* 中 `<aspNetCore>` 元素的 *arguments* 屬性，以確認它是 (a) 與 Framework 相依部署的 *.\my_application.dll*；或 (b) 不存在的空字串 (*arguments=""*)，或獨立部署的應用程式引數清單 (*arguments="arg1, arg2, ..."*)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-423">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it is either (a) *.\my_application.dll* for a framework-dependent deployment; or (b) not present, an empty string (*arguments=""*), or a list of your application's arguments (*arguments="arg1, arg2, ..."*) for a self-contained deployment.</span></span>

### <a name="missing-net-framework-version"></a><span data-ttu-id="ef0c1-424">遺失 .NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="ef0c1-424">Missing .NET Framework version</span></span>

* <span data-ttu-id="ef0c1-425">**瀏覽器：** 502.3 不正確的閘道 - 嘗試路由要求時發生連線錯誤。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-425">**Browser:** 502.3 Bad Gateway - There was a connection error while trying to route the request.</span></span>

* <span data-ttu-id="ef0c1-426">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' 無法以命令列 '"dotnet" .\my_application.dll' 啟動程序，ErrorCode = '0x80004005 : 80008081。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-426">**Application Log:** ErrorCode = Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' failed to start process with commandline '"dotnet" .\my_application.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="ef0c1-427">**ASP.NET Core 模組記錄檔：**遺失方法、檔案或組件例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-427">**ASP.NET Core Module Log:** Missing method, file, or assembly exception.</span></span> <span data-ttu-id="ef0c1-428">例外狀況中指定的方法、檔案或組件是 .NET Framework 方法、檔案或組件。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-428">The method, file, or assembly specified in the exception is a .NET Framework method, file, or assembly.</span></span>

<span data-ttu-id="ef0c1-429">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="ef0c1-429">Troubleshooting:</span></span>

* <span data-ttu-id="ef0c1-430">安裝系統中遺失的 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-430">Install the .NET Framework version missing from the system.</span></span>

* <span data-ttu-id="ef0c1-431">若為與 Framework 相依的部署，請確認已在系統上正確安裝執行階段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-431">For a framework-dependent deployment, confirm that you have the correct runtime installed on the system.</span></span> <span data-ttu-id="ef0c1-432">例如，如果您將專案從 1.0 升級為 1.1、部署到主控系統，然後收到這個例外狀況，請確定主控系統上安裝了 1.1 架構。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-432">For example if you upgrade a project from 1.0 to 1.1, deploy to the hosting system, and receive this exception, ensure you install the 1.1 framework on the hosting system.</span></span>

### <a name="stopped-application-pool"></a><span data-ttu-id="ef0c1-433">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="ef0c1-433">Stopped Application Pool</span></span>

* <span data-ttu-id="ef0c1-434">**瀏覽器：**503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="ef0c1-434">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="ef0c1-435">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-435">**Application Log:** No entry</span></span>

* <span data-ttu-id="ef0c1-436">**ASP.NET Core 模組記錄檔：**未建立記錄檔</span><span class="sxs-lookup"><span data-stu-id="ef0c1-436">**ASP.NET Core Module Log:** Log file not created</span></span>

<span data-ttu-id="ef0c1-437">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ef0c1-437">Troubleshooting</span></span>

* <span data-ttu-id="ef0c1-438">確認應用程式集區不是「已停止」狀態。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-438">Confirm that the Application Pool is not in the *Stopped* state.</span></span>

### <a name="iis-integration-middleware-not-implemented"></a><span data-ttu-id="ef0c1-439">未實作 IIS Integration 中介軟體</span><span class="sxs-lookup"><span data-stu-id="ef0c1-439">IIS Integration middleware not implemented</span></span>

* <span data-ttu-id="ef0c1-440">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="ef0c1-440">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ef0c1-441">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-441">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="ef0c1-442">**ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-442">**ASP.NET Core Module Log:** Log file created and shows normal operation.</span></span>

<span data-ttu-id="ef0c1-443">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ef0c1-443">Troubleshooting</span></span>

* <span data-ttu-id="ef0c1-444">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-444">Confirm that the application runs locally on Kestrel.</span></span> <span data-ttu-id="ef0c1-445">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-445">A process failure might be the result of a problem within the application.</span></span> <span data-ttu-id="ef0c1-446">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-446">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

* <span data-ttu-id="ef0c1-447">在應用程式的 *WebHostBuilder()* 呼叫 *.UseIISIntegration()* 方法，確認您已正確參考 IIS Integration 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-447">Confirm that you have correctly referenced the IIS Integration middleware by calling the *.UseIISIntegration()* method on the application's *WebHostBuilder()*.</span></span>

### <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="ef0c1-448">子應用程式包含\<處理常式\>區段</span><span class="sxs-lookup"><span data-stu-id="ef0c1-448">Sub-application includes a \<handlers\> section</span></span>

* <span data-ttu-id="ef0c1-449">**瀏覽器：**HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="ef0c1-449">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="ef0c1-450">**應用程式記錄檔：**無項目</span><span class="sxs-lookup"><span data-stu-id="ef0c1-450">**Application Log:** No entry</span></span>

* <span data-ttu-id="ef0c1-451">**ASP.NET Core 模組記錄檔：**記錄檔已建立並顯示根應用程式的一般作業。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-451">**ASP.NET Core Module Log:** Log file created and shows normal operation for the root application.</span></span> <span data-ttu-id="ef0c1-452">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-452">Log file not created for the sub-application.</span></span>

<span data-ttu-id="ef0c1-453">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ef0c1-453">Troubleshooting</span></span>

* <span data-ttu-id="ef0c1-454">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>`區段。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-454">Confirm that the sub-application's *web.config* file doesn't include a `<handlers>` section.</span></span>

### <a name="application-configuration-general-issue"></a><span data-ttu-id="ef0c1-455">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="ef0c1-455">Application configuration general issue</span></span>

* <span data-ttu-id="ef0c1-456">**瀏覽器：**HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="ef0c1-456">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="ef0c1-457">**應用程式記錄檔：**實體根目錄為 'C:\\{PATH}\' 的應用程式 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION'，使用命令列 '"C:\\{PATH}\my_application.{exe|dll}" ' 建立了程序，但可能當機、或沒有回應、或未接聽指定的連接埠 '{PORT}'，ErrorCode = '0x800705b4'。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-457">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/MY_APPLICATION' with physical root 'C:\\{PATH}\' created process with commandline '"C:\\{PATH}\my_application.{exe|dll}" ' but either crashed or did not reponse or did not listen on the given port '{PORT}', ErrorCode = '0x800705b4'</span></span>

* <span data-ttu-id="ef0c1-458">**ASP.NET Core 模組記錄檔：**已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-458">**ASP.NET Core Module Log:** Log file created but empty</span></span>

<span data-ttu-id="ef0c1-459">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ef0c1-459">Troubleshooting</span></span>

* <span data-ttu-id="ef0c1-460">這個一般的例外狀況指出程序無法啟動，最可能的原因是應用程式組態問題。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-460">This general exception indicates that the process failed to start, most likely due to an application configuration issue.</span></span> <span data-ttu-id="ef0c1-461">參考[目錄結構](xref:hosting/directory-structure)，確認應用程式部署的檔案和資料夾都是合適的，且應用程式組態檔都存在，並包含應用程式和環境的正確設定。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-461">Referring to [Directory Structure](xref:hosting/directory-structure), confirm that your application's deployed files and folders are appropriate and that your application's configuration files are present and contain the correct settings for your app and environment.</span></span> <span data-ttu-id="ef0c1-462">如需詳細資訊，請參閱[互通性的疑難排解](#troubleshooting-tips)。</span><span class="sxs-lookup"><span data-stu-id="ef0c1-462">For more information, see [Troubleshooting tips](#troubleshooting-tips).</span></span>

## <a name="resources"></a><span data-ttu-id="ef0c1-463">資源</span><span class="sxs-lookup"><span data-stu-id="ef0c1-463">Resources</span></span>

* [<span data-ttu-id="ef0c1-464">ASP.NET Core 模組簡介</span><span class="sxs-lookup"><span data-stu-id="ef0c1-464">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

* [<span data-ttu-id="ef0c1-465">ASP.NET Core 模組組態參考</span><span class="sxs-lookup"><span data-stu-id="ef0c1-465">ASP.NET Core Module configuration reference</span></span>](xref:hosting/aspnet-core-module)

* [<span data-ttu-id="ef0c1-466">使用 IIS 模組與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef0c1-466">Using IIS Modules with ASP.NET Core</span></span>](xref:hosting/iis-modules)

* [<span data-ttu-id="ef0c1-467">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="ef0c1-467">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="ef0c1-468">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="ef0c1-468">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)

* [<span data-ttu-id="ef0c1-469">Microsoft TechNet Library：Windows Server</span><span class="sxs-lookup"><span data-stu-id="ef0c1-469">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
