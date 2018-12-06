---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.2'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/26/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: f857e96108b68bb6ec64a85910bf4d889cdf2822
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458513"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="a7994-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7994-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="a7994-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a7994-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="a7994-105">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="a7994-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="a7994-106">當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="a7994-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="a7994-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a7994-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="a7994-108">部署類型</span><span class="sxs-lookup"><span data-stu-id="a7994-108">Deployment type</span></span>

<span data-ttu-id="a7994-109">您可以建立架構相依或自封式 Windows 服務部署。</span><span class="sxs-lookup"><span data-stu-id="a7994-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="a7994-110">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="a7994-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="a7994-111">架構相的部署</span><span class="sxs-lookup"><span data-stu-id="a7994-111">Framework-dependent deployment</span></span>

<span data-ttu-id="a7994-112">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="a7994-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="a7994-113">搭配 ASP.NET Core Windows 服務應用程式使用 FDD 案例時，SDK 會產生可執行檔 (*\*.exe*)，稱為架構相依可執行檔。</span><span class="sxs-lookup"><span data-stu-id="a7994-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="a7994-114">自封式部署</span><span class="sxs-lookup"><span data-stu-id="a7994-114">Self-contained deployment</span></span>

<span data-ttu-id="a7994-115">自封式部署 (SCD) 不仰賴任何存在於目標系統上的共用元件。</span><span class="sxs-lookup"><span data-stu-id="a7994-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="a7994-116">執行階段與應用程式的相依性會隨著應用程式部署到裝載系統。</span><span class="sxs-lookup"><span data-stu-id="a7994-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="a7994-117">將專案轉換成 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="a7994-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="a7994-118">對現有的 ASP.NET Core 專進行下列變更，以服務形式執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="a7994-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

1. <span data-ttu-id="a7994-119">視您選擇的[部署類型](#deployment-type)而定，更新專案檔：</span><span class="sxs-lookup"><span data-stu-id="a7994-119">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

   * <span data-ttu-id="a7994-120">**架構相依部署 (FDD)** &ndash; 新增 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 到包含目標架構的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="a7994-120">**Framework-dependent Deployment (FDD)** &ndash; Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="a7994-121">新增 `<SelfContained>` 屬性集到 `false`。</span><span class="sxs-lookup"><span data-stu-id="a7994-121">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="a7994-122">透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。</span><span class="sxs-lookup"><span data-stu-id="a7994-122">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <SelfContained>false</SelfContained>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="a7994-123">**自封式部署 (SCD)** &ndash; 確認 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 存在或新增 RID 到包含目標架構的 `<PropertyGroup>`。</span><span class="sxs-lookup"><span data-stu-id="a7994-123">**Self-contained Deployment (SCD)** &ndash; Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="a7994-124">透過新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`，以停用 *web.config* 檔案的建立。</span><span class="sxs-lookup"><span data-stu-id="a7994-124">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

     ```xml
     <PropertyGroup>
       <TargetFramework>netcoreapp2.2</TargetFramework>
       <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
       <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
     </PropertyGroup>
     ```

     <span data-ttu-id="a7994-125">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="a7994-125">To publish for multiple RIDs:</span></span>

     * <span data-ttu-id="a7994-126">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="a7994-126">Provide the RIDs in a semicolon-delimited list.</span></span>
     * <span data-ttu-id="a7994-127">使用屬性名稱 `<RuntimeIdentifiers>` (複數)。</span><span class="sxs-lookup"><span data-stu-id="a7994-127">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

     <span data-ttu-id="a7994-128">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="a7994-128">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="a7994-129">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="a7994-129">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

   * <span data-ttu-id="a7994-130">若要啟用 Windows 事件記錄的記錄功能，請加入對 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="a7994-130">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

     <span data-ttu-id="a7994-131">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="a7994-131">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

1. <span data-ttu-id="a7994-132">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="a7994-132">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="a7994-133">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="a7994-133">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="a7994-134">檢查偵錯工具是否已附加或 `--console` 命令列引數是否存在。</span><span class="sxs-lookup"><span data-stu-id="a7994-134">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

     <span data-ttu-id="a7994-135">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 Web 主機上的 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="a7994-135">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

     <span data-ttu-id="a7994-136">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="a7994-136">If the conditions are false (the app is run as a service):</span></span>

     * <span data-ttu-id="a7994-137">呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="a7994-137">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> and use a path to the app's published location.</span></span> <span data-ttu-id="a7994-138">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 `GetCurrentDirectory` 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a7994-138">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when `GetCurrentDirectory` is called.</span></span> <span data-ttu-id="a7994-139">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="a7994-139">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
     * <span data-ttu-id="a7994-140">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7994-140">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

     <span data-ttu-id="a7994-141">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider) 需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數移除。</span><span class="sxs-lookup"><span data-stu-id="a7994-141">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

   * <span data-ttu-id="a7994-142">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="a7994-142">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="a7994-143">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="a7994-143">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="a7994-144">針對示範與測試用途，範例應用程式的生產設定檔案會將記錄層級設定為 `Information`。</span><span class="sxs-lookup"><span data-stu-id="a7994-144">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="a7994-145">在生產環境中，該值一般會設定為 `Error`。</span><span class="sxs-lookup"><span data-stu-id="a7994-145">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="a7994-146">如需詳細資訊，請參閱<xref:fundamentals/logging/index#windows-eventlog-provider>。</span><span class="sxs-lookup"><span data-stu-id="a7994-146">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

1. <span data-ttu-id="a7994-147">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7994-147">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="a7994-148">使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a7994-148">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="a7994-149">若要使用命令列介面 (CLI) 工具發佈應用程式，請在將發行設定傳遞到 [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) 選項的情況下從專案資料夾的命令提示字元中執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。</span><span class="sxs-lookup"><span data-stu-id="a7994-149">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="a7994-150">搭配路徑使用 [-o|--output](/dotnet/core/tools/dotnet-publish#options) 選項以發行到應用程式以外的資料夾。</span><span class="sxs-lookup"><span data-stu-id="a7994-150">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

   * <span data-ttu-id="a7994-151">**架構相依部署 (FDD)**</span><span class="sxs-lookup"><span data-stu-id="a7994-151">**Framework-dependent Deployment (FDD)**</span></span>

     <span data-ttu-id="a7994-152">在下列範例中，應用程式是發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="a7994-152">In the following example, the app is published to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --output c:\svc
     ```

   * <span data-ttu-id="a7994-153">**自封式部署 (SCD)** &ndash; 必須在專案檔的 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 屬性中指定 RID。</span><span class="sxs-lookup"><span data-stu-id="a7994-153">**Self-contained Deployment (SCD)** &ndash; The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="a7994-154">提供執行階段給 `dotnet publish` 命令的 [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) 選項。</span><span class="sxs-lookup"><span data-stu-id="a7994-154">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

     <span data-ttu-id="a7994-155">在下列範例中，應用程式是針對 `win7-x64` 執行階段發行到 *c:\\svc* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="a7994-155">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

     ```console
     dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
     ```

1. <span data-ttu-id="a7994-156">使用 `net user` 命令為服務建立使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="a7994-156">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="a7994-157">針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶與密碼。</span><span class="sxs-lookup"><span data-stu-id="a7994-157">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="a7994-158">在下列命令中，將 `{PASSWORD}` 取代為[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="a7994-158">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="a7994-159">若需要將使用者新增到某個群組，請使用 `net localgroup` 命令，其中 `{GROUP}` 是群組的名稱：</span><span class="sxs-lookup"><span data-stu-id="a7994-159">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="a7994-160">如需詳細資訊，請參閱[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="a7994-160">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="a7994-161">使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令授與對應用程式資料夾的寫入/讀取/執行存取權：</span><span class="sxs-lookup"><span data-stu-id="a7994-161">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="a7994-162">`{PATH}` &ndash; 應用程式資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="a7994-162">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="a7994-163">`{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。</span><span class="sxs-lookup"><span data-stu-id="a7994-163">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="a7994-164">`(OI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="a7994-164">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="a7994-165">`(CI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="a7994-165">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="a7994-166">`{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。</span><span class="sxs-lookup"><span data-stu-id="a7994-166">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="a7994-167">寫入 (`W`)</span><span class="sxs-lookup"><span data-stu-id="a7994-167">Write (`W`)</span></span>
     * <span data-ttu-id="a7994-168">讀取 (`R`)</span><span class="sxs-lookup"><span data-stu-id="a7994-168">Read (`R`)</span></span>
     * <span data-ttu-id="a7994-169">執行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="a7994-169">Execute (`X`)</span></span>
     * <span data-ttu-id="a7994-170">完整 (`F`)</span><span class="sxs-lookup"><span data-stu-id="a7994-170">Full (`F`)</span></span>
     * <span data-ttu-id="a7994-171">修改 (`M`)</span><span class="sxs-lookup"><span data-stu-id="a7994-171">Modify (`M`)</span></span>
   * <span data-ttu-id="a7994-172">`/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。</span><span class="sxs-lookup"><span data-stu-id="a7994-172">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="a7994-173">針對發行到 *c:\\svc* 資料夾的範例應用程式與具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="a7994-173">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="a7994-174">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。</span><span class="sxs-lookup"><span data-stu-id="a7994-174">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="a7994-175">使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-175">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="a7994-176">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="a7994-176">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="a7994-177">**等號與每個參數與值的引號字元之間必須有空格。**</span><span class="sxs-lookup"><span data-stu-id="a7994-177">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="a7994-178">`{SERVICE NAME}` &ndash; 要指派給[服務控制管理員](/windows/desktop/services/service-control-manager)中之服務的名稱。</span><span class="sxs-lookup"><span data-stu-id="a7994-178">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="a7994-179">`{PATH}` &ndash; 服務可執行檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="a7994-179">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="a7994-180">`{DOMAIN}` &ndash; 已加入網域之機器的網域。</span><span class="sxs-lookup"><span data-stu-id="a7994-180">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="a7994-181">若機器未加入網域，則為本機機器名稱。</span><span class="sxs-lookup"><span data-stu-id="a7994-181">If the machine isn't domain-joined, the local machine name.</span></span>
   * <span data-ttu-id="a7994-182">`{USER ACCOUNT}` &ndash; 用於執行服務的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="a7994-182">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
   * <span data-ttu-id="a7994-183">`{PASSWORD}` &ndash; 使用者帳戶密碼。</span><span class="sxs-lookup"><span data-stu-id="a7994-183">`{PASSWORD}` &ndash; The user account password.</span></span>

   > [!WARNING]
   > <span data-ttu-id="a7994-184">請**勿**省略 `obj` 參數。</span><span class="sxs-lookup"><span data-stu-id="a7994-184">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="a7994-185">`obj` 的預設值是 [LocalSystem 帳戶](/windows/desktop/services/localsystem-account)帳戶。</span><span class="sxs-lookup"><span data-stu-id="a7994-185">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="a7994-186">以 `LocalSystem` 帳戶執行服務會帶來重大安全性風險。</span><span class="sxs-lookup"><span data-stu-id="a7994-186">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="a7994-187">一律使用具有受限權限的使用者帳戶來執行服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-187">Always run a service with a user account that has restricted privileges.</span></span>

   <span data-ttu-id="a7994-188">在範例應用程式的下列範例中：</span><span class="sxs-lookup"><span data-stu-id="a7994-188">In the following example for the sample app:</span></span>

   * <span data-ttu-id="a7994-189">服務的名稱是 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="a7994-189">The service is named **MyService**.</span></span>
   * <span data-ttu-id="a7994-190">已發行的服務位於 *c:\\svc* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a7994-190">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="a7994-191">應用程式可執行檔名稱是 *SampleApp.exe*。</span><span class="sxs-lookup"><span data-stu-id="a7994-191">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="a7994-192">以雙引號 (") 括住 `binPath` 值。</span><span class="sxs-lookup"><span data-stu-id="a7994-192">Enclose the `binPath` value in double quotation marks (").</span></span>
   * <span data-ttu-id="a7994-193">服務是以 `ServiceUser` 帳戶執行。</span><span class="sxs-lookup"><span data-stu-id="a7994-193">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="a7994-194">將 `{DOMAIN}` 取代為使用者帳戶的網域或本機電腦名稱。</span><span class="sxs-lookup"><span data-stu-id="a7994-194">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="a7994-195">以雙引號 (") 括住 `obj` 值。</span><span class="sxs-lookup"><span data-stu-id="a7994-195">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="a7994-196">範例：若裝載系統是名為 `MairaPC` 的本機電腦，請將 `obj` 設定為 `"MairaPC\ServiceUser"`。</span><span class="sxs-lookup"><span data-stu-id="a7994-196">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="a7994-197">將 `{PASSWORD}` 取代為使用者帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="a7994-197">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="a7994-198">以雙引號 (") 括住 `password` 值。</span><span class="sxs-lookup"><span data-stu-id="a7994-198">Enclose the `password` value in double quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="a7994-199">確定參數的等號與參數的值之間有空格。</span><span class="sxs-lookup"><span data-stu-id="a7994-199">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="a7994-200">以 `sc start {SERVICE NAME}` 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-200">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="a7994-201">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a7994-201">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="a7994-202">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-202">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="a7994-203">若要檢查服務的狀態，請使用 `sc query {SERVICE NAME}` 命令。</span><span class="sxs-lookup"><span data-stu-id="a7994-203">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="a7994-204">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="a7994-204">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="a7994-205">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="a7994-205">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="a7994-206">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="a7994-206">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="a7994-207">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a7994-207">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="a7994-208">以 `sc stop {SERVICE NAME}` 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-208">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="a7994-209">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a7994-209">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="a7994-210">在停止服務的短暫延遲之後，請以 `sc delete {SERVICE NAME}` 命令解除安裝服務。</span><span class="sxs-lookup"><span data-stu-id="a7994-210">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="a7994-211">檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="a7994-211">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="a7994-212">當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="a7994-212">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="a7994-213">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="a7994-213">Handle starting and stopping events</span></span>

<span data-ttu-id="a7994-214">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件，請進行下列額外變更：</span><span class="sxs-lookup"><span data-stu-id="a7994-214">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="a7994-215">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="a7994-215">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="a7994-216">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="a7994-216">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="a7994-217">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="a7994-217">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="a7994-218">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[將專案轉換為 Windows 服務](#convert-a-project-into-a-windows-service)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="a7994-218">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="a7994-219">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="a7994-219">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="a7994-220">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="a7994-220">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="a7994-221">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="a7994-221">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="a7994-222">設定 HTTPS</span><span class="sxs-lookup"><span data-stu-id="a7994-222">Configure HTTPS</span></span>

<span data-ttu-id="a7994-223">使用安全端點來設定服務：</span><span class="sxs-lookup"><span data-stu-id="a7994-223">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="a7994-224">使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="a7994-224">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="a7994-225">指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。</span><span class="sxs-lookup"><span data-stu-id="a7994-225">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="a7994-226">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="a7994-226">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="a7994-227">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="a7994-227">Current directory and content root</span></span>

<span data-ttu-id="a7994-228">針對 Windows 服務呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a7994-228">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="a7994-229">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="a7994-229">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="a7994-230">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="a7994-230">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="a7994-231">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="a7994-231">Set the content root path to the app's folder</span></span>

<span data-ttu-id="a7994-232"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="a7994-232">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="a7994-233">請搭配應用程式內容根目錄的路徑呼叫 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>，而不要呼叫 `GetCurrentDirectory` 來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="a7994-233">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> with the path to the app's content root.</span></span>

<span data-ttu-id="a7994-234">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="a7994-234">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);

CreateWebHostBuilder(args)
    .UseContentRoot(pathToContentRoot)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="a7994-235">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="a7994-235">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="a7994-236">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="a7994-236">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7994-237">其他資源</span><span class="sxs-lookup"><span data-stu-id="a7994-237">Additional resources</span></span>

* <span data-ttu-id="a7994-238">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="a7994-238">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
