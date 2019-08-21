---
page_type: sample
description: 此教學課程會示範如何在 ASP.NET Core 上，建立 gRPC 服務與 gRPC 用戶端。
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: a281adc3b1fe90eeb32c1185750f911af683af83
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029011"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="4b9c7-102">使用 Visual Studio 在 ASP.NET Core 3.0 中建立 gRPC 用戶端和伺服器</span><span class="sxs-lookup"><span data-stu-id="4b9c7-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="4b9c7-103">本教學課程示範如何建立 .NET Core [gRPC](https://grpc.io/docs/guides/) 用戶端，以及 ASP.NET Core gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="4b9c7-104">在結束時，您將擁有可與 gRPC Greeter 服務通訊的 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="4b9c7-105">在本教學課程中，您會：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-105">In this tutorial, you;</span></span>

* <span data-ttu-id="4b9c7-106">建立 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="4b9c7-107">建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-107">Create a gRPC client.</span></span>
* <span data-ttu-id="4b9c7-108">利用 gRPC Greeter 服務來測試 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="4b9c7-109">建立 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="4b9c7-109">Create a gRPC service</span></span>

* <span data-ttu-id="4b9c7-110">從 Visual Studio 的 [檔案]  功能表中，選取 [新增]   > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="4b9c7-111">在 [建立新專案]  對話方塊中，選取 [ASP.NET Core Web 應用程式]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="4b9c7-112">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="4b9c7-112">Select **Next**</span></span>
* <span data-ttu-id="4b9c7-113">將專案命名為 **GrpcGreeter**。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="4b9c7-114">請務必將專案命名為 *GrpcGreeter*，如此當您複製並貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="4b9c7-115">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="4b9c7-115">Select **Create**</span></span>
* <span data-ttu-id="4b9c7-116">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="4b9c7-117">從下拉式功能表中，選取 [.NET Core]  與 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="4b9c7-118">選取 [gRPC 服務]  範本。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="4b9c7-119">選取 [建立] </span><span class="sxs-lookup"><span data-stu-id="4b9c7-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="4b9c7-120">執行服務</span><span class="sxs-lookup"><span data-stu-id="4b9c7-120">Run the service</span></span>

* <span data-ttu-id="4b9c7-121">按 `Ctrl+F5` 執行 gRPC 服務，但不啟動偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="4b9c7-122">Visual Studio 會在命令提示字元中執行服務。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="4b9c7-123">該記錄會顯示正在接聽 `https://localhost:5001` 的服務。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-123">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> <span data-ttu-id="4b9c7-124">gRPC 範本已設為使用[傳輸層安全性 (TLS)](https://tools.ietf.org/html/rfc5246)。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="4b9c7-125">gRPC 用戶端需要使用 HTTPS 來呼叫伺服器。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="4b9c7-126">macOS 不支援具有 TLS 的 ASP.NET Core gRPC。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="4b9c7-127">您需要額外的組態才能在 macOS 上成功執行 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="4b9c7-128">如需詳細資訊，請參閱 [macOS 上的 gRPC 和 ASP.NET Core](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos)。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="4b9c7-129">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="4b9c7-129">Examine the project files</span></span>

<span data-ttu-id="4b9c7-130">*GrpcGreeter* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="4b9c7-131">*greet.proto*：*Protos/greet.proto* 檔案會定義 `Greeter` gRPC，並會用來產生 gRPC 伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="4b9c7-132">如需詳細資訊，請參閱 [gRPC 簡介](xref:grpc/index)。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="4b9c7-133">*Services* 資料夾：包含 `Greeter` 服務的實作。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="4b9c7-134">*appSettings.json*：包含組態資料，例如 Kestrel 所使用的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="4b9c7-135">如需詳細資訊，請參閱 <xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="4b9c7-136">*Program.cs*：包含 gRPC 服務的進入點。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="4b9c7-137">如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="4b9c7-138">*Startup.cs*：包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="4b9c7-139">如需詳細資訊，請參閱[應用程式啟動](xref:fundamentals/startup)。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="4b9c7-140">在 .NET 主控台應用程式中建立 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="4b9c7-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="4b9c7-141">開啟 Visual Studio 的第二個執行個體。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="4b9c7-142">從功能表列中選取 [檔案]   >  [新增]   >  [專案]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="4b9c7-143">在 [建立新專案]  對話方塊中，選取 [主控台應用程式 (.NET Core)]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="4b9c7-144">選取 [下一步] </span><span class="sxs-lookup"><span data-stu-id="4b9c7-144">Select **Next**</span></span>
* <span data-ttu-id="4b9c7-145">在 [名稱]  文字方塊中，輸入 "GrpcGreeterClient"。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="4b9c7-146">選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="4b9c7-147">新增必要套件</span><span class="sxs-lookup"><span data-stu-id="4b9c7-147">Add required packages</span></span>

<span data-ttu-id="4b9c7-148">gRPC 用戶端專案需要下列套件：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="4b9c7-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client)，包含 .NET Core 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="4b9c7-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)，包含 C# 的 protobuf 訊息 API。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="4b9c7-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)，包含 protobuf 檔案的 C# 工具支援。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="4b9c7-152">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="4b9c7-153">使用套件管理員主控台 (PMC) 或管理 NuGet 套件來安裝套件。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="4b9c7-154">安裝套件的 PMC 選項</span><span class="sxs-lookup"><span data-stu-id="4b9c7-154">PMC option to install packages</span></span>

* <span data-ttu-id="4b9c7-155">從 Visual Studio 選取 [工具]   > [NuGet 套件管理員]   > [套件管理員主控台] </span><span class="sxs-lookup"><span data-stu-id="4b9c7-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="4b9c7-156">從 [套件管理員主控台]  視窗巡覽至 *GrpcGreeterClient.csproj* 檔案所在的目錄。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="4b9c7-157">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="4b9c7-158">管理 NuGet 套件選項來安裝套件</span><span class="sxs-lookup"><span data-stu-id="4b9c7-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="4b9c7-159">在 [方案總管]   > [管理 NuGet 套件]  中，以滑鼠右鍵按一下專案</span><span class="sxs-lookup"><span data-stu-id="4b9c7-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="4b9c7-160">選取 [瀏覽]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="4b9c7-161">在搜尋方塊中輸入 **Grpc.Net.Client**。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="4b9c7-162">從 [瀏覽]  索引標籤選取 [Grpc.Net.Client]  套件，然後選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="4b9c7-163">對 `Google.Protobuf` 與 `Grpc.Tools` 重複進行。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="4b9c7-164">新增 greet.proto</span><span class="sxs-lookup"><span data-stu-id="4b9c7-164">Add greet.proto</span></span>

* <span data-ttu-id="4b9c7-165">在 gRPC 用戶端專案中建立 [Protos]  資料夾。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="4b9c7-166">將 **Protos\greet.proto** 檔案從 gRPC Greeter 服務複製到 gRPC 用戶端專案。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="4b9c7-167">編輯 *GrpcGreeterClient.csproj* 專案檔：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="4b9c7-168">以滑鼠右鍵按一下專案，然後選取 [編輯專案檔]  。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="4b9c7-169">新增具有代表 **greet.proto** 檔案之 `<Protobuf>` 元素的項目群組：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="4b9c7-170">建立 Greeter 用戶端</span><span class="sxs-lookup"><span data-stu-id="4b9c7-170">Create the Greeter client</span></span>

<span data-ttu-id="4b9c7-171">建置專案，以便在 `GrpcGreeter` 命名空間中建立類型。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="4b9c7-172">`GrpcGreeter` 類型會自動由建置處理序產生。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="4b9c7-173">利用下列程式碼，更新 gRPC 用戶端 *Program.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var httpClient = new HttpClient();
            // The port number(5001) must match the port of the gRPC server.
            httpClient.BaseAddress = new Uri("https://localhost:5001");
            var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="4b9c7-174">*Program.cs* 包含 gRPC 用戶端的進入點和邏輯。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="4b9c7-175">Greeter 用戶端建立者：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="4b9c7-176">具現化包含建立 gRPC 服務連線資訊的 `HttpClient`。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-176">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="4b9c7-177">使用 `HttpClient` 建構 Greeter 用戶端：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-177">Using the `HttpClient` to construct the Greeter client:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

<span data-ttu-id="4b9c7-178">Greeter 用戶端會呼叫非同步的 `SayHello` 方法。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-178">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="4b9c7-179">顯示 `SayHello` 呼叫的結果：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-179">The result of the `SayHello` call is displayed:</span></span>

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
    var reply = await client.SayHelloAsync(
                      new HelloRequest { Name = "GreeterClient" });
    Console.WriteLine("Greeting: " + reply.Message);
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();
}
```

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="4b9c7-180">使用 gRPC Greeter 服務測試 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="4b9c7-180">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="4b9c7-181">在 Greeter 服務中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動伺服器。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-181">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="4b9c7-182">在 `GrpcGreeterClient` 專案中按下 `Ctrl+F5`，在不啟動偵錯工具的情況下啟動用戶端。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-182">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="4b9c7-183">用戶端會以包含其名稱 "GreeterClient" 的訊息向服務傳送問候語。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-183">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="4b9c7-184">該服務會傳送 "Hello GreeterClient" 訊息做為回應。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-184">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="4b9c7-185">"Hello GreeterClient" 回應會顯示在命令提示字元中：</span><span class="sxs-lookup"><span data-stu-id="4b9c7-185">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="4b9c7-186">gRPC 服務會將成功呼叫的詳細資料，記錄在會寫入命令提示字元的記錄中。</span><span class="sxs-lookup"><span data-stu-id="4b9c7-186">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="4b9c7-187">gRPC 的文件協助和後續步驟</span><span class="sxs-lookup"><span data-stu-id="4b9c7-187">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="4b9c7-188">ASP.NET Core 上的 gRPC 簡介</span><span class="sxs-lookup"><span data-stu-id="4b9c7-188">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="4b9c7-189">使用 C# 的 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="4b9c7-189">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="4b9c7-190">將 gRPC 服務從 C-Core 遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4b9c7-190">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
