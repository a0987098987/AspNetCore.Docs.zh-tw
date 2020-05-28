---
<span data-ttu-id="2a84c-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-102">'Blazor'</span></span>
- <span data-ttu-id="2a84c-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-103">'Identity'</span></span>
- <span data-ttu-id="2a84c-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-105">'Razor'</span></span>
- <span data-ttu-id="2a84c-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-106">'SignalR' uid:</span></span> 

---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a><span data-ttu-id="2a84c-107">在 ASP.NET Core 中保存外部提供者的其他宣告和權杖</span><span class="sxs-lookup"><span data-stu-id="2a84c-107">Persist additional claims and tokens from external providers in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2a84c-108">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="2a84c-108">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="2a84c-109">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="2a84c-109">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="2a84c-110">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2a84c-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a84c-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="2a84c-111">Prerequisites</span></span>

<span data-ttu-id="2a84c-112">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="2a84c-112">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="2a84c-113">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="2a84c-113">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="2a84c-114">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-114">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="2a84c-115">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="2a84c-115">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="2a84c-116">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="2a84c-116">Set the client ID and client secret</span></span>

<span data-ttu-id="2a84c-117">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="2a84c-117">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="2a84c-118">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="2a84c-118">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="2a84c-119">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="2a84c-119">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="2a84c-120">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="2a84c-120">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="2a84c-121">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-121">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="2a84c-122">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-122">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="2a84c-123">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-123">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="2a84c-124">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-124">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="2a84c-125">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="2a84c-125">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="2a84c-126">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="2a84c-126">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="2a84c-127">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="2a84c-127">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="2a84c-128">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="2a84c-128">Establish the authentication scope</span></span>

<span data-ttu-id="2a84c-129">藉由指定，指定要從提供者抓取的許可權清單 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-129">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="2a84c-130">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="2a84c-130">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="2a84c-131">提供者</span><span class="sxs-lookup"><span data-stu-id="2a84c-131">Provider</span></span>  | <span data-ttu-id="2a84c-132">影響範圍</span><span class="sxs-lookup"><span data-stu-id="2a84c-132">Scope</span></span>                                                            |
| ---
<span data-ttu-id="2a84c-133">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-133">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-134">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-134">'Blazor'</span></span>
- <span data-ttu-id="2a84c-135">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-135">'Identity'</span></span>
- <span data-ttu-id="2a84c-136">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-136">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-137">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-137">'Razor'</span></span>
- <span data-ttu-id="2a84c-138">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-138">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-139">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-139">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-140">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-140">'Blazor'</span></span>
- <span data-ttu-id="2a84c-141">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-141">'Identity'</span></span>
- <span data-ttu-id="2a84c-142">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-142">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-143">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-143">'Razor'</span></span>
- <span data-ttu-id="2a84c-144">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-144">'SignalR' uid:</span></span> 

<span data-ttu-id="2a84c-145">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-145">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-146">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-146">'Blazor'</span></span>
- <span data-ttu-id="2a84c-147">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-147">'Identity'</span></span>
- <span data-ttu-id="2a84c-148">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-148">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-149">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-149">'Razor'</span></span>
- <span data-ttu-id="2a84c-150">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-150">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-151">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-151">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-152">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-152">'Blazor'</span></span>
- <span data-ttu-id="2a84c-153">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-153">'Identity'</span></span>
- <span data-ttu-id="2a84c-154">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-154">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-155">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-155">'Razor'</span></span>
- <span data-ttu-id="2a84c-156">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-156">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-157">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-157">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-158">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-158">'Blazor'</span></span>
- <span data-ttu-id="2a84c-159">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-159">'Identity'</span></span>
- <span data-ttu-id="2a84c-160">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-160">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-161">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-161">'Razor'</span></span>
- <span data-ttu-id="2a84c-162">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-162">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-163">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-163">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-164">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-164">'Blazor'</span></span>
- <span data-ttu-id="2a84c-165">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-165">'Identity'</span></span>
- <span data-ttu-id="2a84c-166">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-166">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-167">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-167">'Razor'</span></span>
- <span data-ttu-id="2a84c-168">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-168">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-169">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-169">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-170">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-170">'Blazor'</span></span>
- <span data-ttu-id="2a84c-171">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-171">'Identity'</span></span>
- <span data-ttu-id="2a84c-172">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-172">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-173">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-173">'Razor'</span></span>
- <span data-ttu-id="2a84c-174">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-174">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-175">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-175">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-176">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-176">'Blazor'</span></span>
- <span data-ttu-id="2a84c-177">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-177">'Identity'</span></span>
- <span data-ttu-id="2a84c-178">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-178">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-179">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-179">'Razor'</span></span>
- <span data-ttu-id="2a84c-180">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-180">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-181">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-181">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-182">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-182">'Blazor'</span></span>
- <span data-ttu-id="2a84c-183">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-183">'Identity'</span></span>
- <span data-ttu-id="2a84c-184">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-184">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-185">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-185">'Razor'</span></span>
- <span data-ttu-id="2a84c-186">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-186">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-187">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-187">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-188">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-188">'Blazor'</span></span>
- <span data-ttu-id="2a84c-189">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-189">'Identity'</span></span>
- <span data-ttu-id="2a84c-190">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-190">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-191">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-191">'Razor'</span></span>
- <span data-ttu-id="2a84c-192">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-192">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-193">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-193">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-194">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-194">'Blazor'</span></span>
- <span data-ttu-id="2a84c-195">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-195">'Identity'</span></span>
- <span data-ttu-id="2a84c-196">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-196">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-197">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-197">'Razor'</span></span>
- <span data-ttu-id="2a84c-198">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-198">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-199">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-199">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-200">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-200">'Blazor'</span></span>
- <span data-ttu-id="2a84c-201">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-201">'Identity'</span></span>
- <span data-ttu-id="2a84c-202">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-202">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-203">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-203">'Razor'</span></span>
- <span data-ttu-id="2a84c-204">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-204">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-205">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-205">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-206">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-206">'Blazor'</span></span>
- <span data-ttu-id="2a84c-207">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-207">'Identity'</span></span>
- <span data-ttu-id="2a84c-208">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-208">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-209">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-209">'Razor'</span></span>
- <span data-ttu-id="2a84c-210">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-210">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-211">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-211">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-212">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-212">'Blazor'</span></span>
- <span data-ttu-id="2a84c-213">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-213">'Identity'</span></span>
- <span data-ttu-id="2a84c-214">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-214">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-215">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-215">'Razor'</span></span>
- <span data-ttu-id="2a84c-216">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-216">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-217">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-217">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-218">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-218">'Blazor'</span></span>
- <span data-ttu-id="2a84c-219">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-219">'Identity'</span></span>
- <span data-ttu-id="2a84c-220">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-220">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-221">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-221">'Razor'</span></span>
- <span data-ttu-id="2a84c-222">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-222">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-223">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-223">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-224">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-224">'Blazor'</span></span>
- <span data-ttu-id="2a84c-225">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-225">'Identity'</span></span>
- <span data-ttu-id="2a84c-226">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-226">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-227">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-227">'Razor'</span></span>
- <span data-ttu-id="2a84c-228">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-228">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-229">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-229">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-230">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-230">'Blazor'</span></span>
- <span data-ttu-id="2a84c-231">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-231">'Identity'</span></span>
- <span data-ttu-id="2a84c-232">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-232">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-233">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-233">'Razor'</span></span>
- <span data-ttu-id="2a84c-234">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-234">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-235">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-235">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-236">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-236">'Blazor'</span></span>
- <span data-ttu-id="2a84c-237">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-237">'Identity'</span></span>
- <span data-ttu-id="2a84c-238">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-238">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-239">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-239">'Razor'</span></span>
- <span data-ttu-id="2a84c-240">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-240">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-241">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-241">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-242">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-242">'Blazor'</span></span>
- <span data-ttu-id="2a84c-243">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-243">'Identity'</span></span>
- <span data-ttu-id="2a84c-244">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-244">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-245">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-245">'Razor'</span></span>
- <span data-ttu-id="2a84c-246">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-246">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-247">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-247">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-248">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-248">'Blazor'</span></span>
- <span data-ttu-id="2a84c-249">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-249">'Identity'</span></span>
- <span data-ttu-id="2a84c-250">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-250">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-251">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-251">'Razor'</span></span>
- <span data-ttu-id="2a84c-252">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-252">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-253">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-253">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-254">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-254">'Blazor'</span></span>
- <span data-ttu-id="2a84c-255">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-255">'Identity'</span></span>
- <span data-ttu-id="2a84c-256">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-256">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-257">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-257">'Razor'</span></span>
- <span data-ttu-id="2a84c-258">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-258">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-259">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-259">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-260">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-260">'Blazor'</span></span>
- <span data-ttu-id="2a84c-261">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-261">'Identity'</span></span>
- <span data-ttu-id="2a84c-262">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-262">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-263">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-263">'Razor'</span></span>
- <span data-ttu-id="2a84c-264">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-264">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-265">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-265">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-266">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-266">'Blazor'</span></span>
- <span data-ttu-id="2a84c-267">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-267">'Identity'</span></span>
- <span data-ttu-id="2a84c-268">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-268">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-269">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-269">'Razor'</span></span>
- <span data-ttu-id="2a84c-270">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-270">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-271">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-271">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-272">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-272">'Blazor'</span></span>
- <span data-ttu-id="2a84c-273">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-273">'Identity'</span></span>
- <span data-ttu-id="2a84c-274">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-274">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-275">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-275">'Razor'</span></span>
- <span data-ttu-id="2a84c-276">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-276">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-277">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-277">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-278">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-278">'Blazor'</span></span>
- <span data-ttu-id="2a84c-279">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-279">'Identity'</span></span>
- <span data-ttu-id="2a84c-280">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-280">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-281">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-281">'Razor'</span></span>
- <span data-ttu-id="2a84c-282">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-282">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-283">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-283">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-284">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-284">'Blazor'</span></span>
- <span data-ttu-id="2a84c-285">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-285">'Identity'</span></span>
- <span data-ttu-id="2a84c-286">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-286">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-287">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-287">'Razor'</span></span>
- <span data-ttu-id="2a84c-288">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-288">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-289">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-289">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-290">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-290">'Blazor'</span></span>
- <span data-ttu-id="2a84c-291">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-291">'Identity'</span></span>
- <span data-ttu-id="2a84c-292">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-292">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-293">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-293">'Razor'</span></span>
- <span data-ttu-id="2a84c-294">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-294">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-295">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-295">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-296">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-296">'Blazor'</span></span>
- <span data-ttu-id="2a84c-297">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-297">'Identity'</span></span>
- <span data-ttu-id="2a84c-298">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-298">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-299">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-299">'Razor'</span></span>
- <span data-ttu-id="2a84c-300">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-300">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-301">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-301">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-302">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-302">'Blazor'</span></span>
- <span data-ttu-id="2a84c-303">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-303">'Identity'</span></span>
- <span data-ttu-id="2a84c-304">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-304">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-305">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-305">'Razor'</span></span>
- <span data-ttu-id="2a84c-306">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-306">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-307">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-307">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-308">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-308">'Blazor'</span></span>
- <span data-ttu-id="2a84c-309">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-309">'Identity'</span></span>
- <span data-ttu-id="2a84c-310">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-310">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-311">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-311">'Razor'</span></span>
- <span data-ttu-id="2a84c-312">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-312">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-313">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-313">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-314">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-314">'Blazor'</span></span>
- <span data-ttu-id="2a84c-315">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-315">'Identity'</span></span>
- <span data-ttu-id="2a84c-316">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-316">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-317">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-317">'Razor'</span></span>
- <span data-ttu-id="2a84c-318">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-318">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-319">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-319">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-320">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-320">'Blazor'</span></span>
- <span data-ttu-id="2a84c-321">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-321">'Identity'</span></span>
- <span data-ttu-id="2a84c-322">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-322">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-323">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-323">'Razor'</span></span>
- <span data-ttu-id="2a84c-324">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-324">'SignalR' uid:</span></span> 

<span data-ttu-id="2a84c-325">-------------------------------- | |Facebook |`https://www.facebook.com/dialog/oauth`                          |
|Google |`https://www.googleapis.com/auth/userinfo.profile`               |
|Microsoft |`https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
|Twitter |`https://api.twitter.com/oauth/authenticate`                     |</span><span class="sxs-lookup"><span data-stu-id="2a84c-325">-------------------------------- | | Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |</span></span>

<span data-ttu-id="2a84c-326">在範例應用程式中， `userinfo.profile` 當 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 在上呼叫時，架構會自動新增 Google 的範圍 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-326">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="2a84c-327">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="2a84c-327">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="2a84c-328">在下列範例中， `https://www.googleapis.com/auth/user.birthday.read` 會新增 Google 領域來取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="2a84c-328">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="2a84c-329">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-329">Map user data keys and create claims</span></span>

<span data-ttu-id="2a84c-330">在提供者的選項中，針對 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> 外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="2a84c-330">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="2a84c-331">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-331">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="2a84c-332">範例應用程式會 `urn:google:locale` `urn:google:picture` 從 `locale` `picture` Google 使用者資料中的和金鑰建立地區設定（）和圖片（）宣告：</span><span class="sxs-lookup"><span data-stu-id="2a84c-332">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="2a84c-333">在中 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync` ， <xref:Microsoft.AspNetCore.Identity.IdentityUser> `ApplicationUser` 會使用將（）登入應用程式 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-333">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="2a84c-334">在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 `ApplicationUser` 可從取得之使用者資料的宣告 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-334">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="2a84c-335">在範例應用程式中 `OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入的地區設定（ `urn:google:locale` ）和圖片（ `urn:google:picture` ）宣告 `ApplicationUser` ，包括下列各項的聲明 <xref:System.Security.Claims.ClaimTypes.GivenName> ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-335">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="2a84c-336">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="2a84c-336">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="2a84c-337">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="2a84c-337">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="2a84c-338">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="2a84c-338">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="2a84c-339">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="2a84c-339">The overall size of the request is too large.</span></span>

<span data-ttu-id="2a84c-340">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="2a84c-340">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="2a84c-341">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="2a84c-341">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="2a84c-342">使用 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> Cookie 驗證中介軟體的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 來儲存要求之間的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2a84c-342">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="2a84c-343">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="2a84c-343">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="2a84c-344">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="2a84c-344">Save the access token</span></span>

<span data-ttu-id="2a84c-345"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後，是否應將存取和重新整理權杖儲存在中 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-345"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="2a84c-346">`SaveTokens`預設會設定為， `false` 以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="2a84c-346">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="2a84c-347">範例應用程式會將的值設定 `SaveTokens` 為 `true` 中的 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-347">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="2a84c-348">`OnPostConfirmationAsync`執行時，從的外部提供者儲存存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)） `ApplicationUser` `AuthenticationProperties` 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-348">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="2a84c-349">範例應用程式會在 Account/ExternalLogin 中，將存取權杖儲存在 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中： *Account/ExternalLogin.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="2a84c-349">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="2a84c-350">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="2a84c-350">How to add additional custom tokens</span></span>

<span data-ttu-id="2a84c-351">為了示範如何新增會儲存為一部分的自訂權杖 `SaveTokens` ，範例應用程式會在的 AuthenticationToken.Name 中加入 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> 具有目前的 <xref:System.DateTime> [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated` ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-351">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="2a84c-352">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-352">Creating and adding claims</span></span>

<span data-ttu-id="2a84c-353">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="2a84c-353">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="2a84c-354">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-354">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="2a84c-355">使用者可以自訂動作，方法是從衍生 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法。</span><span class="sxs-lookup"><span data-stu-id="2a84c-355">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="2a84c-356">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-356">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="2a84c-357">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-357">Removal of claim actions and claims</span></span>

<span data-ttu-id="2a84c-358">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除指定的的所有宣告動作 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-358">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="2a84c-359">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除指定的宣告 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-359">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="2a84c-360"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="2a84c-360"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="2a84c-361">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="2a84c-361">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2a84c-362">ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。</span><span class="sxs-lookup"><span data-stu-id="2a84c-362">An ASP.NET Core app can establish additional claims and tokens from external authentication providers, such as Facebook, Google, Microsoft, and Twitter.</span></span> <span data-ttu-id="2a84c-363">每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。</span><span class="sxs-lookup"><span data-stu-id="2a84c-363">Each provider reveals different information about users on its platform, but the pattern for receiving and transforming user data into additional claims is the same.</span></span>

<span data-ttu-id="2a84c-364">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="2a84c-364">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a84c-365">必要條件</span><span class="sxs-lookup"><span data-stu-id="2a84c-365">Prerequisites</span></span>

<span data-ttu-id="2a84c-366">決定要在應用程式中支援的外部驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="2a84c-366">Decide which external authentication providers to support in the app.</span></span> <span data-ttu-id="2a84c-367">針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="2a84c-367">For each provider, register the app and obtain a client ID and client secret.</span></span> <span data-ttu-id="2a84c-368">如需詳細資訊，請參閱<xref:security/authentication/social/index>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-368">For more information, see <xref:security/authentication/social/index>.</span></span> <span data-ttu-id="2a84c-369">範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="2a84c-369">The sample app uses the [Google authentication provider](xref:security/authentication/google-logins).</span></span>

## <a name="set-the-client-id-and-client-secret"></a><span data-ttu-id="2a84c-370">設定用戶端識別碼和用戶端秘密</span><span class="sxs-lookup"><span data-stu-id="2a84c-370">Set the client ID and client secret</span></span>

<span data-ttu-id="2a84c-371">OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。</span><span class="sxs-lookup"><span data-stu-id="2a84c-371">The OAuth authentication provider establishes a trust relationship with an app using a client ID and client secret.</span></span> <span data-ttu-id="2a84c-372">當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。</span><span class="sxs-lookup"><span data-stu-id="2a84c-372">Client ID and client secret values are created for the app by the external authentication provider when the app is registered with the provider.</span></span> <span data-ttu-id="2a84c-373">應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。</span><span class="sxs-lookup"><span data-stu-id="2a84c-373">Each external provider that the app uses must be configured independently with the provider's client ID and client secret.</span></span> <span data-ttu-id="2a84c-374">如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：</span><span class="sxs-lookup"><span data-stu-id="2a84c-374">For more information, see the external authentication provider topics that apply to your scenario:</span></span>

* [<span data-ttu-id="2a84c-375">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-375">Facebook authentication</span></span>](xref:security/authentication/facebook-logins)
* [<span data-ttu-id="2a84c-376">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-376">Google authentication</span></span>](xref:security/authentication/google-logins)
* [<span data-ttu-id="2a84c-377">Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-377">Microsoft authentication</span></span>](xref:security/authentication/microsoft-logins)
* [<span data-ttu-id="2a84c-378">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="2a84c-378">Twitter authentication</span></span>](xref:security/authentication/twitter-logins)
* [<span data-ttu-id="2a84c-379">其他驗證提供者</span><span class="sxs-lookup"><span data-stu-id="2a84c-379">Other authentication providers</span></span>](xref:security/authentication/otherlogins)
* [<span data-ttu-id="2a84c-380">OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="2a84c-380">OpenIdConnect</span></span>](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

<span data-ttu-id="2a84c-381">範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="2a84c-381">The sample app configures the Google authentication provider with a client ID and client secret provided by Google:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a><span data-ttu-id="2a84c-382">建立驗證範圍</span><span class="sxs-lookup"><span data-stu-id="2a84c-382">Establish the authentication scope</span></span>

<span data-ttu-id="2a84c-383">藉由指定，指定要從提供者抓取的許可權清單 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-383">Specify the list of permissions to retrieve from the provider by specifying the <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*>.</span></span> <span data-ttu-id="2a84c-384">下表顯示一般外部提供者的驗證範圍。</span><span class="sxs-lookup"><span data-stu-id="2a84c-384">Authentication scopes for common external providers appear in the following table.</span></span>

| <span data-ttu-id="2a84c-385">提供者</span><span class="sxs-lookup"><span data-stu-id="2a84c-385">Provider</span></span>  | <span data-ttu-id="2a84c-386">影響範圍</span><span class="sxs-lookup"><span data-stu-id="2a84c-386">Scope</span></span>                                                            |
| ---
<span data-ttu-id="2a84c-387">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-387">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-388">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-388">'Blazor'</span></span>
- <span data-ttu-id="2a84c-389">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-389">'Identity'</span></span>
- <span data-ttu-id="2a84c-390">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-390">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-391">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-391">'Razor'</span></span>
- <span data-ttu-id="2a84c-392">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-392">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-393">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-393">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-394">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-394">'Blazor'</span></span>
- <span data-ttu-id="2a84c-395">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-395">'Identity'</span></span>
- <span data-ttu-id="2a84c-396">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-396">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-397">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-397">'Razor'</span></span>
- <span data-ttu-id="2a84c-398">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-398">'SignalR' uid:</span></span> 

<span data-ttu-id="2a84c-399">----- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-399">----- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-400">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-400">'Blazor'</span></span>
- <span data-ttu-id="2a84c-401">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-401">'Identity'</span></span>
- <span data-ttu-id="2a84c-402">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-402">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-403">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-403">'Razor'</span></span>
- <span data-ttu-id="2a84c-404">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-404">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-405">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-405">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-406">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-406">'Blazor'</span></span>
- <span data-ttu-id="2a84c-407">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-407">'Identity'</span></span>
- <span data-ttu-id="2a84c-408">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-408">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-409">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-409">'Razor'</span></span>
- <span data-ttu-id="2a84c-410">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-410">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-411">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-411">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-412">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-412">'Blazor'</span></span>
- <span data-ttu-id="2a84c-413">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-413">'Identity'</span></span>
- <span data-ttu-id="2a84c-414">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-414">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-415">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-415">'Razor'</span></span>
- <span data-ttu-id="2a84c-416">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-416">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-417">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-417">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-418">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-418">'Blazor'</span></span>
- <span data-ttu-id="2a84c-419">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-419">'Identity'</span></span>
- <span data-ttu-id="2a84c-420">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-420">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-421">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-421">'Razor'</span></span>
- <span data-ttu-id="2a84c-422">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-422">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-423">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-423">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-424">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-424">'Blazor'</span></span>
- <span data-ttu-id="2a84c-425">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-425">'Identity'</span></span>
- <span data-ttu-id="2a84c-426">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-426">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-427">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-427">'Razor'</span></span>
- <span data-ttu-id="2a84c-428">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-428">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-429">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-429">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-430">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-430">'Blazor'</span></span>
- <span data-ttu-id="2a84c-431">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-431">'Identity'</span></span>
- <span data-ttu-id="2a84c-432">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-432">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-433">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-433">'Razor'</span></span>
- <span data-ttu-id="2a84c-434">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-434">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-435">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-435">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-436">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-436">'Blazor'</span></span>
- <span data-ttu-id="2a84c-437">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-437">'Identity'</span></span>
- <span data-ttu-id="2a84c-438">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-438">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-439">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-439">'Razor'</span></span>
- <span data-ttu-id="2a84c-440">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-440">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-441">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-441">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-442">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-442">'Blazor'</span></span>
- <span data-ttu-id="2a84c-443">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-443">'Identity'</span></span>
- <span data-ttu-id="2a84c-444">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-444">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-445">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-445">'Razor'</span></span>
- <span data-ttu-id="2a84c-446">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-446">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-447">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-447">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-448">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-448">'Blazor'</span></span>
- <span data-ttu-id="2a84c-449">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-449">'Identity'</span></span>
- <span data-ttu-id="2a84c-450">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-450">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-451">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-451">'Razor'</span></span>
- <span data-ttu-id="2a84c-452">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-452">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-453">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-453">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-454">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-454">'Blazor'</span></span>
- <span data-ttu-id="2a84c-455">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-455">'Identity'</span></span>
- <span data-ttu-id="2a84c-456">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-456">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-457">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-457">'Razor'</span></span>
- <span data-ttu-id="2a84c-458">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-458">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-459">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-459">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-460">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-460">'Blazor'</span></span>
- <span data-ttu-id="2a84c-461">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-461">'Identity'</span></span>
- <span data-ttu-id="2a84c-462">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-462">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-463">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-463">'Razor'</span></span>
- <span data-ttu-id="2a84c-464">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-464">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-465">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-465">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-466">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-466">'Blazor'</span></span>
- <span data-ttu-id="2a84c-467">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-467">'Identity'</span></span>
- <span data-ttu-id="2a84c-468">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-468">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-469">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-469">'Razor'</span></span>
- <span data-ttu-id="2a84c-470">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-470">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-471">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-471">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-472">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-472">'Blazor'</span></span>
- <span data-ttu-id="2a84c-473">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-473">'Identity'</span></span>
- <span data-ttu-id="2a84c-474">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-474">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-475">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-475">'Razor'</span></span>
- <span data-ttu-id="2a84c-476">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-476">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-477">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-477">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-478">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-478">'Blazor'</span></span>
- <span data-ttu-id="2a84c-479">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-479">'Identity'</span></span>
- <span data-ttu-id="2a84c-480">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-480">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-481">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-481">'Razor'</span></span>
- <span data-ttu-id="2a84c-482">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-482">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-483">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-483">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-484">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-484">'Blazor'</span></span>
- <span data-ttu-id="2a84c-485">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-485">'Identity'</span></span>
- <span data-ttu-id="2a84c-486">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-486">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-487">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-487">'Razor'</span></span>
- <span data-ttu-id="2a84c-488">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-488">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-489">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-489">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-490">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-490">'Blazor'</span></span>
- <span data-ttu-id="2a84c-491">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-491">'Identity'</span></span>
- <span data-ttu-id="2a84c-492">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-492">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-493">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-493">'Razor'</span></span>
- <span data-ttu-id="2a84c-494">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-494">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-495">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-495">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-496">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-496">'Blazor'</span></span>
- <span data-ttu-id="2a84c-497">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-497">'Identity'</span></span>
- <span data-ttu-id="2a84c-498">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-498">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-499">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-499">'Razor'</span></span>
- <span data-ttu-id="2a84c-500">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-500">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-501">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-501">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-502">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-502">'Blazor'</span></span>
- <span data-ttu-id="2a84c-503">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-503">'Identity'</span></span>
- <span data-ttu-id="2a84c-504">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-504">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-505">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-505">'Razor'</span></span>
- <span data-ttu-id="2a84c-506">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-506">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-507">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-507">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-508">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-508">'Blazor'</span></span>
- <span data-ttu-id="2a84c-509">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-509">'Identity'</span></span>
- <span data-ttu-id="2a84c-510">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-510">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-511">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-511">'Razor'</span></span>
- <span data-ttu-id="2a84c-512">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-512">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-513">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-513">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-514">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-514">'Blazor'</span></span>
- <span data-ttu-id="2a84c-515">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-515">'Identity'</span></span>
- <span data-ttu-id="2a84c-516">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-516">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-517">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-517">'Razor'</span></span>
- <span data-ttu-id="2a84c-518">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-518">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-519">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-519">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-520">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-520">'Blazor'</span></span>
- <span data-ttu-id="2a84c-521">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-521">'Identity'</span></span>
- <span data-ttu-id="2a84c-522">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-522">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-523">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-523">'Razor'</span></span>
- <span data-ttu-id="2a84c-524">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-524">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-525">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-525">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-526">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-526">'Blazor'</span></span>
- <span data-ttu-id="2a84c-527">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-527">'Identity'</span></span>
- <span data-ttu-id="2a84c-528">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-528">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-529">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-529">'Razor'</span></span>
- <span data-ttu-id="2a84c-530">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-530">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-531">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-531">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-532">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-532">'Blazor'</span></span>
- <span data-ttu-id="2a84c-533">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-533">'Identity'</span></span>
- <span data-ttu-id="2a84c-534">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-534">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-535">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-535">'Razor'</span></span>
- <span data-ttu-id="2a84c-536">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-536">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-537">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-537">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-538">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-538">'Blazor'</span></span>
- <span data-ttu-id="2a84c-539">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-539">'Identity'</span></span>
- <span data-ttu-id="2a84c-540">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-540">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-541">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-541">'Razor'</span></span>
- <span data-ttu-id="2a84c-542">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-542">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-543">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-543">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-544">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-544">'Blazor'</span></span>
- <span data-ttu-id="2a84c-545">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-545">'Identity'</span></span>
- <span data-ttu-id="2a84c-546">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-546">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-547">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-547">'Razor'</span></span>
- <span data-ttu-id="2a84c-548">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-548">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-549">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-549">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-550">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-550">'Blazor'</span></span>
- <span data-ttu-id="2a84c-551">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-551">'Identity'</span></span>
- <span data-ttu-id="2a84c-552">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-552">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-553">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-553">'Razor'</span></span>
- <span data-ttu-id="2a84c-554">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-554">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-555">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-555">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-556">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-556">'Blazor'</span></span>
- <span data-ttu-id="2a84c-557">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-557">'Identity'</span></span>
- <span data-ttu-id="2a84c-558">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-558">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-559">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-559">'Razor'</span></span>
- <span data-ttu-id="2a84c-560">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-560">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-561">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-561">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-562">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-562">'Blazor'</span></span>
- <span data-ttu-id="2a84c-563">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-563">'Identity'</span></span>
- <span data-ttu-id="2a84c-564">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-564">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-565">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-565">'Razor'</span></span>
- <span data-ttu-id="2a84c-566">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-566">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-567">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-567">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-568">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-568">'Blazor'</span></span>
- <span data-ttu-id="2a84c-569">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-569">'Identity'</span></span>
- <span data-ttu-id="2a84c-570">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-570">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-571">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-571">'Razor'</span></span>
- <span data-ttu-id="2a84c-572">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-572">'SignalR' uid:</span></span> 

-
<span data-ttu-id="2a84c-573">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="2a84c-573">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="2a84c-574">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-574">'Blazor'</span></span>
- <span data-ttu-id="2a84c-575">'Identity'</span><span class="sxs-lookup"><span data-stu-id="2a84c-575">'Identity'</span></span>
- <span data-ttu-id="2a84c-576">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="2a84c-576">'Let's Encrypt'</span></span>
- <span data-ttu-id="2a84c-577">'Razor'</span><span class="sxs-lookup"><span data-stu-id="2a84c-577">'Razor'</span></span>
- <span data-ttu-id="2a84c-578">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="2a84c-578">'SignalR' uid:</span></span> 

<span data-ttu-id="2a84c-579">-------------------------------- | |Facebook |`https://www.facebook.com/dialog/oauth`                          |
|Google |`https://www.googleapis.com/auth/userinfo.profile`               |
|Microsoft |`https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
|Twitter |`https://api.twitter.com/oauth/authenticate`                     |</span><span class="sxs-lookup"><span data-stu-id="2a84c-579">-------------------------------- | | Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |</span></span>

<span data-ttu-id="2a84c-580">在範例應用程式中， `userinfo.profile` 當 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 在上呼叫時，架構會自動新增 Google 的範圍 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-580">In the sample app, Google's `userinfo.profile` scope is automatically added by the framework when <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> is called on the <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder>.</span></span> <span data-ttu-id="2a84c-581">如果應用程式需要額外的範圍，請將它們新增至選項。</span><span class="sxs-lookup"><span data-stu-id="2a84c-581">If the app requires additional scopes, add them to the options.</span></span> <span data-ttu-id="2a84c-582">在下列範例中， `https://www.googleapis.com/auth/user.birthday.read` 會新增 Google 領域來取得使用者的生日：</span><span class="sxs-lookup"><span data-stu-id="2a84c-582">In the following example, the Google `https://www.googleapis.com/auth/user.birthday.read` scope is added in order to retrieve a user's birthday:</span></span>

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a><span data-ttu-id="2a84c-583">對應使用者資料索引鍵和建立宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-583">Map user data keys and create claims</span></span>

<span data-ttu-id="2a84c-584">在提供者的選項中，針對 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> 外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。</span><span class="sxs-lookup"><span data-stu-id="2a84c-584">In the provider's options, specify a <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> or <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> for each key/subkey in the external provider's JSON user data for the app identity to read on sign in.</span></span> <span data-ttu-id="2a84c-585">如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-585">For more information on claim types, see <xref:System.Security.Claims.ClaimTypes>.</span></span>

<span data-ttu-id="2a84c-586">範例應用程式會 `urn:google:locale` `urn:google:picture` 從 `locale` `picture` Google 使用者資料中的和金鑰建立地區設定（）和圖片（）宣告：</span><span class="sxs-lookup"><span data-stu-id="2a84c-586">The sample app creates locale (`urn:google:locale`) and picture (`urn:google:picture`) claims from the `locale` and `picture` keys in Google user data:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

<span data-ttu-id="2a84c-587">在中 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync` ， <xref:Microsoft.AspNetCore.Identity.IdentityUser> `ApplicationUser` 會使用將（）登入應用程式 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-587">In `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync`, an <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) is signed into the app with <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*>.</span></span> <span data-ttu-id="2a84c-588">在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 `ApplicationUser` 可從取得之使用者資料的宣告 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-588">During the sign in process, the <xref:Microsoft.AspNetCore.Identity.UserManager%601> can store an `ApplicationUser` claims for user data available from the <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*>.</span></span>

<span data-ttu-id="2a84c-589">在範例應用程式中 `OnPostConfirmationAsync` （*Account/ExternalLogin*）會建立已登入的地區設定（ `urn:google:locale` ）和圖片（ `urn:google:picture` ）宣告 `ApplicationUser` ，包括下列各項的聲明 <xref:System.Security.Claims.ClaimTypes.GivenName> ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-589">In the sample app, `OnPostConfirmationAsync` (*Account/ExternalLogin.cshtml.cs*) establishes the locale (`urn:google:locale`) and picture (`urn:google:picture`) claims for the signed in `ApplicationUser`, including a claim for <xref:System.Security.Claims.ClaimTypes.GivenName>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

<span data-ttu-id="2a84c-590">根據預設，使用者的宣告會儲存在驗證 cookie 中。</span><span class="sxs-lookup"><span data-stu-id="2a84c-590">By default, a user's claims are stored in the authentication cookie.</span></span> <span data-ttu-id="2a84c-591">如果驗證 cookie 太大，可能會導致應用程式失敗，因為：</span><span class="sxs-lookup"><span data-stu-id="2a84c-591">If the authentication cookie is too large, it can cause the app to fail because:</span></span>

* <span data-ttu-id="2a84c-592">瀏覽器偵測到 cookie 標頭太長。</span><span class="sxs-lookup"><span data-stu-id="2a84c-592">The browser detects that the cookie header is too long.</span></span>
* <span data-ttu-id="2a84c-593">要求的整體大小太大。</span><span class="sxs-lookup"><span data-stu-id="2a84c-593">The overall size of the request is too large.</span></span>

<span data-ttu-id="2a84c-594">如果需要大量的使用者資料來處理使用者要求：</span><span class="sxs-lookup"><span data-stu-id="2a84c-594">If a large amount of user data is required for processing user requests:</span></span>

* <span data-ttu-id="2a84c-595">將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。</span><span class="sxs-lookup"><span data-stu-id="2a84c-595">Limit the number and size of user claims for request processing to only what the app requires.</span></span>
* <span data-ttu-id="2a84c-596">使用 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> Cookie 驗證中介軟體的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 來儲存要求之間的身分識別。</span><span class="sxs-lookup"><span data-stu-id="2a84c-596">Use a custom <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> for the Cookie Authentication Middleware's <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> to store identity across requests.</span></span> <span data-ttu-id="2a84c-597">在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="2a84c-597">Preserve large quantities of identity information on the server while only sending a small session identifier key to the client.</span></span>

## <a name="save-the-access-token"></a><span data-ttu-id="2a84c-598">儲存存取權杖</span><span class="sxs-lookup"><span data-stu-id="2a84c-598">Save the access token</span></span>

<span data-ttu-id="2a84c-599"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後，是否應將存取和重新整理權杖儲存在中 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-599"><xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*> defines whether access and refresh tokens should be stored in the <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> after a successful authorization.</span></span> <span data-ttu-id="2a84c-600">`SaveTokens`預設會設定為， `false` 以減少最終驗證 cookie 的大小。</span><span class="sxs-lookup"><span data-stu-id="2a84c-600">`SaveTokens` is set to `false` by default to reduce the size of the final authentication cookie.</span></span>

<span data-ttu-id="2a84c-601">範例應用程式會將的值設定 `SaveTokens` 為 `true` 中的 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-601">The sample app sets the value of `SaveTokens` to `true` in <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

<span data-ttu-id="2a84c-602">`OnPostConfirmationAsync`執行時，從的外部提供者儲存存取權杖（[ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)） `ApplicationUser` `AuthenticationProperties` 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-602">When `OnPostConfirmationAsync` executes, store the access token ([ExternalLoginInfo.AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) from the external provider in the `ApplicationUser`'s `AuthenticationProperties`.</span></span>

<span data-ttu-id="2a84c-603">範例應用程式會在 Account/ExternalLogin 中，將存取權杖儲存在 `OnPostConfirmationAsync` （新的使用者註冊）和 `OnGetCallbackAsync` （先前註冊的使用者）中： *Account/ExternalLogin.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="2a84c-603">The sample app saves the access token in `OnPostConfirmationAsync` (new user registration) and `OnGetCallbackAsync` (previously registered user) in *Account/ExternalLogin.cshtml.cs*:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a><span data-ttu-id="2a84c-604">如何新增額外的自訂權杖</span><span class="sxs-lookup"><span data-stu-id="2a84c-604">How to add additional custom tokens</span></span>

<span data-ttu-id="2a84c-605">為了示範如何新增會儲存為一部分的自訂權杖 `SaveTokens` ，範例應用程式會在的 AuthenticationToken.Name 中加入 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> 具有目前的 <xref:System.DateTime> [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated` ：</span><span class="sxs-lookup"><span data-stu-id="2a84c-605">To demonstrate how to add a custom token, which is stored as part of `SaveTokens`, the sample app adds an <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> with the current <xref:System.DateTime> for an [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) of `TicketCreated`:</span></span>

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a><span data-ttu-id="2a84c-606">建立和新增宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-606">Creating and adding claims</span></span>

<span data-ttu-id="2a84c-607">架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。</span><span class="sxs-lookup"><span data-stu-id="2a84c-607">The framework provides common actions and extension methods for creating and adding claims to the collection.</span></span> <span data-ttu-id="2a84c-608">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-608">For more information, see the <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> and <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>.</span></span>

<span data-ttu-id="2a84c-609">使用者可以自訂動作，方法是從衍生 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法。</span><span class="sxs-lookup"><span data-stu-id="2a84c-609">Users can define custom actions by deriving from <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> and implementing the abstract <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> method.</span></span>

<span data-ttu-id="2a84c-610">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。</span><span class="sxs-lookup"><span data-stu-id="2a84c-610">For more information, see <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>.</span></span>

## <a name="removal-of-claim-actions-and-claims"></a><span data-ttu-id="2a84c-611">移除宣告動作和宣告</span><span class="sxs-lookup"><span data-stu-id="2a84c-611">Removal of claim actions and claims</span></span>

<span data-ttu-id="2a84c-612">[ClaimActionCollection （String）](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)會從集合中移除指定的的所有宣告動作 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-612">[ClaimActionCollection.Remove(String)](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*) removes all claim actions for the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the collection.</span></span> <span data-ttu-id="2a84c-613">[ClaimActionCollectionMapExtensions. DeleteClaim （ClaimActionCollection，String）](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除指定的宣告 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。</span><span class="sxs-lookup"><span data-stu-id="2a84c-613">[ClaimActionCollectionMapExtensions.DeleteClaim(ClaimActionCollection, String)](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*) deletes a claim of the given <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> from the identity.</span></span> <span data-ttu-id="2a84c-614"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要與[OpenID connect （OIDC）](/azure/active-directory/develop/v2-protocols-oidc)搭配使用，以移除通訊協定產生的宣告。</span><span class="sxs-lookup"><span data-stu-id="2a84c-614"><xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*> is primarily used with [OpenID Connect (OIDC)](/azure/active-directory/develop/v2-protocols-oidc) to remove protocol-generated claims.</span></span>

## <a name="sample-app-output"></a><span data-ttu-id="2a84c-615">範例應用程式輸出</span><span class="sxs-lookup"><span data-stu-id="2a84c-615">Sample app output</span></span>

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2a84c-616">其他資源</span><span class="sxs-lookup"><span data-stu-id="2a84c-616">Additional resources</span></span>

* <span data-ttu-id="2a84c-617">[dotnet/AspNetCore 工程 SocialSample 應用程式](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)：連結的範例應用程式位於[Dotnet/AspNetCore GitHub 存放庫的](https://github.com/dotnet/AspNetCore) `master` 工程分支。</span><span class="sxs-lookup"><span data-stu-id="2a84c-617">[dotnet/AspNetCore engineering SocialSample app](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample): The linked sample app is on the [dotnet/AspNetCore GitHub repo's](https://github.com/dotnet/AspNetCore) `master` engineering branch.</span></span> <span data-ttu-id="2a84c-618">`master`分支包含適用于下一個版本之 ASP.NET Core 的主動式開發程式碼。</span><span class="sxs-lookup"><span data-stu-id="2a84c-618">The `master` branch contains code under active development for the next release of ASP.NET Core.</span></span> <span data-ttu-id="2a84c-619">若要查看 ASP.NET Core 發行版本的範例應用程式版本，請使用 [**分支**] 下拉式清單來選取發行分支（例如 `release/{X.Y}` ）。</span><span class="sxs-lookup"><span data-stu-id="2a84c-619">To see a version of the sample app for a released version of ASP.NET Core, use the **Branch** drop down list to select a release branch (for example `release/{X.Y}`).</span></span>
