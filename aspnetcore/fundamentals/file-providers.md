---
title: ASP.NET Core 中的檔案提供者
author: guardrex
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: a454ca394546184968222ca2ca44d7159b19a12a
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944304"
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core 中的檔案提供者

作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。 「檔案提供者」在整個 ASP.NET Core 架構中使用：

* `IWebHostEnvironment` 會將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。
* [靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。
* [Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。
* .NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>檔案提供者介面

主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。 `IFileProvider` 公開方法以：

* 取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。
* 取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。
* 設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。

`IFileInfo` 提供可用來使用檔案的方法與屬性：

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期

您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。

範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。

## <a name="file-provider-implementations"></a>檔案提供者實作

我們提供三個 `IFileProvider` 的實作。

| 實作 | 描述 |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | 實體提供者用來存取系統的實體檔案。 |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | 資訊清單內嵌提供者用來存取內嵌於組件的檔案。 |
| [CompositeFileProvider](#compositefileprovider) | 複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。 |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。 `PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。 此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。 建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。

當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。

下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

上述範例中的型別：

* `provider` 是 `IFileProvider`。
* `contents` 是 `IDirectoryContents`。
* `fileInfo` 是 `IFileInfo`。

「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。 「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。

範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。 `ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。

將套件參考新增至 [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) 套件的專案中。

若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。 使用 [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。

範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。

*Startup.cs*：

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

額外的多載可讓您：

* 指定相對檔案路徑。
* 將檔案限定為上次修改日期。
* 為包內嵌檔案資訊清單的內嵌資源命名。

| 多載 | 描述 |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | 接受選擇性的 `root` 相對路徑參數。 指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。 |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | 接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。 `lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。 |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | 接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。 `manifestName` 代表包含資訊清單之內嵌資源的名稱。 |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。 建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。

在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>監視變更

[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。 `Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。 `Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。 變更權杖會公開：

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; 可檢查以判斷是否發生變更的屬性。
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; 當偵測到所指定路徑字串發生變更時要呼叫的項目。 每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。 若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。

在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。 將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。

## <a name="glob-patterns"></a>Glob 模式

檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。 使用這些模式指定檔案群組。 兩種萬用字元為 `*` 與 `**`：

**`*`**  
符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。 相符項是以檔案路徑中的 `/` 和 `.` 字元終止。

**`**`**  
符合多個目錄層級之間的任何項目。 可用來以遞迴方式符合目錄階層內的許多檔案。

**Glob 模式範例**

**`directory/file.txt`**  
符合特定目錄中的特定檔案。

**`directory/*.txt`**  
符合特定目錄中具有 *.txt* 副檔名的所有檔案。

**`directory/*/appsettings.json`**  
符合「目錄」資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。

**`directory/**/*.txt`**  
符合在「目錄」資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。 「檔案提供者」在整個 ASP.NET Core 架構中使用：

* <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 會將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。
* [靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。
* [Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。
* .NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="file-provider-interfaces"></a>檔案提供者介面

主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。 `IFileProvider` 公開方法以：

* 取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。
* 取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。
* 設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。

`IFileInfo` 提供可用來使用檔案的方法與屬性：

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期

您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。

範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。

## <a name="file-provider-implementations"></a>檔案提供者實作

我們提供三個 `IFileProvider` 的實作。

| 實作 | 描述 |
| -------------- | ----------- |
| [PhysicalFileProvider](#physicalfileprovider) | 實體提供者用來存取系統的實體檔案。 |
| [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | 資訊清單內嵌提供者用來存取內嵌於組件的檔案。 |
| [CompositeFileProvider](#compositefileprovider) | 複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。 |

### <a name="physicalfileprovider"></a>PhysicalFileProvider

<xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。 `PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。 此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。 建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。

當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。

下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

上述範例中的型別：

* `provider` 是 `IFileProvider`。
* `contents` 是 `IDirectoryContents`。
* `fileInfo` 是 `IFileInfo`。

「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。 「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。

範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a>ManifestEmbeddedFileProvider

<xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。 `ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。

若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。 使用 [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。

範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。

*Startup.cs*：

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

額外的多載可讓您：

* 指定相對檔案路徑。
* 將檔案限定為上次修改日期。
* 為包內嵌檔案資訊清單的內嵌資源命名。

| 多載 | 描述 |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | 接受選擇性的 `root` 相對路徑參數。 指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。 |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | 接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。 `lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。 |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | 接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。 `manifestName` 代表包含資訊清單之內嵌資源的名稱。 |

### <a name="compositefileprovider"></a>CompositeFileProvider

<xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。 建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。

在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a>監視變更

[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。 `Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。 `Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。 變更權杖會公開：

* <xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; 可檢查以判斷是否發生變更的屬性。
* <xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; 當偵測到所指定路徑字串發生變更時要呼叫的項目。 每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。 若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。

在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。 將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。

## <a name="glob-patterns"></a>Glob 模式

檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。 使用這些模式指定檔案群組。 兩種萬用字元為 `*` 與 `**`：

**`*`**  
符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。 相符項是以檔案路徑中的 `/` 和 `.` 字元終止。

**`**`**  
符合多個目錄層級之間的任何項目。 可用來以遞迴方式符合目錄階層內的許多檔案。

**Glob 模式範例**

**`directory/file.txt`**  
符合特定目錄中的特定檔案。

**`directory/*.txt`**  
符合特定目錄中具有 *.txt* 副檔名的所有檔案。

**`directory/*/appsettings.json`**  
符合「目錄」資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。

**`directory/**/*.txt`**  
符合在「目錄」資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。

::: moniker-end
