---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 使用 C# 編寫 gRPC 服務時,瞭解基本概念。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 59257449e211ddf9c7faa5f21a3986caba4eebc6
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664203"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="72008-103">gRPC 服務,帶 C\#</span><span class="sxs-lookup"><span data-stu-id="72008-103">gRPC services with C\#</span></span>

<span data-ttu-id="72008-104">本文檔概述了在 C# 中編寫[gRPC](https://grpc.io/docs/guides/)應用所需的概念。</span><span class="sxs-lookup"><span data-stu-id="72008-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="72008-105">此處介紹的主題適用於基於[C 核](https://grpc.io/blog/grpc-stacks)和基於 ASP.NET 基於內核的 gRPC 應用。</span><span class="sxs-lookup"><span data-stu-id="72008-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="72008-106">原型檔案</span><span class="sxs-lookup"><span data-stu-id="72008-106">proto file</span></span>

<span data-ttu-id="72008-107">gRPC 使用協定優先的方法進行 API 開發。</span><span class="sxs-lookup"><span data-stu-id="72008-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="72008-108">默認情況下,協定緩衝區(原語)用作介面設計語言 (IDL)。</span><span class="sxs-lookup"><span data-stu-id="72008-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="72008-109">.proto 檔案包含: \* \*\*</span><span class="sxs-lookup"><span data-stu-id="72008-109">The *\*.proto* file contains:</span></span>

* <span data-ttu-id="72008-110">gRPC 服務的定義。</span><span class="sxs-lookup"><span data-stu-id="72008-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="72008-111">用戶端和伺服器之間發送的消息。</span><span class="sxs-lookup"><span data-stu-id="72008-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="72008-112">有關原檔語法的詳細資訊,請參閱[官方文檔(protobuf)。](https://developers.google.com/protocol-buffers/docs/proto3)</span><span class="sxs-lookup"><span data-stu-id="72008-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="72008-113">例如,請考慮使用[gRPC 服務開始](xref:tutorials/grpc/grpc-start)中使用的*greet.proto*檔案:</span><span class="sxs-lookup"><span data-stu-id="72008-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="72008-114">定義`Greeter`服務。</span><span class="sxs-lookup"><span data-stu-id="72008-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="72008-115">服務`Greeter`定義`SayHello`呼叫。</span><span class="sxs-lookup"><span data-stu-id="72008-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="72008-116">`SayHello`傳送`HelloRequest`訊息 並`HelloReply`接收訊息 :</span><span class="sxs-lookup"><span data-stu-id="72008-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="72008-117">將 .proto 檔案\#加入到 C 應用</span><span class="sxs-lookup"><span data-stu-id="72008-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="72008-118">.proto 檔案透過您新增`<Protobuf>`到項目中包含在項目中: \* \*\*</span><span class="sxs-lookup"><span data-stu-id="72008-118">The *\*.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="72008-119">C# 對 .proto 檔案的工具支援</span><span class="sxs-lookup"><span data-stu-id="72008-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="72008-120">工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)需要從*\*.proto*檔案生成 C# 資源。</span><span class="sxs-lookup"><span data-stu-id="72008-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *\*.proto* files.</span></span> <span data-ttu-id="72008-121">產生的資產(檔案):</span><span class="sxs-lookup"><span data-stu-id="72008-121">The generated assets (files):</span></span>

* <span data-ttu-id="72008-122">在每次生成項目時根據需要生成。</span><span class="sxs-lookup"><span data-stu-id="72008-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="72008-123">未添加到專案或簽入原始程式碼管理。</span><span class="sxs-lookup"><span data-stu-id="72008-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="72008-124">是*obj*目錄中的生成專案。</span><span class="sxs-lookup"><span data-stu-id="72008-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="72008-125">伺服器和用戶端專案都需要此包。</span><span class="sxs-lookup"><span data-stu-id="72008-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="72008-126">元`Grpc.AspNetCore`包包括對的`Grpc.Tools`引用。</span><span class="sxs-lookup"><span data-stu-id="72008-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="72008-127">伺服器專案可以使用`Grpc.AspNetCore`Visual Studio 中的套件管理員,`<PackageReference>`也可以加入專案檔案加入 :</span><span class="sxs-lookup"><span data-stu-id="72008-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="72008-128">用戶端專案應直接與`Grpc.Tools`使用 gRPC 用戶端所需的其他包一起引用。</span><span class="sxs-lookup"><span data-stu-id="72008-128">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="72008-129">執行時不需要工具組,因此依賴項用 標記`PrivateAssets="All"`:</span><span class="sxs-lookup"><span data-stu-id="72008-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="72008-130">產生的 C# 資產</span><span class="sxs-lookup"><span data-stu-id="72008-130">Generated C# assets</span></span>

<span data-ttu-id="72008-131">工具套件生成 C# 類型,表示在包含*\*的 .proto*檔中定義的消息。</span><span class="sxs-lookup"><span data-stu-id="72008-131">The tooling package generates the C# types representing the messages defined in the included *\*.proto* files.</span></span>

<span data-ttu-id="72008-132">對於伺服器端資產,將生成抽象服務基礎類型。</span><span class="sxs-lookup"><span data-stu-id="72008-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="72008-133">基類型包含 *.proto*檔中包含的所有 gRPC 調用的定義。</span><span class="sxs-lookup"><span data-stu-id="72008-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="72008-134">創建源自此基類型的具體服務實現,並實現 gRPC 調用的邏輯。</span><span class="sxs-lookup"><span data-stu-id="72008-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="72008-135">對於`greet.proto`前面描述的範例,將生成包含`GreeterBase``SayHello`虛擬方法的抽象類型。</span><span class="sxs-lookup"><span data-stu-id="72008-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="72008-136">具體實現`GreeterService`重寫該方法並實現處理 gRPC 調用的邏輯。</span><span class="sxs-lookup"><span data-stu-id="72008-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="72008-137">對於客戶端資產,將生成具體的用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="72008-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="72008-138">*.proto*檔中的 gRPC 調用被轉換為具體類型上的方法,可以調用。</span><span class="sxs-lookup"><span data-stu-id="72008-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="72008-139">對於`greet.proto`前面描述的示例,將生成具體`GreeterClient`類型。</span><span class="sxs-lookup"><span data-stu-id="72008-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="72008-140">調用`GreeterClient.SayHelloAsync`以啟動對伺服器的 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="72008-140">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="72008-141">預設情況下,為`<Protobuf>`專案組中包括的每個*\*.proto*檔案生成伺服器和用戶端資源。</span><span class="sxs-lookup"><span data-stu-id="72008-141">By default, server and client assets are generated for each *\*.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="72008-142">為了確保伺服器項目中僅產生伺服器資產,該`GrpcServices`屬性設定為`Server`。</span><span class="sxs-lookup"><span data-stu-id="72008-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="72008-143">同樣,該屬性設置為`Client`在用戶端專案中。</span><span class="sxs-lookup"><span data-stu-id="72008-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="72008-144">其他資源</span><span class="sxs-lookup"><span data-stu-id="72008-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
