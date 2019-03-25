---
title: 教學課程：開始在 ASP.NET Core 中使用 gRPC
author: juntaoluo
description: 這一系列的教學課程會示範如何在 ASP.NET Core 上建立 gRPC 服務。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流處理呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320052"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>教學課程：開始在 ASP.NET Core 中使用 gRPC 服務

作者：[John Luo](https://github.com/juntaoluo)

本教學課程將教導在 ASP.NET Core 上建置 gRPC 服務的基本概念。

最後，您將會有一個回應問候語的 gRPC 服務。

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 服務。
> * 執行服務。
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

### <a name="test-the-service"></a>測試服務

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 確定 **GrpcGreeter.Server** 已設定為「啟始專案」，然後按 Ctrl+F5 來執行服務，但不執行偵錯工具。

  Visual Studio 會在命令提示字元中執行服務。 記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_start.png)

* 在服務開始執行之後，將 **GrpcGreeter.Client** 設定為「啟始專案」，然後按 Ctrl+F5 來執行用戶端，但不執行偵錯工具。

  用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。 服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/client.png)

  服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 使用 `dotnet run` 從命令列執行伺服器專案 GrpcGreeter.Server。 記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* 使用 `dotnet run` 從個別的命令列執行用戶端專案 GrpcGreeter.Client。

用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。 服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>檢查 gRPC 專案的專案檔

GrpcGreeter.Server 檔案：

* greet.proto：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。 如需詳細資訊，請參閱<xref:grpc/index>。
* *Services* 資料夾：包含 `Greeter` 服務的實作。
* *appSettings.json*：包含設定資料，例如 Kestrel 所使用的通訊協定。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。
* *Program.cs*：包含 gRPC 服務的進入點。 如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。
* Startup.cs

包含設定應用程式行為的程式碼。 如需詳細資訊，請參閱<xref:fundamentals/startup>。

gRPC 用戶端 GrpcGreeter.Client 檔案：

*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 gRPC 服務。
> * 執行服務和用戶端來測試服務。
> * 檢查專案檔。
