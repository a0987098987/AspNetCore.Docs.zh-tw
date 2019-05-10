---
title: ASP.NET Core 上的 gRPC 簡介
author: juntaoluo
description: 了解搭配 Kestrel 伺服器及 ASP.NET Core 堆疊的 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085544"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="5d6cb-103">ASP.NET Core 上的 gRPC 簡介</span><span class="sxs-lookup"><span data-stu-id="5d6cb-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="5d6cb-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="5d6cb-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="5d6cb-105">[gRPC](https://grpc.io/docs/guides/) 是不限於語言的高效能遠端程序呼叫 (RPC) 架構。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="5d6cb-106">如需詳細的 gRPC 基本概念，請參閱 [gRPC 文件頁面](https://grpc.io/docs/)。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="5d6cb-107">gRPC 的主要優點包括：</span><span class="sxs-lookup"><span data-stu-id="5d6cb-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="5d6cb-108">新式高效能輕量型 RPC 架構。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="5d6cb-109">根據預設使用 Protocol Buffers 的合約優先式 API 開發，使您得以進行不限於語言的實作。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="5d6cb-110">適用於多種語言的工具，可產生強型別伺服器及用戶端。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="5d6cb-111">支援用戶端、伺服器及雙向資料流呼叫。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="5d6cb-112">透過 Protobuf 二進位序列化減少網路使用量。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="5d6cb-113">這些優點讓 gRPC 非常適合：</span><span class="sxs-lookup"><span data-stu-id="5d6cb-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="5d6cb-114">首重效率的輕量型微服務。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="5d6cb-115">必須使用多種語言進行開發的多語言系統。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="5d6cb-116">必須處理資料流要求或回應的點對點即時服務。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="5d6cb-117">雖然目前可在官方 [gRPC 網頁](https://grpc.io/docs/quickstart/csharp.html)上取得 C# 實作，但目前的實作仰賴以 C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)) 撰寫而成的原生程式庫。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="5d6cb-118">目前已在準備根據完全受控的 Kestrel HTTP 伺服器及 ASP.NET Core 堆疊，提供全新實作。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="5d6cb-119">下列文件提供簡介，說明如何使用此全新實作建置 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="5d6cb-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d6cb-120">其他資源</span><span class="sxs-lookup"><span data-stu-id="5d6cb-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>