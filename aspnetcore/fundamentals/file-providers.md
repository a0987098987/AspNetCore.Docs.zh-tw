---
title: ASP.NET Core 中的檔案提供者
author: ardalis
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core 中的檔案提供者

作者：[Steve Smith](https://ardalis.com/)

ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>檔案提供者抽象概念

檔案提供者是對檔案系統的抽象。 主要介面是 `IFileProvider`。 `IFileProvider` 公開了一些方法，用來取得檔案資訊 (`IFileInfo`)、目錄資訊 (`IDirectoryContents`)，以及設定變更通知 (使用 `IChangeToken`)。

`IFileInfo` 提供與個別檔案或目錄有關的方法和屬性。 它有兩個布林值屬性 (`Exists` 和 `IsDirectory`)，以及描述檔案的`Name`、`Length` (以位元組為單位) 和 `LastModified` 日期的屬性。 您可以使用其 `CreateReadStream` 方法從檔案讀取。

## <a name="file-provider-implementations"></a>檔案提供者實作

提供三種 `IFileProvider` 實作：實體、內嵌和複合。 實體提供者用來存取實際系統的檔案。 內嵌提供者用來存取內嵌於組件的檔案。 複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` 提供對實體檔案系統的存取。 它會包裝 `System.IO.File` 類型 (用於實體提供者)，並將所有路徑的範圍設為某個目錄及其子系。 此範圍會限制存取特定目錄及其子系，以防止存取這個界限以外的檔案系統。 具現化此提供者時，您必須為其提供一個目錄路徑，該路徑會作為對此提供者發出之所有要求的基礎路徑 (並限制此路徑之外的存取)。 在 ASP.NET Core 應用程式中，您可以直接具現化 `PhysicalFileProvider` 提供者，也可以透過[相依性插入](dependency-injection.md)在控制器或服務的建構函式中要求 `IFileProvider`。 第二種方法通常會產生更有彈性且可測試的解決方案。

下列範例示範如何建立 `PhysicalFileProvider`。


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

您可以逐一查看其目錄內容，或提供子路徑來取得特定檔案的資訊。

若要從控制器要求提供者，請在控制器的建構函式中指定它，並將其指派給本機欄位。 請使用動作方法中的本機執行個體：

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

接著，在應用程式的 `Startup` 類別中建立提供者：

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

在 *Index.cshtml* 檢視中，逐一查看提供的 `IDirectoryContents`：

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

結果：

![列出實體檔案和資料夾的檔案提供者範例應用程式](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` 用來存取內嵌於組件的檔案。 在 .NET Core 中，您已在 *.csproj* 檔案內使用 `<EmbeddedResource>` 項目將檔案內嵌於組件：

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

指定要內嵌於組件的檔案時，您可以使用[萬用字元模式](#globbing-patterns)。 這些模式可用來比對一或多個檔案。

> [!NOTE]
> 您不太可能想要將專案中的每個 .js 檔案實際內嵌在其組件中；上述範例僅供示範之用。

建立 `EmbeddedFileProvider` 時，請將它會讀取的組件傳遞至其建構函式。

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

上述程式碼片段示範如何建立可存取目前執行組件的 `EmbeddedFileProvider`。

更新範例應用程式以使用 `EmbeddedFileProvider` 時，會產生下列輸出：

![列出內嵌檔案的檔案提供者範例應用程式](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> 內嵌的資源不會公開目錄。 而是使用 `.` 分隔符號，將資源的路徑 (透過其命名空間) 內嵌在其檔案名稱中。

> [!TIP]
> `EmbeddedFileProvider` 建構函式可接受選擇性的 `baseNamespace` 參數。 指定此參數會將 `GetDirectoryContents` 的範圍設為所提供命名空間底下的那些資源。

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。 建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式：

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

更新範例應用程式使用 `CompositeFileProvider` (其同時包含先前設定的實體提供者和內嵌提供者) 時，會產生下列輸出：

![同時列出實體檔案和資料夾與內嵌檔案的檔案提供者範例應用程式](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>監看變更

`IFileProvider` `Watch` 方法提供一種方式，用來監看一或多個檔案或目錄是否有變更。 這個方法接受路徑字串 (它可以使用[萬用字元模式](#globbing-patterns)來指定多個檔案)，並傳回 `IChangeToken`。 這個權杖會將可以檢查的 `HasChanged` 屬性，以及偵測到變更時所呼叫的 `RegisterChangeCallback` 方法公開至指定的路徑字串。 請注意，每個變更權杖只會呼叫其相關聯的回呼，以回應單一變更。 若要啟用持續監視，您可以如下所示使用 `TaskCompletionSource`，或重新建立 `IChangeToken` 執行個體以回應變更。

在本文的範例中，主控台應用程式會設定為每次修改文字檔時，顯示一則訊息：

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

儲存檔案數次之後的結果：

![執行 dotnet run 之後的命令視窗顯示應用程式監視 quotes.txt 檔案是否有變更，並顯示檔案已變更五次。](file-providers/_static/watch-console.png)

> [!NOTE]
> 某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。 請將 `DOTNET_USE_POLLINGFILEWATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒輪詢檔案系統是否有變更。

## <a name="globbing-patterns"></a>萬用字元模式

檔案系統路徑中使用稱為「萬用字元模式」(*globbing patterns*) 的模式。 這些簡單的模式可以用來指定檔案群組。 兩種萬用字元為 `*` 和 `**`。

**`*`**

   符合目前資料夾層級的任何項目，或者任何檔案名稱或任何副檔名。 相符項是以檔案路徑中的 `/` 和 `.` 字元終止。

<strong><code>**</code></strong>

   符合多個目錄層級之間的任何項目。 可用來以遞迴方式符合目錄階層內的許多檔案。

### <a name="globbing-pattern-examples"></a>萬用字元模式範例

**`directory/file.txt`**

   符合特定目錄中的特定檔案。

**<code>directory/*.txt</code>**

   符合特定目錄中具有 `.txt` 副檔名的所有檔案。

**`directory/*/bower.json`**

   符合 `directory` 目錄下一層級的目錄中的所有 `bower.json` 檔案。

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   符合在 `directory` 目錄下的任何地方所找到之具有 `.txt` 副檔名的所有檔案。

## <a name="file-provider-usage-in-aspnet-core"></a>ASP.NET Core 中的檔案提供者使用方式

ASP.NET Core 的數個部分會利用檔案提供者。 `IHostingEnvironment` 將應用程式的內容根目錄與 Web 根目錄公開為 `IFileProvider` 類型。 靜態檔案中介軟體使用檔案提供者來尋找靜態檔案。 Razor 大量使用 `IFileProvider` 來尋找檢視。 Dotnet 的發行功能使用檔案提供者和萬用字元模式來指定應發行哪些檔案。

## <a name="recommendations-for-use-in-apps"></a>在應用程式中使用的建議

如果 ASP.NET Core 應用程式需要檔案系統存取，您可以透過相依性插入要求 `IFileProvider` 的執行個體，然後使用其方法來執行存取，如此範例所示。 這可讓您在應用程式啟動時設定提供者一次，並減少應用程式具現化的實作類型的數目。
