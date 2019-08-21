---
page_type: sample
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: a281adc3b1fe90eeb32c1185750f911af683af83
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029011"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a>使用 Visual Studio 在 ASP.NET Core 3.0 中建立 gRPC 用戶端和伺服器

本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。

在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。

在本教學課程中，您會：

* 建立 gRPC 伺服器。
* 建立 gRPC 用戶端。
* 利用 gRPC Greeter 服務來測試 gRPC 用戶端。

## <a name="create-a-grpc-service"></a>建立 gRPC 服務

* 從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。
* 在 [建立新專案]  對話方塊中，選取 [ASP.NET Core Web 應用程式]  。
* 選取 [下一步] 
* 將專案命名為 **GrpcGreeter**。 請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。
* 選取 [建立] 
* 在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中：
  * 從下拉式功能表中，選取 [.NET Core]  與 [ASP.NET Core 3.0]  。 
  * 選取 [gRPC 服務]  範本。
  * 選取 [建立] 

### <a name="run-the-service"></a>執行服務

* 按 `Ctrl+F5` 執行 gRPC 服務，但不啟動偵錯工具。

  Visual Studio 會在命令提示字元中執行服務。

該記錄會顯示正在接聽 `https://localhost:5001` 的服務。

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> gRPC 範本已設為使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)。 gRPC 用戶端需要使用 HTTPS 來呼叫伺服器。
>
> macOS 不支援具有 TLS 的 ASP.NET Core gRPC。 您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。 如需詳細資訊，請參閱 [macOS 上的 gRPC 和 ASP.NET Core](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos)。

### <a name="examine-the-project-files"></a>檢查專案檔

*GrpcGreeter* 專案檔：

* *greet.proto*：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。 如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。
* *Services* 資料夾：包含 `Greeter` 服務的實作。
* *appSettings.json*：包含組態資料，例如 Kestrel 所使用的通訊協定。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。
* *Program.cs*：包含 gRPC 服務的進入點。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。
* *Startup.cs*：包含設定應用程式行為的程式碼。 如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。

## <a name="create-the-grpc-client-in-a-net-console-app"></a>在 .NET 主控台應用程式中建立 gRPC 用戶端

* 開啟 Visual Studio 的第二個執行個體。
* 從功能表列中選取 [檔案]   >  [新增]   >  [專案]  。
* 在 [建立新專案]  對話方塊中，選取 [主控台應用程式 (.NET Core)]  。
* 選取 [下一步] 
* 在 [名稱]  文字方塊中，輸入 "GrpcGreeterClient"。
* 選取 [建立]  。

### <a name="add-required-packages"></a>新增必要套件

gRPC 用戶端專案需要下列套件：

* [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)，包含 .NET Core 用戶端。
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。 執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。

使用套件管理員主控台 (PMC) 或管理 NuGet 套件來安裝套件。

#### <a name="pmc-option-to-install-packages"></a>安裝套件的 PMC 選項

* 從 Visual Studio 選取 [工具]   > [NuGet 套件管理員]   > [套件管理員主控台] 
* 從 [套件管理員主控台]  視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。
* 執行下列命令：

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>管理 NuGet 套件選項來安裝套件

* 在 [方案總管]   > [管理 NuGet 套件]  中，以滑鼠右鍵按一下專案
* 選取 [瀏覽]  索引標籤。
* 在搜尋方塊中輸入 **Grpc.Net.Client**。
* 從 [瀏覽]  索引標籤選取 [Grpc.Net.Client]  套件，然後選取 [安裝]  。
* 對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。

### <a name="add-greetproto"></a>新增 greet.proto

* 在 gRPC 用戶端專案中建立 [Protos]  資料夾。
* 將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。
* 編輯 *GrpcGreeterClient.csproj* 專案檔：

  以滑鼠右鍵按一下專案，然後選取 [編輯專案檔]  。

* 新增具有代表 **greet.proto** 檔案之 `<Protobuf>` 元素的項目群組：

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>建立 Greeter 用戶端

建置專案，以便在 `GrpcGreeter` 命名空間中建立類型。 `GrpcGreeter` 類型會自動由建置處理序產生。

利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var httpClient = new HttpClient();
            // The port number(5001) must match the port of the gRPC server.
            httpClient.BaseAddress = new Uri("https://localhost:5001");
            var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。

Greeter 用戶端建立者：

* 具現化包含建立 gRPC 服務連線資訊的 `HttpClient`。
* 使用 `HttpClient` 建構 Greeter 用戶端：

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

Greeter 用戶端會呼叫非同步的 `SayHello` 方法。 顯示 `SayHello` 呼叫的結果：

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>使用 gRPC Greeter 服務測試 gRPC 用戶端

* 在 Greeter 服務中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動伺服器。
* 在 `GrpcGreeterClient` 專案中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動用戶端。

用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。 該服務會傳送 "Hello GreeterClient" 訊息做為回應。 "Hello GreeterClient" 回應會顯示在命令提示字元中：

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中。

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="docs-help--next-steps-for-grpc"></a>gRPC 的文件協助和後續步驟

* [ASP.NET Core 上的 gRPC 簡介](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [使用 C# 的 gRPC 服務](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [將 gRPC 服務從 C-Core 遷移至 ASP.NET Core](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
