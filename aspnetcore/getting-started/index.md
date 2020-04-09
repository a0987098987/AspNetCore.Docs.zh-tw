---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 此教學課程時間不長，會使用 ASP.NET Core 建立及執行基本的 Hello World 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 86a0c8d017138a949fddc0356f3de548d368a4c0
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80417602"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>教學課程：ASP.NET Core 使用者入門

本教學示範如何使用 .NET核心 CLI 創建和執行ASP.NET核心 Web 應用。

您將學習如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 信任開發憑證。
> * 執行應用程式。
> * 編輯 Razor 頁面。

最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a>建立 Web 應用程式專案

開啟命令殼層，並輸入下列命令：

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

上述命令會：

* 創建新的 Web 應用。  
* 參數`-o aspnetcoreapp`創建一個名為*aspnetcoreapp*的目錄,該目錄具有應用的源檔。

### <a name="trust-the-development-certificate"></a>信任開發憑證

信任 HTTPS 開發憑證：

# <a name="windows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

上述命令會顯示以下對話方塊：

![安全性警告對話方塊](~/getting-started/_static/cert.png)

若您同意信任開發憑證，請選取 [是]****。

# <a name="macos"></a>[macOS](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

上述命令會顯示以下訊息：

*請求信任 HTTPS 開發證書。如果憑證尚未受信任,我們將執行以下命令:*`'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。 若您同意信任開發憑證，請輸入您的密碼。

# <a name="linux"></a>[Linux](#tab/linux)

請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。

---

如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>執行應用程式

執行下列命令：

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

命令殼層指出應用程式已啟動之後，瀏覽到 `https://localhost:5001`。

## <a name="edit-a-razor-page"></a>編輯 Razor 頁面

開啟*頁面/Index.cshtml,* 使用以下突顯的標記修改並儲存頁面:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

瀏覽`https://localhost:5001`到,刷新頁面,並驗證顯示更改。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 信任開發憑證。
> * 執行專案。
> * 進行變更。

若要深入了解 ASP.NET Core，請參閱簡介中的建議學習路徑：

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
