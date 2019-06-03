---
title: 在 ASP.NET Core 中建立 .NET Core gRPC 用戶端與伺服器
author: juntaoluo
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2b4325d2413e335a3061a7695def88a1b23ee52b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376376"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="f2699-104">教學課程：在 ASP.NET Core 中建立 gRPC 用戶端與伺服器</span><span class="sxs-lookup"><span data-stu-id="f2699-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="f2699-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="f2699-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="f2699-106">本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2699-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="f2699-107">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2699-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="f2699-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f2699-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f2699-109">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="f2699-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f2699-110">建立 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2699-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="f2699-111">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2699-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="f2699-112">利用 gRPC Greeter 服務來測試 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2699-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="f2699-113">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="f2699-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2699-115">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f2699-116">在 [建立新專案]  對話方塊中，選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2699-117">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="f2699-117">Select **Next**</span></span>
* <span data-ttu-id="f2699-118">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="f2699-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="f2699-119">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="f2699-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="f2699-120">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="f2699-120">Select **Create**</span></span>
* <span data-ttu-id="f2699-121">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f2699-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="f2699-122">從下拉式功能表中，選取 [.NET Core]  與 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="f2699-123">選取 [gRPC 服務]  範本。</span><span class="sxs-lookup"><span data-stu-id="f2699-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="f2699-124">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="f2699-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2699-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2699-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f2699-126">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f2699-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f2699-127">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2699-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f2699-128">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2699-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="f2699-129">`dotnet new` 命令會在 [GrpcGreeter]  資料夾中建立一個新的 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="f2699-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="f2699-130">`code` 命令會在新的 Visual Studio Code 執行個體中開啟 [GrpcGreeter]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2699-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="f2699-131">對話方塊隨即顯示，並指出 **'GrpcGreeter' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="f2699-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="f2699-132">選取 [是] </span><span class="sxs-lookup"><span data-stu-id="f2699-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2699-133">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f2699-134">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2699-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="f2699-135">上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="f2699-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="f2699-136">開啟專案</span><span class="sxs-lookup"><span data-stu-id="f2699-136">Open the project</span></span>

<span data-ttu-id="f2699-137">從 Visual Studio 中，選取 [檔案] > [開啟]  ，然後選取 [GrpcGreeter.sln]  檔案。</span><span class="sxs-lookup"><span data-stu-id="f2699-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="f2699-138">執行服務</span><span class="sxs-lookup"><span data-stu-id="f2699-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2699-140">按 Ctrl+F5 執行 gRPC 服務，而不啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="f2699-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="f2699-141">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="f2699-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f2699-142">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f2699-143">使用 `dotnet run` 從命令列執行 gRPC 專案 GrpcGreeter。</span><span class="sxs-lookup"><span data-stu-id="f2699-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f2699-144">該記錄會顯示正在接聽 `http://localhost:50051` 的服務。</span><span class="sxs-lookup"><span data-stu-id="f2699-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="f2699-145">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="f2699-145">Examine the project files</span></span>

<span data-ttu-id="f2699-146">GrpcGreeter 檔案：</span><span class="sxs-lookup"><span data-stu-id="f2699-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="f2699-147">*greet.proto*：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="f2699-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="f2699-148">如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。</span><span class="sxs-lookup"><span data-stu-id="f2699-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="f2699-149">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="f2699-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="f2699-150">*appSettings.json*：包含組態資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="f2699-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="f2699-151">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="f2699-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="f2699-152">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="f2699-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="f2699-153">如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。</span><span class="sxs-lookup"><span data-stu-id="f2699-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="f2699-154">*Startup.cs*：包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f2699-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="f2699-155">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="f2699-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="f2699-156">在 .NET 主控台應用程式中建立 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="f2699-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2699-158">從功能表列中選取 [檔案]   >  [新增]   >  [專案]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="f2699-159">在 [建立新專案]  對話方塊中，選取 [主控台應用程式 (.NET Core)]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="f2699-160">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="f2699-160">Select **Next**</span></span>
* <span data-ttu-id="f2699-161">在 [名稱]  文字方塊中，輸入 "GrpcGreeterClient"。</span><span class="sxs-lookup"><span data-stu-id="f2699-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="f2699-162">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2699-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2699-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f2699-164">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f2699-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f2699-165">將目錄 (`cd`) 變更為其中包含專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2699-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="f2699-166">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2699-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2699-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f2699-168">請遵循[此處](/dotnet/core/tutorials/using-on-mac-vs-full-solution)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2699-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="f2699-169">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="f2699-169">Add required packages</span></span>

<span data-ttu-id="f2699-170">將下列套件新增至 gRPC 用戶端專案：</span><span class="sxs-lookup"><span data-stu-id="f2699-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="f2699-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core)，包含 C 核心用戶端的 C# API。</span><span class="sxs-lookup"><span data-stu-id="f2699-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="f2699-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="f2699-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="f2699-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="f2699-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="f2699-174">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="f2699-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2699-176">使用套件管理員主控台 (PMC) 或 管理 NuGet 套件來安裝套件</span><span class="sxs-lookup"><span data-stu-id="f2699-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

####  <a name="pmc-option-to-install-packages"></a><span data-ttu-id="f2699-177">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="f2699-177">PMC option to install packages</span></span>

* <span data-ttu-id="f2699-178">從 Visual Studio 選取 [工具]   > [NuGet 套件管理員]   > [套件管理員主控台] </span><span class="sxs-lookup"><span data-stu-id="f2699-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="f2699-179">從 [套件管理員主控台]  視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="f2699-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="f2699-180">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2699-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="f2699-181">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="f2699-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="f2699-182">在 [方案總管]   > [管理 NuGet 套件]  中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="f2699-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="f2699-183">選取 [瀏覽]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f2699-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="f2699-184">在搜尋方塊中輸入 **Grpc.Core**。</span><span class="sxs-lookup"><span data-stu-id="f2699-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="f2699-185">從 [瀏覽]  索引標籤選取 [Grpc.Core]  套件，然後選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="f2699-186">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="f2699-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2699-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2699-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f2699-188">從 [整合式終端]  執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2699-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2699-189">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f2699-190">在 [Solution Pad]   > [新增套件]  中，以滑鼠右鍵按一下 [套件]  資料夾</span><span class="sxs-lookup"><span data-stu-id="f2699-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="f2699-191">在搜尋方塊中輸入 **Grpc.Core**。</span><span class="sxs-lookup"><span data-stu-id="f2699-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="f2699-192">從結果窗格中選取 [Grpc.Core]  套件，然後選取 [新增套件] </span><span class="sxs-lookup"><span data-stu-id="f2699-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="f2699-193">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="f2699-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="f2699-194">新增 greet.proto</span><span class="sxs-lookup"><span data-stu-id="f2699-194">Add greet.proto</span></span>

* <span data-ttu-id="f2699-195">在 gRPC 用戶端專案中建立 [Protos]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2699-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="f2699-196">將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="f2699-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="f2699-197">編輯 *GrpcGreeterClient.csproj* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="f2699-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="f2699-199">以滑鼠右鍵按一下該專案，然後選取 [編輯 GrpcGreeterClient.csproj]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f2699-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f2699-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="f2699-201">選取 [GrpcGreeterClient.csproj]  檔案。</span><span class="sxs-lookup"><span data-stu-id="f2699-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f2699-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="f2699-203">以滑鼠右鍵按一下專案，然後選取 [工具] > [編輯檔案]  。</span><span class="sxs-lookup"><span data-stu-id="f2699-203">Right click the project and select **Tools > Edit File**.</span></span>

  ------

* <span data-ttu-id="f2699-204">將 **greet.proto** 檔案新增至 GrpcGreeterClient 專案檔的 `<Protobuf>` 項目群組：</span><span class="sxs-lookup"><span data-stu-id="f2699-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="f2699-205">建置用戶端專案以觸發 C# 用戶端資產的產生。</span><span class="sxs-lookup"><span data-stu-id="f2699-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greater-client"></a><span data-ttu-id="f2699-206">建立更大的用戶端</span><span class="sxs-lookup"><span data-stu-id="f2699-206">Create the greater client</span></span>

<span data-ttu-id="f2699-207">建置專案，以在 [Greeter] 命名空間中建立類型  。</span><span class="sxs-lookup"><span data-stu-id="f2699-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="f2699-208">`Greeter` 類型會自動由建置處理序產生。</span><span class="sxs-lookup"><span data-stu-id="f2699-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="f2699-209">利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f2699-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="f2699-210">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="f2699-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="f2699-211">以下項目會建立更大的用戶端：</span><span class="sxs-lookup"><span data-stu-id="f2699-211">The greater client is created by:</span></span>

* <span data-ttu-id="f2699-212">具現化包含建立與 gRPC 服務連線資訊的 `Channel`。</span><span class="sxs-lookup"><span data-stu-id="f2699-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="f2699-213">使用 `Channel` 建構更大的用戶端：</span><span class="sxs-lookup"><span data-stu-id="f2699-213">Using the `Channel` to construct the greater client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="f2699-214">更大的用戶端會呼叫非同步 `SayHello` 方法。</span><span class="sxs-lookup"><span data-stu-id="f2699-214">The greater client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="f2699-215">顯示 `SayHello` 呼叫的結果：</span><span class="sxs-lookup"><span data-stu-id="f2699-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="f2699-216">當作業完成釋放所有資源時，關閉用戶端所使用的 `Channel`。</span><span class="sxs-lookup"><span data-stu-id="f2699-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="f2699-217">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="f2699-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2699-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2699-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2699-219">在 Greeter 服務中按下 Ctrl + F5 鍵，在不進行偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2699-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="f2699-220">在 GrpcGreeterClient 專案中按下 Ctrl + F5 鍵，在不進行偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2699-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f2699-221">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f2699-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f2699-222">啟動 Greeter 服務。</span><span class="sxs-lookup"><span data-stu-id="f2699-222">Start the Greeter service.</span></span>
* <span data-ttu-id="f2699-223">啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="f2699-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="f2699-224">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="f2699-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="f2699-225">該服務會傳送 "Hello GreeterClient" 訊息做為回應。</span><span class="sxs-lookup"><span data-stu-id="f2699-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="f2699-226">"Hello GreeterClient" 回應會顯示在命令提示字元中：</span><span class="sxs-lookup"><span data-stu-id="f2699-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="f2699-227">gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="f2699-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="f2699-228">後續步驟</span><span class="sxs-lookup"><span data-stu-id="f2699-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
