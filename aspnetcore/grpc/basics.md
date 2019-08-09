---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 瞭解使用C#撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: b236fe6914cf7b780a9d02398ec9c92660dc1063
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862862"
---
# <a name="grpc-services-with-c"></a>使用 C gRPC 服務\#

本檔概述在中C#撰寫[gRPC](https://grpc.io/docs/guides/)應用程式所需的概念。 這裡涵蓋的主題適用于以[C 核心](https://grpc.io/blog/grpc-stacks)和 ASP.NET Core 為基礎的 gRPC 應用程式。

## <a name="proto-file"></a>proto 檔案

gRPC 會使用合約優先的方法來開發 API。 通訊協定緩衝區 (protobuf) 預設會當做介面設計語言 (IDL) 使用。 此*proto*檔案包含:

* GRPC 服務的定義。
* 在用戶端與伺服器之間傳送的訊息。

如需 protobuf 檔語法的詳細資訊, 請參閱[官方檔 (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3)。

例如, 請考慮[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start)中使用的*歡迎*:

* `Greeter`定義服務。
* 服務會定義一個`SayHello`呼叫。 `Greeter`
* `SayHello`傳送訊息並接收`HelloReply`訊息: `HelloRequest`

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>將 proto 檔案新增至 C\#應用程式

在專案中加入此*proto*檔案, 方法是將它加入至`<Protobuf>`專案群組:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#適用于 proto 檔案的工具支援

需要工具套件[Grpc](https://www.nuget.org/packages/Grpc.Tools/) , 才能從C# *proto*檔案產生資產。 產生的資產 (files):

* 每次建立專案時, 會在需要時產生。
* 不會加入至專案或簽入原始檔控制。
* 是包含在*obj*目錄中的組建成品。

伺服器和用戶端專案都需要此封裝。 中繼套件包含的`Grpc.Tools`參考。 `Grpc.AspNetCore` 伺服器專案可以在`Grpc.AspNetCore` Visual Studio 中使用封裝管理員, 或藉由`<PackageReference>`將加入至專案檔來加入:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

用戶端專案應直接`Grpc.Tools`與使用 gRPC 用戶端所需的其他套件一起參考。 執行時間不需要工具套件, 因此會以下列方式`PrivateAssets="All"`標記相依性:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>產生C#的資產

工具套件會產生代表C#包含的 *. proto*檔案中所定義之訊息的類型。

若為伺服器端資產, 則會產生抽象服務基底類型。 基底類型包含包含在*proto*檔案中所有 gRPC 呼叫的定義。 建立衍生自這個基底類型的具象服務實作為, 並針對 gRPC 呼叫執行邏輯。 針對, 先前所述的範例會產生包含`GreeterBase`虛擬`SayHello`方法的抽象類別型。 `greet.proto` 具體的執行`GreeterService`會覆寫方法, 並實行處理 gRPC 呼叫的邏輯。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

針對用戶端資產, 會產生具體的用戶端類型。 在*proto*檔案中的 gRPC 呼叫會轉譯為具象型別上的方法, 可以呼叫。 針對, 先前所述的範例會產生具體`GreeterClient`類型。 `greet.proto` 呼叫`GreeterClient.SayHelloAsync`以起始對伺服器的 gRPC 呼叫。

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=3-6&name=snippet)]

根據預設, 會針對`<Protobuf>`專案群組中包含的每個*proto*檔案產生伺服器和用戶端資產。 為確保伺服器專案中只產生伺服器資產, `GrpcServices`屬性會設定為。 `Server`

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

同樣地, 在用戶端專案`Client`中, 屬性會設定為。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
