---
title: 使用驗證程式庫保護 ASP.NET Core Blazor WebAssembly 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/standalone-with-authentication-library
ms.openlocfilehash: f9cc2884dcd94c729c45a056ae4327a2c75d34be
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083752"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-standalone-app-with-the-authentication-library"></a>使用驗證程式庫保護 ASP.NET Core Blazor WebAssembly 獨立應用程式

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

若要建立使用 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 程式庫的 Blazor WebAssembly 獨立應用程式，請在命令 shell 中執行下列命令：

```dotnetcli
dotnet new blazorwasm -au Individual
```

若要指定輸出位置（如果它不存在，則會建立專案資料夾），請在命令中包含一個路徑（例如 `-o BlazorSample`）的輸出選項。 資料夾名稱也會成為專案名稱的一部分。

在 Visual Studio 中，[建立 Blazor WebAssembly 應用程式](xref:blazor/get-started)。 使用 [**儲存使用者帳戶應用程式內**] 選項，將**驗證**設定為**個別使用者帳戶**。

## <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別使用者帳戶時，應用程式會在應用程式的專案檔中自動接收 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件的套件參考。 封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

以 <xref:blazor/get-started> 文章中顯示的 `Microsoft.AspNetCore.Blazor.Templates` 套件版本取代先前套件參考中的 `{VERSION}`。

## <a name="authentication-service-support"></a>驗證服務支援

驗證使用者的支援是以 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 套件所提供的 `AddOidcAuthentication` 擴充方法，在服務容器中註冊。 這個方法會設定應用程式與身分識別提供者（IP）互動所需的所有服務。

*Program.cs*：

```csharp
builder.Services.AddOidcAuthentication(options =>
{
    options.ProviderOptions.Authority = "{AUTHORITY}";
    options.ProviderOptions.ClientId = "{CLIENT ID}";
});
```

獨立應用程式的驗證支援是使用 Open ID Connect （OIDC）提供。 `AddOidcAuthentication` 方法會接受回呼，以使用 OIDC 來設定驗證應用程式所需的參數。 設定應用程式所需的值可以從 IP 取得，例如 Google、Microsoft 或其他 OIDC 相容的提供者。 當您註冊應用程式時，請取得這些值，這通常會發生在其線上入口網站中。

## <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page.md)]

## <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a>LoginDisplay 元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
