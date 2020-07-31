---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 瞭解如何在 IIS 和 HTTP.sys 的 ASP.NET Core 中設定 Windows 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/26/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/windowsauth
ms.openlocfilehash: 8f6dc8620df04bcebe996119869ca2e498cffccc
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "87330676"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="d6414-103">在 ASP.NET Core 中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="d6414-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="d6414-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d6414-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d6414-105">Windows 驗證（也稱為 Negotiate、Kerberos 或 NTLM 驗證）可以針對以[IIS](xref:host-and-deploy/iis/index)、 [Kestrel](xref:fundamentals/servers/kestrel)或[HTTP.sys](xref:fundamentals/servers/httpsys)裝載的 ASP.NET Core 應用程式進行設定。</span><span class="sxs-lookup"><span data-stu-id="d6414-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d6414-106">Windows 驗證（也稱為 Negotiate、Kerberos 或 NTLM 驗證）可以針對使用[IIS](xref:host-and-deploy/iis/index)或[HTTP.sys](xref:fundamentals/servers/httpsys)的 ASP.NET Core 應用程式進行設定。</span><span class="sxs-lookup"><span data-stu-id="d6414-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="d6414-107">Windows 驗證依賴作業系統來驗證 ASP.NET Core 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="d6414-108">當您的伺服器使用 Active Directory 網域身分識別或 Windows 帳戶在公司網路上執行時，您可以使用 Windows 驗證來識別使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="d6414-109">Windows 驗證最適合內部網路環境，其中使用者、用戶端應用程式和網頁伺服器都屬於相同的 Windows 網域。</span><span class="sxs-lookup"><span data-stu-id="d6414-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-110">HTTP/2 不支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="d6414-111">驗證挑戰可以透過 HTTP/2 回應來傳送，但是用戶端必須先降級為 HTTP/1.1，才能進行驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="d6414-112">Proxy 和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="d6414-112">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="d6414-113">Windows 驗證是主要用於內部網路的具狀態案例，proxy 或負載平衡器通常不會處理用戶端與伺服器之間的流量。</span><span class="sxs-lookup"><span data-stu-id="d6414-113">Windows Authentication is a stateful scenario primarily used in an intranet, where a proxy or load balancer doesn't usually handle traffic between clients and servers.</span></span> <span data-ttu-id="d6414-114">如果使用 proxy 或負載平衡器，Windows 驗證僅適用于 proxy 或負載平衡器：</span><span class="sxs-lookup"><span data-stu-id="d6414-114">If a proxy or load balancer is used, Windows Authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="d6414-115">處理驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-115">Handles the authentication.</span></span>
* <span data-ttu-id="d6414-116">將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="d6414-116">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="d6414-117">在使用 proxy 和負載平衡器的環境中，Windows 驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。</span><span class="sxs-lookup"><span data-stu-id="d6414-117">An alternative to Windows Authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="d6414-118">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="d6414-118">IIS/IIS Express</span></span>

<span data-ttu-id="d6414-119">藉由 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> 在中叫用（命名空間）來新增驗證服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-119">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="d6414-120">啟動設定（偵錯工具）</span><span class="sxs-lookup"><span data-stu-id="d6414-120">Launch settings (debugger)</span></span>

<span data-ttu-id="d6414-121">啟動設定的設定只會影響 IIS Express 的檔案*屬性/launchSettings.js* ，而且不會針對 Windows 驗證設定 IIS。</span><span class="sxs-lookup"><span data-stu-id="d6414-121">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="d6414-122">伺服器設定會在[IIS](#iis)一節中說明。</span><span class="sxs-lookup"><span data-stu-id="d6414-122">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="d6414-123">透過 Visual Studio 或 .NET Core CLI 提供的**Web 應用程式**範本可以設定為支援 Windows 驗證，這會自動更新檔案的*屬性/launchSettings.js* 。</span><span class="sxs-lookup"><span data-stu-id="d6414-123">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d6414-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d6414-124">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d6414-125">**新增專案**</span><span class="sxs-lookup"><span data-stu-id="d6414-125">**New project**</span></span>

1. <span data-ttu-id="d6414-126">建立新專案。</span><span class="sxs-lookup"><span data-stu-id="d6414-126">Create a new project.</span></span>
1. <span data-ttu-id="d6414-127">選取 **ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="d6414-127">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d6414-128">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="d6414-128">Select **Next**.</span></span>
1. <span data-ttu-id="d6414-129">在 [**專案名稱**] 欄位中提供名稱。</span><span class="sxs-lookup"><span data-stu-id="d6414-129">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="d6414-130">確認 [**位置**] 專案正確，或提供專案的 [位置]。</span><span class="sxs-lookup"><span data-stu-id="d6414-130">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="d6414-131">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="d6414-131">Select **Create**.</span></span>
1. <span data-ttu-id="d6414-132">選取 [**驗證**] 底下的 [**變更**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-132">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="d6414-133">在 [**變更驗證**] 視窗中，選取 [ **Windows 驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-133">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="d6414-134">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d6414-134">Select **OK**.</span></span>
1. <span data-ttu-id="d6414-135">選取 [ **Web 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-135">Select **Web Application**.</span></span>
1. <span data-ttu-id="d6414-136">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="d6414-136">Select **Create**.</span></span>

<span data-ttu-id="d6414-137">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-137">Run the app.</span></span> <span data-ttu-id="d6414-138">使用者名稱會出現在轉譯的應用程式的使用者介面中。</span><span class="sxs-lookup"><span data-stu-id="d6414-138">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="d6414-139">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="d6414-139">**Existing project**</span></span>

<span data-ttu-id="d6414-140">專案的屬性會啟用 Windows 驗證並停用匿名驗證：</span><span class="sxs-lookup"><span data-stu-id="d6414-140">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="d6414-141">以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [**屬性**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-141">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="d6414-142">選取 [偵錯]\*\*\*\* 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d6414-142">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="d6414-143">清除 [**啟用匿名驗證**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d6414-143">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="d6414-144">選取 [**啟用 Windows 驗證**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d6414-144">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="d6414-145">儲存並關閉屬性頁。</span><span class="sxs-lookup"><span data-stu-id="d6414-145">Save and close the property page.</span></span>

<span data-ttu-id="d6414-146">或者，您也可以在檔案 `iisSettings` *上launchSettings.js*的節點中設定屬性：</span><span class="sxs-lookup"><span data-stu-id="d6414-146">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-cli"></a>[<span data-ttu-id="d6414-147">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d6414-147">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d6414-148">**新增專案**</span><span class="sxs-lookup"><span data-stu-id="d6414-148">**New project**</span></span>

<span data-ttu-id="d6414-149">使用[dotnet new](/dotnet/core/tools/dotnet-new) `webapp` 引數（ASP.NET Core Web 應用程式）和交換器來執行 dotnet new 命令 `--auth Windows` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-149">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```dotnetcli
dotnet new webapp --auth Windows
```

<span data-ttu-id="d6414-150">**現有專案**</span><span class="sxs-lookup"><span data-stu-id="d6414-150">**Existing project**</span></span>

<span data-ttu-id="d6414-151">更新檔案 `iisSettings` *上launchSettings.js*的節點：</span><span class="sxs-lookup"><span data-stu-id="d6414-151">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="d6414-152">修改現有的專案時，請確認專案檔包含[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)**或** [AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="d6414-152">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="d6414-153">IIS</span><span class="sxs-lookup"><span data-stu-id="d6414-153">IIS</span></span>

<span data-ttu-id="d6414-154">IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-154">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="d6414-155">Windows 驗證是透過*web.config*檔案來設定 IIS。</span><span class="sxs-lookup"><span data-stu-id="d6414-155">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="d6414-156">下列各節將示範如何：</span><span class="sxs-lookup"><span data-stu-id="d6414-156">The following sections show how to:</span></span>

* <span data-ttu-id="d6414-157">提供在部署應用程式時，在伺服器上啟動 Windows 驗證的本機*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d6414-157">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="d6414-158">使用 IIS 管理員來設定已部署至伺服器之 ASP.NET Core 應用程式的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d6414-158">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="d6414-159">如果您尚未這麼做，請啟用 IIS 來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-159">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="d6414-160">如需詳細資訊，請參閱 <xref:host-and-deploy/iis/index>。</span><span class="sxs-lookup"><span data-stu-id="d6414-160">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="d6414-161">啟用 IIS 角色服務以進行 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-161">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="d6414-162">如需詳細資訊，請參閱[在 IIS 角色服務中啟用 Windows 驗證（請參閱步驟2）](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="d6414-162">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="d6414-163">[IIS 整合中介軟體](xref:host-and-deploy/iis/index#enable-the-iisintegration-components)會設定為預設自動驗證要求。</span><span class="sxs-lookup"><span data-stu-id="d6414-163">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="d6414-164">如需詳細資訊，請參閱[在 Windows 上使用 iis 的主機 ASP.NET Core： iis 選項（AutomaticAuthentication）](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="d6414-164">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="d6414-165">預設會將 ASP.NET Core 模組設定為將 Windows 驗證權杖轉送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-165">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="d6414-166">如需詳細資訊，請參閱[ASP.NET Core 模組設定參考： aspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="d6414-166">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="d6414-167">請使用下列**其中一**種方法：</span><span class="sxs-lookup"><span data-stu-id="d6414-167">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="d6414-168">**發行和部署專案之前，請**將下列*web.config*檔案加入至專案根目錄：</span><span class="sxs-lookup"><span data-stu-id="d6414-168">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="d6414-169">當 .NET Core SDK 發行專案時（ `<IsTransformWebConfigDisabled>` 在專案檔中未將屬性設定為 `true` ），已發行的*web.config*檔案會包含 `<location><system.webServer><security><authentication>` 區段。</span><span class="sxs-lookup"><span data-stu-id="d6414-169">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="d6414-170">如需屬性的詳細資訊 `<IsTransformWebConfigDisabled>` ，請參閱 <xref:host-and-deploy/iis/index#webconfig-file> 。</span><span class="sxs-lookup"><span data-stu-id="d6414-170">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="d6414-171">**發行和部署專案之後，請**使用 IIS 管理員來執行伺服器端設定：</span><span class="sxs-lookup"><span data-stu-id="d6414-171">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="d6414-172">在 [IIS 管理員] 中，**選取 [連線] 提要欄位**的 [**網站**] 節點底下的 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="d6414-172">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="d6414-173">按兩下 [ **IIS** ] 區域中的 [**驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-173">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="d6414-174">選取 [**匿名驗證**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-174">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="d6414-175">在 [**動作**] 提要欄位中選取 [**停**用]。</span><span class="sxs-lookup"><span data-stu-id="d6414-175">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="d6414-176">選取 **[Windows 驗證]**。</span><span class="sxs-lookup"><span data-stu-id="d6414-176">Select **Windows Authentication**.</span></span> <span data-ttu-id="d6414-177">選取 [**動作**] 提要欄位中的 [**啟用**]。</span><span class="sxs-lookup"><span data-stu-id="d6414-177">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="d6414-178">當採取這些動作時，IIS 管理員會修改應用程式的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d6414-178">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="d6414-179">`<system.webServer><security><authentication>`加入的節點具有和的更新設定 `anonymousAuthentication` `windowsAuthentication` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-179">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="d6414-180">`<system.webServer>`IIS 管理員新增至*web.config*檔案的區段，是在 `<location>` 發佈應用程式時，由 .NET Core SDK 新增的應用程式區段外。</span><span class="sxs-lookup"><span data-stu-id="d6414-180">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="d6414-181">因為區段會新增 `<location>` 至節點外部，所以所有[子應用](xref:host-and-deploy/iis/index#sub-applications)程式都會將這些設定繼承至目前的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-181">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="d6414-182">若要防止繼承，請將新增的區段移至 `<security>` `<location><system.webServer>` .NET Core SDK 提供的區段內。</span><span class="sxs-lookup"><span data-stu-id="d6414-182">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="d6414-183">當使用 IIS 管理員來新增 IIS 設定時，它只會影響應用程式在伺服器上的*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="d6414-183">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="d6414-184">如果伺服器的*web.config*複本已由專案的*web.config*檔取代，應用程式的後續部署可能會覆寫伺服器上的設定。</span><span class="sxs-lookup"><span data-stu-id="d6414-184">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="d6414-185">使用下列**其中一**種方法來管理設定：</span><span class="sxs-lookup"><span data-stu-id="d6414-185">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="d6414-186">在部署之後覆寫檔案之後，請使用 IIS 管理員來重設*web.config*檔案中的設定。</span><span class="sxs-lookup"><span data-stu-id="d6414-186">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="d6414-187">使用設定在本機將*web.config*檔案新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-187">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="d6414-188">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d6414-188">Kestrel</span></span>

<span data-ttu-id="d6414-189">[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)可以與[Kestrel](xref:fundamentals/servers/kestrel)搭配使用，以支援在 Windows、Linux 和 macOS 上使用 Negotiate 和 Kerberos 來進行 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-189">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate and Kerberos on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="d6414-190">認證可以跨連接的要求保存。</span><span class="sxs-lookup"><span data-stu-id="d6414-190">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="d6414-191">*除非 proxy 使用 Kestrel 維護1:1 連線親和性（持續連線），否則不能將 Negotiate 驗證與 proxy 搭配使用。*</span><span class="sxs-lookup"><span data-stu-id="d6414-191">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-192">Negotiate 處理常式會偵測基礎伺服器是否以原生方式支援 Windows 驗證，以及是否已啟用。</span><span class="sxs-lookup"><span data-stu-id="d6414-192">The Negotiate handler detects if the underlying server supports Windows Authentication natively and if it's enabled.</span></span> <span data-ttu-id="d6414-193">如果伺服器支援 Windows 驗證但已停用，則會擲回錯誤，要求您啟用伺服器執行。</span><span class="sxs-lookup"><span data-stu-id="d6414-193">If the server supports Windows Authentication but it's disabled, an error is thrown asking you to enable the server implementation.</span></span> <span data-ttu-id="d6414-194">在伺服器中啟用 Windows 驗證時，Negotiate 處理常式會以透明方式轉寄給它。</span><span class="sxs-lookup"><span data-stu-id="d6414-194">When Windows Authentication is enabled in the server, the Negotiate handler transparently forwards to it.</span></span>

<span data-ttu-id="d6414-195">藉由在中叫用和來新增驗證服務 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-195">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.NegotiateExtensions.AddNegotiate*> in `Startup.ConfigureServices`:</span></span>

 ```csharp
// using Microsoft.AspNetCore.Authentication.Negotiate;
// using Microsoft.Extensions.DependencyInjection;

services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="d6414-196">藉由呼叫中的來新增驗證中介軟體 <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-196">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();
```

<span data-ttu-id="d6414-197">如需中介軟體的詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。</span><span class="sxs-lookup"><span data-stu-id="d6414-197">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="d6414-198">允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="d6414-198">Anonymous requests are allowed.</span></span> <span data-ttu-id="d6414-199">使用[ASP.NET Core 授權](xref:security/authorization/introduction)來挑戰匿名要求以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-199">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="d6414-200">Windows 環境設定</span><span class="sxs-lookup"><span data-stu-id="d6414-200">Windows environment configuration</span></span>

<span data-ttu-id="d6414-201">[AspNetCore. Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)元件會執行使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-201">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="d6414-202">必須將服務主體名稱（Spn）新增至執行服務的使用者帳戶，而不是電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="d6414-202">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="d6414-203">`setspn -S HTTP/myservername.mydomain.com myuser`在系統管理命令 shell 中執行。</span><span class="sxs-lookup"><span data-stu-id="d6414-203">Execute `setspn -S HTTP/myservername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="d6414-204">Linux 和 macOS 環境設定</span><span class="sxs-lookup"><span data-stu-id="d6414-204">Linux and macOS environment configuration</span></span>

<span data-ttu-id="d6414-205">將 Linux 或 macOS 機器加入 Windows 網域的指示，請參閱[使用 Windows 驗證將 Azure Data Studio 連接到您的 SQL Server-Kerberos 一](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文。</span><span class="sxs-lookup"><span data-stu-id="d6414-205">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="d6414-206">指示會在網域上建立 Linux 機器的電腦帳戶。</span><span class="sxs-lookup"><span data-stu-id="d6414-206">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="d6414-207">Spn 必須新增至該機器帳戶。</span><span class="sxs-lookup"><span data-stu-id="d6414-207">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-208">遵循[使用 Windows 驗證將 Azure Data Studio 連接到您的 SQL Server-Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller)文章中的指導方針時，如有需要，請將取代 `python-software-properties` 為 `python3-software-properties` 。</span><span class="sxs-lookup"><span data-stu-id="d6414-208">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="d6414-209">一旦 Linux 或 macOS 機器加入網域之後，需要額外的步驟，才能提供具有 Spn 的[keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/)檔案：</span><span class="sxs-lookup"><span data-stu-id="d6414-209">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="d6414-210">在網域控制站上，將新的 web 服務 Spn 新增至電腦帳戶：</span><span class="sxs-lookup"><span data-stu-id="d6414-210">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="d6414-211">使用[ktpass](/windows-server/administration/windows-commands/ktpass)來產生 keytab 檔案：</span><span class="sxs-lookup"><span data-stu-id="d6414-211">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="d6414-212">某些欄位必須以大寫指定，如所示。</span><span class="sxs-lookup"><span data-stu-id="d6414-212">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="d6414-213">將 keytab 檔案複製到 Linux 或 macOS 機器。</span><span class="sxs-lookup"><span data-stu-id="d6414-213">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="d6414-214">透過環境變數選取 keytab 檔案：`export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="d6414-214">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="d6414-215">叫 `klist` 用以顯示目前可供使用的 spn。</span><span class="sxs-lookup"><span data-stu-id="d6414-215">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-216">Keytab 檔案包含網域存取認證，必須據以進行保護。</span><span class="sxs-lookup"><span data-stu-id="d6414-216">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="d6414-217">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d6414-217">HTTP.sys</span></span>

<span data-ttu-id="d6414-218">[HTTP.sys](xref:fundamentals/servers/httpsys)支援使用 NEGOTIATE、NTLM 或基本驗證的核心模式 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-218">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="d6414-219">藉由 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> 在中叫用（命名空間）來新增驗證服務 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d6414-219">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="d6414-220">將應用程式的 web 主機設定為使用 HTTP.sys 搭配 Windows 驗證（*Program.cs*）。</span><span class="sxs-lookup"><span data-stu-id="d6414-220">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="d6414-221"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*>位於 <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> 命名空間中。</span><span class="sxs-lookup"><span data-stu-id="d6414-221"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d6414-222">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-222">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="d6414-223">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="d6414-223">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="d6414-224">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-224">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="d6414-225">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-225">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-226">Nano Server 1709 版或更新版本不支援 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="d6414-226">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="d6414-227">若要搭配 Nano Server 使用 Windows 驗證和 HTTP.sys，請使用[Server Core （microsoft/windowsservercore）容器](https://hub.docker.com/r/microsoft/windowsservercore/)。</span><span class="sxs-lookup"><span data-stu-id="d6414-227">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="d6414-228">如需 Server Core 的詳細資訊，請參閱[Windows server 中的 Server core 安裝選項是什麼？](/windows-server/administration/server-core/what-is-server-core)。</span><span class="sxs-lookup"><span data-stu-id="d6414-228">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="d6414-229">授權使用者</span><span class="sxs-lookup"><span data-stu-id="d6414-229">Authorize users</span></span>

<span data-ttu-id="d6414-230">[匿名存取] 的設定狀態會決定在 `[Authorize]` `[AllowAnonymous]` 應用程式中使用和屬性的方式。</span><span class="sxs-lookup"><span data-stu-id="d6414-230">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="d6414-231">下列兩節說明如何處理匿名存取不允許和允許的設定狀態。</span><span class="sxs-lookup"><span data-stu-id="d6414-231">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="d6414-232">不允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="d6414-232">Disallow anonymous access</span></span>

<span data-ttu-id="d6414-233">啟用 Windows 驗證並停用匿名存取時， `[Authorize]` 和 `[AllowAnonymous]` 屬性不會有任何作用。</span><span class="sxs-lookup"><span data-stu-id="d6414-233">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="d6414-234">如果 IIS 網站設定為不允許匿名存取，則要求永遠不會到達應用程式。</span><span class="sxs-lookup"><span data-stu-id="d6414-234">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="d6414-235">基於這個理由，此 `[AllowAnonymous]` 屬性不適用。</span><span class="sxs-lookup"><span data-stu-id="d6414-235">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="d6414-236">允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="d6414-236">Allow anonymous access</span></span>

<span data-ttu-id="d6414-237">同時啟用 Windows 驗證和匿名存取時，請使用 `[Authorize]` 和 `[AllowAnonymous]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d6414-237">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="d6414-238">`[Authorize]`屬性可讓您保護需要驗證之應用程式的端點。</span><span class="sxs-lookup"><span data-stu-id="d6414-238">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="d6414-239">`[AllowAnonymous]`屬性會覆寫 `[Authorize]` 應用程式中允許匿名存取的屬性。</span><span class="sxs-lookup"><span data-stu-id="d6414-239">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="d6414-240">如需屬性使用方式的詳細資訊，請參閱 <xref:security/authorization/simple> 。</span><span class="sxs-lookup"><span data-stu-id="d6414-240">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="d6414-241">根據預設，缺少存取頁面授權的使用者會看到空白的 HTTP 403 回應。</span><span class="sxs-lookup"><span data-stu-id="d6414-241">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="d6414-242">您可以設定[StatusCodePages 中介軟體](xref:fundamentals/error-handling#usestatuscodepages)，為使用者提供更好的「拒絕存取」體驗。</span><span class="sxs-lookup"><span data-stu-id="d6414-242">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="d6414-243">模擬</span><span class="sxs-lookup"><span data-stu-id="d6414-243">Impersonation</span></span>

<span data-ttu-id="d6414-244">ASP.NET Core 不會執行模擬。</span><span class="sxs-lookup"><span data-stu-id="d6414-244">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="d6414-245">應用程式會使用應用程式集區或進程身分識別，針對所有要求以應用程式的身分識別執行。</span><span class="sxs-lookup"><span data-stu-id="d6414-245">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="d6414-246">如果應用程式應該代表使用者執行動作，請在的[終端機內嵌中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)中使用[WindowsIdentity. RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) 。 `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="d6414-246">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="d6414-247">在此內容中執行單一動作，然後關閉內容。</span><span class="sxs-lookup"><span data-stu-id="d6414-247">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="d6414-248">`RunImpersonated`不支援非同步作業，且不應用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="d6414-248">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="d6414-249">例如，不支援或不建議將整個要求或中介軟體鏈換行。</span><span class="sxs-lookup"><span data-stu-id="d6414-249">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d6414-250">雖然[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate)會在 Windows、Linux 和 macOS 上啟用驗證，但只有在 windows 上才支援模擬。</span><span class="sxs-lookup"><span data-stu-id="d6414-250">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="d6414-251">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="d6414-251">Claims transformations</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d6414-252">使用 IIS 裝載時， <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 不會在內部呼叫來初始化使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-252">When hosting with IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="d6414-253">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="d6414-253">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="d6414-254">如需啟用宣告轉換的詳細資訊和程式碼範例，請參閱 <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> 。</span><span class="sxs-lookup"><span data-stu-id="d6414-254">For more information and a code example that activates claims transformations, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d6414-255">使用 IIS 同進程模式裝載時， <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> 不會在內部呼叫來初始化使用者。</span><span class="sxs-lookup"><span data-stu-id="d6414-255">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="d6414-256">因此，預設會在未啟動每個驗證之後，使用 <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> 實作來轉換宣告。</span><span class="sxs-lookup"><span data-stu-id="d6414-256">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="d6414-257">如需在裝載同進程時啟動宣告轉換的詳細資訊和程式碼範例，請參閱 <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model> 。</span><span class="sxs-lookup"><span data-stu-id="d6414-257">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d6414-258">其他資源</span><span class="sxs-lookup"><span data-stu-id="d6414-258">Additional resources</span></span>

* [<span data-ttu-id="d6414-259">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="d6414-259">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
