---
title: 將 gRPC 服務從 C-核心遷移至 ASP.NET Core
author: juntaoluo
description: 瞭解如何移動現有以 C 核心為基礎的 gRPC 應用程式, 以在 ASP.NET Core 堆疊上執行。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 39aa711a1a47cf11ec5b08903b4130c7caa1501c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022293"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>將 gRPC 服務從 C-核心遷移至 ASP.NET Core

作者：[John Luo](https://github.com/juntaoluo)

由於基礎堆疊的執行, 並非所有功能在以[C 核心為基礎的 gRPC](https://grpc.io/blog/grpc-stacks)應用程式和 ASP.NET Core 型應用程式之間以相同的方式工作。 本檔將重點放在兩個堆疊之間進行遷移的主要差異。

## <a name="grpc-service-implementation-lifetime"></a>gRPC 服務實生命週期

在 ASP.NET Core 堆疊中, 預設會以[限定範圍的存留期](xref:fundamentals/dependency-injection#service-lifetimes)來建立 gRPC 服務。 相反地, gRPC C-core 預設會系結至具有[單一存留期](xref:fundamentals/dependency-injection#service-lifetimes)的服務。

限定範圍存留期可讓服務執行解析具有限定範圍存留期的其他服務。 例如, 範圍存留期也可以透過函`DbContext`式插入, 從 DI 容器解析。 使用範圍存留期:

* 服務實的新實例會針對每個要求而建立。
* 您無法透過執行類型上的實例成員, 在要求之間共用狀態。
* 預期的情況是將共用狀態儲存在 DI 容器的單一服務中。 儲存的共用狀態會在 gRPC 服務執行的函式中解析。

如需服務存留期的詳細資訊<xref:fundamentals/dependency-injection#service-lifetimes>, 請參閱。

### <a name="add-a-singleton-service"></a>新增單一服務

為了協助從 gRPC C 核心的執行轉換成 ASP.NET Core, 您可以將服務實的服務存留期從範圍變更為 singleton。 這牽涉到將服務實作為實例新增至 DI 容器:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

不過, 具有單一存留期的服務執行, 無法再透過函式插入來解析已設定範圍的服務。

## <a name="configure-grpc-services-options"></a>設定 gRPC services 選項

在以 C 為基礎的應用程式中, 當`grpc.max_receive_message_length`您`grpc.max_send_message_length`在[建立伺服器實例](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)時, 會使用`ChannelOption`和等設定進行設定。

在 ASP.NET Core 中, gRPC 會透過`GrpcServiceOptions`類型提供設定。 例如, 您可以透過`AddGrpc`下列方式設定 gRPC 服務的最大傳入訊息大小:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

如需設定的詳細資訊, <xref:grpc/configuration>請參閱。

## <a name="logging"></a>記錄

`GrpcEnvironment`以 C 核心為基礎的應用程式依賴來[設定記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)進行偵錯工具。 ASP.NET Core 堆疊會透過[記錄 API](xref:fundamentals/logging/index)提供這種功能。 例如, 您可以透過函式插入, 將記錄器新增至 gRPC 服務:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

以 C 核心為基礎的應用程式會透過[伺服器埠屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)來設定 HTTPS。 在 ASP.NET Core 中設定伺服器時, 會使用類似的概念。 例如, Kestrel 會使用[端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定來取得這種功能。

## <a name="interceptors-and-middleware"></a>攔截器和中介軟體

相較于以 C 核心為基礎的 gRPC 應用程式中的攔截器, ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)提供類似的功能。 中介軟體和攔截器的概念相同, 兩者都是用來建立處理 gRPC 要求的管線。 兩者都允許在管線中的下一個元件之前或之後執行工作。 不過, ASP.NET Core 中介軟體會在基礎 HTTP/2 訊息上運作, 而攔截器則是使用[ServerCallCoNtext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)在抽象概念的 gRPC 層上運作。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
