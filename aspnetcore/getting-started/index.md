---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 此教學課程時間不長，會使用 ASP.NET Core 建立及執行基本的 Hello World 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 5/15/2019
uid: getting-started
ms.openlocfilehash: 9227dcfbc84376d9d73bc6fc0dd76085779acae1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610318"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>教學課程：ASP.NET Core 使用者入門

此教學課程示範如何使用.NET Core 命令列介面建立及執行 ASP.NET Core Web 應用程式。

您將了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 信任開發憑證。
> * 執行應用程式。
> * 編輯 Razor 頁面。

最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a>必要條件

* [.NET Core 2.2 SDK](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>建立 Web 應用程式專案

開啟命令殼層，並輸入下列命令：

```console
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a>信任開發憑證

信任 HTTPS 開發憑證：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

上述命令會顯示以下對話方塊：

![安全性警告對話方塊](~/getting-started/_static/cert.png)

若您同意信任開發憑證，請選取 [是]。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

上述命令會顯示以下訊息：

*已要求信任 HTTPS 開發憑證。若憑證尚未受到信任，我們會執行下列命令：* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。 若您同意信任開發憑證，請輸入您的密碼。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

若是適用於 Linux 的 Windows 子系統，請參閱[信任來自適用於 Linux 的 Windows 子系統的 HTTPS 憑證](xref:security/enforcing-ssl#wsl)。

請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。

---

如需詳細資訊，請參閱[信任 ASP.NET Core HTTPS 開發憑證](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>執行應用程式

執行下列命令：

```console
cd aspnetcoreapp
dotnet run
```

命令殼層指出應用程式已啟動之後，瀏覽到 [https://localhost:5001](https://localhost:5001)。 按一下 [接受] 以接受隱私權與 Cookie 原則。 此應用程式不會保留個人資訊。

## <a name="edit-a-razor-page"></a>編輯 Razor 頁面

開啟 *Pages/Index.cshtml*，然後以下列醒目提示的標記修改頁面：

[!code-cshtml[](sample/index.cshtml?highlight=9)]

瀏覽到 [https://localhost:5001](https://localhost:5001) 並驗證已顯示變更。

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
