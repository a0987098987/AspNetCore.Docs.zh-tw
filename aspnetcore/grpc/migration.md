---
title: 從 C 核心移轉的 gRPC 服務，ASP.NET core
author: juntaoluo
description: 了解如何移動現有的 C core 根據的 gRPC 應用程式來執行 ASP.NET Core 堆疊的頂端。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895235"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>從 C 核心移轉的 gRPC 服務，ASP.NET core

作者：[John Luo](https://github.com/juntaoluo)

由於基礎堆疊實作，並非所有功能則是之間的相同方式都運作[C 核心為基礎的 gRPC](https://grpc.io/blog/grpc-stacks)應用程式和 ASP.NET Core 為基礎的應用程式。 本文件特別說明移轉兩個堆疊之間的主要差異。

## <a name="grpc-service-implementation-lifetime"></a>gRPC 服務實作的存留期

在 ASP.NET Core 堆疊，gRPC 服務，根據預設，會建立與[具範圍存留期](xref:fundamentals/dependency-injection#service-lifetimes)。 相較之下，gRPC C core 預設會將服務繫結[單一存留期](xref:fundamentals/dependency-injection#service-lifetimes)。

具範圍存留期可讓您用來解析已設定領域的存留期與其他服務的服務實作。 比方說，也可以解析已設定領域的存留期`DBContext`從 DI 容器透過建構函式插入。 使用具範圍存留期：

* 服務實作的新執行個體的建構針對每個要求。
* 您無法透過執行個體上的實作類型的成員要求間共用狀態。
* 預期是儲存在 DI 容器中的單一服務中的共用的狀態。 GRPC 服務實作的建構函式中，會解析預存的共用的狀態。

如需有關服務生命週期的詳細資訊，請參閱<xref:fundamentals/dependency-injection#service-lifetimes>。

### <a name="add-a-singleton-service"></a>新增單一服務

為了方便從 gRPC C core 實作轉換到 ASP.NET Core，就可以變更服務實作的服務的存留期只限於單一。 這牽涉到將服務實作的執行個體新增至 DI 容器：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

不過，已不再能夠解析範圍的服務，透過建構函式插入具有單一存留期服務實作。

## <a name="configure-grpc-services-options"></a>設定 gRPC 服務選項

在 C 核心為基礎的應用程式，設定，例如`grpc.max_receive_message_length`並`grpc.max_send_message_length`設有`ChannelOption`時[建構的伺服器執行個體](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)。

ASP.NET Core 中 gRPC 提供透過設定`GrpcServiceOptions`型別。 比方說，gRPC 服務的連入訊息大小上限可以透過設定`AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

如需有關組態的詳細資訊，請參閱<xref:grpc/configuration>。

## <a name="logging"></a>記錄

C 核心為基礎的應用程式依賴`GrpcEnvironment`要[設定記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)以進行偵錯。 ASP.NET Core 堆疊提供這項功能，透過[記錄 API](xref:fundamentals/logging/index)。 比方說，記錄器可以新增至透過建構函式插入 gRPC 服務：

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

C 核心為基礎的應用程式設定透過 HTTPS [Server.Ports 屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)。 類似的概念用來設定 ASP.NET Core 中的伺服器。 例如，會使用 Kestrel[端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration)這項功能。

## <a name="interceptors-and-middleware"></a>攔截器與中介軟體

ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)提供類似的功能相較於 C 核心為基礎的 gRPC 應用程式中的攔截器。 中介軟體和攔截器在概念上是相同，兩者都用來建構管線，以處理 gRPC 要求。 這兩者都可以在管線中的下一個元件的前後執行的工作。 不過，ASP.NET Core 中介軟體作基礎 HTTP/2 的訊息，而使用抽象的 gRPC 圖層上運作的攔截器[ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
