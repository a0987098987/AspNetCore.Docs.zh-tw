---
title: 在 ASP.NET Core 中設定憑證驗證
author: blowdart
description: 瞭解如何在 IIS 和 HTTP.sys 的 ASP.NET Core 中設定憑證驗證。
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 11/05/2019
uid: security/authentication/certauth
ms.openlocfilehash: 081935e6e6248b5fe9b7bf4cd966dc73761d2ec1
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634059"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="83dd4-103">在 ASP.NET Core 中設定憑證驗證</span><span class="sxs-lookup"><span data-stu-id="83dd4-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="83dd4-104">`Microsoft.AspNetCore.Authentication.Certificate` 包含類似于 ASP.NET Core 的[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)的執行方式。</span><span class="sxs-lookup"><span data-stu-id="83dd4-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="83dd4-105">憑證驗證會在 TLS 層級進行，長時間才會到達 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="83dd4-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="83dd4-106">更準確地說，這是驗證憑證的驗證處理常式，並提供您可將該憑證解析成 `ClaimsPrincipal`的事件。</span><span class="sxs-lookup"><span data-stu-id="83dd4-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="83dd4-107">[設定您的主機](#configure-your-host-to-require-certificates)以進行憑證驗證，其為 IIS、Kestrel、Azure Web Apps 或您所使用的任何其他。</span><span class="sxs-lookup"><span data-stu-id="83dd4-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="83dd4-108">Proxy 和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="83dd4-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="83dd4-109">憑證驗證是一種可設定狀態的案例，主要用於 proxy 或負載平衡器不會處理用戶端與伺服器之間的流量。</span><span class="sxs-lookup"><span data-stu-id="83dd4-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="83dd4-110">如果使用 proxy 或負載平衡器，憑證驗證僅適用于 proxy 或負載平衡器：</span><span class="sxs-lookup"><span data-stu-id="83dd4-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="83dd4-111">處理驗證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-111">Handles the authentication.</span></span>
* <span data-ttu-id="83dd4-112">將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="83dd4-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="83dd4-113">在使用 proxy 和負載平衡器的環境中，憑證驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。</span><span class="sxs-lookup"><span data-stu-id="83dd4-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="83dd4-114">開始使用</span><span class="sxs-lookup"><span data-stu-id="83dd4-114">Get started</span></span>

<span data-ttu-id="83dd4-115">取得 HTTPS 憑證並加以套用，並[將您的主機設定](#configure-your-host-to-require-certificates)為需要憑證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="83dd4-116">在您的 web 應用程式中，新增 `Microsoft.AspNetCore.Authentication.Certificate` 封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="83dd4-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="83dd4-117">然後在 `Startup.ConfigureServices` 方法中，使用您的選項呼叫 `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);`，並提供委派給 `OnCertificateValidated`，以在隨要求一起傳送的用戶端憑證上執行任何補充驗證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="83dd4-118">將該資訊轉換成 `ClaimsPrincipal`，並在 `context.Principal` 屬性上進行設定。</span><span class="sxs-lookup"><span data-stu-id="83dd4-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="83dd4-119">如果驗證失敗，此處理程式會傳回 `403 (Forbidden)` 回應，而不是如您所預期的 `401 (Unauthorized)`。</span><span class="sxs-lookup"><span data-stu-id="83dd4-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="83dd4-120">其原因是必須在初始 TLS 連線期間進行驗證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="83dd4-121">當它到達處理常式時，就太晚了。</span><span class="sxs-lookup"><span data-stu-id="83dd4-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="83dd4-122">沒有任何方法可將連接從匿名連接升級為具有憑證的連線。</span><span class="sxs-lookup"><span data-stu-id="83dd4-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="83dd4-123">此外，也請在 `Startup.Configure` 方法中新增 `app.UseAuthentication();`。</span><span class="sxs-lookup"><span data-stu-id="83dd4-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="83dd4-124">否則，HttpCoNtext 使用者將不會設定為從憑證建立 `ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="83dd4-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="83dd4-125">例如:</span><span class="sxs-lookup"><span data-stu-id="83dd4-125">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

<span data-ttu-id="83dd4-126">上述範例示範新增憑證驗證的預設方式。</span><span class="sxs-lookup"><span data-stu-id="83dd4-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="83dd4-127">處理常式會使用一般憑證屬性來建立使用者主體。</span><span class="sxs-lookup"><span data-stu-id="83dd4-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="83dd4-128">設定憑證驗證</span><span class="sxs-lookup"><span data-stu-id="83dd4-128">Configure certificate validation</span></span>

<span data-ttu-id="83dd4-129">`CertificateAuthenticationOptions` 處理常式有一些內建的驗證，這是您應該在憑證上執行的最小驗證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="83dd4-130">預設會啟用這些設定。</span><span class="sxs-lookup"><span data-stu-id="83dd4-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="83dd4-131">AllowedCertificateTypes = 連鎖、Lnk-selfsigned 之類或 All （連鎖 |Lnk-selfsigned 之類</span><span class="sxs-lookup"><span data-stu-id="83dd4-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="83dd4-132">這種檢查會驗證是否只允許適當的憑證類型。</span><span class="sxs-lookup"><span data-stu-id="83dd4-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="83dd4-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="83dd4-133">ValidateCertificateUse</span></span>

<span data-ttu-id="83dd4-134">這項檢查會驗證用戶端所提供的憑證是否有用戶端驗證擴充金鑰使用（EKU），或完全沒有 Eku。</span><span class="sxs-lookup"><span data-stu-id="83dd4-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="83dd4-135">如規格所示，如果未指定任何 EKU，則所有 Eku 都會被視為有效。</span><span class="sxs-lookup"><span data-stu-id="83dd4-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="83dd4-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="83dd4-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="83dd4-137">這種檢查會驗證憑證是否在其有效期間內。</span><span class="sxs-lookup"><span data-stu-id="83dd4-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="83dd4-138">在每個要求上，處理常式可確保在其目前的會話期間，當憑證呈現時，其有效的憑證尚未到期。</span><span class="sxs-lookup"><span data-stu-id="83dd4-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="83dd4-139">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="83dd4-139">RevocationFlag</span></span>

<span data-ttu-id="83dd4-140">指定要檢查鏈中哪些憑證以進行撤銷的旗標。</span><span class="sxs-lookup"><span data-stu-id="83dd4-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="83dd4-141">只有當憑證連結至根憑證時，才會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="83dd4-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="83dd4-142">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="83dd4-142">RevocationMode</span></span>

<span data-ttu-id="83dd4-143">指定撤銷檢查執行方式的旗標。</span><span class="sxs-lookup"><span data-stu-id="83dd4-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="83dd4-144">當您連線到憑證授權單位單位時，指定線上檢查可能會造成長時間的延遲。</span><span class="sxs-lookup"><span data-stu-id="83dd4-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="83dd4-145">只有當憑證連結至根憑證時，才會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="83dd4-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="83dd4-146">我可以將我的應用程式設定為只在特定路徑上要求憑證嗎？</span><span class="sxs-lookup"><span data-stu-id="83dd4-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="83dd4-147">這是不可能的。</span><span class="sxs-lookup"><span data-stu-id="83dd4-147">This isn't possible.</span></span> <span data-ttu-id="83dd4-148">請記住，憑證交換已完成 HTTPS 交談的開頭，它是由伺服器在該連線上收到第一個要求之前完成，因此不可能根據任何要求欄位來界定範圍。</span><span class="sxs-lookup"><span data-stu-id="83dd4-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="83dd4-149">處理程式事件</span><span class="sxs-lookup"><span data-stu-id="83dd4-149">Handler events</span></span>

<span data-ttu-id="83dd4-150">此處理程式有兩個事件：</span><span class="sxs-lookup"><span data-stu-id="83dd4-150">The handler has two events:</span></span>

* <span data-ttu-id="83dd4-151">如果在驗證期間發生例外狀況，則會呼叫 `OnAuthenticationFailed` &ndash;，並可讓您做出回應。</span><span class="sxs-lookup"><span data-stu-id="83dd4-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="83dd4-152">在驗證憑證後呼叫 &ndash; `OnCertificateValidated`，通過驗證並已建立預設主體。</span><span class="sxs-lookup"><span data-stu-id="83dd4-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="83dd4-153">此事件可讓您執行自己的驗證，並增強或取代主體。</span><span class="sxs-lookup"><span data-stu-id="83dd4-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="83dd4-154">範例包括：</span><span class="sxs-lookup"><span data-stu-id="83dd4-154">For examples include:</span></span>
  * <span data-ttu-id="83dd4-155">判斷您的服務是否知道憑證。</span><span class="sxs-lookup"><span data-stu-id="83dd4-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="83dd4-156">建立您自己的主體。</span><span class="sxs-lookup"><span data-stu-id="83dd4-156">Constructing your own principal.</span></span> <span data-ttu-id="83dd4-157">請考慮 `Startup.ConfigureServices` 中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="83dd4-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="83dd4-158">如果您發現輸入憑證不符合您的額外驗證，請呼叫 `context.Fail("failure reason")`，但發生失敗原因。</span><span class="sxs-lookup"><span data-stu-id="83dd4-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="83dd4-159">針對實際的功能，您可能會想要呼叫在相依性插入中註冊的服務，而這些相依性會連接到資料庫或其他類型的使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="83dd4-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="83dd4-160">使用傳入委派的內容存取您的服務。</span><span class="sxs-lookup"><span data-stu-id="83dd4-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="83dd4-161">請考慮 `Startup.ConfigureServices` 中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="83dd4-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="83dd4-162">就概念而言，驗證憑證是一項授權考慮。</span><span class="sxs-lookup"><span data-stu-id="83dd4-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="83dd4-163">例如，在授權原則中新增核取簽發者或指紋，而不是在 `OnCertificateValidated`內，是完全可接受的。</span><span class="sxs-lookup"><span data-stu-id="83dd4-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="83dd4-164">將您的主機設定為需要憑證</span><span class="sxs-lookup"><span data-stu-id="83dd4-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="83dd4-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="83dd4-165">Kestrel</span></span>

<span data-ttu-id="83dd4-166">在*Program.cs*中，設定 Kestrel，如下所示：</span><span class="sxs-lookup"><span data-stu-id="83dd4-166">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp

public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                    webBuilder.ConfigureKestrel(o =>
                    {
                        o.ConfigureHttpsDefaults(o => o.ClientCertificateMode = ClientCertificateMode.RequireCertificate);
                    });
                });
}
```

### <a name="iis"></a><span data-ttu-id="83dd4-167">IIS</span><span class="sxs-lookup"><span data-stu-id="83dd4-167">IIS</span></span>

<span data-ttu-id="83dd4-168">在 IIS 管理員中完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="83dd4-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="83dd4-169">從 [**連接**] 索引標籤中選取您的網站。</span><span class="sxs-lookup"><span data-stu-id="83dd4-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="83dd4-170">按兩下 [**功能] 視圖**視窗中的 [ **SSL 設定**] 選項。</span><span class="sxs-lookup"><span data-stu-id="83dd4-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="83dd4-171">勾選 [**需要 SSL** ] 核取方塊，然後選取 [**用戶端憑證**] 區段中的 [**需要**] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="83dd4-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="83dd4-173">Azure 和自訂 web proxy</span><span class="sxs-lookup"><span data-stu-id="83dd4-173">Azure and custom web proxies</span></span>

<span data-ttu-id="83dd4-174">如需如何設定憑證轉送中介軟體的詳細說明，請參閱[主機和部署檔](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)。</span><span class="sxs-lookup"><span data-stu-id="83dd4-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
