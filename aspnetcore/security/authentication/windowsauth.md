---
title: "在 ASP.NET Core 中設定 Windows 驗證"
author: ardalis
description: "本文說明如何在 ASP.NET Core，使用 IIS Express、 IIS、 HTTP.sys 和 WebListener 中設定 Windows 驗證。"
keywords: "ASP.NET Core，Windows 驗證、 授權屬性、 AllowAnonymous 屬性"
ms.author: riande
manager: wpickett
ms.date: 10/24/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 703f924d049a267cb8bee22e63e6669b13099c53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="80fc5-104">在 ASP.NET Core 應用程式中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-104">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="80fc5-105">由[Steve Smith](https://ardalis.com)和[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="80fc5-105">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="80fc5-106">可以針對以 IIS 裝載的 ASP.NET Core 應用程式設定 Windows 驗證[HTTP.sys](xref:fundamentals/servers/httpsys)，或[WebListener](xref:fundamentals/servers/weblistener)。</span><span class="sxs-lookup"><span data-stu-id="80fc5-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="80fc5-107">什麼是 Windows 驗證？</span><span class="sxs-lookup"><span data-stu-id="80fc5-107">What is Windows authentication?</span></span>

<span data-ttu-id="80fc5-108">Windows 驗證會仰賴來驗證使用者的 ASP.NET Core 應用程式的作業系統。</span><span class="sxs-lookup"><span data-stu-id="80fc5-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="80fc5-109">使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路上執行您的伺服器時，您可以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="80fc5-110">Windows 驗證最適合內部網路環境中的使用者、 用戶端應用程式，以及網頁伺服器屬於相同的 Windows 網域。</span><span class="sxs-lookup"><span data-stu-id="80fc5-110">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="80fc5-111">[深入了解 Windows 驗證並將它安裝為 IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/)。</span><span class="sxs-lookup"><span data-stu-id="80fc5-111">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="80fc5-112">啟用 ASP.NET Core 應用程式中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-112">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="80fc5-113">Visual Studio Web 應用程式範本可以設定為支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="80fc5-114">使用 Windows 驗證應用程式範本</span><span class="sxs-lookup"><span data-stu-id="80fc5-114">Use the Windows authentication app template</span></span>

<span data-ttu-id="80fc5-115">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="80fc5-115">In Visual Studio:</span></span>
1. <span data-ttu-id="80fc5-116">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-116">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="80fc5-117">從範本清單中選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-117">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="80fc5-118">選取**變更驗證**按鈕，然後選取**Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="80fc5-118">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="80fc5-119">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-119">Run the app.</span></span> <span data-ttu-id="80fc5-120">使用者名稱會出現在最上層應用程式的權限。</span><span class="sxs-lookup"><span data-stu-id="80fc5-120">The username appears in the top right of the app.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="80fc5-122">針對使用 IIS Express 的開發工作，範本會提供所有必要的組態，以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="80fc5-123">下一節顯示如何手動設定 Windows 驗證的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-123">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="80fc5-124">Visual Studio 設定 Windows 和匿名驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="80fc5-125">Visual Studio 專案**屬性**網頁的**偵錯** 索引標籤提供 Windows 驗證和匿名驗證 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="80fc5-125">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="80fc5-127">或者，可以設定這兩個屬性在*launchSettings.json*檔案：</span><span class="sxs-lookup"><span data-stu-id="80fc5-127">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="80fc5-128">啟用與 IIS Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-128">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="80fc5-129">IIS 會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)(ANCM) 來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="80fc5-130">ANCM 流程 Windows 驗證來 IIS 預設。</span><span class="sxs-lookup"><span data-stu-id="80fc5-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="80fc5-131">於 IIS 時，應用程式專案，會完成設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="80fc5-132">下列各節將示範如何使用 IIS 管理員來設定 ASP.NET Core 應用程式使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="80fc5-133">建立新的 IIS 站台</span><span class="sxs-lookup"><span data-stu-id="80fc5-133">Create a new IIS site</span></span>

<span data-ttu-id="80fc5-134">指定的名稱和資料夾，並允許其建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="80fc5-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="80fc5-135">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-135">Customize authentication</span></span>

<span data-ttu-id="80fc5-136">開啟網站的 [驗證] 功能表。</span><span class="sxs-lookup"><span data-stu-id="80fc5-136">Open the Authentication menu for the site.</span></span>

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="80fc5-138">停用匿名驗證，並啟用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="80fc5-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="80fc5-140">將專案發行到 IIS 站台資料夾</span><span class="sxs-lookup"><span data-stu-id="80fc5-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="80fc5-141">使用 Visual Studio 或.NET 核心 CLI，將應用程式發行至目的資料夾。</span><span class="sxs-lookup"><span data-stu-id="80fc5-141">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Visual Studio 發行對話方塊](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="80fc5-143">深入了解[發行至 IIS](xref:publishing/iis)。</span><span class="sxs-lookup"><span data-stu-id="80fc5-143">Learn more about [publishing to IIS](xref:publishing/iis).</span></span>

<span data-ttu-id="80fc5-144">啟動應用程式，以確認 Windows 驗證運作。</span><span class="sxs-lookup"><span data-stu-id="80fc5-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="80fc5-145">啟用 Windows 驗證與 HTTP.sys 或 WebListener</span><span class="sxs-lookup"><span data-stu-id="80fc5-145">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="80fc5-146">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="80fc5-146">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="80fc5-147">雖然 Kestrel 不支援 Windows 驗證，您可以使用[HTTP.sys](xref:fundamentals/servers/httpsys)以支援在 Windows 上的自我裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="80fc5-147">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="80fc5-148">下列範例會設定 HTTP.sys 使用 Windows 驗證的應用程式的 web 主機：</span><span class="sxs-lookup"><span data-stu-id="80fc5-148">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="80fc5-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="80fc5-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="80fc5-150">雖然 Kestrel 不支援 Windows 驗證，您可以使用[WebListener](xref:fundamentals/servers/weblistener)以支援在 Windows 上的自我裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="80fc5-150">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="80fc5-151">下列範例會設定應用程式的 web 主機，以搭配 Windows 驗證使用 WebListener:</span><span class="sxs-lookup"><span data-stu-id="80fc5-151">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="80fc5-152">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="80fc5-152">Work with Windows authentication</span></span>

<span data-ttu-id="80fc5-153">匿名存取的設定狀態的方式會決定`[Authorize]`和`[AllowAnonymous]`應用程式中使用屬性。</span><span class="sxs-lookup"><span data-stu-id="80fc5-153">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="80fc5-154">下列兩節會說明如何處理不允許和允許設定狀態的匿名存取。</span><span class="sxs-lookup"><span data-stu-id="80fc5-154">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="80fc5-155">不允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="80fc5-155">Disallow anonymous access</span></span>

<span data-ttu-id="80fc5-156">啟用 Windows 驗證，且已停用匿名存取，`[Authorize]`和`[AllowAnonymous]`沒有作用。</span><span class="sxs-lookup"><span data-stu-id="80fc5-156">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="80fc5-157">如果 IIS 網站 （或 HTTP.sys 或 WebListener 伺服器） 設定為不允許匿名存取，要求永遠不會到達您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="80fc5-157">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="80fc5-158">基於這個理由，`[AllowAnonymous]`屬性不適用。</span><span class="sxs-lookup"><span data-stu-id="80fc5-158">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="80fc5-159">允許匿名存取</span><span class="sxs-lookup"><span data-stu-id="80fc5-159">Allow anonymous access</span></span>

<span data-ttu-id="80fc5-160">啟用 Windows 驗證和匿名存取，當使用`[Authorize]`和`[AllowAnonymous]`屬性。</span><span class="sxs-lookup"><span data-stu-id="80fc5-160">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="80fc5-161">`[Authorize]`屬性可讓您保護應用程式確實需要 Windows 驗證的片段。</span><span class="sxs-lookup"><span data-stu-id="80fc5-161">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="80fc5-162">`[AllowAnonymous]`屬性覆寫`[Authorize]`屬性允許匿名存取的應用程式內的用法。</span><span class="sxs-lookup"><span data-stu-id="80fc5-162">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="80fc5-163">請參閱[簡單授權](xref:security/authorization/simple)的屬性使用方式詳細資料。</span><span class="sxs-lookup"><span data-stu-id="80fc5-163">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="80fc5-164">在 ASP.NET Core 2.x，`[Authorize]`屬性需要額外的設定中*Startup.cs*挑戰 Windows 驗證的匿名要求。</span><span class="sxs-lookup"><span data-stu-id="80fc5-164">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="80fc5-165">建議的組態設定稍微依據所使用的 web 伺服器而異。</span><span class="sxs-lookup"><span data-stu-id="80fc5-165">The recommended configuration varies slightly based on the web server being used.</span></span>

#### <a name="iis"></a><span data-ttu-id="80fc5-166">IIS</span><span class="sxs-lookup"><span data-stu-id="80fc5-166">IIS</span></span>

<span data-ttu-id="80fc5-167">如果使用 IIS，將下列內容加入`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="80fc5-167">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="80fc5-168">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="80fc5-168">HTTP.sys</span></span>

<span data-ttu-id="80fc5-169">如果使用 HTTP.sys，將下列內容加入`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="80fc5-169">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="80fc5-170">模擬</span><span class="sxs-lookup"><span data-stu-id="80fc5-170">Impersonation</span></span>

<span data-ttu-id="80fc5-171">ASP.NET Core 不會實作模擬。</span><span class="sxs-lookup"><span data-stu-id="80fc5-171">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="80fc5-172">應用程式執行與使用應用程式集區或處理序身分識別的所有要求的應用程式識別。</span><span class="sxs-lookup"><span data-stu-id="80fc5-172">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="80fc5-173">如果您需要明確地執行代表使用者的動作，使用`WindowsIdentity.RunImpersonated`。</span><span class="sxs-lookup"><span data-stu-id="80fc5-173">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="80fc5-174">在此內容中執行單一動作，然後關閉 內容。</span><span class="sxs-lookup"><span data-stu-id="80fc5-174">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="80fc5-175">請注意，`RunImpersonated`不支援非同步作業，而且不應該用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="80fc5-175">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="80fc5-176">例如，包裝整個要求或中介軟體鏈結不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="80fc5-176">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
