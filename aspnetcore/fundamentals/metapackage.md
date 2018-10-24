---
title: ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.All 中繼套件不建議用於 ASP.NET Core 2.1 和更新版本。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 1942426dbd5c15ae4a5fa5fbb931b94f50aa6043
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454735"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a>ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件

> [!NOTE]
> 建議以 ASP.NET Core 2.1 及更新版本為目標的應用程式使用 [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app)，而非本套件。 請參閱本文章中的[從 Microsoft.AspNetCore.All 移轉到 Microsoft.AspNetCore.App](#migrate)。

這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。

ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：

* 所有由 ASP.NET Core 小組支援的套件。
* 所有由 Entity Framework Core 支援的套件。
* ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。

`Microsoft.AspNetCore.All` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。 以 ASP.NET Core 2.0 為目標的預設專案範本會使用此套件。

`Microsoft.AspNetCore.All` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。

使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用 [.NET Core 執行階段存放區](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)。 此執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。 當您使用 `Microsoft.AspNetCore.All` 中繼套件時，**不會**使用應用程式部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; .NET Core 執行階段存放區包含這些資產。 執行階段存放區中的資產會先行編譯，以改善應用程式啟動時間。

您可以使用套件修剪流程來移除未使用的套件。 發行的應用程式輸出中會排除已修剪的套件。

下列 *.csproj* 檔案參考 ASP.NET Core 的 `Microsoft.AspNetCore.All` 中繼套件：

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a>從 Microsoft.AspNetCore.All 移轉到 Microsoft.AspNetCore.App

下列套件隨附於 `Microsoft.AspNetCore.All`，而不是 `Microsoft.AspNetCore.App` 套件。 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

若要從 `Microsoft.AspNetCore.All` 移到 `Microsoft.AspNetCore.App`，如果您的應用程式使用來自上述套件的任何 API，或這些套件中隨附的套件，請將參考新增到專案中的這些套件。

若前述套件的任何相依性不是 `Microsoft.AspNetCore.App` 的相依性，即不包含在內。 例如: 

* `StackExchange.Redis` 為 `Microsoft.Extensions.Caching.Redis` 的相依性
* `Microsoft.ApplicationInsights` 為 `Microsoft.AspNetCore.ApplicationInsights.HostingStartup` 的相依性

## <a name="update-aspnet-core-21"></a>更新 ASP.NET Core 2.1

建議您遷移至 `Microsoft.AspNetCore.App` 中繼套件 2.1 和更新版本。 若要繼續使用 `Microsoft.AspNetCore.All` 中繼套件並確保已部署最新的更新程式版本：

* 在開發電腦和組建伺服器上：安裝最新的 [.NET Core SDK](https://www.microsoft.com/net/download)。
* 在部署伺服器上：安裝最新的 [.NET Core 執行階段](https://www.microsoft.com/net/download)。
 您的應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。
