---
title: "在 ASP.NET Core 檔案提供者"
author: ardalis
description: "了解 ASP.NET Core 抽象化透過檔案提供者使用的檔案系統存取的方式。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a>在 ASP.NET Core 檔案提供者

由[Steve Smith](https://ardalis.com/)

ASP.NET Core 抽象化透過檔案提供者使用的檔案系統存取權。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="file-provider-abstractions"></a>檔案提供者的抽象概念

檔案提供者是透過檔案系統的抽象概念。 主要介面為`IFileProvider`。 `IFileProvider`公開方法，以取得檔案資訊 (`IFileInfo`)，目錄資訊 (`IDirectoryContents`)，並設定變更通知 (使用`IChangeToken`)。

`IFileInfo`提供方法和相關的個別檔案或目錄的屬性。 它有兩個布林值屬性，`Exists`和`IsDirectory`，描述之檔案的內容以及`Name`， `Length` （以位元組為單位），和`LastModified`日期。 您可以從檔案使用讀取其`CreateReadStream`方法。

## <a name="file-provider-implementations"></a>檔案提供者實作

三種實作方式`IFileProvider`可用： 實體、 內嵌，以及複合。 實體的提供者用來存取實際系統的檔案。 內嵌的提供者用來存取內嵌於組件的檔案。 複合的提供者用來提供檔案和目錄從一或多個其他提供者的合併的存取。

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider`提供實體檔案系統的存取權。 它所包裝`System.IO.File`型別 （適用於實體提供者），範圍設定的目錄和其子系的所有路徑。 此範圍會限制存取特定目錄和子系，導致無法存取此界限以外的檔案系統。 當具現化此提供者，您必須將它提供一個目錄路徑，會作為此提供者進行的所有要求的基底路徑 （和以限制存取此路徑之外）。 在 ASP.NET Core 應用程式，您可以具現化`PhysicalFileProvider`直接管理，提供者，或者您可以要求`IFileProvider`中的控制站或服務的建構函式透過[相依性插入](dependency-injection.md)。 第二種方法通常會導致更有彈性且可測試的方案。

下列範例示範如何建立`PhysicalFileProvider`。


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

您可以逐一查看目錄內容，或提供子路徑，以取得特定的檔案資訊。

若要要求提供者從控制器，指定控制器的建構函式中，並將它指派給本機欄位。 使用本機的執行個體，在動作方法：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

接著，建立的應用程式中的 提供者`Startup`類別：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

在*Index.cshtml*檢視中，逐一查看`IDirectoryContents`提供：

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

結果：

![提供者範例應用程式清單實體檔案和資料夾的檔案](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider`用來存取內嵌於組件的檔案。 在.NET Core 您內嵌的組件的檔案`<EmbeddedResource>`中的項目*.csproj*檔案：

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

您可以使用[通用慣例模式](#globbing-patterns)時指定要內嵌在組件中的檔案。 這些模式可用來比對一個或多個檔案。

> [!NOTE]
> 不太可能想要實際內嵌在組件中; 在專案中的每一個.js 檔案上述範例只會用於示範用途。

建立時`EmbeddedFileProvider`，傳遞至其建構函式，它會讀取組件。

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

上述程式碼片段示範如何建立`EmbeddedFileProvider`存取目前執行的組件。

更新範例應用程式使用`EmbeddedFileProvider`會產生下列輸出：

![列出內嵌的檔案的檔案提供者範例應用程式](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> 內嵌的資源不會公開目錄。 相反地，在它的檔名使用內嵌資源 （透過其命名空間） 的路徑`.`分隔符號。

> [!TIP]
> `EmbeddedFileProvider`建構函式可接受選擇`baseNamespace`參數。 指定這個會範圍呼叫`GetDirectoryContents`提供的命名空間底下的這些資源。

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider`結合`IFileProvider`執行個體，公開單一介面來處理來自多個提供者的檔案。 建立時`CompositeFileProvider`，您將一或多個傳遞`IFileProvider`其建構函式的執行個體：

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

更新範例應用程式使用`CompositeFileProvider`包含這兩個實體和內嵌的提供者之前設定，會產生下列輸出：

![列出實體檔案和資料夾以及內嵌的檔案的檔案提供者範例應用程式](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>監看的變更

`IFileProvider` `Watch`方法可用來監看一或多個檔案或目錄的變更。 這個方法會接受路徑字串，可以使用[通用慣例模式](#globbing-patterns)來指定多個檔案，並傳回`IChangeToken`。 這個語彙基元會公開`HasChanged`屬性可檢查，和`RegisterChangeCallback`偵測到指定的路徑字串的變更時呼叫的方法。 請注意，每個變更權杖只會呼叫其關聯的回呼回應單一變更。 若要啟用持續監控，您可以使用`TaskCompletionSource`如下所示，或重新建立`IChangeToken`執行個體以回應變更。

在本文的範例主控台應用程式會設定為每當修改文字檔案，顯示一則訊息：

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

儲存檔案數次之後結果：

![之後執行 dotnet 執行顯示監視 quotes.txt 檔案變更的應用程式和檔案已變更五次的命令視窗。](file-providers/_static/watch-console.png)

> [!NOTE]
> 有些檔案系統，例如 Docker 容器和網路共用，可能不可靠地傳送變更通知。 設定`DOTNET_USE_POLLINGFILEWATCHER`環境變數，以`1`或`true`輪詢變更的檔案系統每 4 秒。

## <a name="globbing-patterns"></a>通用慣例模式

檔案系統路徑中使用萬用字元模式比對呼叫*通用慣例模式*。 這些簡單的模式可以用來指定檔案群組。 兩個萬用字元只有`*`和`**`。

**`*`**

   符合任何項目在目前的資料夾層級，或任何檔名或任何副檔名。 相符項目會終止`/`和`.`檔案路徑中的字元。

<strong><code>**</code></strong>

   符合任何項目的跨多個目錄層級。 可用以遞迴方式來比對目錄階層內的許多檔案。

### <a name="globbing-pattern-examples"></a>通用慣例模式範例

**`directory/file.txt`**

   比對特定目錄中的特定檔案。

**<code>directory/*.txt</code>**

   比對所有檔案與`.txt`特定目錄中的擴充功能。

**`directory/*/bower.json`**

   比對所有`bower.json`目錄剛好只有一個層級中的檔案`directory`目錄。

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   比對所有檔案與`.txt`延伸模組會在下任何地方找到`directory`目錄。

## <a name="file-provider-usage-in-aspnet-core"></a>在 ASP.NET Core 檔案提供者使用量

ASP.NET Core 的幾個部分會利用檔案提供者。 `IHostingEnvironment`公開應用程式的根內容與 web 根目錄為`IFileProvider`型別。 靜態檔案中介軟體會使用檔案提供者尋找靜態檔案。 Razor 讓大量使用`IFileProvider`中尋找檢視。 Dotnet 的發佈功能會使用檔案提供者和通用慣例模式以指定要發行哪些檔案。

## <a name="recommendations-for-use-in-apps"></a>建議以供在應用程式中使用

如果您的 ASP.NET Core 應用程式需要檔案系統存取權，您可以要求的執行個體`IFileProvider`透過相依性插入，然後使用其方法來執行存取權，此範例中所示。 這可讓您一次，當應用程式啟動，而且可以減少您的應用程式具現化的實作類型的設定提供者。
