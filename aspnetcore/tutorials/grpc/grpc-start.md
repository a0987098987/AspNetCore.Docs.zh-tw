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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="5bd4b-104">教學課程：開始在 ASP.NET Core 中使用 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="5bd4b-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="5bd4b-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="5bd4b-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="5bd4b-106">本教學課程將教導在 ASP.NET Core 上建置 gRPC 服務的基本概念。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="5bd4b-107">最後，您將會有一個回應問候語的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="5bd4b-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5bd4b-109">建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="5bd4b-110">執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="5bd4b-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="5bd4b-112">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="5bd4b-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5bd4b-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5bd4b-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5bd4b-114">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="5bd4b-115">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="5bd4b-116">![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="5bd4b-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="5bd4b-117">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="5bd4b-118">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="5bd4b-119">![新增 ASP.NET Core Web 應用程式](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="5bd4b-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="5bd4b-120">從下拉式清單中選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="5bd4b-121">選擇 [gRPC 服務] 範本。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="5bd4b-122">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-122">The following starter project is created:</span></span>

  ![底下提供說明，包括方案總管](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5bd4b-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5bd4b-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5bd4b-125">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="5bd4b-126">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="5bd4b-127">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="5bd4b-128">`dotnet new` 命令會在 [GrpcGreeter] 資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="5bd4b-129">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="5bd4b-130">對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="5bd4b-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="5bd4b-131">選取 [是]</span><span class="sxs-lookup"><span data-stu-id="5bd4b-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="5bd4b-132">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5bd4b-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="5bd4b-133">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="5bd4b-134">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="5bd4b-135">開啟專案</span><span class="sxs-lookup"><span data-stu-id="5bd4b-135">Open the project</span></span>

<span data-ttu-id="5bd4b-136">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 [GrpcGreeter.sln] 檔案。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="5bd4b-137">執行服務</span><span class="sxs-lookup"><span data-stu-id="5bd4b-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5bd4b-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5bd4b-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5bd4b-139">按 Ctrl+F5 執行 gRPC 服務，而不啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="5bd4b-140">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="5bd4b-141">記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5bd4b-143">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5bd4b-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="5bd4b-144">使用 `dotnet run` 從命令列執行 gRPC 專案 GrpcGreeter。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="5bd4b-145">記錄會顯示服務已開始在 `http://localhost:50051` 進行接聽。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

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

<span data-ttu-id="5bd4b-146">下一個教學課程將示範如何建置 gRPC 用戶端，這是測試 Greeter 服務所必要的。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="5bd4b-147">檢查 gRPC 專案的專案檔</span><span class="sxs-lookup"><span data-stu-id="5bd4b-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="5bd4b-148">GrpcGreeter 檔案：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="5bd4b-149">greet.proto：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="5bd4b-150">如需詳細資訊，請參閱<xref:grpc/index>。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="5bd4b-151">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="5bd4b-152">*appSettings.json*：包含設定資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="5bd4b-153">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="5bd4b-154">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="5bd4b-155">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="5bd4b-156">Startup.cs：包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="5bd4b-157">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="5bd4b-158">測試服務</span><span class="sxs-lookup"><span data-stu-id="5bd4b-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5bd4b-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="5bd4b-159">Additional resources</span></span>

<span data-ttu-id="5bd4b-160">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="5bd4b-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5bd4b-161">建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="5bd4b-162">執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="5bd4b-163">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="5bd4b-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5bd4b-164">下一步：建立 .NET Core gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="5bd4b-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)