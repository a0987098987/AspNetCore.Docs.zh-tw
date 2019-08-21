---
title: 在 ASP.NET Core 中建立 .NET Core gRPC 用戶端與伺服器
author: juntaoluo
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 496f659bd51e2404a936bea8aad77e674e1a285d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022498"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="961e5-104">教學課程：在 ASP.NET Core 中建立 gRPC 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="961e5-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="961e5-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="961e5-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="961e5-106">本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="961e5-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="961e5-107">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="961e5-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="961e5-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="961e5-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="961e5-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="961e5-110">建立 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="961e5-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="961e5-111">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="961e5-112">利用 gRPC Greeter 服務來測試 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="961e5-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="961e5-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="961e5-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="961e5-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="961e5-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="961e5-117">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="961e5-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="961e5-119">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="961e5-120">在 [建立新專案]  對話方塊中，選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="961e5-121">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="961e5-121">Select **Next**</span></span>
* <span data-ttu-id="961e5-122">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="961e5-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="961e5-123">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="961e5-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="961e5-124">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="961e5-124">Select **Create**</span></span>
* <span data-ttu-id="961e5-125">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="961e5-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="961e5-126">從下拉式功能表中，選取 [.NET Core]  與 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="961e5-127">選取 [gRPC 服務]  範本。</span><span class="sxs-lookup"><span data-stu-id="961e5-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="961e5-128">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="961e5-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="961e5-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="961e5-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="961e5-130">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="961e5-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="961e5-131">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="961e5-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="961e5-132">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="961e5-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="961e5-133">`dotnet new` 命令會在 [GrpcGreeter]  資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="961e5-134">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="961e5-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="961e5-135">對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="961e5-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="961e5-136">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="961e5-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="961e5-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="961e5-138">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="961e5-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="961e5-139">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="961e5-140">開啟專案</span><span class="sxs-lookup"><span data-stu-id="961e5-140">Open the project</span></span>

<span data-ttu-id="961e5-141">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 [GrpcGreeter.sln]  檔案。</span><span class="sxs-lookup"><span data-stu-id="961e5-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="961e5-142">執行服務</span><span class="sxs-lookup"><span data-stu-id="961e5-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="961e5-144">按 `Ctrl+F5` 執行 gRPC 服務，但不啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="961e5-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="961e5-145">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="961e5-146">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="961e5-147">使用 `dotnet run` 從命令列執行 gRPC Greeter 專案 *GrpcGreeter*。</span><span class="sxs-lookup"><span data-stu-id="961e5-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="961e5-148">該記錄會顯示正在接聽 `https://localhost:5001` 的服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="961e5-149">gRPC 範本已設為使用 [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) (傳輸層安全性 (TLS))。</span><span class="sxs-lookup"><span data-stu-id="961e5-149">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="961e5-150">gRPC 用戶端需要使用 HTTPS 來呼叫伺服器。</span><span class="sxs-lookup"><span data-stu-id="961e5-150">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="961e5-151">macOS 不支援具備 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="961e5-151">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="961e5-152">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-152">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="961e5-153">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="961e5-153">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="961e5-154">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="961e5-154">Examine the project files</span></span>

<span data-ttu-id="961e5-155">*GrpcGreeter* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="961e5-155">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="961e5-156">*greet.proto*：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="961e5-156">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="961e5-157">如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。</span><span class="sxs-lookup"><span data-stu-id="961e5-157">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="961e5-158">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="961e5-158">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="961e5-159">*appSettings.json*：包含組態資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="961e5-159">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="961e5-160">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="961e5-160">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="961e5-161">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="961e5-161">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="961e5-162">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="961e5-162">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="961e5-163">*Startup.cs*：包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="961e5-163">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="961e5-164">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="961e5-164">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="961e5-165">在 .NET 主控台應用程式中建立 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="961e5-165">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-166">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="961e5-167">開啟 Visual Studio 的第二個執行個體。</span><span class="sxs-lookup"><span data-stu-id="961e5-167">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="961e5-168">從功能表列中選取 [檔案]   >  [新增]   >  [專案]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-168">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="961e5-169">在 [建立新專案]  對話方塊中，選取 [主控台應用程式 (.NET Core)]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-169">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="961e5-170">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="961e5-170">Select **Next**</span></span>
* <span data-ttu-id="961e5-171">在 [名稱]  文字方塊中，輸入 "GrpcGreeterClient"。</span><span class="sxs-lookup"><span data-stu-id="961e5-171">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="961e5-172">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-172">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="961e5-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="961e5-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="961e5-174">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="961e5-174">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="961e5-175">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="961e5-175">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="961e5-176">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="961e5-176">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="961e5-177">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="961e5-178">請遵循[此處](/dotnet/core/tutorials/using-on-mac-vs-full-solution)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="961e5-178">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="961e5-179">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="961e5-179">Add required packages</span></span>

<span data-ttu-id="961e5-180">gRPC 用戶端專案需要下列套件：</span><span class="sxs-lookup"><span data-stu-id="961e5-180">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="961e5-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)，包含 .NET Core 用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="961e5-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="961e5-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="961e5-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="961e5-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="961e5-184">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="961e5-184">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-185">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="961e5-186">使用套件管理員主控台 (PMC) 或管理 NuGet 套件來安裝套件。</span><span class="sxs-lookup"><span data-stu-id="961e5-186">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="961e5-187">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="961e5-187">PMC option to install packages</span></span>

* <span data-ttu-id="961e5-188">從 Visual Studio 選取 [工具]   > [NuGet 套件管理員]   > [套件管理員主控台] </span><span class="sxs-lookup"><span data-stu-id="961e5-188">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="961e5-189">從 [套件管理員主控台]  視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="961e5-189">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="961e5-190">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="961e5-190">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="961e5-191">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="961e5-191">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="961e5-192">在 [方案總管]   > [管理 NuGet 套件]  中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="961e5-192">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="961e5-193">選取 [瀏覽]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="961e5-193">Select the **Browse** tab.</span></span>
* <span data-ttu-id="961e5-194">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="961e5-194">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="961e5-195">從 [瀏覽]  索引標籤選取 [Grpc.Net.Client]  套件，然後選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-195">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="961e5-196">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="961e5-196">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="961e5-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="961e5-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="961e5-198">從 [整合式終端]  執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="961e5-198">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="961e5-199">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="961e5-200">在 [Solution Pad]   > [新增套件]  中，以滑鼠右鍵按一下 [套件]  資料夾</span><span class="sxs-lookup"><span data-stu-id="961e5-200">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="961e5-201">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="961e5-201">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="961e5-202">從結果窗格中選取 [Grpc.Net.Client]  套件，然後選取 [新增套件] </span><span class="sxs-lookup"><span data-stu-id="961e5-202">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="961e5-203">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="961e5-203">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="961e5-204">新增 greet.proto</span><span class="sxs-lookup"><span data-stu-id="961e5-204">Add greet.proto</span></span>

* <span data-ttu-id="961e5-205">在 gRPC 用戶端專案中建立 [Protos]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="961e5-205">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="961e5-206">將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="961e5-206">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="961e5-207">編輯 *GrpcGreeterClient.csproj* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="961e5-207">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-208">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="961e5-209">以滑鼠右鍵按一下專案，然後選取 [編輯專案檔]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-209">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="961e5-210">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="961e5-210">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="961e5-211">選取 [GrpcGreeterClient.csproj]  檔案。</span><span class="sxs-lookup"><span data-stu-id="961e5-211">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="961e5-212">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="961e5-213">以滑鼠右鍵按一下專案，然後選取 [工具] > [編輯檔案]  。</span><span class="sxs-lookup"><span data-stu-id="961e5-213">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="961e5-214">新增具有代表 **greet.proto** 檔案之 `<Protobuf>` 元素的項目群組：</span><span class="sxs-lookup"><span data-stu-id="961e5-214">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="961e5-215">建立 Greeter 用戶端</span><span class="sxs-lookup"><span data-stu-id="961e5-215">Create the Greeter client</span></span>

<span data-ttu-id="961e5-216">建置專案，以便在 `GrpcGreeter` 命名空間中建立類型。</span><span class="sxs-lookup"><span data-stu-id="961e5-216">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="961e5-217">`GrpcGreeter` 類型會自動由建置處理序產生。</span><span class="sxs-lookup"><span data-stu-id="961e5-217">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="961e5-218">利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="961e5-218">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="961e5-219">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="961e5-219">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="961e5-220">Greeter 用戶端建立者：</span><span class="sxs-lookup"><span data-stu-id="961e5-220">The Greeter client is created by:</span></span>

* <span data-ttu-id="961e5-221">具現化包含建立 gRPC 服務連線資訊的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="961e5-221">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="961e5-222">使用 `HttpClient` 建構 Greeter 用戶端：</span><span class="sxs-lookup"><span data-stu-id="961e5-222">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="961e5-223">Greeter 用戶端會呼叫非同步的 `SayHello` 方法。</span><span class="sxs-lookup"><span data-stu-id="961e5-223">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="961e5-224">顯示 `SayHello` 呼叫的結果：</span><span class="sxs-lookup"><span data-stu-id="961e5-224">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="961e5-225">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="961e5-225">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="961e5-226">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="961e5-226">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="961e5-227">在 Greeter 服務中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="961e5-227">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="961e5-228">在 `GrpcGreeterClient` 專案中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-228">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="961e5-229">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="961e5-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="961e5-230">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="961e5-230">Start the Greeter service.</span></span>
* <span data-ttu-id="961e5-231">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="961e5-231">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="961e5-232">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="961e5-232">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="961e5-233">該服務會傳送 "Hello GreeterClient" 訊息做為回應。</span><span class="sxs-lookup"><span data-stu-id="961e5-233">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="961e5-234">"Hello GreeterClient" 回應會顯示在命令提示字元中：</span><span class="sxs-lookup"><span data-stu-id="961e5-234">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="961e5-235">gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="961e5-235">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="961e5-236">後續步驟</span><span class="sxs-lookup"><span data-stu-id="961e5-236">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
