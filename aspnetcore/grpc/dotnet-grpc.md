---
title: 使用 dotnet 管理 Protobuf 參考-grpc
author: juntaoluo
description: 瞭解如何使用 dotnet-grpc 通用工具新增、更新、移除和列出 Protobuf 參考。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/24/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: ebd57419be24f7f4ed9765e36cf14189be8438b1
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290058"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>使用 dotnet 管理 Protobuf 參考-grpc

作者：[John Luo](https://github.com/juntaoluo)

`dotnet-grpc` 是 .NET Core 通用工具，可用於管理 .NET gRPC 專案內的 Protobuf 參考。 此工具可以用來新增、重新整理、移除和列出 Protobuf 的參考。

## <a name="installation"></a>安裝

若要安裝 `dotnet-grpc` [.Net Core 通用工具](/dotnet/core/tools/global-tools)，請執行下列命令：

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>新增參考

`dotnet-grpc` 可以用來將 Protobuf 參考新增為 *.csproj*檔案中的 @no__t 1 專案：

```xml
<Protobuf Include="..\Proto\count.proto" GrpcServices="Server" Link="Protos\count.proto" />
```

Protobuf 參考是用來產生C#用戶端和/或伺服器資產。 @No__t 0tool 可以：

* 從磁片上的本機檔案建立 Protobuf 參考。
* 從 URL 所指定的遠端檔案建立 Protobuf 參考。
* 請確定已將正確的 gRPC 套件相依性新增至專案。

例如，`Grpc.AspNetCore` 套件會新增至 web 應用程式。 `Grpc.AspNetCore` 包含 gRPC 伺服器和用戶端程式庫和工具支援。 或者，只包含 gRPC 用戶端程式庫和工具支援的 `Grpc.Net.Client`、@no__t 1 和 @no__t 2 套件會新增至主控台應用程式。

### <a name="add-file"></a>新增檔案

@No__t-0 命令是用來將磁片上的本機檔案新增為 Protobuf 參考。 提供的檔案路徑：

* 可以相對於目前的目錄或絕對路徑。
* 可能包含以模式為基礎之檔案[通配](https://wikipedia.org/wiki/Glob_(programming))的萬用字元。

如果有任何檔案位於專案目錄外，則會加入 `Link` 元素，以在 Visual Studio 中的資料夾 `Protos` 顯示檔案。

### <a name="usage"></a>使用量

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| files | Protobuf 檔案會參考。 這些可以是本機 protobuf 檔案的 glob 路徑。 |

#### <a name="options"></a>選項。

| Short 選項 | Long 選項 | 描述 |
|-|-|-|
| -p | --project | 要操作之專案檔的路徑。 如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。
| -s | --服務 | 應產生的 gRPC 服務類型。 如果指定了 `Default`，則會將 `Both` 用於 Web 專案，而 `Client` 則用於非 Web 專案。 接受的值為 `Both`，`Client`，`Default`，`None`，`Server`。
| -i | --其他-匯入-目錄 | 解析 protobuf 檔案的匯入時，所要使用的其他目錄。 這是以分號分隔的路徑清單。
| | --access | 要用於產生C#之類別的存取修飾詞。 預設值為 `Public`。 接受的值為 `Internal`，`Public`。

### <a name="add-url"></a>新增 URL

@No__t-0 命令是用來將來源 URL 所指定的遠端檔案新增為 Protobuf 參考。 必須提供檔案路徑，以指定要下載遠端檔案的位置。 檔案路徑可以相對於目前的目錄或絕對路徑。 如果檔案路徑位於專案目錄外，則會新增 `Link` 元素，以在 Visual Studio 中的虛擬資料夾 `Protos` 顯示該檔案。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| url | 遠端 protobuf 檔的 URL。 |

#### <a name="options"></a>選項。

| Short 選項 | Long 選項 | 描述 |
|-|-|-|
| -o | --output | 指定遠端 protobuf 檔的下載路徑。 這是必要選項。
| -p | --project | 要操作之專案檔的路徑。 如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。
| -s | --服務 | 應產生的 gRPC 服務類型。 如果指定了 `Default`，則會將 `Both` 用於 Web 專案，而 `Client` 則用於非 Web 專案。 接受的值為 `Both`，`Client`，`Default`，`None`，`Server`。
| -i | --其他-匯入-目錄 | 解析 protobuf 檔案的匯入時，所要使用的其他目錄。 這是以分號分隔的路徑清單。
| | --access | 要用於產生C#之類別的存取修飾詞。 預設值為 `Public`。 接受的值為 `Internal`，`Public`。

## <a name="remove"></a>移除

@No__t-0 命令是用來從 *.csproj*檔案中移除 Protobuf 參考。 命令接受路徑引數和來源 Url 做為引數。 工具：

* 只會移除 Protobuf 參考。
* 不會刪除此*proto*檔案，即使該檔案原本是從遠端 URL 下載也一樣。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| 參考 | 要移除之 protobuf 參考的 Url 或檔案路徑。 |

### <a name="options"></a>選項。

| Short 選項 | Long 選項 | 描述 |
|-|-|-|
| -p | --project | 要操作之專案檔的路徑。 如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。

## <a name="refresh"></a>重新整理

@No__t-0 命令是用來以來源 URL 的最新內容來更新遠端參考。 下載檔案路徑和來源 URL 都可以用來指定要更新的參考。 注意：

* 檔案內容的雜湊會進行比較，以判斷是否應該更新本機檔案。
* 不會比較任何時間戳記資訊。

如果需要更新，此工具一律會將本機檔案取代為遠端檔案。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| 參考 | 應更新之遠端 protobuf 參考的 Url 或檔案路徑。 將此引數保留空白以重新整理所有遠端參考。 |

### <a name="options"></a>選項。

| Short 選項 | Long 選項 | 描述 |
|-|-|-|
| -p | --project | 要操作之專案檔的路徑。 如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。
| | --dry-run | 輸出會更新而不下載任何新內容的檔案清單。

## <a name="list"></a>List

@No__t-0 命令是用來顯示專案檔中的所有 Protobuf 參考。 如果資料行的所有值都是預設值，可能會省略資料行。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>選項。

| Short 選項 | Long 選項 | 描述 |
|-|-|-|
| -p | --project | 要操作之專案檔的路徑。 如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
