---
title: "取代`<machineKey>`ASP.NET 中"
author: rick-anderson
description: "取代`<machineKey>`ASP.NET 中"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 865c949a526e07d41ac4627fdf0411d5422adc16
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="replacing-machinekey-in-aspnet"></a>取代`<machineKey>`ASP.NET 中

<a name="compatibility-replacing-machinekey"></a>

實作`<machineKey>`ASP.NET 中的項目[是可取代](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。 這可讓大部分呼叫 ASP.NET 密碼編譯常式，以透過取代資料保護機制，包括新的資料保護系統進行路由。

## <a name="package-installation"></a>封裝安裝

> [!NOTE]
> 新的資料保護系統只能安裝到現有的 ASP.NET 應用程式為目標.NET 4.5.1 或更新版本。 安裝將會失敗，如果應用程式的目標.NET 4.5 或降低。

若要安裝新的資料保護系統到現有的 ASP.NET 4.5.1+ 專案，請將封裝 Microsoft.AspNetCore.DataProtection.SystemWeb 安裝。 這會具現化的資料保護系統使用[預設組態](xref:security/data-protection/configuration/default-settings)設定。

當您安裝封裝時，它會插入到行*Web.config*這會告知 ASP.NET，若要將其用於[最密碼編譯作業](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括表單驗證、 檢視狀態和呼叫MachineKey.Protect。 插入的行讀取，如下所示。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> 您可以知道新的資料保護系統是否作用中，藉由檢查欄位`__VIEWSTATE`，其中應該有"CfDJ8"如下列範例所示。 「 CfDJ8"會識別裝載受資料保護系統中的 magic"09 F0 C9 F0"標頭的 base64 表示法。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a>封裝組態

資料保護系統具現化，且預設零安裝程式組態。 不過，因為依照預設索引鍵會保存到本機檔案系統，這不會運作的應用程式的部署伺服器陣列中。 若要解決此問題，您可以藉由建立類型的子類別 DataProtectionStartup 提供組態，並覆寫其 ConfigureServices 方法。

以下是自訂的資料保護啟動類型設定金鑰保存的位置和它們如何加密在靜止的範例。 它也會覆寫預設應用程式隔離原則藉由提供它自己的應用程式名稱。

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
> 您也可以使用`<machineKey applicationName="my-app" ... />`SetApplicationName 明確呼叫取代。 這是一個方便機制來避免強制開發人員建立 DataProtectionStartup 衍生型別，如果他們想要設定所有已設定應用程式名稱。

若要啟用此自訂設定，請回到 Web.config，並尋找`<appSettings>`封裝安裝的元素加入至組態檔。 它看起來像下列標記：

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

填入的空白值與您剛才建立的 DataProtectionStartup 衍生的類型的組件限定名稱。 如果應用程式的名稱是 DataProtectionDemo，這看起來會像下面。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

現在可以使用應用程式內的新設定好的資料保護系統。
