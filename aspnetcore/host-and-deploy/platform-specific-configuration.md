---
title: "新增應用程式中 ASP.NET Core 使用平台專屬組態的功能"
author: guardrex
description: "了解如何將功能加入至 ASP.NET Core 應用程式中，使用 IHostingStartup 實作的外部組件。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 2663cd1e05be9e8695966df959082e6e574d0b4a
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/15/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a>新增應用程式中 ASP.NET Core 使用平台專屬組態的功能

作者：[Luke Latham](https://github.com/guardrex)

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)實作可允許從外部應用程式的外部組件，將功能加入至應用程式在啟動`Startup`類別。 例如，可以使用外部工具程式庫`IHostingStartup`提供額外的組態提供者或服務應用程式的實作。 `IHostingStartup` *是可在 ASP.NET Core 2.0 及更新版本。*

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>探索載入裝載的啟動組件

若要探索裝載啟動的組件載入應用程式或程式庫，請啟用記錄，並檢查應用程式記錄檔。 載入組件時，會發生的錯誤會記錄。 在偵錯層級，記錄載入裝載的啟動組件，並會記錄所有錯誤。

範例應用程式讀取[HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey)到`string`陣列，並在應用程式的索引頁面中顯示結果：

[!code-csharp[Main](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>停用自動載入裝載啟動的組件

有兩種方式可以停用自動載入裝載啟動的組件：

* 設定[防止裝載啟動](xref:fundamentals/hosting#prevent-hosting-startup)裝載組態設定。
* 設定`ASPNETCORE_preventHostingStartup`環境變數。

當主控件設定或環境變數設為`true`或`1`、 裝載啟動的組件不會自動載入。 如果同時設定，主機設定控制的行為。

停用裝載使用主機的設定或環境變數的啟動組件全域停用它們，並可能會停用應用程式的數個功能。 它不是目前可以選擇性地停用裝載的啟動組件，加入程式庫，除非程式庫提供它自己的組態選項。 未來的版本會提供選擇性地停用裝載的啟動組件的能力 (請參閱[GitHub 發出 aspnet/主控 # 1243年](https://github.com/aspnet/Hosting/pull/1243))。

## <a name="implement-ihostingstartup-features"></a>實作 IHostingStartup 功能

### <a name="create-the-assembly"></a>建立組件

`IHostingStartup`做為主控台應用程式沒有進入點為基礎的組件部署功能。 組件參考[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/)封裝：

[!code-xml[Main](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)屬性會識別類別的實作為`IHostingStartup`載入和執行時建置[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost)。 在下列範例中，命名空間是`StartupFeature`，而類別是`StartupFeatureHostingStartup`:

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

類別會實作`IHostingStartup`。 類別的[設定](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)方法會使用[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)將功能加入至應用程式：

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

建置時`IHostingStartup`專案、 相依性檔案 (*\*。 deps.json*) 設定`runtime`之組件的位置*bin*資料夾：

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

部分檔案會顯示。 在範例中的組件名稱是`StartupFeature`。

### <a name="update-the-dependencies-file"></a>更新相依檔案

已指定的執行階段位置 *\*。 deps.json*檔案。 為 作用中的功能，`runtime`元素必須指定功能的執行階段組件的位置。 前置詞`runtime`位置`lib/netcoreapp2.0/`:

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

範例應用程式修改 *\*。 deps.json*檔案由執行[PowerShell](/powershell/scripting/powershell-scripting)指令碼。 PowerShell 指令碼會自動觸發建置 」 目標之專案檔中。

### <a name="feature-activation"></a>功能啟用

**將組件檔放在**

`IHostingStartup`實作的組件檔案必須是*bin*-部署的應用程式中，或置於[階段存放區](/dotnet/core/deploying/runtime-store):

對於每個使用者使用，請在使用者設定檔的執行階段存放區中放置組件：

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

供全域使用，將.NET Core 安裝的執行階段存放區中的組件：

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

時將組件部署至執行階段存放區中，符號檔可能也會部署，但不是必要的功能運作。

**相依性檔案的位置**

這個實作的 *\*。 deps.json*檔案必須是可存取的位置。

對於每個使用者使用，將檔案置於`additonalDeps`使用者設定檔的資料夾`.dotnet`設定： 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

供全域使用，將檔案置於`additonalDeps`.NET Core 安裝的資料夾：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

請注意該版本， `2.0.0`，也會反映目標應用程式所使用的共用執行階段版本。 共用的執行階段如下所示 *\*。 runtimeconfig.json*檔案。 範例應用程式中指定共用的執行階段*HostingStartupSample.runtimeconfig.json*檔案。

**設定環境變數**

使用此功能的應用程式的內容中設定下列環境變數。

ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES

只有裝載的啟動組件會受到掃描`HostingStartupAttribute`。 在這個環境變數中提供實作的組件名稱。 範例應用程式會將此值設`StartupDiagnostics`。

值也可以設定使用[裝載啟動組件](xref:fundamentals/hosting#hosting-startup-assemblies)裝載組態設定。

DOTNET\_其他\_DEPS

實作的位置 *\*。 deps.json*檔案。

如果要將檔案放置在使用者設定檔的*.dotnet*每位使用者使用的資料夾：

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

如果檔案放在全域使用的.NET Core 安裝，請提供檔案的完整路徑：

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

範例應用程式會將此值設定為：

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

如需如何設定各種作業系統的環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。

## <a name="sample-app"></a>範例應用程式

[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 會使用`IHostingStartup`建立的診斷工具。 此工具會加入兩個 middlewares 應用程式啟動時提供診斷資訊：

* 已註冊的服務
* 位址： 配置、 主機、 基底路徑、 路徑、 查詢字串
* 連接： 遠端 IP、 遠端連接埠、 本機 IP，本機連接埠，用戶端憑證
* 要求標頭
* 環境變數

若要執行範例：

1. 診斷啟動專案使用[PowerShell](/powershell/scripting/powershell-scripting)修改其*StartupDiagnostics.deps.json*檔案。 從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 的 Windows 作業系統上預設會安裝 PowerShell。 若要取得其他平台上的 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell)。
2. 建置診斷啟動專案。 在專案檔建置 」 目標：
   * 將組件和符號存放區-執行階段的使用者設定檔的檔案。
   * PowerShell 指令碼來修改觸發程序*StartupDiagnostics.deps.json*檔案。
   * 移動*StartupDiagnostics.deps.json*使用者設定檔的檔案`additionalDeps`資料夾。
3. 設定環境變數：
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. 執行範例應用程式。
5. 要求`/services`端點，請參閱應用程式的註冊服務。 要求`/diag`端點，請參閱 < 診斷的資訊。
