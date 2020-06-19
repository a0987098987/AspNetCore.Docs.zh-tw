---
title: ASP.NET Core Blazor 環境
author: guardrex
description: 瞭解中的環境 Blazor ，包括如何設定 Blazor WebAssembly 應用程式的環境。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/10/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/fundamentals/environments
ms.openlocfilehash: 203f29ce606a313463e416b068177ce02acd6231
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103611"
---
# <a name="aspnet-core-blazor-environments"></a>ASP.NET Core Blazor 環境

> [!NOTE]
> 本主題適用于 Blazor WebAssembly。 如需 ASP.NET Core 應用程式設定的一般指引，請參閱 <xref:fundamentals/environments> 。

在本機執行應用程式時，環境會預設為開發。 當應用程式發佈時，環境會預設為 [生產]。

裝載的 Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增標頭來將環境傳達給瀏覽器 `blazor-environment` 。 標頭的值為環境。 裝載的 Blazor 應用程式和伺服器應用程式會共用相同的環境。 如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments> 。

針對在本機執行的獨立應用程式，開發伺服器會新增 `blazor-environment` 標頭以指定開發環境。 若要指定其他裝載環境的環境，請新增 `blazor-environment` 標頭。

在 IIS 的下列範例中，將自訂標頭加入至已發佈的*web.config*檔案。 *web.config*檔案位於*bin/RELEASE/{TARGET FRAMEWORK}/publish*資料夾中：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>

    ...

    <httpProtocol>
      <customHeaders>
        <add name="blazor-environment" value="Staging" />
      </customHeaders>
    </httpProtocol>
  </system.webServer>
</configuration>
```

> [!NOTE]
> 若要使用 IIS 的自訂*web.config*檔案，而該檔案不會在應用程式發佈至*發行*資料夾時遭到覆寫，請參閱 <xref:blazor/host-and-deploy/webassembly#use-a-custom-webconfig> 。

藉由插入和讀取屬性，在元件中取得應用程式的環境 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.Environment> ：

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

在啟動期間， <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> 會透過屬性公開 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.HostEnvironment> ，讓開發人員在其程式碼中具有環境特定邏輯：

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：

* `IsDevelopment()`
* `IsProduction()`
* `IsStaging()`
* `IsEnvironment("{ENVIRONMENT NAME}")`

```csharp
if (builder.HostEnvironment.IsStaging())
{
    ...
};

if (builder.HostEnvironment.IsEnvironment("Custom"))
{
    ...
};
```

<xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType>當服務無法使用時，可以在啟動期間使用此屬性 <xref:Microsoft.AspNetCore.Components.NavigationManager> 。

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/environments>
