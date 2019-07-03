---
title: 在 ASP.NET Core 中設定憑證驗證
author: blowdart
description: 了解如何設定 ASP.NET Core 中的憑證驗證的 IIS 和 HTTP.sys。
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 8609c58265340da1d618135795915d6c49e750a3
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538726"
---
# <a name="overview"></a>總覽

`Microsoft.AspNetCore.Authentication.Certificate` 包含實作類似於[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)適用於 ASP.NET Core。 憑證驗證會在之前曾經到達 ASP.NET Core，發生在 TLS 層級，長時間。 更精確地說，這是驗證憑證，然後提供您的事件，您可以在其中解析該憑證的驗證處理常式`ClaimsPrincipal`。 

[設定您的主機](#configure-your-host-to-require-certificates)進行憑證驗證，是 IIS，Kestrel，Azure Web 應用程式，或其他任何您使用。

## <a name="get-started"></a>開始使用

取得的 HTTPS 憑證，請套用它，並[設定您的主機](#configure-your-host-to-require-certificates)以要求憑證。

Web 應用程式中加入的參考`Microsoft.AspNetCore.Authentication.Certificate`封裝。 然後在`Startup.Configure`方法中，呼叫`app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);`與您的選項，提供的委派`OnCertificateValidated`來執行任何增補的驗證與要求一起傳送的用戶端憑證。 將該資訊化為`ClaimsPrincipal`並將它設定在`context.Principal`屬性。

如果驗證失敗時，這個處理常式會傳回`403 (Forbidden)`回應而`401 (Unauthorized)`，跟您預期的一樣。 原因是驗證應該在初始的 TLS 連線期間發生。 到達這個處理常式時，就太遲了。 沒有任何方法可將連接從匿名連線升級成使用憑證。

也加入`app.UseAuthentication();`在`Startup.Configure`方法。 否則 HttpContext.User 將不會設定`ClaimsPrincipal`從憑證建立。 例如:

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

上述範例示範以新增憑證驗證的預設方式。 處理常式會建構使用常見的憑證內容的使用者主體。

## <a name="configure-certificate-validation"></a>設定憑證驗證

`CertificateAuthenticationOptions`處理常式有一些內建的驗證，您應該將憑證執行的最小值驗證。 預設會啟用這些設定。

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = 鏈結，SelfSigned，或全部 (鏈結 |SelfSigned)

這項檢查會驗證只有適當的憑證類型允許的。

### <a name="validatecertificateuse"></a>ValidateCertificateUse

這項檢查會驗證用戶端出示的憑證具有用戶端驗證完全擴充金鑰使用方法 (EKU) 或任何 Eku。 如規格所說，如果指定了沒有任何 EKU，然後所有 Eku 會被都視為有效。

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

這項檢查會驗證憑證的有效期內。 每個要求的處理常式可確保之前呈現時有效的憑證尚未過期其目前的工作階段期間。

### <a name="revocationflag"></a>RevocationFlag

指定的憑證鏈結中的旗標會檢查已被撤銷。

當憑證要鏈結至根憑證時，只會執行撤銷檢查。

### <a name="revocationmode"></a>RevocationMode

指定如何執行撤銷檢查的旗標。

指定線上檢查會導致長時間的延遲，而會連絡憑證授權單位。

當憑證要鏈結至根憑證時，只會執行撤銷檢查。

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>可以設定我的應用程式，以要求只能在特定路徑上的憑證嗎？

這不可能。 請記住，HTTPS 交談開始時，它由伺服器因此它無法根據要求的任何欄位的範圍在該連接上收到的第一個要求之前完成憑證交換。

## <a name="handler-events"></a>處理常式事件

處理常式有兩個事件：

* `OnAuthenticationFailed` &ndash; 如果在驗證期間發生例外狀況，並可讓您回應，呼叫。
* `OnCertificateValidated` &ndash; 憑證經過驗證，通過驗證，且已建立預設的主體後，就會呼叫。 此事件可讓您執行您自己的驗證和擴充或取代主體。 如需範例包括：
  * 判斷您的服務是否已知憑證。
  * 建構您自己的主體。 請考慮 `Startup.ConfigureServices` 中的下列範例：

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

如果您發現輸入的憑證不符合您額外的驗證，請呼叫`context.Fail("failure reason")`包含失敗原因。

實際的功能，您可能需要以呼叫服務，在連接到資料庫或其他類型的使用者存放區的相依性插入中註冊。 使用內容傳遞至您的委派，以存取您的服務。 請考慮 `Startup.ConfigureServices` 中的下列範例：

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

就概念而言，憑證的驗證是授權考量。 例如，新增核取、 簽發者或授權原則，而不是內部的憑證指紋`OnCertificateValidated`，是完全可以接受。

## <a name="configure-your-host-to-require-certificates"></a>設定您的主機需要憑證

### <a name="kestrel"></a>Kestrel

在  *Program.cs*，設定 Kestrel，如下所示：

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

### <a name="iis"></a>IIS

完成下列步驟在 [IIS 管理員] 中：

1. 選取您的網站，從**連線** 索引標籤。
1. 按兩下**SSL 設定**選項**功能檢視**視窗。
1. 檢查**需要 SSL**核取方塊，然後選取**需要**中的選項按鈕**用戶端憑證**一節。

![在 IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure 和自訂的 web proxy

請參閱[裝載和部署文件](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)如何設定轉送中介軟體的憑證。
