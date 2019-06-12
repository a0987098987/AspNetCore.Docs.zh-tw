---
title: 在 ASP.NET Core 中設定憑證驗證
author: blowdart
description: 了解如何設定 ASP.NET Core 中的憑證驗證的 IIS 和 HTTP.sys。
monikerRange: '>= aspnetcore-3.0'
ms.author: barry.dorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 37567128531187bfe7dd6c9f5aa990226e70f35f
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837535"
---
# <a name="overview"></a><span data-ttu-id="64b62-103">總覽</span><span class="sxs-lookup"><span data-stu-id="64b62-103">Overview</span></span>

<span data-ttu-id="64b62-104">`Microsoft.AspNetCore.Authentication.Certificate` 包含實作類似於[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)適用於 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="64b62-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="64b62-105">憑證驗證會在之前曾經到達 ASP.NET Core，發生在 TLS 層級，長時間。</span><span class="sxs-lookup"><span data-stu-id="64b62-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="64b62-106">更精確地說，這是驗證憑證，然後提供您的事件，您可以在其中解析該憑證的驗證處理常式`ClaimsPrincipal`。</span><span class="sxs-lookup"><span data-stu-id="64b62-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="64b62-107">[設定您的主機](#configure-your-host-to-require-certificates)進行憑證驗證，是 IIS，Kestrel，Azure Web 應用程式，或其他任何您使用。</span><span class="sxs-lookup"><span data-stu-id="64b62-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="64b62-108">開始使用</span><span class="sxs-lookup"><span data-stu-id="64b62-108">Get started</span></span>

<span data-ttu-id="64b62-109">取得的 HTTPS 憑證，請套用它，並[設定您的主機](#configure-your-host-to-require-certificates)以要求憑證。</span><span class="sxs-lookup"><span data-stu-id="64b62-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="64b62-110">Web 應用程式中加入的參考`Microsoft.AspNetCore.Authentication.Certificate`封裝。</span><span class="sxs-lookup"><span data-stu-id="64b62-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="64b62-111">然後在`Startup.Configure`方法中，呼叫`app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);`與您的選項，提供的委派`OnCertificateValidated`來執行任何增補的驗證與要求一起傳送的用戶端憑證。</span><span class="sxs-lookup"><span data-stu-id="64b62-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="64b62-112">將該資訊化為`ClaimsPrincipal`並將它設定在`context.Principal`屬性。</span><span class="sxs-lookup"><span data-stu-id="64b62-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="64b62-113">如果驗證失敗時，這個處理常式會傳回`403 (Forbidden)`回應而`401 (Unauthorized)`，跟您預期的一樣。</span><span class="sxs-lookup"><span data-stu-id="64b62-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="64b62-114">原因是驗證應該在初始的 TLS 連線期間發生。</span><span class="sxs-lookup"><span data-stu-id="64b62-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="64b62-115">到達這個處理常式時，就太遲了。</span><span class="sxs-lookup"><span data-stu-id="64b62-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="64b62-116">沒有任何方法可將連接從匿名連線升級成使用憑證。</span><span class="sxs-lookup"><span data-stu-id="64b62-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="64b62-117">也加入`app.UseAuthentication();`在`Startup.Configure`方法。</span><span class="sxs-lookup"><span data-stu-id="64b62-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="64b62-118">否則 HttpContext.User 將不會設定`ClaimsPrincipal`從憑證建立。</span><span class="sxs-lookup"><span data-stu-id="64b62-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="64b62-119">例如:</span><span class="sxs-lookup"><span data-stu-id="64b62-119">For example:</span></span>

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

<span data-ttu-id="64b62-120">上述範例示範以新增憑證驗證的預設方式。</span><span class="sxs-lookup"><span data-stu-id="64b62-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="64b62-121">處理常式會建構使用常見的憑證內容的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="64b62-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="64b62-122">設定憑證驗證</span><span class="sxs-lookup"><span data-stu-id="64b62-122">Configure certificate validation</span></span>

<span data-ttu-id="64b62-123">`CertificateAuthenticationOptions`處理常式有一些內建的驗證，您應該將憑證執行的最小值驗證。</span><span class="sxs-lookup"><span data-stu-id="64b62-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="64b62-124">預設會啟用這些設定。</span><span class="sxs-lookup"><span data-stu-id="64b62-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="64b62-125">AllowedCertificateTypes = 鏈結，SelfSigned，或全部 (鏈結 |SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="64b62-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="64b62-126">這項檢查會驗證只有適當的憑證類型允許的。</span><span class="sxs-lookup"><span data-stu-id="64b62-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="64b62-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="64b62-127">ValidateCertificateUse</span></span>

<span data-ttu-id="64b62-128">這項檢查會驗證用戶端出示的憑證具有用戶端驗證完全擴充金鑰使用方法 (EKU) 或任何 Eku。</span><span class="sxs-lookup"><span data-stu-id="64b62-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="64b62-129">如規格所說，如果指定了沒有任何 EKU，然後所有 Eku 會被都視為有效。</span><span class="sxs-lookup"><span data-stu-id="64b62-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="64b62-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="64b62-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="64b62-131">這項檢查會驗證憑證的有效期內。</span><span class="sxs-lookup"><span data-stu-id="64b62-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="64b62-132">每個要求的處理常式可確保之前呈現時有效的憑證尚未過期其目前的工作階段期間。</span><span class="sxs-lookup"><span data-stu-id="64b62-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="64b62-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="64b62-133">RevocationFlag</span></span>

<span data-ttu-id="64b62-134">指定的憑證鏈結中的旗標會檢查已被撤銷。</span><span class="sxs-lookup"><span data-stu-id="64b62-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="64b62-135">當憑證要鏈結至根憑證時，只會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="64b62-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="64b62-136">RevocationMode</span><span class="sxs-lookup"><span data-stu-id="64b62-136">RevocationMode</span></span>

<span data-ttu-id="64b62-137">指定如何執行撤銷檢查的旗標。</span><span class="sxs-lookup"><span data-stu-id="64b62-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="64b62-138">指定線上檢查會導致長時間的延遲，而會連絡憑證授權單位。</span><span class="sxs-lookup"><span data-stu-id="64b62-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="64b62-139">當憑證要鏈結至根憑證時，只會執行撤銷檢查。</span><span class="sxs-lookup"><span data-stu-id="64b62-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="64b62-140">可以設定我的應用程式，以要求只能在特定路徑上的憑證嗎？</span><span class="sxs-lookup"><span data-stu-id="64b62-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="64b62-141">這不可能。</span><span class="sxs-lookup"><span data-stu-id="64b62-141">This isn't possible.</span></span> <span data-ttu-id="64b62-142">請記住，HTTPS 交談開始時，它由伺服器因此它無法根據要求的任何欄位的範圍在該連接上收到的第一個要求之前完成憑證交換。</span><span class="sxs-lookup"><span data-stu-id="64b62-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="64b62-143">處理常式事件</span><span class="sxs-lookup"><span data-stu-id="64b62-143">Handler events</span></span>

<span data-ttu-id="64b62-144">處理常式有兩個事件：</span><span class="sxs-lookup"><span data-stu-id="64b62-144">The handler has two events:</span></span>

* <span data-ttu-id="64b62-145">`OnAuthenticationFailed` &ndash; 如果在驗證期間發生例外狀況，並可讓您回應，呼叫。</span><span class="sxs-lookup"><span data-stu-id="64b62-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="64b62-146">`OnCertificateValidated` &ndash; 憑證經過驗證，通過驗證，且已建立預設的主體後，就會呼叫。</span><span class="sxs-lookup"><span data-stu-id="64b62-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="64b62-147">此事件可讓您執行您自己的驗證和擴充或取代主體。</span><span class="sxs-lookup"><span data-stu-id="64b62-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="64b62-148">如需範例包括：</span><span class="sxs-lookup"><span data-stu-id="64b62-148">For examples include:</span></span>
  * <span data-ttu-id="64b62-149">判斷您的服務是否已知憑證。</span><span class="sxs-lookup"><span data-stu-id="64b62-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="64b62-150">建構您自己的主體。</span><span class="sxs-lookup"><span data-stu-id="64b62-150">Constructing your own principal.</span></span> <span data-ttu-id="64b62-151">下列範例中的，請考慮`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64b62-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="64b62-152">如果您發現輸入的憑證不符合您額外的驗證，請呼叫`context.Fail("failure reason")`包含失敗原因。</span><span class="sxs-lookup"><span data-stu-id="64b62-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="64b62-153">實際的功能，您可能需要以呼叫服務，在連接到資料庫或其他類型的使用者存放區的相依性插入中註冊。</span><span class="sxs-lookup"><span data-stu-id="64b62-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="64b62-154">使用內容傳遞至您的委派，以存取您的服務。</span><span class="sxs-lookup"><span data-stu-id="64b62-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="64b62-155">下列範例中的，請考慮`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="64b62-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="64b62-156">就概念而言，憑證的驗證是授權考量。</span><span class="sxs-lookup"><span data-stu-id="64b62-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="64b62-157">例如，新增核取、 簽發者或授權原則，而不是內部的憑證指紋`OnCertificateValidated`，是完全可以接受。</span><span class="sxs-lookup"><span data-stu-id="64b62-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="64b62-158">設定您的主機需要憑證</span><span class="sxs-lookup"><span data-stu-id="64b62-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="64b62-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="64b62-159">Kestrel</span></span>

<span data-ttu-id="64b62-160">在  *Program.cs*，設定 Kestrel，如下所示：</span><span class="sxs-lookup"><span data-stu-id="64b62-160">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a><span data-ttu-id="64b62-161">IIS</span><span class="sxs-lookup"><span data-stu-id="64b62-161">IIS</span></span>

<span data-ttu-id="64b62-162">完成下列步驟在 [IIS 管理員] 中：</span><span class="sxs-lookup"><span data-stu-id="64b62-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="64b62-163">選取您的網站，從**連線** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="64b62-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="64b62-164">按兩下**SSL 設定**選項**功能檢視**視窗。</span><span class="sxs-lookup"><span data-stu-id="64b62-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="64b62-165">檢查**需要 SSL**核取方塊，然後選取**需要**中的選項按鈕**用戶端憑證**一節。</span><span class="sxs-lookup"><span data-stu-id="64b62-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![在 IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="64b62-167">Azure 和自訂的 web proxy</span><span class="sxs-lookup"><span data-stu-id="64b62-167">Azure and custom web proxies</span></span>

<span data-ttu-id="64b62-168">請參閱[裝載和部署文件](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)如何設定轉送中介軟體的憑證。</span><span class="sxs-lookup"><span data-stu-id="64b62-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
