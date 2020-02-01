---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 此教學課程時間不長，會使用 ASP.NET Core 建立及執行基本的 Hello World 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 4f7e67e1e422afe3f7e2970e0c40380f065390ac
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928323"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>教學課程：ASP.NET Core 使用者入門

本教學課程說明如何使用 .NET Core CLI 來建立及執行 ASP.NET Core web 應用程式。

您將了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 信任開發憑證。
> * 執行應用程式。
> * 編輯 Razor 頁面。

最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a>必要條件：

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a>建立 Web 應用程式專案

開啟命令殼層，並輸入下列命令：

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

上述命令：

* 建立新的 web 應用程式。  
* `-o aspnetcoreapp` 參數會建立名為*aspnetcoreapp*的目錄，其中包含應用程式的來源檔案。

### <a name="trust-the-development-certificate"></a>信任開發憑證

信任 HTTPS 開發憑證：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

上述命令會顯示以下對話方塊：

![安全性警告對話方塊](~/getting-started/_static/cert.png)

若您同意信任開發憑證，請選取 [是]。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

上述命令會顯示以下訊息：

*已要求信任 HTTPS 開發憑證。如果憑證尚未受到信任，我們會執行下列命令：* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。 若您同意信任開發憑證，請輸入您的密碼。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。

---

如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>執行應用程式

執行下列命令：

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

命令殼層指出應用程式已啟動之後，瀏覽到 [https://localhost:5001](https://localhost:5001)。

## <a name="edit-a-razor-page"></a>編輯 Razor 頁面

開啟*Pages/Index. cshtml* ，並以下列反白顯示的標記修改並儲存頁面：

[!code-cshtml[](sample/index.cshtml?highlight=9)]

流覽至[https://localhost:5001](https://localhost:5001)、重新整理頁面，並確認已顯示變更。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 信任開發憑證。
> * 執行專案。
> * 進行變更。

若要深入了解 ASP.NET Core，請參閱簡介中的建議學習路徑：

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
