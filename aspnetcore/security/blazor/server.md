---
title: 安全ASP.NET核心Blazor伺服器應用
author: guardrex
description: 瞭解如何緩解對伺服器應用的Blazor安全威脅。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/server
ms.openlocfilehash: bd03f811d0425fdfdb7bbbc24fea5481b49b8ed3
ms.sourcegitcommit: 9675db7bf4b67ae269f9226b6f6f439b5cce4603
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80626015"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a><span data-ttu-id="bc5c8-103">安全ASP.NET核心布拉佐伺服器應用程式</span><span class="sxs-lookup"><span data-stu-id="bc5c8-103">Secure ASP.NET Core Blazor Server apps</span></span>

<span data-ttu-id="bc5c8-104">哈威爾[·卡爾瓦羅·納爾遜](https://github.com/javiercn)</span><span class="sxs-lookup"><span data-stu-id="bc5c8-104">By [Javier Calvarro Nelson](https://github.com/javiercn)</span></span>

<span data-ttu-id="bc5c8-105">Blazor Server 應用採用*有狀態*的數據處理模型,其中伺服器和用戶端保持長期關係。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-105">Blazor Server apps adopt a *stateful* data processing model, where the server and client maintain a long-lived relationship.</span></span> <span data-ttu-id="bc5c8-106">持久狀態由電路保持,該[電路](xref:blazor/state-management)可以跨越可能壽命很長的連接。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-106">The persistent state is maintained by a [circuit](xref:blazor/state-management), which can span connections that are also potentially long-lived.</span></span>

<span data-ttu-id="bc5c8-107">當使用者存訪 Blazor Server 網站時,伺服器會在伺服器的記憶體中創建一個電路。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-107">When a user visits a Blazor Server site, the server creates a circuit in the server's memory.</span></span> <span data-ttu-id="bc5c8-108">該電路向瀏覽器指示要呈現哪些內容並回應事件,例如當使用者在 UI 中選擇按鈕時。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-108">The circuit indicates to the browser what content to render and responds to events, such as when the user selects a button in the UI.</span></span> <span data-ttu-id="bc5c8-109">要執行這些操作,電路在使用者的瀏覽器和伺服器上的 .NET 方法中調用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-109">To perform these actions, a circuit invokes JavaScript functions in the user's browser and .NET methods on the server.</span></span> <span data-ttu-id="bc5c8-110">這種基於JAVAScript的雙向交互稱為[JAVAScript互操作(JS互操作)。](xref:blazor/call-javascript-from-dotnet)</span><span class="sxs-lookup"><span data-stu-id="bc5c8-110">This two-way JavaScript-based interaction is referred to as [JavaScript interop (JS interop)](xref:blazor/call-javascript-from-dotnet).</span></span>

<span data-ttu-id="bc5c8-111">由於 JS 互通透過 Internet 進行,並且用戶端使用遠端瀏覽器,因此 Blazor Server 應用共用大多數 Web 應用安全問題。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-111">Because JS interop occurs over the Internet and the client uses a remote browser, Blazor Server apps share most web app security concerns.</span></span> <span data-ttu-id="bc5c8-112">本主題介紹對 Blazor Server 應用的常見威脅,並提供側重於面向 Internet 的應用的威脅緩解指南。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-112">This topic describes common threats to Blazor Server apps and provides threat mitigation guidance focused on Internet-facing apps.</span></span>

<span data-ttu-id="bc5c8-113">在受限環境中(如公司網路或 Intranet 內部)中,一些緩解指南要麼:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-113">In constrained environments, such as inside corporate networks or intranets, some of the mitigation guidance either:</span></span>

* <span data-ttu-id="bc5c8-114">不適用於受約束的環境。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-114">Doesn't apply in the constrained environment.</span></span>
* <span data-ttu-id="bc5c8-115">不值得花費成本來實現,因為安全風險在受限的環境中很低。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-115">Isn't worth the cost to implement because the security risk is low in a constrained environment.</span></span>

## <a name="blazor-server-project-template"></a><span data-ttu-id="bc5c8-116">布拉佐伺服器項目範本</span><span class="sxs-lookup"><span data-stu-id="bc5c8-116">Blazor Server project template</span></span>

<span data-ttu-id="bc5c8-117">布拉佐伺服器項目範本可以配置為在創建專案時進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-117">The Blazor Server project template can be configured for authentication when the project is created.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="bc5c8-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc5c8-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bc5c8-119">按照<xref:blazor/get-started>文章中的可視化工作室指南創建具有身份驗證機制的新 Blazor Server 專案。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-119">Follow the Visual Studio guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism.</span></span>

<span data-ttu-id="bc5c8-120">在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 對話方塊中選擇 [Blazor 伺服器應用程式]\*\*\*\* 範本之後，請選取 [驗證]\*\*\*\* 下的 [變更]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-120">After choosing the **Blazor Server App** template in the **Create a new ASP.NET Core Web Application** dialog, select **Change** under **Authentication**.</span></span>

<span data-ttu-id="bc5c8-121">對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：</span><span class="sxs-lookup"><span data-stu-id="bc5c8-121">A dialog opens to offer the same set of authentication mechanisms available for other ASP.NET Core projects:</span></span>

* <span data-ttu-id="bc5c8-122">**沒有認證**</span><span class="sxs-lookup"><span data-stu-id="bc5c8-122">**No Authentication**</span></span>
* <span data-ttu-id="bc5c8-123">**個別使用者帳戶** &ndash; 使用者帳戶能以下列方式儲存：</span><span class="sxs-lookup"><span data-stu-id="bc5c8-123">**Individual User Accounts** &ndash; User accounts can be stored:</span></span>
  * <span data-ttu-id="bc5c8-124">使用 ASP.NET Core 的[身分識別](xref:security/authentication/identity)系統儲存在應用程式內。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-124">Within the app using ASP.NET Core's [Identity](xref:security/authentication/identity) system.</span></span>
  * <span data-ttu-id="bc5c8-125">使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-125">With [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="bc5c8-126">**工作或學校帳戶**</span><span class="sxs-lookup"><span data-stu-id="bc5c8-126">**Work or School Accounts**</span></span>
* <span data-ttu-id="bc5c8-127">**Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="bc5c8-127">**Windows Authentication**</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="bc5c8-128">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc5c8-128">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bc5c8-129">按照<xref:blazor/get-started>本文中的可視化工作室代碼指南創建具有身份驗證機制的新 Blazor Server 專案:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-129">Follow the Visual Studio Code guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="bc5c8-130">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-130">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="bc5c8-131">驗證機制</span><span class="sxs-lookup"><span data-stu-id="bc5c8-131">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="bc5c8-132">`{AUTHENTICATION}` 值</span><span class="sxs-lookup"><span data-stu-id="bc5c8-132">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="bc5c8-133">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="bc5c8-133">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="bc5c8-134">個人</span><span class="sxs-lookup"><span data-stu-id="bc5c8-134">Individual</span></span><br><span data-ttu-id="bc5c8-135">搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-135">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="bc5c8-136">個人</span><span class="sxs-lookup"><span data-stu-id="bc5c8-136">Individual</span></span><br><span data-ttu-id="bc5c8-137">儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-137">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="bc5c8-138">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="bc5c8-138">Work or School Accounts</span></span><br><span data-ttu-id="bc5c8-139">適用於單一租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-139">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="bc5c8-140">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="bc5c8-140">Work or School Accounts</span></span><br><span data-ttu-id="bc5c8-141">適用於多個租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-141">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="bc5c8-142">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="bc5c8-142">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="bc5c8-143">命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-143">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="bc5c8-144">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-144">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="bc5c8-145">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bc5c8-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="bc5c8-146">請按照「視覺工作室」瞭解本文中的<xref:blazor/get-started>Mac 指南。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-146">Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.</span></span>

1. <span data-ttu-id="bc5c8-147">在 **「設定新的 Blazor 伺服器應用**」步驟中,從 **「身份驗證**」下拉清單中選擇 **「個人身份驗證(應用內」。)。**</span><span class="sxs-lookup"><span data-stu-id="bc5c8-147">On the **Configure your new Blazor Server App** step, select **Individual Authentication (in-app)** from the **Authentication** drop down.</span></span>

1. <span data-ttu-id="bc5c8-148">該應用程式是為存儲在應用中的具有ASP.NET核心標識的單個用戶創建的。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-148">The app is created for individual users stored in the app with ASP.NET Core Identity.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="bc5c8-149">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bc5c8-149">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="bc5c8-150">按照<xref:blazor/get-started>本文中的 .NET Core CLI 指南建立具有身份驗證機制的新 Blazor Server 專案:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-150">Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:</span></span>

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

<span data-ttu-id="bc5c8-151">下表顯示允許的驗證值 (`{AUTHENTICATION}`)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-151">Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.</span></span>

| <span data-ttu-id="bc5c8-152">驗證機制</span><span class="sxs-lookup"><span data-stu-id="bc5c8-152">Authentication mechanism</span></span>                                                                 | <span data-ttu-id="bc5c8-153">`{AUTHENTICATION}` 值</span><span class="sxs-lookup"><span data-stu-id="bc5c8-153">`{AUTHENTICATION}` value</span></span> |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| <span data-ttu-id="bc5c8-154">不需要驗證</span><span class="sxs-lookup"><span data-stu-id="bc5c8-154">No Authentication</span></span>                                                                        | `None`                   |
| <span data-ttu-id="bc5c8-155">個人</span><span class="sxs-lookup"><span data-stu-id="bc5c8-155">Individual</span></span><br><span data-ttu-id="bc5c8-156">搭配 ASP.NET Core 身分識別儲存在應用程式中的使用者。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-156">Users stored in the app with ASP.NET Core Identity.</span></span>                        | `Individual`             |
| <span data-ttu-id="bc5c8-157">個人</span><span class="sxs-lookup"><span data-stu-id="bc5c8-157">Individual</span></span><br><span data-ttu-id="bc5c8-158">儲存在 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 中的使用者。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-158">Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span> | `IndividualB2C`          |
| <span data-ttu-id="bc5c8-159">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="bc5c8-159">Work or School Accounts</span></span><br><span data-ttu-id="bc5c8-160">適用於單一租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-160">Organizational authentication for a single tenant.</span></span>            | `SingleOrg`              |
| <span data-ttu-id="bc5c8-161">公司或學校帳戶</span><span class="sxs-lookup"><span data-stu-id="bc5c8-161">Work or School Accounts</span></span><br><span data-ttu-id="bc5c8-162">適用於多個租用戶的組織驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-162">Organizational authentication for multiple tenants.</span></span>           | `MultiOrg`               |
| <span data-ttu-id="bc5c8-163">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="bc5c8-163">Windows Authentication</span></span>                                                                   | `Windows`                |

<span data-ttu-id="bc5c8-164">命令會建立搭配為 `{APP NAME}` 所提供的值來命名的資料夾，並會使用該資料夾名稱作為應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-164">The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name.</span></span> <span data-ttu-id="bc5c8-165">如需詳細資訊，請參閱 .NET Core 指南中的 [dotnet new](/dotnet/core/tools/dotnet-new) 命令。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-165">For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.</span></span>

---

## <a name="pass-tokens-to-a-blazor-server-app"></a><span data-ttu-id="bc5c8-166">將權杖傳遞到 Blazor 伺服器應用</span><span class="sxs-lookup"><span data-stu-id="bc5c8-166">Pass tokens to a Blazor Server app</span></span>

<span data-ttu-id="bc5c8-167">使用常規剃刀頁面或 MVC 應用驗證 Blazor Server 應用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-167">Authenticate the Blazor Server app as you would with a regular Razor Pages or MVC app.</span></span> <span data-ttu-id="bc5c8-168">預配令牌並將其保存到身份驗證 Cookie。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-168">Provision and save the tokens to the authentication cookie.</span></span> <span data-ttu-id="bc5c8-169">例如：</span><span class="sxs-lookup"><span data-stu-id="bc5c8-169">For example:</span></span>

```csharp
using Microsoft.AspNetCore.Authentication.OpenIdConnect;

...

services.Configure<OpenIdConnectOptions>(AzureADDefaults.OpenIdScheme, options =>
{
    options.ResponseType = "code";
    options.SaveTokens = true;

    options.Scope.Add("offline_access");
    options.Scope.Add("{SCOPE}");
    options.Resource = "{RESOURCE}";
});
```

<span data-ttu-id="bc5c8-170">有關範例代碼(包括完整`Startup.ConfigureServices`範例),請參閱[將權杖傳遞到伺服器端 Blazor 應用程式](https://github.com/javiercn/blazor-server-aad-sample)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-170">For sample code, including a complete `Startup.ConfigureServices` example, see the [Passing tokens to a server-side Blazor application](https://github.com/javiercn/blazor-server-aad-sample).</span></span>

<span data-ttu-id="bc5c8-171">定義要在初始應用狀態中傳遞的類,並帶有存取和刷新權杖:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-171">Define a class to pass in the initial app state with the access and refresh tokens:</span></span>

```csharp
public class InitialApplicationState
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="bc5c8-172">定義以 Blazor 應用中使用**的範圍範圍**權碼提供者服務來解決 DI 中的權杖:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-172">Define a **scoped** token provider service that can be used within the Blazor app to resolve the tokens from DI:</span></span>

```csharp
using System;
using System.Security.Claims;
using System.Threading.Tasks;

public class TokenProvider
{
    public string AccessToken { get; set; }
    public string RefreshToken { get; set; }
}
```

<span data-ttu-id="bc5c8-173">在`Startup.ConfigureServices`中,新增服務:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-173">In `Startup.ConfigureServices`, add services for:</span></span>

* `IHttpClientFactory`
* `TokenProvider`

```csharp
services.AddHttpClient();
services.AddScoped<TokenProvider>();
```

<span data-ttu-id="bc5c8-174">在 *_Host.cshtml*檔案中,`InitialApplicationState`建立與實體並將其作為參數傳遞給應用:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-174">In the *_Host.cshtml* file, create and instance of `InitialApplicationState` and pass it as a parameter to the app:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authentication

...

@{
    var tokens = new InitialApplicationState
    {
        AccessToken = await HttpContext.GetTokenAsync("access_token"),
        RefreshToken = await HttpContext.GetTokenAsync("refresh_token")
    };
}

<app>
    <component type="typeof(App)" param-InitialState="tokens" 
        render-mode="ServerPrerendered" />
</app>
```

<span data-ttu-id="bc5c8-175">在`App`元件 *(App.razor)* 中,解析服務,並使用參數中的資料初始化該服務:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-175">In the `App` component (*App.razor*), resolve the service and initialize it with the data from the parameter:</span></span>

```razor
@inject TokenProvider TokensProvider

...

@code {
    [Parameter]
    public InitialApplicationState InitialState { get; set; }

    protected override Task OnInitializedAsync()
    {
        TokensProvider.AccessToken = InitialState.AccessToken;
        TokensProvider.RefreshToken = InitialState.RefreshToken;

        return base.OnInitializedAsync();
    }
}
```

<span data-ttu-id="bc5c8-176">在發出安全 API 要求的服務中,注入權杖提供者並檢索權以呼叫 API:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-176">In the service that makes a secure API request, inject the token provider and retrieve the token to call the API:</span></span>

```csharp
public class WeatherForecastService
{
    private readonly TokenProvider _store;

    public WeatherForecastService(IHttpClientFactory clientFactory, 
        TokenProvider tokenProvider)
    {
        Client = clientFactory.CreateClient();
        _store = tokenProvider;
    }

    public HttpClient Client { get; }

    public async Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        var token = _store.AccessToken;
        var request = new HttpRequestMessage(HttpMethod.Get, 
            "https://localhost:5003/WeatherForecast");
        request.Headers.Add("Authorization", $"Bearer {token}");
        var response = await Client.SendAsync(request);
        response.EnsureSuccessStatusCode();

        return await response.Content.ReadAsAsync<WeatherForecast[]>();
    }
}
```

## <a name="resource-exhaustion"></a><span data-ttu-id="bc5c8-177">資源耗盡</span><span class="sxs-lookup"><span data-stu-id="bc5c8-177">Resource exhaustion</span></span>

<span data-ttu-id="bc5c8-178">當用戶端與伺服器交互並導致伺服器消耗過多資源時,可能會出現資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-178">Resource exhaustion can occur when a client interacts with the server and causes the server to consume excessive resources.</span></span> <span data-ttu-id="bc5c8-179">過多的資源消耗主要影響:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-179">Excessive resource consumption primarily affects:</span></span>

* [<span data-ttu-id="bc5c8-180">Cpu</span><span class="sxs-lookup"><span data-stu-id="bc5c8-180">CPU</span></span>](#cpu)
* [<span data-ttu-id="bc5c8-181">記憶體</span><span class="sxs-lookup"><span data-stu-id="bc5c8-181">Memory</span></span>](#memory)
* [<span data-ttu-id="bc5c8-182">用戶端連線</span><span class="sxs-lookup"><span data-stu-id="bc5c8-182">Client connections</span></span>](#client-connections)

<span data-ttu-id="bc5c8-183">拒絕服務 (DoS) 攻擊通常會消耗應用或伺服器的資源。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-183">Denial of service (DoS) attacks usually seek to exhaust an app or server's resources.</span></span> <span data-ttu-id="bc5c8-184">但是,資源耗盡不一定是系統攻擊的結果。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-184">However, resource exhaustion isn't necessarily the result of an attack on the system.</span></span> <span data-ttu-id="bc5c8-185">例如,由於使用者需求高,有限資源可能會耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-185">For example, finite resources can be exhausted due to high user demand.</span></span> <span data-ttu-id="bc5c8-186">DoS 在[拒絕服務 (DoS) 攻擊](#denial-of-service-dos-attacks)部分中進一步介紹。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-186">DoS is covered further in the [Denial of service (DoS) attacks](#denial-of-service-dos-attacks) section.</span></span>

<span data-ttu-id="bc5c8-187">Blazor 框架外部的資源(如資料庫和檔句柄(用於讀取和寫入檔)也可能經歷資源耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-187">Resources external to the Blazor framework, such as databases and file handles (used to read and write files), may also experience resource exhaustion.</span></span> <span data-ttu-id="bc5c8-188">如需詳細資訊，請參閱 <xref:performance/performance-best-practices>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-188">For more information, see <xref:performance/performance-best-practices>.</span></span>

### <a name="cpu"></a><span data-ttu-id="bc5c8-189">CPU</span><span class="sxs-lookup"><span data-stu-id="bc5c8-189">CPU</span></span>

<span data-ttu-id="bc5c8-190">當一個或多個用戶端強制伺服器執行密集的 CPU 工作時,可能會出現 CPU 耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-190">CPU exhaustion can occur when one or more clients force the server to perform intensive CPU work.</span></span>

<span data-ttu-id="bc5c8-191">例如,考慮一個布拉佐伺服器應用程式,它計算*一個斐波納奇數位*。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-191">For example, consider a Blazor Server app that calculates a *Fibonnacci number*.</span></span> <span data-ttu-id="bc5c8-192">菲博納奇數位由菲博納奇序列生成,其中序列中的每個數位都是前兩個數位的總和。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-192">A Fibonnacci number is produced from a Fibonnacci sequence, where each number in the sequence is the sum of the two preceding numbers.</span></span> <span data-ttu-id="bc5c8-193">到達答案所需的工作量取決於序列的長度和初始值的大小。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-193">The amount of work required to reach the answer depends on the length of the sequence and the size of the initial value.</span></span> <span data-ttu-id="bc5c8-194">如果應用未對用戶端的請求進行限制,則 CPU 密集型計算可能會支配 CPU 的時間,並降低其他任務的性能。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-194">If the app doesn't place limits on a client's request, the CPU-intensive calculations may dominate the CPU's time and diminish the performance of other tasks.</span></span> <span data-ttu-id="bc5c8-195">過多的資源消耗是影響可用性的安全問題。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-195">Excessive resource consumption is a security concern impacting availability.</span></span>

<span data-ttu-id="bc5c8-196">CPU 耗盡是所有面向公眾的應用的問題。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-196">CPU exhaustion is a concern for all public-facing apps.</span></span> <span data-ttu-id="bc5c8-197">在常規 Web 應用中,請求和連接超時作為一種保護措施,但 Blazor Server 應用不提供相同的安全措施。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-197">In regular web apps, requests and connections time out as a safeguard, but Blazor Server apps don't provide the same safeguards.</span></span> <span data-ttu-id="bc5c8-198">Blazor Server 應用在執行潛在的 CPU 密集型工作之前必須包括適當的檢查和限制。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-198">Blazor Server apps must include appropriate checks and limits before performing potentially CPU-intensive work.</span></span>

### <a name="memory"></a><span data-ttu-id="bc5c8-199">記憶體</span><span class="sxs-lookup"><span data-stu-id="bc5c8-199">Memory</span></span>

<span data-ttu-id="bc5c8-200">當一個或多個客戶端強制伺服器消耗大量記憶體時,可能會出現記憶體耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-200">Memory exhaustion can occur when one or more clients force the server to consume a large amount of memory.</span></span>

<span data-ttu-id="bc5c8-201">例如,請考慮 Blazor 伺服器端應用,該應用具有接受並顯示專案列表的元件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-201">For example, consider a Blazor-server side app with a component that accepts and displays a list of items.</span></span> <span data-ttu-id="bc5c8-202">如果 Blazor 應用沒有限制允許的項目數或呈現回用戶端的專案數,則記憶體密集型處理和呈現可能會支配伺服器的記憶體,使其達到伺服器性能受損程度。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-202">If the Blazor app doesn't place limits on the number of items allowed or the number of items rendered back to the client, the memory-intensive processing and rendering may dominate the server's memory to the point where performance of the server suffers.</span></span> <span data-ttu-id="bc5c8-203">伺服器可能會崩潰或慢速到它似乎已崩潰的點。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-203">The server may crash or slow to the point that it appears to have crashed.</span></span>

<span data-ttu-id="bc5c8-204">請考慮以下方案,用於維護和顯示與伺服器上的潛在記憶體耗盡方案相關的專案清單:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-204">Consider the following scenario for maintaining and displaying a list of items that pertain to a potential memory exhaustion scenario on the server:</span></span>

* <span data-ttu-id="bc5c8-205">屬性或欄位中的專案`List<MyItem>`使用伺服器的記憶體。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-205">The items in a `List<MyItem>` property or field use the server's memory.</span></span> <span data-ttu-id="bc5c8-206">如果應用允許項目清單無限制增長,則存在伺服器記憶體不足的風險。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-206">If the app allows the list of items to grow unbounded, there's a risk of the server running out of memory.</span></span> <span data-ttu-id="bc5c8-207">記憶體不足會導致當前會話結束(崩潰),並且該伺服器實例中的所有併發會話都會收到記憶體不足異常。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-207">Running out of memory causes the current session to end (crash) and all of the concurrent sessions in that server instance receive an out-of-memory exception.</span></span> <span data-ttu-id="bc5c8-208">為了防止發生此情況,應用必須使用對併發使用者施加項限制的數據結構。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-208">To prevent this scenario from occurring, the app must use a data structure that imposes an item limit on concurrent users.</span></span>
* <span data-ttu-id="bc5c8-209">如果分頁方案不用於呈現,則伺服器對 UI 中不可見的物件使用其他記憶體。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-209">If a paging scheme isn't used for rendering, the server uses additional memory for objects that aren't visible in the UI.</span></span> <span data-ttu-id="bc5c8-210">如果項目數量沒有限制,記憶體需求可能會耗盡可用的伺服器記憶體。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-210">Without a limit on the number of items, memory demands may exhaust the available server memory.</span></span> <span data-ttu-id="bc5c8-211">要防止這種情況,請使用以下方法之一:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-211">To prevent this scenario, use one of the following approaches:</span></span>
  * <span data-ttu-id="bc5c8-212">渲染時使用分頁清單。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-212">Use paginated lists when rendering.</span></span>
  * <span data-ttu-id="bc5c8-213">僅顯示前 100 到 1,000 個專案,並要求使用者輸入搜尋條件以查找顯示的專案以外的專案。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-213">Only display the first 100 to 1,000 items and require the user to enter search criteria to find items beyond the items displayed.</span></span>
  * <span data-ttu-id="bc5c8-214">對於更高級的呈現方案,實現支援*虛擬化*的清單或網格。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-214">For a more advanced rendering scenario, implement lists or grids that support *virtualization*.</span></span> <span data-ttu-id="bc5c8-215">使用虛擬化,清單僅呈現當前對用戶可見的項子集。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-215">Using virtualization, lists only render a subset of items currently visible to the user.</span></span> <span data-ttu-id="bc5c8-216">當使用者與 UI 中的滾動條互動時,元件僅呈現顯示所需的項。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-216">When the user interacts with the scrollbar in the UI, the component renders only those items required for display.</span></span> <span data-ttu-id="bc5c8-217">顯示當前不需要的專案可以保存在輔助存儲中,這是理想的方法。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-217">The items that aren't currently required for display can be held in secondary storage, which is the ideal approach.</span></span> <span data-ttu-id="bc5c8-218">未顯示的專案也可以記在記憶體中,這不太理想。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-218">Undisplayed items can also be held in memory, which is less ideal.</span></span>

<span data-ttu-id="bc5c8-219">Blazor Server 應用為有狀態應用(如 WPF、Windows 窗體或 Blazor WebAssembly)提供與其他 UI 框架類似的程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-219">Blazor Server apps offer a similar programming model to other UI frameworks for stateful apps, such as WPF, Windows Forms, or Blazor WebAssembly.</span></span> <span data-ttu-id="bc5c8-220">主要區別是,在幾個 UI 框架中,應用使用的記憶體屬於用戶端,並且只影響該單個用戶端。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-220">The main difference is that in several of the UI frameworks the memory consumed by the app belongs to the client and only affects that individual client.</span></span> <span data-ttu-id="bc5c8-221">例如,Blazor WebAssembly 應用完全在用戶端上運行,並且僅使用用戶端記憶體資源。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-221">For example, a Blazor WebAssembly app runs entirely on the client and only uses client memory resources.</span></span> <span data-ttu-id="bc5c8-222">在 Blazor Server 方案中,應用使用的記憶體屬於伺服器,並在伺服器實例上的客戶端之間共用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-222">In the Blazor Server scenario, the memory consumed by the app belongs to the server and is shared among clients on the server instance.</span></span>

<span data-ttu-id="bc5c8-223">伺服器端記憶體需求是所有 Blazor Server 應用的考慮因素。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-223">Server-side memory demands are a consideration for all Blazor Server apps.</span></span> <span data-ttu-id="bc5c8-224">但是,大多數 Web 應用都是無狀態的,在返回回應時,在處理請求時使用的記憶體將被釋放。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-224">However, most web apps are stateless, and the memory used while processing a request is released when the response is returned.</span></span> <span data-ttu-id="bc5c8-225">作為一般建議,不允許用戶端分配未綁定的記憶體量,就像保留用戶端連接的任何其他伺服器端應用一樣。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-225">As a general recommendation, don't permit clients to allocate an unbound amount of memory as in any other server-side app that persists client connections.</span></span> <span data-ttu-id="bc5c8-226">Blazor Server 應用使用的記憶體保留的時間比單個請求長。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-226">The memory consumed by a Blazor Server app persists for a longer time than a single request.</span></span>

> [!NOTE]
> <span data-ttu-id="bc5c8-227">在開發過程中,可以使用探查器或捕獲跟蹤來評估客戶端的記憶體需求。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-227">During development, a profiler can be used or a trace captured to assess memory demands of clients.</span></span> <span data-ttu-id="bc5c8-228">探查器或跟蹤不會捕獲分配給特定用戶端的記憶體。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-228">A profiler or trace won't capture the memory allocated to a specific client.</span></span> <span data-ttu-id="bc5c8-229">要捕獲開發過程中特定用戶端的記憶體使用方式,捕獲轉儲並檢查根植於用戶電路中的所有物件的記憶體需求。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-229">To capture the memory use of a specific client during development, capture a dump and examine the memory demand of all the objects rooted at a user's circuit.</span></span>

### <a name="client-connections"></a><span data-ttu-id="bc5c8-230">用戶端連接</span><span class="sxs-lookup"><span data-stu-id="bc5c8-230">Client connections</span></span>

<span data-ttu-id="bc5c8-231">當一個或多個用戶端打開與伺服器的併發連接過多,從而阻止其他用戶端建立新連接時,可能會導致連接耗盡。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-231">Connection exhaustion can occur when one or more clients open too many concurrent connections to the server, preventing other clients from establishing new connections.</span></span>

<span data-ttu-id="bc5c8-232">Blazor 用戶端為每個工作階段建立單個連接,並在瀏覽器窗口打開時保持連接打開狀態。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-232">Blazor clients establish a single connection per session and keep the connection open for as long as the browser window is open.</span></span> <span data-ttu-id="bc5c8-233">對伺服器維護所有連接的要求並不特定於 Blazor 應用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-233">The demands on the server of maintaining all of the connections isn't specific to Blazor apps.</span></span> <span data-ttu-id="bc5c8-234">鑒於連接的持久性和 Blazor Server 應用的狀態性,連接耗盡對應用的可用性風險更大。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-234">Given the persistent nature of the connections and the stateful nature of Blazor Server apps, connection exhaustion is a greater risk to availability of the app.</span></span>

<span data-ttu-id="bc5c8-235">默認情況下,Blazor Server 應用的每位使用者的連接數沒有限制。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-235">By default, there's no limit on the number of connections per user for a Blazor Server app.</span></span> <span data-ttu-id="bc5c8-236">如果應用需要連接限制,請採用以下一種或多種方法:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-236">If the app requires a connection limit, take one or more of the following approaches:</span></span>

* <span data-ttu-id="bc5c8-237">需要身份驗證,這自然會限制未經授權的使用者連接到應用的能力。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-237">Require authentication, which naturally limits the ability of unauthorized users to connect to the app.</span></span> <span data-ttu-id="bc5c8-238">要使此方案有效,必須防止使用者以任何意願預配新使用者。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-238">For this scenario to be effective, users must be prevented from provisioning new users at will.</span></span>
* <span data-ttu-id="bc5c8-239">限制每個使用者的連接數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-239">Limit the number of connections per user.</span></span> <span data-ttu-id="bc5c8-240">限制連接可以通過以下方法完成。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-240">Limiting connections can be accomplished via the following approaches.</span></span> <span data-ttu-id="bc5c8-241">練習謹慎以允許合法使用者存取應用(例如,噹基於用戶端的IP位址建立連接限制時)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-241">Exercise care to allow legitimate users to access the app (for example, when a connection limit is established based on the client's IP address).</span></span>
  * <span data-ttu-id="bc5c8-242">在應用程式等級:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-242">At the app level:</span></span>
    * <span data-ttu-id="bc5c8-243">端點路由可擴充性。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-243">Endpoint routing extensibility.</span></span>
    * <span data-ttu-id="bc5c8-244">需要身份驗證才能連接到應用並追蹤每個使用者的活動作業階段。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-244">Require authentication to connect to the app and keep track of the active sessions per user.</span></span>
    * <span data-ttu-id="bc5c8-245">達到限制時拒絕新會話。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-245">Reject new sessions upon reaching a limit.</span></span>
    * <span data-ttu-id="bc5c8-246">使用代理(如[Azure SignalR 服務](/azure/azure-signalr/signalr-overview))與應用的連接,該服務將連接從用戶端到應用進行多路複用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-246">Proxy WebSocket connections to an app through the use of a proxy, such as the [Azure SignalR Service](/azure/azure-signalr/signalr-overview) that multiplexes connections from clients to an app.</span></span> <span data-ttu-id="bc5c8-247">這為具有比單個用戶端可以建立的連接容量更大的應用提供了,從而防止用戶端耗盡與伺服器的連接。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-247">This provides an app with greater connection capacity than a single client can establish, preventing a client from exhausting the connections to the server.</span></span>
  * <span data-ttu-id="bc5c8-248">在伺服器級別:在應用前面使用代理/閘道。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-248">At the server level: Use a proxy/gateway in front of the app.</span></span> <span data-ttu-id="bc5c8-249">例如[,Azure 前門](/azure/frontdoor/front-door-overview)使您能夠定義、管理和監視 Web 流量到應用的全域路由。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-249">For example, [Azure Front Door](/azure/frontdoor/front-door-overview) enables you to define, manage, and monitor the global routing of web traffic to an app.</span></span>

## <a name="denial-of-service-dos-attacks"></a><span data-ttu-id="bc5c8-250">拒絕服務 (DoS) 攻擊</span><span class="sxs-lookup"><span data-stu-id="bc5c8-250">Denial of service (DoS) attacks</span></span>

<span data-ttu-id="bc5c8-251">拒絕服務 (DoS) 攻擊涉及用戶端,導致伺服器耗盡其一個或多個資源,使應用不可用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-251">Denial of service (DoS) attacks involve a client causing the server to exhaust one or more of its resources making the app unavailable.</span></span> <span data-ttu-id="bc5c8-252">Blazor Server 應用包含一些預設限制,並依賴於其他ASP.NET核心和訊號R限制來抵禦 DoS 攻擊:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-252">Blazor Server apps include some default limits and rely on other ASP.NET Core and SignalR limits to protect against DoS attacks:</span></span>

| <span data-ttu-id="bc5c8-253">布拉佐伺服器應用程式限制</span><span class="sxs-lookup"><span data-stu-id="bc5c8-253">Blazor Server app limit</span></span>                            | <span data-ttu-id="bc5c8-254">描述</span><span class="sxs-lookup"><span data-stu-id="bc5c8-254">Description</span></span> | <span data-ttu-id="bc5c8-255">預設</span><span class="sxs-lookup"><span data-stu-id="bc5c8-255">Default</span></span> |
| ------------------------------------------------------- | ----------- | ------- |
| `CircuitOptions.DisconnectedCircuitMaxRetained`         | <span data-ttu-id="bc5c8-256">給定伺服器一次在記憶體中保存的最大斷開連接電路數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-256">Maximum number of disconnected circuits that a given server holds in memory at a time.</span></span> | <span data-ttu-id="bc5c8-257">100</span><span class="sxs-lookup"><span data-stu-id="bc5c8-257">100</span></span> |
| `CircuitOptions.DisconnectedCircuitRetentionPeriod`     | <span data-ttu-id="bc5c8-258">斷開電路在斷開之前在記憶體中保持最大時間。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-258">Maximum amount of time a disconnected circuit is held in memory before being torn down.</span></span> | <span data-ttu-id="bc5c8-259">3 分鐘</span><span class="sxs-lookup"><span data-stu-id="bc5c8-259">3 minutes</span></span> |
| `CircuitOptions.JSInteropDefaultCallTimeout`            | <span data-ttu-id="bc5c8-260">伺服器在超時異步 JavaScript 函數調用之前等待的時間最長。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-260">Maximum amount of time the server waits before timing out an asynchronous JavaScript function invocation.</span></span> | <span data-ttu-id="bc5c8-261">1 分鐘</span><span class="sxs-lookup"><span data-stu-id="bc5c8-261">1 minute</span></span> |
| `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches` | <span data-ttu-id="bc5c8-262">伺服器在給定時間每個電路的記憶體中保留的最大未確認呈現批處理數,以支援可靠的重新連接。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-262">Maximum number of unacknowledged render batches the server keeps in memory per circuit at a given time to support robust reconnection.</span></span> <span data-ttu-id="bc5c8-263">達到限制后,伺服器將停止生成新的呈現批處理,直到用戶端確認一個或多個批處理。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-263">After reaching the limit, the server stops producing new render batches until one or more batches have been acknowledged by the client.</span></span> | <span data-ttu-id="bc5c8-264">10</span><span class="sxs-lookup"><span data-stu-id="bc5c8-264">10</span></span> |


| <span data-ttu-id="bc5c8-265">訊號R 與 ASP.NET核心限制</span><span class="sxs-lookup"><span data-stu-id="bc5c8-265">SignalR and ASP.NET Core limit</span></span>             | <span data-ttu-id="bc5c8-266">描述</span><span class="sxs-lookup"><span data-stu-id="bc5c8-266">Description</span></span> | <span data-ttu-id="bc5c8-267">預設</span><span class="sxs-lookup"><span data-stu-id="bc5c8-267">Default</span></span> |
| ------------------------------------------ | ----------- | ------- |
| `CircuitOptions.MaximumReceiveMessageSize` | <span data-ttu-id="bc5c8-268">單個郵件的消息大小。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-268">Message size for an individual message.</span></span> | <span data-ttu-id="bc5c8-269">32 KB</span><span class="sxs-lookup"><span data-stu-id="bc5c8-269">32 KB</span></span> |

## <a name="interactions-with-the-browser-client"></a><span data-ttu-id="bc5c8-270">與瀏覽器(用戶端)的互動</span><span class="sxs-lookup"><span data-stu-id="bc5c8-270">Interactions with the browser (client)</span></span>

<span data-ttu-id="bc5c8-271">用戶端透過 JS 互通事件調度和呈現完成與伺服器互動。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-271">A client interacts with the server through JS interop event dispatching and render completion.</span></span> <span data-ttu-id="bc5c8-272">JS 互通通訊在 JavaScript 和 .NET 之間雙向進行:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-272">JS interop communication goes both ways between JavaScript and .NET:</span></span>

* <span data-ttu-id="bc5c8-273">瀏覽器事件以非同步方式從用戶端發送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-273">Browser events are dispatched from the client to the server in an asynchronous fashion.</span></span>
* <span data-ttu-id="bc5c8-274">伺服器根據需要異步重新呈現 UI。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-274">The server responds asynchronously rerendering the UI as necessary.</span></span>

### <a name="javascript-functions-invoked-from-net"></a><span data-ttu-id="bc5c8-275">從 .NET 呼叫的 JavaScript 函式</span><span class="sxs-lookup"><span data-stu-id="bc5c8-275">JavaScript functions invoked from .NET</span></span>

<span data-ttu-id="bc5c8-276">對於從 .NET 方法到 JAVAScript 的呼叫:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-276">For calls from .NET methods to JavaScript:</span></span>

* <span data-ttu-id="bc5c8-277">所有調用都有可配置的超時,之後它們將返回<xref:System.OperationCanceledException>給調用方。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-277">All invocations have a configurable timeout after which they fail, returning a <xref:System.OperationCanceledException> to the caller.</span></span>
  * <span data-ttu-id="bc5c8-278">呼叫的預設超時`CircuitOptions.JSInteropDefaultCallTimeout`( ) 為一分鐘。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-278">There's a default timeout for the calls (`CircuitOptions.JSInteropDefaultCallTimeout`) of one minute.</span></span> <span data-ttu-id="bc5c8-279">要設定此限制,請參閱<xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-279">To configure this limit, see <xref:blazor/call-javascript-from-dotnet#harden-js-interop-calls>.</span></span>
  * <span data-ttu-id="bc5c8-280">可以提供取消權杖以按每次呼叫控制取消。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-280">A cancellation token can be provided to control the cancellation on a per-call basis.</span></span> <span data-ttu-id="bc5c8-281">如果提供了取消權杖,則盡可能依賴預設調用超時,並規定對用戶端的任何調用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-281">Rely on the default call timeout where possible and time-bound any call to the client if a cancellation token is provided.</span></span>
* <span data-ttu-id="bc5c8-282">不能信任 JAVAScript 調用的結果。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-282">The result of a JavaScript call can't be trusted.</span></span> <span data-ttu-id="bc5c8-283">在Blazor瀏覽器中運行的應用用戶端搜索要調用的 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-283">The Blazor app client running in the browser searches for the JavaScript function to invoke.</span></span> <span data-ttu-id="bc5c8-284">調用該函數,並生成結果或錯誤。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-284">The function is invoked, and either the result or an error is produced.</span></span> <span data-ttu-id="bc5c8-285">惡意客戶端可以嘗試:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-285">A malicious client can attempt to:</span></span>
  * <span data-ttu-id="bc5c8-286">通過從 JavaScript 函數傳回錯誤,在應用中導致問題。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-286">Cause an issue in the app by returning an error from the JavaScript function.</span></span>
  * <span data-ttu-id="bc5c8-287">通過從 JAVAScript 函數返回意外結果,在伺服器上引發意外行為。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-287">Induce an unintended behavior on the server by returning an unexpected result from the JavaScript function.</span></span>

<span data-ttu-id="bc5c8-288">採取以下預防措施,防止上述情況:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-288">Take the following precautions to guard against the preceding scenarios:</span></span>

* <span data-ttu-id="bc5c8-289">在[try-catch](/dotnet/csharp/language-reference/keywords/try-catch)語句中包裝 JS 互通調用,以考慮呼叫期間可能發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-289">Wrap JS interop calls within [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) statements to account for errors that might occur during the invocations.</span></span> <span data-ttu-id="bc5c8-290">如需詳細資訊，請參閱 <xref:blazor/handle-errors#javascript-interop>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-290">For more information, see <xref:blazor/handle-errors#javascript-interop>.</span></span>
* <span data-ttu-id="bc5c8-291">在採取任何操作之前,驗證從 JS 互通調用傳回的數據,包括錯誤消息。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-291">Validate data returned from JS interop invocations, including error messages, before taking any action.</span></span>

### <a name="net-methods-invoked-from-the-browser"></a><span data-ttu-id="bc5c8-292">從瀏覽器呼叫 .NET 方法</span><span class="sxs-lookup"><span data-stu-id="bc5c8-292">.NET methods invoked from the browser</span></span>

<span data-ttu-id="bc5c8-293">不要信任從 JAVAScript 到 .NET 方法的調用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-293">Don't trust calls from JavaScript to .NET methods.</span></span> <span data-ttu-id="bc5c8-294">當 .NET 方法向 JAVAScript 公開時,請考慮如何調用 .NET 方法:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-294">When a .NET method is exposed to JavaScript, consider how the .NET method is invoked:</span></span>

* <span data-ttu-id="bc5c8-295">對待任何向 JAVAScript 公開的任何 .NET 方法,就像將應用的公共終結點一樣。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-295">Treat any .NET method exposed to JavaScript as you would a public endpoint to the app.</span></span>
  * <span data-ttu-id="bc5c8-296">驗證輸入。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-296">Validate input.</span></span>
    * <span data-ttu-id="bc5c8-297">確保值在預期範圍內。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-297">Ensure that values are within expected ranges.</span></span>
    * <span data-ttu-id="bc5c8-298">確保使用者具有執行請求的操作的許可權。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-298">Ensure that the user has permission to perform the action requested.</span></span>
  * <span data-ttu-id="bc5c8-299">不要將過多的資源分配為 .NET 方法調用的一部分。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-299">Don't allocate an excessive quantity of resources as part of the .NET method invocation.</span></span> <span data-ttu-id="bc5c8-300">例如,執行檢查並限制 CPU 和記憶體使用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-300">For example, perform checks and place limits on CPU and memory use.</span></span>
  * <span data-ttu-id="bc5c8-301">考慮到靜態和實例方法可以公開給 JAVAScript 用戶端。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-301">Take into account that static and instance methods can be exposed to JavaScript clients.</span></span> <span data-ttu-id="bc5c8-302">除非設計要求共用具有適當約束的狀態,否則應避免跨會話共享狀態。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-302">Avoid sharing state across sessions unless the design calls for sharing state with appropriate constraints.</span></span>
    * <span data-ttu-id="bc5c8-303">對於最初通過`DotNetReference`依賴項注入 (DI) 創建的物件公開的方法,這些物件應註冊為作用域物件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-303">For instance methods exposed through `DotNetReference` objects that are originally created through dependency injection (DI), the objects should be registered as scoped objects.</span></span> <span data-ttu-id="bc5c8-304">這適用於Blazor伺服器應用使用的任何 DI 服務。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-304">This applies to any DI service that the Blazor Server app uses.</span></span>
    * <span data-ttu-id="bc5c8-305">對於靜態方法,避免建立無法限定到用戶端的狀態,除非應用在伺服器實例上的所有用戶之間顯式共享狀態。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-305">For static methods, avoid establishing state that can't be scoped to the client unless the app is explicitly sharing state by-design across all users on a server instance.</span></span>
  * <span data-ttu-id="bc5c8-306">避免將參數中使用者提供的數據傳遞給 JAVAScript 調用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-306">Avoid passing user-supplied data in parameters to JavaScript calls.</span></span> <span data-ttu-id="bc5c8-307">如果絕對需要傳入參數中的數據,請確保 JavaScript 代碼處理傳遞數據,而不會引入[跨網站腳本 (XSS)](#cross-site-scripting-xss)漏洞。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-307">If passing data in parameters is absolutely required, ensure that the JavaScript code handles passing the data without introducing [Cross-site scripting (XSS)](#cross-site-scripting-xss) vulnerabilities.</span></span> <span data-ttu-id="bc5c8-308">例如,不要通過設置元素`innerHTML`的屬性將使用者提供的數據寫入文檔物件模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-308">For example, don't write user-supplied data to the Document Object Model (DOM) by setting the `innerHTML` property of an element.</span></span> <span data-ttu-id="bc5c8-309">請考慮使用[內容安全原則 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)`eval`來禁用和其他不安全的 JavaScript 基元。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-309">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to disable `eval` and other unsafe JavaScript primitives.</span></span>
* <span data-ttu-id="bc5c8-310">避免在框架的調度實現之上實現自定義的 .NET 調用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-310">Avoid implementing custom dispatching of .NET invocations on top of the framework's dispatching implementation.</span></span> <span data-ttu-id="bc5c8-311">向瀏覽器公開 .NET 方法是一種高級方案,不Blazor建議用於一般開發。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-311">Exposing .NET methods to the browser is an advanced scenario, not recommended for general Blazor development.</span></span>

### <a name="events"></a><span data-ttu-id="bc5c8-312">事件</span><span class="sxs-lookup"><span data-stu-id="bc5c8-312">Events</span></span>

<span data-ttu-id="bc5c8-313">事件為Blazor伺服器應用提供入口點。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-313">Events provide an entry point to a Blazor Server app.</span></span> <span data-ttu-id="bc5c8-314">保護 Web 應用中終結點的相同規則適用於伺服器Blazor應用中 的事件處理。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-314">The same rules for safeguarding endpoints in web apps apply to event handling in Blazor Server apps.</span></span> <span data-ttu-id="bc5c8-315">惡意用戶端可以發送它希望作為事件的有效負載發送的任何數據。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-315">A malicious client can send any data it wishes to send as the payload for an event.</span></span>

<span data-ttu-id="bc5c8-316">例如：</span><span class="sxs-lookup"><span data-stu-id="bc5c8-316">For example:</span></span>

* <span data-ttu-id="bc5c8-317">的更改事件`<select>`可以發送不在應用向客戶端顯示的選項中的值。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-317">A change event for a `<select>` could send a value that isn't within the options that the app presented to the client.</span></span>
* <span data-ttu-id="bc5c8-318">可以`<input>`繞過客戶端驗證向伺服器發送任何文本數據。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-318">An `<input>` could send any text data to the server, bypassing client-side validation.</span></span>

<span data-ttu-id="bc5c8-319">應用必須驗證應用處理的任何事件的數據。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-319">The app must validate the data for any event that the app handles.</span></span> <span data-ttu-id="bc5c8-320">框架Blazor[表單元件](xref:blazor/forms-validation)執行基本驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-320">The Blazor framework [forms components](xref:blazor/forms-validation) perform basic validations.</span></span> <span data-ttu-id="bc5c8-321">如果應用使用自定義表單元件,則必須編寫自訂代碼以根據需要驗證事件資料。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-321">If the app uses custom forms components, custom code must be written to validate event data as appropriate.</span></span>

Blazor<span data-ttu-id="bc5c8-322">伺服器事件是異步的,因此在應用有時間通過生成新的呈現來做出反應之前,可以將多個事件調度到伺服器。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-322"> Server events are asynchronous, so multiple events can be dispatched to the server before the app has time to react by producing a new render.</span></span> <span data-ttu-id="bc5c8-323">這需要考慮一些安全問題。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-323">This has some security implications to consider.</span></span> <span data-ttu-id="bc5c8-324">必須在事件處理程式內執行限制應用中的用戶端操作,而不是依賴於當前呈現的檢視狀態。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-324">Limiting client actions in the app must be performed inside event handlers and not depend on the current rendered view state.</span></span>

<span data-ttu-id="bc5c8-325">考慮一個計數器元件,該元件應允許使用者將計數器增量最多三次。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-325">Consider a counter component that should allow a user to increment a counter a maximum of three times.</span></span> <span data-ttu-id="bc5c8-326">增加計數器的按鈕 :`count`的值有條件的 :</span><span class="sxs-lookup"><span data-stu-id="bc5c8-326">The button to increment the counter is conditionally based on the value of `count`:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        count++;
    }
}
```

<span data-ttu-id="bc5c8-327">用戶端可以在框架生成此元件的新呈現之前調度一個或多個增量事件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-327">A client can dispatch one or more increment events before the framework produces a new render of this component.</span></span> <span data-ttu-id="bc5c8-328">結果是,用戶可以增量`count`*三次以上*,因為 UI 未足夠快地刪除該按鈕。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-328">The result is that the `count` can be incremented *over three times* by the user because the button isn't removed by the UI quickly enough.</span></span> <span data-ttu-id="bc5c8-329">實現三`count`個增量限制的正確方法如下示例所示:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-329">The correct way to achieve the limit of three `count` increments is shown in the following example:</span></span>

```razor
<p>Count: @count<p>

@if (count < 3)
{
    <button @onclick="IncrementCount" value="Increment count" />
}

@code 
{
    private int count = 0;

    private void IncrementCount()
    {
        if (count < 3)
        {
            count++;
        }
    }
}
```

<span data-ttu-id="bc5c8-330">通過在處理程式中`if (count < 3) { ... }`添加檢查,`count`增量 決定基於當前應用狀態。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-330">By adding the `if (count < 3) { ... }` check inside the handler, the decision to increment `count` is based on the current app state.</span></span> <span data-ttu-id="bc5c8-331">決策不像以前示例中那樣基於 UI 的狀態,後者可能暫時過時。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-331">The decision isn't based on the state of the UI as it was in the previous example, which might be temporarily stale.</span></span>

### <a name="guard-against-multiple-dispatches"></a><span data-ttu-id="bc5c8-332">防止多個排程</span><span class="sxs-lookup"><span data-stu-id="bc5c8-332">Guard against multiple dispatches</span></span>

<span data-ttu-id="bc5c8-333">如果事件回調非同步調用長時間運行的操作(例如從外部服務或資料庫提取資料),請考慮使用保護。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-333">If an event callback invokes a long running operation asynchronously, such as fetching data from an external service or database, consider using a guard.</span></span> <span data-ttu-id="bc5c8-334">在操作進行中,使用視覺反饋,保護可以防止用戶排隊執行多個操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-334">The guard can prevent the user from queueing up multiple operations while the operation is in progress with visual feedback.</span></span> <span data-ttu-id="bc5c8-335">從伺服器取得資料時`isLoading``GetForecastAsync`,`true`以下元件代碼將集到。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-335">The following component code sets `isLoading` to `true` while `GetForecastAsync` obtains data from the server.</span></span> <span data-ttu-id="bc5c8-336">當`isLoading``true`是 時,該按鈕在 UI 中禁用:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-336">While `isLoading` is `true`, the button is disabled in the UI:</span></span>

```razor
@page "/fetchdata"
@using BlazorServerSample.Data
@inject WeatherForecastService ForecastService

<button disabled="@isLoading" @onclick="UpdateForecasts">Update</button>

@code {
    private bool isLoading;
    private WeatherForecast[] forecasts;

    private async Task UpdateForecasts()
    {
        if (!isLoading)
        {
            isLoading = true;
            forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
            isLoading = false;
        }
    }
}
```

<span data-ttu-id="bc5c8-337">如果後台操作使用`async`-`await`模式非同步執行,則上述示例中演示的防護模式有效。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-337">The guard pattern demonstrated in the preceding example works if the background operation is executed asynchronously with the `async`-`await` pattern.</span></span>

### <a name="cancel-early-and-avoid-use-after-dispose"></a><span data-ttu-id="bc5c8-338">提前取消,避免處置后使用</span><span class="sxs-lookup"><span data-stu-id="bc5c8-338">Cancel early and avoid use-after-dispose</span></span>

<span data-ttu-id="bc5c8-339">除了使用[「防護」中所述的防護裝置(如針對多個調度部分](#guard-against-multiple-dispatches))外,<xref:System.Threading.CancellationToken>請考慮 在釋放元件時使用 取消長時間運行的操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-339">In addition to using a guard as described in the [Guard against multiple dispatches](#guard-against-multiple-dispatches) section, consider using a <xref:System.Threading.CancellationToken> to cancel long-running operations when the component is disposed.</span></span> <span data-ttu-id="bc5c8-340">此方法具有避免元件在*處置後使用*的額外好處:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-340">This approach has the added benefit of avoiding *use-after-dispose* in components:</span></span>

```razor
@implements IDisposable

...

@code {
    private readonly CancellationTokenSource TokenSource = 
        new CancellationTokenSource();

    private async Task UpdateForecasts()
    {
        ...

        forecasts = await ForecastService.GetForecastAsync(DateTime.Now, 
            TokenSource.Token);

        if (TokenSource.Token.IsCancellationRequested)
        {
           return;
        }

        ...
    }

    public void Dispose()
    {
        TokenSource.Cancel();
    }
}
```

### <a name="avoid-events-that-produce-large-amounts-of-data"></a><span data-ttu-id="bc5c8-341">避免產生大量資料的事件</span><span class="sxs-lookup"><span data-stu-id="bc5c8-341">Avoid events that produce large amounts of data</span></span>

<span data-ttu-id="bc5c8-342">某些 DOM`oninput`事件`onscroll`( 如或 )可以生成大量資料。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-342">Some DOM events, such as `oninput` or `onscroll`, can produce a large amount of data.</span></span> <span data-ttu-id="bc5c8-343">避免在Blazor伺服器應用中使用這些事件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-343">Avoid using these events in Blazor server apps.</span></span>

## <a name="additional-security-guidance"></a><span data-ttu-id="bc5c8-344">其他安全指南</span><span class="sxs-lookup"><span data-stu-id="bc5c8-344">Additional security guidance</span></span>

<span data-ttu-id="bc5c8-345">保護ASP.NET核心應用的指南適用於Blazor伺服器應用,並涵蓋以下各節:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-345">The guidance for securing ASP.NET Core apps apply to Blazor Server apps and are covered in the following sections:</span></span>

* [<span data-ttu-id="bc5c8-346">紀錄記錄與敏感資料</span><span class="sxs-lookup"><span data-stu-id="bc5c8-346">Logging and sensitive data</span></span>](#logging-and-sensitive-data)
* [<span data-ttu-id="bc5c8-347">使用 HTTPS 保護傳輸中的資訊</span><span class="sxs-lookup"><span data-stu-id="bc5c8-347">Protect information in transit with HTTPS</span></span>](#protect-information-in-transit-with-https)
* <span data-ttu-id="bc5c8-348">[跨網站文稿 (XSS)](#cross-site-scripting-xss))</span><span class="sxs-lookup"><span data-stu-id="bc5c8-348">[Cross-site scripting (XSS)](#cross-site-scripting-xss))</span></span>
* [<span data-ttu-id="bc5c8-349">跨源保護</span><span class="sxs-lookup"><span data-stu-id="bc5c8-349">Cross-origin protection</span></span>](#cross-origin-protection)
* [<span data-ttu-id="bc5c8-350">按一下劫持</span><span class="sxs-lookup"><span data-stu-id="bc5c8-350">Click-jacking</span></span>](#click-jacking)
* [<span data-ttu-id="bc5c8-351">開啟重定</span><span class="sxs-lookup"><span data-stu-id="bc5c8-351">Open redirects</span></span>](#open-redirects)

### <a name="logging-and-sensitive-data"></a><span data-ttu-id="bc5c8-352">紀錄記錄與敏感資料</span><span class="sxs-lookup"><span data-stu-id="bc5c8-352">Logging and sensitive data</span></span>

<span data-ttu-id="bc5c8-353">用戶端和伺服器之間的 JS 交互記錄在伺服器的日誌中,並與實例<xref:Microsoft.Extensions.Logging.ILogger>一起 記錄。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-353">JS interop interactions between the client and server are recorded in the server's logs with <xref:Microsoft.Extensions.Logging.ILogger> instances.</span></span> Blazor<span data-ttu-id="bc5c8-354">避免記錄敏感資訊,如實際事件或 JS 互通輸入和輸出。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-354"> avoids logging sensitive information, such as actual events or JS interop inputs and outputs.</span></span>

<span data-ttu-id="bc5c8-355">當伺服器上發生錯誤時,框架會通知用戶端並撕下會話。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-355">When an error occurs on the server, the framework notifies the client and tears down the session.</span></span> <span data-ttu-id="bc5c8-356">默認情況下,用戶端會收到一條通用錯誤消息,可在瀏覽器的開發人員工具中看到。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-356">By default, the client receives a generic error message that can be seen in the browser's developer tools.</span></span>

<span data-ttu-id="bc5c8-357">用戶端錯誤不包括調用堆疊,並且不提供有關錯誤原因的詳細資訊,但伺服器日誌確實包含此類資訊。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-357">The client-side error doesn't include the callstack and doesn't provide detail on the cause of the error, but server logs do contain such information.</span></span> <span data-ttu-id="bc5c8-358">出於開發目的,可以通過啟用詳細的錯誤來向用戶端提供敏感的錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-358">For development purposes, sensitive error information can be made available to the client by enabling detailed errors.</span></span>

<span data-ttu-id="bc5c8-359">通過:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-359">Enable detailed errors with:</span></span>

* <span data-ttu-id="bc5c8-360">`CircuitOptions.DetailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="bc5c8-360">`CircuitOptions.DetailedErrors`.</span></span>
* <span data-ttu-id="bc5c8-361">`DetailedErrors`配置金鑰。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-361">`DetailedErrors` configuration key.</span></span> <span data-ttu-id="bc5c8-362">例如,將`ASPNETCORE_DETAILEDERRORS`環境變數設置為`true`的值。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-362">For example, set the `ASPNETCORE_DETAILEDERRORS` environment variable to a value of `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="bc5c8-363">向 Internet 上的用戶端公開錯誤資訊是應始終避免的安全風險。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-363">Exposing error information to clients on the Internet is a security risk that should always be avoided.</span></span>

### <a name="protect-information-in-transit-with-https"></a><span data-ttu-id="bc5c8-364">使用 HTTPS 保護傳輸中的資訊</span><span class="sxs-lookup"><span data-stu-id="bc5c8-364">Protect information in transit with HTTPS</span></span>

Blazor<span data-ttu-id="bc5c8-365">伺服器用於SignalR用戶端和伺服器之間的通信。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-365"> Server uses SignalR for communication between the client and the server.</span></span> Blazor<span data-ttu-id="bc5c8-366">伺服器通常使用協商的SignalR傳輸,通常是 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-366"> Server normally uses the transport that SignalR negotiates, which is typically WebSockets.</span></span>

Blazor<span data-ttu-id="bc5c8-367">伺服器不能確保伺服器和客戶端之間發送的數據的完整性和機密性。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-367"> Server doesn't ensure the integrity and confidentiality of the data sent between the server and the client.</span></span> <span data-ttu-id="bc5c8-368">始終使用 HHH。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-368">Always use HTTPS.</span></span>

### <a name="cross-site-scripting-xss"></a><span data-ttu-id="bc5c8-369">跨網站文稿 (XSS)</span><span class="sxs-lookup"><span data-stu-id="bc5c8-369">Cross-site scripting (XSS)</span></span>

<span data-ttu-id="bc5c8-370">跨網站文本 (XSS) 允許未經授權的一方在瀏覽器上下文中執行任意邏輯。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-370">Cross-site scripting (XSS) allows an unauthorized party to execute arbitrary logic in the context of the browser.</span></span> <span data-ttu-id="bc5c8-371">受攻擊的應用可能在用戶端上運行任意代碼。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-371">A compromised app could potentially run arbitrary code on the client.</span></span> <span data-ttu-id="bc5c8-372">該漏洞可用於對伺服器執行許多惡意操作:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-372">The vulnerability could be used to potentially perform a number of malicious actions against the server:</span></span>

* <span data-ttu-id="bc5c8-373">向伺服器發送虛假/無效事件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-373">Dispatch fake/invalid events to the server.</span></span>
* <span data-ttu-id="bc5c8-374">派單失敗/無效呈現完成。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-374">Dispatch fail/invalid render completions.</span></span>
* <span data-ttu-id="bc5c8-375">避免調度渲染完成。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-375">Avoid dispatching render completions.</span></span>
* <span data-ttu-id="bc5c8-376">將跨攻呼叫從 JavaScript 調度到 .NET。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-376">Dispatch interop calls from JavaScript to .NET.</span></span>
* <span data-ttu-id="bc5c8-377">修改從 .NET 到 JAvaScript 的互通調用的回應。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-377">Modify the response of interop calls from .NET to JavaScript.</span></span>
* <span data-ttu-id="bc5c8-378">避免將 .NET 調度到 JS 互通結果。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-378">Avoid dispatching .NET to JS interop results.</span></span>

<span data-ttu-id="bc5c8-379">Blazor伺服器框架採取措施防止上述威脅:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-379">The Blazor Server framework takes steps to protect against some of the preceding threats:</span></span>

* <span data-ttu-id="bc5c8-380">如果用戶端未確認呈現批處理,則停止生成新的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-380">Stops producing new UI updates if the client isn't acknowledging render batches.</span></span> <span data-ttu-id="bc5c8-381">設定了`CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-381">Configured with `CircuitOptions.MaxBufferedUnacknowledgedRenderBatches`.</span></span>
* <span data-ttu-id="bc5c8-382">在一分鐘後超時任何 .NET 到 JavaScript 調用,而不會收到用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-382">Times out any .NET to JavaScript call after one minute without receiving a response from the client.</span></span> <span data-ttu-id="bc5c8-383">設定了`CircuitOptions.JSInteropDefaultCallTimeout`。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-383">Configured with `CircuitOptions.JSInteropDefaultCallTimeout`.</span></span>
* <span data-ttu-id="bc5c8-384">在 JS 互通期間對來自瀏覽器的所有輸入執行基本驗證:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-384">Performs basic validation on all input coming from the browser during JS interop:</span></span>
  * <span data-ttu-id="bc5c8-385">.NET 引用有效,且為 .NET 方法預期的類型。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-385">.NET references are valid and of the type expected by the .NET method.</span></span>
  * <span data-ttu-id="bc5c8-386">數據沒有格式錯誤。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-386">The data isn't malformed.</span></span>
  * <span data-ttu-id="bc5c8-387">有效負載中存在該方法的正確參數數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-387">The correct number of arguments for the method are present in the payload.</span></span>
  * <span data-ttu-id="bc5c8-388">在調用 方法之前,可以正確反序列化參數或結果。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-388">The arguments or result can be deserialized correctly before invoking the method.</span></span>
* <span data-ttu-id="bc5c8-389">在來自來自來自調度事件的所有輸入中執行基本驗證:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-389">Performs basic validation in all input coming from the browser from dispatched events:</span></span>
  * <span data-ttu-id="bc5c8-390">事件具有有效類型。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-390">The event has a valid type.</span></span>
  * <span data-ttu-id="bc5c8-391">可以對事件的數據進行反序列化。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-391">The data for the event can be deserialized.</span></span>
  * <span data-ttu-id="bc5c8-392">有一個事件處理程式與事件相關聯。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-392">There's an event handler associated with the event.</span></span>

<span data-ttu-id="bc5c8-393">除了框架實現的安全措施外,開發人員還必須對應用進行編碼,以防止威脅並採取適當操作:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-393">In addition to the safeguards that the framework implements, the app must be coded by the developer to safeguard against threats and take appropriate actions:</span></span>

* <span data-ttu-id="bc5c8-394">在處理事件時,始終驗證數據。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-394">Always validate data when handling events.</span></span>
* <span data-ttu-id="bc5c8-395">在接收不合法資料時採取適當的操作:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-395">Take appropriate action upon receiving invalid data:</span></span>
  * <span data-ttu-id="bc5c8-396">忽略數據並返回。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-396">Ignore the data and return.</span></span> <span data-ttu-id="bc5c8-397">這允許應用繼續處理請求。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-397">This allows the app to continue processing requests.</span></span>
  * <span data-ttu-id="bc5c8-398">如果應用確定輸入是非法的,並且無法由合法用戶端生成,則引發異常。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-398">If the app determines that the input is illegitimate and couldn't be produced by legitimate client, throw an exception.</span></span> <span data-ttu-id="bc5c8-399">引發異常會撕裂電路並結束會話。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-399">Throwing an exception tears down the circuit and ends the session.</span></span>
* <span data-ttu-id="bc5c8-400">不要相信日誌中包含的呈現批處理完成提供的錯誤消息。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-400">Don't trust the error message provided by render batch completions included in the logs.</span></span> <span data-ttu-id="bc5c8-401">該錯誤由用戶端提供,通常不能信任,因為用戶端可能會受到威脅。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-401">The error is provided by the client and can't generally be trusted, as the client might be compromised.</span></span>
* <span data-ttu-id="bc5c8-402">不要信任在 JavaScript 和 .NET 方法之間朝任一方向對 JS 互通調用的輸入。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-402">Don't trust the input on JS interop calls in either direction between JavaScript and .NET methods.</span></span>
* <span data-ttu-id="bc5c8-403">應用負責驗證參數和結果的內容是否有效,即使參數或結果已正確反序列化。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-403">The app is responsible for validating that the content of arguments and results are valid, even if the arguments or results are correctly deserialized.</span></span>

<span data-ttu-id="bc5c8-404">要存在 XSS 漏洞,應用必須在呈現的頁面中包含使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-404">For a XSS vulnerability to exist, the app must incorporate user input in the rendered page.</span></span> Blazor<span data-ttu-id="bc5c8-405">伺服器元件執行編譯時間步驟,其中 *.razor*檔中的標記轉換為過程 C# 邏輯。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-405"> Server components execute a compile-time step where the markup in a *.razor* file is transformed into procedural C# logic.</span></span> <span data-ttu-id="bc5c8-406">在執行時,C# 邏輯產生描述元素、文字與子元件的*呈現樹*。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-406">At runtime, the C# logic builds a *render tree* describing the elements, text, and child components.</span></span> <span data-ttu-id="bc5c8-407">這使用 JavaScript 指令應用於瀏覽器的 DOM(或預先使用清單的 HTML):</span><span class="sxs-lookup"><span data-stu-id="bc5c8-407">This is applied to the browser's DOM via a sequence of JavaScript instructions (or is serialized to HTML in the case of prerendering):</span></span>

* <span data-ttu-id="bc5c8-408">透過普通 Razor 語法(例如`@someStringValue`), 呈現的使用者輸入不會公開 XSS 漏洞,因為 Razor 語法通過只能寫入文本的命令添加到 DOM。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-408">User input rendered via normal Razor syntax (for example, `@someStringValue`) doesn't expose a XSS vulnerability because the Razor syntax is added to the DOM via commands that can only write text.</span></span> <span data-ttu-id="bc5c8-409">即使該值包含 HTML 標記,該值也會顯示為靜態文本。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-409">Even if the value includes HTML markup, the value is displayed as static text.</span></span> <span data-ttu-id="bc5c8-410">預顯時,輸出由 HTML 編碼,該輸出還會將內容顯示為靜態文本。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-410">When prerendering, the output is HTML-encoded, which also displays the content as static text.</span></span>
* <span data-ttu-id="bc5c8-411">不允許腳本標記,不應包含在應用的元件呈現樹中。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-411">Script tags aren't allowed and shouldn't be included in the app's component render tree.</span></span> <span data-ttu-id="bc5c8-412">如果元件的標記中包含腳本標記,則生成編譯時間錯誤。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-412">If a script tag is included in a component's markup, a compile-time error is generated.</span></span>
* <span data-ttu-id="bc5c8-413">元件作者無需使用 Razor 即可創作 C# 中的元件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-413">Component authors can author components in C# without using Razor.</span></span> <span data-ttu-id="bc5c8-414">元件作者負責在發出輸出時使用正確的 API。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-414">The component author is responsible for using the correct APIs when emitting output.</span></span> <span data-ttu-id="bc5c8-415">例如,使用`builder.AddContent(0, someUserSuppliedString)`*而不是*`builder.AddMarkupContent(0, someUserSuppliedString)`,因為後者可能會創建 XSS 漏洞。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-415">For example, use `builder.AddContent(0, someUserSuppliedString)` and *not* `builder.AddMarkupContent(0, someUserSuppliedString)`, as the latter could create a XSS vulnerability.</span></span>

<span data-ttu-id="bc5c8-416">作為防範 XSS 攻擊的一部分,請考慮實施 XSS 緩解措施,如[內容安全策略 (CSP)。](https://developer.mozilla.org/docs/Web/HTTP/CSP)</span><span class="sxs-lookup"><span data-stu-id="bc5c8-416">As part of protecting against XSS attacks, consider implementing XSS mitigations, such as [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP).</span></span>

<span data-ttu-id="bc5c8-417">如需詳細資訊，請參閱 <xref:security/cross-site-scripting>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-417">For more information, see <xref:security/cross-site-scripting>.</span></span>

### <a name="cross-origin-protection"></a><span data-ttu-id="bc5c8-418">跨源保護</span><span class="sxs-lookup"><span data-stu-id="bc5c8-418">Cross-origin protection</span></span>

<span data-ttu-id="bc5c8-419">跨源攻擊涉及來自不同來源的用戶端對伺服器執行操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-419">Cross-origin attacks involve a client from a different origin performing an action against the server.</span></span> <span data-ttu-id="bc5c8-420">惡意操作通常是 GET 請求或表單 POST(跨網站請求偽造、CSRF),但也可以打開惡意 WebSocket。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-420">The malicious action is typically a GET request or a form POST (Cross-Site Request Forgery, CSRF), but opening a malicious WebSocket is also possible.</span></span> Blazor<span data-ttu-id="bc5c8-421">伺服器應用提供[與使用中心協定的任何SignalR其他 應用相同的保證:](xref:signalr/security)</span><span class="sxs-lookup"><span data-stu-id="bc5c8-421"> Server apps offer [the same guarantees that any other SignalR app using the hub protocol offer](xref:signalr/security):</span></span>

* Blazor<span data-ttu-id="bc5c8-422">除非採取其他措施防止伺服器應用跨源訪問,否則可以跨源訪問伺服器應用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-422"> Server apps can be accessed cross-origin unless additional measures are taken to prevent it.</span></span> <span data-ttu-id="bc5c8-423">要禁用跨源訪問,請通過將 CORS 中間件添加到管道並將`DisableCorsAttribute`Blazor添加到 終結點元數據或透過[配置跨源SignalR資源分享 來](xref:signalr/security#cross-origin-resource-sharing)限制允許源集,從而禁用終結點中的 CORS。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-423">To disable cross-origin access, either disable CORS in the endpoint by adding the CORS middleware to the pipeline and adding the `DisableCorsAttribute` to the Blazor endpoint metadata or limit the set of allowed origins by [configuring SignalR for cross-origin resource sharing](xref:signalr/security#cross-origin-resource-sharing).</span></span>
* <span data-ttu-id="bc5c8-424">如果啟用了 CORS,則可能需要執行額外的步驟來保護應用,具體取決於 CORS 配置。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-424">If CORS is enabled, extra steps might be required to protect the app depending on the CORS configuration.</span></span> <span data-ttu-id="bc5c8-425">如果 CORS 已全域啟用,則可以通過在Blazor`DisableCorsAttribute``hub.MapBlazorHub()`調用 後將中繼資料添加到終結點元數據來禁用伺服器中心。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-425">If CORS is globally enabled, CORS can be disabled for the Blazor Server hub by adding the `DisableCorsAttribute` metadata to the endpoint metadata after calling `hub.MapBlazorHub()`.</span></span>

<span data-ttu-id="bc5c8-426">如需詳細資訊，請參閱 <xref:security/anti-request-forgery>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-426">For more information, see <xref:security/anti-request-forgery>.</span></span>

### <a name="click-jacking"></a><span data-ttu-id="bc5c8-427">按一下劫持</span><span class="sxs-lookup"><span data-stu-id="bc5c8-427">Click-jacking</span></span>

<span data-ttu-id="bc5c8-428">單擊劫持涉及將網站從不同來源呈現為`<iframe>`網站內部,以誘使用戶在受到攻擊的網站上執行操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-428">Click-jacking involves rendering a site as an `<iframe>` inside a site from a different origin in order to trick the user into performing actions on the site under attack.</span></span>

<span data-ttu-id="bc5c8-429">要保護應用不在`<iframe>`中呈現,請使用[內容安全策略 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)和`X-Frame-Options`標頭。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-429">To protect an app from rendering inside of an `<iframe>`, use [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) and the `X-Frame-Options` header.</span></span> <span data-ttu-id="bc5c8-430">有關詳細資訊,請參閱[MDN 網頁文件:X 幀選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-430">For more information, see [MDN web docs: X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options).</span></span>

### <a name="open-redirects"></a><span data-ttu-id="bc5c8-431">開啟重定</span><span class="sxs-lookup"><span data-stu-id="bc5c8-431">Open redirects</span></span>

<span data-ttu-id="bc5c8-432">當Blazor伺服器應用作業階段啟動時,伺服器將執行作為啟動作業階段的一部分發送的 URL 的基本驗證。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-432">When a Blazor Server app session starts, the server performs basic validation of the URLs sent as part of starting the session.</span></span> <span data-ttu-id="bc5c8-433">在建立電路之前,框架會檢查基本 URL 是否是當前 URL 的父 URL。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-433">The framework checks that the base URL is a parent of the current URL before establishing the circuit.</span></span> <span data-ttu-id="bc5c8-434">框架不執行其他檢查。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-434">No additional checks are performed by the framework.</span></span>

<span data-ttu-id="bc5c8-435">當使用者選擇用戶端上的連結時,連結的 URL 將發送到伺服器,從而確定要執行的操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-435">When a user selects a link on the client, the URL for the link is sent to the server, which determines what action to take.</span></span> <span data-ttu-id="bc5c8-436">例如,應用可以執行客戶端導航或指示瀏覽器轉到新位置。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-436">For example, the app may perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="bc5c8-437">元件還可以使用 以程式設計方式觸發瀏覽`NavigationManager`要求 。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-437">Components can also trigger navigation requests programatically through the use of `NavigationManager`.</span></span> <span data-ttu-id="bc5c8-438">在這種情況下,應用可能會執行客戶端導航或指示瀏覽器轉到新位置。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-438">In such scenarios, the app might perform a client-side navigation or indicate to the browser to go to the new location.</span></span>

<span data-ttu-id="bc5c8-439">元件必須:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-439">Components must:</span></span>

* <span data-ttu-id="bc5c8-440">避免將使用者輸入用作導航調用參數的一部分。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-440">Avoid using user input as part of the navigation call arguments.</span></span>
* <span data-ttu-id="bc5c8-441">驗證參數以確保應用允許目標。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-441">Validate arguments to ensure that the target is allowed by the app.</span></span>

<span data-ttu-id="bc5c8-442">否則,惡意用戶可以強制瀏覽器轉到攻擊者控制的網站。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-442">Otherwise, a malicious user can force the browser to go to an attacker-controlled site.</span></span> <span data-ttu-id="bc5c8-443">在這種情況下,攻擊者會誘使應用使用某些用戶輸入作為`NavigationManager.Navigate`方法調用的一部分。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-443">In this scenario, the attacker tricks the app into using some user input as part of the invocation of the `NavigationManager.Navigate` method.</span></span>

<span data-ttu-id="bc5c8-444">在將連結作為應用的一部分呈現時,此建議也適用於:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-444">This advice also applies when rendering links as part of the app:</span></span>

* <span data-ttu-id="bc5c8-445">如果可能,請使用相對連結。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-445">If possible, use relative links.</span></span>
* <span data-ttu-id="bc5c8-446">在將絕對連結目標包括在頁面中之前,驗證其絕對連結目標是否有效。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-446">Validate that absolute link destinations are valid before including them in a page.</span></span>

<span data-ttu-id="bc5c8-447">如需詳細資訊，請參閱 <xref:security/preventing-open-redirects>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-447">For more information, see <xref:security/preventing-open-redirects>.</span></span>

## <a name="authentication-and-authorization"></a><span data-ttu-id="bc5c8-448">驗證和授權</span><span class="sxs-lookup"><span data-stu-id="bc5c8-448">Authentication and authorization</span></span>

<span data-ttu-id="bc5c8-449">有關身份驗證和授權的指南,請參閱<xref:security/blazor/index>。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-449">For guidance on authentication and authorization, see <xref:security/blazor/index>.</span></span>

## <a name="security-checklist"></a><span data-ttu-id="bc5c8-450">安全性檢查清單</span><span class="sxs-lookup"><span data-stu-id="bc5c8-450">Security checklist</span></span>

<span data-ttu-id="bc5c8-451">以下安全注意事項清單並不全面:</span><span class="sxs-lookup"><span data-stu-id="bc5c8-451">The following list of security considerations isn't comprehensive:</span></span>

* <span data-ttu-id="bc5c8-452">驗證事件參數。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-452">Validate arguments from events.</span></span>
* <span data-ttu-id="bc5c8-453">驗證 JS 互通調用的輸入和結果。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-453">Validate inputs and results from JS interop calls.</span></span>
* <span data-ttu-id="bc5c8-454">避免使用 (或事先驗證) 用戶輸入 .NET 到 JS 互通調用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-454">Avoid using (or validate beforehand) user input for .NET to JS interop calls.</span></span>
* <span data-ttu-id="bc5c8-455">防止用戶端分配未綁定的內存量。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-455">Prevent the client from allocating an unbound amount of memory.</span></span>
  * <span data-ttu-id="bc5c8-456">元件中的數據。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-456">Data within the component.</span></span>
  * <span data-ttu-id="bc5c8-457">`DotNetObject`返回給用戶端的引用。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-457">`DotNetObject` references returned to the client.</span></span>
* <span data-ttu-id="bc5c8-458">防止多個調度。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-458">Guard against multiple dispatches.</span></span>
* <span data-ttu-id="bc5c8-459">釋放元件時取消長時間運行的操作。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-459">Cancel long-running operations when the component is disposed.</span></span>
* <span data-ttu-id="bc5c8-460">避免生成大量數據的事件。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-460">Avoid events that produce large amounts of data.</span></span>
* <span data-ttu-id="bc5c8-461">避免將使用者輸入用作調用`NavigationManager.Navigate`URL 的一部分,如果不可避免,請先根據一組允許的原點驗證 URL 的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-461">Avoid using user input as part of calls to `NavigationManager.Navigate` and validate user input for URLs against a set of allowed origins first if unavoidable.</span></span>
* <span data-ttu-id="bc5c8-462">不要根據 UI 的狀態做出授權決策,而只能根據元件狀態做出授權決策。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-462">Don't make authorization decisions based on the state of the UI but only from component state.</span></span>
* <span data-ttu-id="bc5c8-463">請考慮使用[內容安全原則 (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP)來抵禦 XSS 攻擊。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-463">Consider using [Content Security Policy (CSP)](https://developer.mozilla.org/docs/Web/HTTP/CSP) to protect against XSS attacks.</span></span>
* <span data-ttu-id="bc5c8-464">請考慮使用 CSP 和[X 幀選項](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options)來防止咔嗒聲劫持。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-464">Consider using CSP and [X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options) to protect against click-jacking.</span></span>
* <span data-ttu-id="bc5c8-465">在啟用 CORS 或顯式禁Blazor用應用的 CORS 時,確保 CORS 設定是合適的。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-465">Ensure CORS settings are appropriate when enabling CORS or explicitly disable CORS for Blazor apps.</span></span>
* <span data-ttu-id="bc5c8-466">測試以確保Blazor應用的伺服器端限制提供可接受的用戶體驗,而不會造成不可接受的風險級別。</span><span class="sxs-lookup"><span data-stu-id="bc5c8-466">Test to ensure that the server-side limits for the Blazor app provide an acceptable user experience without unacceptable levels of risk.</span></span>
