---
title: .NET Core 上的 gRPC 簡介
author: juntaoluo
description: 了解搭配 Kestrel 伺服器及 ASP.NET Core 堆疊的 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/20/2019
uid: grpc/index
ms.openlocfilehash: d97eea1da28424680a3cfa38102637b1e20ff661
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667311"
---
# <a name="introduction-to-grpc-on-net-core"></a>.NET Core 上的 gRPC 簡介

依[John 羅文](https://github.com/juntaoluo)和[James 的牛頓-王](https://twitter.com/jamesnk)

[gRPC](https://grpc.io/docs/guides/) 是不限於語言的高效能遠端程序呼叫 (RPC) 架構。

gRPC 的主要優點包括：
* 現代化、高效能、輕量的 RPC 架構。
* 根據預設使用 Protocol Buffers 的合約優先式 API 開發，使您得以進行不限於語言的實作。
* 適用於多種語言的工具，可產生強型別伺服器及用戶端。
* 支援用戶端、伺服器及雙向資料流呼叫。
* 透過 Protobuf 二進位序列化減少網路使用量。

這些優點讓 gRPC 非常適合：
* 首重效率的輕量型微服務。
* 必須使用多種語言進行開發的多語言系統。
* 必須處理資料流要求或回應的點對點即時服務。

## <a name="c-tooling-support-for-proto-files"></a>C#適用于 proto 檔案的工具支援

gRPC 會使用合約優先的方法來開發 API。 服務和訊息會定義在 *\*的 proto*檔案中：

```protobuf
syntax = "proto3";

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}
```

服務、用戶端和訊息的 .NET 類型會藉由在專案中包含 *\*的 proto*檔案來自動產生：

* 將套件參考新增至[Grpc](https://www.nuget.org/packages/Grpc.Tools/)套件。
* 將 *\*的 proto*檔案加入至 `<Protobuf>` 專案群組。

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" />
</ItemGroup>
```

如需 gRPC 工具支援的詳細資訊，請參閱 <xref:grpc/basics>。

## <a name="grpc-services-on-aspnet-core"></a>ASP.NET Core 上的 gRPC 服務

gRPC 服務可以裝載于 ASP.NET Core 上。 服務具有與熱門 ASP.NET Core 功能的完整整合，例如記錄、相依性插入（DI）、驗證和授權。

GRPC 服務專案範本提供入門服務：

```csharp
public class GreeterService : Greeter.GreeterBase
{
    private readonly ILogger<GreeterService> _logger;

    public GreeterService(ILogger<GreeterService> logger)
    {
        _logger = logger;
    }

    public override Task<HelloReply> SayHello(HelloRequest request,
        ServerCallContext context)
    {
        _logger.LogInformation("Saying hello to {Name}", request.Name);
        return Task.FromResult(new HelloReply 
        {
            Message = "Hello " + request.Name
        });
    }
}
```

`GreeterService` 繼承自 `GreeterBase` 類型，這是從 *\** 的 `Greeter` 服務中產生的。 此服務可供*Startup.cs*中的用戶端存取：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapGrpcService<GreeterService>();
});
```

若要深入瞭解 ASP.NET Core 上的 gRPC 服務，請參閱 <xref:grpc/aspnetcore>。

## <a name="call-grpc-services-with-a-net-client"></a>使用 .NET 用戶端呼叫 gRPC 服務

gRPC 用戶端是[從 *\*的 proto*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。 具體 gRPC 用戶端的方法會轉譯為 *\*的 proto*檔案中的 gRPC 服務。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greeter.GreeterClient(channel);

var response = await client.SayHelloAsync(
    new HelloRequest { Name = "World" });

Console.WriteLine(response.Message);
```

GRPC 用戶端是使用通道所建立，這代表 gRPC 服務的長時間連接。 您可以使用 `GrpcChannel.ForAddress`來建立通道。

如需建立用戶端和呼叫不同服務方法的詳細資訊，請參閱 <xref:grpc/client>。

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/clientfactory>
* <xref:tutorials/grpc/grpc-start>
