---
title: 取代 ASP.NET Core 中的 ASP.NET 電腦金鑰
author: rick-anderson
description: 了解如何在 ASP.NET 中以允許新且更安全的資料保護系統使用 machineKey 的取代。
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705545"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="05b3f-103">取代 ASP.NET Core 中的 ASP.NET 電腦金鑰</span><span class="sxs-lookup"><span data-stu-id="05b3f-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="05b3f-104">實作`<machineKey>`在 ASP.NET 中的項目[可取代](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。</span><span class="sxs-lookup"><span data-stu-id="05b3f-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="05b3f-105">這可讓 ASP.NET 密碼編譯常式大部分呼叫會透過取代資料保護機制，包括新的資料保護系統路由。</span><span class="sxs-lookup"><span data-stu-id="05b3f-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="05b3f-106">套件安裝</span><span class="sxs-lookup"><span data-stu-id="05b3f-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="05b3f-107">新的資料保護系統只能安裝到現有的 ASP.NET 應用程式目標為.NET 4.5.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="05b3f-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or later.</span></span> <span data-ttu-id="05b3f-108">安裝將會失敗，如果應用程式以.NET 4.5 為目標，或降低。</span><span class="sxs-lookup"><span data-stu-id="05b3f-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="05b3f-109">若要安裝新的資料保護系統，到現有的 ASP.NET 4.5.1+ 專案，安裝套件 Microsoft.AspNetCore.DataProtection.SystemWeb。</span><span class="sxs-lookup"><span data-stu-id="05b3f-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="05b3f-110">這會具現化的資料保護系統使用[預設組態](xref:security/data-protection/configuration/default-settings)設定。</span><span class="sxs-lookup"><span data-stu-id="05b3f-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="05b3f-111">當您安裝封裝時，它會插入一行*Web.config*這會告知 ASP.NET，以將其用於[最密碼編譯作業](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括表單驗證、 檢視狀態和呼叫MachineKey.Protect。</span><span class="sxs-lookup"><span data-stu-id="05b3f-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="05b3f-112">插入一行，如下所示。</span><span class="sxs-lookup"><span data-stu-id="05b3f-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="05b3f-113">您可以知道新的資料保護系統是否作用中，藉由檢查等欄位`__VIEWSTATE`，這應該如下列範例所示以"CfDJ8"開頭。</span><span class="sxs-lookup"><span data-stu-id="05b3f-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="05b3f-114">「 CfDJ8"是識別受資料保護系統的承載的 magic"09 F0 C9 F0"標頭的 base64 表示法。</span><span class="sxs-lookup"><span data-stu-id="05b3f-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a><span data-ttu-id="05b3f-115">封裝組態</span><span class="sxs-lookup"><span data-stu-id="05b3f-115">Package configuration</span></span>

<span data-ttu-id="05b3f-116">資料保護系統具現化使用預設的零設定組態。</span><span class="sxs-lookup"><span data-stu-id="05b3f-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="05b3f-117">不過，由於預設索引鍵會保存到本機檔案系統，這不適用於部署伺服器陣列中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="05b3f-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="05b3f-118">若要解決此問題，您可以藉由建立類型的子類別 DataProtectionStartup 提供組態，並覆寫及其 ConfigureServices 方法。</span><span class="sxs-lookup"><span data-stu-id="05b3f-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="05b3f-119">以下是自訂的資料保護啟動類型設定金鑰保存的位置和它們如何加密靜止的範例。</span><span class="sxs-lookup"><span data-stu-id="05b3f-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="05b3f-120">它也會覆寫預設應用程式隔離原則藉由提供它自己的應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="05b3f-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="05b3f-121">您也可以使用`<machineKey applicationName="my-app" ... />`SetApplicationName 明確呼叫取代。</span><span class="sxs-lookup"><span data-stu-id="05b3f-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="05b3f-122">這是為了避免強制開發人員建立 DataProtectionStartup 衍生型別，如果他們想要設定所有已設定應用程式名稱的方便機制。</span><span class="sxs-lookup"><span data-stu-id="05b3f-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="05b3f-123">若要啟用此自訂的設定，請回到 Web.config，並尋找`<appSettings>`套件安裝新增至組態檔的元素。</span><span class="sxs-lookup"><span data-stu-id="05b3f-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="05b3f-124">它看起來像下列標記：</span><span class="sxs-lookup"><span data-stu-id="05b3f-124">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="05b3f-125">填寫您剛才建立的 DataProtectionStartup 衍生的類型的組件限定名稱空白的值。</span><span class="sxs-lookup"><span data-stu-id="05b3f-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="05b3f-126">如果應用程式的名稱是 DataProtectionDemo，這會看起來如下。</span><span class="sxs-lookup"><span data-stu-id="05b3f-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="05b3f-127">現在可供在應用程式內使用新設定的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="05b3f-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
