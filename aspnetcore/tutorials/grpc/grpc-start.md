---
title: 在 ASP.NET Core 中建立 .NET Core gRPC 用戶端與伺服器
author: juntaoluo
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8c0f57e69d4cdee5b5f5510d7db04991ed6df475
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250831"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="d3578-104">教學課程：在 ASP.NET Core 中建立 gRPC 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="d3578-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="d3578-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="d3578-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="d3578-106">本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d3578-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="d3578-107">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="d3578-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d3578-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="d3578-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="d3578-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d3578-110">建立 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="d3578-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="d3578-111">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="d3578-112">利用 gRPC Greeter 服務來測試 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3578-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="d3578-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="d3578-117">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="d3578-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3578-119">啟動 Visual Studio 並選取 [建立新專案]。</span><span class="sxs-lookup"><span data-stu-id="d3578-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="d3578-120">或者，從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="d3578-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d3578-121">在 [**建立新專案**] 對話方塊中，選取 [ **gRPC 服務**]，然後選取 **[下一步]** ：</span><span class="sxs-lookup"><span data-stu-id="d3578-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![[建立新專案] 對話方塊](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="d3578-123">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="d3578-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="d3578-124">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="d3578-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="d3578-125">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="d3578-125">Select **Create**.</span></span>
* <span data-ttu-id="d3578-126">在 [建立新的 gRPC 服務] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="d3578-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="d3578-127">已選取 [gRPC 服務] 範本。</span><span class="sxs-lookup"><span data-stu-id="d3578-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="d3578-128">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="d3578-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d3578-130">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="d3578-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d3578-131">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d3578-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d3578-132">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d3578-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="d3578-133">`dotnet new` 命令會在 [GrpcGreeter] 資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="d3578-134">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d3578-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="d3578-135">對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="d3578-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="d3578-136">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="d3578-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d3578-138">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d3578-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="d3578-139">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="d3578-140">開啟專案</span><span class="sxs-lookup"><span data-stu-id="d3578-140">Open the project</span></span>

<span data-ttu-id="d3578-141">從 Visual Studio，選取 [檔案] > [開啟]，然後選取 [GrpcGreeter.sln] 檔案。</span><span class="sxs-lookup"><span data-stu-id="d3578-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="d3578-142">執行服務</span><span class="sxs-lookup"><span data-stu-id="d3578-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3578-144">按 `Ctrl+F5` 執行 gRPC 服務，但不啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="d3578-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="d3578-145">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d3578-147">使用 `dotnet run` 從命令列執行 gRPC Greeter 專案 *GrpcGreeter*。</span><span class="sxs-lookup"><span data-stu-id="d3578-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d3578-149">使用 `dotnet run` 從命令列執行 gRPC Greeter 專案 *GrpcGreeter*。</span><span class="sxs-lookup"><span data-stu-id="d3578-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="d3578-150">該記錄會顯示正在接聽 `https://localhost:5001` 的服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="d3578-151">gRPC 範本已設為使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)。</span><span class="sxs-lookup"><span data-stu-id="d3578-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="d3578-152">gRPC 用戶端需要使用 HTTPS 來呼叫伺服器。</span><span class="sxs-lookup"><span data-stu-id="d3578-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="d3578-153">macOS 不支援具有 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="d3578-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="d3578-154">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="d3578-155">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="d3578-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="d3578-156">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="d3578-156">Examine the project files</span></span>

<span data-ttu-id="d3578-157">*GrpcGreeter* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="d3578-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="d3578-158">*greet.proto* &ndash; *Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="d3578-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="d3578-159">如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。</span><span class="sxs-lookup"><span data-stu-id="d3578-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="d3578-160">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="d3578-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="d3578-161">*appSettings.json* &ndash; 包含設定資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d3578-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="d3578-162">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="d3578-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="d3578-163">*Program.cs* &ndash; 包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="d3578-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="d3578-164">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="d3578-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="d3578-165">*Startup.cs* &ndash; 包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="d3578-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="d3578-166">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="d3578-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="d3578-167">在 .NET 主控台應用程式中建立 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="d3578-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3578-169">開啟第二個 Visual Studio 執行個體，並選取 [建立新專案]。</span><span class="sxs-lookup"><span data-stu-id="d3578-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="d3578-170">在 [建立新專案] 對話方塊中，選取 [主控台應用程式 (.NET Core)] 並選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="d3578-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="d3578-171">在 [名稱] 文字方塊中，輸入 **GrpcGreeterClient**，並選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="d3578-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d3578-173">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="d3578-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="d3578-174">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="d3578-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="d3578-175">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d3578-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-176">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="d3578-177">請遵循[使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)中的指示，以建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="d3578-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="d3578-178">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="d3578-178">Add required packages</span></span>

<span data-ttu-id="d3578-179">gRPC 用戶端專案需要下列套件：</span><span class="sxs-lookup"><span data-stu-id="d3578-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="d3578-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)，包含 .NET Core 用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="d3578-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="d3578-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="d3578-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="d3578-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="d3578-183">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="d3578-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d3578-185">使用套件管理員主控台 (PMC) 或管理 NuGet 套件來安裝套件。</span><span class="sxs-lookup"><span data-stu-id="d3578-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="d3578-186">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="d3578-186">PMC option to install packages</span></span>

* <span data-ttu-id="d3578-187">從 Visual Studio 選取 [工具] > [NuGet 套件管理員] > [套件管理員主控台]</span><span class="sxs-lookup"><span data-stu-id="d3578-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="d3578-188">從 [套件管理員主控台] 視窗，執行 `cd GrpcGreeterClient` 以變更包含 *GrpcGreeterClient.csproj* 檔案之資料夾的目錄。</span><span class="sxs-lookup"><span data-stu-id="d3578-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="d3578-189">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d3578-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="d3578-190">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="d3578-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="d3578-191">在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="d3578-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="d3578-192">選取 [瀏覽] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d3578-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="d3578-193">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="d3578-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="d3578-194">從 [瀏覽] 索引標籤選取 [Grpc.Net.Client] 套件，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="d3578-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="d3578-195">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="d3578-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="d3578-197">從 [整合式終端] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d3578-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-198">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d3578-199">在 [Solution Pad] > [新增套件] 中，以滑鼠右鍵按一下 [套件] 資料夾</span><span class="sxs-lookup"><span data-stu-id="d3578-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="d3578-200">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="d3578-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="d3578-201">從結果窗格中選取 [Grpc.Net.Client] 套件，然後選取 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="d3578-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="d3578-202">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="d3578-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="d3578-203">新增 greet.proto</span><span class="sxs-lookup"><span data-stu-id="d3578-203">Add greet.proto</span></span>

* <span data-ttu-id="d3578-204">在 gRPC 用戶端專案中建立 [Protos] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d3578-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="d3578-205">將 *Protos\greet.proto* 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="d3578-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="d3578-206">編輯 *GrpcGreeterClient.csproj* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="d3578-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="d3578-208">以滑鼠右鍵按一下專案，然後選取 [編輯專案檔]。</span><span class="sxs-lookup"><span data-stu-id="d3578-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="d3578-210">選取 [GrpcGreeterClient.csproj] 檔案。</span><span class="sxs-lookup"><span data-stu-id="d3578-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-211">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="d3578-212">以滑鼠右鍵按一下專案，然後選取 [工具] > [編輯檔案]。</span><span class="sxs-lookup"><span data-stu-id="d3578-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="d3578-213">新增具有代表 *greet.proto* 檔案之 `<Protobuf>` 元素的項目群組：</span><span class="sxs-lookup"><span data-stu-id="d3578-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="d3578-214">建立 Greeter 用戶端</span><span class="sxs-lookup"><span data-stu-id="d3578-214">Create the Greeter client</span></span>

<span data-ttu-id="d3578-215">建置專案，以便在 `GrpcGreeter` 命名空間中建立類型。</span><span class="sxs-lookup"><span data-stu-id="d3578-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="d3578-216">`GrpcGreeter` 類型會自動由建置處理序產生。</span><span class="sxs-lookup"><span data-stu-id="d3578-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="d3578-217">利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d3578-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="d3578-218">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="d3578-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="d3578-219">Greeter 用戶端建立者：</span><span class="sxs-lookup"><span data-stu-id="d3578-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="d3578-220">具現化包含建立 gRPC 服務連線資訊的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="d3578-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="d3578-221">`HttpClient`使用來建立 gRPC 通道和 Greeter 用戶端：</span><span class="sxs-lookup"><span data-stu-id="d3578-221">Using the `HttpClient` to construct a gRPC channel and the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="d3578-222">Greeter 用戶端會呼叫非同步的 `SayHello` 方法。</span><span class="sxs-lookup"><span data-stu-id="d3578-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="d3578-223">顯示 `SayHello` 呼叫的結果：</span><span class="sxs-lookup"><span data-stu-id="d3578-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="d3578-224">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="d3578-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3578-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3578-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3578-226">在 Greeter 服務中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="d3578-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="d3578-227">在 `GrpcGreeterClient` 專案中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d3578-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d3578-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d3578-229">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-229">Start the Greeter service.</span></span>
* <span data-ttu-id="d3578-230">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d3578-231">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="d3578-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d3578-232">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-232">Start the Greeter service.</span></span>
* <span data-ttu-id="d3578-233">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="d3578-233">Start the client.</span></span>

---

<span data-ttu-id="d3578-234">用戶端會以包含其名稱 *GreeterClient* 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="d3578-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="d3578-235">該服務會傳送 "Hello GreeterClient" 訊息做為回應。</span><span class="sxs-lookup"><span data-stu-id="d3578-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="d3578-236">"Hello GreeterClient" 回應會顯示在命令提示字元中：</span><span class="sxs-lookup"><span data-stu-id="d3578-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="d3578-237">gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中：</span><span class="sxs-lookup"><span data-stu-id="d3578-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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

> [!NOTE]
> <span data-ttu-id="d3578-238">此文章中的程式碼需要 ASP.NET Core HTTPS 開發憑證來保護 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="d3578-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="d3578-239">如果用戶端失敗並顯示訊息 `The remote certificate is invalid according to the validation procedure.`，則開發憑證不受信任。</span><span class="sxs-lookup"><span data-stu-id="d3578-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="d3578-240">如需修正此問題的指示，請參閱[信任 Windows 和 macOS上的 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)。</span><span class="sxs-lookup"><span data-stu-id="d3578-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

### <a name="next-steps"></a><span data-ttu-id="d3578-241">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d3578-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
