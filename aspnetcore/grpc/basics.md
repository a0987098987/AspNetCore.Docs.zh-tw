---
title: 搭配 C# 的 gRPC 服務
author: juntaoluo
description: 撰寫 gRPC 服務時，了解基本概念C#。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 5a88bd0e9f789058b3606691c5ebd9a74325ac9b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376350"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="091ab-103">使用 c# 的 gRPC 服務\#</span><span class="sxs-lookup"><span data-stu-id="091ab-103">gRPC services with C\#</span></span>

<span data-ttu-id="091ab-104">本文件概述撰寫所需的概念[gRPC](https://grpc.io/docs/guides/)中的應用程式C#。</span><span class="sxs-lookup"><span data-stu-id="091ab-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="091ab-105">本主題同時適用於[C core](https://grpc.io/blog/grpc-stacks)-根據和 ASP.NET Core 為基礎的 gRPC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="091ab-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="091ab-106">proto 檔案</span><span class="sxs-lookup"><span data-stu-id="091ab-106">proto file</span></span>

<span data-ttu-id="091ab-107">gRPC 使用合約優先方法 API 的開發。</span><span class="sxs-lookup"><span data-stu-id="091ab-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="091ab-108">依預設會以介面設計語言 (IDL) 為使用通訊協定緩衝區 (protobuf)。</span><span class="sxs-lookup"><span data-stu-id="091ab-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="091ab-109">*.Proto*檔案包含：</span><span class="sxs-lookup"><span data-stu-id="091ab-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="091ab-110">GRPC 服務的定義。</span><span class="sxs-lookup"><span data-stu-id="091ab-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="091ab-111">用戶端與伺服器之間傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="091ab-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="091ab-112">如需詳細的語法 protobuf 檔案的詳細資訊，請參閱[官方文件 (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3)。</span><span class="sxs-lookup"><span data-stu-id="091ab-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="091ab-113">例如，請考慮*greet.proto*中所使用的檔案[開始使用 gRPC service](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="091ab-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="091ab-114">定義`Greeter`服務。</span><span class="sxs-lookup"><span data-stu-id="091ab-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="091ab-115">`Greeter`服務會定義`SayHello`呼叫。</span><span class="sxs-lookup"><span data-stu-id="091ab-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="091ab-116">`SayHello` 傳送`HelloRequest`訊息，並接收`HelloResponse`訊息：</span><span class="sxs-lookup"><span data-stu-id="091ab-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="091ab-117">將.proto 檔案新增至 C\#應用程式</span><span class="sxs-lookup"><span data-stu-id="091ab-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="091ab-118">*.Proto*檔案包含在專案中將它加入至`<Protobuf>`項目群組：</span><span class="sxs-lookup"><span data-stu-id="091ab-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="091ab-119">C#.Proto 檔案的工具支援</span><span class="sxs-lookup"><span data-stu-id="091ab-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="091ab-120">工具套件[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)才能產生C#中的資產 *.proto*檔案。</span><span class="sxs-lookup"><span data-stu-id="091ab-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="091ab-121">產生的資產 （檔案）：</span><span class="sxs-lookup"><span data-stu-id="091ab-121">The generated assets (files):</span></span>

* <span data-ttu-id="091ab-122">會產生可視需要在每次建置專案。</span><span class="sxs-lookup"><span data-stu-id="091ab-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="091ab-123">不會加入至專案，或簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="091ab-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="091ab-124">會包含在組建成品*obj*目錄。</span><span class="sxs-lookup"><span data-stu-id="091ab-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="091ab-125">此套件所需的伺服器和用戶端專案中。</span><span class="sxs-lookup"><span data-stu-id="091ab-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="091ab-126">`Grpc.Tools` 您可以加入 Visual Studio 中使用封裝管理員，或新增`<PackageReference>`souboru projektu:</span><span class="sxs-lookup"><span data-stu-id="091ab-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

<span data-ttu-id="091ab-127">執行階段不需要工具套件，因此相依性會標示為 `PrivateAssets="All"`。</span><span class="sxs-lookup"><span data-stu-id="091ab-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="091ab-128">產生C#資產</span><span class="sxs-lookup"><span data-stu-id="091ab-128">Generated C# assets</span></span>

<span data-ttu-id="091ab-129">工具套件會產生C#類型代表的定義中包含的訊息 *.proto*檔案。</span><span class="sxs-lookup"><span data-stu-id="091ab-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="091ab-130">針對伺服器端資產產生服務的抽象基底型別。</span><span class="sxs-lookup"><span data-stu-id="091ab-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="091ab-131">基底型別包含所有 gRPC 呼叫中所包含的定義 *.proto*檔案。</span><span class="sxs-lookup"><span data-stu-id="091ab-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="091ab-132">建立衍生自這個基底型別並實作 gRPC 呼叫的邏輯實體的服務實作。</span><span class="sxs-lookup"><span data-stu-id="091ab-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="091ab-133">針對`greet.proto`，範例先前所述，抽象`GreeterBase`型別，包含虛擬`SayHello`方法產生。</span><span class="sxs-lookup"><span data-stu-id="091ab-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="091ab-134">具象實作`GreeterService`覆寫方法，並實作處理 gRPC 呼叫的邏輯。</span><span class="sxs-lookup"><span data-stu-id="091ab-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="091ab-135">針對用戶端資產產生實體的用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="091ab-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="091ab-136">在中，呼叫 gRPC *.proto*檔案會轉譯成具象類型，可呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="091ab-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="091ab-137">針對`greet.proto`，範例先前所述，具體`GreeterClient`會產生型別。</span><span class="sxs-lookup"><span data-stu-id="091ab-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="091ab-138">呼叫`GreeterClient.SayHello`起始 gRPC 呼叫給伺服器。</span><span class="sxs-lookup"><span data-stu-id="091ab-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

<span data-ttu-id="091ab-139">根據預設，為每個產生伺服器和用戶端的資產 *.proto*檔案中包含`<Protobuf>`項目群組。</span><span class="sxs-lookup"><span data-stu-id="091ab-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="091ab-140">若要確保只有伺服器資產在伺服器專案中，產生`GrpcServices`屬性設為`Server`。</span><span class="sxs-lookup"><span data-stu-id="091ab-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

<span data-ttu-id="091ab-141">同樣地，此屬性設為`Client`用戶端專案中。</span><span class="sxs-lookup"><span data-stu-id="091ab-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="091ab-142">其他資源</span><span class="sxs-lookup"><span data-stu-id="091ab-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
