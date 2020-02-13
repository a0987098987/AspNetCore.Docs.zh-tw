---
title: 從 ASP.NET 移轉至 ASP.NET Core
author: isaac2004
description: 取得將現有 ASP.NET MVC 或 Web API 應用程式，移轉至 ASP.NET Core.web 的指導
ms.author: scaddie
ms.date: 10/18/2019
uid: migration/proper-to-2x/index
ms.openlocfilehash: e9ebfa7352350cf39917e515a1a66d6271829f38
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172345"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a>從 ASP.NET 移轉至 ASP.NET Core

作者：[Isaac Levin](https://isaaclevin.com)

這篇文章可作為將 ASP.NET 應用程式移轉至 ASP.NET Core 的參考指南。

## <a name="prerequisites"></a>必要條件

[.NET Core SDK 2.2 或更新版本](https://www.microsoft.com/net/download)

## <a name="target-frameworks"></a>目標 Framework

ASP.NET Core 專案為開發人員提供了彈性，能以 .NET Core、.NET Framework 或兩者為目標。 請參閱[針對伺服器應用程式在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/standard/choosing-core-framework-server)，判斷哪些目標架構最適合。

以 .NET Framework 為目標時，專案需要參考個別的 NuGet 套件。

以 .NET Core 為目標，借助 ASP.NET Core [中繼套件](xref:fundamentals/metapackage-app)，可讓您消除許多明確的套件參考。 在您的專案中安裝 `Microsoft.AspNetCore.App` 中繼套件：

```xml
<ItemGroup>
   <PackageReference Include="Microsoft.AspNetCore.App" />
</ItemGroup>
```

使用中繼套件時，不使用應用程式部署中繼套件中參考的任何套件。 .NET Core 執行階段存放區包含這些資產，而且它們會先行編譯以改善效能。 如需詳細資訊，請參閱 [ASP.NET Core 的 Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。

## <a name="project-structure-differences"></a>專案結構差異

ASP.NET Core 中已簡化 *.csproj* 檔案格式。 值得注意的變更包括：

- 檔案不需要明確包含也會被視為專案的一部分。 在處理大型小組時，這會減少 XML 合併衝突的風險。
- 其他專案沒有可改善檔案可讀性之以 GUID 為基礎的參考。
- 您可以編輯檔案，卻不用在 Visual Studio 中卸載它：

    ![在 Visual Studio 2017 中編輯 CSPROJ 操作功能表選項](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>取代 Global.asax 檔案

ASP.NET Core 導入了啟動應用程式的新機制。 ASP.NET 應用程式的進入點是 *Global.asax* 檔案。 路由組態和篩選器和區域登錄等工作，會在 *Global.asax* 檔案中處理。

[!code-csharp[](samples/globalasax-sample.cs)]

這個方法是以會影響到實作的方式，將應用程式和其部署所在的伺服器結合在一起。 為將它們分開，引進了 [OWIN](https://owin.org/) 以提供簡潔的方式，同時使用多個架構。 OWIN 提供的管線只新增所需的模組。 裝載環境採用 [Startup](xref:fundamentals/startup) 函式，設定服務和應用程式的要求管線。 `Startup` 向應用程式註冊一組中介軟體。 對於每項要求，應用程式會使用現有處理常式集合連結清單的標頭指標，呼叫每個中介軟體元件。 每個中介軟體元件都可以在要求處理管線新增一或多個處理常式。 這項作業是透過將參考傳回處理常式所完成，而此處理常式為清單的新標頭。 每個處理常式都負責記住和叫用清單中的下一個處理常式。 使用 ASP.NET Core，應用程式的進入點是 `Startup`，對 *Global.asax* 不會再有相依性。 使用 OWIN 和 .NET Framework 時，請將類似下列的項目當成管線使用：

[!code-csharp[](samples/webapi-owin.cs)]

這會設定您的預設路由，並透過 JSON 將 XmlSerialization 設為預設值。 視需要在此管線新增其他中介軟體 (載入服務、組態設定、靜態檔案等等)。

ASP.NET Core 使用類似的方法，但不依賴 OWIN 處理項目。 而是透過*Program.cs* `Main` 方法（類似于主控台應用程式）來完成，而且 `Startup` 會透過該處載入。

[!code-csharp[](samples/program.cs)]

`Startup` 必須包含 `Configure` 方法。 在 `Configure` 中將必要的中介軟體新增至管線。 在下列範例 (來自從預設的網站範本) 中，擴充方法會使用對下列項目的支援來設定管線：

- 錯誤頁面
- HTTP 嚴格的傳輸安全性
- HTTP 重新導向 到 HTTPS
- ASP.NET Core MVC

[!code-csharp[](samples/startup.cs)]

主機與應用程式已分離，這讓您未來可以彈性移至不同的平台。

> [!NOTE]
> 如需 ASP.NET Core 啟動與中介軟體更深入的參考，請參閱 [ASP.NET Core 中的啟動](xref:fundamentals/startup)

## <a name="store-configurations"></a>儲存組態

ASP.NET 支援儲存設定。 例如，這些設定是用來支援要部署應用程式的環境。 過去的常見做法是將所有自訂機碼值組儲存在 `<appSettings>`Web.config*檔案的* 區段中：

[!code-xml[](samples/webconfig-sample.xml)]

應用程式會讀取 `ConfigurationManager.AppSettings` 命名空間中這些使用 `System.Configuration` 集合的設定：

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core 可將應用程式的組態資料儲存在任何檔案中，將它們當成中介軟體啟動程序的一部分載入。 專案範本中所用的預設檔案是 *appsettings.json*：

[!code-json[](samples/appsettings-sample.json)]

將此檔案載入應用程式內的 `IConfiguration` 執行個體，是在 *Startup.cs* 中完成：

[!code-csharp[](samples/startup-builder.cs)]

應用程式讀取自 `Configuration` 以取得設定：

[!code-csharp[](samples/read-appsettings.cs)]

此方法有延伸模組可讓處理序更強固，例如使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 載入具有這些值的服務。 DI 方法提供強型別的組態物件集合。

```csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
```

> [!NOTE]
> 如需 ASP.NET Core 組態更深入的參考，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index)。

## <a name="native-dependency-injection"></a>原生相依性插入

建置可延展的大型應用程式時，鬆散的元件和服務結合程度就是重要的目標。 [相依性插入](xref:fundamentals/dependency-injection)是達到此目標的常用技巧，它也是 ASP.NET Core 的原生元件。

在 ASP.NET 應用程式中，開發人員仰賴協力廠商程式庫來實作插入相依性。 Microsoft 模式和實務提供的 [Unity](https://github.com/unitycontainer/unity) 就是這樣的程式庫。

使用 Unity 設定相依性插入的範例，是實作包裝 `IDependencyResolver` 的 `UnityContainer`：

[!code-csharp[](samples/sample8.cs)]

建立您 `UnityContainer` 的執行個體、註冊您的服務，以及為容器設定 `HttpConfiguration` 新執行個體的 `UnityResolver` 相依性解析程式：

[!code-csharp[](samples/sample9.cs)]

在需要的位置插入 `IProductRepository`：

[!code-csharp[](samples/sample5.cs)]

因為相依性插入是 ASP.NET Core 的一部分，所以您可以在 `ConfigureServices`Startup.cs*的* 方法中新增服務：

[!code-csharp[](samples/configure-services.cs)]

存放庫可插入任何位置，就像以前的 Unity 一樣。

> [!NOTE]
> 如需有關相依性插入的詳細資訊，請參閱[相依性插入](xref:fundamentals/dependency-injection)。

## <a name="serve-static-files"></a>提供靜態檔案

網頁程式開發很重要的一部分是能夠提供靜態的用戶端資產。 最常見的靜態檔案範例包括 HTML、CSS、Javascript 和影像。 這些檔案需要儲存在應用程式 (或 CDN) 的發佈位置供參考，以便要求可以載入它們。 此程序在 ASP.NET Core 中已變更。

在 ASP.NET 中，靜態檔案會儲存在不同目錄中，於檢視中提供參考。

在 ASP.NET Core 中，靜態檔案會儲存在「Web 根目錄」( *&lt;內容根目錄&gt;/wwwroot*) 中，除非另行設定。 從 `UseStaticFiles` 叫用 `Startup.Configure` 擴充方法，將檔案載入至要求管線：

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> 如以 .NET Framework 為目標，請安裝 NuGet 套件 `Microsoft.AspNetCore.StaticFiles`。

例如，位在 *等位置的瀏覽器可存取*wwwroot/images`http://<app>/images/<imageFileName>` 資料夾中的影像資產。

> [!NOTE]
> 如需在 ASP.NET Core 中提供靜態檔案更深入的參考，請參閱[靜態檔案](xref:fundamentals/static-files)。

## <a name="multi-value-cookies"></a>多重值 cookie

ASP.NET Core 中不支援[多重值的 cookie](xref:System.Web.HttpCookie.Values) 。 為每個值建立一個 cookie。

## <a name="partial-app-migration"></a>部分應用程式遷移

部分應用程式遷移的其中一個方法是建立 IIS 子應用程式，並只將特定路由從 ASP.NET 4.x 移至 ASP.NET Core，同時保留該應用程式的 URL 結構。 例如，請考慮*applicationhost.config*檔案中應用程式的 URL 結構：

```xml
<sites>
    <site name="Default Web Site" id="1" serverAutoStart="true">
        <application path="/">
            <virtualDirectory path="/" physicalPath="D:\sites\MainSite\" />
        </application>
        <application path="/api" applicationPool="DefaultAppPool">
            <virtualDirectory path="/" physicalPath="D:\sites\netcoreapi" />
        </application>
        <bindings>
            <binding protocol="http" bindingInformation="*:80:" />
            <binding protocol="https" bindingInformation="*:443:" sslFlags="0" />
        </bindings>
    </site>
    ...
</sites>
```

目錄結構：

```
.
├── MainSite
│   ├── ...
│   └── Web.config
└── NetCoreApi
    ├── ...
    └── web.config
```

## <a name="additional-resources"></a>其他資源

- [將程式庫移轉到 .NET Core](/dotnet/core/porting/libraries)
