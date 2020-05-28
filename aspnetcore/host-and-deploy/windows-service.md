---
<span data-ttu-id="525f9-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="525f9-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="525f9-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="525f9-102">'Blazor'</span></span>
- <span data-ttu-id="525f9-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="525f9-103">'Identity'</span></span>
- <span data-ttu-id="525f9-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="525f9-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="525f9-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="525f9-105">'Razor'</span></span>
- <span data-ttu-id="525f9-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="525f9-106">'SignalR' uid:</span></span> 

---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="525f9-107">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="525f9-107">Host ASP.NET Core in a Windows Service</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="525f9-108">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="525f9-108">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="525f9-109">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="525f9-109">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="525f9-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="525f9-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="525f9-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="525f9-111">Prerequisites</span></span>

* [<span data-ttu-id="525f9-112">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-112">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="525f9-113">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-113">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="worker-service-template"></a><span data-ttu-id="525f9-114">背景工作服務範本</span><span class="sxs-lookup"><span data-stu-id="525f9-114">Worker Service template</span></span>

<span data-ttu-id="525f9-115">ASP.NET Core 背景工作服務範本提供撰寫長期執行服務應用程式的起點。</span><span class="sxs-lookup"><span data-stu-id="525f9-115">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="525f9-116">使用範本作為 Windows 服務應用程式的基礎：</span><span class="sxs-lookup"><span data-stu-id="525f9-116">To use the template as a basis for a Windows Service app:</span></span>

1. <span data-ttu-id="525f9-117">從 .NET Core 範本建立背景工作服務應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-117">Create a Worker Service app from the .NET Core template.</span></span>
1. <span data-ttu-id="525f9-118">請遵循[應用程式組態](#app-configuration)一節中的指導方針，更新背景工作服務應用程式，以便其執行為 Windows 服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-118">Follow the guidance in the [App configuration](#app-configuration) section to update the Worker Service app so that it can run as a Windows Service.</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="app-configuration"></a><span data-ttu-id="525f9-119">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="525f9-119">App configuration</span></span>

<span data-ttu-id="525f9-120">應用程式需要[WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices)的套件參考。</span><span class="sxs-lookup"><span data-stu-id="525f9-120">The app requires a package reference for [Microsoft.Extensions.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.Extensions.Hosting.WindowsServices).</span></span>

<span data-ttu-id="525f9-121">`IHostBuilder.UseWindowsService`建立主機時，會呼叫。</span><span class="sxs-lookup"><span data-stu-id="525f9-121">`IHostBuilder.UseWindowsService` is called when building the host.</span></span> <span data-ttu-id="525f9-122">如果應用程式以 Windows 服務形式執行，則方法會：</span><span class="sxs-lookup"><span data-stu-id="525f9-122">If the app is running as a Windows Service, the method:</span></span>

* <span data-ttu-id="525f9-123">將主機存留期設定為 `WindowsServiceLifetime`。</span><span class="sxs-lookup"><span data-stu-id="525f9-123">Sets the host lifetime to `WindowsServiceLifetime`.</span></span>
* <span data-ttu-id="525f9-124">將[內容根目錄](xref:fundamentals/index#content-root)設定為[AppCoNtext. BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="525f9-124">Sets the [content root](xref:fundamentals/index#content-root) to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span> <span data-ttu-id="525f9-125">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="525f9-125">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
* <span data-ttu-id="525f9-126">啟用記錄至事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="525f9-126">Enables logging to the event log:</span></span>
  * <span data-ttu-id="525f9-127">應用程式名稱會用來做為預設的來源名稱。</span><span class="sxs-lookup"><span data-stu-id="525f9-127">The application name is used as the default source name.</span></span>
  * <span data-ttu-id="525f9-128">根據呼叫來建立主機的 ASP.NET Core 範本，應用程式的預設記錄層級為*警告*或更高 `CreateDefaultBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-128">The default log level is *Warning* or higher for an app based on an ASP.NET Core template that calls `CreateDefaultBuilder` to build the host.</span></span>
  * <span data-ttu-id="525f9-129">使用 `Logging:EventLog:LogLevel:Default` *appsettings. json*appsettings 中的金鑰覆寫預設記錄層級 / *。 {環境}. json*或其他設定提供者。</span><span class="sxs-lookup"><span data-stu-id="525f9-129">Override the default log level with the `Logging:EventLog:LogLevel:Default` key in *appsettings.json*/*appsettings.{Environment}.json* or other configuration provider.</span></span>
  * <span data-ttu-id="525f9-130">只有系統管理員才能建立新的事件來源。</span><span class="sxs-lookup"><span data-stu-id="525f9-130">Only administrators can create new event sources.</span></span> <span data-ttu-id="525f9-131">如果無法使用應用程式名稱建立事件來源，則會向「應用程式」\*\* 來源記錄警告，並停用事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-131">When an event source can't be created using the application name, a warning is logged to the *Application* source and event logs are disabled.</span></span>

<span data-ttu-id="525f9-132">在 `CreateHostBuilder` *Program.cs*中：</span><span class="sxs-lookup"><span data-stu-id="525f9-132">In `CreateHostBuilder` of *Program.cs*:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseWindowsService()
    ...
```

<span data-ttu-id="525f9-133">本主題隨附的下列範例應用程式：</span><span class="sxs-lookup"><span data-stu-id="525f9-133">The following sample apps accompany this topic:</span></span>

* <span data-ttu-id="525f9-134">背景工作角色服務範例：非 web 應用程式範例，以使用[託管服務](xref:fundamentals/host/hosted-services)來執行背景[工作的工作者服務範本](#worker-service-template)為基礎。</span><span class="sxs-lookup"><span data-stu-id="525f9-134">Background Worker Service Sample: A non-web app sample based on the [Worker Service template](#worker-service-template) that uses [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>
* <span data-ttu-id="525f9-135">Web App Service 範例： Razor 頁面 web 應用程式範例，會以 Windows 服務的形式執行，並具有適用于背景工作的[託管服務](xref:fundamentals/host/hosted-services)。</span><span class="sxs-lookup"><span data-stu-id="525f9-135">Web App Service Sample: A Razor Pages web app sample that runs as a Windows Service with [hosted services](xref:fundamentals/host/hosted-services) for background tasks.</span></span>

<span data-ttu-id="525f9-136">如需 MVC 指引，請參閱和底下的文章 <xref:mvc/overview> <xref:migration/22-to-30> 。</span><span class="sxs-lookup"><span data-stu-id="525f9-136">For MVC guidance, see the articles under <xref:mvc/overview> and <xref:migration/22-to-30>.</span></span>

## <a name="deployment-type"></a><span data-ttu-id="525f9-137">部署類型</span><span class="sxs-lookup"><span data-stu-id="525f9-137">Deployment type</span></span>

<span data-ttu-id="525f9-138">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="525f9-138">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="525f9-139">SDK</span><span class="sxs-lookup"><span data-stu-id="525f9-139">SDK</span></span>

<span data-ttu-id="525f9-140">針對使用頁面或 MVC 架構的 web 應用程式服務 Razor ，請在專案檔中指定 WEB SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-140">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="525f9-141">如果服務只執行背景工作（例如，[託管服務](xref:fundamentals/host/hosted-services)），請在專案檔中指定工作者 SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-141">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="525f9-142">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="525f9-142">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="525f9-143">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="525f9-143">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="525f9-144">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-144">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="525f9-145">如果使用[WEB SDK](#sdk)，通常是在發佈 ASP.NET Core 應用程式時所*產生的 web.config*檔案，對於 Windows 服務應用程式而言是不必要的。</span><span class="sxs-lookup"><span data-stu-id="525f9-145">If using the [Web SDK](#sdk), a *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="525f9-146">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="525f9-146">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.0</TargetFramework>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="525f9-147">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="525f9-147">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="525f9-148">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="525f9-148">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="525f9-149">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="525f9-149">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="525f9-150">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="525f9-150">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="525f9-151">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="525f9-151">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="525f9-152">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="525f9-152">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="525f9-153">使用屬性名稱 [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) （複數）。</span><span class="sxs-lookup"><span data-stu-id="525f9-153">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="525f9-154">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="525f9-154">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

## <a name="service-user-account"></a><span data-ttu-id="525f9-155">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="525f9-155">Service user account</span></span>

<span data-ttu-id="525f9-156">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="525f9-156">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="525f9-157">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="525f9-157">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-158">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="525f9-158">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="525f9-159">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="525f9-159">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="525f9-160">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="525f9-160">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="525f9-161">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="525f9-161">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="525f9-162">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="525f9-162">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="525f9-163">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="525f9-163">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="525f9-164">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="525f9-164">Log on as a service rights</span></span>

<span data-ttu-id="525f9-165">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="525f9-165">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="525f9-166">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="525f9-166">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="525f9-167">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-167">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="525f9-168">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-168">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="525f9-169">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-169">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="525f9-170">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="525f9-170">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="525f9-171">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-171">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="525f9-172">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="525f9-172">Select **Advanced**.</span></span> <span data-ttu-id="525f9-173">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-173">Select **Find Now**.</span></span> <span data-ttu-id="525f9-174">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="525f9-174">Select the user account from the list.</span></span> <span data-ttu-id="525f9-175">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="525f9-175">Select **OK**.</span></span> <span data-ttu-id="525f9-176">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-176">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="525f9-177">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="525f9-177">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="525f9-178">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="525f9-178">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="525f9-179">建立服務</span><span class="sxs-lookup"><span data-stu-id="525f9-179">Create a service</span></span>

<span data-ttu-id="525f9-180">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-180">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="525f9-181">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-181">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="525f9-182">`{EXE PATH}`：主機上的應用程式資料夾路徑（例如， `d:\myservice` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-182">`{EXE PATH}`: Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="525f9-183">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-183">Don't include the app's executable in the path.</span></span> <span data-ttu-id="525f9-184">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="525f9-184">A trailing slash isn't required.</span></span>
* <span data-ttu-id="525f9-185">`{DOMAIN OR COMPUTER NAME\USER}`：服務使用者帳戶（例如 `Contoso\ServiceUser` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-185">`{DOMAIN OR COMPUTER NAME\USER}`: Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="525f9-186">`{SERVICE NAME}`：服務名稱（例如， `MyService` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-186">`{SERVICE NAME}`: Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="525f9-187">`{EXE FILE PATH}`：應用程式的可執行檔路徑（例如， `d:\myservice\myservice.exe` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-187">`{EXE FILE PATH}`: The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="525f9-188">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="525f9-188">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="525f9-189">`{DESCRIPTION}`：服務描述（例如， `My sample service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-189">`{DESCRIPTION}`: Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="525f9-190">`{DISPLAY NAME}`：服務顯示名稱（例如， `My Service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-190">`{DISPLAY NAME}`: Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="525f9-191">啟動服務</span><span class="sxs-lookup"><span data-stu-id="525f9-191">Start a service</span></span>

<span data-ttu-id="525f9-192">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-192">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-193">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-193">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="525f9-194">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="525f9-194">Determine a service's status</span></span>

<span data-ttu-id="525f9-195">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-195">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-196">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="525f9-196">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="525f9-197">停止服務</span><span class="sxs-lookup"><span data-stu-id="525f9-197">Stop a service</span></span>

<span data-ttu-id="525f9-198">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-198">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="525f9-199">移除服務</span><span class="sxs-lookup"><span data-stu-id="525f9-199">Remove a service</span></span>

<span data-ttu-id="525f9-200">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-200">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="525f9-201">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="525f9-201">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="525f9-202">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-202">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="525f9-203">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="525f9-203">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="525f9-204">設定端點</span><span class="sxs-lookup"><span data-stu-id="525f9-204">Configure endpoints</span></span>

<span data-ttu-id="525f9-205">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="525f9-205">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="525f9-206">設定環境變數，以設定 URL 和埠 `ASPNETCORE_URLS` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-206">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="525f9-207">如需其他 URL 和埠設定方法，請參閱相關的伺服器文章：</span><span class="sxs-lookup"><span data-stu-id="525f9-207">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="525f9-208">前述指引涵蓋 HTTPS 端點的支援。</span><span class="sxs-lookup"><span data-stu-id="525f9-208">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="525f9-209">例如，當搭配 Windows 服務使用驗證時，請為 HTTPS 設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-209">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="525f9-210">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="525f9-210">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="525f9-211">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="525f9-211">Current directory and content root</span></span>

<span data-ttu-id="525f9-212">呼叫 Windows 服務所傳回的目前工作目錄 <xref:System.IO.Directory.GetCurrentDirectory*> 是*C： \\ Windows \\ system32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-212">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="525f9-213">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="525f9-213">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="525f9-214">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-214">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="use-contentrootpath-or-contentrootfileprovider"></a><span data-ttu-id="525f9-215">使用 ContentRootPath 或 ContentRootFileProvider</span><span class="sxs-lookup"><span data-stu-id="525f9-215">Use ContentRootPath or ContentRootFileProvider</span></span>

<span data-ttu-id="525f9-216">使用 [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) 或 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> 來找出應用程式的資源。</span><span class="sxs-lookup"><span data-stu-id="525f9-216">Use [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath) or <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider> to locate an app's resources.</span></span>

<span data-ttu-id="525f9-217">當應用程式以服務方式執行時，會 <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> 將設定 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> 為[AppCoNtext. BaseDirectory](xref:System.AppContext.BaseDirectory)。</span><span class="sxs-lookup"><span data-stu-id="525f9-217">When the app runs as a service, <xref:Microsoft.Extensions.Hosting.WindowsServiceLifetimeHostBuilderExtensions.UseWindowsService*> sets the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath> to [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory).</span></span>

<span data-ttu-id="525f9-218">應用程式的預設設定檔案*appsettings. json*和*appsettings。 {環境}. json*會在[主機結構期間呼叫 CreateDefaultBuilder](xref:fundamentals/host/generic-host#set-up-a-host)，從應用程式的內容根目錄載入。</span><span class="sxs-lookup"><span data-stu-id="525f9-218">The app's default settings files, *appsettings.json* and *appsettings.{Environment}.json*, are loaded from the app's content root by calling [CreateDefaultBuilder during host construction](xref:fundamentals/host/generic-host#set-up-a-host).</span></span>

<span data-ttu-id="525f9-219">若為開發人員程式碼在中載入的其他設定檔案 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ，則不需要呼叫 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 。</span><span class="sxs-lookup"><span data-stu-id="525f9-219">For other settings files loaded by developer code in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, there's no need to call <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="525f9-220">在下列範例中， *custom_settings 的 json*檔案存在於應用程式的內容根目錄中，並在未明確設定基底路徑的情況下載入：</span><span class="sxs-lookup"><span data-stu-id="525f9-220">In the following example, the *custom_settings.json* file exists in the app's content root and is loaded without explicitly setting a base path:</span></span>

[!code-csharp[](windows-service/samples_snapshot/CustomSettingsExample.cs?highlight=13)]

<span data-ttu-id="525f9-221">請不要嘗試使用 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得資源路徑，因為 Windows 服務應用程式會傳回*C： \\ windows \\ system32*資料夾做為其目前目錄。</span><span class="sxs-lookup"><span data-stu-id="525f9-221">Don't attempt to use <xref:System.IO.Directory.GetCurrentDirectory*> to obtain a resource path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder as its current directory.</span></span>

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="525f9-222">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="525f9-222">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="525f9-223">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-223">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="525f9-224">疑難排解</span><span class="sxs-lookup"><span data-stu-id="525f9-224">Troubleshoot</span></span>

<span data-ttu-id="525f9-225">若要疑難排解 Windows 服務應用程式，請參閱 <xref:test/troubleshoot> 。</span><span class="sxs-lookup"><span data-stu-id="525f9-225">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="525f9-226">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="525f9-226">Common errors</span></span>

* <span data-ttu-id="525f9-227">舊版或發行前版本的 PowerShell 已在使用中。</span><span class="sxs-lookup"><span data-stu-id="525f9-227">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="525f9-228">已註冊的服務不會使用來自[dotnet publish](/dotnet/core/tools/dotnet-publish)命令的應用程式**已發佈**輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-228">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="525f9-229">應用程式部署不支援[dotnet build](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-229">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="525f9-230">根據部署類型，可以在下列其中一個資料夾中找到已發佈的資產：</span><span class="sxs-lookup"><span data-stu-id="525f9-230">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="525f9-231">*bin/Release/{TARGET FRAMEWORK}/publish* （FDD）</span><span class="sxs-lookup"><span data-stu-id="525f9-231">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="525f9-232">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* （SCD）</span><span class="sxs-lookup"><span data-stu-id="525f9-232">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="525f9-233">服務不是處於執行中狀態。</span><span class="sxs-lookup"><span data-stu-id="525f9-233">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="525f9-234">應用程式所使用資源的路徑（例如憑證）不正確。</span><span class="sxs-lookup"><span data-stu-id="525f9-234">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="525f9-235">Windows 服務的基底路徑是*c： \\ windows \\ System32*。</span><span class="sxs-lookup"><span data-stu-id="525f9-235">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="525f9-236">使用者不具有 [*以服務方式登*入] 許可權。</span><span class="sxs-lookup"><span data-stu-id="525f9-236">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="525f9-237">執行 PowerShell 命令時，使用者的密碼已過期或傳遞錯誤 `New-Service` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-237">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="525f9-238">應用程式需要 ASP.NET Core 驗證，但未設定安全連線（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="525f9-238">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="525f9-239">要求 URL 埠不正確，或未在應用程式中正確設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-239">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="525f9-240">系統和應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="525f9-240">System and Application Event Logs</span></span>

<span data-ttu-id="525f9-241">存取系統和應用程式事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="525f9-241">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="525f9-242">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-242">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="525f9-243">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="525f9-243">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="525f9-244">選取 [**系統**] 以開啟 [系統] 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-244">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="525f9-245">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="525f9-245">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="525f9-246">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="525f9-246">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="525f9-247">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-247">Run the app at a command prompt</span></span>

<span data-ttu-id="525f9-248">許多啟動錯誤不會在事件記錄檔中產生有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="525f9-248">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="525f9-249">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-249">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="525f9-250">若要從應用程式記錄其他詳細資料，請降低[記錄層級](xref:fundamentals/logging/index#log-level)，或在[開發環境](xref:fundamentals/environments)中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-250">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="525f9-251">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="525f9-251">Clear package caches</span></span>

<span data-ttu-id="525f9-252">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="525f9-252">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="525f9-253">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-253">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="525f9-254">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="525f9-254">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="525f9-255">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-255">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="525f9-256">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="525f9-256">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="525f9-257">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令 `nuget locals all -clear` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-257">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="525f9-258">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="525f9-258">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="525f9-259">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="525f9-259">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="525f9-260">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-260">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="525f9-261">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-261">Slow or hanging app</span></span>

<span data-ttu-id="525f9-262">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-262">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="525f9-263">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="525f9-263">App crashes or encounters an exception</span></span>

<span data-ttu-id="525f9-264">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="525f9-264">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="525f9-265">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-265">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="525f9-266">以應用程式可執行檔名稱執行[EnableDumps PowerShell 腳本](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-266">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```powershell
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="525f9-267">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-267">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="525f9-268">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-268">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/samples/scripts/DisableDumps.ps1):</span></span>

   ```powershell
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="525f9-269">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-269">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="525f9-270">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-270">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="525f9-271">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="525f9-271">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="525f9-272">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="525f9-272">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="525f9-273">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-273">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="525f9-274">分析傾印</span><span class="sxs-lookup"><span data-stu-id="525f9-274">Analyze the dump</span></span>

<span data-ttu-id="525f9-275">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-275">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="525f9-276">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="525f9-276">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="525f9-277">其他資源</span><span class="sxs-lookup"><span data-stu-id="525f9-277">Additional resources</span></span>

* <span data-ttu-id="525f9-278">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="525f9-278">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/generic-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="525f9-279">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="525f9-279">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="525f9-280">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="525f9-280">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="525f9-281">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="525f9-281">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="525f9-282">必要條件</span><span class="sxs-lookup"><span data-stu-id="525f9-282">Prerequisites</span></span>

* [<span data-ttu-id="525f9-283">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-283">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="525f9-284">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-284">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="525f9-285">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="525f9-285">App configuration</span></span>

<span data-ttu-id="525f9-286">應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="525f9-286">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="525f9-287">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="525f9-287">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="525f9-288">檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。</span><span class="sxs-lookup"><span data-stu-id="525f9-288">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="525f9-289">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-289">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="525f9-290">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="525f9-290">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="525f9-291">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-291">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="525f9-292">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-292">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="525f9-293">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="525f9-293">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="525f9-294">在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="525f9-294">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="525f9-295">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-295">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="525f9-296">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。</span><span class="sxs-lookup"><span data-stu-id="525f9-296">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="525f9-297">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-297">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="525f9-298">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="525f9-298">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="525f9-299">在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-299">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="525f9-300">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="525f9-300">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="525f9-301">部署類型</span><span class="sxs-lookup"><span data-stu-id="525f9-301">Deployment type</span></span>

<span data-ttu-id="525f9-302">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="525f9-302">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="525f9-303">SDK</span><span class="sxs-lookup"><span data-stu-id="525f9-303">SDK</span></span>

<span data-ttu-id="525f9-304">針對使用頁面或 MVC 架構的 web 應用程式服務 Razor ，請在專案檔中指定 WEB SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-304">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="525f9-305">如果服務只執行背景工作（例如，[託管服務](xref:fundamentals/host/hosted-services)），請在專案檔中指定工作者 SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-305">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="525f9-306">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="525f9-306">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="525f9-307">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="525f9-307">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="525f9-308">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-308">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="525f9-309">Windows[執行時間識別碼（RID）](/dotnet/core/rid-catalog) （ [\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier) ）包含目標 framework。</span><span class="sxs-lookup"><span data-stu-id="525f9-309">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="525f9-310">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="525f9-310">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="525f9-311">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="525f9-311">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="525f9-312">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="525f9-312">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="525f9-313">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="525f9-313">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="525f9-314">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="525f9-314">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="525f9-315">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="525f9-315">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="525f9-316">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="525f9-316">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="525f9-317">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="525f9-317">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="525f9-318">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="525f9-318">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="525f9-319">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="525f9-319">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="525f9-320">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="525f9-320">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="525f9-321">使用屬性名稱 [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) （複數）。</span><span class="sxs-lookup"><span data-stu-id="525f9-321">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="525f9-322">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="525f9-322">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="525f9-323">`<SelfContained>` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="525f9-323">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="525f9-324">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="525f9-324">Service user account</span></span>

<span data-ttu-id="525f9-325">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="525f9-325">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="525f9-326">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="525f9-326">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-327">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="525f9-327">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="525f9-328">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="525f9-328">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="525f9-329">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="525f9-329">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="525f9-330">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="525f9-330">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="525f9-331">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="525f9-331">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="525f9-332">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="525f9-332">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="525f9-333">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="525f9-333">Log on as a service rights</span></span>

<span data-ttu-id="525f9-334">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="525f9-334">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="525f9-335">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="525f9-335">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="525f9-336">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-336">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="525f9-337">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-337">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="525f9-338">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-338">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="525f9-339">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="525f9-339">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="525f9-340">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-340">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="525f9-341">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="525f9-341">Select **Advanced**.</span></span> <span data-ttu-id="525f9-342">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-342">Select **Find Now**.</span></span> <span data-ttu-id="525f9-343">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="525f9-343">Select the user account from the list.</span></span> <span data-ttu-id="525f9-344">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="525f9-344">Select **OK**.</span></span> <span data-ttu-id="525f9-345">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-345">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="525f9-346">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="525f9-346">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="525f9-347">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="525f9-347">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="525f9-348">建立服務</span><span class="sxs-lookup"><span data-stu-id="525f9-348">Create a service</span></span>

<span data-ttu-id="525f9-349">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-349">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="525f9-350">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-350">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="525f9-351">`{EXE PATH}`：主機上的應用程式資料夾路徑（例如， `d:\myservice` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-351">`{EXE PATH}`: Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="525f9-352">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-352">Don't include the app's executable in the path.</span></span> <span data-ttu-id="525f9-353">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="525f9-353">A trailing slash isn't required.</span></span>
* <span data-ttu-id="525f9-354">`{DOMAIN OR COMPUTER NAME\USER}`：服務使用者帳戶（例如 `Contoso\ServiceUser` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-354">`{DOMAIN OR COMPUTER NAME\USER}`: Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="525f9-355">`{SERVICE NAME}`：服務名稱（例如， `MyService` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-355">`{SERVICE NAME}`: Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="525f9-356">`{EXE FILE PATH}`：應用程式的可執行檔路徑（例如， `d:\myservice\myservice.exe` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-356">`{EXE FILE PATH}`: The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="525f9-357">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="525f9-357">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="525f9-358">`{DESCRIPTION}`：服務描述（例如， `My sample service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-358">`{DESCRIPTION}`: Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="525f9-359">`{DISPLAY NAME}`：服務顯示名稱（例如， `My Service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-359">`{DISPLAY NAME}`: Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="525f9-360">啟動服務</span><span class="sxs-lookup"><span data-stu-id="525f9-360">Start a service</span></span>

<span data-ttu-id="525f9-361">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-361">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-362">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-362">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="525f9-363">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="525f9-363">Determine a service's status</span></span>

<span data-ttu-id="525f9-364">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-364">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-365">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="525f9-365">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="525f9-366">停止服務</span><span class="sxs-lookup"><span data-stu-id="525f9-366">Stop a service</span></span>

<span data-ttu-id="525f9-367">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-367">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="525f9-368">移除服務</span><span class="sxs-lookup"><span data-stu-id="525f9-368">Remove a service</span></span>

<span data-ttu-id="525f9-369">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-369">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="525f9-370">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="525f9-370">Handle starting and stopping events</span></span>

<span data-ttu-id="525f9-371">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：</span><span class="sxs-lookup"><span data-stu-id="525f9-371">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="525f9-372">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="525f9-372">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="525f9-373">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="525f9-373">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="525f9-374">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="525f9-374">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="525f9-375">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="525f9-375">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="525f9-376">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="525f9-376">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="525f9-377">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-377">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="525f9-378">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="525f9-378">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="525f9-379">設定端點</span><span class="sxs-lookup"><span data-stu-id="525f9-379">Configure endpoints</span></span>

<span data-ttu-id="525f9-380">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="525f9-380">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="525f9-381">設定環境變數，以設定 URL 和埠 `ASPNETCORE_URLS` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-381">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="525f9-382">如需其他 URL 和埠設定方法，請參閱相關的伺服器文章：</span><span class="sxs-lookup"><span data-stu-id="525f9-382">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="525f9-383">前述指引涵蓋 HTTPS 端點的支援。</span><span class="sxs-lookup"><span data-stu-id="525f9-383">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="525f9-384">例如，當搭配 Windows 服務使用驗證時，請為 HTTPS 設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-384">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="525f9-385">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="525f9-385">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="525f9-386">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="525f9-386">Current directory and content root</span></span>

<span data-ttu-id="525f9-387">呼叫 Windows 服務所傳回的目前工作目錄 <xref:System.IO.Directory.GetCurrentDirectory*> 是*C： \\ Windows \\ system32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-387">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="525f9-388">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="525f9-388">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="525f9-389">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-389">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="525f9-390">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="525f9-390">Set the content root path to the app's folder</span></span>

<span data-ttu-id="525f9-391"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="525f9-391">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="525f9-392">`GetCurrentDirectory`請 <xref:System.IO.Directory.SetCurrentDirectory*> 使用應用程式[內容根目錄](xref:fundamentals/index#content-root)的路徑呼叫，而不是呼叫來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-392">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="525f9-393">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="525f9-393">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="525f9-394">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="525f9-394">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="525f9-395">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-395">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="525f9-396">疑難排解</span><span class="sxs-lookup"><span data-stu-id="525f9-396">Troubleshoot</span></span>

<span data-ttu-id="525f9-397">若要疑難排解 Windows 服務應用程式，請參閱 <xref:test/troubleshoot> 。</span><span class="sxs-lookup"><span data-stu-id="525f9-397">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="525f9-398">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="525f9-398">Common errors</span></span>

* <span data-ttu-id="525f9-399">舊版或發行前版本的 PowerShell 已在使用中。</span><span class="sxs-lookup"><span data-stu-id="525f9-399">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="525f9-400">已註冊的服務不會使用來自[dotnet publish](/dotnet/core/tools/dotnet-publish)命令的應用程式**已發佈**輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-400">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="525f9-401">應用程式部署不支援[dotnet build](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-401">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="525f9-402">根據部署類型，可以在下列其中一個資料夾中找到已發佈的資產：</span><span class="sxs-lookup"><span data-stu-id="525f9-402">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="525f9-403">*bin/Release/{TARGET FRAMEWORK}/publish* （FDD）</span><span class="sxs-lookup"><span data-stu-id="525f9-403">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="525f9-404">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* （SCD）</span><span class="sxs-lookup"><span data-stu-id="525f9-404">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="525f9-405">服務不是處於執行中狀態。</span><span class="sxs-lookup"><span data-stu-id="525f9-405">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="525f9-406">應用程式所使用資源的路徑（例如憑證）不正確。</span><span class="sxs-lookup"><span data-stu-id="525f9-406">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="525f9-407">Windows 服務的基底路徑是*c： \\ windows \\ System32*。</span><span class="sxs-lookup"><span data-stu-id="525f9-407">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="525f9-408">使用者不具有 [*以服務方式登*入] 許可權。</span><span class="sxs-lookup"><span data-stu-id="525f9-408">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="525f9-409">執行 PowerShell 命令時，使用者的密碼已過期或傳遞錯誤 `New-Service` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-409">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="525f9-410">應用程式需要 ASP.NET Core 驗證，但未設定安全連線（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="525f9-410">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="525f9-411">要求 URL 埠不正確，或未在應用程式中正確設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-411">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="525f9-412">系統和應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="525f9-412">System and Application Event Logs</span></span>

<span data-ttu-id="525f9-413">存取系統和應用程式事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="525f9-413">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="525f9-414">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-414">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="525f9-415">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="525f9-415">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="525f9-416">選取 [**系統**] 以開啟 [系統] 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-416">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="525f9-417">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="525f9-417">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="525f9-418">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="525f9-418">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="525f9-419">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-419">Run the app at a command prompt</span></span>

<span data-ttu-id="525f9-420">許多啟動錯誤不會在事件記錄檔中產生有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="525f9-420">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="525f9-421">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-421">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="525f9-422">若要從應用程式記錄其他詳細資料，請降低[記錄層級](xref:fundamentals/logging/index#log-level)，或在[開發環境](xref:fundamentals/environments)中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-422">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="525f9-423">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="525f9-423">Clear package caches</span></span>

<span data-ttu-id="525f9-424">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="525f9-424">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="525f9-425">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-425">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="525f9-426">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="525f9-426">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="525f9-427">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-427">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="525f9-428">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="525f9-428">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="525f9-429">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令 `nuget locals all -clear` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-429">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="525f9-430">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="525f9-430">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="525f9-431">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="525f9-431">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="525f9-432">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-432">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="525f9-433">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-433">Slow or hanging app</span></span>

<span data-ttu-id="525f9-434">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-434">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="525f9-435">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="525f9-435">App crashes or encounters an exception</span></span>

<span data-ttu-id="525f9-436">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="525f9-436">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="525f9-437">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-437">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="525f9-438">以應用程式可執行檔名稱執行[EnableDumps PowerShell 腳本](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-438">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="525f9-439">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-439">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="525f9-440">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-440">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="525f9-441">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-441">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="525f9-442">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-442">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="525f9-443">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="525f9-443">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="525f9-444">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="525f9-444">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="525f9-445">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-445">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="525f9-446">分析傾印</span><span class="sxs-lookup"><span data-stu-id="525f9-446">Analyze the dump</span></span>

<span data-ttu-id="525f9-447">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-447">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="525f9-448">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="525f9-448">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="525f9-449">其他資源</span><span class="sxs-lookup"><span data-stu-id="525f9-449">Additional resources</span></span>

* <span data-ttu-id="525f9-450">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="525f9-450">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="525f9-451">ASP.NET Core 應用程式可以裝載在 Windows 上作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)，不需要使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="525f9-451">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="525f9-452">當裝載為 Windows 服務時，應用程式將會在伺服器重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="525f9-452">When hosted as a Windows Service, the app automatically starts after server reboots.</span></span>

<span data-ttu-id="525f9-453">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="525f9-453">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="525f9-454">必要條件</span><span class="sxs-lookup"><span data-stu-id="525f9-454">Prerequisites</span></span>

* [<span data-ttu-id="525f9-455">ASP.NET Core SDK 2.1 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-455">ASP.NET Core SDK 2.1 or later</span></span>](https://dotnet.microsoft.com/download)
* [<span data-ttu-id="525f9-456">PowerShell 6.2 或更新版本</span><span class="sxs-lookup"><span data-stu-id="525f9-456">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="app-configuration"></a><span data-ttu-id="525f9-457">應用程式組態</span><span class="sxs-lookup"><span data-stu-id="525f9-457">App configuration</span></span>

<span data-ttu-id="525f9-458">應用程式需要 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 和 [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="525f9-458">The app requires package references for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) and [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="525f9-459">若要在於服務外執行時測試及偵錯，請新增程式碼以判斷應用程式是以服務形式執行或以主控台應用程式形式執行。</span><span class="sxs-lookup"><span data-stu-id="525f9-459">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="525f9-460">檢查偵錯工具是否已附加或 `--console` 切換參數是否存在。</span><span class="sxs-lookup"><span data-stu-id="525f9-460">Inspect if the debugger is attached or a `--console` switch is present.</span></span> <span data-ttu-id="525f9-461">若任一條件為真 (應用程式不是以服務形式執行)，請呼叫 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-461">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*>.</span></span> <span data-ttu-id="525f9-462">若條件為偽 (應用程式是以服務形式執行)：</span><span class="sxs-lookup"><span data-stu-id="525f9-462">If the conditions are false (the app is run as a service):</span></span>

* <span data-ttu-id="525f9-463">呼叫 <xref:System.IO.Directory.SetCurrentDirectory*> 並使用應用程式發佈位置的路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-463">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="525f9-464">請勿呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 來取得路徑，因為當呼叫 <xref:System.IO.Directory.GetCurrentDirectory*> 時，Windows 服務應用程式會傳回 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-464">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="525f9-465">如需詳細資訊，請參閱[目前目錄與內容根目錄](#current-directory-and-content-root)一節。</span><span class="sxs-lookup"><span data-stu-id="525f9-465">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span> <span data-ttu-id="525f9-466">在 `CreateWebHostBuilder` 中設定應用程式之前執行此步驟。</span><span class="sxs-lookup"><span data-stu-id="525f9-466">This step is performed before the app is configured in `CreateWebHostBuilder`.</span></span>
* <span data-ttu-id="525f9-467">呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 以服務形式執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-467">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

<span data-ttu-id="525f9-468">因為[命令列設定提供者](xref:fundamentals/configuration/index#command-line-configuration-provider)需要命令列引數的名稱值組，在 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> 收到引數之前，`--console` 切換參數就會從引數中移除。</span><span class="sxs-lookup"><span data-stu-id="525f9-468">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives the arguments.</span></span>

<span data-ttu-id="525f9-469">若要寫入 Windows 事件記錄檔，請新增 EventLog 提供者到 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-469">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="525f9-470">使用 *appsettings.Production.json* 檔案中的 `Logging:LogLevel:Default` 機碼設定記錄層級。</span><span class="sxs-lookup"><span data-stu-id="525f9-470">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span>

<span data-ttu-id="525f9-471">在範例應用程式的下列範例中，為了處理應用程式內的存留期事件，會呼叫 `RunAsCustomService` 而不是 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>。</span><span class="sxs-lookup"><span data-stu-id="525f9-471">In the following example from the sample app, `RunAsCustomService` is called instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in order to handle lifetime events within the app.</span></span> <span data-ttu-id="525f9-472">如需詳細資訊，請參閱[處理開始與停止事件](#handle-starting-and-stopping-events)一節。</span><span class="sxs-lookup"><span data-stu-id="525f9-472">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="deployment-type"></a><span data-ttu-id="525f9-473">部署類型</span><span class="sxs-lookup"><span data-stu-id="525f9-473">Deployment type</span></span>

<span data-ttu-id="525f9-474">如需詳細資訊與部署案例建議，請參閱 [.NET Core 應用程式部署](/dotnet/core/deploying/)。</span><span class="sxs-lookup"><span data-stu-id="525f9-474">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="sdk"></a><span data-ttu-id="525f9-475">SDK</span><span class="sxs-lookup"><span data-stu-id="525f9-475">SDK</span></span>

<span data-ttu-id="525f9-476">針對使用頁面或 MVC 架構的 web 應用程式服務 Razor ，請在專案檔中指定 WEB SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-476">For a web app-based service that uses the Razor Pages or MVC frameworks, specify the Web SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="525f9-477">如果服務只執行背景工作（例如，[託管服務](xref:fundamentals/host/hosted-services)），請在專案檔中指定工作者 SDK：</span><span class="sxs-lookup"><span data-stu-id="525f9-477">If the service only executes background tasks (for example, [hosted services](xref:fundamentals/host/hosted-services)), specify the Worker SDK in the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="525f9-478">架構相依部署 (FDD)</span><span class="sxs-lookup"><span data-stu-id="525f9-478">Framework-dependent deployment (FDD)</span></span>

<span data-ttu-id="525f9-479">架構相依部署 (FDD) 仰賴存在於目標系統上的全系統共用 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="525f9-479">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="525f9-480">依照此文章中的指導方針採用 FDD 案例時，SDK 會產生可執行檔 (*.exe*)，稱為「架構相依可執行檔」\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-480">When the FDD scenario is adopted following the guidance in this article, the SDK produces an executable (*.exe*), called a *framework-dependent executable*.</span></span>

<span data-ttu-id="525f9-481">Windows[執行時間識別碼（RID）](/dotnet/core/rid-catalog) （ [\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier) ）包含目標 framework。</span><span class="sxs-lookup"><span data-stu-id="525f9-481">The Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) ([\<RuntimeIdentifier>](/dotnet/core/tools/csproj#runtimeidentifier)) contains the target framework.</span></span> <span data-ttu-id="525f9-482">在下列範例中，RID 已設定為 `win7-x64`。</span><span class="sxs-lookup"><span data-stu-id="525f9-482">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="525f9-483">`<SelfContained>` 屬性設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="525f9-483">The `<SelfContained>` property is set to `false`.</span></span> <span data-ttu-id="525f9-484">這些屬性會指示 SDK，針對 Windows 和相依於共用 .NET Core framework 的應用程式產生可執行檔 (*.exe*)。</span><span class="sxs-lookup"><span data-stu-id="525f9-484">These properties instruct the SDK to generate an executable (*.exe*) file for Windows and an app that depends on the shared .NET Core framework.</span></span>

<span data-ttu-id="525f9-485">`<UseAppHost>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="525f9-485">The `<UseAppHost>` property is set to `true`.</span></span> <span data-ttu-id="525f9-486">此屬性為服務提供 FDD 的啟用路徑 (可執行檔 *.exe*)。</span><span class="sxs-lookup"><span data-stu-id="525f9-486">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

<span data-ttu-id="525f9-487">針對 Windows Services 應用程式，不需要 *web.config* 檔案 (發行 ASP.NET Core 應用程式時通常會產生此檔案)。</span><span class="sxs-lookup"><span data-stu-id="525f9-487">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="525f9-488">若要停用 *web.config* 檔案的建立，請新增 `<IsTransformWebConfigDisabled>` 屬性集到 `true`。</span><span class="sxs-lookup"><span data-stu-id="525f9-488">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

### <a name="self-contained-deployment-scd"></a><span data-ttu-id="525f9-489">自封式部署 (SCD)</span><span class="sxs-lookup"><span data-stu-id="525f9-489">Self-contained deployment (SCD)</span></span>

<span data-ttu-id="525f9-490">自封式部署 (SCD) 不仰賴任何存在於主機系統上的共用架構。</span><span class="sxs-lookup"><span data-stu-id="525f9-490">Self-contained deployment (SCD) doesn't rely on the presence of a shared framework on the host system.</span></span> <span data-ttu-id="525f9-491">執行階段與應用程式的相依性會隨著應用程式進行部署。</span><span class="sxs-lookup"><span data-stu-id="525f9-491">The runtime and the app's dependencies are deployed with the app.</span></span>

<span data-ttu-id="525f9-492">Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog) 會納入包含目標 Framework 的 `<PropertyGroup>` 中：</span><span class="sxs-lookup"><span data-stu-id="525f9-492">A Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) is included in the `<PropertyGroup>` that contains the target framework:</span></span>

```xml
<RuntimeIdentifier>win7-x64</RuntimeIdentifier>
```

<span data-ttu-id="525f9-493">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="525f9-493">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="525f9-494">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="525f9-494">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="525f9-495">使用屬性名稱 [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) （複數）。</span><span class="sxs-lookup"><span data-stu-id="525f9-495">Use the property name [\<RuntimeIdentifiers>](/dotnet/core/tools/csproj#runtimeidentifiers) (plural).</span></span>

<span data-ttu-id="525f9-496">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="525f9-496">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="525f9-497">`<SelfContained>` 屬性設定為 `true`：</span><span class="sxs-lookup"><span data-stu-id="525f9-497">A `<SelfContained>` property is set to `true`:</span></span>

```xml
<SelfContained>true</SelfContained>
```

## <a name="service-user-account"></a><span data-ttu-id="525f9-498">服務使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="525f9-498">Service user account</span></span>

<span data-ttu-id="525f9-499">若要為服務建立使用者帳戶，請從系統管理 PowerShell 6 命令殼層使用 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="525f9-499">To create a user account for a service, use the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell.</span></span>

<span data-ttu-id="525f9-500">Windows 10 2018 年 10 月更新 (版本 1809/組建 10.0.17763) 或更新版本：</span><span class="sxs-lookup"><span data-stu-id="525f9-500">On Windows 10 October 2018 Update (version 1809/build 10.0.17763) or later:</span></span>

```powershell
New-LocalUser -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-501">在 Windows 10 2018 年 10 月更新之前 (1809 版/組建 10.0.17763) 的 Windows OS 上：</span><span class="sxs-lookup"><span data-stu-id="525f9-501">On Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763):</span></span>

```console
powershell -Command "New-LocalUser -Name {SERVICE NAME}"
```

<span data-ttu-id="525f9-502">在系統提示時提供[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="525f9-502">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="525f9-503">除非搭配過期 <xref:System.DateTime> 將 `-AccountExpires` 參數提供給 [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) Cmdlet，否則該帳戶將不會過期。</span><span class="sxs-lookup"><span data-stu-id="525f9-503">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="525f9-504">如需詳細資訊，請參閱 [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) 和[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="525f9-504">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="525f9-505">使用 Active Directory 時有一個管理使用者的替代方法，就是使用「受控服務帳戶」。</span><span class="sxs-lookup"><span data-stu-id="525f9-505">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="525f9-506">如需詳細資訊，請參閱[群組受控服務帳戶概觀](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)。</span><span class="sxs-lookup"><span data-stu-id="525f9-506">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="log-on-as-a-service-rights"></a><span data-ttu-id="525f9-507">以服務方式登入權限</span><span class="sxs-lookup"><span data-stu-id="525f9-507">Log on as a service rights</span></span>

<span data-ttu-id="525f9-508">若要為服務使用者帳戶建立「以服務方式登入」\*\* 權限：</span><span class="sxs-lookup"><span data-stu-id="525f9-508">To establish *Log on as a service* rights for a service user account:</span></span>

1. <span data-ttu-id="525f9-509">執行 *secpol.msc* 來開啟 [本機安全性原則編輯器]。</span><span class="sxs-lookup"><span data-stu-id="525f9-509">Open the Local Security Policy editor by running *secpol.msc*.</span></span>
1. <span data-ttu-id="525f9-510">展開 [本機原則]\*\*\*\* 節點，然後選取 [使用者權限指派]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-510">Expand the **Local Policies** node and select **User Rights Assignment**.</span></span>
1. <span data-ttu-id="525f9-511">開啟 [以服務方式登入]\*\*\*\* 原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-511">Open the **Log on as a service** policy.</span></span>
1. <span data-ttu-id="525f9-512">選取 [新增使用者或群組]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-512">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="525f9-513">使用下列其中一種方法提供物件名稱 (使用者帳戶)：</span><span class="sxs-lookup"><span data-stu-id="525f9-513">Provide the object name (user account) using either of the following approaches:</span></span>
   1. <span data-ttu-id="525f9-514">在物件名稱欄位中輸入使用者帳戶 (`{DOMAIN OR COMPUTER NAME\USER}`)，然後選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-514">Type the user account (`{DOMAIN OR COMPUTER NAME\USER}`) in the object name field and select **OK** to add the user to the policy.</span></span>
   1. <span data-ttu-id="525f9-515">選取 [進階]  。</span><span class="sxs-lookup"><span data-stu-id="525f9-515">Select **Advanced**.</span></span> <span data-ttu-id="525f9-516">選取 [立即尋找]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="525f9-516">Select **Find Now**.</span></span> <span data-ttu-id="525f9-517">從清單中選取使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="525f9-517">Select the user account from the list.</span></span> <span data-ttu-id="525f9-518">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="525f9-518">Select **OK**.</span></span> <span data-ttu-id="525f9-519">再次選取 [確定]\*\*\*\* 將使用者新增至原則。</span><span class="sxs-lookup"><span data-stu-id="525f9-519">Select **OK** again to add the user to the policy.</span></span>
1. <span data-ttu-id="525f9-520">選取 [確定]\*\*\*\* 或 [套用]\*\*\*\* 以接受變更。</span><span class="sxs-lookup"><span data-stu-id="525f9-520">Select **OK** or **Apply** to accept the changes.</span></span>

## <a name="create-and-manage-the-windows-service"></a><span data-ttu-id="525f9-521">建立及管理 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="525f9-521">Create and manage the Windows Service</span></span>

### <a name="create-a-service"></a><span data-ttu-id="525f9-522">建立服務</span><span class="sxs-lookup"><span data-stu-id="525f9-522">Create a service</span></span>

<span data-ttu-id="525f9-523">使用 PowerShell 命令來註冊服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-523">Use PowerShell commands to register a service.</span></span> <span data-ttu-id="525f9-524">透過系統管理 PowerShell 6 命令殼層，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-524">From an administrative PowerShell 6 command shell, execute the following commands:</span></span>

```powershell
$acl = Get-Acl "{EXE PATH}"
$aclRuleArgs = {DOMAIN OR COMPUTER NAME\USER}, "Read,Write,ReadAndExecute", "ContainerInherit,ObjectInherit", "None", "Allow"
$accessRule = New-Object System.Security.AccessControl.FileSystemAccessRule($aclRuleArgs)
$acl.SetAccessRule($accessRule)
$acl | Set-Acl "{EXE PATH}"

New-Service -Name {SERVICE NAME} -BinaryPathName {EXE FILE PATH} -Credential {DOMAIN OR COMPUTER NAME\USER} -Description "{DESCRIPTION}" -DisplayName "{DISPLAY NAME}" -StartupType Automatic
```

* <span data-ttu-id="525f9-525">`{EXE PATH}`：主機上的應用程式資料夾路徑（例如， `d:\myservice` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-525">`{EXE PATH}`: Path to the app's folder on the host (for example, `d:\myservice`).</span></span> <span data-ttu-id="525f9-526">請勿包含路徑中應用程式的可執行檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-526">Don't include the app's executable in the path.</span></span> <span data-ttu-id="525f9-527">不需要結尾的斜線。</span><span class="sxs-lookup"><span data-stu-id="525f9-527">A trailing slash isn't required.</span></span>
* <span data-ttu-id="525f9-528">`{DOMAIN OR COMPUTER NAME\USER}`：服務使用者帳戶（例如 `Contoso\ServiceUser` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-528">`{DOMAIN OR COMPUTER NAME\USER}`: Service user account (for example, `Contoso\ServiceUser`).</span></span>
* <span data-ttu-id="525f9-529">`{SERVICE NAME}`：服務名稱（例如， `MyService` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-529">`{SERVICE NAME}`: Service name (for example, `MyService`).</span></span>
* <span data-ttu-id="525f9-530">`{EXE FILE PATH}`：應用程式的可執行檔路徑（例如， `d:\myservice\myservice.exe` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-530">`{EXE FILE PATH}`: The app's executable path (for example, `d:\myservice\myservice.exe`).</span></span> <span data-ttu-id="525f9-531">包含可執行檔的檔案名稱 (包含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="525f9-531">Include the executable's file name with extension.</span></span>
* <span data-ttu-id="525f9-532">`{DESCRIPTION}`：服務描述（例如， `My sample service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-532">`{DESCRIPTION}`: Service description (for example, `My sample service`).</span></span>
* <span data-ttu-id="525f9-533">`{DISPLAY NAME}`：服務顯示名稱（例如， `My Service` ）。</span><span class="sxs-lookup"><span data-stu-id="525f9-533">`{DISPLAY NAME}`: Service display name (for example, `My Service`).</span></span>

### <a name="start-a-service"></a><span data-ttu-id="525f9-534">啟動服務</span><span class="sxs-lookup"><span data-stu-id="525f9-534">Start a service</span></span>

<span data-ttu-id="525f9-535">以下列 PowerShell 6 命令啟動服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-535">Start a service with the following PowerShell 6 command:</span></span>

```powershell
Start-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-536">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="525f9-536">The command takes a few seconds to start the service.</span></span>

### <a name="determine-a-services-status"></a><span data-ttu-id="525f9-537">判斷服務的狀態</span><span class="sxs-lookup"><span data-stu-id="525f9-537">Determine a service's status</span></span>

<span data-ttu-id="525f9-538">若要檢查服務狀態，請使用下列 PowerShell 6 命令：</span><span class="sxs-lookup"><span data-stu-id="525f9-538">To check the status of a service, use the following PowerShell 6 command:</span></span>

```powershell
Get-Service -Name {SERVICE NAME}
```

<span data-ttu-id="525f9-539">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="525f9-539">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

### <a name="stop-a-service"></a><span data-ttu-id="525f9-540">停止服務</span><span class="sxs-lookup"><span data-stu-id="525f9-540">Stop a service</span></span>

<span data-ttu-id="525f9-541">使用下列 Powershell 6 命令停止服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-541">Stop a service with the following Powershell 6 command:</span></span>

```powershell
Stop-Service -Name {SERVICE NAME}
```

### <a name="remove-a-service"></a><span data-ttu-id="525f9-542">移除服務</span><span class="sxs-lookup"><span data-stu-id="525f9-542">Remove a service</span></span>

<span data-ttu-id="525f9-543">在停止服務的短暫延遲之後，請以下列 Powershell 6 命令移除服務：</span><span class="sxs-lookup"><span data-stu-id="525f9-543">After a short delay to stop a service, remove a service with the following Powershell 6 command:</span></span>

```powershell
Remove-Service -Name {SERVICE NAME}
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="525f9-544">處理開始與停止事件</span><span class="sxs-lookup"><span data-stu-id="525f9-544">Handle starting and stopping events</span></span>

<span data-ttu-id="525f9-545">若要處理 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>、<xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> 與 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> 事件：</span><span class="sxs-lookup"><span data-stu-id="525f9-545">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events:</span></span>

1. <span data-ttu-id="525f9-546">使用 `OnStarting`、`OnStarted` 與 `OnStopping` 方法建立衍生自 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> 的類別：</span><span class="sxs-lookup"><span data-stu-id="525f9-546">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="525f9-547">為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 建立一個會將 `CustomWebHostService` 傳遞給 <xref:System.ServiceProcess.ServiceBase.Run*> 的擴充方法：</span><span class="sxs-lookup"><span data-stu-id="525f9-547">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="525f9-548">在 `Program.Main` 中，呼叫 `RunAsCustomService` 擴充方法，而不是呼叫 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>：</span><span class="sxs-lookup"><span data-stu-id="525f9-548">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="525f9-549">若要查看 `Program.Main` 中的 <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> 位置，請參閱[部署類型](#deployment-type)一節中的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="525f9-549">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Deployment type](#deployment-type) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="525f9-550">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="525f9-550">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="525f9-551">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-551">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="525f9-552">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="525f9-552">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-endpoints"></a><span data-ttu-id="525f9-553">設定端點</span><span class="sxs-lookup"><span data-stu-id="525f9-553">Configure endpoints</span></span>

<span data-ttu-id="525f9-554">ASP.NET Core 預設會繫結至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="525f9-554">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="525f9-555">設定環境變數，以設定 URL 和埠 `ASPNETCORE_URLS` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-555">Configure the URL and port by setting the `ASPNETCORE_URLS` environment variable.</span></span>

<span data-ttu-id="525f9-556">如需其他 URL 和埠設定方法，請參閱相關的伺服器文章：</span><span class="sxs-lookup"><span data-stu-id="525f9-556">For additional URL and port configuration approaches, see the relevant server article:</span></span>

* <xref:fundamentals/servers/kestrel#endpoint-configuration>
* <xref:fundamentals/servers/httpsys#configure-windows-server>

<span data-ttu-id="525f9-557">前述指引涵蓋 HTTPS 端點的支援。</span><span class="sxs-lookup"><span data-stu-id="525f9-557">The preceding guidance covers support for HTTPS endpoints.</span></span> <span data-ttu-id="525f9-558">例如，當搭配 Windows 服務使用驗證時，請為 HTTPS 設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-558">For example, configure the app for HTTPS when authentication is used with a Windows Service.</span></span>

> [!NOTE]
> <span data-ttu-id="525f9-559">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="525f9-559">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="525f9-560">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="525f9-560">Current directory and content root</span></span>

<span data-ttu-id="525f9-561">呼叫 Windows 服務所傳回的目前工作目錄 <xref:System.IO.Directory.GetCurrentDirectory*> 是*C： \\ Windows \\ system32*資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-561">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="525f9-562">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="525f9-562">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="525f9-563">使用下列其中一個方式來維護及存取服務的資產與設定檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-563">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="525f9-564">將內容根目錄路徑設定到應用程式的資料夾</span><span class="sxs-lookup"><span data-stu-id="525f9-564">Set the content root path to the app's folder</span></span>

<span data-ttu-id="525f9-565"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="525f9-565">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when a service is created.</span></span> <span data-ttu-id="525f9-566">`GetCurrentDirectory`請 <xref:System.IO.Directory.SetCurrentDirectory*> 使用應用程式[內容根目錄](xref:fundamentals/index#content-root)的路徑呼叫，而不是呼叫來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-566">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's [content root](xref:fundamentals/index#content-root).</span></span>

<span data-ttu-id="525f9-567">在 `Program.Main` 中，判斷服務可執行檔資料夾的路徑，然後使用該路徑來建立應用程式的內容根目錄：</span><span class="sxs-lookup"><span data-stu-id="525f9-567">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-a-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="525f9-568">將服務的檔案儲存在磁碟上的適當位置</span><span class="sxs-lookup"><span data-stu-id="525f9-568">Store a service's files in a suitable location on disk</span></span>

<span data-ttu-id="525f9-569">使用包含檔案的 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 資料夾，使用 <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> 來指定絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="525f9-569">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="525f9-570">疑難排解</span><span class="sxs-lookup"><span data-stu-id="525f9-570">Troubleshoot</span></span>

<span data-ttu-id="525f9-571">若要疑難排解 Windows 服務應用程式，請參閱 <xref:test/troubleshoot> 。</span><span class="sxs-lookup"><span data-stu-id="525f9-571">To troubleshoot a Windows Service app, see <xref:test/troubleshoot>.</span></span>

### <a name="common-errors"></a><span data-ttu-id="525f9-572">常見錯誤</span><span class="sxs-lookup"><span data-stu-id="525f9-572">Common errors</span></span>

* <span data-ttu-id="525f9-573">舊版或發行前版本的 PowerShell 已在使用中。</span><span class="sxs-lookup"><span data-stu-id="525f9-573">An old or pre-release version of PowerShell is in use.</span></span>
* <span data-ttu-id="525f9-574">已註冊的服務不會使用來自[dotnet publish](/dotnet/core/tools/dotnet-publish)命令的應用程式**已發佈**輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-574">The registered service doesn't use the app's **published** output from the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="525f9-575">應用程式部署不支援[dotnet build](/dotnet/core/tools/dotnet-build)命令的輸出。</span><span class="sxs-lookup"><span data-stu-id="525f9-575">Output of the [dotnet build](/dotnet/core/tools/dotnet-build) command isn't supported for app deployment.</span></span> <span data-ttu-id="525f9-576">根據部署類型，可以在下列其中一個資料夾中找到已發佈的資產：</span><span class="sxs-lookup"><span data-stu-id="525f9-576">Published assets are found in either of the following folders depending on the deployment type:</span></span>
  * <span data-ttu-id="525f9-577">*bin/Release/{TARGET FRAMEWORK}/publish* （FDD）</span><span class="sxs-lookup"><span data-stu-id="525f9-577">*bin/Release/{TARGET FRAMEWORK}/publish* (FDD)</span></span>
  * <span data-ttu-id="525f9-578">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* （SCD）</span><span class="sxs-lookup"><span data-stu-id="525f9-578">*bin/Release/{TARGET FRAMEWORK}/{RUNTIME IDENTIFIER}/publish* (SCD)</span></span>
* <span data-ttu-id="525f9-579">服務不是處於執行中狀態。</span><span class="sxs-lookup"><span data-stu-id="525f9-579">The service isn't in the RUNNING state.</span></span>
* <span data-ttu-id="525f9-580">應用程式所使用資源的路徑（例如憑證）不正確。</span><span class="sxs-lookup"><span data-stu-id="525f9-580">The paths to resources that the app uses (for example, certificates) are incorrect.</span></span> <span data-ttu-id="525f9-581">Windows 服務的基底路徑是*c： \\ windows \\ System32*。</span><span class="sxs-lookup"><span data-stu-id="525f9-581">The base path of a Windows Service is *c:\\Windows\\System32*.</span></span>
* <span data-ttu-id="525f9-582">使用者不具有 [*以服務方式登*入] 許可權。</span><span class="sxs-lookup"><span data-stu-id="525f9-582">The user doesn't have *Log on as a service* rights.</span></span>
* <span data-ttu-id="525f9-583">執行 PowerShell 命令時，使用者的密碼已過期或傳遞錯誤 `New-Service` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-583">The user's password is expired or incorrectly passed when executing the `New-Service` PowerShell command.</span></span>
* <span data-ttu-id="525f9-584">應用程式需要 ASP.NET Core 驗證，但未設定安全連線（HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="525f9-584">The app requires ASP.NET Core authentication but isn't configured for secure connections (HTTPS).</span></span>
* <span data-ttu-id="525f9-585">要求 URL 埠不正確，或未在應用程式中正確設定。</span><span class="sxs-lookup"><span data-stu-id="525f9-585">The request URL port is incorrect or not configured correctly in the app.</span></span>

### <a name="system-and-application-event-logs"></a><span data-ttu-id="525f9-586">系統和應用程式事件記錄檔</span><span class="sxs-lookup"><span data-stu-id="525f9-586">System and Application Event Logs</span></span>

<span data-ttu-id="525f9-587">存取系統和應用程式事件記錄檔：</span><span class="sxs-lookup"><span data-stu-id="525f9-587">Access the System and Application Event Logs:</span></span>

1. <span data-ttu-id="525f9-588">開啟 [開始] 功能表，搜尋 [*事件檢視器*]，然後選取 [**事件檢視器**] 應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-588">Open the Start menu, search for *Event Viewer*, and select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="525f9-589">在 [事件檢視器]\*\*\*\* 中，開啟 [Windows 記錄]\*\*\*\* 節點。</span><span class="sxs-lookup"><span data-stu-id="525f9-589">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="525f9-590">選取 [**系統**] 以開啟 [系統] 事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="525f9-590">Select **System** to open the System Event Log.</span></span> <span data-ttu-id="525f9-591">選取 [應用程式]\*\*\*\* 以開啟「應用程式事件記錄檔」。</span><span class="sxs-lookup"><span data-stu-id="525f9-591">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="525f9-592">搜尋與失敗應用程式相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="525f9-592">Search for errors associated with the failing app.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="525f9-593">在命令提示字元中執行應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-593">Run the app at a command prompt</span></span>

<span data-ttu-id="525f9-594">許多啟動錯誤不會在事件記錄檔中產生有用的資訊。</span><span class="sxs-lookup"><span data-stu-id="525f9-594">Many startup errors don't produce useful information in the event logs.</span></span> <span data-ttu-id="525f9-595">您可以藉由在主控系統上的命令提示字元中執行應用程式，來找出一些錯誤的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-595">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span> <span data-ttu-id="525f9-596">若要從應用程式記錄其他詳細資料，請降低[記錄層級](xref:fundamentals/logging/index#log-level)，或在[開發環境](xref:fundamentals/environments)中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-596">To log additional detail from the app, lower the [log level](xref:fundamentals/logging/index#log-level) or run the app in the [Development environment](xref:fundamentals/environments).</span></span>

### <a name="clear-package-caches"></a><span data-ttu-id="525f9-597">清除套件快取</span><span class="sxs-lookup"><span data-stu-id="525f9-597">Clear package caches</span></span>

<span data-ttu-id="525f9-598">升級開發電腦上的 .NET Core SDK 或變更應用程式內的套件版本之後，正常運作的應用程式可能會立即失敗。</span><span class="sxs-lookup"><span data-stu-id="525f9-598">A functioning app may fail immediately after upgrading either the .NET Core SDK on the development machine or changing package versions within the app.</span></span> <span data-ttu-id="525f9-599">在某些情況下，執行主要升級時，不一致的套件可能會中斷應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-599">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="525f9-600">大多數這些問題都可依照下列指示來進行修正：</span><span class="sxs-lookup"><span data-stu-id="525f9-600">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="525f9-601">刪除 [bin]\*\* 和 [obj]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="525f9-601">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="525f9-602">從命令 shell 執行[dotnet nuget 區域變數 all--clear](/dotnet/core/tools/dotnet-nuget-locals) ，以清除套件快取。</span><span class="sxs-lookup"><span data-stu-id="525f9-602">Clear the package caches by executing [dotnet nuget locals all --clear](/dotnet/core/tools/dotnet-nuget-locals) from a command shell.</span></span>

   <span data-ttu-id="525f9-603">清除套件快取也可以使用[nuget.exe](https://www.nuget.org/downloads)工具來完成，並執行命令 `nuget locals all -clear` 。</span><span class="sxs-lookup"><span data-stu-id="525f9-603">Clearing package caches can also be accomplished with the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="525f9-604">*nuget.exe* 並未隨附在 Windows 桌面作業系統的安裝中，必須另外從 [NuGet 網站](https://www.nuget.org/downloads)取得。</span><span class="sxs-lookup"><span data-stu-id="525f9-604">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

1. <span data-ttu-id="525f9-605">還原並重建專案。</span><span class="sxs-lookup"><span data-stu-id="525f9-605">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="525f9-606">重新部署應用程式之前，請先刪除伺服器上 [部署] 資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-606">Delete all of the files in the deployment folder on the server prior to redeploying the app.</span></span>

### <a name="slow-or-hanging-app"></a><span data-ttu-id="525f9-607">回應緩慢或無回應的應用程式</span><span class="sxs-lookup"><span data-stu-id="525f9-607">Slow or hanging app</span></span>

<span data-ttu-id="525f9-608">損*毀*傾印是系統記憶體的快照集，有助於判斷應用程式損毀、啟動失敗或應用程式緩慢的原因。</span><span class="sxs-lookup"><span data-stu-id="525f9-608">A *crash dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

#### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="525f9-609">應用程式損毀或發生例外狀況</span><span class="sxs-lookup"><span data-stu-id="525f9-609">App crashes or encounters an exception</span></span>

<span data-ttu-id="525f9-610">從 [Windows 錯誤報告 (WER)](/windows/desktop/wer/windows-error-reporting) 取得並分析傾印：</span><span class="sxs-lookup"><span data-stu-id="525f9-610">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="525f9-611">在 `c:\dumps` 中建立資料夾以保存損毀傾印檔案。</span><span class="sxs-lookup"><span data-stu-id="525f9-611">Create a folder to hold crash dump files at `c:\dumps`.</span></span>
1. <span data-ttu-id="525f9-612">以應用程式可執行檔名稱執行[EnableDumps PowerShell 腳本](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-612">Run the [EnableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/EnableDumps.ps1) with the application executable name:</span></span>

   ```console
   .\EnableDumps {APPLICATION EXE} c:\dumps
   ```

1. <span data-ttu-id="525f9-613">在會導致損毀的情況下，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-613">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="525f9-614">發生損毀之後，請執行 [DisableDumps PowerShell 指令碼](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1)：</span><span class="sxs-lookup"><span data-stu-id="525f9-614">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/windows-service/scripts/DisableDumps.ps1):</span></span>

   ```console
   .\DisableDumps {APPLICATION EXE}
   ```

<span data-ttu-id="525f9-615">在應用程式損毀且傾印收集完成之後，即可正常結束應用程式。</span><span class="sxs-lookup"><span data-stu-id="525f9-615">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="525f9-616">PowerShell 指令碼會將 WER 設定為每個應用程式收集最多五個傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-616">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="525f9-617">損毀傾印可能會佔用大量磁碟空間 (每個高達好幾 GB)。</span><span class="sxs-lookup"><span data-stu-id="525f9-617">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

#### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="525f9-618">應用程式停止回應、在啟動期間失敗，或正常執行</span><span class="sxs-lookup"><span data-stu-id="525f9-618">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="525f9-619">當*應用程式*當機（停止回應但未損毀）、啟動期間失敗，或正常執行時，請參閱使用者模式傾印檔案[：選擇最適合的工具](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool)來選取適當的工具以產生傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-619">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

#### <a name="analyze-the-dump"></a><span data-ttu-id="525f9-620">分析傾印</span><span class="sxs-lookup"><span data-stu-id="525f9-620">Analyze the dump</span></span>

<span data-ttu-id="525f9-621">您可以使用數種方法來分析傾印。</span><span class="sxs-lookup"><span data-stu-id="525f9-621">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="525f9-622">如需詳細資訊，請參閱[分析使用者模式傾印檔案](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file)。</span><span class="sxs-lookup"><span data-stu-id="525f9-622">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="525f9-623">其他資源</span><span class="sxs-lookup"><span data-stu-id="525f9-623">Additional resources</span></span>

* <span data-ttu-id="525f9-624">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="525f9-624">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>

::: moniker-end
