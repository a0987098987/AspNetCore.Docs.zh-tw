---
title: 使用 dotnet-grpc 管理 Protobuf 參考
author: juntaoluo
description: 瞭解如何使用 dotnet-grpc 全域工具添加、更新、刪除和列出 Protobuf 引用。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667332"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a>使用 dotnet-grpc 管理 Protobuf 參考

作者：[John Luo](https://github.com/juntaoluo)

`dotnet-grpc`是 .NET 核心全域工具,用於管理 .NET gRPC 專案中[的 Protobuf *(.proto*)](xref:grpc/basics#proto-file)引用。 該工具可用於添加、刷新、刪除和列出 Protobuf 引用。

## <a name="installation"></a>安裝

要安裝`dotnet-grpc` [.NET 核心全域工具](/dotnet/core/tools/global-tools),請執行以下指令:

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a>新增參考

`dotnet-grpc`可用於將 Protobuf`<Protobuf />`引用為項目新增*到 .csproj*檔:

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

Protobuf 引用用於生成 C# 用戶端和/或伺服器資產。 該工具`dotnet-grpc`可以:

* 從磁碟上的本地檔創建 Protobuf 引用。
* 從 URL 指定的遠端檔案中創建 Protobuf 引用。
* 確保向專案添加了正確的 gRPC 包依賴項。

例如,包`Grpc.AspNetCore`將添加到 Web 應用。 `Grpc.AspNetCore`包含 gRPC 伺服器和用戶端庫以及工具支援。 或者,`Grpc.Net.Client`僅`Grpc.Tools``Google.Protobuf`包含 gRPC 用戶端庫和工具支援的 的和 包將添加到主控台應用。

### <a name="add-file"></a>新增檔案

該`add-file`命令用於在磁碟上添加本地檔作為 Protobuf 引用。 提供的檔案路徑:

* 可以相對於當前目錄或絕對路徑。
* 可能包含基於模式的檔案[globing](https://wikipedia.org/wiki/Glob_(programming))的通配符。

如果專案目錄之外有任何檔,則添加一`Link`個元素以在 Visual Studio 中的`Protos`資料夾下顯示該檔。

### <a name="usage"></a>使用量

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| files | 原檔引用。 這些可以是本地原體檔 glob 的路徑。 |

#### <a name="options"></a>選項。

| 短選項 | 長選項 | 描述 |
|-|-|-|
| -p | --專案 | 要操作的專案檔的路徑。 如果未指定檔,該命令將搜索當前目錄。
| -S | --服務 | 應生成的 gRPC 服務的類型。 如果`Default`指定,`Both`則用於 Web`Client`專案, 用於非 Web 專案。 接受的值是`Both``Client` `Default` `None`, `Server`, , , , , , ,
| -i | --附加導入-迪爾 | 解析原文件導入時要使用的其他目錄。 這是路徑的分號分隔清單。
| | --訪問 | 用於生成的 C# 類別的訪問修改器。 預設值是 `Public`。 接受的值為 `Internal` 和 `Public`。

### <a name="add-url"></a>新增 URL

該`add-url`指令用於添加源網址指定的遠端檔案作為 Protobuf 引用。 必須提供檔案路徑以指定下載遠端檔案的位置。 檔路徑可以是相對於當前目錄或絕對路徑。 如果檔案路徑位於項目目錄之外,則添加一`Link`個元素以在 Visual Studio`Protos`中的虛擬 資料夾下顯示該檔。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| url | 遠端原檔案 URL。 |

#### <a name="options"></a>選項。

| 短選項 | 長選項 | 描述 |
|-|-|-|
| -o | --output | 指定遠端原檔案的下載路徑。 這是必要選項。
| -p | --專案 | 要操作的專案檔的路徑。 如果未指定檔,該命令將搜索當前目錄。
| -S | --服務 | 應生成的 gRPC 服務的類型。 如果`Default`指定,`Both`則用於 Web`Client`專案, 用於非 Web 專案。 接受的值是`Both``Client` `Default` `None`, `Server`, , , , , , ,
| -i | --附加導入-迪爾 | 解析原文件導入時要使用的其他目錄。 這是路徑的分號分隔清單。
| | --訪問 | 用於生成的 C# 類別的訪問修改器。 預設值為 `Public`。 接受的值為 `Internal` 和 `Public`。

## <a name="remove"></a>移除

該`remove`命令用於從 *.csproj*檔中刪除 Protobuf 引用。 該命令接受路徑參數和源 URL 作為參數。 該工具:

* 僅刪除 Protobuf 引用。
* 不會刪除 *.proto*檔,即使它最初是從遠端 URL 下載的。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| 參考 | 要刪除的原語引用的 URL 或檔案路徑。 |

### <a name="options"></a>選項。

| 短選項 | 長選項 | 描述 |
|-|-|-|
| -p | --專案 | 要操作的專案檔的路徑。 如果未指定檔,該命令將搜索當前目錄。

## <a name="refresh"></a>Refresh

該`refresh`指令用於使用源 URL 中的最新內容更新遠端引用。 下載檔案路徑和源 URL 都可用於指定要更新的引用。 注意:

* 比較文件內容的哈希,以確定是否應更新本地檔案。
* 不比較時間戳資訊。

如果需要更新,該工具始終將本地檔替換為遠端檔。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a>引數

| 引數 | 描述 |
|-|-|
| 參考 | 應更新的 URL 或檔案路徑到遠端原代引用引用。 將此參數留空以刷新所有遠端引用。 |

### <a name="options"></a>選項。

| 短選項 | 長選項 | 描述 |
|-|-|-|
| -p | --專案 | 要操作的專案檔的路徑。 如果未指定檔,該命令將搜索當前目錄。
| | --幹跑 | 輸出將在不下載任何新內容的情況下更新的檔案清單。

## <a name="list"></a>清單

該`list`命令用於在專案檔中顯示所有 Protobuf 引用。 如果列的所有值都是預設值,則可以省略該列。

### <a name="usage"></a>使用量

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a>選項。

| 短選項 | 長選項 | 描述 |
|-|-|-|
| -p | --專案 | 要操作的專案檔的路徑。 如果未指定檔,該命令將搜索當前目錄。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
