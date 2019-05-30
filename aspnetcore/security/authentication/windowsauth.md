---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 了解如何設定 ASP.NET Core 中的 Windows 驗證的 IIS 和 HTTP.sys。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/29/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9dfff5dcba409ddca7e05c771b864ab121e0ea85
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395932"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="59c20-103">在 ASP.NET Core 中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="59c20-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="59c20-104">作者：[Scott Addie](https://twitter.com/Scott_Addie) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="59c20-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="59c20-105">[Windows 驗證](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) 可以針對 [IIS](xref:host-and-deploy/iis/index) 或 [HTTP.sys](xref:fundamentals/servers/httpsys) 上裝載的 ASP.NET Core 應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="59c20-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="59c20-106">Windows 驗證仰賴作業系統來驗證 ASP.NET Core 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="59c20-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="59c20-107">當您的伺服器在公司網路上執行時，您可以透過 Active Directory 網域身分識別進行 Windows 驗證或透過 Windows 帳戶來識別使用者。</span><span class="sxs-lookup"><span data-stu-id="59c20-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="59c20-108">Windows 驗證最適合用於使用者、用戶端應用程式與 Web 伺服器皆屬於相同 Windows 網域的內部網路環境。</span><span class="sxs-lookup"><span data-stu-id="59c20-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="launch-settings-debugger"></a><span data-ttu-id="59c20-109">啟動設定 （偵錯工具）</span><span class="sxs-lookup"><span data-stu-id="59c20-109">Launch settings (debugger)</span></span>

<span data-ttu-id="59c20-110">啟動設定的設定只會影響*Properties/launchSettings.json*檔案，並不會將 IIS 或 HTTP.sys 伺服器設定為 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="59c20-110">Configuration for launch settings only affects the *Properties/launchSettings.json* file and doesn't configure the IIS or HTTP.sys server for Windows Authentication.</span></span> <span data-ttu-id="59c20-111">伺服器的設定會說明[啟用的驗證服務的 IIS 或 HTTP.sys](#authentication-services-for-iis-or-httpsys)一節。</span><span class="sxs-lookup"><span data-stu-id="59c20-111">Configuration of the server is explained in the [Enable authentication services for IIS or HTTP.sys](#authentication-services-for-iis-or-httpsys) section.</span></span>

<span data-ttu-id="59c20-112">**Web 應用程式**可透過 Visual Studio 或.NET Core CLI 的範本可以設定為支援 Windows 驗證，這會更新*Properties/launchSettings.json*檔案自動的。</span><span class="sxs-lookup"><span data-stu-id="59c20-112">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59c20-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59c20-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="59c20-114">**新的專案**</span><span class="sxs-lookup"><span data-stu-id="59c20-114">**New project**</span></span>

1. <span data-ttu-id="59c20-115">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="59c20-115">Create a new project.</span></span>
1. <span data-ttu-id="59c20-116">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="59c20-117">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-117">Select **Next**.</span></span>
1. <span data-ttu-id="59c20-118">提供的名稱**專案名稱**欄位。</span><span class="sxs-lookup"><span data-stu-id="59c20-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="59c20-119">確認**位置**項目是否正確，或提供專案的位置。</span><span class="sxs-lookup"><span data-stu-id="59c20-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="59c20-120">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-120">Select **Create**.</span></span>
1. <span data-ttu-id="59c20-121">選取 **變更**下方**驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-121">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="59c20-122">在 **變更驗證**視窗中，選取**Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-122">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="59c20-123">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-123">Select **OK**.</span></span>
1. <span data-ttu-id="59c20-124">選取 [Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-124">Select **Web Application**.</span></span>
1. <span data-ttu-id="59c20-125">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-125">Select **Create**.</span></span>

<span data-ttu-id="59c20-126">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="59c20-126">Run the app.</span></span> <span data-ttu-id="59c20-127">使用者名稱會出現在呈現的應用程式使用者介面。</span><span class="sxs-lookup"><span data-stu-id="59c20-127">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="59c20-128">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="59c20-128">**Existing project**</span></span>

<span data-ttu-id="59c20-129">專案的屬性會啟用 Windows 驗證，並停用匿名驗證：</span><span class="sxs-lookup"><span data-stu-id="59c20-129">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="59c20-130">以滑鼠右鍵按一下 [方案總管]  中的專案，然後選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="59c20-130">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="59c20-131">選取 [偵錯]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="59c20-131">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="59c20-132">清除核取方塊**啟用匿名驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-132">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="59c20-133">選取核取方塊**啟用 Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-133">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="59c20-134">儲存並關閉 [屬性] 頁面。</span><span class="sxs-lookup"><span data-stu-id="59c20-134">Save and close the property page.</span></span>

<span data-ttu-id="59c20-135">或者，設定屬性，在`iisSettings`的節點*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="59c20-135">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="59c20-136">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="59c20-136">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="59c20-137">**新的專案**</span><span class="sxs-lookup"><span data-stu-id="59c20-137">**New project**</span></span>

<span data-ttu-id="59c20-138">執行[dotnet 新](/dotnet/core/tools/dotnet-new)命令搭配`webapp`引數 （ASP.NET Core Web 應用程式） 和`--auth Windows`切換：</span><span class="sxs-lookup"><span data-stu-id="59c20-138">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="59c20-139">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="59c20-139">**Existing project**</span></span>

<span data-ttu-id="59c20-140">更新`iisSettings`的節點*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="59c20-140">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="59c20-141">當修改現有的專案，請確認專案檔包含的套件參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)**或是** [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="59c20-141">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="authentication-services-for-iis-or-httpsys"></a><span data-ttu-id="59c20-142">IIS 或 HTTP.sys 的驗證服務</span><span class="sxs-lookup"><span data-stu-id="59c20-142">Authentication services for IIS or HTTP.sys</span></span>

<span data-ttu-id="59c20-143">根據裝載的案例，請依照下列中的指導方針**任一** [IIS](#iis)區段**或是** [HTTP.sys](#httpsys)一節。</span><span class="sxs-lookup"><span data-stu-id="59c20-143">Depending on the hosting scenario, follow the guidance in **either** the [IIS](#iis) section **or** [HTTP.sys](#httpsys) section.</span></span>

### <a name="iis"></a><span data-ttu-id="59c20-144">IIS</span><span class="sxs-lookup"><span data-stu-id="59c20-144">IIS</span></span>

<span data-ttu-id="59c20-145">加入驗證服務所叫用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName>命名空間) 中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59c20-145">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="59c20-146">IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主機 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="59c20-146">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="59c20-147">Windows 驗證針對透過 IIS *web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-147">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="59c20-148">下列各節將示範如何：</span><span class="sxs-lookup"><span data-stu-id="59c20-148">The following sections show how to:</span></span>

* <span data-ttu-id="59c20-149">提供本機*web.config*部署應用程式時，請在伺服器啟動 Windows 驗證的檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-149">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="59c20-150">使用 IIS 管理員設定*web.config*已經部署到伺服器的 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-150">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="59c20-151">如果您尚未這麼做，請啟用 IIS 可裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="59c20-151">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="59c20-152">如需詳細資訊，請參閱<xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="59c20-152">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="59c20-153">啟用 Windows 驗證的 IIS 角色服務。</span><span class="sxs-lookup"><span data-stu-id="59c20-153">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="59c20-154">如需詳細資訊，請參閱 <<c0> [ 啟用 IIS 角色服務 （請參閱步驟 2） 中的 Windows 驗證](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="59c20-154">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="59c20-155">[IIS Integration 中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)預設設定來自動驗證要求。</span><span class="sxs-lookup"><span data-stu-id="59c20-155">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="59c20-156">如需詳細資訊，請參閱[裝載 ASP.NET Core 與 IIS 的 Windows 上：IIS 選項 (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="59c20-156">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="59c20-157">ASP.NET Core 模組預設設定為轉送至應用程式的 Windows 驗證語彙基元。</span><span class="sxs-lookup"><span data-stu-id="59c20-157">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="59c20-158">如需詳細資訊，請參閱[ASP.NET Core 模組組態參考：AspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="59c20-158">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="59c20-159">使用**任一**下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="59c20-159">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="59c20-160">**發行和部署專案，再**新增下列*web.config*至專案根目錄的檔案：</span><span class="sxs-lookup"><span data-stu-id="59c20-160">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="59c20-161">當專案發行.NET Core SDK (不含`<IsTransformWebConfigDisabled>`屬性設定為`true`專案檔中)，發行*web.config*檔案包含`<location><system.webServer><security><authentication>`一節。</span><span class="sxs-lookup"><span data-stu-id="59c20-161">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="59c20-162">如需詳細資訊`<IsTransformWebConfigDisabled>`屬性，請參閱<xref:host-and-deploy/iis/index#webconfig-file>。</span><span class="sxs-lookup"><span data-stu-id="59c20-162">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="59c20-163">**發行和部署專案之後,** 執行伺服器端設定使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="59c20-163">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="59c20-164">在 [IIS 管理員] 中，選取 IIS 站台之下**站台**節點**連線**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="59c20-164">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="59c20-165">按兩下**驗證**中**IIS**區域。</span><span class="sxs-lookup"><span data-stu-id="59c20-165">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="59c20-166">選取 **匿名驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-166">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="59c20-167">選取 **停用**中**動作**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="59c20-167">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="59c20-168">選取  **Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="59c20-168">Select **Windows Authentication**.</span></span> <span data-ttu-id="59c20-169">選取 **啟用**中**動作**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="59c20-169">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="59c20-170">IIS 管理員在採取這些動作，會修改應用程式的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-170">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="59c20-171">A`<system.webServer><security><authentication>`節點新增與更新的設定，如`anonymousAuthentication`和`windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="59c20-171">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="59c20-172">`<system.webServer>`區段新增至*web.config*由 IIS 管理員中的檔案超出應用程式的`<location>`發佈應用程式時，由.NET Core SDK 加入的區段。</span><span class="sxs-lookup"><span data-stu-id="59c20-172">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="59c20-173">因為區段會新增外部`<location>`節點，設定會由任何繼承[子應用程式](xref:host-and-deploy/iis/index#sub-applications)目前的應用程式。</span><span class="sxs-lookup"><span data-stu-id="59c20-173">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="59c20-174">若要防止繼承，移動加入`<security>`區段內的`<location><system.webServer>`.NET Core SDK 提供的一節。</span><span class="sxs-lookup"><span data-stu-id="59c20-174">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="59c20-175">將 IIS 設定使用 IIS 管理員時，它只會影響應用程式的*web.config*伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-175">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="59c20-176">後續部署應用程式可能會覆寫伺服器上的設定，如果伺服器的複本*web.config*專案的取代*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-176">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="59c20-177">使用**任一**下列其中一個方法來管理設定：</span><span class="sxs-lookup"><span data-stu-id="59c20-177">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="59c20-178">使用 IIS 管理員中的設定重設*web.config*檔案之後部署上覆寫該檔案。</span><span class="sxs-lookup"><span data-stu-id="59c20-178">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="59c20-179">新增*web.config 檔案*應用程式在本機使用的設定。</span><span class="sxs-lookup"><span data-stu-id="59c20-179">Add a *web.config file* to the app locally with the settings.</span></span>

### <a name="httpsys"></a><span data-ttu-id="59c20-180">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="59c20-180">HTTP.sys</span></span>

<span data-ttu-id="59c20-181">雖然[Kestrel](xref:fundamentals/servers/kestrel)不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)支援在 Windows 上的自我裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="59c20-181">Although [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span>

<span data-ttu-id="59c20-182">加入驗證服務所叫用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>命名空間) 中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="59c20-182">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="59c20-183">使用 Windows 驗證使用 HTTP.sys 的應用程式的 web 主機設定 (*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="59c20-183">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="59c20-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 處於<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>命名空間。</span><span class="sxs-lookup"><span data-stu-id="59c20-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="59c20-185">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="59c20-185">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="59c20-186">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="59c20-186">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="59c20-187">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="59c20-187">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="59c20-188">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="59c20-188">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="59c20-189">HTTP.sys 不支援 Nano Server 1709 版或更新版本上。</span><span class="sxs-lookup"><span data-stu-id="59c20-189">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="59c20-190">若要使用 Windows 驗證和 HTTP.sys 使用 Nano Server，請使用[Server Core (microsoft/windowsservercore) 容器](https://hub.docker.com/r/microsoft/windowsservercore/)。</span><span class="sxs-lookup"><span data-stu-id="59c20-190">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="59c20-191">如需有關 Server Core 的詳細資訊，請參閱[什麼是 Windows Server 中的 Server Core 安裝選項？](/windows-server/administration/server-core/what-is-server-core)。</span><span class="sxs-lookup"><span data-stu-id="59c20-191">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="59c20-192">授權使用者</span><span class="sxs-lookup"><span data-stu-id="59c20-192">Authorize users</span></span>

<span data-ttu-id="59c20-193">匿名存取的設定狀態決定的方式`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。</span><span class="sxs-lookup"><span data-stu-id="59c20-193">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="59c20-194">下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。</span><span class="sxs-lookup"><span data-stu-id="59c20-194">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="59c20-195">不允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="59c20-195">Disallow anonymous access</span></span>

<span data-ttu-id="59c20-196">當您啟用 Windows 驗證，並已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`屬性沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="59c20-196">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="59c20-197">如果 IIS 站台設定為不允許匿名存取，要求永遠不會到達應用程式。</span><span class="sxs-lookup"><span data-stu-id="59c20-197">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="59c20-198">基於這個理由，`[AllowAnonymous]`屬性不適用。</span><span class="sxs-lookup"><span data-stu-id="59c20-198">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="59c20-199">允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="59c20-199">Allow anonymous access</span></span>

<span data-ttu-id="59c20-200">當啟用 Windows 驗證和匿名存取時，使用`[Authorize]`和`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="59c20-200">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="59c20-201">`[Authorize]`屬性可讓您保護應用程式需要驗證的端點。</span><span class="sxs-lookup"><span data-stu-id="59c20-201">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="59c20-202">`[AllowAnonymous]`屬性覆寫`[Authorize]`允許匿名存取的應用程式中的屬性。</span><span class="sxs-lookup"><span data-stu-id="59c20-202">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="59c20-203">屬性使用方式詳細資料，請參閱<xref:security/authorization/simple>。</span><span class="sxs-lookup"><span data-stu-id="59c20-203">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="59c20-204">根據預設，缺少授權，才能存取頁面的使用者會看到空的 HTTP 403 回應。</span><span class="sxs-lookup"><span data-stu-id="59c20-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="59c20-205">[StatusCodePages 中介軟體](xref:fundamentals/error-handling#usestatuscodepages)可以設定為使用者提供更好的 「 拒絕存取 」 體驗。</span><span class="sxs-lookup"><span data-stu-id="59c20-205">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="59c20-206">模擬</span><span class="sxs-lookup"><span data-stu-id="59c20-206">Impersonation</span></span>

<span data-ttu-id="59c20-207">ASP.NET Core 不會實作模擬。</span><span class="sxs-lookup"><span data-stu-id="59c20-207">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="59c20-208">應用程式執行的所有要求，使用應用程式集區或處理序身分識別的應用程式的身分識別。</span><span class="sxs-lookup"><span data-stu-id="59c20-208">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="59c20-209">如果應用程式應該執行代表使用者的動作，使用[WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)中[終端機內嵌中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)在`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="59c20-209">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="59c20-210">在此內容中執行單一動作，然後關閉 內容。</span><span class="sxs-lookup"><span data-stu-id="59c20-210">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="59c20-211">`RunImpersonated` 不支援非同步作業，而不應該用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="59c20-211">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="59c20-212">比方說，包裝整個要求或中介軟體鏈結不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="59c20-212">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="59c20-213">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="59c20-213">Claims transformations</span></span>

<span data-ttu-id="59c20-214">當 IIS 同處理序模式中，裝載<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>不在內部呼叫以初始化使用者。</span><span class="sxs-lookup"><span data-stu-id="59c20-214">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="59c20-215">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="59c20-215">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="59c20-216">如需裝載同處理序時，會啟用宣告轉換的程式碼範例和詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="59c20-216">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59c20-217">其他資源</span><span class="sxs-lookup"><span data-stu-id="59c20-217">Additional resources</span></span>

* [<span data-ttu-id="59c20-218">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="59c20-218">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
