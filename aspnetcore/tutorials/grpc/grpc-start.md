---
title: 在 ASP.NET Core 中建立 .NET Core gRPC 用戶端與伺服器
author: juntaoluo
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
ms.author: johluo
ms.date: 04/08/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a4676803361d71a3199b2cd1232d0ced8c93db5f
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84451936"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="96c9d-104">教學課程：在 ASP.NET Core 中建立 gRPC 用戶端和伺服器</span><span class="sxs-lookup"><span data-stu-id="96c9d-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="96c9d-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="96c9d-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="96c9d-106">本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="96c9d-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="96c9d-107">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="96c9d-108">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="96c9d-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="96c9d-109">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="96c9d-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="96c9d-110">建立 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="96c9d-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="96c9d-111">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="96c9d-112">利用 gRPC Greeter 服務來測試 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="96c9d-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="96c9d-113">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="96c9d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="96c9d-117">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="96c9d-117">Create a gRPC service</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="96c9d-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96c9d-119">啟動 Visual Studio，然後選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="96c9d-120">或者，從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]\*\*\*\* > [專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="96c9d-121">在 [**建立新專案**] 對話方塊中，選取 [ **gRPC 服務**]，然後選取 **[下一步]**：</span><span class="sxs-lookup"><span data-stu-id="96c9d-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![[建立新專案] 對話方塊](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="96c9d-123">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="96c9d-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="96c9d-124">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="96c9d-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="96c9d-125">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="96c9d-125">Select **Create**.</span></span>
* <span data-ttu-id="96c9d-126">在 [建立新的 gRPC 服務]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="96c9d-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="96c9d-127">已選取 [gRPC 服務]\*\*\*\* 範本。</span><span class="sxs-lookup"><span data-stu-id="96c9d-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="96c9d-128">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="96c9d-128">Select **Create**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="96c9d-130">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="96c9d-131">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="96c9d-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="96c9d-132">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="96c9d-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="96c9d-133">`dotnet new` 命令會在 [GrpcGreeter]\*\* 資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="96c9d-134">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="96c9d-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="96c9d-135">此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' GrpcGreeter ' 中遺漏了 debug。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="96c9d-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="96c9d-136">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="96c9d-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96c9d-138">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="96c9d-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="96c9d-139">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="96c9d-140">開啟專案</span><span class="sxs-lookup"><span data-stu-id="96c9d-140">Open the project</span></span>

<span data-ttu-id="96c9d-141">從**Visual Studio 選取**[檔案]  >  [**開啟**]，然後選取*GrpcGreeter .csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="96c9d-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.csproj* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="96c9d-142">執行服務</span><span class="sxs-lookup"><span data-stu-id="96c9d-142">Run the service</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

<span data-ttu-id="96c9d-143">該記錄會顯示正在接聽 `https://localhost:5001` 的服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-143">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="96c9d-144">gRPC 範本已設為使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-144">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="96c9d-145">gRPC 用戶端需要使用 HTTPS 來呼叫伺服器。</span><span class="sxs-lookup"><span data-stu-id="96c9d-145">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="96c9d-146">macOS 不支援具有 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="96c9d-146">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="96c9d-147">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-147">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="96c9d-148">如需詳細資訊，請參閱[無法在 macOS 上啟動 ASP.NET Core gRPC 應用程式](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-148">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="96c9d-149">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="96c9d-149">Examine the project files</span></span>

<span data-ttu-id="96c9d-150">*GrpcGreeter* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="96c9d-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="96c9d-151">*歡迎. proto*： *Protos/歡迎*檔案 `Greeter` 會定義 gRPC，並用於產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="96c9d-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="96c9d-152">如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="96c9d-153">*Services*資料夾：包含服務的執行 `Greeter` 。</span><span class="sxs-lookup"><span data-stu-id="96c9d-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="96c9d-154">*appSettings*：包含設定資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="96c9d-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="96c9d-155">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。</span><span class="sxs-lookup"><span data-stu-id="96c9d-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="96c9d-156">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="96c9d-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="96c9d-157">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。</span><span class="sxs-lookup"><span data-stu-id="96c9d-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="96c9d-158">*Startup.cs*：包含可設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="96c9d-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="96c9d-159">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="96c9d-160">在 .NET 主控台應用程式中建立 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="96c9d-160">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="96c9d-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96c9d-162">開啟第二個 Visual Studio 執行個體，並選取 [建立新專案]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-162">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="96c9d-163">在 [建立新專案]\*\*\*\* 對話方塊中，選取 [主控台應用程式 (.NET Core)]\*\*\*\* 並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-163">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="96c9d-164">在 [**專案名稱**] 文字方塊中，輸入**GrpcGreeterClient** ，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="96c9d-164">In the **Project name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="96c9d-166">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-166">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="96c9d-167">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="96c9d-167">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="96c9d-168">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="96c9d-168">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-169">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="96c9d-170">請遵循[使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)中的指示，以建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="96c9d-170">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="96c9d-171">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="96c9d-171">Add required packages</span></span>

<span data-ttu-id="96c9d-172">gRPC 用戶端專案需要下列套件：</span><span class="sxs-lookup"><span data-stu-id="96c9d-172">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="96c9d-173">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)，包含 .NET Core 用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-173">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="96c9d-174">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="96c9d-174">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="96c9d-175">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="96c9d-175">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="96c9d-176">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="96c9d-176">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="96c9d-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="96c9d-178">使用套件管理員主控台 (PMC) 或管理 NuGet 套件來安裝套件。</span><span class="sxs-lookup"><span data-stu-id="96c9d-178">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="96c9d-179">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="96c9d-179">PMC option to install packages</span></span>

* <span data-ttu-id="96c9d-180">從 Visual Studio 選取 [**工具**] [  >  **NuGet 套件管理員**] [  >  **套件管理員主控台**]</span><span class="sxs-lookup"><span data-stu-id="96c9d-180">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="96c9d-181">從 [套件管理員主控台]\*\*\*\* 視窗，執行 `cd GrpcGreeterClient` 以變更包含 *GrpcGreeterClient.csproj* 檔案之資料夾的目錄。</span><span class="sxs-lookup"><span data-stu-id="96c9d-181">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="96c9d-182">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="96c9d-182">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="96c9d-183">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="96c9d-183">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="96c9d-184">以滑鼠右鍵按一下**方案總管**  >  **管理 NuGet 套件**] 中的專案</span><span class="sxs-lookup"><span data-stu-id="96c9d-184">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="96c9d-185">選取 [瀏覽]\*\*\*\* 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="96c9d-185">Select the **Browse** tab.</span></span>
* <span data-ttu-id="96c9d-186">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="96c9d-186">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="96c9d-187">從 [瀏覽]\*\*\*\* 索引標籤選取 [Grpc.Net.Client]\*\*\*\* 套件，然後選取 [安裝]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-187">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="96c9d-188">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="96c9d-188">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="96c9d-190">從 [整合式終端]\*\*\*\* 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="96c9d-190">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-191">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-191">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="96c9d-192">以滑鼠右鍵按一下**Packages** **Solution Pad**  >  **新增套件**] 中的 [套件] 資料夾</span><span class="sxs-lookup"><span data-stu-id="96c9d-192">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="96c9d-193">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="96c9d-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="96c9d-194">從結果窗格中選取 [Grpc.Net.Client]\*\*\*\* 套件，然後選取 [新增套件]\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="96c9d-194">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="96c9d-195">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="96c9d-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="96c9d-196">新增 greet.proto</span><span class="sxs-lookup"><span data-stu-id="96c9d-196">Add greet.proto</span></span>

* <span data-ttu-id="96c9d-197">在 gRPC 用戶端專案中建立 [Protos]\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="96c9d-197">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="96c9d-198">將 *Protos\greet.proto* 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="96c9d-198">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="96c9d-199">編輯 *GrpcGreeterClient.csproj* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="96c9d-199">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studio"></a>[<span data-ttu-id="96c9d-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-200">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="96c9d-201">以滑鼠右鍵按一下專案，然後選取 [編輯專案檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="96c9d-201">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="96c9d-203">選取 [GrpcGreeterClient.csproj]\*\* 檔案。</span><span class="sxs-lookup"><span data-stu-id="96c9d-203">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="96c9d-205">以滑鼠右鍵按一下專案，然後選取 [**工具**] [  >  **編輯**檔案]。</span><span class="sxs-lookup"><span data-stu-id="96c9d-205">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="96c9d-206">新增具有代表 *greet.proto* 檔案之 `<Protobuf>` 元素的項目群組：</span><span class="sxs-lookup"><span data-stu-id="96c9d-206">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="96c9d-207">建立 Greeter 用戶端</span><span class="sxs-lookup"><span data-stu-id="96c9d-207">Create the Greeter client</span></span>

<span data-ttu-id="96c9d-208">建置專案，以便在 `GrpcGreeter` 命名空間中建立類型。</span><span class="sxs-lookup"><span data-stu-id="96c9d-208">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="96c9d-209">`GrpcGreeter` 類型會自動由建置處理序產生。</span><span class="sxs-lookup"><span data-stu-id="96c9d-209">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="96c9d-210">利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="96c9d-210">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="96c9d-211">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="96c9d-211">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="96c9d-212">Greeter 用戶端建立者：</span><span class="sxs-lookup"><span data-stu-id="96c9d-212">The Greeter client is created by:</span></span>

* <span data-ttu-id="96c9d-213">具現化包含建立與 gRPC 服務連線資訊的 `GrpcChannel`。</span><span class="sxs-lookup"><span data-stu-id="96c9d-213">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="96c9d-214">使用 `GrpcChannel` 建構 Greeter 用戶端：</span><span class="sxs-lookup"><span data-stu-id="96c9d-214">Using the `GrpcChannel` to construct the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="96c9d-215">Greeter 用戶端會呼叫非同步的 `SayHello` 方法。</span><span class="sxs-lookup"><span data-stu-id="96c9d-215">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="96c9d-216">顯示 `SayHello` 呼叫的結果：</span><span class="sxs-lookup"><span data-stu-id="96c9d-216">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="96c9d-217">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="96c9d-217">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="96c9d-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c9d-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="96c9d-219">在 Greeter 服務中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="96c9d-219">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="96c9d-220">在 `GrpcGreeterClient` 專案中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-220">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="96c9d-221">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="96c9d-221">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="96c9d-222">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-222">Start the Greeter service.</span></span>
* <span data-ttu-id="96c9d-223">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-223">Start the client.</span></span>


# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="96c9d-224">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="96c9d-224">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="96c9d-225">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-225">Start the Greeter service.</span></span>
* <span data-ttu-id="96c9d-226">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="96c9d-226">Start the client.</span></span>

---

<span data-ttu-id="96c9d-227">用戶端會使用包含其名稱*GreeterClient*的訊息，將問候語傳送給服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-227">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="96c9d-228">該服務會傳送 "Hello GreeterClient" 訊息做為回應。</span><span class="sxs-lookup"><span data-stu-id="96c9d-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="96c9d-229">"Hello GreeterClient" 回應會顯示在命令提示字元中：</span><span class="sxs-lookup"><span data-stu-id="96c9d-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="96c9d-230">gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中：</span><span class="sxs-lookup"><span data-stu-id="96c9d-230">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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
> <span data-ttu-id="96c9d-231">此文章中的程式碼需要 ASP.NET Core HTTPS 開發憑證來保護 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="96c9d-231">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="96c9d-232">如果 .NET gRPC 用戶端因訊息 `The remote certificate is invalid according to the validation procedure.` 或而失敗 `The SSL connection could not be established.` ，則開發憑證不受信任。</span><span class="sxs-lookup"><span data-stu-id="96c9d-232">If the .NET gRPC client fails with the message `The remote certificate is invalid according to the validation procedure.` or `The SSL connection could not be established.`, the development certificate isn't trusted.</span></span> <span data-ttu-id="96c9d-233">若要修正此問題，請參閱[使用不受信任/不正確憑證呼叫 gRPC 服務](xref:grpc/troubleshoot#call-a-grpc-service-with-an-untrustedinvalid-certificate)。</span><span class="sxs-lookup"><span data-stu-id="96c9d-233">To fix this issue, see [Call a gRPC service with an untrusted/invalid certificate](xref:grpc/troubleshoot#call-a-grpc-service-with-an-untrustedinvalid-certificate).</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a><span data-ttu-id="96c9d-234">後續步驟</span><span class="sxs-lookup"><span data-stu-id="96c9d-234">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
