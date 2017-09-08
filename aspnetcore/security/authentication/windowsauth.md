---
title: "在 ASP.NET Core 中設定 Windows 驗證"
author: ardalis
description: "如何在 ASP.NET Core 中設定 Windows 驗證"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 008a647295334e957c33c6db7f80687645b3b928
ms.sourcegitcommit: 69b3255f8b6f5db9e7d21f391420602d7ba9f4db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="beddd-104">在 ASP.NET Core 中設定 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-104">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="beddd-105">由[Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="beddd-105">By [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="beddd-106">裝載於 IIS 或 WebListener ASP.NET Core 應用程式可以設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-106">Windows authentication can be configured for ASP.NET Core apps hosted with IIS or WebListener.</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="beddd-107">什麼是 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-107">What is Windows authentication</span></span>

<span data-ttu-id="beddd-108">Windows 驗證會仰賴來驗證使用者的 ASP.NET Core 應用程式的作業系統。</span><span class="sxs-lookup"><span data-stu-id="beddd-108">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="beddd-109">使用 Active Directory 網域身分識別或其他 Windows 帳戶來識別使用者在公司網路上執行您的伺服器時，您可以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-109">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="beddd-110">Windows 驗證是驗證的安全形式最佳適合內部網路的環境，其中使用者、 用戶端應用程式，以及網頁伺服器屬於相同的 Windows 網域。</span><span class="sxs-lookup"><span data-stu-id="beddd-110">Windows authentication is a secure form of authentication best suited to intranet environments where users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="beddd-111">[深入了解 Windows 驗證並將它安裝為 IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication)。</span><span class="sxs-lookup"><span data-stu-id="beddd-111">[Learn more about Windows Authentication and installing it for IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a><span data-ttu-id="beddd-112">啟用 ASP.NET Core 應用程式中的 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-112">Enabling Windows authentication in an ASP.NET Core application</span></span>

<span data-ttu-id="beddd-113">Visual Studio Web 應用程式範本可以設定為支援 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-113">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="using-the-windows-authentication-app-template"></a><span data-ttu-id="beddd-114">使用 Windows 驗證應用程式範本</span><span class="sxs-lookup"><span data-stu-id="beddd-114">Using the Windows authentication app template</span></span>

<span data-ttu-id="beddd-115">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="beddd-115">In Visual Studio:</span></span>
* <span data-ttu-id="beddd-116">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="beddd-116">Create a new ASP.NET Core Web Application.</span></span> 
* <span data-ttu-id="beddd-117">從範本清單中選取 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="beddd-117">Select Web Application from the list of templates.</span></span>
* <span data-ttu-id="beddd-118">選取 [變更驗證] 按鈕，然後選取**Windows 驗證**。</span><span class="sxs-lookup"><span data-stu-id="beddd-118">Select the Change Authentication button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="beddd-119">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="beddd-119">Run the app.</span></span> <span data-ttu-id="beddd-120">使用者名稱會出現在最上層應用程式的權限。</span><span class="sxs-lookup"><span data-stu-id="beddd-120">The username appears in the top right of the app.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="beddd-122">針對使用 IIS Express 的開發工作，範本會提供所有必要的組態，以使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-122">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="beddd-123">下一節顯示如何設定 ASP.NET Core 應用程式以手動方式提供 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-123">The next section shows how to configure an ASP.NET Core app manually for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="beddd-124">Visual Studio 設定 Windows 和匿名驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-124">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="beddd-125">[Visual Studio 屬性] 頁面中，偵錯索引標籤提供核取方塊進行 Windows 驗證和匿名驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-125">The Visual Studio properties page, debug tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Windows 驗證的瀏覽器螢幕擷取畫面](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="beddd-127">您也可以設定這些屬性在`launchSettings.json`檔案：</span><span class="sxs-lookup"><span data-stu-id="beddd-127">You can also configure these properties in the `launchSettings.json` file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a><span data-ttu-id="beddd-128">啟用與 IIS Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-128">Enabling Windows Authentication with IIS</span></span>

<span data-ttu-id="beddd-129">IIS 會使用[ASP.NET 核心模組](xref:fundamentals/servers/aspnet-core-module)(ANCM) 來裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="beddd-129">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) (ANCM) to host ASP.NET Core apps.</span></span> <span data-ttu-id="beddd-130">ANCM 流程 Windows 驗證來 IIS 預設。</span><span class="sxs-lookup"><span data-stu-id="beddd-130">The ANCM flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="beddd-131">於 IIS 時，應用程式專案，會完成設定 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-131">Configuration of Windows authentication is done within IIS, not the application project.</span></span> <span data-ttu-id="beddd-132">下列各節顯示如何使用 IIS 管理員來設定 ASP.NET Core 應用程式使用 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="beddd-132">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication:</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="beddd-133">建立新的 IIS 站台</span><span class="sxs-lookup"><span data-stu-id="beddd-133">Create a new IIS site</span></span>

<span data-ttu-id="beddd-134">指定的名稱和資料夾，並允許其建立新的應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="beddd-134">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="beddd-135">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-135">Customize Authentication</span></span>

<span data-ttu-id="beddd-136">開啟網站的 [驗證] 功能表。</span><span class="sxs-lookup"><span data-stu-id="beddd-136">Open the Authentication menu for the site.</span></span>

![IIS 驗證 功能表](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="beddd-138">停用匿名驗證，並啟用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="beddd-138">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![IIS 驗證設定](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="beddd-140">將專案發行到 IIS 站台資料夾</span><span class="sxs-lookup"><span data-stu-id="beddd-140">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="beddd-141">使用 Visual Studio 或.NET 核心 CLI*發行*應用程式的目的資料夾。</span><span class="sxs-lookup"><span data-stu-id="beddd-141">Using Visual Studio or the .NET Core CLI, *publish* your app to the destination folder.</span></span>

![Visual Studio 發行對話方塊](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="beddd-143">深入了解[發行至 IIS](https://docs.microsoft.com/aspnet/core/publishing/iis)。</span><span class="sxs-lookup"><span data-stu-id="beddd-143">Learn more about [publishing to IIS](https://docs.microsoft.com/aspnet/core/publishing/iis).</span></span>

<span data-ttu-id="beddd-144">啟動應用程式，以確認 Windows 驗證運作。</span><span class="sxs-lookup"><span data-stu-id="beddd-144">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enabling-windows-authentication-with-weblistener"></a><span data-ttu-id="beddd-145">啟用 Windows 驗證與 WebListener</span><span class="sxs-lookup"><span data-stu-id="beddd-145">Enabling Windows authentication with WebListener</span></span>

<span data-ttu-id="beddd-146">雖然 Kestrel 不支援 Windows 驗證，您可以使用[WebListener](xref:fundamentals/servers/weblistener)以支援在 Windows 上的自我裝載的案例。</span><span class="sxs-lookup"><span data-stu-id="beddd-146">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="beddd-147">下列範例會設定應用程式的 web 主機，以搭配 Windows 驗證使用 WebListener:</span><span class="sxs-lookup"><span data-stu-id="beddd-147">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a><span data-ttu-id="beddd-148">使用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="beddd-148">Working with Windows authentication</span></span>

<span data-ttu-id="beddd-149">如果您的應用程式使用 Windows 驗證和匿名存取，您可以使用``[Authorize]``和``[AllowAnonymous]``屬性。</span><span class="sxs-lookup"><span data-stu-id="beddd-149">If your app uses Windows authentication and anonymous access, you can use the ``[Authorize]`` and ``[AllowAnonymous]`` attributes.</span></span> <span data-ttu-id="beddd-150">沒有不需要匿名啟用的執行的應用程式``[Authorize]``; 應用程式會被視為要求驗證，會拒絕匿名要求。</span><span class="sxs-lookup"><span data-stu-id="beddd-150">Apps that do not have anonymous enabled do not require ``[Authorize]``; the  app is treated as requiring authentication, anonymous requests are rejected.</span></span> <span data-ttu-id="beddd-151">請注意，如果已設定的 IIS 站台**不**允許匿名存取，``[AllowAnonymous]``屬性會執行**不**允許匿名要求。</span><span class="sxs-lookup"><span data-stu-id="beddd-151">Note, if the IIS site is configured **not** to allow anonymous access, the ``[AllowAnonymous]`` attribute does **not** allow anonymous requests.</span></span> <span data-ttu-id="beddd-152">``[AllowAnonymous]``屬性覆寫``[Authorize]``屬性允許匿名存取的應用程式內的用法。</span><span class="sxs-lookup"><span data-stu-id="beddd-152">The ``[AllowAnonymous]`` attribute overrides ``[Authorize]`` attribute usage within apps that allow anonymous access.</span></span>

### <a name="impersonation"></a><span data-ttu-id="beddd-153">模擬</span><span class="sxs-lookup"><span data-stu-id="beddd-153">Impersonation</span></span>

<span data-ttu-id="beddd-154">ASP.NET Core 未實作模擬。</span><span class="sxs-lookup"><span data-stu-id="beddd-154">ASP.NET Core does not implement impersonation.</span></span> <span data-ttu-id="beddd-155">應用程式執行與使用應用程式集區或處理序身分識別的所有要求的應用程式識別。</span><span class="sxs-lookup"><span data-stu-id="beddd-155">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="beddd-156">如果您需要明確地執行代表使用者的動作，使用``WindowsIdentity.RunImpersonated``。</span><span class="sxs-lookup"><span data-stu-id="beddd-156">If you need to explicitly perform an action on behalf of a user, use ``WindowsIdentity.RunImpersonated``.</span></span> <span data-ttu-id="beddd-157">在此內容中執行單一動作，然後關閉 內容。</span><span class="sxs-lookup"><span data-stu-id="beddd-157">Run a single action in this context and then close the context.</span></span> <span data-ttu-id="beddd-158">請注意，``RunImpersonated``不支援非同步和不應該用於複雜的案例。</span><span class="sxs-lookup"><span data-stu-id="beddd-158">Note that ``RunImpersonated`` does not support async and should not be used for complex scenarios.</span></span> <span data-ttu-id="beddd-159">例如，包裝整個要求或中介軟體鏈結不支援或建議。</span><span class="sxs-lookup"><span data-stu-id="beddd-159">For example, wrapping entire requests or middleware chains is not supported or recommended.</span></span>
