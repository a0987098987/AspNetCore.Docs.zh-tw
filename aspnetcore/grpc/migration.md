---
title: 將 gRPC 服務從 C-Core 遷移至 ASP.NET Core
author: juntaoluo
description: 瞭解如何行動現有的基於 C 核的 gRPC 應用以在 ASP.NET 核心堆疊上運行。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 451171a041f7bbb3711babd73d2fa2e245aadd28
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664133"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>將 gRPC 服務從 C-Core 遷移至 ASP.NET Core

作者：[John Luo](https://github.com/juntaoluo)

由於底層堆疊的實現,並非所有功能在[基於 C 核的 gRPC](https://grpc.io/blog/grpc-stacks)應用和基於 ASP.NET 的基於核心的應用程序之間以相同的方式工作。 本文檔突出顯示了在兩個堆疊之間遷移的關鍵差異。

## <a name="grpc-service-implementation-lifetime"></a>gRPC 服務實現存留期

在ASP.NET核心堆疊中,默認情況下,gRPC 服務使用[作用域的存留期](xref:fundamentals/dependency-injection#service-lifetimes)創建。 相反,默認情況下 gRPC C 核綁定到具有[單噸存留期的](xref:fundamentals/dependency-injection#service-lifetimes)服務。

作用域存留期允許服務實現使用作用域存留期解析其他服務。 例如,作用域存留期還可以通過構造函數`DbContext`注入從DI容器解析。 使用作用域存留期:

* 為每個請求構造服務實現的新實例。
* 無法通過實現類型的實例成員在請求之間共享狀態。
* 期望將共用狀態存儲在 DI 容器中的單個服務中。 存儲的共享狀態在 gRPC 服務實現的構造函數中解析。

有關服務存留期的詳細資訊,請參<xref:fundamentals/dependency-injection#service-lifetimes>閱 。

### <a name="add-a-singleton-service"></a>新增單例服務

為了便於從 gRPC C 核心實現過渡到ASP.NET核心,可以將服務實現的服務存留期從作用域更改為單例。 這涉及將服務實現的實體加入 DI 容器:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

但是,具有 singleton 生存期的服務實現不再能夠通過構造函數注入解析作用域服務。

## <a name="configure-grpc-services-options"></a>設定 gRPC 服務選項

在基於 C 核的應用中,[在建構 Server](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)實體`ChannelOption`設定`grpc.max_receive_message_length``grpc.max_send_message_length`了 和 等設定。

在ASP.NET核心中,gRPC`GrpcServiceOptions`通過類型提供配置。 例如,gRPC 服務的最大傳入消息大小`AddGrpc`可以通過進行配置。 以下範例將預設值`MaxReceiveMessageSize`4 MB 變更為 16 MB:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

有關設定的詳細資訊,請參閱<xref:grpc/configuration>。

## <a name="logging"></a>記錄

基於 C 核的應用`GrpcEnvironment`依賴於[配置記錄器](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)以進行調試。 ASP.NET核心堆疊通過[日誌記錄 API](xref:fundamentals/logging/index)提供此功能。 例如,可以透過建構函數注入將記錄器添加到 gRPC 服務中:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

基於 C 核的應用程式透過[Server.Ports 屬性](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)配置 HTTPS。 類似的概念用於配置ASP.NET核心中的伺服器。 例如,Kestrel 使用[終結點配置](xref:fundamentals/servers/kestrel#endpoint-configuration)進行此功能。

## <a name="grpc-interceptors-vs-middleware"></a>gRPC 攔截器 vs 中間件

ASP.NET核心[中間件](xref:fundamentals/middleware/index)提供類似的功能,與基於C核的gRPC應用中的攔截器相比。 ASP.NET核心中間件和攔截器在概念上相似。 Both (兩者)：

* 用於建構處理 gRPC 請求的管道。
* 允許在管道中的下一個元件之前或之後執行工作。
* 提供存`HttpContext`取存取權限:
  * 在中間件中`HttpContext`,是一個參數。
  * 在攔截器中,`HttpContext`可以使用擴充`ServerCallContext`方法`ServerCallContext.GetHttpContext`的 參數 存取。 請注意,此功能特定於在 ASP.NET 核心中運行的攔截器。

gRPC 攔截器與 ASP.NET 核心中間件的差異:

* 攔截器:
  * 使用[ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)在 gRPC 抽象層上操作。
  * 提供對:
    * 發送到呼叫的序列化消息。
    * 在序列化之前從調用返回的消息。
  * 可以捕獲和處理從 gRPC 服務引發的異常。
* 中介軟體：
  * 在 gRPC 攔截器之前運行。
  * 對基礎 HTTP/2 消息進行操作。
  * 只能從請求和回應流訪問位元組。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
