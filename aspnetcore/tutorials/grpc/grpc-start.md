---
title: 教學課程：開始在 ASP.NET Core 中使用 gRPC
author: juntaoluo
description: 這一系列的教學課程會示範如何在 ASP.NET Core 上建立 gRPC 服務。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流處理呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672563"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>教學課程：開始在 ASP.NET Core 中使用 gRPC 服務

作者：[John Luo](https://github.com/juntaoluo)

本教學課程將教導在 ASP.NET Core 上建置 gRPC 服務的基本概念。

最後，您將會有一個回應問候語的 gRPC 服務。

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 服務。
> * 執行 gRPC 服務。
> * 檢查專案檔。

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>建立 gRPC 服務

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。
  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.1.png)
* 將專案命名為 **GrpcGreeter**。 請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。
  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.2.png)
* 從下拉式清單中選取 [.NET Core] 和 [ASP.NET Core 3.0]。 選擇 [gRPC 服務] 範本。

  下列起始專案會隨即建立：

  ![底下提供說明，包括方案總管](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為其中包含專案的資料夾。
* 執行下列命令：

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * `dotnet new` 命令會在 [GrpcGreeter] 資料夾中建立一個新的 gRPC 服務。
  * `code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter] 資料夾。

  對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**
* 選取 [是]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

從終端機中，執行下列命令：

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。

### <a name="open-the-project"></a>開啟專案

從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 [GrpcGreeter.sln] 檔案。

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>執行服務

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 按 Ctrl+F5 執行 gRPC 服務，而不啟動偵錯工具。

  Visual Studio 會在命令提示字元中執行服務。 記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 使用 `dotnet run` 從命令列執行 gRPC 專案 GrpcGreeter。 記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。

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
```

<!-- End of combined VS/Mac tabs -->

---

下一個教學課程將示範如何建置 gRPC 用戶端，這是測試 Greeter 服務所必要的。

### <a name="examine-the-project-files-of-the-grpc-project"></a>檢查 gRPC 專案的專案檔

GrpcGreeter 檔案：

* greet.proto：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。 如需詳細資訊，請參閱<xref:grpc/index>。
* *Services* 資料夾：包含 `Greeter` 服務的實作。
* *appSettings.json*：包含設定資料，例如 Kestrel 所使用的通訊協定。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。
* *Program.cs*：包含 gRPC 服務的進入點。 如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。
* Startup.cs：包含設定應用程式行為的程式碼。 如需詳細資訊，請參閱<xref:fundamentals/startup>。

### <a name="test-the-service"></a>測試服務

## <a name="additional-resources"></a>其他資源

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 服務。
> * 執行 gRPC 服務。
> * 檢查專案檔。

> [!div class="step-by-step"]
> [下一步：建立 .NET Core gRPC 用戶端](xref:tutorials/grpc/grpc-client)