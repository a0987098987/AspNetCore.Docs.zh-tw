---
title: "ASP.NET Core 2.0 使用者入門"
author: rick-anderson
description: "使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。"
keywords: "ASP.NET Core, 教學課程, 使用者入門"
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core 使用者入門

> [!NOTE]
> 這些指示是針對最新版本的 ASP.NET Core。 要從較早版本開始入門嗎？ 請參閱 [1.1 版的本教學課程](xref:getting-started-1.1)。

1. 安裝 [.NET Core](https://www.microsoft.com/net/core/)。

2. 建立新的 .NET Core 專案。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。 輸入下列命令：

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. 執行應用程式。

    使用以下命令來執行應用程式：

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. 瀏覽至 [http://localhost:5000](http://localhost:5000)

6. 開啟 *Pages/About.cshtml* 並修改頁面以顯示訊息 "Hello, world! 伺服器的時間為 @DateTime.Now "：

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. 瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並驗證變更。

### <a name="next-steps"></a>後續步驟

如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。

如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。

ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。 如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。
