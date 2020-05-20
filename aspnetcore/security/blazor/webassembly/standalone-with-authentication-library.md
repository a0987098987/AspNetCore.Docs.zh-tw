---
<span data-ttu-id="21f09-101">標題： ' Blazor 使用驗證程式庫保護 ASP.NET Core WebAssembly 獨立應用程式的作者：描述： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="21f09-101">title: 'Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library' author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="21f09-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="21f09-102">'Blazor'</span></span>
- <span data-ttu-id="21f09-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="21f09-103">'Identity'</span></span>
- <span data-ttu-id="21f09-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="21f09-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="21f09-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="21f09-105">'Razor'</span></span>
- <span data-ttu-id="21f09-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="21f09-106">'SignalR' uid:</span></span> 

---
# <a name="secure-an-aspnet-core-blazor-webassembly-standalone-app-with-the-authentication-library"></a><span data-ttu-id="21f09-107">Blazor使用驗證程式庫保護 ASP.NET Core WebAssembly 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="21f09-107">Secure an ASP.NET Core Blazor WebAssembly standalone app with the Authentication library</span></span>

<span data-ttu-id="21f09-108">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="21f09-108">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="21f09-109">*若為 Azure Active Directory （AAD）和 Azure Active Directory B2C （AAD B2C），請不要遵循本主題中的指導方針。請參閱此目錄節點中的 AAD 和 AAD B2C 主題。*</span><span class="sxs-lookup"><span data-stu-id="21f09-109">*For Azure Active Directory (AAD) and Azure Active Directory B2C (AAD B2C), don't follow the guidance in this topic. See the AAD and AAD B2C topics in this table of contents node.*</span></span>

<span data-ttu-id="21f09-110">若要建立 Blazor 使用程式庫的 WebAssembly 獨立應用程式 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` ，請在命令 shell 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="21f09-110">To create a Blazor WebAssembly standalone app that uses `Microsoft.AspNetCore.Components.WebAssembly.Authentication` library, execute the following command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual
```

<span data-ttu-id="21f09-111">若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如）的 output 選項 `-o BlazorSample` 。</span><span class="sxs-lookup"><span data-stu-id="21f09-111">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="21f09-112">資料夾名稱也會成為專案名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="21f09-112">The folder name also becomes part of the project's name.</span></span>

<span data-ttu-id="21f09-113">在 Visual Studio 中，[建立 Blazor WebAssembly 應用程式](xref:blazor/get-started)。</span><span class="sxs-lookup"><span data-stu-id="21f09-113">In Visual Studio, [create a Blazor WebAssembly app](xref:blazor/get-started).</span></span> <span data-ttu-id="21f09-114">使用 [**儲存使用者帳戶應用程式內**] 選項，將**驗證**設定為**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="21f09-114">Set **Authentication** to **Individual User Accounts** with the **Store user accounts in-app** option.</span></span>

## <a name="authentication-package"></a><span data-ttu-id="21f09-115">驗證套件</span><span class="sxs-lookup"><span data-stu-id="21f09-115">Authentication package</span></span>

<span data-ttu-id="21f09-116">建立應用程式以使用個別使用者帳戶時，應用程式會 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 在應用程式的專案檔中自動接收套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="21f09-116">When an app is created to use Individual User Accounts, the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="21f09-117">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="21f09-117">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="21f09-118">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="21f09-118">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
  Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
  Version="3.2.0" />
```

## <a name="authentication-service-support"></a><span data-ttu-id="21f09-119">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="21f09-119">Authentication service support</span></span>

<span data-ttu-id="21f09-120">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 `AddOidcAuthentication` `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 。</span><span class="sxs-lookup"><span data-stu-id="21f09-120">Support for authenticating users is registered in the service container with the `AddOidcAuthentication` extension method provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="21f09-121">這個方法會設定應用程式與 Identity 提供者（IP）互動所需的服務。</span><span class="sxs-lookup"><span data-stu-id="21f09-121">This method sets up the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="21f09-122">*Program.cs*：</span><span class="sxs-lookup"><span data-stu-id="21f09-122">*Program.cs*:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    builder.Configuration.Bind("Local", options.ProviderOptions);
});
```

<span data-ttu-id="21f09-123">Configuration 是由*wwwroot/appsettings*檔案所提供：</span><span class="sxs-lookup"><span data-stu-id="21f09-123">Configuration is supplied by the *wwwroot/appsettings.json* file:</span></span>

```json
{
    "Local": {
        "Authority": "{AUTHORITY}",
        "ClientId": "{CLIENT ID}"
    }
}
```

<span data-ttu-id="21f09-124">獨立應用程式的驗證支援是使用 Open ID Connect （OIDC）提供。</span><span class="sxs-lookup"><span data-stu-id="21f09-124">Authentication support for standalone apps is offered using Open ID Connect (OIDC).</span></span> <span data-ttu-id="21f09-125">`AddOidcAuthentication`方法會接受回呼來設定使用 OIDC 驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="21f09-125">The `AddOidcAuthentication` method accepts a callback to configure the parameters required to authenticate an app using OIDC.</span></span> <span data-ttu-id="21f09-126">設定應用程式所需的值可從符合 OIDC 規範的 IP 取得。</span><span class="sxs-lookup"><span data-stu-id="21f09-126">The values required for configuring the app can be obtained from the OIDC-compliant IP.</span></span> <span data-ttu-id="21f09-127">當您註冊應用程式時，請取得這些值，這通常會發生在其線上入口網站中。</span><span class="sxs-lookup"><span data-stu-id="21f09-127">Obtain the values when you register the app, which typically occurs in their online portal.</span></span>

## <a name="access-token-scopes"></a><span data-ttu-id="21f09-128">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="21f09-128">Access token scopes</span></span>

<span data-ttu-id="21f09-129">BlazorWebAssembly 範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="21f09-129">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="21f09-130">若要在登入流程中布建存取權杖，請將範圍新增至的預設權杖範圍 `OidcProviderOptions` ：</span><span class="sxs-lookup"><span data-stu-id="21f09-130">To provision an access token as part of the sign-in flow, add the scope to the default token scopes of the `OidcProviderOptions`:</span></span>

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

<span data-ttu-id="21f09-131">如需詳細資訊，請參閱*其他案例*文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="21f09-131">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="21f09-132">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="21f09-132">Request additional access tokens</span></span>](xref:security/blazor/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="21f09-133">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="21f09-133">Attach tokens to outgoing requests</span></span>](xref:security/blazor/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

## <a name="imports-file"></a><span data-ttu-id="21f09-134">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="21f09-134">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="21f09-135">索引頁面</span><span class="sxs-lookup"><span data-stu-id="21f09-135">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

## <a name="app-component"></a><span data-ttu-id="21f09-136">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="21f09-136">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="21f09-137">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="21f09-137">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="21f09-138">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="21f09-138">LoginDisplay component</span></span>

<span data-ttu-id="21f09-139">`LoginDisplay`元件（*Shared/LoginDisplay*）會在 `MainLayout` 元件（*shared/MainLayout*）中轉譯，並管理下列行為：</span><span class="sxs-lookup"><span data-stu-id="21f09-139">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="21f09-140">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="21f09-140">For authenticated users:</span></span>
  * <span data-ttu-id="21f09-141">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="21f09-141">Displays the current username.</span></span>
  * <span data-ttu-id="21f09-142">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="21f09-142">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="21f09-143">若為匿名使用者，則提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="21f09-143">For anonymous users, offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

## <a name="authentication-component"></a><span data-ttu-id="21f09-144">驗證元件</span><span class="sxs-lookup"><span data-stu-id="21f09-144">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="21f09-145">其他資源</span><span class="sxs-lookup"><span data-stu-id="21f09-145">Additional resources</span></span>

* <xref:security/blazor/webassembly/additional-scenarios>
* [<span data-ttu-id="21f09-146">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="21f09-146">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:security/blazor/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
