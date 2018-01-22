---
title: "ASP.NET Core 1.1 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 1.1 建立並執行簡單 Hello World 應用程式的快速教學課程。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: 7f178618508e1a1e9c49d8ace619b9f942998dae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-aspnet-core-11"></a>ASP.NET Core 1.1 使用者入門

> [!NOTE]
> 這些指示是針對 ASP.NET Core 1.1。 要尋找最新版本？ 請參閱[目前版本的本教學課程](xref:getting-started)。

1. 從 [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 下載頁面](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md)安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。

2. 為新的 .NET Core 專案建立資料夾。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. 如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. 建立新的 .NET Core 專案。

   ```terminal
   dotnet new web
   ```
   
3.  還原套件。

    ```terminal
    dotnet restore
    ```

4. 執行應用程式。

   `dotnet run` 命令會在必要時先建置應用程式。

   ```terminal
   dotnet run
   ```

5. 瀏覽至 `http://localhost:5000`。

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>後續步驟

如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。

如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。

ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。 如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。
