---
title: 教學課程：建立 .NET Core gRPC 用戶端
author: juntaoluo
description: 這一系列的教學課程會示範如何在 ASP.NET Core 上建立 gRPC 服務。 了解如何建立 gRPC 服務專案、編輯通訊協定檔案，以及新增雙工資料流呼叫。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674130"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="9d3eb-104">教學課程：建立 .NET Core gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="9d3eb-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="9d3eb-105">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="9d3eb-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="9d3eb-106">本教學課程示範如何建立可與 gRPC 服務通訊的 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="9d3eb-107">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="9d3eb-108">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9d3eb-109">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="9d3eb-110">對上一個教學課程中建立的 gRPC Greeter 服務執行該服務。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="9d3eb-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="9d3eb-112">建立 .NET 主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="9d3eb-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d3eb-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d3eb-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9d3eb-114">請遵循[此處](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-114">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d3eb-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d3eb-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9d3eb-116">請遵循[此處](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-116">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d3eb-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9d3eb-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9d3eb-118">請遵循[此處](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution)的指示，建立名為 *GrpcGreeterClient* 的主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-118">Follow the instructions [here](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="9d3eb-119">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="9d3eb-119">Add required packages</span></span>

<span data-ttu-id="9d3eb-120">將下列套件新增至 gRPC 用戶端專案：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="9d3eb-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core)，包含 C 核心用戶端的 C# API。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="9d3eb-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="9d3eb-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="9d3eb-124">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="9d3eb-125">可使用下列方法新增套件：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d3eb-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d3eb-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="9d3eb-127">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="9d3eb-127">PMC option to install packages</span></span>

* <span data-ttu-id="9d3eb-128">從 Visual Studio 選取 [工具] > [NuGet 套件管理員] > [套件管理員主控台]</span><span class="sxs-lookup"><span data-stu-id="9d3eb-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="9d3eb-129">從 [套件管理員主控台] 視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="9d3eb-130">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="9d3eb-131">為 Google.Protobuf 和 Grpc.Tools 重複進行 `Install-Package`</span><span class="sxs-lookup"><span data-stu-id="9d3eb-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="9d3eb-132">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="9d3eb-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="9d3eb-133">在 [方案總管] > [管理 NuGet 套件] 中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="9d3eb-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="9d3eb-134">將 [套件來源] 設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="9d3eb-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="9d3eb-135">在搜尋方塊中輸入 "Grpc.Core"</span><span class="sxs-lookup"><span data-stu-id="9d3eb-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="9d3eb-136">從 [瀏覽] 索引標籤中選取 "Grpc.Core" 套件，並按一下 [安裝]</span><span class="sxs-lookup"><span data-stu-id="9d3eb-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="9d3eb-137">為 Google.Protobuf 和 Grpc.Tools 重複進行</span><span class="sxs-lookup"><span data-stu-id="9d3eb-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9d3eb-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9d3eb-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9d3eb-139">從 [整合式終端機] 執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Grpc.Core
```

<span data-ttu-id="9d3eb-140">為 Google.Protobuf 和 Grpc.Tools 重複進行</span><span class="sxs-lookup"><span data-stu-id="9d3eb-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9d3eb-141">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9d3eb-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9d3eb-142">在 [Solution Pad] > [新增套件...] 中，以滑鼠右鍵按一下 *Packages* 資料夾</span><span class="sxs-lookup"><span data-stu-id="9d3eb-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="9d3eb-143">將 [新增套件] 視窗的 [來源] 下拉式清單設定為 "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="9d3eb-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="9d3eb-144">在搜尋方塊中輸入 "Grpc.Core"</span><span class="sxs-lookup"><span data-stu-id="9d3eb-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="9d3eb-145">從結果窗格中選取 "Grpc.Core" 套件，並按一下 [新增套件]</span><span class="sxs-lookup"><span data-stu-id="9d3eb-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="9d3eb-146">為 Google.Protobuf 和 Grpc.Tools 重複進行</span><span class="sxs-lookup"><span data-stu-id="9d3eb-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="9d3eb-147">新增 greet.proto 檔案</span><span class="sxs-lookup"><span data-stu-id="9d3eb-147">Add the greet.proto file</span></span>

<span data-ttu-id="9d3eb-148">將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="9d3eb-149">將 **greet.proto** 檔案新增至 GrpcGreeterClient 專案檔的 `<Protobuf>` 項目群組：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="9d3eb-150">以滑鼠右鍵按一下專案並從下拉式功能表中選取 [編輯 GrpcGreeterClient.csproj] 選項，可讓您開啟GrpcGreeterClient 的專案檔。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="9d3eb-152">或者，您可以巡覽至 GrpcGreeterClient 目錄，使用您慣用的編輯器來編輯 `GrpcGreeterClient.csproj`。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="9d3eb-153">會新增 `GrpcServices="Client"` 屬性，以僅為包含的 protobuf 檔案產生 C# 用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="9d3eb-154">建置用戶端專案以觸發 C# 用戶端資產的產生。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="9d3eb-155">建立 GreeterClient 並叫用 SayHello 一元呼叫</span><span class="sxs-lookup"><span data-stu-id="9d3eb-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="9d3eb-156">將下列程式碼新增至 gRPC 用戶端專案 `Program.cs` 檔案的 `Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="9d3eb-157">若要存取必要的類型，下列 using 陳述式是必要項目：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="9d3eb-158">GreeterClient 是透過具現化 `Channel` (包含用於建立與 gRPC 服務連線的資訊) 所建立，並使用其來建構 `GreeterClient`：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="9d3eb-159">GreeterClient 包含一元呼叫 `SayHello` (可透過非同步方式叫用)：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="9d3eb-160">`SayHello` 呼叫的結果，會儲存在可接著顯示的 `reply`：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="9d3eb-161">用戶端所使用的 `Channel` 應在作業完成釋放所有資源時關閉：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="9d3eb-162">您需要在可以解析 **Greeter** 命名空間中的類型之前建置專案。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="9d3eb-163">這些類型會在建置期間自動產生，且必須在執行組建後才能使用。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="9d3eb-164">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="9d3eb-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9d3eb-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9d3eb-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9d3eb-166">請確定在上一個教學課程中建立的 Greeter 服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="9d3eb-167">服務開始執行後，返回 **GrpcGreeterClient** 專案並將其設定為啟始專案。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="9d3eb-168">按 Ctrl+F5 即可執行用戶端而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="9d3eb-169">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="9d3eb-170">服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/client.png)

  <span data-ttu-id="9d3eb-172">服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![新增 ASP.NET Core Web 應用程式](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9d3eb-174">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9d3eb-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9d3eb-175">請確定在上一個教學課程中建立的 Greeter 服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="9d3eb-176">使用 `dotnet run` 從個別的命令列執行用戶端專案 GrpcGreeter.Client。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="9d3eb-177">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="9d3eb-178">服務會傳送 "Hello GreeterClient" 訊息作為在命令提示字元中顯示的回應。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="9d3eb-179">服務會將成功呼叫的詳細資料記錄在寫入至命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="9d3eb-180">檢查 gRPC 專案的專案檔</span><span class="sxs-lookup"><span data-stu-id="9d3eb-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="9d3eb-181">gRPC 用戶端 GrpcGreeterClient 檔案：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="9d3eb-182">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d3eb-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d3eb-183">Additional resources</span></span>

<span data-ttu-id="9d3eb-184">在本教學課程中，您已：</span><span class="sxs-lookup"><span data-stu-id="9d3eb-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9d3eb-185">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="9d3eb-186">對上一個教學課程中建立的 gRPC Greeter 服務執行該服務。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="9d3eb-187">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="9d3eb-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9d3eb-188">上一步：建立 gRPC Greeter 服務</span><span class="sxs-lookup"><span data-stu-id="9d3eb-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
