---
title: 取代 ASP.NET machineKey 中 ASP.NET Core
author: rick-anderson
description: 了解如何取代 machineKey ASP.NET 為允許使用更安全的新資料保護系統中。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 5f9e5cec02b66e1315548c4e7c18fe168ad161eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278820"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a><span data-ttu-id="5be23-103">取代 ASP.NET machineKey 中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5be23-103">Replace the ASP.NET machineKey in ASP.NET Core</span></span>

<a name="compatibility-replacing-machinekey"></a>

<span data-ttu-id="5be23-104">實作`<machineKey>`ASP.NET 中的項目[是可取代](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。</span><span class="sxs-lookup"><span data-stu-id="5be23-104">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="5be23-105">這可讓大部分呼叫 ASP.NET 密碼編譯常式，以透過取代資料保護機制，包括新的資料保護系統進行路由。</span><span class="sxs-lookup"><span data-stu-id="5be23-105">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="5be23-106">套件安裝</span><span class="sxs-lookup"><span data-stu-id="5be23-106">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="5be23-107">新的資料保護系統只能安裝到現有的 ASP.NET 應用程式為目標.NET 4.5.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="5be23-107">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="5be23-108">安裝將會失敗，如果應用程式的目標.NET 4.5 或降低。</span><span class="sxs-lookup"><span data-stu-id="5be23-108">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="5be23-109">若要安裝新的資料保護系統到現有的 ASP.NET 4.5.1+ 專案，請將封裝 Microsoft.AspNetCore.DataProtection.SystemWeb 安裝。</span><span class="sxs-lookup"><span data-stu-id="5be23-109">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="5be23-110">這會具現化的資料保護系統使用[預設組態](xref:security/data-protection/configuration/default-settings)設定。</span><span class="sxs-lookup"><span data-stu-id="5be23-110">This will instantiate the data protection system using the [default configuration](xref:security/data-protection/configuration/default-settings) settings.</span></span>

<span data-ttu-id="5be23-111">當您安裝封裝時，它會插入到行*Web.config*這會告知 ASP.NET，若要將其用於[最密碼編譯作業](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括表單驗證、 檢視狀態和呼叫MachineKey.Protect。</span><span class="sxs-lookup"><span data-stu-id="5be23-111">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="5be23-112">插入的行讀取，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5be23-112">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="5be23-113">您可以知道新的資料保護系統是否作用中，藉由檢查欄位`__VIEWSTATE`，其中應該有"CfDJ8"如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="5be23-113">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="5be23-114">「 CfDJ8"會識別裝載受資料保護系統中的 magic"09 F0 C9 F0"標頭的 base64 表示法。</span><span class="sxs-lookup"><span data-stu-id="5be23-114">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="5be23-115">封裝組態</span><span class="sxs-lookup"><span data-stu-id="5be23-115">Package configuration</span></span>

<span data-ttu-id="5be23-116">資料保護系統具現化，且預設零安裝程式組態。</span><span class="sxs-lookup"><span data-stu-id="5be23-116">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="5be23-117">不過，因為依照預設索引鍵會保存到本機檔案系統，這不會運作的應用程式的部署伺服器陣列中。</span><span class="sxs-lookup"><span data-stu-id="5be23-117">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="5be23-118">若要解決此問題，您可以藉由建立類型的子類別 DataProtectionStartup 提供組態，並覆寫其 ConfigureServices 方法。</span><span class="sxs-lookup"><span data-stu-id="5be23-118">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="5be23-119">以下是自訂的資料保護啟動類型設定金鑰保存的位置和它們如何加密在靜止的範例。</span><span class="sxs-lookup"><span data-stu-id="5be23-119">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="5be23-120">它也會覆寫預設應用程式隔離原則藉由提供它自己的應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="5be23-120">It also overrides the default app isolation policy by providing its own application name.</span></span>

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
> <span data-ttu-id="5be23-121">您也可以使用`<machineKey applicationName="my-app" ... />`SetApplicationName 明確呼叫取代。</span><span class="sxs-lookup"><span data-stu-id="5be23-121">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="5be23-122">這是一個方便機制來避免強制開發人員建立 DataProtectionStartup 衍生型別，如果他們想要設定所有已設定應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="5be23-122">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="5be23-123">若要啟用此自訂設定，請回到 Web.config，並尋找`<appSettings>`封裝安裝的元素加入至組態檔。</span><span class="sxs-lookup"><span data-stu-id="5be23-123">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="5be23-124">它看起來像下列標記：</span><span class="sxs-lookup"><span data-stu-id="5be23-124">It will look like the following markup:</span></span>

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

<span data-ttu-id="5be23-125">填入的空白值與您剛才建立的 DataProtectionStartup 衍生的類型的組件限定名稱。</span><span class="sxs-lookup"><span data-stu-id="5be23-125">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="5be23-126">如果應用程式的名稱是 DataProtectionDemo，這看起來會像下面。</span><span class="sxs-lookup"><span data-stu-id="5be23-126">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="5be23-127">現在可以使用應用程式內的新設定好的資料保護系統。</span><span class="sxs-lookup"><span data-stu-id="5be23-127">The newly-configured data protection system is now ready for use inside the application.</span></span>
