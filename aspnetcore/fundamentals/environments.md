---
title: "使用多個環境中 ASP.NET Core"
author: ardalis
description: "了解 ASP.NET Core 應用程式行為控制跨多個環境所提供的支援。"
keywords: "ASP.NET Core，環境設定，ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: becdfa647acb6483b39f5421ab881c4817f31c40
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2017
---
# <a name="working-with-multiple-environments"></a>使用多個環境

由[Steve Smith](https://ardalis.com/)

ASP.NET Core 提供支援跨多個環境，例如開發、 預備及生產環境中控制應用程式行為。 表示執行階段環境中，允許應用程式設定為該環境使用環境變數。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>開發、 暫存、 生產環境

ASP.NET Core 參考特定環境變數，`ASPNETCORE_ENVIRONMENT`來描述應用程式目前執行中的環境。 此變數可以設定任何值，但是慣例會使用三個值： `Development`， `Staging`，和`Production`。 您會發現這些範例中所使用的值和 ASP.NET Core 提供的範本。

目前的環境設定可以偵測到以程式設計方式從您的應用程式內。 此外，您可以使用環境[標記協助程式](../mvc/views/tag-helpers/index.md)中的特定區段中加入您[檢視](../mvc/views/index.md)根據目前的應用程式環境。

注意： 在 Windows 及 macOS，指定的環境名稱是不區分大小寫。 是否將變數設`Development`或`development`或`DEVELOPMENT`結果將會相同。 不過，Linux 是**區分大小寫**預設的作業系統。 環境變數、 檔案名稱和設定需要區分大小寫。

### <a name="development"></a>開發

這應該是開發應用程式時使用的環境。 它通常用來啟用功能，您不想要可供使用，當應用程式執行於生產環境，例如[開發人員例外狀況頁面](xref:fundamentals/error-handling#the-developer-exception-page)。

如果您使用 Visual Studio，可以在專案的偵錯設定檔中設定環境。 偵錯設定檔指定[伺服器](xref:fundamentals/servers/index)来設定啟動應用程式和任何環境變數時使用。 您的專案可以有多個以不同方式設定環境變數的偵錯設定檔。 使用管理這些設定檔**偵錯**] 索引標籤的 [web 應用程式專案的**屬性**功能表。 您在專案屬性中設定的值會保存在*launchSettings.json*檔案，而且您也可以設定設定檔藉由直接編輯該檔案。

IIS Express 的設定檔如下所示：

![專案屬性設定環境變數](environments/_static/project-properties-debug.png)

以下是`launchSettings.json`檔案，其中包含的設定檔`Development`和`Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

專案設定檔所做的變更可能不會生效，直到重新啟動使用網頁伺服器 （特別是，Kestrel 必須重新啟動之前它會偵測到它的環境所做的變更）。

>[!WARNING]
> 環境變數會儲存在*launchSettings.json*並未受到保護以任何方式，並將原始程式碼儲存機制在專案的一部分，如果您使用其中一個。 **絕對不要儲存這個檔案中的認證或其他機密資料。** 如果您需要儲存這類資料的位置，使用*密碼管理員*中所述的工具[安全存放應用程式密碼，在開發期間](xref:security/app-secrets)。

### <a name="staging"></a>預備環境

依照慣例，`Staging`環境是用於部署至生產環境前進行最終測試進入生產階段前環境。 在理想情況下，其實體特性應該會鏡像的生產環境中，，以便在生產環境中可能會發生任何問題進行第一次在臨時環境中，而不會影響使用者來處理。

### <a name="production"></a>生產環境

`Production`環境是即時時，執行應用程式的環境，使用者正在使用。 此環境應該設定為最大化安全性、 效能及應用程式的健全性。 某些常見設定生產環境中可能會不同於開發包括：

* 開啟快取

* 請確認所有用戶端資源配套、 縮短，而且可能由 CDN

* 關閉診斷 ErrorPages

* 開啟易懂的錯誤頁面

* 啟用記錄和監視的生產環境 (例如， [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

這是不是當做完整清單。 您最好避免均勻環境檢查在您的應用程式的許多部分。 相反地，建議的方法是執行中應用程式的這類檢查`Startup`類別可能的情況下

## <a name="setting-the-environment"></a>設定環境

設定環境的方法取決於作業系統。

### <a name="windows"></a>Windows
若要設定`ASPNETCORE_ENVIRONMENT`目前工作階段，如果應用程式會使用啟動`dotnet run`，使用下列命令

**命令列**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

這些命令才會生效，僅針對目前的視窗。 視窗關閉時，ASPNETCORE_ENVIRONMENT 設定會還原為預設值或 machine 值中。 若要開啟 Windows 全域設定的值**控制台** > **系統** > **進階系統設定**以及新增或編輯`ASPNETCORE_ENVIRONMENT`值。

![進階屬性的系統](environments/_static/systemsetting_environment.png)

![ASPNET 核心環境變數](environments/_static/windows_aspnetcore_environment.png) 

**web.config**

請參閱*設定環境變數*區段[ASP.NET 核心模組的組態參考](xref:hosting/aspnet-core-module#setting-environment-variables)主題。

**每個 IIS 應用程式集區**

如果您需要設定在隔離的應用程式集區中執行之個別應用程式的環境變數 (IIS 10.0+ 支援)，請參閱 IIS 參考文件之[環境變數\<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) 主題的 *AppCmd.exe 命令*一節。

### <a name="macos"></a>MacOS
設定 macOS 的目前環境可以是內建作業時完成執行應用程式。

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
或使用`export`設定之前執行應用程式。

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
電腦層級環境變數中設定*.bashrc*或*.bash_profile*檔案。 編輯檔案使用任何文字編輯器，並加入下列陳述式。

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
針對 Linux 散發版本，請使用`export`工作階段以變數設定為基礎的命令在命令列和*bash_profile*機器層級的環境設定的檔案。

## <a name="determining-the-environment-at-runtime"></a>判斷在執行階段環境

`IHostingEnvironment`服務所提供的核心概念與環境搭配使用。 這個服務是由 ASP.NET 裝載圖層，而且可以插入您的啟動邏輯透過[相依性插入](dependency-injection.md)。 ASP.NET Core web 網站範本在 Visual Studio 中的使用這個方法載入特定環境的組態檔 （如果有的話），以及自訂應用程式的錯誤處理設定。 在這兩種情況下，此行為藉由呼叫目前指定的環境參考來達成`EnvironmentName`或`IsEnvironment`的執行個體上`IHostingEnvironment`傳遞至適當的方法。

> [!NOTE]
> 如果您要檢查應用程式是否正在執行中的特定環境使用`env.IsEnvironment("environmentname")`因為正確，就會忽略大小寫 (而不是檢查`env.EnvironmentName == "Development"`例如)。

例如，您可以使用下列程式碼在您設定的方法設定環境特定的錯誤處理：

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

如果應用程式在執行`Development`環境中，則可讓使用 Visual Studio，開發特定的錯誤網頁 （這通常不應在生產環境中） 和特殊的資料庫錯誤中的"BrowserLink 」 功能所需的執行階段支援頁面 （其提供一個方式來套用移轉，因此應該只用於開發工作）。 否則，如果未在開發環境中執行應用程式，標準錯誤事件處理常式設定為會顯示在任何未處理的例外狀況的回應。

若要判斷要傳送給用戶端在執行階段，根據目前的環境的內容。 例如，在開發環境中您通常做非最小化指令碼和樣式表，可簡化偵錯更容易。 生產和測試環境應該用於縮短的版本，通常是從 CDN。 您可以使用環境[標記協助程式](../mvc/views/tag-helpers/intro.md)。 環境標記協助程式只會呈現其內容，如果目前的環境符合使用指定的環境的其中一個`names`屬性。

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

若要開始使用您的應用程式請參閱使用標記 helper[標記協助程式簡介](../mvc/views/tag-helpers/intro.md)。

## <a name="startup-conventions"></a>啟動慣例

ASP.NET Core 支援以慣例為基礎的方式來設定應用程式的啟動根據目前的環境。 您也以程式設計方式可以控制您的應用程式的行為方式根據要何種環境處於，可讓您建立及管理自己的慣例。

ASP.NET Core 應用程式啟動時，`Startup`類別用來啟動載入應用程式時，載入其組態設定等 ([深入了解 ASP.NET 啟動](startup.md))。 不過，如果存在的類別命名為`Startup{EnvironmentName}`(例如`StartupDevelopment`)，而`ASPNETCORE_ENVIRONMENT`環境變數符合該名稱，然後，`Startup`改為使用類別。 因此，您可以設定`Startup`進行開發，但有不同的`StartupProduction`，會在生產環境中執行應用程式時使用。 反之亦然。

> [!NOTE]
> 呼叫`WebHostBuilder.UseStartup<TStartup>()`覆寫組態區段。

除了使用完全不同`Startup`類別根據目前的環境中，您也可以進行調整應用程式內的設定方式`Startup`類別。 `Configure()`和`ConfigureServices()`方法支援類似於的環境特定版本`Startup`類別本身的表單`Configure{EnvironmentName}()`和`Configure{EnvironmentName}Services()`。 如果定義方法`ConfigureDevelopment()`將呼叫而不是`Configure()`當環境設定為開發。 同樣地，`ConfigureDevelopmentServices()`而不是呼叫`ConfigureServices()`相同環境中。

## <a name="summary"></a>總結

ASP.NET Core 提供數種功能可讓開發人員輕鬆地控制其應用程式在不同環境中的行為方式的慣例。 當發行應用程式從開發到預備環境到實際執行環境，環境變數集適當地環境允許偵錯、 測試或生產環境使用，視需要的應用程式的最佳化。

## <a name="additional-resources"></a>其他資源

* [組態](configuration.md)

* [標記協助程式簡介](../mvc/views/tag-helpers/intro.md)
