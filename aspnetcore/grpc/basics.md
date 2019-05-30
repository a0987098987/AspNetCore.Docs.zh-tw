---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 撰寫 gRPC 服務時，了解基本概念C#。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 5a88bd0e9f789058b3606691c5ebd9a74325ac9b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376350"
---
# <a name="grpc-services-with-c"></a>使用 c# 的 gRPC 服務\#

本文件概述撰寫所需的概念[gRPC](https://grpc.io/docs/guides/)中的應用程式C#。 本主題同時適用於[C core](https://grpc.io/blog/grpc-stacks)-根據和 ASP.NET Core 為基礎的 gRPC 應用程式。

## <a name="proto-file"></a>proto 檔案

gRPC 使用合約優先方法 API 的開發。 依預設會以介面設計語言 (IDL) 為使用通訊協定緩衝區 (protobuf)。 *.Proto*檔案包含：

* GRPC 服務的定義。
* 用戶端與伺服器之間傳送的訊息。

如需詳細的語法 protobuf 檔案的詳細資訊，請參閱[官方文件 (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3)。

例如，請考慮*greet.proto*中所使用的檔案[開始使用 gRPC service](xref:tutorials/grpc/grpc-start):

* 定義`Greeter`服務。
* `Greeter`服務會定義`SayHello`呼叫。
* `SayHello` 傳送`HelloRequest`訊息，並接收`HelloResponse`訊息：

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>將.proto 檔案新增至 C\#應用程式

*.Proto*檔案包含在專案中將它加入至`<Protobuf>`項目群組：

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a>C#.Proto 檔案的工具支援

工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)才能產生C#中的資產 *.proto*檔案。 產生的資產 （檔案）：

* 會產生可視需要在每次建置專案。
* 不會加入至專案，或簽入原始檔控制。
* 會包含在組建成品*obj*目錄。

此套件所需的伺服器和用戶端專案中。 `Grpc.Tools` 您可以加入 Visual Studio 中使用封裝管理員，或新增`<PackageReference>`souboru projektu:

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。

## <a name="generated-c-assets"></a>產生C#資產

工具套件會產生C#類型代表的定義中包含的訊息 *.proto*檔案。

針對伺服器端資產產生服務的抽象基底型別。 基底型別包含所有 gRPC 呼叫中所包含的定義 *.proto*檔案。 建立衍生自這個基底型別並實作 gRPC 呼叫的邏輯實體的服務實作。 針對`greet.proto`，範例先前所述，抽象`GreeterBase`型別，包含虛擬`SayHello`方法產生。 具象實作`GreeterService`覆寫方法，並實作處理 gRPC 呼叫的邏輯。

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

針對用戶端資產產生實體的用戶端類型。 在中，呼叫 gRPC *.proto*檔案會轉譯成具象類型，可呼叫的方法。 針對`greet.proto`，範例先前所述，具體`GreeterClient`會產生型別。 呼叫`GreeterClient.SayHello`起始 gRPC 呼叫給伺服器。

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

根據預設，為每個產生伺服器和用戶端的資產 *.proto*檔案中包含`<Protobuf>`項目群組。 若要確保只有伺服器資產在伺服器專案中，產生`GrpcServices`屬性設為`Server`。

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

同樣地，此屬性設為`Client`用戶端專案中。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
