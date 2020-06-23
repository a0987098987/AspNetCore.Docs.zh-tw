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
ms.openlocfilehash: a527e04cf97dd2d2b88dcc6e866475835498545d
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243612"
---
# <a name="aspnet-core-blazor-environments"></a><span data-ttu-id="fefb1-103">ASP.NET Core Blazor 環境</span><span class="sxs-lookup"><span data-stu-id="fefb1-103">ASP.NET Core Blazor environments</span></span>

> [!NOTE]
> <span data-ttu-id="fefb1-104">本主題適用于 Blazor WebAssembly。</span><span class="sxs-lookup"><span data-stu-id="fefb1-104">This topic applies to Blazor WebAssembly.</span></span> <span data-ttu-id="fefb1-105">如需 ASP.NET Core 應用程式設定的一般指引，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-105">For general guidance on ASP.NET Core app configuration, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="fefb1-106">在本機執行應用程式時，環境會預設為開發。</span><span class="sxs-lookup"><span data-stu-id="fefb1-106">When running an app locally, the environment defaults to Development.</span></span> <span data-ttu-id="fefb1-107">當應用程式發佈時，環境會預設為 [生產]。</span><span class="sxs-lookup"><span data-stu-id="fefb1-107">When the app is published, the environment defaults to Production.</span></span>

<span data-ttu-id="fefb1-108">裝載的 Blazor WebAssembly 應用程式會透過中介軟體來從伺服器挑選環境，藉由新增標頭來將環境傳達給瀏覽器 `blazor-environment` 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-108">A hosted Blazor WebAssembly app picks up the environment from the server via a middleware that communicates the environment to the browser by adding the `blazor-environment` header.</span></span> <span data-ttu-id="fefb1-109">標頭的值為環境。</span><span class="sxs-lookup"><span data-stu-id="fefb1-109">The value of the header is the environment.</span></span> <span data-ttu-id="fefb1-110">裝載的 Blazor 應用程式和伺服器應用程式會共用相同的環境。</span><span class="sxs-lookup"><span data-stu-id="fefb1-110">The hosted Blazor app and the server app share the same environment.</span></span> <span data-ttu-id="fefb1-111">如需詳細資訊，包括如何設定環境，請參閱 <xref:fundamentals/environments> 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-111">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="fefb1-112">針對在本機執行的獨立應用程式，開發伺服器會新增 `blazor-environment` 標頭以指定開發環境。</span><span class="sxs-lookup"><span data-stu-id="fefb1-112">For a standalone app running locally, the development server adds the `blazor-environment` header to specify the Development environment.</span></span> <span data-ttu-id="fefb1-113">若要指定其他裝載環境的環境，請新增 `blazor-environment` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fefb1-113">To specify the environment for other hosting environments, add the `blazor-environment` header.</span></span>

<span data-ttu-id="fefb1-114">在 IIS 的下列範例中，將自訂標頭加入至已發行的檔案 `web.config` 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-114">In the following example for IIS, add the custom header to the published `web.config` file.</span></span> <span data-ttu-id="fefb1-115">檔案 `web.config` 位於 `bin/Release/{TARGET FRAMEWORK}/publish` 下列資料夾中：</span><span class="sxs-lookup"><span data-stu-id="fefb1-115">The `web.config` file is located in the `bin/Release/{TARGET FRAMEWORK}/publish` folder:</span></span>

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
> <span data-ttu-id="fefb1-116">若要使用在 `web.config` 應用程式發行至資料夾時不會覆寫之 IIS 的自訂檔案 `publish` ，請參閱 <xref:blazor/host-and-deploy/webassembly#use-a-custom-webconfig> 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-116">To use a custom `web.config` file for IIS that isn't overwritten when the app is published to the `publish` folder, see <xref:blazor/host-and-deploy/webassembly#use-a-custom-webconfig>.</span></span>

<span data-ttu-id="fefb1-117">藉由插入和讀取屬性，在元件中取得應用程式的環境 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.Environment> ：</span><span class="sxs-lookup"><span data-stu-id="fefb1-117">Obtain the app's environment in a component by injecting <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> and reading the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.Environment> property:</span></span>

```razor
@page "/"
@using Microsoft.AspNetCore.Components.WebAssembly.Hosting
@inject IWebAssemblyHostEnvironment HostEnvironment

<h1>Environment example</h1>

<p>Environment: @HostEnvironment.Environment</p>
```

<span data-ttu-id="fefb1-118">在啟動期間， <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder> <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> 會透過屬性公開 <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.HostEnvironment> ，讓開發人員在其程式碼中具有環境特定邏輯：</span><span class="sxs-lookup"><span data-stu-id="fefb1-118">During startup, the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder> exposes the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment> through the <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.WebAssemblyHostBuilder.HostEnvironment> property, which enables developers to have environment-specific logic in their code:</span></span>

```csharp
if (builder.HostEnvironment.Environment == "Custom")
{
    ...
};
```

<span data-ttu-id="fefb1-119">下列便利的擴充方法可讓您檢查目前的環境，以進行開發、生產、預備和自訂環境名稱：</span><span class="sxs-lookup"><span data-stu-id="fefb1-119">The following convenience extension methods permit checking the current environment for Development, Production, Staging, and custom environment names:</span></span>

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

<span data-ttu-id="fefb1-120"><xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType>當服務無法使用時，可以在啟動期間使用此屬性 <xref:Microsoft.AspNetCore.Components.NavigationManager> 。</span><span class="sxs-lookup"><span data-stu-id="fefb1-120">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Hosting.IWebAssemblyHostEnvironment.BaseAddress?displayProperty=nameWithType> property can be used during startup when the <xref:Microsoft.AspNetCore.Components.NavigationManager> service isn't available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fefb1-121">其他資源</span><span class="sxs-lookup"><span data-stu-id="fefb1-121">Additional resources</span></span>

* <xref:fundamentals/environments>
