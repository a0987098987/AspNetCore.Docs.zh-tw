---
title: .NET 內核上的 gRPC 簡介
author: juntaoluo
description: 了解搭配 Kestrel 伺服器及 ASP.NET Core 堆疊的 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/20/2019
uid: grpc/index
ms.openlocfilehash: d97eea1da28424680a3cfa38102637b1e20ff661
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667311"
---
# <a name="introduction-to-grpc-on-net-core"></a>.NET 內核上的 gRPC 簡介

由[約翰·羅](https://github.com/juntaoluo)和[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

[gRPC](https://grpc.io/docs/guides/) 是不限於語言的高效能遠端程序呼叫 (RPC) 架構。

gRPC 的主要優點包括：
* 現代、高性能、輕量級的 RPC 框架。
* 根據預設使用 Protocol Buffers 的合約優先式 API 開發，使您得以進行不限於語言的實作。
* 適用於多種語言的工具，可產生強型別伺服器及用戶端。
* 支援用戶端、伺服器及雙向資料流呼叫。
* 透過 Protobuf 二進位序列化減少網路使用量。

這些優點讓 gRPC 非常適合：
* 首重效率的輕量型微服務。
* 必須使用多種語言進行開發的多語言系統。
* 必須處理資料流要求或回應的點對點即時服務。

## <a name="c-tooling-support-for-proto-files"></a>C# 對 .proto 檔案的工具支援

gRPC 使用協定優先的方法進行 API 開發。 服務和訊息在*\*.proto*檔案中定義:

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

服務的 .NET 型態、用戶端和訊息通過在專案中包含*\*.proto*檔案自動產生:

* 添加對[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)套件的包引用。
* 將*\*.proto*檔案`<Protobuf>`添加到專案組。

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" />
</ItemGroup>
```

有關 gRPC 工具支援的詳細資訊,請<xref:grpc/basics>參閱 。

## <a name="grpc-services-on-aspnet-core"></a>ASP.NET核心上的 gRPC 服務

gRPC 服務可以託管在 ASP.NET 核心上。 服務與流行的ASP.NET核心功能(如日誌記錄、依賴項注入 (DI)、身份驗證和授權)完全整合。

gRPC 服務專案樣本提供啟動服務:

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

`GreeterService`從類型繼承,`GreeterBase`該類型是`Greeter`從*\*.proto*檔案中的服務生成的類型。 該服務可供*Startup.cs*客戶端存取:

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapGrpcService<GreeterService>();
});
```

要瞭解有關 ASP.NET核心上的 gRPC 服務<xref:grpc/aspnetcore>的更多詳細資訊,請參閱 。

## <a name="call-grpc-services-with-a-net-client"></a>使用 .NET 用戶端呼叫 gRPC 服務

gRPC 用戶端是從[*\*.proto*檔生成的](xref:grpc/basics#generated-c-assets)具體用戶端類型。 具體的 gRPC 用戶端具有在*\*.proto*檔中轉換為 gRPC 服務的方法。

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greeter.GreeterClient(channel);

var response = await client.SayHelloAsync(
    new HelloRequest { Name = "World" });

Console.WriteLine(response.Message);
```

gRPC 用戶端使用通道創建,該通道表示與 gRPC 服務的長期連接。 可以使用 建立`GrpcChannel.ForAddress`色版 。

有關建立客戶端和呼叫不同服務方法的詳細資訊,請參<xref:grpc/client>閱 。

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>其他資源

* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
* <xref:grpc/clientfactory>
* <xref:tutorials/grpc/grpc-start>
