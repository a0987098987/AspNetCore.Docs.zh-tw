---
title: "ASP.NET Core 2.0 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。"
keywords: "ASP.NET Core, 教學課程, 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a>開始使用 ASP.NET Core

> [!NOTE]
> 這些指示是針對最新版本的 ASP.NET Core。 要從較早版本開始入門嗎？ 請參閱 [1.1 版的本教學課程](xref:getting-started-1.1)。

1. 安裝 [.NET Core](https://microsoft.com/net/core/)。

2. 建立新的 .NET Core 專案。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. 執行應用程式。

   `dotnet run` 命令會在必要時先建置應用程式。

   ```terminal
   dotnet run
   ```

5. 瀏覽至 `http://localhost:5000`。

### <a name="next-steps"></a>後續步驟

如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。

如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。

ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。 如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。
