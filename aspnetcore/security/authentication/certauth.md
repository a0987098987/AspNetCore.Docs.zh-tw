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
# <a name="configure-certificate-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定憑證驗證

`Microsoft.AspNetCore.Authentication.Certificate` 包含類似于 ASP.NET Core 的[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)的執行方式。 憑證驗證會在 TLS 層級進行，長時間才會到達 ASP.NET Core。 更準確地說，這是驗證憑證的驗證處理常式，並提供您可將該憑證解析成 `ClaimsPrincipal`的事件。 

[設定您的主機](#configure-your-host-to-require-certificates)以進行憑證驗證，其為 IIS、Kestrel、Azure Web Apps 或您所使用的任何其他。

## <a name="proxy-and-load-balancer-scenarios"></a>Proxy 和負載平衡器案例

憑證驗證是一種可設定狀態的案例，主要用於 proxy 或負載平衡器不會處理用戶端與伺服器之間的流量。 如果使用 proxy 或負載平衡器，憑證驗證僅適用于 proxy 或負載平衡器：

* 處理驗證。
* 將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。

在使用 proxy 和負載平衡器的環境中，憑證驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。

## <a name="get-started"></a>開始使用

取得 HTTPS 憑證並加以套用，並[將您的主機設定](#configure-your-host-to-require-certificates)為需要憑證。

在您的 web 應用程式中，新增 `Microsoft.AspNetCore.Authentication.Certificate` 封裝的參考。 然後在 `Startup.ConfigureServices` 方法中，使用您的選項呼叫 `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);`，並提供委派給 `OnCertificateValidated`，以在隨要求一起傳送的用戶端憑證上執行任何補充驗證。 將該資訊轉換成 `ClaimsPrincipal`，並在 `context.Principal` 屬性上進行設定。

如果驗證失敗，此處理程式會傳回 `403 (Forbidden)` 回應，而不是如您所預期的 `401 (Unauthorized)`。 其原因是必須在初始 TLS 連線期間進行驗證。 當它到達處理常式時，就太晚了。 沒有任何方法可將連接從匿名連接升級為具有憑證的連線。

此外，也請在 `Startup.Configure` 方法中新增 `app.UseAuthentication();`。 否則，HttpCoNtext 使用者將不會設定為從憑證建立 `ClaimsPrincipal`。 例如:

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

上述範例示範新增憑證驗證的預設方式。 處理常式會使用一般憑證屬性來建立使用者主體。

## <a name="configure-certificate-validation"></a>設定憑證驗證

`CertificateAuthenticationOptions` 處理常式有一些內建的驗證，這是您應該在憑證上執行的最小驗證。 預設會啟用這些設定。

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = 連鎖、Lnk-selfsigned 之類或 All （連鎖 |Lnk-selfsigned 之類

這種檢查會驗證是否只允許適當的憑證類型。

### <a name="validatecertificateuse"></a>ValidateCertificateUse

這項檢查會驗證用戶端所提供的憑證是否有用戶端驗證擴充金鑰使用（EKU），或完全沒有 Eku。 如規格所示，如果未指定任何 EKU，則所有 Eku 都會被視為有效。

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

這種檢查會驗證憑證是否在其有效期間內。 在每個要求上，處理常式可確保在其目前的會話期間，當憑證呈現時，其有效的憑證尚未到期。

### <a name="revocationflag"></a>RevocationFlag

指定要檢查鏈中哪些憑證以進行撤銷的旗標。

只有當憑證連結至根憑證時，才會執行撤銷檢查。

### <a name="revocationmode"></a>RevocationMode

指定撤銷檢查執行方式的旗標。

當您連線到憑證授權單位單位時，指定線上檢查可能會造成長時間的延遲。

只有當憑證連結至根憑證時，才會執行撤銷檢查。

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>我可以將我的應用程式設定為只在特定路徑上要求憑證嗎？

這是不可能的。 請記住，憑證交換已完成 HTTPS 交談的開頭，它是由伺服器在該連線上收到第一個要求之前完成，因此不可能根據任何要求欄位來界定範圍。

## <a name="handler-events"></a>處理程式事件

此處理程式有兩個事件：

* 如果在驗證期間發生例外狀況，則會呼叫 `OnAuthenticationFailed` &ndash;，並可讓您做出回應。
* 在驗證憑證後呼叫 &ndash; `OnCertificateValidated`，通過驗證並已建立預設主體。 此事件可讓您執行自己的驗證，並增強或取代主體。 範例包括：
  * 判斷您的服務是否知道憑證。
  * 建立您自己的主體。 請考慮 `Startup.ConfigureServices` 中的下列範例：

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

如果您發現輸入憑證不符合您的額外驗證，請呼叫 `context.Fail("failure reason")`，但發生失敗原因。

針對實際的功能，您可能會想要呼叫在相依性插入中註冊的服務，而這些相依性會連接到資料庫或其他類型的使用者存放區。 使用傳入委派的內容存取您的服務。 請考慮 `Startup.ConfigureServices` 中的下列範例：

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

就概念而言，驗證憑證是一項授權考慮。 例如，在授權原則中新增核取簽發者或指紋，而不是在 `OnCertificateValidated`內，是完全可接受的。

## <a name="configure-your-host-to-require-certificates"></a>將您的主機設定為需要憑證

### <a name="kestrel"></a>Kestrel

在*Program.cs*中，設定 Kestrel，如下所示：

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

### <a name="iis"></a>IIS

在 IIS 管理員中完成下列步驟：

1. 從 [**連接**] 索引標籤中選取您的網站。
1. 按兩下 [**功能] 視圖**視窗中的 [ **SSL 設定**] 選項。
1. 勾選 [**需要 SSL** ] 核取方塊，然後選取 [**用戶端憑證**] 區段中的 [**需要**] 選項按鈕。

![IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure 和自訂 web proxy

如需如何設定憑證轉送中介軟體的詳細說明，請參閱[主機和部署檔](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)。
