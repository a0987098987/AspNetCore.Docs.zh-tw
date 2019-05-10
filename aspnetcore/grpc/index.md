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
# <a name="introduction-to-grpc-on-aspnet-core"></a>ASP.NET Core 上的 gRPC 簡介

作者：[John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) 是不限於語言的高效能遠端程序呼叫 (RPC) 架構。 如需詳細的 gRPC 基本概念，請參閱 [gRPC 文件頁面](https://grpc.io/docs/)。

gRPC 的主要優點包括：
* 新式高效能輕量型 RPC 架構。
* 根據預設使用 Protocol Buffers 的合約優先式 API 開發，使您得以進行不限於語言的實作。
* 適用於多種語言的工具，可產生強型別伺服器及用戶端。
* 支援用戶端、伺服器及雙向資料流呼叫。
* 透過 Protobuf 二進位序列化減少網路使用量。

這些優點讓 gRPC 非常適合：
* 首重效率的輕量型微服務。
* 必須使用多種語言進行開發的多語言系統。
* 必須處理資料流要求或回應的點對點即時服務。

雖然目前可在官方 [gRPC 網頁](https://grpc.io/docs/quickstart/csharp.html)上取得 C# 實作，但目前的實作仰賴以 C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)) 撰寫而成的原生程式庫。 目前已在準備根據完全受控的 Kestrel HTTP 伺服器及 ASP.NET Core 堆疊，提供全新實作。 下列文件提供簡介，說明如何使用此全新實作建置 gRPC 服務。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>