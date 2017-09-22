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
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="cc41d-103">共用應用程式之間的 cookie</span><span class="sxs-lookup"><span data-stu-id="cc41d-103">Sharing cookies between applications</span></span>

<span data-ttu-id="cc41d-104">網站通常包含許多個別 web 應用程式，所有工作一起 harmoniously。</span><span class="sxs-lookup"><span data-stu-id="cc41d-104">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="cc41d-105">如果應用程式開發人員想要提供良好的單一登入體驗，其通常需要的所有不同的網頁應用程式的網站內共用彼此之間的驗證票證。</span><span class="sxs-lookup"><span data-stu-id="cc41d-105">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="cc41d-106">若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。</span><span class="sxs-lookup"><span data-stu-id="cc41d-106">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="cc41d-107">應用程式之間共用的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="cc41d-107">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="cc41d-108">若要共用兩個不同的 ASP.NET Core 應用程式之間的驗證 cookie，設定每個應用程式，都應該共用 cookie，如下所示。</span><span class="sxs-lookup"><span data-stu-id="cc41d-108">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="cc41d-109">在您設定方法使用，若要設定資料保護服務的 cookie CookieAuthenticationOptions 和比對 ASP.NET AuthenticationScheme 4.X。</span><span class="sxs-lookup"><span data-stu-id="cc41d-109">In your configure method use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.X.</span></span>

<span data-ttu-id="cc41d-110">如果您正在使用身分識別：</span><span class="sxs-lookup"><span data-stu-id="cc41d-110">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
   {
       options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
       var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
       options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
       options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
   });
   ```

<span data-ttu-id="cc41d-111">如果您直接使用 cookie:</span><span class="sxs-lookup"><span data-stu-id="cc41d-111">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
   {
       DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
   });
   ```
   
<span data-ttu-id="cc41d-112">`DataProtectionProvider`需要`Microsoft.AspNetCore.DataProtection.Extensions`NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="cc41d-112">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="cc41d-113">當使用這種方式，DirectoryInfo 應該指向特別設定的驗證 cookie 擱置在一旁的金鑰儲存位置。</span><span class="sxs-lookup"><span data-stu-id="cc41d-113">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="cc41d-114">Cookie 驗證中介軟體將會使用 DataProtectionProvider，現在的明確提供的實作隔離的應用程式其他部分所使用的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="cc41d-114">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="cc41d-115">應用程式名稱會被忽略 （有意如此，由於您正嘗試取得共用裝載的多個應用程式）。</span><span class="sxs-lookup"><span data-stu-id="cc41d-115">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="cc41d-116">您應該考慮設定 DataProtectionProvider 金鑰會加密待用，如下列範例。</span><span class="sxs-lookup"><span data-stu-id="cc41d-116">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="cc41d-117">共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="cc41d-117">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="cc41d-118">使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生的驗證 cookie 的 ASP.NET Core cookie 驗證中介軟體相容。</span><span class="sxs-lookup"><span data-stu-id="cc41d-118">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="cc41d-119">這可讓但仍在整個站台的經驗提供平滑的單一登分次升級大站台的個別應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc41d-119">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="cc41d-120">您可以告訴您現有的應用程式是否使用 Katana cookie 驗證中介軟體在您的專案 Startup.Auth.cs UseCookieAuthentication 呼叫是否存在。</span><span class="sxs-lookup"><span data-stu-id="cc41d-120">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="cc41d-121">ASP.NET 4.x web 應用程式專案與 Visual Studio 2013 所建立，然後依預設使用 Katana cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="cc41d-121">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="cc41d-122">ASP.NET 4.x 應用程式必須為目標.NET Framework 4.5.1 或更新版本中，否則為必要的 NuGet 套件將無法安裝。</span><span class="sxs-lookup"><span data-stu-id="cc41d-122">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="cc41d-123">若要共用您的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後遵循下列步驟來設定 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc41d-123">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="cc41d-124">ASP.NET 4.x 應用程式的每個安裝封裝 Microsoft.Owin.Security.Interop。</span><span class="sxs-lookup"><span data-stu-id="cc41d-124">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="cc41d-125">在 Startup.Auth.cs，找出 UseCookieAuthentication，通常看起來像下列的呼叫。</span><span class="sxs-lookup"><span data-stu-id="cc41d-125">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="cc41d-126">UseCookieAuthentication 的呼叫，如下所示修改資料、 變更 ASP.NET Core cookie 驗證中介軟體，提供金鑰的儲存位置，初始化 DataProtectionProvider 的執行個體所使用的名稱相符 CookieName 和使區塊格式相容，則您可以將 interop ChunkingCookieManager CookieManager。</span><span class="sxs-lookup"><span data-stu-id="cc41d-126">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

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
    <span data-ttu-id="cc41d-127">DirectoryInfo 有指向相同的儲存位置，指向您的 ASP.NET Core 應用程式，並應使用相同的設定進行設定。</span><span class="sxs-lookup"><span data-stu-id="cc41d-127">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="cc41d-128">ASP.NET 4.x 和 ASP.NET Core 應用程式現在都設定為共用的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="cc41d-128">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="cc41d-129">您必須確定每個應用程式的識別系統指向相同的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="cc41d-129">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="cc41d-130">否則識別系統在會嘗試比對中針對其資料庫中資訊的驗證 cookie 的資訊時，會產生在執行階段失敗。</span><span class="sxs-lookup"><span data-stu-id="cc41d-130">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
