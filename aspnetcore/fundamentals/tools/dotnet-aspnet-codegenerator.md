---
title: dotnet aspnet-codegenerator 命令
author: rick-anderson
description: dotnet aspnet-codegenerator 命令會架起 ASP.NET Core 專案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: a106654c8a37e84e9186a2f06d90605df753e8a7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405597"
---
# <a name="dotnet-aspnet-codegenerator"></a>dotnet aspnet-codegenerator

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

`dotnet aspnet-codegenerator` - 執行 ASP.NET Core Scaffolding 引擎。 從命令列進行 Scaffolding 時才需要 `dotnet aspnet-codegenerator`，在 Visual Studio 中不需要進行 Scaffolding。

本文適用於 [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) 與更新版本。

## <a name="installing-aspnet-codegenerator"></a>Installing aspnet-codegenerator

`dotnet-aspnet-codegenerator` 是必須安裝的[全域工具](/dotnet/core/tools/global-tools)。 下列命令會安裝 `dotnet-aspnet-codegenerator` 工具的最新穩定版本：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

下列命令會將 `dotnet-aspnet-codegenerator` 更新到可從已安裝之 .NET Core SDK 中取得的最新穩定版本：

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a>概要

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a>說明

`dotnet aspnet-codegenerator` 全域命令會執行 ASP.NET Core 程式碼產生器與 Scaffolding 引擎。

## <a name="arguments"></a>引數

`generator`

要執行的程式碼產生器。 可用產生器如下︰

| 產生器 | 作業 |
| ----------------- | ------------ | 
| 區域      | [架起區域](/aspnet/core/mvc/controllers/areas) |
  controller| [架起控制器](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  身分識別  | [ScaffoldIdentity](/aspnet/core/security/authentication/scaffold-identity) |
  razorpage | [Scaffold Razor 頁面](/aspnet/core/tutorials/razor-pages/model) |
  檢視      | [架起檢視](/aspnet/core/mvc/views/overview) |

## <a name="options"></a>選項

`-n|--nuget-package-dir`

指定 NuGet 套件目錄。

`-c|--configuration {Debug|Release}`

定義組建組態。 預設值是 `Debug`。

`-tfm|--target-framework`

要使用的目標 [Framework](/dotnet/standard/frameworks)。 例如： `net46` 。

`-b|--build-base-path`

建置基底路徑。

`-h|--help`

印出命令的簡短說明。

`--no-build`

不會在執行前建置專案。 選項也會隱含設定 `--no-restore` 旗標。

`-p|--project <PATH>`

指定要執行的專案檔路徑 (資料夾名稱或完整路徑)。 如果未指定，則會預設為目前目錄。

## <a name="generator-options"></a>產生器選項

以下各節詳細說明支援之產生器的可用選項：

* 區域
* 控制器
* Identity  
* Razorpage
* 檢視

<a name="area"></a>

### <a name="area-options"></a>區域選項

此工具旨在用於搭配控制器與檢視使用 ASP.NET Core Web 專案。 並非適用于 Razor 頁面應用程式。

使用方式：`dotnet aspnet-codegenerator area AreaNameToGenerate`

上述命令會產生下列資料夾：

* *區域*
  * *AreaNameToGenerate*
    * *控制器*
    * *Data*
    * *模型*
    * *檢視*

<a name="ctl"></a>

### <a name="controller-options"></a>控制器選項

下表列出和的選項 `aspnet-codegenerator` `controller` `razorpage` ：

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

下表列出 `aspnet-codegenerator controller` 的專用選項：

| 選項               | 說明|
| ----------------- | ------------ |
| --controllerName 或 -name | 控制器的名稱。 |
| --useAsyncActions 或 -async | 產生非同步控制器動作。 |
| --noViews 或 -nv | **不**產生任何檢視。 |
| --restWithNoViews 或 -api  | 使用 REST 樣式 API 產生控制器。 假設使用 `noViews` 且會忽略所有檢視相關選項。 |
| --readWriteActions 或 -actions | 在不使用模型的情況下使用讀取/寫入動作產生控制器。 |

使用 `-h` 參數取得 `aspnet-codegenerator controller` 命令的說明：

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator controller` 的範例。

### <a name="razorpage"></a>Razorpage

<a name="rp"></a>

Razor您可以藉由指定要使用的新頁面名稱和範本來個別 scaffold 頁面。 支援的範本為：

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

例如，下列命令會使用「編輯」範本來產生 *MyEdit.cshtml* 與 *MyEdit.cshtml.cs*：

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

一般而言，不會指定範本與產生的檔案名稱，而且會建立下列範本：

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

下表列出和的選項 `aspnet-codegenerator` `razorpage` `controller` ：

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

下表列出 `aspnet-codegenerator razorpage` 的專用選項：

| 選項               | 說明|
| ----------------- | ------------ |
|   --namespaceName 或 -namespace | 要用於產生之 PageModel 的命名空間名稱 |
| --partialView 或 -partial | 產生部分檢視。 若指定此選項，會忽略版面配置選項 -l 與 -udl。 |
| --noPageModel 或 -npm | 切換為不產生空白範本的 PageModel 類別 |

使用 `-h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator razorpage` 的範例。

### Identity

請[參閱 Identity Scaffold](/aspnet/core/security/authentication/scaffold-identity)
