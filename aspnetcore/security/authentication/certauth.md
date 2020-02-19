---
title: 在 ASP.NET Core 中設定憑證驗證
author: blowdart
description: 瞭解如何在 IIS 和 HTTP.sys 的 ASP.NET Core 中設定憑證驗證。
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 01/02/2020
uid: security/authentication/certauth
ms.openlocfilehash: 280daa86510d4445c791b6952653122961f13aeb
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447278"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="597f1-103">在 ASP.NET Core 中設定憑證驗證</span><span class="sxs-lookup"><span data-stu-id="597f1-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="597f1-104">`Microsoft.AspNetCore.Authentication.Certificate` 包含類似于 ASP.NET Core 的[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)的執行方式。</span><span class="sxs-lookup"><span data-stu-id="597f1-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="597f1-105">憑證驗證會在 TLS 層級進行，長時間才會到達 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="597f1-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="597f1-106">更準確地說，這是驗證憑證的驗證處理常式，並提供您可將該憑證解析成 `ClaimsPrincipal`的事件。</span><span class="sxs-lookup"><span data-stu-id="597f1-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="597f1-107">[設定您的主機](#configure-your-host-to-require-certificates)以進行憑證驗證，其為 IIS、Kestrel、Azure Web Apps 或您所使用的任何其他。</span><span class="sxs-lookup"><span data-stu-id="597f1-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="597f1-108">Proxy 和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="597f1-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="597f1-109">憑證驗證是一種可設定狀態的案例，主要用於 proxy 或負載平衡器不會處理用戶端與伺服器之間的流量。</span><span class="sxs-lookup"><span data-stu-id="597f1-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="597f1-110">如果使用 proxy 或負載平衡器，憑證驗證僅適用于 proxy 或負載平衡器：</span><span class="sxs-lookup"><span data-stu-id="597f1-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="597f1-111">處理驗證。</span><span class="sxs-lookup"><span data-stu-id="597f1-111">Handles the authentication.</span></span>
* <span data-ttu-id="597f1-112">將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。</span><span class="sxs-lookup"><span data-stu-id="597f1-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="597f1-113">在使用 proxy 和負載平衡器的環境中，憑證驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。</span><span class="sxs-lookup"><span data-stu-id="597f1-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="597f1-114">開始使用</span><span class="sxs-lookup"><span data-stu-id="597f1-114">Get started</span></span>

<span data-ttu-id="597f1-115">取得 HTTPS 憑證並加以套用，並[將您的主機設定](#configure-your-host-to-require-certificates)為需要憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="597f1-116">在您的 web 應用程式中，新增 `Microsoft.AspNetCore.Authentication.Certificate` 封裝的參考。</span><span class="sxs-lookup"><span data-stu-id="597f1-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="597f1-117">然後在 `Startup.ConfigureServices` 方法中，使用您的選項呼叫 `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);`，並提供委派給 `OnCertificateValidated`，以在隨要求一起傳送的用戶端憑證上執行任何補充驗證。</span><span class="sxs-lookup"><span data-stu-id="597f1-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="597f1-118">將該資訊轉換成 `ClaimsPrincipal`，並在 `context.Principal` 屬性上進行設定。</span><span class="sxs-lookup"><span data-stu-id="597f1-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="597f1-119">如果驗證失敗，此處理程式會傳回 `403 (Forbidden)` 回應，而不是如您所預期的 `401 (Unauthorized)`。</span><span class="sxs-lookup"><span data-stu-id="597f1-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="597f1-120">其原因是必須在初始 TLS 連線期間進行驗證。</span><span class="sxs-lookup"><span data-stu-id="597f1-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="597f1-121">當它到達處理常式時，就太晚了。</span><span class="sxs-lookup"><span data-stu-id="597f1-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="597f1-122">沒有任何方法可將連接從匿名連接升級為具有憑證的連線。</span><span class="sxs-lookup"><span data-stu-id="597f1-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="597f1-123">此外，也請在 `Startup.Configure` 方法中新增 `app.UseAuthentication();`。</span><span class="sxs-lookup"><span data-stu-id="597f1-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="597f1-124">否則，`HttpContext.User` 將不會設定為從憑證建立 `ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="597f1-124">Otherwise, the `HttpContext.User` will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="597f1-125">例如，</span><span class="sxs-lookup"><span data-stu-id="597f1-125">For example:</span></span>

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

<span data-ttu-id="597f1-126">上述範例示範新增憑證驗證的預設方式。</span><span class="sxs-lookup"><span data-stu-id="597f1-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="597f1-127">處理常式會使用一般憑證屬性來建立使用者主體。</span><span class="sxs-lookup"><span data-stu-id="597f1-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="597f1-128">設定憑證驗證</span><span class="sxs-lookup"><span data-stu-id="597f1-128">Configure certificate validation</span></span>

<span data-ttu-id="597f1-129">`CertificateAuthenticationOptions` 處理常式有一些內建的驗證，這是您應該在憑證上執行的最小驗證。</span><span class="sxs-lookup"><span data-stu-id="597f1-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="597f1-130">預設會啟用這些設定。</span><span class="sxs-lookup"><span data-stu-id="597f1-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="597f1-131">AllowedCertificateTypes = 連鎖、Lnk-selfsigned 之類或 All （連鎖 |Lnk-selfsigned 之類</span><span class="sxs-lookup"><span data-stu-id="597f1-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="597f1-132">預設值：`CertificateTypes.Chained`</span><span class="sxs-lookup"><span data-stu-id="597f1-132">Default value: `CertificateTypes.Chained`</span></span>

<span data-ttu-id="597f1-133">這種檢查會驗證是否只允許適當的憑證類型。</span><span class="sxs-lookup"><span data-stu-id="597f1-133">This check validates that only the appropriate certificate type is allowed.</span></span> <span data-ttu-id="597f1-134">如果應用程式使用自我簽署憑證，則必須將此選項設定為 `CertificateTypes.All` 或 `CertificateTypes.SelfSigned`。</span><span class="sxs-lookup"><span data-stu-id="597f1-134">If the app is using self-signed certificates, this option needs to be set to `CertificateTypes.All` or `CertificateTypes.SelfSigned`.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="597f1-135">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="597f1-135">ValidateCertificateUse</span></span>

<span data-ttu-id="597f1-136">預設值：`true`</span><span class="sxs-lookup"><span data-stu-id="597f1-136">Default value: `true`</span></span>

<span data-ttu-id="597f1-137">這項檢查會驗證用戶端所提供的憑證是否有用戶端驗證擴充金鑰使用（EKU），或完全沒有 Eku。</span><span class="sxs-lookup"><span data-stu-id="597f1-137">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="597f1-138">如規格所示，如果未指定任何 EKU，則所有 Eku 都會被視為有效。</span><span class="sxs-lookup"><span data-stu-id="597f1-138">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="597f1-139">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="597f1-139">ValidateValidityPeriod</span></span>

<span data-ttu-id="597f1-140">預設值：`true`</span><span class="sxs-lookup"><span data-stu-id="597f1-140">Default value: `true`</span></span>

<span data-ttu-id="597f1-141">這種檢查會驗證憑證是否在其有效期間內。</span><span class="sxs-lookup"><span data-stu-id="597f1-141">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="597f1-142">在每個要求上，處理常式可確保在其目前的會話期間，當憑證呈現時，其有效的憑證尚未到期。</span><span class="sxs-lookup"><span data-stu-id="597f1-142">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="597f1-143">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="597f1-143">RevocationFlag</span></span>

<span data-ttu-id="597f1-144">預設值：`X509RevocationFlag.ExcludeRoot`</span><span class="sxs-lookup"><span data-stu-id="597f1-144">Default value: `X509RevocationFlag.ExcludeRoot`</span></span>

<span data-ttu-id="597f1-145">指定要檢查鏈中哪些憑證以進行撤銷的旗標。</span><span class="sxs-lookup"><span data-stu-id="597f1-145">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="597f1-146">只有當憑證連結至根憑證時，才會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="597f1-146">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="597f1-147">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="597f1-147">RevocationMode</span></span>

<span data-ttu-id="597f1-148">預設值：`X509RevocationMode.Online`</span><span class="sxs-lookup"><span data-stu-id="597f1-148">Default value: `X509RevocationMode.Online`</span></span>

<span data-ttu-id="597f1-149">指定撤銷檢查執行方式的旗標。</span><span class="sxs-lookup"><span data-stu-id="597f1-149">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="597f1-150">當您連線到憑證授權單位單位時，指定線上檢查可能會造成長時間的延遲。</span><span class="sxs-lookup"><span data-stu-id="597f1-150">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="597f1-151">只有當憑證連結至根憑證時，才會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="597f1-151">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="597f1-152">我可以將我的應用程式設定為只在特定路徑上要求憑證嗎？</span><span class="sxs-lookup"><span data-stu-id="597f1-152">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="597f1-153">這是不可能的。</span><span class="sxs-lookup"><span data-stu-id="597f1-153">This isn't possible.</span></span> <span data-ttu-id="597f1-154">請記住，憑證交換已完成 HTTPS 交談的開頭，它是由伺服器在該連線上收到第一個要求之前完成，因此不可能根據任何要求欄位來界定範圍。</span><span class="sxs-lookup"><span data-stu-id="597f1-154">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="597f1-155">處理程式事件</span><span class="sxs-lookup"><span data-stu-id="597f1-155">Handler events</span></span>

<span data-ttu-id="597f1-156">此處理程式有兩個事件：</span><span class="sxs-lookup"><span data-stu-id="597f1-156">The handler has two events:</span></span>

* <span data-ttu-id="597f1-157">如果在驗證期間發生例外狀況，則會呼叫 `OnAuthenticationFailed` &ndash;，並可讓您做出回應。</span><span class="sxs-lookup"><span data-stu-id="597f1-157">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="597f1-158">在驗證憑證後呼叫 &ndash; `OnCertificateValidated`，通過驗證並已建立預設主體。</span><span class="sxs-lookup"><span data-stu-id="597f1-158">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="597f1-159">此事件可讓您執行自己的驗證，並增強或取代主體。</span><span class="sxs-lookup"><span data-stu-id="597f1-159">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="597f1-160">範例包括：</span><span class="sxs-lookup"><span data-stu-id="597f1-160">For examples include:</span></span>
  * <span data-ttu-id="597f1-161">判斷您的服務是否知道憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-161">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="597f1-162">建立您自己的主體。</span><span class="sxs-lookup"><span data-stu-id="597f1-162">Constructing your own principal.</span></span> <span data-ttu-id="597f1-163">請考慮 `Startup.ConfigureServices` 中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="597f1-163">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="597f1-164">如果您發現輸入憑證不符合您的額外驗證，請呼叫 `context.Fail("failure reason")`，但發生失敗原因。</span><span class="sxs-lookup"><span data-stu-id="597f1-164">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="597f1-165">針對實際的功能，您可能會想要呼叫在相依性插入中註冊的服務，而這些相依性會連接到資料庫或其他類型的使用者存放區。</span><span class="sxs-lookup"><span data-stu-id="597f1-165">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="597f1-166">使用傳入委派的內容存取您的服務。</span><span class="sxs-lookup"><span data-stu-id="597f1-166">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="597f1-167">請考慮 `Startup.ConfigureServices` 中的下列範例：</span><span class="sxs-lookup"><span data-stu-id="597f1-167">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="597f1-168">就概念而言，驗證憑證是一項授權考慮。</span><span class="sxs-lookup"><span data-stu-id="597f1-168">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="597f1-169">例如，在授權原則中新增核取簽發者或指紋，而不是在 `OnCertificateValidated`內，是完全可接受的。</span><span class="sxs-lookup"><span data-stu-id="597f1-169">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="597f1-170">將您的主機設定為需要憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-170">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="597f1-171">Kestrel</span><span class="sxs-lookup"><span data-stu-id="597f1-171">Kestrel</span></span>

<span data-ttu-id="597f1-172">在*Program.cs*中，設定 Kestrel，如下所示：</span><span class="sxs-lookup"><span data-stu-id="597f1-172">In *Program.cs*, configure Kestrel as follows:</span></span>

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
                o.ConfigureHttpsDefaults(o => 
            o.ClientCertificateMode = 
                ClientCertificateMode.RequireCertificate);
            });
        });
}
```

> [!NOTE]
> <span data-ttu-id="597f1-173">在呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*>**之前**呼叫 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> 所建立的端點不會套用預設值。</span><span class="sxs-lookup"><span data-stu-id="597f1-173">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="iis"></a><span data-ttu-id="597f1-174">IIS</span><span class="sxs-lookup"><span data-stu-id="597f1-174">IIS</span></span>

<span data-ttu-id="597f1-175">在 IIS 管理員中完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="597f1-175">Complete the following steps in IIS Manager:</span></span>

1. <span data-ttu-id="597f1-176">從 [**連接**] 索引標籤中選取您的網站。</span><span class="sxs-lookup"><span data-stu-id="597f1-176">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="597f1-177">按兩下 [**功能] 視圖**視窗中的 [ **SSL 設定**] 選項。</span><span class="sxs-lookup"><span data-stu-id="597f1-177">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="597f1-178">勾選 [**需要 SSL** ] 核取方塊，然後選取 [**用戶端憑證**] 區段中的 [**需要**] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="597f1-178">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="597f1-180">Azure 和自訂 web proxy</span><span class="sxs-lookup"><span data-stu-id="597f1-180">Azure and custom web proxies</span></span>

<span data-ttu-id="597f1-181">如需如何設定憑證轉送中介軟體的詳細說明，請參閱[主機和部署檔](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)。</span><span class="sxs-lookup"><span data-stu-id="597f1-181">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>

### <a name="use-certificate-authentication-in-azure-web-apps"></a><span data-ttu-id="597f1-182">在 Azure Web Apps 中使用憑證驗證</span><span class="sxs-lookup"><span data-stu-id="597f1-182">Use certificate authentication in Azure Web Apps</span></span>

<span data-ttu-id="597f1-183">Azure 不需要轉送設定。</span><span class="sxs-lookup"><span data-stu-id="597f1-183">No forwarding configuration is required for Azure.</span></span> <span data-ttu-id="597f1-184">憑證轉送中介軟體已設定此功能。</span><span class="sxs-lookup"><span data-stu-id="597f1-184">This is already setup in the certificate forwarding middleware.</span></span>

> [!NOTE]
> <span data-ttu-id="597f1-185">這需要 CertificateForwardingMiddleware 存在。</span><span class="sxs-lookup"><span data-stu-id="597f1-185">This requires that the CertificateForwardingMiddleware is present.</span></span>

### <a name="use-certificate-authentication-in-custom-web-proxies"></a><span data-ttu-id="597f1-186">在自訂 web proxy 中使用憑證驗證</span><span class="sxs-lookup"><span data-stu-id="597f1-186">Use certificate authentication in custom web proxies</span></span>

<span data-ttu-id="597f1-187">`AddCertificateForwarding` 方法是用來指定：</span><span class="sxs-lookup"><span data-stu-id="597f1-187">The `AddCertificateForwarding` method is used to specify:</span></span>

* <span data-ttu-id="597f1-188">用戶端標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="597f1-188">The client header name.</span></span>
* <span data-ttu-id="597f1-189">如何載入憑證（使用 `HeaderConverter` 屬性）。</span><span class="sxs-lookup"><span data-stu-id="597f1-189">How the certificate is to be loaded (using the `HeaderConverter` property).</span></span>

<span data-ttu-id="597f1-190">在自訂的 web proxy 中，憑證會以自訂要求標頭的形式傳遞，例如 `X-SSL-CERT`。</span><span class="sxs-lookup"><span data-stu-id="597f1-190">In custom web proxies, the certificate is passed as a custom request header, for example `X-SSL-CERT`.</span></span> <span data-ttu-id="597f1-191">若要使用它，請在 `Startup.ConfigureServices`中設定憑證轉送：</span><span class="sxs-lookup"><span data-stu-id="597f1-191">To use it, configure certificate forwarding in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCertificateForwarding(options =>
    {
        options.CertificateHeader = "X-SSL-CERT";
        options.HeaderConverter = (headerValue) =>
        {
            X509Certificate2 clientCertificate = null;
        
            if(!string.IsNullOrWhiteSpace(headerValue))
            {
                byte[] bytes = StringToByteArray(headerValue);
                clientCertificate = new X509Certificate2(bytes);
            }

            return clientCertificate;
        };
    });
}

private static byte[] StringToByteArray(string hex)
{
    int NumberChars = hex.Length;
    byte[] bytes = new byte[NumberChars / 2];

    for (int i = 0; i < NumberChars; i += 2)
    {
        bytes[i / 2] = Convert.ToByte(hex.Substring(i, 2), 16);
    }

    return bytes;
}
```

<span data-ttu-id="597f1-192">然後 `Startup.Configure` 方法會新增中介軟體。</span><span class="sxs-lookup"><span data-stu-id="597f1-192">The `Startup.Configure` method then adds the middleware.</span></span> <span data-ttu-id="597f1-193">在呼叫 `UseAuthentication` 和 `UseAuthorization`之前，會呼叫 `UseCertificateForwarding`：</span><span class="sxs-lookup"><span data-stu-id="597f1-193">`UseCertificateForwarding` is called before the calls to `UseAuthentication` and `UseAuthorization`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseRouting();

    app.UseCertificateForwarding();
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="597f1-194">個別的類別可以用來執行驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="597f1-194">A separate class can be used to implement validation logic.</span></span> <span data-ttu-id="597f1-195">因為此範例中使用了相同的自我簽署憑證，請確定只能使用您的憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-195">Because the same self-signed certificate is used in this example, ensure that only your certificate can be used.</span></span> <span data-ttu-id="597f1-196">驗證用戶端憑證和伺服器憑證的指紋是否相符，否則就可以使用任何憑證，而且就足以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="597f1-196">Validate that the thumbprints of both the client certificate and the server certificate match, otherwise any certificate can be used and will be enough to authenticate.</span></span> <span data-ttu-id="597f1-197">這會在 `AddCertificate` 方法內使用。</span><span class="sxs-lookup"><span data-stu-id="597f1-197">This would be used inside the `AddCertificate` method.</span></span> <span data-ttu-id="597f1-198">如果您使用的是中繼或子系憑證，您也可以在這裡驗證主旨或簽發者。</span><span class="sxs-lookup"><span data-stu-id="597f1-198">You could also validate the subject or the issuer here if you're using intermediate or child certificates.</span></span>

```csharp
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            // Do not hardcode passwords in production code
            // Use thumbprint or key vault
            var cert = new X509Certificate2(
                Path.Combine("sts_dev_cert.pfx"), "1234");

            if (clientCertificate.Thumbprint == cert.Thumbprint)
            {
                return true;
            }

            return false;
        }
    }
}
```

#### <a name="implement-an-httpclient-using-a-certificate-and-the-httpclienthandler"></a><span data-ttu-id="597f1-199">使用憑證和 HttpClientHandler 來執行 HttpClient</span><span class="sxs-lookup"><span data-stu-id="597f1-199">Implement an HttpClient using a certificate and the HttpClientHandler</span></span>

<span data-ttu-id="597f1-200">HttpClientHandler 可以直接加入 HttpClient 類別的函式中。</span><span class="sxs-lookup"><span data-stu-id="597f1-200">The HttpClientHandler could be added directly in the constructor of the HttpClient class.</span></span> <span data-ttu-id="597f1-201">建立 HttpClient 的實例時，請務必小心。</span><span class="sxs-lookup"><span data-stu-id="597f1-201">Care should be taken when creating instances of the HttpClient.</span></span> <span data-ttu-id="597f1-202">然後，HttpClient 會在每個要求中傳送憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-202">The HttpClient will then send the certificate with each request.</span></span>

```csharp
private async Task<JsonDocument> GetApiDataUsingHttpClientHandler()
{
    var cert = new X509Certificate2(Path.Combine(_environment.ContentRootPath, "sts_dev_cert.pfx"), "1234");
    var handler = new HttpClientHandler();
    handler.ClientCertificates.Add(cert);
    var client = new HttpClient(handler);
     
    var request = new HttpRequestMessage()
    {
        RequestUri = new Uri("https://localhost:44379/api/values"),
        Method = HttpMethod.Get,
    };
    var response = await client.SendAsync(request);
    if (response.IsSuccessStatusCode)
    {
        var responseContent = await response.Content.ReadAsStringAsync();
        var data = JsonDocument.Parse(responseContent);
        return data;
    }
 
    throw new ApplicationException($"Status code: {response.StatusCode}, Error: {response.ReasonPhrase}");
}
```

#### <a name="implement-an-httpclient-using-a-certificate-and-a-named-httpclient-from-ihttpclientfactory"></a><span data-ttu-id="597f1-203">使用憑證和 IHttpClientFactory 中名為 HttpClient 的來執行 HttpClient</span><span class="sxs-lookup"><span data-stu-id="597f1-203">Implement an HttpClient using a certificate and a named HttpClient from IHttpClientFactory</span></span> 

<span data-ttu-id="597f1-204">在下列範例中，會使用處理程式中的 ClientCertificates 屬性，將用戶端憑證新增至 HttpClientHandler。</span><span class="sxs-lookup"><span data-stu-id="597f1-204">In the following example, a client certificate is added to a HttpClientHandler using the ClientCertificates property from the handler.</span></span> <span data-ttu-id="597f1-205">然後，可以使用 ConfigurePrimaryHttpMessageHandler 方法，在 HttpClient 的命名實例中使用這個處理常式。</span><span class="sxs-lookup"><span data-stu-id="597f1-205">This handler can then be used in a named instance of a HttpClient using the ConfigurePrimaryHttpMessageHandler method.</span></span> <span data-ttu-id="597f1-206">這會在 ConfigureServices 方法的 Startup 類別中設定。</span><span class="sxs-lookup"><span data-stu-id="597f1-206">This is setup in the Startup class in the ConfigureServices method.</span></span>

```csharp
var clientCertificate = 
    new X509Certificate2(
      Path.Combine(_environment.ContentRootPath, "sts_dev_cert.pfx"), "1234");
 
var handler = new HttpClientHandler();
handler.ClientCertificates.Add(clientCertificate);
 
services.AddHttpClient("namedClient", c =>
{
}).ConfigurePrimaryHttpMessageHandler(() => handler);
```

<span data-ttu-id="597f1-207">然後，IHttpClientFactory 可以用來取得具有處理常式和憑證的已命名實例。</span><span class="sxs-lookup"><span data-stu-id="597f1-207">The IHttpClientFactory can then be used to get the named instance with the handler and the certificate.</span></span> <span data-ttu-id="597f1-208">使用 Startup 類別中定義之用戶端名稱的 CreateClient 方法，會用來取得實例。</span><span class="sxs-lookup"><span data-stu-id="597f1-208">The CreateClient method with the name of the client defined in the Startup class is used to get the instance.</span></span> <span data-ttu-id="597f1-209">您可以視需要使用用戶端來傳送 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="597f1-209">The HTTP request can be sent using the client as required.</span></span>

```csharp
private readonly IHttpClientFactory _clientFactory;
 
public ApiService(IHttpClientFactory clientFactory)
{
    _clientFactory = clientFactory;
}
 
private async Task<JsonDocument> GetApiDataWithNamedClient()
{
    var client = _clientFactory.CreateClient("namedClient");
 
    var request = new HttpRequestMessage()
    {
        RequestUri = new Uri("https://localhost:44379/api/values"),
        Method = HttpMethod.Get,
    };
    var response = await client.SendAsync(request);
    if (response.IsSuccessStatusCode)
    {
        var responseContent = await response.Content.ReadAsStringAsync();
        var data = JsonDocument.Parse(responseContent);
        return data;
    }
 
    throw new ApplicationException($"Status code: {response.StatusCode}, Error: {response.ReasonPhrase}");
}
```

<span data-ttu-id="597f1-210">如果將正確的憑證傳送至伺服器，則會傳回資料。</span><span class="sxs-lookup"><span data-stu-id="597f1-210">If the correct certificate is sent to the server, the data is returned.</span></span> <span data-ttu-id="597f1-211">如果未傳送憑證或錯誤的憑證，則會傳回 HTTP 403 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="597f1-211">If no certificate or the wrong certificate is sent, an HTTP 403 status code is returned.</span></span>

### <a name="create-certificates-in-powershell"></a><span data-ttu-id="597f1-212">在 PowerShell 中建立憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-212">Create certificates in PowerShell</span></span>

<span data-ttu-id="597f1-213">建立憑證是設定此流程最困難的部分。</span><span class="sxs-lookup"><span data-stu-id="597f1-213">Creating the certificates is the hardest part in setting up this flow.</span></span> <span data-ttu-id="597f1-214">您可以使用 `New-SelfSignedCertificate` PowerShell Cmdlet 來建立根憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-214">A root certificate can be created using the `New-SelfSignedCertificate` PowerShell cmdlet.</span></span> <span data-ttu-id="597f1-215">建立憑證時，請使用強式密碼。</span><span class="sxs-lookup"><span data-stu-id="597f1-215">When creating the certificate, use a strong password.</span></span> <span data-ttu-id="597f1-216">請務必加入 `KeyUsageProperty` 參數和 `KeyUsage` 參數，如下所示。</span><span class="sxs-lookup"><span data-stu-id="597f1-216">It's important to add the `KeyUsageProperty` parameter and the `KeyUsage` parameter as shown.</span></span>

#### <a name="create-root-ca"></a><span data-ttu-id="597f1-217">建立根 CA</span><span class="sxs-lookup"><span data-stu-id="597f1-217">Create root CA</span></span>

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

> [!NOTE]
> <span data-ttu-id="597f1-218">`-DnsName` 參數值必須符合應用程式的部署目標。</span><span class="sxs-lookup"><span data-stu-id="597f1-218">The `-DnsName` parameter value must match the deployment target of the app.</span></span> <span data-ttu-id="597f1-219">例如，"localhost" 用於開發。</span><span class="sxs-lookup"><span data-stu-id="597f1-219">For example, "localhost" for development.</span></span>

#### <a name="install-in-the-trusted-root"></a><span data-ttu-id="597f1-220">在受信任的根目錄中安裝</span><span class="sxs-lookup"><span data-stu-id="597f1-220">Install in the trusted root</span></span>

<span data-ttu-id="597f1-221">您的主機系統上必須信任根憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-221">The root certificate needs to be trusted on your host system.</span></span> <span data-ttu-id="597f1-222">預設不會信任憑證授權單位單位所建立的根憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-222">A root certificate which was not created by a certificate authority won't be trusted by default.</span></span> <span data-ttu-id="597f1-223">下列連結會說明如何在 Windows 上完成這項作業：</span><span class="sxs-lookup"><span data-stu-id="597f1-223">The following link explains how this can be accomplished on Windows:</span></span>

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a><span data-ttu-id="597f1-224">中繼憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-224">Intermediate certificate</span></span>

<span data-ttu-id="597f1-225">現在可以從根憑證建立中繼憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-225">An intermediate certificate can now be created from the root certificate.</span></span> <span data-ttu-id="597f1-226">並非所有使用案例都需要這麼做，但您可能需要建立許多憑證，或需要啟用或停用憑證群組。</span><span class="sxs-lookup"><span data-stu-id="597f1-226">This isn't required for all use cases, but you might need to create many certificates or need to activate or disable groups of certificates.</span></span> <span data-ttu-id="597f1-227">必須要有 `TextExtension` 參數，才能在憑證的基本條件約束中設定路徑長度。</span><span class="sxs-lookup"><span data-stu-id="597f1-227">The `TextExtension` parameter is required to set the path length in the basic constraints of the certificate.</span></span>

<span data-ttu-id="597f1-228">接著，您可以將中繼憑證新增至 Windows 主機系統中受信任的中繼憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-228">The intermediate certificate can then be added to the trusted intermediate certificate in the Windows host system.</span></span>

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a><span data-ttu-id="597f1-229">從中繼憑證建立子憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-229">Create child certificate from intermediate certificate</span></span>

<span data-ttu-id="597f1-230">您可以從中繼憑證建立子系憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-230">A child certificate can be created from the intermediate certificate.</span></span> <span data-ttu-id="597f1-231">這是終端實體，不需要建立更多子憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-231">This is the end entity and doesn't need to create more child certificates.</span></span>

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a><span data-ttu-id="597f1-232">從根憑證建立子憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-232">Create child certificate from root certificate</span></span>

<span data-ttu-id="597f1-233">子憑證也可以直接從根憑證建立。</span><span class="sxs-lookup"><span data-stu-id="597f1-233">A child certificate can also be created from the root certificate directly.</span></span> 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a><span data-ttu-id="597f1-234">範例根-中繼憑證-憑證</span><span class="sxs-lookup"><span data-stu-id="597f1-234">Example root - intermediate certificate - certificate</span></span>

```powershell
$mypwdroot = ConvertTo-SecureString -String "1234" -Force -AsPlainText
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

Get-ChildItem -Path cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwdroot

Export-Certificate -Cert cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 -FilePath root_ca_dev_damienbod.crt

$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\0C89639E4E2998A93E423F919B36D4009A0F9991 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 -FilePath child_a_dev_damienbod.crt

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\BA9BF91ED35538A01375EFC212A2F46104B33A44 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_b_from_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_b_from_a_dev_damienbod.com" 

Get-ChildItem -Path cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_b_from_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A -FilePath child_b_from_a_dev_damienbod.crt
```

<span data-ttu-id="597f1-235">使用根、中繼或子系憑證時，可以視需要使用指紋或 PublicKey 來驗證憑證。</span><span class="sxs-lookup"><span data-stu-id="597f1-235">When using the root, intermediate, or child certificates, the certificates can be validated using the Thumbprint or PublicKey as required.</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService 
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            return CheckIfThumbprintIsValid(clientCertificate);
        }

        private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
        {
            var listOfValidThumbprints = new List<string>
            {
                "141594A0AE38CBBECED7AF680F7945CD51D8F28A",
                "0C89639E4E2998A93E423F919B36D4009A0F9991",
                "BA9BF91ED35538A01375EFC212A2F46104B33A44"
            };

            if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
            {
                return true;
            }

            return false;
        }
    }
}
```
