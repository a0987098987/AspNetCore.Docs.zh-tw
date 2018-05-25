---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core 使用者入門

::: moniker range=">= aspnetcore-2.0"

1. 安裝 [!INCLUDE[](~/includes/net-core-sdk-download-link.md)]。

2. 建立新的 .NET Core 專案。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。 輸入下列命令：

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. 使用下列命令來執行應用程式：

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. 瀏覽至 [http://localhost:5000](http://localhost:5000)。

5. 開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world! 伺服器的時間為 @DateTime.Now“ ：

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. 瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. 從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。

2. 為新的 .NET Core 專案建立資料夾。

   在 macOS 和 Linux 上，開啟終端機視窗。 在 Windows 上，開啟命令提示字元。

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. 如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. 建立新的 .NET Core 專案。

   ```terminal
   dotnet new web
   ```

5. 還原套件。

    ```terminal
    dotnet restore
    ```

6. 執行應用程式。

   ```terminal
   dotnet run
   ```

   [dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。

7. 瀏覽至 `http://localhost:5000`。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end