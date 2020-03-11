---
title: 取代中的 ASP.NET machineKey ASP.NET Core
author: rick-anderson
description: 探索如何取代 ASP.NET 中的 machineKey，以允許使用新的、更安全的資料保護系統。
ms.author: riande
ms.date: 04/06/2019
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 2317cb50cfe63226baf336ebfc5d681d1cebe5c6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667983"
---
# <a name="replace-the-aspnet-machinekey-in-aspnet-core"></a>取代中的 ASP.NET machineKey ASP.NET Core

<a name="compatibility-replacing-machinekey"></a>

ASP.NET 中的 `<machineKey>` 元素的實作為[可取代](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)。 這可讓大部分的呼叫 ASP.NET 密碼編譯常式，以透過取代資料保護機制（包括新的資料保護系統）來路由傳送。

## <a name="package-installation"></a>套件安裝

> [!NOTE]
> 新的資料保護系統只能安裝到以 .NET 4.5.1 或更新版本為目標的現有 ASP.NET 應用程式。 如果應用程式是以 .NET 4.5 或更低版本為目標，安裝將會失敗。

若要將新的資料保護系統安裝到現有的 ASP.NET 4.5.1 + 專案，請安裝 AspNetCore. DataProtection. SystemWeb 套件。 這會使用[預設](xref:security/data-protection/configuration/default-settings)的設定來將資料保護系統具現化。

當您安裝套件時，它會在*web.config*中插入一行，告訴 ASP.NET 將它用於大部分的[密碼編譯作業](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/)，包括表單驗證、檢視狀態和 MachineKey 的呼叫。 插入的程式程式碼會如下所示。

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> 您可以檢查如 `__VIEWSTATE`之類的欄位（如下列範例所示），以判斷新的資料保護系統是否為作用中。 "CfDJ8" 是神奇 "09 F0 C9 F0" 標頭的 base64 標記法，可識別資料保護系統所保護的承載。

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk...">
```

## <a name="package-configuration"></a>套件設定

資料保護系統會以預設的零安裝設定進行具現化。 不過，由於預設金鑰會保存在本機檔案系統中，因此這不適用於部署在伺服器陣列中的應用程式。 若要解決這個問題，您可以藉由建立子類別所 DataProtectionStartup 的類型，並覆寫其 ConfigureServices 方法，來提供設定。

以下是自訂資料保護啟動類型的範例，其設定了金鑰的保存位置，以及它們在待用時的加密方式。 它也會藉由提供自己的應用程式名稱來覆寫預設的應用程式隔離原則。

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
> 您也可以使用 `<machineKey applicationName="my-app" ... />` 來取代對 SetApplicationName 的明確呼叫。 這是一種便利的機制，如果他們想要設定的是應用程式名稱，就可以避免強制開發人員建立 DataProtectionStartup 衍生的型別。

若要啟用此自訂設定，請回到 web.config，然後尋找封裝安裝新增至設定檔的 `<appSettings>` 專案。 它看起來會像下面這樣的標記：

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

以您剛才建立之 DataProtectionStartup 衍生類型的元件限定名稱填入空白值。 如果應用程式的名稱是 DataProtectionDemo，這看起來會像下面這樣。

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

新設定的資料保護系統現在已準備好在應用程式內使用。
