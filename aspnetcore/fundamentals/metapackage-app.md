---
title: ASP.NET Core 2.1 和更新版本的 Microsoft.AspNetCore.App 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.App 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 95fd6b7e73cf325674f1c1e03f9eea88cbc1af13
ms.sourcegitcommit: f3538693a12cf55b7f124a6519677239170b7c43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "43114771"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>ASP.NET Core 2.1 的 Microsoft.AspNetCore.App 中繼套件

這項功能需要以 .NET Core 2.1 和更新版本為目標的 ASP.NET Core 2.1 和更新版本。

ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [中繼套件](/dotnet/core/packages#metapackages)：

* 不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。 這些協力廠商相依性是確保主要架構功能運作的必要項。
* 包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。
* 包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。

`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本的所有功能。 以 ASP.NET Core 2.1 和更新版本為目標的預設專案範本會使用此套件。 建議以 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本為目標的應用程式使用 `Microsoft.AspNetCore.App` 套件。

`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。

使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：

* 如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。
* 其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。
* 版本一致性有助於確保可靠的體驗。 `Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。

使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。 當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**在應用程式中部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。 共用架構中的資產會先行編譯，以改善應用程式啟動時間。 如需詳細資訊，請參閱 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)中的「共用架構」。

下列專案檔參考 ASP.NET Core 的 `Microsoft.AspNetCore.App` 中繼套件，並呈現出標準的 ASP.NET Core 2.1 範本：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.1" />
  </ItemGroup>

</Project>
```

`Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會使用該版本的共用架構。 例如，假設指定版本 `2.1.1`，但安裝 `2.1.3`。 在此情況下，應用程式會使用 `2.1.3`。 您可以停用向前復原行為 (修補及 (或) 次要)，但不建議這樣做。 如需套件版本向前復原行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。

## <a name="update-aspnet-core"></a>更新 ASP.NET Core

`Microsoft.AspNetCore.App` [中繼套件](/dotnet/core/packages#metapackages)不是從 NuGet 更新的傳統套件。 類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。 如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。

更新 ASP.NET Core：

* 在開發電腦和組建伺服器上：下載並安裝 [.NET Core SDK](https://www.microsoft.com/net/download)。
* 在部署伺服器上：下載並安裝 [.NET Core 執行階段](https://www.microsoft.com/net/download)。

 應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。 您不必更新專案檔中的 `Microsoft.AspNetCore.App` 版本號碼。 如需詳細資訊，請參閱[向前復原 Framework 相依的應用程式](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。

如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。
