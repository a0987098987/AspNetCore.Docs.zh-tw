---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core 使用者入門

> [!NOTE]
> 這些指示是針對最新版本的 ASP.NET Core。 如需本文件的 1.1 版本，請參閱 [ASP.NET Core 1.1 使用者入門](xref:getting-started-1.1)。

1. 安裝 [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]。

2. 建立新的 .NET Core 專案。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。 輸入下列命令：

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. 執行應用程式。

    使用以下命令來執行應用程式：

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. 瀏覽至 [http://localhost:5000](http://localhost:5000)

5. 開啟 <em>Pages/About.cshtml</em> 並將頁面顯示訊息修改為 "Hello, world! The time on the server is @DateTime.Now ":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. 瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。

### <a name="next-steps"></a>後續步驟

如需使用者入門教學課程，請參閱 [ASP.NET Core 教學課程](tutorials/index.md)。

如需 ASP.NET Core 概念與架構的簡介，請參閱 [ASP.NET Core 簡介](index.md)和 [ASP.NET Core 基礎概念](fundamentals/index.md)。

ASP.NET Core 應用程式可以使用 .NET Core 或 .NET Framework 基底類別庫和執行階段。 如需詳細資訊，請參閱[在 .NET Core 和 .NET Framework 之間進行選擇](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)。
