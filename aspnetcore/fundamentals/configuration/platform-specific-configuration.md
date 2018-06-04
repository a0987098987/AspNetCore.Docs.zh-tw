---
title: 在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式
author: guardrex
description: 了解如何使用 IHostingStartup 實作，從外部組件增強 ASP.NET Core 應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 618cb4349dcff696db37012af3aee844b82974f2
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729047"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式

作者：[Luke Latham](https://github.com/guardrex)

實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 即可在啟動時，從應用程式非 `Startup` 類別的外部組件，對應用程式新增強功能。 例如，外部工具程式庫可以使用 `IHostingStartup` 實作，提供額外的組態提供者或服務給應用程式。 ASP.NET Core 2.0 和更新版本提供了 `IHostingStartup`。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>探索載入的裝載啟動組件

若要探索應用程式或程式庫所載入的裝載啟動組件，請啟用記錄並檢查應用程式記錄檔。 系統會記錄載入組件時發生的錯誤。 將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。

範例應用程式會將 [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 讀取到 `string` 陣列，並在應用程式的 [索引] 頁面中顯示結果：

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>停用自動載入裝載啟動組件

有兩種方式可以停用自動載入裝載啟動組件：

* 設定[防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。
* 設定 `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。

當主機設定或環境變數設定為 `true` 或 `1` 時，不會自動載入裝載啟動組件。 如果這兩者皆已設定，則主機設定會控制該行為。

使用主機設定或環境變數停用裝載啟動組件會在全域停用它們，並且可能會停用數項應用程式特性。 除非程式庫提供自己的組態選項，否則目前不能選擇性地停用程式庫所新增的裝載啟動組件。 未來版本會提供選擇性地停用裝載啟動組件的功能 (請參閱 [GitHub 問題 aspnet/裝載 # 1243](https://github.com/aspnet/Hosting/pull/1243) (英文))。

## <a name="implement-ihostingstartup"></a>實作 IHostingStartup

### <a name="create-the-assembly"></a>建立組件

`IHostingStartup` 增強功能會根據主控台應用程式部署為組件，而無需進入點。 此組件會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

建置 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 時，[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性會將類別識別為用於載入和執行的 `IHostingStartup` 實作。 在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

類別會實作 `IHostingStartup`。 此類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 將增強功能新增至應用程式中：

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

建置 `IHostingStartup` 專案時，相依性檔案 (*\*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

僅顯示部分檔案。 範例中的組件名稱是 `StartupEnhancement`。

### <a name="update-the-dependencies-file"></a>更新相依性檔案

執行階段位置是在 *\*.deps.json* 檔案中指定。 若要啟用增強功能，`runtime` 元素必須指定增強功能之執行階段組件的位置。 請在 `runtime` 位置前面加上 `lib/<TARGET_FRAMEWORK_MONIKER>/`：

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

在範例應用程式中，*\*.deps.json* 檔案的修改是由 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼所執行。 PowerShell 指令碼則是由專案檔中的建置目標自動觸發。

### <a name="enhancement-activation"></a>啟動增強功能

**放置組件檔**

`IHostingStartup` 實作的組件檔案必須部署在應用程式的 *bin* 中，或置於[執行階段存放區](/dotnet/core/deploying/runtime-store)：

如果是個別使用者使用，請將組件置於使用者設定檔的執行階段存放區中：

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

如果是全域使用，請將組件置於 .NET Core 安裝的執行階段存放區中：

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

將組件部署至執行階段存放區時，可能也會部署符號檔，但運作增強功能並不需要該檔案。

**放置相依性檔案**

此實作的 *\*.deps.json* 檔案必須位於可存取的位置。

如果是個別使用者使用，請將檔案置於使用者設定檔之 `.dotnet` 設定的 `additonalDeps` 資料夾中： 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

如果是全域使用，請將檔案置於 .NET Core 安裝的 `additonalDeps` 資料夾中：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\
```

請注意，版本 `2.1.0` 反映了目標應用程式所使用的共用執行階段版本。 *\*.runtimeconfig.json* 檔案會顯示共用的執行階段。 在範例應用程式中，共用的執行階段則是在 *HostingStartupSample.runtimeconfig.json* 檔案中指定。

**設定環境變數**

在使用增強功能之應用程式的內容中設定下列環境變數。

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

只會掃描裝載啟動組件是否有 `HostingStartupAttribute`。 這個環境變數中提供了實作的組件名稱。 範例應用程式會將此值設定為 `StartupDiagnostics`。

您也可以使用[裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)的主機組態設定來設定此值。

DOTNET\_ADDITIONAL\_DEPS

實作的 *\*.deps.json* 檔案的位置。

如果針對個別使用者使用，將檔案置於使用者設定檔的 *.dotnet* 資料夾：

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

如果針對全域使用將檔案置於 .NET Core 安裝，請提供檔案的完整路徑：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.1.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

範例應用程式會將此值設定為：

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。

## <a name="sample-app"></a>範例應用程式

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([如何下載](xref:tutorials/index#how-to-download-a-sample)) 會使用 `IHostingStartup` 來建立診斷工具。 此工具會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：

* 已註冊服務
* 位址：配置、主機、基底路徑、路徑、查詢字串
* 連線：遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證
* 要求標頭
* 環境變數

若要執行範例：

1. 啟動診斷專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 來修改其 *StartupDiagnostics.deps.json* 檔案。 從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，Windows 作業系統上預設會安裝 PowerShell。 若要在其他平台上取得 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell)。
2. 建置啟動診斷專案。 專案檔中的建置目標會：
   * 將組件檔和符號檔移至使用者設定檔的執行階段存放區。
   * 觸發 PowerShell 指令碼來修改 *StartupDiagnostics.deps.json* 檔案。
   * 將 *StartupDiagnostics.deps.json* 檔案移至使用者設定檔的 `additionalDeps` 資料夾。
3. 設定環境變數：
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. 執行範例應用程式。
5. 要求 `/services` 端點來查看應用程式的註冊服務。 要求 `/diag` 端點來查看診斷資訊。
