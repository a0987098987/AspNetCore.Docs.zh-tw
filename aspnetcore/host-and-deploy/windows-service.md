---
title: 在 Windows 服務上裝載 ASP.NET Core
author: rick-anderson
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: host-and-deploy/windows-service
ms.openlocfilehash: 5cb61d330df7e15fbd54396207792596ae018fd3
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417581"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="22fc7-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22fc7-103">Host ASP.NET Core in a Windows Service</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="22fc7-104">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="22fc7-104">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="22fc7-105">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="22fc7-105">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="22fc7-106">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22fc7-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22fc7-107">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="22fc7-107">Prerequisites</span></span>

* [<span data-ttu-id="22fc7-108">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-108">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="22fc7-109">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="worker-service-template"></a><span data-ttu-id="22fc7-110">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="22fc7-110">Worker Service template</span></span>

<span data-ttu-id="22fc7-111">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-111">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="22fc7-112">使用範本作為 Windows 服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="22fc7-112">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="22fc7-113">從 .NET Core 範本建立背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-113">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="22fc7-114">請遵循[應用程式組態](#app-configuration)一節中的指導方針，更新背景工作服務應用程式，以便其執行為 Windows 服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-114">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="app-configuration"></a><span data-ttu-id="22fc7-115">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="22fc7-115">App configuration</span></span>

<span data-ttu-id="22fc7-116">該應用程式需要[微軟的套件引用. 擴展.託管.Windows服務](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="22fc7-116">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="22fc7-117">`IHostBuilder.UseWindowsService`生成主機時調用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-117">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="22fc7-118">如果應用程式以 Windows 服務形式執行，則方法會：</span><span class="sxs-lookup"><span data-stu-id="22fc7-118">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="22fc7-119">將主機存留期設定為 `WindowsServiceLifetime`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-119">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="22fc7-120">將[內容根](xref:fundamentals/index#content-root)目錄設定為[AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-120">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="22fc7-121">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="22fc7-121">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="22fc7-122">開啟登入事件紀錄:</span><span class="sxs-lookup"><span data-stu-id="22fc7-122">Enables logging to the event log:</span></span>
  * <span data-ttu-id="22fc7-123">應用程式名稱用作預設源名稱。</span><span class="sxs-lookup"><span data-stu-id="22fc7-123">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="22fc7-124">默認日誌級別為 *「警告*」或更高,具體取決於`CreateDefaultBuilder`調用 生成主機ASP.NET核心範本的應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-124">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="22fc7-125">使用`Logging:EventLog:LogLevel:Default`*應用程式設定*/中鍵覆蓋預設紀錄等級 *。環境*.json\*或其他配置提供程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-125">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="22fc7-126">只有系統管理員才能建立新的事件來源。</span><span class="sxs-lookup"><span data-stu-id="22fc7-126">Only administrators can create new event sources.</span></span> <span data-ttu-id="22fc7-127">如果無法使用應用程式名稱建立事件來源，則會向「應用程式」\*\* 來源記錄警告，並停用事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-127">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="22fc7-128">Program.cs`CreateHostBuilder` *Program.cs*中 :</span><span class="sxs-lookup"><span data-stu-id="22fc7-128">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="22fc7-129">本主題附帶以下範例應用:</span><span class="sxs-lookup"><span data-stu-id="22fc7-129">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="22fc7-130">後台輔助服務範例&ndash;基於使用[託管服務](xref:fundamentals/host/hosted-services)執行後臺任務[的工作服務範本](#worker-service-template)的非 Web 應用範例。</span><span class="sxs-lookup"><span data-stu-id="22fc7-130">Background Worker Service Sample &ndash; A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="22fc7-131">Web 應用&ndash;服務 範例 Razor Pages Web 應用範例,該範例作為 Windows 服務運行,託管[服務](xref:fundamentals/host/hosted-services)用於後台任務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-131">Web App Service Sample &ndash; A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="22fc7-132">有關 MVC 指南,請<xref:mvc/overview>參<xref:migration/22-to-30>閱和下的文章。</span><span class="sxs-lookup"><span data-stu-id="22fc7-132">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

## <a name="deployment-type"></a><span data-ttu-id="22fc7-133">部署類型</span><span class="sxs-lookup"><span data-stu-id="22fc7-133">Deployment type</span></span>

<span data-ttu-id="22fc7-134">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-134">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="22fc7-135">SDK</span><span class="sxs-lookup"><span data-stu-id="22fc7-135">SDK</span></span>

<span data-ttu-id="22fc7-136">對於使用 Razor 頁面或 MVC 框架的 Web 應用的服務,請在專案檔中指定 Web SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-136">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="22fc7-137">如果服務僅執行後台工作(例如[託管服務](xref:fundamentals/host/hosted-services)),請在專案檔中指定輔助角色 SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-137">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="22fc7-138">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-138">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="22fc7-139">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="22fc7-139">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="22fc7-140">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-140">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="22fc7-141">如果使用[Web SDK](#sdk),則對於 Windows 服務應用來說,通常在發布 ASP.NET 酷睿應用時生成的*Web.config*檔是不必要的。</span><span class="sxs-lookup"><span data-stu-id="22fc7-141">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="22fc7-142">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-142">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="22fc7-143">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-143">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="22fc7-144">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="22fc7-144">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="22fc7-145">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="22fc7-145">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="22fc7-146">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="22fc7-146">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="22fc7-147">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="22fc7-147">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="22fc7-148">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="22fc7-148">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="22fc7-149">使用屬性名稱[\<「執行時識別符」>(](/dotnet/core/tools/csproj#runtimeidentifiers)複數)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-149">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="22fc7-150">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-150">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

## <a name="service-user-account"></a><span data-ttu-id="22fc7-151">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="22fc7-151">Service user account</span></span>

<span data-ttu-id="22fc7-152">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="22fc7-152">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="22fc7-153">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="22fc7-153">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-154">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="22fc7-154">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="22fc7-155">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-155">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="22fc7-156">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="22fc7-156">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="22fc7-157">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-157">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="22fc7-158">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-158">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="22fc7-159">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-159">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="22fc7-160">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="22fc7-160">Log on as a service rights</span></span>

<span data-ttu-id="22fc7-161">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="22fc7-161">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="22fc7-162">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="22fc7-162">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="22fc7-163">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-163">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="22fc7-164">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-164">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="22fc7-165">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-165">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="22fc7-166">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-166">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="22fc7-167">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-167">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="22fc7-168">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-168">Select **Advanced**.</span></span> <span data-ttu-id="22fc7-169">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-169">Select **Find Now**.</span></span> <span data-ttu-id="22fc7-170">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="22fc7-170">Select the user account from the list.</span></span> <span data-ttu-id="22fc7-171">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-171">Select **OK**.</span></span> <span data-ttu-id="22fc7-172">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-172">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="22fc7-173">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="22fc7-173">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="22fc7-174">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-174">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="22fc7-175">建立服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-175">Create a service</span></span>

<span data-ttu-id="22fc7-176">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-176">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="22fc7-177">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-177">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="22fc7-178">`{EXE PATH}`&ndash;主機上應用資料夾的路徑(例如。 `d:\myservice`</span><span class="sxs-lookup"><span data-stu-id="22fc7-178">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="22fc7-179">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-179">Don't include the app's executable in the path.</span></span> <span data-ttu-id="22fc7-180">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="22fc7-180">A trailing slash isn't required.</span></span>
* <span data-ttu-id="22fc7-181">`{DOMAIN OR COMPUTER NAME\USER}`&ndash;服務用戶帳戶(例如。 `Contoso\ServiceUser`</span><span class="sxs-lookup"><span data-stu-id="22fc7-181">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="22fc7-182">`{SERVICE NAME}`&ndash;服務名稱(例如。 `MyService`</span><span class="sxs-lookup"><span data-stu-id="22fc7-182">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="22fc7-183">`{EXE FILE PATH}`&ndash;應用的可執行路徑(例如。 `d:\myservice\myservice.exe`</span><span class="sxs-lookup"><span data-stu-id="22fc7-183">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="22fc7-184">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-184">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="22fc7-185">`{DESCRIPTION}`&ndash;服務描述(例如。 `My sample service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-185">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="22fc7-186">`{DISPLAY NAME}`&ndash;服務顯示名稱(例如, `My Service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-186">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="22fc7-187">啟動服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-187">Start a service</span></span>

<span data-ttu-id="22fc7-188">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-188">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-189">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-189">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="22fc7-190">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="22fc7-190">Determine a service's status</span></span>

<span data-ttu-id="22fc7-191">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-191">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-192">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="22fc7-192">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="22fc7-193">停止服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-193">Stop a service</span></span>

<span data-ttu-id="22fc7-194">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-194">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="22fc7-195">移除服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-195">Remove a service</span></span>

<span data-ttu-id="22fc7-196">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-196">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="22fc7-197">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="22fc7-197">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="22fc7-198">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="22fc7-198">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="22fc7-199">如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-199">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="22fc7-200">設定端點</span><span class="sxs-lookup"><span data-stu-id="22fc7-200">Configure endpoints</span></span>

<span data-ttu-id="22fc7-201">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-201">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="22fc7-202">通過設置環境變數配置`ASPNETCORE_URLS`URL 和埠。</span><span class="sxs-lookup"><span data-stu-id="22fc7-202">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="22fc7-203">有關其他網址和埠配置方法,請參閱相關伺服器文章:</span><span class="sxs-lookup"><span data-stu-id="22fc7-203">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="22fc7-204">前面的指南介紹了對 HTTPS 終結點的支援。</span><span class="sxs-lookup"><span data-stu-id="22fc7-204">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="22fc7-205">例如,在與 Windows 服務一起使用身份驗證時,為 HTTPS 配置應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-205">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="22fc7-206">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-206">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="22fc7-207">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-207">Current directory and content root</span></span>

<span data-ttu-id="22fc7-208">通過調用<xref:System.IO.Directory.GetCurrentDirectory*>Windows 服務返回的當前工作目錄是*C:\\\\WINDOWS 系統32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-208">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="22fc7-209">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-209">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="22fc7-210">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-210">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="22fc7-211">使用 ContentRootPath 或 ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="22fc7-211">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="22fc7-212">使用 [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) 或 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> 來找出應用程式的資源。</span><span class="sxs-lookup"><span data-stu-id="22fc7-212">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="22fc7-213">當應用程式作為服務執行時,<xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*><xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath>將設定為[AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-213">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="22fc7-214">應用的預設設定檔、*應用設置.json*和應用*設置。環境_.json*,通過在[主機構造期間調用 CreateDefaultBuilder](xref:fundamentals/host/generic-host#set-up-a-host)從應用的內容根載入。</span><span class="sxs-lookup"><span data-stu-id="22fc7-214">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="22fc7-215">此設定檔,無需呼<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>叫<xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-215">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="22fc7-216">在下面的範例中 *,custom_settings.json*檔存在於應用的內容根中,並且載入時沒有顯式設置基本路徑:</span><span class="sxs-lookup"><span data-stu-id="22fc7-216">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="22fc7-217">不要嘗試使用<xref:System.IO.Directory.GetCurrentDirectory*>以獲取資源路徑,因為 Windows 服務應用將*C:\\WINDOWS\\系統 32*資料夾作為當前目錄。</span><span class="sxs-lookup"><span data-stu-id="22fc7-217">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="22fc7-218">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="22fc7-218">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="22fc7-219">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-219">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="22fc7-220">疑難排解</span><span class="sxs-lookup"><span data-stu-id="22fc7-220">Troubleshoot</span></span>

<span data-ttu-id="22fc7-221">要對 Windows 服務<xref:test/troubleshoot>應用進行故障排除,請參閱。</span><span class="sxs-lookup"><span data-stu-id="22fc7-221">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="22fc7-222">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="22fc7-222">Common errors</span></span>

* <span data-ttu-id="22fc7-223">正在使用舊版或預發行版本的 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="22fc7-223">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="22fc7-224">已註冊的服務不使用來自[dotnet 發佈](/dotnet/core/tools/dotnet-publish)命令的應用**已發布**的 輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-224">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="22fc7-225">應用部署不支援[dotnet 生成](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-225">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="22fc7-226">根據部署類型,在以下任一檔夾中找到已發佈的資產:</span><span class="sxs-lookup"><span data-stu-id="22fc7-226">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="22fc7-227">*bin/釋放/[目標框架]/發佈*(FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-227">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="22fc7-228">*箱/發佈/[目標框架]/[運行時間標識符]/發佈*(SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-228">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="22fc7-229">服務未處於"運行"狀態。</span><span class="sxs-lookup"><span data-stu-id="22fc7-229">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="22fc7-230">應用使用的資源(例如證書)的路徑不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-230">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="22fc7-231">Windows 服務的基本路徑是*\\c:\\Windows System32*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-231">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="22fc7-232">用戶沒有 *「作為服務許可權登錄*」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-232">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="22fc7-233">執行`New-Service`PowerShell 命令時,使用者的密碼已過期或傳遞不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-233">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="22fc7-234">應用需要ASP.NET核心身份驗證,但未配置為安全連接 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-234">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="22fc7-235">請求 URL 連接埠不正確或未在應用中正確配置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-235">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="22fc7-236">系統與應用程式事件紀錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-236">System and Application Event Logs</span></span>

<span data-ttu-id="22fc7-237">存取系統與應用程式事件紀錄:</span><span class="sxs-lookup"><span data-stu-id="22fc7-237">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="22fc7-238">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-238">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="22fc7-239">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-239">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="22fc7-240">選擇**系統**以打開系統事件日誌。</span><span class="sxs-lookup"><span data-stu-id="22fc7-240">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="22fc7-241">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-241">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="22fc7-242">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="22fc7-242">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="22fc7-243">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-243">Run the app at a command prompt</span></span>

<span data-ttu-id="22fc7-244">許多啟動錯誤在事件日誌中不生成有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="22fc7-244">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="22fc7-245">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-245">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="22fc7-246">要從應用中記錄其他詳細資訊,請降低[日誌級別](xref:fundamentals/logging/index#log-level)或在[開發環境中](xref:fundamentals/environments)運行應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-246">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="22fc7-247">清除包快取</span><span class="sxs-lookup"><span data-stu-id="22fc7-247">Clear package caches</span></span>

<span data-ttu-id="22fc7-248">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="22fc7-248">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="22fc7-249">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-249">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="22fc7-250">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="22fc7-250">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="22fc7-251">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-251">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="22fc7-252">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="22fc7-252">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="22fc7-253">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="22fc7-253">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="22fc7-254">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="22fc7-254">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="22fc7-255">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-255">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="22fc7-256">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-256">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="22fc7-257">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-257">Slow or hanging app</span></span>

<span data-ttu-id="22fc7-258">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-258">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="22fc7-259">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="22fc7-259">App crashes or encounters an exception</span></span>

<span data-ttu-id="22fc7-260">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="22fc7-260">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="22fc7-261">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-261">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="22fc7-262">使用應用程式執行名稱執行[啟用傾印 PowerShell 文稿](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="22fc7-262">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```powershell
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="22fc7-263">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-263">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="22fc7-264">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-264">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/DisableDumps.ps1):</span></span>

   ```powershell
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="22fc7-265">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-265">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="22fc7-266">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-266">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="22fc7-267">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-267">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="22fc7-268">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="22fc7-268">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="22fc7-269">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="22fc7-269">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="22fc7-270">分析傾印</span><span class="sxs-lookup"><span data-stu-id="22fc7-270">Analyze the dump</span></span>

<span data-ttu-id="22fc7-271">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-271">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="22fc7-272">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-272">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22fc7-273">其他資源</span><span class="sxs-lookup"><span data-stu-id="22fc7-273">Additional resources</span></span>

* <span data-ttu-id="22fc7-274">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="22fc7-274">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="22fc7-275">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="22fc7-275">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="22fc7-276">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="22fc7-276">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="22fc7-277">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22fc7-277">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22fc7-278">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="22fc7-278">Prerequisites</span></span>

* [<span data-ttu-id="22fc7-279">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-279">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="22fc7-280">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-280">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="22fc7-281">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="22fc7-281">App configuration</span></span>

<span data-ttu-id="22fc7-282">應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="22fc7-282">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="22fc7-283">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="22fc7-283">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="22fc7-284">檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。</span><span class="sxs-lookup"><span data-stu-id="22fc7-284">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="22fc7-285">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-285">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="22fc7-286">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-286">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="22fc7-287">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-287">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="22fc7-288">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-288">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="22fc7-289">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="22fc7-289">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="22fc7-290">在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="22fc7-290">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="22fc7-291">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-291">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="22fc7-292">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。</span><span class="sxs-lookup"><span data-stu-id="22fc7-292">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="22fc7-293">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-293">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="22fc7-294">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="22fc7-294">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="22fc7-295">在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-295">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="22fc7-296">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="22fc7-296">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="22fc7-297">部署類型</span><span class="sxs-lookup"><span data-stu-id="22fc7-297">Deployment type</span></span>

<span data-ttu-id="22fc7-298">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-298">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="22fc7-299">SDK</span><span class="sxs-lookup"><span data-stu-id="22fc7-299">SDK</span></span>

<span data-ttu-id="22fc7-300">對於使用 Razor 頁面或 MVC 框架的 Web 應用的服務,請在專案檔中指定 Web SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-300">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="22fc7-301">如果服務僅執行後台工作(例如[託管服務](xref:fundamentals/host/hosted-services)),請在專案檔中指定輔助角色 SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-301">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="22fc7-302">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-302">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="22fc7-303">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="22fc7-303">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="22fc7-304">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-304">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="22fc7-305">Windows[執行時識別碼 (RID)](/dotnet/core/rid-catalog) ([\<執行時識別碼>)](/dotnet/core/tools/csproj#runtimeidentifier)包含目標框架。</span><span class="sxs-lookup"><span data-stu-id="22fc7-305">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="22fc7-306">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-306">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="22fc7-307">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-307">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="22fc7-308">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-308">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="22fc7-309">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-309">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="22fc7-310">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-310">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="22fc7-311">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-311">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="22fc7-312">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="22fc7-312">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="22fc7-313">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="22fc7-313">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="22fc7-314">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="22fc7-314">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="22fc7-315">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="22fc7-315">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="22fc7-316">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="22fc7-316">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="22fc7-317">使用屬性名稱[\<「執行時識別符」>(](/dotnet/core/tools/csproj#runtimeidentifiers)複數)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-317">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="22fc7-318">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-318">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="22fc7-319">`<SelfContained>` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="22fc7-319">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="22fc7-320">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="22fc7-320">Service user account</span></span>

<span data-ttu-id="22fc7-321">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="22fc7-321">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="22fc7-322">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="22fc7-322">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-323">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="22fc7-323">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="22fc7-324">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-324">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="22fc7-325">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="22fc7-325">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="22fc7-326">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-326">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="22fc7-327">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-327">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="22fc7-328">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-328">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="22fc7-329">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="22fc7-329">Log on as a service rights</span></span>

<span data-ttu-id="22fc7-330">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="22fc7-330">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="22fc7-331">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="22fc7-331">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="22fc7-332">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-332">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="22fc7-333">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-333">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="22fc7-334">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-334">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="22fc7-335">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-335">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="22fc7-336">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-336">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="22fc7-337">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-337">Select **Advanced**.</span></span> <span data-ttu-id="22fc7-338">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-338">Select **Find Now**.</span></span> <span data-ttu-id="22fc7-339">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="22fc7-339">Select the user account from the list.</span></span> <span data-ttu-id="22fc7-340">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-340">Select **OK**.</span></span> <span data-ttu-id="22fc7-341">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-341">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="22fc7-342">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="22fc7-342">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="22fc7-343">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-343">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="22fc7-344">建立服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-344">Create a service</span></span>

<span data-ttu-id="22fc7-345">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-345">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="22fc7-346">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-346">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="22fc7-347">`{EXE PATH}`&ndash;主機上應用資料夾的路徑(例如。 `d:\myservice`</span><span class="sxs-lookup"><span data-stu-id="22fc7-347">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="22fc7-348">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-348">Don't include the app's executable in the path.</span></span> <span data-ttu-id="22fc7-349">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="22fc7-349">A trailing slash isn't required.</span></span>
* <span data-ttu-id="22fc7-350">`{DOMAIN OR COMPUTER NAME\USER}`&ndash;服務用戶帳戶(例如。 `Contoso\ServiceUser`</span><span class="sxs-lookup"><span data-stu-id="22fc7-350">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="22fc7-351">`{SERVICE NAME}`&ndash;服務名稱(例如。 `MyService`</span><span class="sxs-lookup"><span data-stu-id="22fc7-351">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="22fc7-352">`{EXE FILE PATH}`&ndash;應用的可執行路徑(例如。 `d:\myservice\myservice.exe`</span><span class="sxs-lookup"><span data-stu-id="22fc7-352">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="22fc7-353">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-353">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="22fc7-354">`{DESCRIPTION}`&ndash;服務描述(例如。 `My sample service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-354">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="22fc7-355">`{DISPLAY NAME}`&ndash;服務顯示名稱(例如, `My Service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-355">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="22fc7-356">啟動服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-356">Start a service</span></span>

<span data-ttu-id="22fc7-357">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-357">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-358">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-358">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="22fc7-359">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="22fc7-359">Determine a service's status</span></span>

<span data-ttu-id="22fc7-360">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-360">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-361">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="22fc7-361">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="22fc7-362">停止服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-362">Stop a service</span></span>

<span data-ttu-id="22fc7-363">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-363">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="22fc7-364">移除服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-364">Remove a service</span></span>

<span data-ttu-id="22fc7-365">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-365">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="22fc7-366">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="22fc7-366">Handle starting and stopping events</span></span>

<span data-ttu-id="22fc7-367">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：</span><span class="sxs-lookup"><span data-stu-id="22fc7-367">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="22fc7-368">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="22fc7-368">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="22fc7-369">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="22fc7-369">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="22fc7-370">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="22fc7-370">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="22fc7-371">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="22fc7-371">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="22fc7-372">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="22fc7-372">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="22fc7-373">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="22fc7-373">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="22fc7-374">如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-374">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="22fc7-375">設定端點</span><span class="sxs-lookup"><span data-stu-id="22fc7-375">Configure endpoints</span></span>

<span data-ttu-id="22fc7-376">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-376">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="22fc7-377">通過設置環境變數配置`ASPNETCORE_URLS`URL 和埠。</span><span class="sxs-lookup"><span data-stu-id="22fc7-377">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="22fc7-378">有關其他網址和埠配置方法,請參閱相關伺服器文章:</span><span class="sxs-lookup"><span data-stu-id="22fc7-378">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="22fc7-379">前面的指南介紹了對 HTTPS 終結點的支援。</span><span class="sxs-lookup"><span data-stu-id="22fc7-379">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="22fc7-380">例如,在與 Windows 服務一起使用身份驗證時,為 HTTPS 配置應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-380">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="22fc7-381">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-381">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="22fc7-382">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-382">Current directory and content root</span></span>

<span data-ttu-id="22fc7-383">通過調用<xref:System.IO.Directory.GetCurrentDirectory*>Windows 服務返回的當前工作目錄是*C:\\\\WINDOWS 系統32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-383">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="22fc7-384">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-384">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="22fc7-385">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-385">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="22fc7-386">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="22fc7-386">Set the content root path to the app's folder</span></span>

<span data-ttu-id="22fc7-387"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="22fc7-387">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="22fc7-388">呼叫應用`GetCurrentDirectory`[的內容根](xref:fundamentals/index#content-root)的路徑不是呼叫以創建設定檔<xref:System.IO.Directory.SetCurrentDirectory*>的路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-388">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="22fc7-389">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="22fc7-389">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="22fc7-390">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="22fc7-390">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="22fc7-391">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-391">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="22fc7-392">疑難排解</span><span class="sxs-lookup"><span data-stu-id="22fc7-392">Troubleshoot</span></span>

<span data-ttu-id="22fc7-393">要對 Windows 服務<xref:test/troubleshoot>應用進行故障排除,請參閱。</span><span class="sxs-lookup"><span data-stu-id="22fc7-393">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="22fc7-394">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="22fc7-394">Common errors</span></span>

* <span data-ttu-id="22fc7-395">正在使用舊版或預發行版本的 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="22fc7-395">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="22fc7-396">已註冊的服務不使用來自[dotnet 發佈](/dotnet/core/tools/dotnet-publish)命令的應用**已發布**的 輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-396">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="22fc7-397">應用部署不支援[dotnet 生成](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-397">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="22fc7-398">根據部署類型,在以下任一檔夾中找到已發佈的資產:</span><span class="sxs-lookup"><span data-stu-id="22fc7-398">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="22fc7-399">*bin/釋放/[目標框架]/發佈*(FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-399">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="22fc7-400">*箱/發佈/[目標框架]/[運行時間標識符]/發佈*(SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-400">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="22fc7-401">服務未處於"運行"狀態。</span><span class="sxs-lookup"><span data-stu-id="22fc7-401">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="22fc7-402">應用使用的資源(例如證書)的路徑不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-402">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="22fc7-403">Windows 服務的基本路徑是*\\c:\\Windows System32*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-403">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="22fc7-404">用戶沒有 *「作為服務許可權登錄*」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-404">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="22fc7-405">執行`New-Service`PowerShell 命令時,使用者的密碼已過期或傳遞不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-405">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="22fc7-406">應用需要ASP.NET核心身份驗證,但未配置為安全連接 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-406">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="22fc7-407">請求 URL 連接埠不正確或未在應用中正確配置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-407">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="22fc7-408">系統與應用程式事件紀錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-408">System and Application Event Logs</span></span>

<span data-ttu-id="22fc7-409">存取系統與應用程式事件紀錄:</span><span class="sxs-lookup"><span data-stu-id="22fc7-409">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="22fc7-410">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-410">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="22fc7-411">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-411">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="22fc7-412">選擇**系統**以打開系統事件日誌。</span><span class="sxs-lookup"><span data-stu-id="22fc7-412">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="22fc7-413">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-413">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="22fc7-414">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="22fc7-414">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="22fc7-415">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-415">Run the app at a command prompt</span></span>

<span data-ttu-id="22fc7-416">許多啟動錯誤在事件日誌中不生成有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="22fc7-416">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="22fc7-417">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-417">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="22fc7-418">要從應用中記錄其他詳細資訊,請降低[日誌級別](xref:fundamentals/logging/index#log-level)或在[開發環境中](xref:fundamentals/environments)運行應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-418">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="22fc7-419">清除包快取</span><span class="sxs-lookup"><span data-stu-id="22fc7-419">Clear package caches</span></span>

<span data-ttu-id="22fc7-420">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="22fc7-420">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="22fc7-421">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-421">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="22fc7-422">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="22fc7-422">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="22fc7-423">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-423">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="22fc7-424">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="22fc7-424">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="22fc7-425">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="22fc7-425">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="22fc7-426">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="22fc7-426">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="22fc7-427">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-427">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="22fc7-428">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-428">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="22fc7-429">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-429">Slow or hanging app</span></span>

<span data-ttu-id="22fc7-430">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-430">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="22fc7-431">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="22fc7-431">App crashes or encounters an exception</span></span>

<span data-ttu-id="22fc7-432">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="22fc7-432">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="22fc7-433">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-433">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="22fc7-434">使用應用程式執行名稱執行[啟用傾印 PowerShell 文稿](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="22fc7-434">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="22fc7-435">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-435">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="22fc7-436">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-436">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="22fc7-437">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-437">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="22fc7-438">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-438">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="22fc7-439">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-439">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="22fc7-440">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="22fc7-440">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="22fc7-441">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="22fc7-441">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="22fc7-442">分析傾印</span><span class="sxs-lookup"><span data-stu-id="22fc7-442">Analyze the dump</span></span>

<span data-ttu-id="22fc7-443">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-443">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="22fc7-444">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-444">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22fc7-445">其他資源</span><span class="sxs-lookup"><span data-stu-id="22fc7-445">Additional resources</span></span>

* <span data-ttu-id="22fc7-446">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="22fc7-446">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="22fc7-447">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="22fc7-447">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="22fc7-448">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="22fc7-448">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="22fc7-449">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22fc7-449">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22fc7-450">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="22fc7-450">Prerequisites</span></span>

* [<span data-ttu-id="22fc7-451">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-451">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="22fc7-452">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="22fc7-452">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="22fc7-453">應用程式設定</span><span class="sxs-lookup"><span data-stu-id="22fc7-453">App configuration</span></span>

<span data-ttu-id="22fc7-454">應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="22fc7-454">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="22fc7-455">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="22fc7-455">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="22fc7-456">檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。</span><span class="sxs-lookup"><span data-stu-id="22fc7-456">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="22fc7-457">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-457">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="22fc7-458">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-458">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="22fc7-459">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-459">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="22fc7-460">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-460">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="22fc7-461">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="22fc7-461">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="22fc7-462">在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="22fc7-462">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="22fc7-463">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-463">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="22fc7-464">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。</span><span class="sxs-lookup"><span data-stu-id="22fc7-464">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="22fc7-465">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-465">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="22fc7-466">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="22fc7-466">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="22fc7-467">在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-467">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="22fc7-468">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="22fc7-468">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="22fc7-469">部署類型</span><span class="sxs-lookup"><span data-stu-id="22fc7-469">Deployment type</span></span>

<span data-ttu-id="22fc7-470">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-470">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="22fc7-471">SDK</span><span class="sxs-lookup"><span data-stu-id="22fc7-471">SDK</span></span>

<span data-ttu-id="22fc7-472">對於使用 Razor 頁面或 MVC 框架的 Web 應用的服務,請在專案檔中指定 Web SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-472">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="22fc7-473">如果服務僅執行後台工作(例如[託管服務](xref:fundamentals/host/hosted-services)),請在專案檔中指定輔助角色 SDK:</span><span class="sxs-lookup"><span data-stu-id="22fc7-473">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="22fc7-474">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-474">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="22fc7-475">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="22fc7-475">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="22fc7-476">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-476">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="22fc7-477">Windows[執行時識別碼 (RID)](/dotnet/core/rid-catalog) ([\<執行時識別碼>)](/dotnet/core/tools/csproj#runtimeidentifier)包含目標框架。</span><span class="sxs-lookup"><span data-stu-id="22fc7-477">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="22fc7-478">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-478">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="22fc7-479">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-479">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="22fc7-480">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-480">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="22fc7-481">`<UseAppHost>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-481">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="22fc7-482">此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-482">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="22fc7-483">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-483">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="22fc7-484">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-484">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="22fc7-485">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-485">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="22fc7-486">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="22fc7-486">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="22fc7-487">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="22fc7-487">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="22fc7-488">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="22fc7-488">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="22fc7-489">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="22fc7-489">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="22fc7-490">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="22fc7-490">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="22fc7-491">使用屬性名稱[\<「執行時識別符」>(](/dotnet/core/tools/csproj#runtimeidentifiers)複數)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-491">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="22fc7-492">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-492">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="22fc7-493">`<SelfContained>` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="22fc7-493">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="22fc7-494">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="22fc7-494">Service user account</span></span>

<span data-ttu-id="22fc7-495">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="22fc7-495">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="22fc7-496">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="22fc7-496">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-497">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="22fc7-497">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="22fc7-498">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-498">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="22fc7-499">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="22fc7-499">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="22fc7-500">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-500">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="22fc7-501">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-501">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="22fc7-502">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-502">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="22fc7-503">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="22fc7-503">Log on as a service rights</span></span>

<span data-ttu-id="22fc7-504">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="22fc7-504">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="22fc7-505">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="22fc7-505">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="22fc7-506">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-506">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="22fc7-507">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-507">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="22fc7-508">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-508">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="22fc7-509">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-509">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="22fc7-510">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-510">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="22fc7-511">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-511">Select **Advanced**.</span></span> <span data-ttu-id="22fc7-512">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-512">Select **Find Now**.</span></span> <span data-ttu-id="22fc7-513">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="22fc7-513">Select the user account from the list.</span></span> <span data-ttu-id="22fc7-514">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="22fc7-514">Select **OK**.</span></span> <span data-ttu-id="22fc7-515">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="22fc7-515">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="22fc7-516">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="22fc7-516">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="22fc7-517">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-517">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="22fc7-518">建立服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-518">Create a service</span></span>

<span data-ttu-id="22fc7-519">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-519">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="22fc7-520">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-520">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="22fc7-521">`{EXE PATH}`&ndash;主機上應用資料夾的路徑(例如。 `d:\myservice`</span><span class="sxs-lookup"><span data-stu-id="22fc7-521">`{EXE PATH}` &ndash; Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="22fc7-522">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-522">Don't include the app's executable in the path.</span></span> <span data-ttu-id="22fc7-523">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="22fc7-523">A trailing slash isn't required.</span></span>
* <span data-ttu-id="22fc7-524">`{DOMAIN OR COMPUTER NAME\USER}`&ndash;服務用戶帳戶(例如。 `Contoso\ServiceUser`</span><span class="sxs-lookup"><span data-stu-id="22fc7-524">`{DOMAIN OR COMPUTER NAME\USER}` &ndash; Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="22fc7-525">`{SERVICE NAME}`&ndash;服務名稱(例如。 `MyService`</span><span class="sxs-lookup"><span data-stu-id="22fc7-525">`{SERVICE NAME}` &ndash; Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="22fc7-526">`{EXE FILE PATH}`&ndash;應用的可執行路徑(例如。 `d:\myservice\myservice.exe`</span><span class="sxs-lookup"><span data-stu-id="22fc7-526">`{EXE FILE PATH}` &ndash; The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="22fc7-527">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-527">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="22fc7-528">`{DESCRIPTION}`&ndash;服務描述(例如。 `My sample service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-528">`{DESCRIPTION}` &ndash; Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="22fc7-529">`{DISPLAY NAME}`&ndash;服務顯示名稱(例如, `My Service`</span><span class="sxs-lookup"><span data-stu-id="22fc7-529">`{DISPLAY NAME}` &ndash; Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="22fc7-530">啟動服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-530">Start a service</span></span>

<span data-ttu-id="22fc7-531">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-531">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-532">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="22fc7-532">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="22fc7-533">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="22fc7-533">Determine a service's status</span></span>

<span data-ttu-id="22fc7-534">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="22fc7-534">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="22fc7-535">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="22fc7-535">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="22fc7-536">停止服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-536">Stop a service</span></span>

<span data-ttu-id="22fc7-537">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-537">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="22fc7-538">移除服務</span><span class="sxs-lookup"><span data-stu-id="22fc7-538">Remove a service</span></span>

<span data-ttu-id="22fc7-539">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="22fc7-539">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="22fc7-540">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="22fc7-540">Handle starting and stopping events</span></span>

<span data-ttu-id="22fc7-541">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：</span><span class="sxs-lookup"><span data-stu-id="22fc7-541">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="22fc7-542">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="22fc7-542">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="22fc7-543">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="22fc7-543">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="22fc7-544">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="22fc7-544">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="22fc7-545">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="22fc7-545">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="22fc7-546">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="22fc7-546">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="22fc7-547">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="22fc7-547">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="22fc7-548">如需詳細資訊，請參閱 <xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="22fc7-548">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="22fc7-549">設定端點</span><span class="sxs-lookup"><span data-stu-id="22fc7-549">Configure endpoints</span></span>

<span data-ttu-id="22fc7-550">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="22fc7-550">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="22fc7-551">通過設置環境變數配置`ASPNETCORE_URLS`URL 和埠。</span><span class="sxs-lookup"><span data-stu-id="22fc7-551">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="22fc7-552">有關其他網址和埠配置方法,請參閱相關伺服器文章:</span><span class="sxs-lookup"><span data-stu-id="22fc7-552">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="22fc7-553">前面的指南介紹了對 HTTPS 終結點的支援。</span><span class="sxs-lookup"><span data-stu-id="22fc7-553">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="22fc7-554">例如,在與 Windows 服務一起使用身份驗證時,為 HTTPS 配置應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-554">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="22fc7-555">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-555">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="22fc7-556">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-556">Current directory and content root</span></span>

<span data-ttu-id="22fc7-557">通過調用<xref:System.IO.Directory.GetCurrentDirectory*>Windows 服務返回的當前工作目錄是*C:\\\\WINDOWS 系統32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-557">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="22fc7-558">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-558">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="22fc7-559">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-559">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="22fc7-560">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="22fc7-560">Set the content root path to the app's folder</span></span>

<span data-ttu-id="22fc7-561"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="22fc7-561">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="22fc7-562">呼叫應用`GetCurrentDirectory`[的內容根](xref:fundamentals/index#content-root)的路徑不是呼叫以創建設定檔<xref:System.IO.Directory.SetCurrentDirectory*>的路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-562">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="22fc7-563">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="22fc7-563">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="22fc7-564">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="22fc7-564">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="22fc7-565">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="22fc7-565">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="22fc7-566">疑難排解</span><span class="sxs-lookup"><span data-stu-id="22fc7-566">Troubleshoot</span></span>

<span data-ttu-id="22fc7-567">要對 Windows 服務<xref:test/troubleshoot>應用進行故障排除,請參閱。</span><span class="sxs-lookup"><span data-stu-id="22fc7-567">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="22fc7-568">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="22fc7-568">Common errors</span></span>

* <span data-ttu-id="22fc7-569">正在使用舊版或預發行版本的 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="22fc7-569">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="22fc7-570">已註冊的服務不使用來自[dotnet 發佈](/dotnet/core/tools/dotnet-publish)命令的應用**已發布**的 輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-570">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="22fc7-571">應用部署不支援[dotnet 生成](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="22fc7-571">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="22fc7-572">根據部署類型,在以下任一檔夾中找到已發佈的資產:</span><span class="sxs-lookup"><span data-stu-id="22fc7-572">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="22fc7-573">*bin/釋放/[目標框架]/發佈*(FDD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-573">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="22fc7-574">*箱/發佈/[目標框架]/[運行時間標識符]/發佈*(SCD)</span><span class="sxs-lookup"><span data-stu-id="22fc7-574">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="22fc7-575">服務未處於"運行"狀態。</span><span class="sxs-lookup"><span data-stu-id="22fc7-575">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="22fc7-576">應用使用的資源(例如證書)的路徑不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-576">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="22fc7-577">Windows 服務的基本路徑是*\\c:\\Windows System32*。</span><span class="sxs-lookup"><span data-stu-id="22fc7-577">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="22fc7-578">用戶沒有 *「作為服務許可權登錄*」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-578">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="22fc7-579">執行`New-Service`PowerShell 命令時,使用者的密碼已過期或傳遞不正確。</span><span class="sxs-lookup"><span data-stu-id="22fc7-579">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="22fc7-580">應用需要ASP.NET核心身份驗證,但未配置為安全連接 (HTTPS)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-580">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="22fc7-581">請求 URL 連接埠不正確或未在應用中正確配置。</span><span class="sxs-lookup"><span data-stu-id="22fc7-581">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="22fc7-582">系統與應用程式事件紀錄</span><span class="sxs-lookup"><span data-stu-id="22fc7-582">System and Application Event Logs</span></span>

<span data-ttu-id="22fc7-583">存取系統與應用程式事件紀錄:</span><span class="sxs-lookup"><span data-stu-id="22fc7-583">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="22fc7-584">打開"開始"功能表,搜索*事件查看器*,然後選擇 **「事件查看器」** 應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-584">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="22fc7-585">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="22fc7-585">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="22fc7-586">選擇**系統**以打開系統事件日誌。</span><span class="sxs-lookup"><span data-stu-id="22fc7-586">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="22fc7-587">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="22fc7-587">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="22fc7-588">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="22fc7-588">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="22fc7-589">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-589">Run the app at a command prompt</span></span>

<span data-ttu-id="22fc7-590">許多啟動錯誤在事件日誌中不生成有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="22fc7-590">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="22fc7-591">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-591">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="22fc7-592">要從應用中記錄其他詳細資訊,請降低[日誌級別](xref:fundamentals/logging/index#log-level)或在[開發環境中](xref:fundamentals/environments)運行應用。</span><span class="sxs-lookup"><span data-stu-id="22fc7-592">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="22fc7-593">清除包快取</span><span class="sxs-lookup"><span data-stu-id="22fc7-593">Clear package caches</span></span>

<span data-ttu-id="22fc7-594">在升級開發電腦上的 .NET Core SDK 或更改應用中的包版本後,正常運行的應用可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="22fc7-594">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="22fc7-595">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-595">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="22fc7-596">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="22fc7-596">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="22fc7-597">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="22fc7-597">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="22fc7-598">通過執行[dotnet nuget 本地變數](/dotnet/core/tools/dotnet-nuget-locals)來清除包快取 -- 從命令 shell 中清除。</span><span class="sxs-lookup"><span data-stu-id="22fc7-598">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="22fc7-599">清除包快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具`nuget locals all -clear`執行命令 。</span><span class="sxs-lookup"><span data-stu-id="22fc7-599">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="22fc7-600">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="22fc7-600">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="22fc7-601">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-601">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="22fc7-602">在重新部署應用之前,請刪除伺服器上的部署資料夾中的所有檔。</span><span class="sxs-lookup"><span data-stu-id="22fc7-602">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="22fc7-603">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="22fc7-603">Slow or hanging app</span></span>

<span data-ttu-id="22fc7-604">*崩潰轉儲*是系統記憶體的快照,可幫助確定應用崩潰、啟動失敗或應用慢速的原因。</span><span class="sxs-lookup"><span data-stu-id="22fc7-604">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="22fc7-605">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="22fc7-605">App crashes or encounters an exception</span></span>

<span data-ttu-id="22fc7-606">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="22fc7-606">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="22fc7-607">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="22fc7-607">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="22fc7-608">使用應用程式執行名稱執行[啟用傾印 PowerShell 文稿](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="22fc7-608">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="22fc7-609">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-609">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="22fc7-610">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="22fc7-610">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="22fc7-611">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="22fc7-611">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="22fc7-612">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-612">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="22fc7-613">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-613">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="22fc7-614">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="22fc7-614">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="22fc7-615">當應用*掛起*(停止回應但不崩潰)、在啟動期間失敗或正常運行時,請參閱[使用者模式轉儲檔:選擇最佳工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)以選擇適當的工具來生成轉儲。</span><span class="sxs-lookup"><span data-stu-id="22fc7-615">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="22fc7-616">分析傾印</span><span class="sxs-lookup"><span data-stu-id="22fc7-616">Analyze the dump</span></span>

<span data-ttu-id="22fc7-617">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="22fc7-617">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="22fc7-618">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="22fc7-618">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22fc7-619">其他資源</span><span class="sxs-lookup"><span data-stu-id="22fc7-619">Additional resources</span></span>

* <span data-ttu-id="22fc7-620">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="22fc7-620">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
