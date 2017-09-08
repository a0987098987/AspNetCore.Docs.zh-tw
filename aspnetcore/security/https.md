---
title: "設定 HTTPS 進行開發的 ASP.NET Core"
author: Rick-Anderson
description: "示範如何開發 ASP.NET Core 2.0 中設定 HTTPS。"
keywords: "ASP.NET Core，SSL，HTTPS"
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: e06f4194d496b5b11aa867e66563bec317e735ff
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="bdafa-104">設定 HTTPS 進行開發的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bdafa-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="bdafa-105">本主題適用於 ASP.NET Core 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="bdafa-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="bdafa-106">您可以設定您的應用程式在開發期間使用 HTTPS，來模擬實際執行環境中的 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="bdafa-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="bdafa-107">可能需要啟用 HTTPS 啟用整合與各種身分識別提供者 (例如[Azure AD](https://azure.microsoft.com/services/active-directory)和[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c))。</span><span class="sxs-lookup"><span data-stu-id="bdafa-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="bdafa-108">如果您已安裝 Visual Studio 或 IIS Express、 IIS Express 開發憑證將會在 LocalMachine 憑證存放區的 windows。</span><span class="sxs-lookup"><span data-stu-id="bdafa-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="bdafa-109">您可以更新您的專案屬性，在 Visual Studio 中執行後置 IIS Express 時，請使用此憑證。</span><span class="sxs-lookup"><span data-stu-id="bdafa-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="bdafa-110">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="bdafa-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="bdafa-111">在左窗格中，選取**偵錯**。</span><span class="sxs-lookup"><span data-stu-id="bdafa-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="bdafa-112">請檢查**啟用 SSL**</span><span class="sxs-lookup"><span data-stu-id="bdafa-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="bdafa-113">複製 SSL URL，並將它貼入**應用程式 URL**</span><span class="sxs-lookup"><span data-stu-id="bdafa-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![偵錯 web 應用程式內容 索引標籤](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="bdafa-115">您可以使用 IIS Express 開發憑證，如果有的話，或建立新的憑證，供開發應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="bdafa-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="bdafa-116">開發憑證應在中設定`appsettings.Development.json`檔案，所以不用於生產環境：</span><span class="sxs-lookup"><span data-stu-id="bdafa-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

<span data-ttu-id="bdafa-117">應用程式，但在生產環境中執行此組態將會擲回例外狀況指出 「 沒有名為 'HTTPS' 設定為目前的環境 （生產環境） 中找到的憑證 」。</span><span class="sxs-lookup"><span data-stu-id="bdafa-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="bdafa-118">若要切換[環境](xref:fundamentals/environments)至`Development`，將`ASPNETCORE_ENVIRONMENT`環境變數，以`Development`。</span><span class="sxs-lookup"><span data-stu-id="bdafa-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="bdafa-119">如果沒有 IIS Express 開發安裝的憑證，您可以自行建立開發憑證。</span><span class="sxs-lookup"><span data-stu-id="bdafa-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="bdafa-120">Windows 上，您可以建立開發憑證，並且將它加入至目前使用者的受信任的根存放區中，在提升權限的提示字元中執行下列 PowerShell 命令：</span><span class="sxs-lookup"><span data-stu-id="bdafa-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="bdafa-121">Kestrel macOS 和 Linux 上</span><span class="sxs-lookup"><span data-stu-id="bdafa-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="bdafa-122">您可以設定為與所需的 IP 位址、 連接埠，以及憑證設定的端點，透過 HTTPS 接聽 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="bdafa-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="bdafa-123">憑證可以是已設定的內嵌，或在最上層`Certificates`區段，然後依名稱參考：</span><span class="sxs-lookup"><span data-stu-id="bdafa-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

<span data-ttu-id="bdafa-124">您可以在 macOS 和 Linux 上建立 HTTPS 使用的自我簽署的憑證[OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="bdafa-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="bdafa-125">一次`certificate.pfx`已產生檔案，設定 HTTPS 憑證，在您`appsettings.Development.json`檔案：</span><span class="sxs-lookup"><span data-stu-id="bdafa-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

<span data-ttu-id="bdafa-126">您也必須設定 「 憑證： HTTPS:Password"設定屬性來指定憑證的複雜密碼。</span><span class="sxs-lookup"><span data-stu-id="bdafa-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="bdafa-127">密碼不會以純文字方式進行儲存。</span><span class="sxs-lookup"><span data-stu-id="bdafa-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="bdafa-128">請參閱[安全存放裝置的應用程式密碼在開發期間](app-secrets.md)來適當處理憑證的複雜密碼。</span><span class="sxs-lookup"><span data-stu-id="bdafa-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="bdafa-129">您可以在 macOS[將憑證新增至您的金鑰鏈](https://support.apple.com/kb/PH20129?locale=en_US)和[變更其信任設定](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US)使其具備在開發期間針對 HTTPS 受信任。</span><span class="sxs-lookup"><span data-stu-id="bdafa-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="bdafa-130">若要將憑證新增至您的金鑰鏈 (相當於`CurrentUser/My`儲存在 Windows 上) 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bdafa-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="bdafa-131">然後再信任的憑證：</span><span class="sxs-lookup"><span data-stu-id="bdafa-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="bdafa-132">接著，您可以設定您的應用程式，就像這樣的開發工作中使用此憑證：</span><span class="sxs-lookup"><span data-stu-id="bdafa-132">You can then configure your app to use this certificate in development like this:</span></span>

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
