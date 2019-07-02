---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 了解如何設定 ASP.NET Core 中的 Windows 驗證的 IIS 和 HTTP.sys。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/01/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 30f1f554a29412ed6b84115d457d2da1aba91c17
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500508"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="d08fc-103">在 ASP.NET Core 中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="d08fc-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="d08fc-104">作者：[Scott Addie](https://twitter.com/Scott_Addie) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d08fc-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d08fc-105">Windows 驗證 （也稱為交涉、 Kerberos 或 NTLM 驗證） 可以設定與裝載的 ASP.NET Core 應用程式[IIS](xref:host-and-deploy/iis/index)， [Kestrel](xref:fundamentals/servers/kestrel)，或[HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="d08fc-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d08fc-106">Windows 驗證 （也稱為交涉、 Kerberos 或 NTLM 驗證） 可以設定與裝載的 ASP.NET Core 應用程式[IIS](xref:host-and-deploy/iis/index)或是[HTTP.sys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="d08fc-107">Windows 驗證仰賴作業系統來驗證 ASP.NET Core 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="d08fc-108">當您的伺服器在公司網路上執行時，您可以透過 Active Directory 網域身分識別進行 Windows 驗證或透過 Windows 帳戶來識別使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="d08fc-109">Windows 驗證最適合用於使用者、用戶端應用程式與 Web 伺服器皆屬於相同 Windows 網域的內部網路環境。</span><span class="sxs-lookup"><span data-stu-id="d08fc-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-110">使用 HTTP/2，不支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="d08fc-111">可以傳送 HTTP/2 回應的驗證挑戰，但用戶端驗證之前，必須降級為 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="d08fc-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="d08fc-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="d08fc-112">IIS/IIS Express</span></span>

<span data-ttu-id="d08fc-113">加入驗證服務所叫用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName>命名空間) 中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d08fc-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="d08fc-114">啟動設定 （偵錯工具）</span><span class="sxs-lookup"><span data-stu-id="d08fc-114">Launch settings (debugger)</span></span>

<span data-ttu-id="d08fc-115">啟動設定的設定只會影響*Properties/launchSettings.json*適用於 IIS Express 檔案，並不會設定 IIS 的 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="d08fc-116">伺服器組態中會說明[IIS](#iis)一節。</span><span class="sxs-lookup"><span data-stu-id="d08fc-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="d08fc-117">**Web 應用程式**可透過 Visual Studio 或.NET Core CLI 的範本可以設定為支援 Windows 驗證，這會更新*Properties/launchSettings.json*檔案自動的。</span><span class="sxs-lookup"><span data-stu-id="d08fc-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d08fc-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d08fc-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d08fc-119">**新的專案**</span><span class="sxs-lookup"><span data-stu-id="d08fc-119">**New project**</span></span>

1. <span data-ttu-id="d08fc-120">建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-120">Create a new project.</span></span>
1. <span data-ttu-id="d08fc-121">選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d08fc-122">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-122">Select **Next**.</span></span>
1. <span data-ttu-id="d08fc-123">提供的名稱**專案名稱**欄位。</span><span class="sxs-lookup"><span data-stu-id="d08fc-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="d08fc-124">確認**位置**項目是否正確，或提供專案的位置。</span><span class="sxs-lookup"><span data-stu-id="d08fc-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d08fc-125">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-125">Select **Create**.</span></span>
1. <span data-ttu-id="d08fc-126">選取 **變更**下方**驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="d08fc-127">在 **變更驗證**視窗中，選取**Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="d08fc-128">選取 [確定]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-128">Select **OK**.</span></span>
1. <span data-ttu-id="d08fc-129">選取 [Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="d08fc-130">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-130">Select **Create**.</span></span>

<span data-ttu-id="d08fc-131">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-131">Run the app.</span></span> <span data-ttu-id="d08fc-132">使用者名稱會出現在呈現的應用程式使用者介面。</span><span class="sxs-lookup"><span data-stu-id="d08fc-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="d08fc-133">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="d08fc-133">**Existing project**</span></span>

<span data-ttu-id="d08fc-134">專案的屬性會啟用 Windows 驗證，並停用匿名驗證：</span><span class="sxs-lookup"><span data-stu-id="d08fc-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="d08fc-135">以滑鼠右鍵按一下 [方案總管]  中的專案，然後選取 [屬性]  。</span><span class="sxs-lookup"><span data-stu-id="d08fc-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="d08fc-136">選取 [偵錯]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d08fc-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="d08fc-137">清除核取方塊**啟用匿名驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="d08fc-138">選取核取方塊**啟用 Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="d08fc-139">儲存並關閉 [屬性] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d08fc-139">Save and close the property page.</span></span>

<span data-ttu-id="d08fc-140">或者，設定屬性，在`iisSettings`的節點*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="d08fc-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="d08fc-141">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d08fc-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="d08fc-142">**新的專案**</span><span class="sxs-lookup"><span data-stu-id="d08fc-142">**New project**</span></span>

<span data-ttu-id="d08fc-143">執行[dotnet 新](/dotnet/core/tools/dotnet-new)命令搭配`webapp`引數 （ASP.NET Core Web 應用程式） 和`--auth Windows`切換：</span><span class="sxs-lookup"><span data-stu-id="d08fc-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="d08fc-144">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="d08fc-144">**Existing project**</span></span>

<span data-ttu-id="d08fc-145">更新`iisSettings`的節點*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="d08fc-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="d08fc-146">當修改現有的專案，請確認專案檔包含的套件參考[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)**或是** [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="d08fc-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="d08fc-147">IIS</span><span class="sxs-lookup"><span data-stu-id="d08fc-147">IIS</span></span>

<span data-ttu-id="d08fc-148">IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主機 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="d08fc-149">Windows 驗證針對透過 IIS *web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="d08fc-150">下列各節將示範如何：</span><span class="sxs-lookup"><span data-stu-id="d08fc-150">The following sections show how to:</span></span>

* <span data-ttu-id="d08fc-151">提供本機*web.config*部署應用程式時，請在伺服器啟動 Windows 驗證的檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="d08fc-152">使用 IIS 管理員設定*web.config*已經部署到伺服器的 ASP.NET Core 應用程式的檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="d08fc-153">如果您尚未這麼做，請啟用 IIS 可裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="d08fc-154">如需詳細資訊，請參閱<xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="d08fc-155">啟用 Windows 驗證的 IIS 角色服務。</span><span class="sxs-lookup"><span data-stu-id="d08fc-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="d08fc-156">如需詳細資訊，請參閱 <<c0> [ 啟用 IIS 角色服務 （請參閱步驟 2） 中的 Windows 驗證](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="d08fc-157">[IIS Integration 中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)預設設定來自動驗證要求。</span><span class="sxs-lookup"><span data-stu-id="d08fc-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="d08fc-158">如需詳細資訊，請參閱[裝載 ASP.NET Core 與 IIS 的 Windows 上：IIS 選項 (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="d08fc-159">ASP.NET Core 模組預設設定為轉送至應用程式的 Windows 驗證語彙基元。</span><span class="sxs-lookup"><span data-stu-id="d08fc-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="d08fc-160">如需詳細資訊，請參閱[ASP.NET Core 模組組態參考：AspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="d08fc-161">使用**任一**下列其中一個方法：</span><span class="sxs-lookup"><span data-stu-id="d08fc-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="d08fc-162">**發行和部署專案，再**新增下列*web.config*至專案根目錄的檔案：</span><span class="sxs-lookup"><span data-stu-id="d08fc-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="d08fc-163">當專案發行.NET Core SDK (不含`<IsTransformWebConfigDisabled>`屬性設定為`true`專案檔中)，發行*web.config*檔案包含`<location><system.webServer><security><authentication>`一節。</span><span class="sxs-lookup"><span data-stu-id="d08fc-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="d08fc-164">如需詳細資訊`<IsTransformWebConfigDisabled>`屬性，請參閱<xref:host-and-deploy/iis/index#webconfig-file>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="d08fc-165">**發行和部署專案之後,** 執行伺服器端設定使用 IIS 管理員：</span><span class="sxs-lookup"><span data-stu-id="d08fc-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="d08fc-166">在 [IIS 管理員] 中，選取 IIS 站台之下**站台**節點**連線**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="d08fc-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="d08fc-167">按兩下**驗證**中**IIS**區域。</span><span class="sxs-lookup"><span data-stu-id="d08fc-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="d08fc-168">選取 **匿名驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="d08fc-169">選取 **停用**中**動作**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="d08fc-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="d08fc-170">選取  **Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="d08fc-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="d08fc-171">選取 **啟用**中**動作**資訊看板。</span><span class="sxs-lookup"><span data-stu-id="d08fc-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="d08fc-172">IIS 管理員在採取這些動作，會修改應用程式的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="d08fc-173">A`<system.webServer><security><authentication>`節點新增與更新的設定，如`anonymousAuthentication`和`windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="d08fc-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="d08fc-174">`<system.webServer>`區段新增至*web.config*由 IIS 管理員中的檔案超出應用程式的`<location>`發佈應用程式時，由.NET Core SDK 加入的區段。</span><span class="sxs-lookup"><span data-stu-id="d08fc-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="d08fc-175">因為區段會新增外部`<location>`節點，設定會由任何繼承[子應用程式](xref:host-and-deploy/iis/index#sub-applications)目前的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="d08fc-176">若要防止繼承，移動加入`<security>`區段內的`<location><system.webServer>`.NET Core SDK 提供的一節。</span><span class="sxs-lookup"><span data-stu-id="d08fc-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="d08fc-177">將 IIS 設定使用 IIS 管理員時，它只會影響應用程式的*web.config*伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="d08fc-178">後續部署應用程式可能會覆寫伺服器上的設定，如果伺服器的複本*web.config*專案的取代*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="d08fc-179">使用**任一**下列其中一個方法來管理設定：</span><span class="sxs-lookup"><span data-stu-id="d08fc-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="d08fc-180">使用 IIS 管理員中的設定重設*web.config*檔案之後部署上覆寫該檔案。</span><span class="sxs-lookup"><span data-stu-id="d08fc-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="d08fc-181">新增*web.config 檔案*應用程式在本機使用的設定。</span><span class="sxs-lookup"><span data-stu-id="d08fc-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="d08fc-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d08fc-182">Kestrel</span></span>

 <span data-ttu-id="d08fc-183">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet 套件可以搭配[Kestrel](xref:fundamentals/servers/kestrel)以支援在 Windows、 Linux 和 macOS 上使用 Negotiate、 Kerberos 和 NTLM Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="d08fc-184">認證可以保存在連接上的要求。</span><span class="sxs-lookup"><span data-stu-id="d08fc-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="d08fc-185">*交涉驗證必須不使用 proxy 使用，除非 proxy 會使用 Kestrel 的 1 對 1 連接同質 （持續連線）。*</span><span class="sxs-lookup"><span data-stu-id="d08fc-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-186">如果基礎伺服器以原生方式支援 Windows 驗證，而且如果已啟用，會偵測到的交涉處理常式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-186">The Negotiate handler detects if the underlying server supports Windows Authentication natively and if it's enabled.</span></span> <span data-ttu-id="d08fc-187">如果伺服器支援 Windows 驗證，但已停用，詢問是否要啟用伺服器實作擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="d08fc-187">If the server supports Windows Authentication but it's disabled, an error is thrown asking you to enable the server implementation.</span></span> <span data-ttu-id="d08fc-188">在伺服器中啟用 Windows 驗證時，Negotiate 處理常式無障礙地轉送給它。</span><span class="sxs-lookup"><span data-stu-id="d08fc-188">When Windows Authentication is enabled in the server, the Negotiate handler transparently forwards to it.</span></span>

 <span data-ttu-id="d08fc-189">加入驗證服務所叫用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(`Microsoft.AspNetCore.Authentication.Negotiate`命名空間) 和`AddNegotitate`(`Microsoft.AspNetCore.Authentication.Negotiate`命名空間) 中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d08fc-189">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="d08fc-190">新增驗證中介軟體，藉由呼叫<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>在`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d08fc-190">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="d08fc-191">如需有關中介軟體的詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-191">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="d08fc-192">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="d08fc-192">Anonymous requests are allowed.</span></span> <span data-ttu-id="d08fc-193">使用[ASP.NET Core 授權](xref:security/authorization/introduction)挑戰驗證的匿名要求。</span><span class="sxs-lookup"><span data-stu-id="d08fc-193">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="d08fc-194">Windows 環境設定</span><span class="sxs-lookup"><span data-stu-id="d08fc-194">Windows environment configuration</span></span>

<span data-ttu-id="d08fc-195">[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)元件會執行使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-195">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="d08fc-196">服務主體名稱 (Spn) 必須將執行服務，而不是電腦帳戶的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="d08fc-196">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="d08fc-197">執行`setspn -S HTTP/mysrevername.mydomain.com myuser`在系統管理命令介面中。</span><span class="sxs-lookup"><span data-stu-id="d08fc-197">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="d08fc-198">Linux 和 macOS 的環境設定</span><span class="sxs-lookup"><span data-stu-id="d08fc-198">Linux and macOS environment configuration</span></span>

<span data-ttu-id="d08fc-199">加入 Windows 網域中的 Linux 或 macOS 機器的指示位於[到 SQL Server 使用 Windows 驗證 Kerberos 連接 Azure Data Studio](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文章。</span><span class="sxs-lookup"><span data-stu-id="d08fc-199">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="d08fc-200">指示在網域上建立的 Linux 機器的機器帳戶。</span><span class="sxs-lookup"><span data-stu-id="d08fc-200">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="d08fc-201">Spn 必須新增至該電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="d08fc-201">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-202">遵循中的指導方針時[到 SQL Server 使用 Windows 驗證 Kerberos 連接 Azure Data Studio](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文章中，取代`python-software-properties`使用`python3-software-properties`如有需要。</span><span class="sxs-lookup"><span data-stu-id="d08fc-202">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="d08fc-203">一旦在 Linux 或 macOS 機器已加入網域，需要額外的步驟，以提供[keytab 檔案](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)利用 Spn:</span><span class="sxs-lookup"><span data-stu-id="d08fc-203">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="d08fc-204">在網域控制站，用電腦帳戶來加入新的 web 服務的 Spn:</span><span class="sxs-lookup"><span data-stu-id="d08fc-204">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="d08fc-205">使用[ktpass](/windows-server/administration/windows-commands/ktpass)產生 keytab 檔案：</span><span class="sxs-lookup"><span data-stu-id="d08fc-205">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="d08fc-206">某些欄位中必須指定大寫所示。</span><span class="sxs-lookup"><span data-stu-id="d08fc-206">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="d08fc-207">將 keytab 檔案複製到 Linux 或 macOS 電腦。</span><span class="sxs-lookup"><span data-stu-id="d08fc-207">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="d08fc-208">選取透過環境變數的 keytab 檔案： `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="d08fc-208">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="d08fc-209">叫用`klist`以顯示目前可供使用的 Spn。</span><span class="sxs-lookup"><span data-stu-id="d08fc-209">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-210">Keytab 檔案包含網域存取認證，而且必須據此加以保護。</span><span class="sxs-lookup"><span data-stu-id="d08fc-210">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="d08fc-211">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d08fc-211">HTTP.sys</span></span>

<span data-ttu-id="d08fc-212">[HTTP.sys](xref:fundamentals/servers/httpsys)支援使用 Negotiate、 NTLM、 或基本驗證的核心模式 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-212">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="d08fc-213">加入驗證服務所叫用<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>(<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>命名空間) 中`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d08fc-213">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="d08fc-214">使用 Windows 驗證使用 HTTP.sys 的應用程式的 web 主機設定 (*Program.cs*)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-214">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="d08fc-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> 處於<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName>命名空間。</span><span class="sxs-lookup"><span data-stu-id="d08fc-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d08fc-216">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-216">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="d08fc-217">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d08fc-217">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="d08fc-218">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-218">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="d08fc-219">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-219">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-220">HTTP.sys 不支援 Nano Server 1709 版或更新版本上。</span><span class="sxs-lookup"><span data-stu-id="d08fc-220">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="d08fc-221">若要使用 Windows 驗證和 HTTP.sys 使用 Nano Server，請使用[Server Core (microsoft/windowsservercore) 容器](https://hub.docker.com/r/microsoft/windowsservercore/)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-221">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="d08fc-222">如需有關 Server Core 的詳細資訊，請參閱[什麼是 Windows Server 中的 Server Core 安裝選項？](/windows-server/administration/server-core/what-is-server-core)。</span><span class="sxs-lookup"><span data-stu-id="d08fc-222">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="d08fc-223">授權使用者</span><span class="sxs-lookup"><span data-stu-id="d08fc-223">Authorize users</span></span>

<span data-ttu-id="d08fc-224">匿名存取的設定狀態決定的方式`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。</span><span class="sxs-lookup"><span data-stu-id="d08fc-224">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="d08fc-225">下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。</span><span class="sxs-lookup"><span data-stu-id="d08fc-225">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="d08fc-226">不允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="d08fc-226">Disallow anonymous access</span></span>

<span data-ttu-id="d08fc-227">當您啟用 Windows 驗證，並已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`屬性沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="d08fc-227">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="d08fc-228">如果 IIS 站台設定為不允許匿名存取，要求永遠不會到達應用程式。</span><span class="sxs-lookup"><span data-stu-id="d08fc-228">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="d08fc-229">基於這個理由，`[AllowAnonymous]`屬性不適用。</span><span class="sxs-lookup"><span data-stu-id="d08fc-229">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="d08fc-230">允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="d08fc-230">Allow anonymous access</span></span>

<span data-ttu-id="d08fc-231">當啟用 Windows 驗證和匿名存取時，使用`[Authorize]`和`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="d08fc-231">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="d08fc-232">`[Authorize]`屬性可讓您保護應用程式需要驗證的端點。</span><span class="sxs-lookup"><span data-stu-id="d08fc-232">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="d08fc-233">`[AllowAnonymous]`屬性覆寫`[Authorize]`允許匿名存取的應用程式中的屬性。</span><span class="sxs-lookup"><span data-stu-id="d08fc-233">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="d08fc-234">屬性使用方式詳細資料，請參閱<xref:security/authorization/simple>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-234">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="d08fc-235">根據預設，缺少授權，才能存取頁面的使用者會看到空的 HTTP 403 回應。</span><span class="sxs-lookup"><span data-stu-id="d08fc-235">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="d08fc-236">[StatusCodePages 中介軟體](xref:fundamentals/error-handling#usestatuscodepages)可以設定為使用者提供更好的 「 拒絕存取 」 體驗。</span><span class="sxs-lookup"><span data-stu-id="d08fc-236">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="d08fc-237">模擬</span><span class="sxs-lookup"><span data-stu-id="d08fc-237">Impersonation</span></span>

<span data-ttu-id="d08fc-238">ASP.NET Core 不會實作模擬。</span><span class="sxs-lookup"><span data-stu-id="d08fc-238">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="d08fc-239">應用程式執行的所有要求，使用應用程式集區或處理序身分識別的應用程式的身分識別。</span><span class="sxs-lookup"><span data-stu-id="d08fc-239">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="d08fc-240">如果應用程式應該執行代表使用者的動作，使用[WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*)中[終端機內嵌中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)在`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="d08fc-240">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="d08fc-241">在此內容中執行單一動作，然後關閉 內容。</span><span class="sxs-lookup"><span data-stu-id="d08fc-241">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="d08fc-242">`RunImpersonated` 不支援非同步作業，而不應該用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="d08fc-242">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="d08fc-243">比方說，包裝整個要求或中介軟體鏈結不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="d08fc-243">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d08fc-244">雖然[Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)套件會啟用 Windows 驗證，Windows 才支援 Linux 和 macOS 的模擬。</span><span class="sxs-lookup"><span data-stu-id="d08fc-244">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="d08fc-245">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="d08fc-245">Claims transformations</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d08fc-246">當使用 IIS 時，裝載<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>不在內部呼叫以初始化使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-246">When hosting with IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="d08fc-247">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="d08fc-247">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="d08fc-248">如需宣告轉換就會啟動的程式碼範例和詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-248">For more information and a code example that activates claims transformations, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d08fc-249">當 IIS 同處理序模式中，裝載<xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*>不在內部呼叫以初始化使用者。</span><span class="sxs-lookup"><span data-stu-id="d08fc-249">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="d08fc-250">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="d08fc-250">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="d08fc-251">如需裝載同處理序時，會啟用宣告轉換的程式碼範例和詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>。</span><span class="sxs-lookup"><span data-stu-id="d08fc-251">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d08fc-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="d08fc-252">Additional resources</span></span>

* [<span data-ttu-id="d08fc-253">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="d08fc-253">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
