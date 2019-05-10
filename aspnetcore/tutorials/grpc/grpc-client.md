---
title: 教學課程：建立 .NET Core gRPC 用戶端
author: juntaoluo
description: 這一系列的教學課程會示範如何在 ASP.NET Core 上建立 gRPC 服務。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: ec6bf5072c76de640a78b2c3f13dd1fc552b9d04
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212645"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>教學課程：建立 .NET Core gRPC 用戶端

作者：[John Luo](https://github.com/juntaoluo)

本教學課程示範如何建立可與 gRPC 服務通訊的 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端。

在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 用戶端。
> * 對上一個教學課程中建立的 gRPC Greeter 服務執行該服務。
> * 檢查專案檔。

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>建立 .NET 主控台應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

請遵循[此處](/dotnet/core/tutorials/with-visual-studio)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

請遵循[此處](/dotnet/core/tutorials/with-visual-studio-code)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

請遵循[此處](/dotnet/core/tutorials/using-on-mac-vs-full-solution)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>新增必要套件

將下列套件新增至 gRPC 用戶端專案：

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core)，包含 C 核心用戶端的 C# API。
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。 執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。

可使用下列方法新增套件：

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>安裝套件的 PMC 選項

* 從 Visual Studio 選取 [工具] > [NuGet 套件管理員] > [套件管理員主控台]
* 從 [套件管理員主控台] 視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。
* 執行下列命令：

    ```powershell
    Install-Package Grpc.Core
    ```

* 為 Google.Protobuf 和 Grpc.Tools 重複進行 `Install-Package`

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>管理 NuGet 套件選項來安裝套件

* 在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案
* 將 [套件來源] 設定為 "nuget.org"
* 在搜尋方塊中輸入 "Grpc.Core"
* 從 [瀏覽] 索引標籤中選取 "Grpc.Core" 套件，並按一下 [安裝]
* 為 Google.Protobuf 和 Grpc.Tools 重複進行

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

從 [整合式終端機] 執行下列命令：

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

為 Google.Protobuf 和 Grpc.Tools 重複進行

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾
* 將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"
* 在搜尋方塊中輸入 "Grpc.Core"
* 從結果窗格中選取 "Grpc.Core" 套件，並按一下 [新增套件]
* 為 Google.Protobuf 和 Grpc.Tools 重複進行

---

## <a name="add-the-greetproto-file"></a>新增 greet.proto 檔案

將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。 將 **greet.proto** 檔案新增至 GrpcGreeterClient 專案檔的 `<Protobuf>` 項目群組：

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> 以滑鼠右鍵按一下專案並從下拉式功能表中選取 [編輯 GrpcGreeterClient.csproj] 選項，可讓您開啟GrpcGreeterClient 的專案檔。
>
> ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/edit_csproj.png)
>
> 或者，您可以巡覽至 GrpcGreeterClient 目錄，使用您慣用的編輯器來編輯 `GrpcGreeterClient.csproj`。

會新增 `GrpcServices="Client"` 屬性，以僅為包含的 protobuf 檔案產生 C# 用戶端資產。 建置用戶端專案以觸發 C# 用戶端資產的產生。

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>建立 GreeterClient 並叫用 SayHello 一元呼叫

將下列程式碼新增至 gRPC 用戶端專案 `Program.cs` 檔案的 `Main` 方法：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

若要存取必要的類型，下列 using 陳述式是必要項目：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient 是透過具現化 `Channel` (包含用於建立與 gRPC 服務連線的資訊) 所建立，並使用其來建構 `GreeterClient`：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient 包含一元呼叫 `SayHello` (可透過非同步方式叫用)：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

`SayHello` 呼叫的結果，會儲存在可接著顯示的 `reply`：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

用戶端所使用的 `Channel` 應在作業完成釋放所有資源時關閉：

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> 您需要在可以解析 **Greeter** 命名空間中的類型之前建置專案。 這些類型會在建置期間自動產生，且必須在執行組建後才能使用。

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>使用 gRPC Greeter 服務測試 gRPC 用戶端

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 請確定在上一個教學課程中建立的 Greeter 服務正在執行。

* 服務開始執行後，返回 **GrpcGreeterClient** 專案並將其設定為啟始專案。 按 Ctrl+F5 即可執行用戶端而不使用偵錯工具。

  用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。 服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/client.png)

  服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 請確定在上一個教學課程中建立的 Greeter 服務正在執行。

* 使用 `dotnet run` 從個別的命令列執行用戶端專案 GrpcGreeter.Client。

用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。 服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>檢查 gRPC 專案的專案檔

gRPC 用戶端 GrpcGreeterClient 檔案：

*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。

## <a name="additional-resources"></a>其他資源

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 用戶端。
> * 對上一個教學課程中建立的 gRPC Greeter 服務執行該服務。
> * 檢查專案檔。

> [!div class="step-by-step"]
> [上一步：建立 gRPC Greeter 服務](xref:tutorials/grpc/grpc-start)
