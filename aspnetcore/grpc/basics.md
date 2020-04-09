---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 使用 C# 編寫 gRPC 服務時,瞭解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 59257449e211ddf9c7faa5f21a3986caba4eebc6
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664203"
---
# <a name="grpc-services-with-c"></a>gRPC 服務,帶 C\#

本文檔概述了在 C# 中編寫[gRPC](https://grpc.io/docs/guides/)應用所需的概念。 此處介紹的主題適用於基於[C 核](https://grpc.io/blog/grpc-stacks)和基於 ASP.NET 基於內核的 gRPC 應用。

## <a name="proto-file"></a>原型檔案

gRPC 使用協定優先的方法進行 API 開發。 默認情況下,協定緩衝區(原語)用作介面設計語言 (IDL)。 .proto 檔案包含: * \**

* gRPC 服務的定義。
* 用戶端和伺服器之間發送的消息。

有關原檔語法的詳細資訊,請參閱[官方文檔(protobuf)。](https://developers.google.com/protocol-buffers/docs/proto3)

例如,請考慮使用[gRPC 服務開始](xref:tutorials/grpc/grpc-start)中使用的*greet.proto*檔案:

* 定義`Greeter`服務。
* 服務`Greeter`定義`SayHello`呼叫。
* `SayHello`傳送`HelloRequest`訊息 並`HelloReply`接收訊息 :

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="add-a-proto-file-to-a-c-app"></a>將 .proto 檔案\#加入到 C 應用

.proto 檔案透過您新增`<Protobuf>`到項目中包含在項目中: * \**

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C# 對 .proto 檔案的工具支援

工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)需要從*\*.proto*檔案生成 C# 資源。 產生的資產(檔案):

* 在每次生成項目時根據需要生成。
* 未添加到專案或簽入原始程式碼管理。
* 是*obj*目錄中的生成專案。

伺服器和用戶端專案都需要此包。 元`Grpc.AspNetCore`包包括對的`Grpc.Tools`引用。 伺服器專案可以使用`Grpc.AspNetCore`Visual Studio 中的套件管理員,`<PackageReference>`也可以加入專案檔案加入 :

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

用戶端專案應直接與`Grpc.Tools`使用 gRPC 用戶端所需的其他包一起引用。 執行時不需要工具組,因此依賴項用 標記`PrivateAssets="All"`:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>產生的 C# 資產

工具套件生成 C# 類型,表示在包含*\*的 .proto*檔中定義的消息。

對於伺服器端資產,將生成抽象服務基礎類型。 基類型包含 *.proto*檔中包含的所有 gRPC 調用的定義。 創建源自此基類型的具體服務實現,並實現 gRPC 調用的邏輯。 對於`greet.proto`前面描述的範例,將生成包含`GreeterBase``SayHello`虛擬方法的抽象類型。 具體實現`GreeterService`重寫該方法並實現處理 gRPC 調用的邏輯。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

對於客戶端資產,將生成具體的用戶端類型。 *.proto*檔中的 gRPC 調用被轉換為具體類型上的方法,可以調用。 對於`greet.proto`前面描述的示例,將生成具體`GreeterClient`類型。 調用`GreeterClient.SayHelloAsync`以啟動對伺服器的 gRPC 呼叫。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

預設情況下,為`<Protobuf>`專案組中包括的每個*\*.proto*檔案生成伺服器和用戶端資源。 為了確保伺服器項目中僅產生伺服器資產,該`GrpcServices`屬性設定為`Server`。

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

同樣,該屬性設置為`Client`在用戶端專案中。

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
