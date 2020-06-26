---
title: 在 ASP.NET Core 中設定憑證驗證
author: blowdart
description: 瞭解如何在 IIS 和 HTTP.sys 的 ASP.NET Core 中設定憑證驗證。
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 01/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/certauth
ms.openlocfilehash: 493046e288c6b1ccd8e41f15a8e6e532a10a4adc
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85403192"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a>在 ASP.NET Core 中設定憑證驗證

`Microsoft.AspNetCore.Authentication.Certificate`包含類似于 ASP.NET Core 的[憑證驗證](https://tools.ietf.org/html/rfc5246#section-7.4.4)的執行。 憑證驗證會在 TLS 層級進行，長時間才會到達 ASP.NET Core。 更準確地說，這是驗證憑證的驗證處理常式，並提供您可將該憑證解析成的事件 `ClaimsPrincipal` 。 

[設定您的伺服器](#configure-your-server-to-require-certificates)以進行憑證驗證，不論是 IIS、Kestrel、Azure Web Apps 或您所使用的任何其他方式。

## <a name="proxy-and-load-balancer-scenarios"></a>Proxy 和負載平衡器案例

憑證驗證是一種可設定狀態的案例，主要用於 proxy 或負載平衡器不會處理用戶端與伺服器之間的流量。 如果使用 proxy 或負載平衡器，憑證驗證僅適用于 proxy 或負載平衡器：

* 處理驗證。
* 將使用者驗證資訊傳遞給應用程式（例如，在要求標頭中），其作用於驗證資訊。

在使用 proxy 和負載平衡器的環境中，憑證驗證的替代方法是使用 OpenID Connect （OIDC） Active Directory 同盟服務（ADFS）。

## <a name="get-started"></a>開始使用

取得 HTTPS 憑證並加以套用，並[將您的伺服器設定](#configure-your-server-to-require-certificates)為需要憑證。

在您的 web 應用程式中，新增對 `Microsoft.AspNetCore.Authentication.Certificate` 封裝的參考。 然後在 `Startup.ConfigureServices` 方法中， `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` 使用您的選項呼叫，提供的委派， `OnCertificateValidated` 以在隨要求傳送的用戶端憑證上執行任何補充驗證。 將該資訊轉換成 `ClaimsPrincipal` ，並在屬性上設定它 `context.Principal` 。

如果驗證失敗，此處理程式 `403 (Forbidden)` 會傳迴響應，而不是 `401 (Unauthorized)` ，如您所預期。 其原因是必須在初始 TLS 連線期間進行驗證。 當它到達處理常式時，就太晚了。 沒有任何方法可將連接從匿名連接升級為具有憑證的連線。

此外，也會 `app.UseAuthentication();` 在方法中新增 `Startup.Configure` 。 否則， `HttpContext.User` 將不會設定為 `ClaimsPrincipal` 從憑證建立。 例如：

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

`CertificateAuthenticationOptions`處理常式有一些內建的驗證，這是您應該在憑證上執行的最小驗證。 預設會啟用這些設定。

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = 連鎖、Lnk-selfsigned 之類或 All （連鎖 |Lnk-selfsigned 之類

預設值：`CertificateTypes.Chained`

這種檢查會驗證是否只允許適當的憑證類型。 如果應用程式使用自我簽署憑證，則必須將此選項設定為 `CertificateTypes.All` 或 `CertificateTypes.SelfSigned` 。

### <a name="validatecertificateuse"></a>ValidateCertificateUse

預設值：`true`

這項檢查會驗證用戶端所提供的憑證是否有用戶端驗證擴充金鑰使用（EKU），或完全沒有 Eku。 如規格所示，如果未指定任何 EKU，則所有 Eku 都會被視為有效。

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

預設值：`true`

這種檢查會驗證憑證是否在其有效期間內。 在每個要求上，處理常式可確保在其目前的會話期間，當憑證呈現時，其有效的憑證尚未到期。

### <a name="revocationflag"></a>RevocationFlag

預設值：`X509RevocationFlag.ExcludeRoot`

指定要檢查鏈中哪些憑證以進行撤銷的旗標。

只有當憑證連結至根憑證時，才會執行撤銷檢查。

### <a name="revocationmode"></a>RevocationMode

預設值：`X509RevocationMode.Online`

指定撤銷檢查執行方式的旗標。

當您連線到憑證授權單位單位時，指定線上檢查可能會造成長時間的延遲。

只有當憑證連結至根憑證時，才會執行撤銷檢查。

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>我可以將我的應用程式設定為只在特定路徑上要求憑證嗎？

這是不可能的。 請記住，憑證交換已完成 HTTPS 交談的開頭，它是由伺服器在該連線上收到第一個要求之前完成，因此不可能根據任何要求欄位來界定範圍。

## <a name="handler-events"></a>處理程式事件

此處理程式有兩個事件：

* `OnAuthenticationFailed`：如果在驗證期間發生例外狀況，並可讓您做出回應，則呼叫。
* `OnCertificateValidated`：在驗證憑證後呼叫，通過驗證並建立預設主體。 此事件可讓您執行自己的驗證，並增強或取代主體。 範例包括：
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

如果您發現輸入憑證不符合您的額外驗證，請呼叫 `context.Fail("failure reason")` 失敗原因。

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

就概念而言，驗證憑證是一項授權考慮。 在授權原則中新增核取簽發者或指紋，而不是在內部 `OnCertificateValidated` ，是完全可接受的。

## <a name="configure-your-server-to-require-certificates"></a>將您的伺服器設定為需要憑證

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
                o.ConfigureHttpsDefaults(o => 
            o.ClientCertificateMode = 
                ClientCertificateMode.RequireCertificate);
            });
        });
}
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*>**在呼叫之前**呼叫所建立 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> 的端點，將不會套用預設值。

### <a name="iis"></a>IIS

在 IIS 管理員中完成下列步驟：

1. 從 [**連接**] 索引標籤中選取您的網站。
1. 按兩下 [**功能] 視圖**視窗中的 [ **SSL 設定**] 選項。
1. 勾選 [**需要 SSL** ] 核取方塊，然後選取 [**用戶端憑證**] 區段中的 [**需要**] 選項按鈕。

![IIS 中的用戶端憑證設定](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure 和自訂 web proxy

如需如何設定憑證轉送中介軟體的詳細說明，請參閱[主機和部署檔](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding)。

### <a name="use-certificate-authentication-in-azure-web-apps"></a>在 Azure Web Apps 中使用憑證驗證

Azure 不需要轉送設定。 憑證轉送中介軟體已設定此功能。

> [!NOTE]
> 這需要 CertificateForwardingMiddleware 存在。

### <a name="use-certificate-authentication-in-custom-web-proxies"></a>在自訂 web proxy 中使用憑證驗證

`AddCertificateForwarding`方法是用來指定：

* 用戶端標頭名稱。
* 如何載入憑證（使用 `HeaderConverter` 屬性）。

例如，在自訂的 web proxy 中，憑證會當做自訂要求標頭來傳遞 `X-SSL-CERT` 。 若要使用它，請在中設定憑證轉送 `Startup.ConfigureServices` ：

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

`Startup.Configure`然後，方法會新增中介軟體。 `UseCertificateForwarding`呼叫和之前，會先 `UseAuthentication` 呼叫 `UseAuthorization` ：

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

個別的類別可以用來執行驗證邏輯。 因為此範例中使用了相同的自我簽署憑證，請確定只能使用您的憑證。 驗證用戶端憑證和伺服器憑證的指紋是否相符，否則就可以使用任何憑證，而且就足以進行驗證。 這會在方法內使用 `AddCertificate` 。 如果您使用的是中繼或子系憑證，您也可以在這裡驗證主旨或簽發者。

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

#### <a name="implement-an-httpclient-using-a-certificate-and-the-httpclienthandler"></a>使用憑證和 HttpClientHandler 來執行 HttpClient

HttpClientHandler 可以直接加入 HttpClient 類別的函式中。 建立 HttpClient 的實例時，請務必小心。 然後，HttpClient 會在每個要求中傳送憑證。

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

#### <a name="implement-an-httpclient-using-a-certificate-and-a-named-httpclient-from-ihttpclientfactory"></a>使用憑證和 IHttpClientFactory 中名為 HttpClient 的來執行 HttpClient 

在下列範例中，會使用處理程式中的 ClientCertificates 屬性，將用戶端憑證新增至 HttpClientHandler。 然後，可以使用 ConfigurePrimaryHttpMessageHandler 方法，在 HttpClient 的命名實例中使用這個處理常式。 這會在 ConfigureServices 方法的 Startup 類別中設定。

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

然後，IHttpClientFactory 可以用來取得具有處理常式和憑證的已命名實例。 使用 Startup 類別中定義之用戶端名稱的 CreateClient 方法，會用來取得實例。 您可以視需要使用用戶端來傳送 HTTP 要求。

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

如果將正確的憑證傳送至伺服器，則會傳回資料。 如果未傳送憑證或錯誤的憑證，則會傳回 HTTP 403 狀態碼。

### <a name="create-certificates-in-powershell"></a>在 PowerShell 中建立憑證

建立憑證是設定此流程最困難的部分。 您可以使用 PowerShell Cmdlet 來建立根憑證 `New-SelfSignedCertificate` 。 建立憑證時，請使用強式密碼。 請務必新增 `KeyUsageProperty` 參數和 `KeyUsage` 參數，如下所示。

#### <a name="create-root-ca"></a>建立根 CA

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

> [!NOTE]
> `-DnsName`參數值必須符合應用程式的部署目標。 例如，"localhost" 用於開發。

#### <a name="install-in-the-trusted-root"></a>在受信任的根目錄中安裝

您的主機系統上必須信任根憑證。 預設不會信任憑證授權單位單位所建立的根憑證。 下列連結會說明如何在 Windows 上完成這項作業：

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a>中繼憑證

現在可以從根憑證建立中繼憑證。 並非所有使用案例都需要這麼做，但您可能需要建立許多憑證，或需要啟用或停用憑證群組。 `TextExtension`必須有參數，才能在憑證的基本條件約束中設定路徑長度。

接著，您可以將中繼憑證新增至 Windows 主機系統中受信任的中繼憑證。

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a>從中繼憑證建立子憑證

您可以從中繼憑證建立子系憑證。 這是終端實體，不需要建立更多子憑證。

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a>從根憑證建立子憑證

子憑證也可以直接從根憑證建立。 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a>範例根-中繼憑證-憑證

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

使用根、中繼或子系憑證時，可以視需要使用指紋或 PublicKey 來驗證憑證。

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

<a name="occ"></a>

## <a name="optional-client-certificates"></a>選擇性用戶端憑證

本節提供的資訊適用于必須使用憑證保護應用程式子集的應用程式。 例如， Razor 應用程式中的頁面或控制器可能需要用戶端憑證。 這會以用戶端憑證的形式呈現挑戰：
  
* 是 TLS 功能，不是 HTTP 功能。
* 會針對每個連接進行協商，而且必須在連接開始時進行協商，才能使用任何 HTTP 資料。 在連接開始時，只會知道伺服器名稱指示（SNI） &dagger; 。 用戶端和伺服器憑證會在第一次要求連線之前進行協商，而要求通常無法重新協商。 HTTP/2 禁止重新協商。

ASP.NET Core 5 preview 4 和更新版本為選用的用戶端憑證增加了更方便的支援。 如需詳細資訊，請參閱[選用憑證範例](https://github.com/dotnet/aspnetcore/tree/9ce4a970a21bace3fb262da9591ed52359309592/src/Security/Authentication/Certificate/samples/Certificate.Optional.Sample)。

下列方法支援選用的用戶端憑證：

* 設定網域和子域的系結：
  * 例如，在和上設定系 `contoso.com` 結 `myClient.contoso.com` 。 `contoso.com`主機不需要用戶端憑證，而是 `myClient.contoso.com` 。
  * 如需詳細資訊，請參閱：
    * [Kestrel](/fundamentals/servers/kestrel)：
      * [ListenOptions.UseHttps](xref:fundamentals/servers/kestrel#listenoptionsusehttps)
      * <xref:Microsoft.AspNetCore.Server.Kestrel.Https.HttpsConnectionAdapterOptions.ClientCertificateMode>
      * 注意 Kestrel 目前不支援一個系結上的多個 TLS 設定，您需要兩個具有唯一 Ip 或埠的系結。 請參閱https://github.com/dotnet/runtime/issues/31097
    * IIS
      * [裝載 IIS](xref:host-and-deploy/iis/index#create-the-iis-site)
      * [在 IIS 上設定安全性](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis#configure-ssl-settings-2)
    * Http.Sys：[設定 Windows Server](xref:fundamentals/servers/httpsys#configure-windows-server)
* 對於需要用戶端憑證且沒有此 web 應用程式的要求：
  * 使用受用戶端憑證保護的子域重新導向至相同的頁面。
  * 例如，將重新導向至 `myClient.contoso.com/requestedPage` 。 因為對的要求 `myClient.contoso.com/requestedPage` 是與不同的主機名稱 `contoso.com/requestedPage` ，用戶端會建立不同的連線，並提供用戶端憑證。
  * 如需詳細資訊，請參閱 <xref:security/authorization/introduction> 。

針對[此 GitHub 討論](https://github.com/dotnet/AspNetCore.Docs/issues/18720)問題中的選擇性用戶端憑證，留下問題、意見和其他意見反應。

&dagger;伺服器名稱指示（SNI）是 TLS 延伸模組，可在 SSL 協調中包含虛擬網域。 這實際上表示虛擬功能變數名稱或主機名稱，可以用來識別網路端點。
