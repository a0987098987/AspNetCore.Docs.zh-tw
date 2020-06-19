---
title: ASP.NET Core Blazor 設定
author: guardrex
description: 瞭解應用程式的 Blazor 設定，包括應用程式設定、驗證和記錄設定。
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
uid: blazor/fundamentals/configuration
ms.openlocfilehash: b43eae03c71cabbaafa2bc0d704765e89f743279
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103614"
---
# <a name="aspnet-core-blazor-configuration"></a>ASP.NET Core Blazor 設定

> [!NOTE]
> 本主題適用于 Blazor WebAssembly。 如需 ASP.NET Core 應用程式設定的一般指引，請參閱 <xref:fundamentals/configuration/index> 。

BlazorWebAssembly 會從載入設定：

* 應用程式佈建檔案（預設為）：
  * *wwwroot/appsettings.js開啟*
  * *wwwroot/appsettings。{環境}. json*
* 應用程式註冊的其他設定[提供者](xref:fundamentals/configuration/index)。 並非所有提供者都適用于 Blazor WebAssembly apps。 澄清 Blazor [ Blazor WASM （dotnet/AspNetCore.Doc#18134）的設定提供者](https://github.com/dotnet/AspNetCore.Docs/issues/18134)，即可追蹤支援 WebAssembly 的提供者。

> [!WARNING]
> Blazor使用者可以看到 WebAssembly 應用程式中的設定。 **請勿在設定中儲存應用程式秘密或認證。**

如需設定提供者的詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。

## <a name="app-settings-configuration"></a>應用程式設定

*wwwroot/appsettings.js開啟*：

```json
{
  "message": "Hello from config!"
}
```

將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<p>Message: @Configuration["message"]</p>
```

## <a name="provider-configuration"></a>提供者設定

下列範例會使用 <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationSource> 來提供額外的設定：

`Program.Main`:

```csharp
using Microsoft.Extensions.Configuration.Memory;

...

var vehicleData = new Dictionary<string, string>()
{
    { "color", "blue" },
    { "type", "car" },
    { "wheels:count", "3" },
    { "wheels:brand", "Blazin" },
    { "wheels:brand:type", "rally" },
    { "wheels:year", "2008" },
};

var memoryConfig = new MemoryConfigurationSource { InitialData = vehicleData };

...

builder.Configuration.Add(memoryConfig);
```

將 <xref:Microsoft.Extensions.Configuration.IConfiguration> 實例插入元件以存取設定資料：

```razor
@page "/"
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<h1>Configuration example</h1>

<h2>Wheels</h2>

<ul>
    <li>Count: @Configuration["wheels:count"]</li>
    <li>Brand: @Configuration["wheels:brand"]</li>
    <li>Type: @Configuration["wheels:brand:type"]</li>
    <li>Year: @Configuration["wheels:year"]</li>
</ul>

@code {
    var wheelsSection = Configuration.GetSection("wheels");
    
    ...
}
```

若要將*wwwroot*資料夾中的其他設定檔讀取到設定中，請使用 <xref:System.Net.Http.HttpClient> 來取得檔案的內容。 使用此方法時，現有的 <xref:System.Net.Http.HttpClient> 服務註冊可以使用已建立的本機用戶端來讀取檔案，如下列範例所示：

*wwwroot/cars.js開啟*：

```json
{
    "size": "tiny"
}
```

`Program.Main`:

```csharp
using Microsoft.Extensions.Configuration;

...

var client = new HttpClient()
{
    BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
};

builder.Services.AddTransient(sp => client);

using var response = await client.GetAsync("cars.json");
using var stream = await response.Content.ReadAsStreamAsync();

builder.Configuration.AddJsonStream(stream);
```

## <a name="authentication-configuration"></a>驗證設定

*wwwroot/appsettings.js開啟*：

```json
{
  "Local": {
    "Authority": "{AUTHORITY}",
    "ClientId": "{CLIENT ID}"
  }
}
```

`Program.Main`:

```csharp
builder.Services.AddOidcAuthentication(options =>
    builder.Configuration.Bind("Local", options.ProviderOptions));
```

## <a name="logging-configuration"></a>記錄設定

新增[Microsoft.Extensions.Logging.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Configuration/)的套件參考：

```xml
<PackageReference Include="Microsoft.Extensions.Logging.Configuration" Version="{VERSION}" />
```

*wwwroot/appsettings.js開啟*：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```

`Program.Main`:

```csharp
using Microsoft.Extensions.Logging;

...

builder.Logging.AddConfiguration(
    builder.Configuration.GetSection("Logging"));
```

## <a name="host-builder-configuration"></a>主機產生器設定

`Program.Main`:

```csharp
var hostname = builder.Configuration["HostName"];
```

## <a name="cached-configuration"></a>快取設定

系統會快取設定檔以供離線使用。 使用[漸進式 Web 應用程式（pwa）](xref:blazor/progressive-web-app)，您只能在建立新的部署時更新設定檔。 在部署之間編輯設定檔沒有任何作用，因為：

* 使用者有檔案的快取版本，這些檔案會繼續使用。
* PWA 的*service-worker.js*和*service-worker-assets.js*檔案必須在編譯時重建，這會在使用者下一次線上流覽應用程式，以重新部署應用程式。

如需 Pwa 如何處理背景更新的詳細資訊，請參閱 <xref:blazor/progressive-web-app#background-updates> 。
