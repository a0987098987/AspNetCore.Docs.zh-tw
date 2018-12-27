---
title: 在 ASP.NET Core 中設定 Windows 驗證
author: scottaddie
description: 了解如何在 ASP.NET Core，使用 IIS Express、 IIS 和 HTTP.sys 中設定 Windows 驗證。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 94dff2f47b2b076cb15f8d385239179b52786678
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637816"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="fd6a3-103">在 ASP.NET Core 中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="fd6a3-104">作者：[Steve Smith](https://ardalis.com) 和 [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="fd6a3-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="fd6a3-105">可以使用 IIS 裝載的 ASP.NET Core 應用程式設定 Windows 驗證或[HTTP.sys](xref:fundamentals/servers/httpsys)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="fd6a3-106">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-106">Windows Authentication</span></span>

<span data-ttu-id="fd6a3-107">Windows 驗證會仰賴作業系統來驗證的 ASP.NET Core 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="fd6a3-108">使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路中執行您的伺服器時，您可以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="fd6a3-109">Windows 驗證最適合內部網路環境中使用者、 用戶端應用程式，以及 web 伺服器屬於相同 Windows 網域。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="fd6a3-110">[深入了解 Windows 驗證並將它安裝為 IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="fd6a3-111">啟用 ASP.NET Core 應用程式中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="fd6a3-112">若要支援 Windows 驗證，可以設定 Visual Studio Web 應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="fd6a3-113">使用 Windows 驗證應用程式範本</span><span class="sxs-lookup"><span data-stu-id="fd6a3-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="fd6a3-114">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="fd6a3-114">In Visual Studio:</span></span>

1. <span data-ttu-id="fd6a3-115">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="fd6a3-116">從範本清單中選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="fd6a3-117">選取 **變更驗證**按鈕，然後選取**Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="fd6a3-118">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-118">Run the app.</span></span> <span data-ttu-id="fd6a3-119">使用者名稱會出現在右上方的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-119">The username appears in the top right of the app.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="fd6a3-121">針對使用 IIS Express 的開發工作，此範本會提供所有必要的組態，若要使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="fd6a3-122">下一節示範如何手動設定 Windows 驗證的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="fd6a3-123">Visual Studio 設定 Windows 和匿名驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="fd6a3-124">Visual Studio 專案**屬性**頁面的**偵錯** 索引標籤能提供 Windows 驗證和匿名驗證的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面反白顯示的驗證選項](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="fd6a3-126">或者，設定這兩個屬性，在*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="fd6a3-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="fd6a3-127">啟用 iis 的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="fd6a3-128">IIS 會使用[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)主機 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-128">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="fd6a3-129">在 IIS 中，應用程式設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="fd6a3-130">下列各節示範如何使用 IIS 管理員來設定 ASP.NET Core 應用程式以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="fd6a3-131">IIS 組態</span><span class="sxs-lookup"><span data-stu-id="fd6a3-131">IIS configuration</span></span>

<span data-ttu-id="fd6a3-132">啟用 Windows 驗證的 IIS 角色服務。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="fd6a3-133">如需詳細資訊，請參閱 <<c0> [ 啟用 IIS 角色服務 （請參閱步驟 2） 中的 Windows 驗證](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="fd6a3-134">根據預設，IIS Integration 中介軟體會設定來自動驗證要求。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="fd6a3-135">如需詳細資訊，請參閱[裝載 ASP.NET Core 與 IIS 的 Windows 上：IIS 選項 (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="fd6a3-136">ASP.NET Core 模組預設設定為轉送至應用程式的 Windows 驗證語彙基元。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="fd6a3-137">如需詳細資訊，請參閱[ASP.NET Core 模組組態參考：AspNetCore 元素的屬性](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="fd6a3-138">建立新的 IIS 站台</span><span class="sxs-lookup"><span data-stu-id="fd6a3-138">Create a new IIS site</span></span>

<span data-ttu-id="fd6a3-139">指定的名稱和資料夾，並允許它建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="fd6a3-140">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-140">Customize authentication</span></span>

<span data-ttu-id="fd6a3-141">開啟站台的驗證功能。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-141">Open the Authentication features for the site.</span></span>

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="fd6a3-143">停用匿名驗證，並啟用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="fd6a3-145">將專案發佈至 IIS 的站台資料夾</span><span class="sxs-lookup"><span data-stu-id="fd6a3-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="fd6a3-146">使用 Visual Studio 或.NET Core CLI，將應用程式發行至目的資料夾。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio 發佈對話方塊](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="fd6a3-148">深入了解[發行至 IIS](xref:host-and-deploy/iis/index)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="fd6a3-149">啟動應用程式以確認 Windows 驗證正常運作。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-149">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="fd6a3-150">啟用 Windows 驗證，http.sys</span><span class="sxs-lookup"><span data-stu-id="fd6a3-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="fd6a3-151">雖然 Kestrel 不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)支援在 Windows 上的自我裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="fd6a3-152">下列範例會設定要搭配 Windows 驗證使用 HTTP.sys 的應用程式的 web 主機：</span><span class="sxs-lookup"><span data-stu-id="fd6a3-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="fd6a3-153">HTTP.sys 使用 Kerberos 驗證通訊協定委派給核心模式驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="fd6a3-154">Kerberos 和 HTTP.sys 不支援使用者模式驗證。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="fd6a3-155">必須使用電腦帳戶來解密 Kerberos 權杖/票證，該權杖/票證取自 Active Directory，並由用戶端將其轉送至伺服器來驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="fd6a3-156">請註冊主機的服務主體名稱 (SPN)，而非應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="fd6a3-157">HTTP.sys 不支援 Nano Server 1709 版或更新版本上。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="fd6a3-158">若要使用 Windows 驗證和 HTTP.sys 使用 Nano Server，請使用[Server Core (microsoft/windowsservercore) 容器](https://hub.docker.com/r/microsoft/windowsservercore/)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="fd6a3-159">如需有關 Server Core 的詳細資訊，請參閱[什麼是 Windows Server 中的 Server Core 安裝選項？](/windows-server/administration/server-core/what-is-server-core)。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="fd6a3-160">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="fd6a3-160">Work with Windows Authentication</span></span>

<span data-ttu-id="fd6a3-161">匿名存取的設定狀態決定的方式`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-161">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="fd6a3-162">下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-162">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="fd6a3-163">不允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="fd6a3-163">Disallow anonymous access</span></span>

<span data-ttu-id="fd6a3-164">當您啟用 Windows 驗證，並已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`屬性沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-164">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="fd6a3-165">如果 IIS 網站 （或 HTTP.sys） 設定為不允許匿名存取，要求永遠不會到達您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-165">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="fd6a3-166">基於這個理由，`[AllowAnonymous]`屬性不適用。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-166">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="fd6a3-167">允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="fd6a3-167">Allow anonymous access</span></span>

<span data-ttu-id="fd6a3-168">當啟用 Windows 驗證和匿名存取時，使用`[Authorize]`和`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-168">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="fd6a3-169">`[Authorize]`屬性可讓您安全的應用程式真正需要 Windows 驗證的項目。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-169">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="fd6a3-170">`[AllowAnonymous]`屬性覆寫`[Authorize]`屬性允許匿名存取的應用程式內的使用方式。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-170">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="fd6a3-171">請參閱[簡單授權](xref:security/authorization/simple)屬性使用方式詳細資料。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-171">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="fd6a3-172">在 ASP.NET Core 2.x 中，`[Authorize]`屬性需要額外的設定，在*Startup.cs*挑戰進行 Windows 驗證的匿名要求。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-172">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="fd6a3-173">建議的設定會有些許出入所使用的 web 伺服器而有所不同。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-173">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="fd6a3-174">根據預設，缺少授權，才能存取頁面的使用者會看到空的 HTTP 403 回應。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-174">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="fd6a3-175">[StatusCodePages 中介軟體](xref:fundamentals/error-handling#configure-status-code-pages)可以設定為使用者提供更好的 「 拒絕存取 」 體驗。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-175">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="fd6a3-176">IIS</span><span class="sxs-lookup"><span data-stu-id="fd6a3-176">IIS</span></span>

<span data-ttu-id="fd6a3-177">如果使用 IIS，請將下列內容加入`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="fd6a3-177">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="fd6a3-178">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="fd6a3-178">HTTP.sys</span></span>

<span data-ttu-id="fd6a3-179">如果使用 HTTP.sys，將下列內容加入`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="fd6a3-179">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="fd6a3-180">模擬</span><span class="sxs-lookup"><span data-stu-id="fd6a3-180">Impersonation</span></span>

<span data-ttu-id="fd6a3-181">ASP.NET Core 不會實作模擬。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-181">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="fd6a3-182">應用程式執行的所有要求，使用應用程式集區或處理序身分識別的應用程式身分識別。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-182">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="fd6a3-183">如果您需要明確地執行動作的使用者身分，使用`WindowsIdentity.RunImpersonated`。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-183">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="fd6a3-184">在此內容中執行單一動作，然後關閉 內容。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-184">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="fd6a3-185">請注意，`RunImpersonated`不支援非同步作業，而不應該用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-185">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="fd6a3-186">比方說，包裝整個要求或中介軟體鏈結不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="fd6a3-186">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
