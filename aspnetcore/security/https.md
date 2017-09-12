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
ms.openlocfilehash: 7c366ffbac71152c2f29901ff12bac2962e83e3e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>設定 HTTPS 進行開發的 ASP.NET Core

> [!NOTE] 
> 本主題適用於 ASP.NET Core 2.0 Preview 1

您可以設定您的應用程式在開發期間使用 HTTPS，來模擬實際執行環境中的 HTTPS。 可能需要啟用 HTTPS 啟用整合與各種身分識別提供者 (例如[Azure AD](https://azure.microsoft.com/services/active-directory)和[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/))。

<a name="iisxpress"></a>

如果您已安裝 Visual Studio 或 IIS Express、 IIS Express 開發憑證將會在 LocalMachine 憑證存放區的 windows。 您可以更新您的專案屬性，在 Visual Studio 中執行後置 IIS Express 時，請使用此憑證。

   * 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。
   * 在左窗格中，選取**偵錯**。
   * 請檢查**啟用 SSL**
   * 複製 SSL URL，並將它貼入**應用程式 URL**

![偵錯 web 應用程式內容 索引標籤](enforcing-ssl/_static/ssl.png)

您可以使用 IIS Express 開發憑證，如果有的話，或建立新的憑證，供開發應用程式開發。 開發憑證應在中設定`appsettings.Development.json`檔案，所以不用於生產環境：

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

應用程式，但在生產環境中執行此組態將會擲回例外狀況指出 「 沒有名為 'HTTPS' 設定為目前的環境 （生產環境） 中找到的憑證 」。 若要切換[環境](xref:fundamentals/environments)至`Development`，將`ASPNETCORE_ENVIRONMENT`環境變數，以`Development`。

如果沒有 IIS Express 開發安裝的憑證，您可以自行建立開發憑證。 Windows 上，您可以建立開發憑證，並且將它加入至目前使用者的受信任的根存放區中，在提升權限的提示字元中執行下列 PowerShell 命令：

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>Kestrel macOS 和 Linux 上

您可以設定為與所需的 IP 位址、 連接埠，以及憑證設定的端點，透過 HTTPS 接聽 Kestrel。 憑證可以是已設定的內嵌，或在最上層`Certificates`區段，然後依名稱參考：

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

您可以在 macOS 和 Linux 上建立 HTTPS 使用的自我簽署的憑證[OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

一次`certificate.pfx`已產生檔案，設定 HTTPS 憑證，在您`appsettings.Development.json`檔案：

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

您也必須設定 「 憑證： HTTPS:Password"設定屬性來指定憑證的複雜密碼。 密碼不會以純文字方式進行儲存。 請參閱[安全存放裝置的應用程式密碼在開發期間](app-secrets.md)來適當處理憑證的複雜密碼。

您可以在 macOS[將憑證新增至您的金鑰鏈](https://support.apple.com/kb/PH20129?locale=en_US)和[變更其信任設定](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US)使其具備在開發期間針對 HTTPS 受信任。 若要將憑證新增至您的金鑰鏈 (相當於`CurrentUser/My`儲存在 Windows 上) 執行下列命令：

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

然後再信任的憑證：

```bash
security add-trusted-cert localhost.cer
```

接著，您可以設定您的應用程式，就像這樣的開發工作中使用此憑證：

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
