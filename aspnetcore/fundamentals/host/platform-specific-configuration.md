---
title: 在 ASP.NET Core 中使用裝載啟動組件
author: guardrex
description: 了解如何使用 IHostingStartup 實作，從外部組件增強 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/06/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: df078a2a2a50538a070bb0b49ff3853682cb17df
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64889053"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a>在 ASP.NET Core 中使用裝載啟動組件

作者：[Luke Latham](https://github.com/guardrex) 與 [Pavel Krymets](https://github.com/pakrym)

實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (裝載啟動) 會在啟動時，從外部組件將增強功能新增至應用程式。 例如，外部程式庫可以使用裝載啟動實作，向應用程式提供額外的組態提供者或服務。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>HostingStartup 屬性

[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性指出裝載啟動組件的出現，於執行階段啟動。

系統會自動掃描包含 `Startup` 類別的進入組件或組件是否有 `HostingStartup` 屬性。 搜尋 `HostingStartup` 屬性的組件清單會在執行階段從 [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 中的組態載入。 要從探索中排除的組件清單會從 [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) 載入。 如需詳細資訊，請參閱 [Web 主機：裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)和 [Web 主機：裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)。

在下列範例中，裝載啟動組件的命名空間為 `StartupEnhancement`。 包含裝載啟動程式碼的類別為 `StartupEnhancementHostingStartup`：

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

`HostingStartup` 屬性通常位於裝載啟動組件的 `IHostingStartup` 實作類別檔案。

## <a name="discover-loaded-hosting-startup-assemblies"></a>探索載入的裝載啟動組件

若要探索載入的裝載啟動組件，請啟用記錄並檢查應用程式的記錄檔。 系統會記錄載入組件時發生的錯誤。 將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>停用自動載入裝載啟動組件

若要停用自動載入裝載啟動組件，請使用下列任一方法：

* 若要防止載入所有裝載啟動組件，請將下列任一項目設為 `true` 或 `1`：
  * [防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。
  * `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。
* 若要防止載入特定的裝載啟動組件，請將下列任一項目設為以分號分隔的裝載啟動組件字串，藉此於啟動時排除：
  * [裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)主機組態設定。
  * `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境變數。

如已設定主機組態設定和環境變數，主機設定即會控制行為。

使用主機設定或環境變數停用裝載啟動組件會停用全域的組件，並且可能會停用數項應用程式特性。

## <a name="project"></a>專案

以下列兩種專案類型的任一類型建立裝載啟動：

* [類別庫](#class-library)
* [沒有進入點的主控台應用程式](#console-app-without-an-entry-point)

### <a name="class-library"></a>類別庫

類別庫可以提供裝載啟動的增強功能。 此程式庫包含 `HostingStartup` 屬性。

[程式碼範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)包含 Razor Pages 應用程式 *HostingStartupApp* 和類別庫 *HostingStartupLibrary*。 類別庫：

* 包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。 `ServiceKeyInjection` 使用記憶體內部組態提供者 ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection))，將成對的服務字串新增至應用程式的組態。
* 包含 `HostingStartup` 屬性，可識別裝載啟動的命名空間和類別。

`ServiceKeyInjection` 類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)，將增強功能新增至應用程式中。

*HostingStartupLibrary/ServiceKeyInjection.cs*：

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

應用程式的 [索引] 頁面會讀取並呈現類別庫裝載啟動組件所設定之兩個索引鍵的組態值：

*HostingStartupApp/Pages/Index.cshtml.cs*：

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

[程式碼範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)也包含提供另一個主機啟動 (*HostingStartupPackage*) 的 NuGet 套件專案。 此套件特性與前文所述之類別庫相同。 套件：

* 包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。 `ServiceKeyInjection` 將成對的服務字串新增到應用程式組態。
* 包含 `HostingStartup` 屬性。

*HostingStartupPackage/ServiceKeyInjection.cs*：

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

應用程式的 [索引] 頁面會讀取並呈現套件裝載啟動組件所設定之兩個索引鍵的組態值：

*HostingStartupApp/Pages/Index.cshtml.cs*：

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>沒有進入點的主控台應用程式

這個方法僅適用於 .NET Core 應用程式，不適用於 .NET Framework。

在沒有包含 `HostingStartup` 屬性之進入點的主控台應用程式中，可以提供不需要啟用編譯時間參考的動態裝載啟動增強功能。 發佈主控台應用程式會產生裝載啟動組件，這些組件可從執行階段存放區取用。

沒有進入點的主控台應用程式會在此程序中使用，因為：

* 需要相依性檔案才能取用裝載啟動組件中的裝載啟動。 相依性檔案是可執行的應用程式資產，這是透過發佈應用程式 (而非程式庫) 所產生。
* 程式庫無法直接新增至[執行階段套件存放區](/dotnet/core/deploying/runtime-store)，這需要以共用執行階段為目標的可執行專案。

在建立動態裝載啟動階段中：

* 會從沒有進入點的主控台應用程式建立裝載啟動組件：
  * 包括包含 `IHostingStartup` 實作的類別。
  * 包括 [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性以識別 `IHostingStartup` 實作類別。
* 發佈主控台應用程式以取得裝載啟動的相依性。 發佈主控台應用程式的結果是從相依性檔案中刪減未使用的相依性。
* 相依性檔案已修改以設定裝載啟動組件的執行階段位置。
* 裝載啟動組件與其相依性檔案會放入執行階段套件存放區。 若要探索裝載啟動組件與其相依性檔案，兩者會在成對的環境變數中列出。

主控台應用程式會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

建置 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 時，[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性會將類別識別為用於載入和執行的 `IHostingStartup` 實作。 在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

類別會實作 `IHostingStartup`。 此類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 將增強功能新增至應用程式中。 執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

建置 `IHostingStartup` 專案時，相依性檔案 (*\*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

僅顯示部分檔案。 範例中的組件名稱是 `StartupEnhancement`。

## <a name="configuration-provided-by-the-hosting-startup"></a>由裝載啟動所提供的設定

視您是否要讓裝載啟動設定的優先順序高於應用程式的設定，有兩種方式可用來處理設定：

1. 在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之後使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 載入設定，以提供設定給應用程式。 使用此方法時，裝載啟動設定的優先順序高於應用程式的設定。
1. 在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之前使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 載入設定，以提供設定給應用程式。 使用此方法時，應用程式設定值的優先順序高於裝載啟動程式所提供的設定值。

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a>指定裝載啟動組件

為類別庫 (或主控台應用程式) 提供的裝載啟動，在 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數中指定裝載啟動組件的名稱。 環境變數是以分號分隔的組件清單。

只掃描裝載啟動組件是否有 `HostingStartup` 屬性。 範例應用程式 *HostingStartupApp* 若要探索前文所述的裝載啟動，環境變數要設定為下列值：

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

您也可以使用[裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)的主機組態設定來設定裝載啟動組件。

當有多個裝載啟動組合存在時，其 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法會依組件的列示順序執行。

## <a name="activation"></a>啟用

裝載啟動啟用的選項如下：

* [執行階段存放區](#runtime-store) &ndash; 啟用不需要啟用的編譯時間參考。 範例應用程式會將裝載啟動組件和相依性檔案放入 *deployment* 資料夾，以利在多機器環境中部署裝載啟動。 *deployment* 資料夾也包含 PowerShell 指令碼，可在部署系統上建立或修改環境變數，以啟用裝載啟動。
* 啟用所需之編譯時間參考
  * [NuGet 套件](#nuget-package)
  * [專案 bin 資料夾](#project-bin-folder)

### <a name="runtime-store"></a>執行階段存放區

裝載啟動實作置於[執行階段存放區](/dotnet/core/deploying/runtime-store)。 增強的應用程式不需要組件的編譯時間參考。

建置裝載啟動之後，會使用資訊清單專案檔與 [dotnet store](/dotnet/core/tools/dotnet-store) 命令來產生執行階段存放區。

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

在範例應用程式 (*RuntimeStore* 專案) 中，我們使用下列命令：

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

為讓執行階段探索執行階段存放區，執行階段存放區的位置會新增到 `DOTNET_SHARED_STORE` 環境變數。

**修改並放置裝載啟動的相依性檔案**

若要在沒有對加強功能之套件參考的情況下啟用加強功能，請使用 `additionalDeps` 指定對執行階段的額外相依性。 `additionalDeps` 可讓您：

* 透過提供一組額外的 *\*.deps.json* 檔案在啟動時與應用程式的自有 *\*.deps.json* 檔案合併，以延伸應用程式的程式庫圖表。
* 讓裝載啟動組件可供探索且可載入。

用於產生額外相依性的建議方法是：

 1. 在上一節參考的執行階段存放區資訊清單檔上執行 `dotnet publish`。
 1. 從程式庫與產生之 *\*deps.json* 檔案的 `runtime` 區段移除資訊清單參考。

在範例專案中，`store.manifest/1.0.0` 屬已從 `targets` 與 `libraries` 區段移除：

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

將 *\*.deps.json* 檔案放到下列位置：

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* `{ADDITIONAL DEPENDENCIES PATH}` &ndash; 位置已新增到 `DOTNET_ADDITIONAL_DEPS` 環境變數。
* `{SHARED FRAMEWORK NAME}` &ndash; 這個額外相依性檔案需要共用架構。
* `{SHARED FRAMEWORK VERSION}` &ndash; 最小共用架構版本。
* `{ENHANCEMENT ASSEMBLY NAME}` &ndash; 加強功能的組件名稱。

在範例應用程式 (*RuntimeStore* 專案) 中，額外的相依性檔案是放到下列位置：

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

為讓執行階段探索執行階段存放區，額外的相依性檔案位置會新增到 `DOTNET_ADDITIONAL_DEPS` 環境變數。

在範例應用程式 (*RuntimeStore* 專案) 中，建置執行階段存放區並產生額外的相依性檔案是透過使用 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼來完成的。

如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。

**部署**

為利於在多機器環境中部署裝載啟動，範例應用程式會在包含下列項目的發佈輸出中建立 *deployment* 資料夾：

* 裝載啟動執行階段存放區。
* 裝載啟動相依性檔案。
* 建立或修改 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`、`DOTNET_SHARED_STORE` 與 `DOTNET_ADDITIONAL_DEPS` 以支援啟用裝載啟動的 PowerShell 指令碼。 在部署系統上，從系統管理 PowerShell 命令提示字元執行指令碼。

### <a name="nuget-package"></a>NuGet 套件

NuGet 套件可以提供裝載啟動的增強功能。 此套件具有 `HostingStartup` 屬性。 使用下列兩種方法的任一方法，套件提供的裝載啟動類型即可提供應用程式使用：

* 增強應用程式的專案檔會在應用程式專案檔 (編譯時間參考) 中建立裝載啟動的套件參考。 當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*\*.deps.json*)。 這種方法適用於發佈至 [nuget.org](https://www.nuget.org/) 的裝載啟動組件套件。
* 裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。

如需有關 NuGet 套件和執行階段存放區的詳細資訊，請參閱下列主題：

* [如何使用跨平台工具建立 NuGet 套件](/dotnet/core/deploying/creating-nuget-packages)
* [發佈套件](/nuget/create-packages/publish-a-package)
* [執行階段套件存放區](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>專案 bin 資料夾

部署在增強應用程式 *bin* 下的組件可提供裝載啟動增強功能。 使用下列兩種方法的任一方法，組件提供的裝載啟動類型即可提供應用程式使用：

* 增強應用程式的專案檔會建立裝載啟動的組件參考 (編譯時間參考)。 當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*\*.deps.json*)。 當部署案例要求將已編譯裝載啟動程式庫的組件 (DLL 檔案) 移至取用專案或取用專案可存取的位置，且已為裝載啟動組件建立編譯時間參考時，即適用這種方法。
* 裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。

## <a name="sample-code"></a>範例程式碼

[程式碼範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) 示範裝載啟動實作案例：

* 兩個裝載啟動組件 (類別程式庫) 各設定一對記憶體內部組態索引鍵/值組：
  * NuGet 套件 (*HostingStartupPackage*)
  * 類別庫 (*HostingStartupLibrary*)
* 從部署在執行階段存放區的組件啟動裝載啟動 (*StartupDiagnostics*)。 此組件會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：
  * 已註冊服務
  * 位址 (配置、主機、基底路徑、路徑、查詢字串)
  * 連線 (遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證)
  * 要求標頭
  * 環境變數

若要執行範例：

**從 NuGet 套件啟用**

1. 使用 [dotnet pack](/dotnet/core/tools/dotnet-pack) 命令編譯 *HostingStartupPackage* 套件。
1. 將 *HostingStartupPackage* 的套件組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。
1. 編譯並執行應用程式。 增強的應用程式中有套件參考 (編譯時間參考)。 應用程式專案檔中的 `<PropertyGroup>` 會指定套件專案的輸出 (*../HostingStartupPackage/bin/Debug*) 為套件來源。 這可讓應用程式使用套件，卻不用將套件上傳至 [nuget.org](https://www.nuget.org/)。如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. 觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合套件之 `ServiceKeyInjection.Configure` 方法設定的值。

如果您變更並重新編譯 *HostingStartupPackage* 專案，請清除本機的 NuGet 套件快取，以確保 *HostingStartupApp* 從本機快取接收更新的套件，而非過時的套件。 若要清除本機 NuGet 快取，請執行下列 [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) 命令：

```console
dotnet nuget locals all --clear
```

**從類別庫啟用**

1. 使用 [dotnet build](/dotnet/core/tools/dotnet-build) 命令編譯 *HostingStartupLibrary* 類別庫。
1. 將 *HostingStartupLibrary* 的類別庫組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。
1. 將 *HostingStartupLibrary.dll* 檔案從類別庫的編譯輸出複製到應用程式的 *bin/Debug* 資料夾，在應用程式的 *bin* 下部署類別庫組件。
1. 編譯並執行應用程式。 應用程式專案檔中的 `<ItemGroup>` 參考類別庫的組件 (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (編譯時間參考)。 如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. 觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合類別庫之 `ServiceKeyInjection.Configure` 方法所設定的值。

**從部署在執行階段存放區的組件啟用**

1. *StartupDiagnostics* 專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 修改其 *StartupDiagnostics.deps.json* 檔案。 從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，會在 Windows 上預設安裝 PowerShell。 若要在其他平台上取得 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core)。
1. 執行 *RuntimeStore* 資料夾中的 *build.ps1* 指令碼。 指令碼會：
   * 產生 `StartupDiagnostics` 套件。
   * 在 *store* 資料夾中產生 `StartupDiagnostics` 的執行階段存放區。 指令碼中的 `dotnet store` 命令會使用 `win7-x64` 的 [runtime identifier (RID) (執行階段識別碼 (RID))](/dotnet/core/rid-catalog) 作為部署至 Windows 的裝載啟動。 為不同的執行階段提供裝載啟動時，請在指令碼的行 37 上替換成正確的 RID。
   * 在 *additionalDeps/shared/Microsoft.AspNetCore.App/{共用架構版本}/* 資料夾中產生 `StartupDiagnostics` 的 `additionalDeps`。
   * 將 *deploy.ps1* 檔案置於 *deployment* 資料夾中。
1. 執行 *deployment* 資料夾中的 *deploy.ps1* 指令碼。 該指令碼會附加至：
   * `StartupDiagnostics` 至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。
   * 針對 `DOTNET_ADDITIONAL_DEPS` 環境變數的裝載啟動相依性。
   * 針對 `DOTNET_SHARED_STORE` 環境變數的執行階段存放區路徑。
1. 執行範例應用程式。
1. 要求 `/services` 端點來查看應用程式的註冊服務。 要求 `/diag` 端點來查看診斷資訊。
