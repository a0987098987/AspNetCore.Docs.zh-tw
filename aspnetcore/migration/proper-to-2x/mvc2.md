---
title: 從 ASP.NET 遷移至 ASP.NET Core 2.0
author: isaac2004
description: 收到現有 ASP.NET MVC 或 Web API 應用程式移轉至 ASP.NET Core 2.0 的指導的方針。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/24/2018
uid: migration/mvc2
ms.openlocfilehash: 9960932bd288ea12e346272f1838026778f1d355
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148859"
---
# <a name="migrate-from-aspnet-to-aspnet-core-20"></a>從 ASP.NET 遷移至 ASP.NET Core 2.0

作者：[Isaac Levin](https://isaaclevin.com)

這篇文章可作為 ASP.NET 應用程式移轉至 ASP.NET Core 2.0 的參考指南。

## <a name="prerequisites"></a>必要條件

安裝**一個**的 從下列[.NET 下載： Windows](https://www.microsoft.com/net/download/windows):

* .NET Core SDK
* Visual Studio for Windows
  * **ASP.NET 與網頁程式開發**工作負載
  * **.NET Core 跨平台開發**工作負載

## <a name="target-frameworks"></a>目標 Framework

ASP.NET Core 2.0 專案讓開發人員能夠彈性以 .NET Core、.NET Framework 或兩者為目標。 請參閱[針對伺服器應用程式在 .NET Core 和 .NET Framework 之間進行選擇](/dotnet/standard/choosing-core-framework-server)，判斷哪些目標架構最適合。

以 .NET Framework 為目標時，專案需要參考個別的 NuGet 套件。

以 .NET Core 為目標，可讓您消除許多明確的套件參考，這點多虧了 ASP.NET Core 2.0 [中繼套件](xref:fundamentals/metapackage)。 在您的專案中安裝 `Microsoft.AspNetCore.All` 中繼套件：

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
</ItemGroup>
```

使用中繼套件時，不使用應用程式部署中繼套件中參考的任何套件。 .NET Core 執行階段存放區包含這些資產，而且它們會先行編譯以改善效能。 請參閱<xref:fundamentals/metapackage>如需詳細資訊。

## <a name="project-structure-differences"></a>專案結構差異

ASP.NET Core 中已簡化 *.csproj* 檔案格式。 值得注意的變更包括：

* 檔案不需要明確包含也會被視為專案的一部分。 在處理大型小組時，這會減少 XML 合併衝突的風險。
* 其他專案沒有可改善檔案可讀性之以 GUID 為基礎的參考。
* 您可以編輯檔案，卻不用在 Visual Studio 中卸載它：

  ![在 Visual Studio 2017 中編輯 CSPROJ 操作功能表選項](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>取代 Global.asax 檔案

ASP.NET Core 導入了啟動應用程式的新機制。 ASP.NET 應用程式的進入點是 *Global.asax* 檔案。 路由組態和篩選器和區域登錄等工作，會在 *Global.asax* 檔案中處理。

[!code-csharp[](samples/globalasax-sample.cs)]

這個方法是以會影響到實作的方式，將應用程式和其部署所在的伺服器結合在一起。 為將它們分開，引進了 [OWIN](http://owin.org/) 以提供簡潔的方式，同時使用多個架構。 OWIN 提供的管線只新增所需的模組。 裝載環境採用 [Startup](xref:fundamentals/startup) 函式，設定服務和應用程式的要求管線。 `Startup` 向應用程式註冊一組中介軟體。 對於每項要求，應用程式會使用現有處理常式集合連結清單的標頭指標，呼叫每個中介軟體元件。 每個中介軟體元件都可以在要求處理管線新增一或多個處理常式。 這項作業是透過將參考傳回處理常式所完成，而此處理常式為清單的新標頭。 每個處理常式都負責記住和叫用清單中的下一個處理常式。 使用 ASP.NET Core，應用程式的進入點是 `Startup`，對 *Global.asax* 不會再有相依性。 使用 OWIN 和 .NET Framework 時，請將類似下列的項目當成管線使用：

[!code-csharp[](samples/webapi-owin.cs)]

這會設定您的預設路由，並透過 JSON 將 XmlSerialization 設為預設值。 視需要在此管線新增其他中介軟體 (載入服務、組態設定、靜態檔案等等)。

ASP.NET Core 使用類似的方法，但不依賴 OWIN 處理項目。 相反地，這會透過 *Program.cs* `Main` 方法完成 (類似主控台應用程式)，而 `Startup` 也是透過該處載入。

[!code-csharp[](samples/program.cs)]

`Startup` 必須包含 `Configure` 方法。 在 `Configure` 中將必要的中介軟體新增至管線。 在下列範例中 (從預設的網站範本)，會使用數個擴充方法設定管線，以支援：

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* 錯誤頁面
* 靜態檔案
* ASP.NET Core MVC
* 身分識別

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

主機與應用程式已分離，這讓您未來可以彈性移至不同的平台。

ASP.NET Core 啟動和中介軟體的更深入參考，請參閱<xref:fundamentals/startup>。

## <a name="storing-configurations"></a>正在儲存組態

ASP.NET 支援儲存設定。 例如，這些設定是用來支援要部署應用程式的環境。 過去的常見做法是將所有自訂機碼值組儲存在 *Web.config* 檔案的 `<appSettings>` 區段中：

[!code-xml[](samples/webconfig-sample.xml)]

應用程式會讀取 `System.Configuration` 命名空間中這些使用 `ConfigurationManager.AppSettings` 集合的設定：

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core 可將應用程式的組態資料儲存在任何檔案中，將它們當成中介軟體啟動程序的一部分載入。 專案範本中所用的預設檔案是 *appsettings.json*：

[!code-json[](samples/appsettings-sample.json)]

將此檔案載入應用程式內的 `IConfiguration` 執行個體，是在 *Startup.cs* 中完成：

[!code-csharp[](samples/startup-builder.cs)]

應用程式讀取自 `Configuration` 以取得設定：

[!code-csharp[](samples/read-appsettings.cs)]

此方法有延伸模組可讓處理序更強固，例如使用[相依性插入](xref:fundamentals/dependency-injection) (DI) 載入具有這些值的服務。 DI 方法提供強型別的組態物件集合。

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**注意︰** ASP.NET Core 組態更深入的參考，請參閱<xref:fundamentals/configuration/index>。

## <a name="native-dependency-injection"></a>原生相依性插入

建置可延展的大型應用程式時，鬆散的元件和服務結合程度就是重要的目標。 [相依性插入](xref:fundamentals/dependency-injection)是常用的技巧，為達到此目標，而且是 ASP.NET Core 的原生元件。

在 ASP.NET 應用程式，開發人員會依賴協力廠商程式庫實作相依性插入。 Microsoft 模式和實務提供的 [Unity](https://github.com/unitycontainer/unity) 就是這樣的程式庫。

設定使用 Unity 的相依性插入的範例實作`IDependencyResolver`包裝`UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

建立您 `UnityContainer` 的執行個體、註冊您的服務，以及為容器設定 `UnityResolver` 新執行個體的 `HttpConfiguration` 相依性解析程式：

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

在需要的位置插入 `IProductRepository`：

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

因為相依性插入是 ASP.NET Core 的一部分，您可以新增您的服務中`Startup.ConfigureServices`:

[!code-csharp[](samples/configure-services.cs)]

存放庫可插入任何位置，就像以前的 Unity 一樣。

如需有關在 ASP.NET Core 中的相依性插入的詳細資訊，請參閱<xref:fundamentals/dependency-injection>。

## <a name="serving-static-files"></a>提供靜態檔案

網頁程式開發很重要的一部分是能夠提供靜態的用戶端資產。 最常見的靜態檔案範例包括 HTML、CSS、Javascript 和影像。 這些檔案需要儲存在應用程式 (或 CDN) 的發佈位置供參考，以便要求可以載入它們。 此程序在 ASP.NET Core 中已變更。

在 ASP.NET 中，靜態檔案會儲存在不同目錄中，於檢視中提供參考。

在 ASP.NET Core 中，靜態檔案會儲存在「Web 根目錄」(*&lt;內容根目錄&gt;/wwwroot*) 中，除非另行設定。 從 `Startup.Configure` 叫用 `UseStaticFiles` 擴充方法，將檔案載入至要求管線：

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**注意：** 如以 .NET Framework 為目標，請安裝 NuGet 套件 `Microsoft.AspNetCore.StaticFiles`。

例如，位在 `http://<app>/images/<imageFileName>` 等位置的瀏覽器可存取 *wwwroot/images* 資料夾中的影像資產。

**注意︰** 提供 ASP.NET Core 中的靜態檔案的更深入參考，請參閱<xref:fundamentals/static-files>。

## <a name="additional-resources"></a>其他資源

* [將程式庫移轉到 .NET Core](/dotnet/core/porting/libraries)
