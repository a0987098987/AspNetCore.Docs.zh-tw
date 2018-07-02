---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296765"
---
# <a name="get-started-with-aspnet-core"></a>ASP.NET Core 使用者入門

::: moniker range=">= aspnetcore-2.1"

1. 安裝 [!INCLUDE[](~/includes/2.1-SDK.md)]。

2. 建立 ASP.NET Core 專案。 開啟命令殼層，並輸入下列命令：

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. 信任 HTTPS 開發憑證：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. 執行應用程式：

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. 瀏覽至 [http://localhost:5001](http://localhost:5001)。  按一下 [接受] 以接受隱私權與 Cookie 原則。 此應用程式不會保留個人資訊。

6. 開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. 瀏覽到 [http://localhost:5001/About](http://localhost:5001/About) 並驗證已顯示變更。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. 安裝 [!INCLUDE[](~/includes/net-core-sdk-download-link.md)]。

2. 建立新的 ASP.NET Core 專案。

   開啟命令殼層。 輸入下列命令：

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. 使用下列命令來執行應用程式：

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. 瀏覽至 [http://localhost:5000](http://localhost:5000)。

5. 開啟 *Pages/About.cshtml* 並將頁面顯示訊息修改為 "Hello, world! 伺服器的時間為 @DateTime.Now“ ：

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. 瀏覽至 [http://localhost:5000/About](http://localhost:5000/About) 並確認所做的變更。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. 從 [.NET Core All Downloads](https://www.microsoft.com/net/download/all) (.NET Core 所有下載) 頁面安裝適用於 SDK 1.0.4 的 .NET Core **SDK 安裝程式**。

2. 為新的 ASP.NET Core 專案建立資料夾。

   開啟命令殼層。 輸入下列命令：

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. 如果您已在電腦上安裝較新版的 SDK，請建立 *global.json* 檔案來選取 1.0.4 SDK。

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. 建立新的 ASP.NET Core 專案。

   ```console
   dotnet new web
   ```

5. 還原套件。

    ```console
    dotnet restore
    ```

6. 執行應用程式。

   ```console
   dotnet run
   ```

   [dotnet run](/dotnet/core/tools/dotnet-run) 命令會在必要時先建置應用程式。

7. 瀏覽至 `http://localhost:5000`。

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
