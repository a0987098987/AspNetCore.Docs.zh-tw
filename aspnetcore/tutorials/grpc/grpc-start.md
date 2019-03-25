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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="b4155-104">教學課程：開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="b4155-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="b4155-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="b4155-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b4155-106">本教學課程將教導在 ASP.NET Core 上建置 gRPC 服務的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b4155-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="b4155-107">最後，您將會有一個回應問候語的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="b4155-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="b4155-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b4155-109">建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="b4155-110">執行服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-110">Run the service.</span></span>
> * <span data-ttu-id="b4155-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="b4155-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="b4155-112">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="b4155-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4155-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4155-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b4155-114">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="b4155-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b4155-115">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4155-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="b4155-116">![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="b4155-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="b4155-117">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="b4155-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="b4155-118">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="b4155-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="b4155-119">![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="b4155-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="b4155-120">從下拉式清單中選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="b4155-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="b4155-121">選擇 [gRPC 服務] 範本。</span><span class="sxs-lookup"><span data-stu-id="b4155-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="b4155-122">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="b4155-122">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b4155-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b4155-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b4155-125">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="b4155-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b4155-126">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4155-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b4155-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b4155-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="b4155-128">`dotnet new` 命令會在 [GrpcGreeter] 資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="b4155-129">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4155-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="b4155-130">對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="b4155-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="b4155-131">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="b4155-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b4155-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b4155-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b4155-133">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b4155-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="b4155-134">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="b4155-135">開啟專案</span><span class="sxs-lookup"><span data-stu-id="b4155-135">Open the project</span></span>

<span data-ttu-id="b4155-136">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 [GrpcGreeter.sln] 檔案。</span><span class="sxs-lookup"><span data-stu-id="b4155-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="b4155-137">測試服務</span><span class="sxs-lookup"><span data-stu-id="b4155-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b4155-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b4155-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b4155-139">確定 **GrpcGreeter.Server** 已設定為「啟始專案」，然後按 Ctrl+F5 來執行服務，但不執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b4155-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="b4155-140">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="b4155-141">記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。</span><span class="sxs-lookup"><span data-stu-id="b4155-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_start.png)

* <span data-ttu-id="b4155-143">在服務開始執行之後，將 **GrpcGreeter.Client** 設定為「啟始專案」，然後按 Ctrl+F5 來執行用戶端，但不執行偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b4155-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="b4155-144">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="b4155-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b4155-145">服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。</span><span class="sxs-lookup"><span data-stu-id="b4155-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/client.png)

  <span data-ttu-id="b4155-147">服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="b4155-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b4155-149">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b4155-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b4155-150">使用 `dotnet run` 從命令列執行伺服器專案 GrpcGreeter.Server。</span><span class="sxs-lookup"><span data-stu-id="b4155-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="b4155-151">記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。</span><span class="sxs-lookup"><span data-stu-id="b4155-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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

* <span data-ttu-id="b4155-152">使用 `dotnet run` 從個別的命令列執行用戶端專案 GrpcGreeter.Client。</span><span class="sxs-lookup"><span data-stu-id="b4155-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="b4155-153">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="b4155-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b4155-154">服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。</span><span class="sxs-lookup"><span data-stu-id="b4155-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="b4155-155">服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="b4155-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="b4155-156">檢查 gRPC 專案的專案檔</span><span class="sxs-lookup"><span data-stu-id="b4155-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="b4155-157">GrpcGreeter.Server 檔案：</span><span class="sxs-lookup"><span data-stu-id="b4155-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="b4155-158">greet.proto：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="b4155-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="b4155-159">如需詳細資訊，請參閱<xref:grpc/index>。</span><span class="sxs-lookup"><span data-stu-id="b4155-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="b4155-160">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="b4155-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="b4155-161">*appSettings.json*：包含設定資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="b4155-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="b4155-162">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="b4155-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="b4155-163">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="b4155-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="b4155-164">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="b4155-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="b4155-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b4155-165">Startup.cs</span></span>

<span data-ttu-id="b4155-166">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b4155-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="b4155-167">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="b4155-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="b4155-168">gRPC 用戶端 GrpcGreeter.Client 檔案：</span><span class="sxs-lookup"><span data-stu-id="b4155-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="b4155-169">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="b4155-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="b4155-170">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="b4155-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b4155-171">建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="b4155-172">執行服務和用戶端來測試服務。</span><span class="sxs-lookup"><span data-stu-id="b4155-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="b4155-173">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="b4155-173">Examined the project files.</span></span>
