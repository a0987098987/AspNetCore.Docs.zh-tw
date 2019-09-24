---
title: 適用于 ASP.NET Core 的 AspNetCore 應用程式中繼套件
author: Rick-Anderson
description: AspNetCore 應用程式共用架構
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/24/2019
uid: fundamentals/metapackage-app
ms.openlocfilehash: 8435445890ce00f33ab9a8692f5442b1609192da
ms.sourcegitcommit: 8a36be1bfee02eba3b07b7a86085ec25c38bae6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219106"
---
# <a name="microsoftaspnetcoreapp-for-aspnet-core"></a>ASP.NET Core 的 AspNetCore 應用程式

::: moniker range=">= aspnetcore-3.0"

 ASP.NET Core 共用架構（`Microsoft.AspNetCore.App`）包含由 Microsoft 開發及支援的元件。 `Microsoft.AspNetCore.App`安裝[.Net Core 3.0 或更新版本的 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)時，會進行安裝。 *共用架構*是一組安裝在電腦上的元件（ *.dll*檔案），其中包含執行時間元件和目標套件。 如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。

* 以 SDK 為目標`Microsoft.NET.Sdk.Web`的專案會隱含`Microsoft.AspNetCore.App`地參考架構。

這些專案不需要其他參考：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>
    ...
</Project>
```

ASP.NET Core 共用架構：

* 不包含協力廠商相依性。
* 包含 ASP.NET Core 小組所支援的所有封裝。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。

ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [中繼套件](/dotnet/core/packages#metapackages)：

* 不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。 這些協力廠商相依性是確保主要架構功能運作的必要項。
* 包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。
* 包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。

`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。 以 ASP.NET Core 2.x 為目標的預設專案範本會使用此套件。 我們建議以 ASP.NET Core 2.x 和 Entity Framework Core 2.x 為目標的`Microsoft.AspNetCore.App`應用程式使用套件。

`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表最低的 ASP.NET Core 版本和 Entity Framework Core 版本。

使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：

* 如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。
* 其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。
* 版本一致性有助於確保可靠的體驗。 `Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。

使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。 當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**在應用程式中部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。 共用架構中的資產會先行編譯，以改善應用程式啟動時間。 如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。

下列專案檔會參考`Microsoft.AspNetCore.App` ASP.NET Core 的中繼套件，並代表一般的 ASP.NET Core 2.2 範本：

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

上述標記代表一般 ASP.NET Core 2.x 範本。 它不會指定 `Microsoft.AspNetCore.App` 套件參考的版本號碼。 未指定版本時，SDK 會指定[隱含](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)版本，也就是 `Microsoft.NET.Sdk.Web`。 建議依賴 SDK 指定的隱含版本，而不要明確設定套件參考的版本號碼。 如果您對此方法有疑問，請在 [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/AspNetCore.Docs/issues/6430) (Microsoft.AspNetCore.App 隱含版本討論區) 留下 GitHub 意見。

可攜式應用程式的隱含版本會設定為 `major.minor.0`。 共用架構向前復原機制會在已安裝共用架構中的最新相容版本上執行應用程式。 為了保證開發、測試和生產均使用相同版本，請務必在所有環境中安裝相同版本的共用架構。 針對獨立應用程式，隱含版本號碼會設定為已安裝 SDK 隨附之共用架構的 `major.minor.patch`。

指定 `Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會選擇該版本的共用架構。 例如，假設已指定 "2.2.1" 版本，但已安裝 "2.2.3"。 在此情況下，應用程式將會使用 "2.2.3"。 您可以停用向前復原 (修補及/或次要)，但不建議這樣做。 如需 dotnet 主機向前復原及如何設定其行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

`<Project Sdk` 必須設定為 `Microsoft.NET.Sdk.Web`，才能使用隱含版本 `Microsoft.AspNetCore.App`。 當使用 `<Project Sdk="Microsoft.NET.Sdk">` (不含後置 `.Web`) 時：

* 會產生下列警告：

  *警告 NU1604：專案相依性 Microsoft.AspNetCore.App 不包含界限下限 (含)。請在相依性版本包含下限，以確保還原結果一致。*

* 這是 .NET Core 2.1 SDK 的已知問題。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<a name="update"></a>

## <a name="update-aspnet-core"></a>更新 ASP.NET Core

`Microsoft.AspNetCore.App` [中繼套件](/dotnet/core/packages#metapackages)不是從 NuGet 更新的傳統套件。 類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。 如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。

更新 ASP.NET Core：

* 在開發電腦和組建伺服器上：下載並安裝 [.NET Core SDK](https://www.microsoft.com/net/download)。
* 在部署伺服器上：下載並安裝 [.NET Core 執行階段](https://www.microsoft.com/net/download)。

 應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。 您不必更新專案檔中的 `Microsoft.AspNetCore.App` 版本號碼。 如需詳細資訊，請參閱[向前復原 Framework 相依的應用程式](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。

如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。

::: moniker-end
