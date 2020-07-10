---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: '瞭解使用 c # 撰寫 gRPC 服務時的基本概念。'
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/09/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/basics
ms.openlocfilehash: a99e5c78e291469bdce5c2b5cfa6eec86caa9735
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176288"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="48c56-103">使用 C gRPC 服務\#</span><span class="sxs-lookup"><span data-stu-id="48c56-103">gRPC services with C\#</span></span>

<span data-ttu-id="48c56-104">本檔概述在 c # 中撰寫[gRPC](https://grpc.io/docs/guides/)應用程式所需的概念。</span><span class="sxs-lookup"><span data-stu-id="48c56-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="48c56-105">這裡涵蓋的主題適用于以[C 核心](https://grpc.io/blog/grpc-stacks)和 ASP.NET Core 為基礎的 gRPC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="48c56-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="proto-file"></a><span data-ttu-id="48c56-106">proto 檔案</span><span class="sxs-lookup"><span data-stu-id="48c56-106">proto file</span></span>

<span data-ttu-id="48c56-107">gRPC 會使用合約優先的方法來開發 API。</span><span class="sxs-lookup"><span data-stu-id="48c56-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="48c56-108">預設會使用 (protobuf) 的通訊協定緩衝區作為介面設計語言 (IDL) 。</span><span class="sxs-lookup"><span data-stu-id="48c56-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="48c56-109">此\* \* proto\*檔案包含：</span><span class="sxs-lookup"><span data-stu-id="48c56-109">The *\*.proto* file contains:</span></span>

* <span data-ttu-id="48c56-110">GRPC 服務的定義。</span><span class="sxs-lookup"><span data-stu-id="48c56-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="48c56-111">在用戶端與伺服器之間傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="48c56-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="48c56-112">如需 protobuf 檔語法的詳細資訊，請參閱[官方檔 (protobuf) ](https://developers.google.com/protocol-buffers/docs/proto3)。</span><span class="sxs-lookup"><span data-stu-id="48c56-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="48c56-113">例如，請考慮[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start)中使用的*歡迎*：</span><span class="sxs-lookup"><span data-stu-id="48c56-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="48c56-114">定義 `Greeter` 服務。</span><span class="sxs-lookup"><span data-stu-id="48c56-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="48c56-115">`Greeter`服務會定義一個 `SayHello` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="48c56-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="48c56-116">`SayHello`傳送 `HelloRequest` 訊息並接收 `HelloReply` 訊息：</span><span class="sxs-lookup"><span data-stu-id="48c56-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="48c56-117">將 proto 檔案新增至 C \# 應用程式</span><span class="sxs-lookup"><span data-stu-id="48c56-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="48c56-118">在專案中加入此\* \* proto\*檔案，方法是將它加入至專案 `<Protobuf>` 群組：</span><span class="sxs-lookup"><span data-stu-id="48c56-118">The *\*.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="48c56-119">根據預設， `<Protobuf>` 參考會產生具體的用戶端和服務基類。</span><span class="sxs-lookup"><span data-stu-id="48c56-119">By default, a `<Protobuf>` reference generates a concrete client and a service base class.</span></span> <span data-ttu-id="48c56-120">參考專案的 `GrpcServices` 屬性可以用來限制 c # 資產產生。</span><span class="sxs-lookup"><span data-stu-id="48c56-120">The reference element's `GrpcServices` attribute can be used to limit C# asset generation.</span></span> <span data-ttu-id="48c56-121">有效 `GrpcServices` 的選項為：</span><span class="sxs-lookup"><span data-stu-id="48c56-121">Valid `GrpcServices` options are:</span></span>

* <span data-ttu-id="48c56-122">`Both`不存在時 (預設值) </span><span class="sxs-lookup"><span data-stu-id="48c56-122">`Both` (default when not present)</span></span>
* `Server`
* `Client`
* `None`

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="48c56-123">適用于 proto 檔案的 c # 工具支援</span><span class="sxs-lookup"><span data-stu-id="48c56-123">C# Tooling support for .proto files</span></span>

<span data-ttu-id="48c56-124">[工具套件 Grpc](https://www.nuget.org/packages/Grpc.Tools/) ：從\* \* Proto\*檔案產生 c # 資產時所需。</span><span class="sxs-lookup"><span data-stu-id="48c56-124">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *\*.proto* files.</span></span> <span data-ttu-id="48c56-125">產生的資產 (檔案) ：</span><span class="sxs-lookup"><span data-stu-id="48c56-125">The generated assets (files):</span></span>

* <span data-ttu-id="48c56-126">每次建立專案時，會在需要時產生。</span><span class="sxs-lookup"><span data-stu-id="48c56-126">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="48c56-127">不會加入至專案或簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="48c56-127">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="48c56-128">是包含在*obj*目錄中的組建成品。</span><span class="sxs-lookup"><span data-stu-id="48c56-128">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="48c56-129">伺服器和用戶端專案都需要此封裝。</span><span class="sxs-lookup"><span data-stu-id="48c56-129">This package is required by both the server and client projects.</span></span> <span data-ttu-id="48c56-130">`Grpc.AspNetCore`中繼套件包含的參考 `Grpc.Tools` 。</span><span class="sxs-lookup"><span data-stu-id="48c56-130">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="48c56-131">伺服器專案可以 `Grpc.AspNetCore` 在 Visual Studio 中使用封裝管理員，或藉由將加入 `<PackageReference>` 至專案檔來加入：</span><span class="sxs-lookup"><span data-stu-id="48c56-131">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="48c56-132">用戶端專案應直接 `Grpc.Tools` 與使用 gRPC 用戶端所需的其他套件一起參考。</span><span class="sxs-lookup"><span data-stu-id="48c56-132">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="48c56-133">執行時間不需要工具套件，因此會以下列方式標記相依性 `PrivateAssets="All"` ：</span><span class="sxs-lookup"><span data-stu-id="48c56-133">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="48c56-134">產生的 c # 資產</span><span class="sxs-lookup"><span data-stu-id="48c56-134">Generated C# assets</span></span>

<span data-ttu-id="48c56-135">工具套件會產生 c # 型別，代表包含的\* \* proto\*檔案中定義的訊息。</span><span class="sxs-lookup"><span data-stu-id="48c56-135">The tooling package generates the C# types representing the messages defined in the included *\*.proto* files.</span></span>

<span data-ttu-id="48c56-136">若為伺服器端資產，則會產生抽象服務基底類型。</span><span class="sxs-lookup"><span data-stu-id="48c56-136">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="48c56-137">基底類型包含包含在*proto*檔案中所有 gRPC 呼叫的定義。</span><span class="sxs-lookup"><span data-stu-id="48c56-137">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="48c56-138">建立衍生自這個基底類型的具象服務實作為，並針對 gRPC 呼叫執行邏輯。</span><span class="sxs-lookup"><span data-stu-id="48c56-138">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="48c56-139">針對 `greet.proto` ，先前所述的範例 `GreeterBase` 會產生包含虛擬方法的抽象類別型 `SayHello` 。</span><span class="sxs-lookup"><span data-stu-id="48c56-139">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="48c56-140">具體的執行會 `GreeterService` 覆寫方法，並實行處理 gRPC 呼叫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="48c56-140">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="48c56-141">針對用戶端資產，會產生具體的用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="48c56-141">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="48c56-142">在*proto*檔案中的 gRPC 呼叫會轉譯為具象型別上的方法，可以呼叫。</span><span class="sxs-lookup"><span data-stu-id="48c56-142">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="48c56-143">針對 `greet.proto` ，先前所述的範例 `GreeterClient` 會產生具體類型。</span><span class="sxs-lookup"><span data-stu-id="48c56-143">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="48c56-144">呼叫 `GreeterClient.SayHelloAsync` 以起始對伺服器的 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="48c56-144">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="48c56-145">根據預設，會針對專案群組中包含的每個\* \* proto\*檔案產生伺服器和用戶端資產 `<Protobuf>` 。</span><span class="sxs-lookup"><span data-stu-id="48c56-145">By default, server and client assets are generated for each *\*.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="48c56-146">為確保伺服器專案中只產生伺服器資產， `GrpcServices` 屬性會設定為 `Server` 。</span><span class="sxs-lookup"><span data-stu-id="48c56-146">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="48c56-147">同樣地， `Client` 在用戶端專案中，屬性會設定為。</span><span class="sxs-lookup"><span data-stu-id="48c56-147">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48c56-148">其他資源</span><span class="sxs-lookup"><span data-stu-id="48c56-148">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
