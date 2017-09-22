---
title: "共用應用程式之間的 cookie"
author: rick-anderson
description: 
keywords: "共用的 ASP.NET Core,ASP.NET,cookies,Interop,cookie"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: dbf52b0a990a3627b8eded22db033c45d51ba6ad
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="sharing-cookies-between-applications"></a>共用應用程式之間的 cookie

網站通常包含許多個別 web 應用程式，所有工作一起 harmoniously。 如果應用程式開發人員想要提供良好的單一登入體驗，其通常需要的所有不同的網頁應用程式的網站內共用彼此之間的驗證票證。

若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。

## <a name="sharing-authentication-cookies-between-applications"></a>應用程式之間共用的驗證 cookie

若要共用兩個不同的 ASP.NET Core 應用程式之間的驗證 cookie，設定每個應用程式，都應該共用 cookie，如下所示。

在您設定方法使用，若要設定資料保護服務的 cookie CookieAuthenticationOptions 和比對 ASP.NET AuthenticationScheme 4.X。

如果您正在使用身分識別：

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
   {
       options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
       var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
       options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
       options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
   });
   ```

如果您直接使用 cookie:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
   {
       DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
   });
   ```
   
`DataProtectionProvider`需要`Microsoft.AspNetCore.DataProtection.Extensions`NuGet 封裝。

當使用這種方式，DirectoryInfo 應該指向特別設定的驗證 cookie 擱置在一旁的金鑰儲存位置。 Cookie 驗證中介軟體將會使用 DataProtectionProvider，現在的明確提供的實作隔離的應用程式其他部分所使用的資料保護系統。 應用程式名稱會被忽略 （有意如此，由於您正嘗試取得共用裝載的多個應用程式）。

>[!WARNING]
>您應該考慮設定 DataProtectionProvider 金鑰會加密待用，如下列範例。
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式

使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生的驗證 cookie 的 ASP.NET Core cookie 驗證中介軟體相容。 這可讓但仍在整個站台的經驗提供平滑的單一登分次升級大站台的個別應用程式。

>[!TIP]
> 您可以告訴您現有的應用程式是否使用 Katana cookie 驗證中介軟體在您的專案 Startup.Auth.cs UseCookieAuthentication 呼叫是否存在。 ASP.NET 4.x web 應用程式專案與 Visual Studio 2013 所建立，然後依預設使用 Katana cookie 驗證中介軟體。

> [!NOTE]
> ASP.NET 4.x 應用程式必須為目標.NET Framework 4.5.1 或更新版本中，否則為必要的 NuGet 套件將無法安裝。

若要共用您的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後遵循下列步驟來設定 ASP.NET 4.x 應用程式。

1.  ASP.NET 4.x 應用程式的每個安裝封裝 Microsoft.Owin.Security.Interop。

2.   在 Startup.Auth.cs，找出 UseCookieAuthentication，通常看起來像下列的呼叫。

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  UseCookieAuthentication 的呼叫，如下所示修改資料、 變更 ASP.NET Core cookie 驗證中介軟體，提供金鑰的儲存位置，初始化 DataProtectionProvider 的執行個體所使用的名稱相符 CookieName 和使區塊格式相容，則您可以將 interop ChunkingCookieManager CookieManager。

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
       {
           AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
           CookieName = ".AspNetCore.Cookies",
           // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
           // CookiePath = "...", (if necessary)
           // ...
           TicketDataFormat = new AspNetTicketDataFormat(
               new DataProtectorShim(
                   DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                   .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                   "Cookies", "v2"))),
           CookieManager = new ChunkingCookieManager()
       });
       ```
    DirectoryInfo 有指向相同的儲存位置，指向您的 ASP.NET Core 應用程式，並應使用相同的設定進行設定。

ASP.NET 4.x 和 ASP.NET Core 應用程式現在都設定為共用的驗證 cookie。

> [!NOTE]
> 您必須確定每個應用程式的識別系統指向相同的使用者資料庫。 否則識別系統在會嘗試比對中針對其資料庫中資訊的驗證 cookie 的資訊時，會產生在執行階段失敗。
