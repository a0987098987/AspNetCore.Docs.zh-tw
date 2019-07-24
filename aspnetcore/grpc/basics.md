---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 瞭解使用C#撰寫 gRPC 服務時的基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 700fe9463317f9ee30dfe4ebf5201c7b9c0c5ad6
ms.sourcegitcommit: f30b18442ed12831c7e86b0db249183ccd749f59
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "68412478"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="3faed-103">使用 C gRPC 服務\#</span><span class="sxs-lookup"><span data-stu-id="3faed-103">gRPC services with C\#</span></span>

<span data-ttu-id="3faed-104">本檔概述在中C#撰寫[gRPC](https://grpc.io/docs/guides/)應用程式所需的概念。</span><span class="sxs-lookup"><span data-stu-id="3faed-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="3faed-105">這裡涵蓋的主題適用于以[C 核心](https://grpc.io/blog/grpc-stacks)和 ASP.NET Core 為基礎的 gRPC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3faed-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="3faed-106">proto 檔案</span><span class="sxs-lookup"><span data-stu-id="3faed-106">proto file</span></span>

<span data-ttu-id="3faed-107">gRPC 會使用合約優先的方法來開發 API。</span><span class="sxs-lookup"><span data-stu-id="3faed-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="3faed-108">通訊協定緩衝區 (protobuf) 預設會當做介面設計語言 (IDL) 使用。</span><span class="sxs-lookup"><span data-stu-id="3faed-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="3faed-109">此*proto*檔案包含:</span><span class="sxs-lookup"><span data-stu-id="3faed-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="3faed-110">GRPC 服務的定義。</span><span class="sxs-lookup"><span data-stu-id="3faed-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="3faed-111">在用戶端與伺服器之間傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="3faed-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="3faed-112">如需 protobuf 檔語法的詳細資訊, 請參閱[官方檔 (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3)。</span><span class="sxs-lookup"><span data-stu-id="3faed-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="3faed-113">例如, 請考慮[開始使用 gRPC 服務](xref:tutorials/grpc/grpc-start)中使用的*歡迎*:</span><span class="sxs-lookup"><span data-stu-id="3faed-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="3faed-114">`Greeter`定義服務。</span><span class="sxs-lookup"><span data-stu-id="3faed-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="3faed-115">服務會定義一個`SayHello`呼叫。 `Greeter`</span><span class="sxs-lookup"><span data-stu-id="3faed-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="3faed-116">`SayHello`傳送訊息並接收`HelloReply`訊息: `HelloRequest`</span><span class="sxs-lookup"><span data-stu-id="3faed-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="3faed-117">將 proto 檔案新增至 C\#應用程式</span><span class="sxs-lookup"><span data-stu-id="3faed-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="3faed-118">在專案中加入此*proto*檔案, 方法是將它加入至`<Protobuf>`專案群組:</span><span class="sxs-lookup"><span data-stu-id="3faed-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="3faed-119">C#適用于 proto 檔案的工具支援</span><span class="sxs-lookup"><span data-stu-id="3faed-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="3faed-120">需要工具套件[Grpc](https://www.nuget.org/packages/Grpc.Tools/) , 才能從C# *proto*檔案產生資產。</span><span class="sxs-lookup"><span data-stu-id="3faed-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="3faed-121">產生的資產 (files):</span><span class="sxs-lookup"><span data-stu-id="3faed-121">The generated assets (files):</span></span>

* <span data-ttu-id="3faed-122">每次建立專案時, 會在需要時產生。</span><span class="sxs-lookup"><span data-stu-id="3faed-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="3faed-123">不會加入至專案或簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="3faed-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="3faed-124">是包含在*obj*目錄中的組建成品。</span><span class="sxs-lookup"><span data-stu-id="3faed-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="3faed-125">伺服器和用戶端專案都需要此封裝。</span><span class="sxs-lookup"><span data-stu-id="3faed-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="3faed-126">中繼套件包含的`Grpc.Tools`參考。 `Grpc.AspNetCore`</span><span class="sxs-lookup"><span data-stu-id="3faed-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="3faed-127">伺服器專案可以在`Grpc.AspNetCore` Visual Studio 中使用封裝管理員, 或藉由`<PackageReference>`將加入至專案檔來加入:</span><span class="sxs-lookup"><span data-stu-id="3faed-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="3faed-128">用戶端專案應`Grpc.Tools`直接參考。</span><span class="sxs-lookup"><span data-stu-id="3faed-128">Client projects should reference `Grpc.Tools` directly.</span></span> <span data-ttu-id="3faed-129">執行時間不需要工具套件, 因此會以下列方式`PrivateAssets="All"`標記相依性:</span><span class="sxs-lookup"><span data-stu-id="3faed-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=1&range=11)]

## <a name="generated-c-assets"></a><span data-ttu-id="3faed-130">產生C#的資產</span><span class="sxs-lookup"><span data-stu-id="3faed-130">Generated C# assets</span></span>

<span data-ttu-id="3faed-131">工具套件會產生代表C#包含的 *. proto*檔案中所定義之訊息的類型。</span><span class="sxs-lookup"><span data-stu-id="3faed-131">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="3faed-132">若為伺服器端資產, 則會產生抽象服務基底類型。</span><span class="sxs-lookup"><span data-stu-id="3faed-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="3faed-133">基底類型包含包含在*proto*檔案中所有 gRPC 呼叫的定義。</span><span class="sxs-lookup"><span data-stu-id="3faed-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="3faed-134">建立衍生自這個基底類型的具象服務實作為, 並針對 gRPC 呼叫執行邏輯。</span><span class="sxs-lookup"><span data-stu-id="3faed-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="3faed-135">針對, 先前所述的範例會產生包含`GreeterBase`虛擬`SayHello`方法的抽象類別型。 `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="3faed-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="3faed-136">具體的執行`GreeterService`會覆寫方法, 並實行處理 gRPC 呼叫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="3faed-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="3faed-137">針對用戶端資產, 會產生具體的用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="3faed-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="3faed-138">在*proto*檔案中的 gRPC 呼叫會轉譯為具象型別上的方法, 可以呼叫。</span><span class="sxs-lookup"><span data-stu-id="3faed-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="3faed-139">針對, 先前所述的範例會產生具體`GreeterClient`類型。 `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="3faed-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="3faed-140">呼叫`GreeterClient.SayHello`以起始對伺服器的 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="3faed-140">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=3-6&name=snippet)]

<span data-ttu-id="3faed-141">根據預設, 會針對`<Protobuf>`專案群組中包含的每個*proto*檔案產生伺服器和用戶端資產。</span><span class="sxs-lookup"><span data-stu-id="3faed-141">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="3faed-142">為確保伺服器專案中只產生伺服器資產, `GrpcServices`屬性會設定為。 `Server`</span><span class="sxs-lookup"><span data-stu-id="3faed-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="3faed-143">同樣地, 在用戶端專案`Client`中, 屬性會設定為。</span><span class="sxs-lookup"><span data-stu-id="3faed-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3faed-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="3faed-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
