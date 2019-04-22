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
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>取代 ASP.NET Core 中的 ASP.NET 電腦金鑰

<a name="compatibility-replacing-machinekey"></a>

實作`<machineKey>`在 ASP.NET 中的項目[可取代](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。 這可讓 ASP.NET 密碼編譯常式大部分呼叫會透過取代資料保護機制，包括新的資料保護系統路由。

## <a name="package-installation"></a>套件安裝

> [!NOTE]
> 新的資料保護系統只能安裝到現有的 ASP.NET 應用程式目標為.NET 4.5.1 或更新版本。 安裝將會失敗，如果應用程式以.NET 4.5 為目標，或降低。

若要安裝新的資料保護系統，到現有的 ASP.NET 4.5.1+ 專案，安裝套件 Microsoft.AspNetCore.DataProtection.SystemWeb。 這會具現化的資料保護系統使用[預設組態](xref:security/data-protection/configuration/default-settings)設定。

當您安裝封裝時，它會插入一行*Web.config*這會告知 ASP.NET，以將其用於[最密碼編譯作業](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括表單驗證、 檢視狀態和呼叫MachineKey.Protect。 插入一行，如下所示。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> 您可以知道新的資料保護系統是否作用中，藉由檢查等欄位`__VIEWSTATE`，這應該如下列範例所示以"CfDJ8"開頭。 「 CfDJ8"是識別受資料保護系統的承載的 magic"09 F0 C9 F0"標頭的 base64 表示法。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>封裝組態

資料保護系統具現化使用預設的零設定組態。 不過，由於預設索引鍵會保存到本機檔案系統，這不適用於部署伺服器陣列中的應用程式。 若要解決此問題，您可以藉由建立類型的子類別 DataProtectionStartup 提供組態，並覆寫及其 ConfigureServices 方法。

以下是自訂的資料保護啟動類型設定金鑰保存的位置和它們如何加密靜止的範例。 它也會覆寫預設應用程式隔離原則藉由提供它自己的應用程式名稱。

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
> 您也可以使用`<machineKey applicationName="my-app" ... />`SetApplicationName 明確呼叫取代。 這是為了避免強制開發人員建立 DataProtectionStartup 衍生型別，如果他們想要設定所有已設定應用程式名稱的方便機制。

若要啟用此自訂的設定，請回到 Web.config，並尋找`<appSettings>`套件安裝新增至組態檔的元素。 它看起來像下列標記：

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

填寫您剛才建立的 DataProtectionStartup 衍生的類型的組件限定名稱空白的值。 如果應用程式的名稱是 DataProtectionDemo，這會看起來如下。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

現在可供在應用程式內使用新設定的資料保護系統。
