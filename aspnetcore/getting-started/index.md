---
title: ASP.NET Core 使用者入門
author: rick-anderson
description: 使用 ASP.NET Core 建立並執行簡單 Hello World 應用程式的快速教學課程。
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: getting-started
ms.openlocfilehash: 29a328b610b0a6e1616cd6ebc70a8fa3e515eb92
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861702"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>教學課程：ASP.NET Core 使用者入門

本教學課程示範如何使用 .NET Core 命令列介面來建立 ASP.NET Core Web 應用程式。

您將了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 啟用本機 HTTPS。
> * 執行應用程式。
> * 編輯 Razor 頁面。

最後，您會在本機電腦上執行一個運作正常的 Web 應用程式。

![Web 應用程式首頁](_static/home-page.png)

## <a name="prerequisites"></a>必要條件

安裝 [!INCLUDE [](~/includes/2.1-SDK.md)]。

## <a name="create-a-web-app-project"></a>建立 Web 應用程式專案

開啟命令殼層，並輸入下列命令：

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>啟用本機 HTTPS

信任 HTTPS 開發憑證：

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

上述命令會顯示以下對話方塊：

![安全性警告對話方塊](_static/cert.png)

若您同意信任開發憑證，請選取 [是]。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

上述命令會顯示以下訊息：

已要求信任 HTTPS 開發憑證。若憑證尚未受到信任，我們會執行下列命令：*`'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`。  
*此命令可能會提示您提供密碼，以在系統金鑰鏈上安裝憑證。

密碼：*

若您同意信任開發憑證，請輸入您的密碼。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

請參閱您 Linux 發行版本的文件，來了解如何信任 HTTPS 開發憑證。

---

## <a name="run-the-app"></a>執行應用程式

執行下列命令：

```console
cd aspnetcoreapp
dotnet run
```

瀏覽至 [https://localhost:5001](https://localhost:5001)。 按一下 [接受] 以接受隱私權與 Cookie 原則。 此應用程式不會保留個人資訊。

## <a name="edit-a-razor-page"></a>編輯 Razor 頁面

開啟 *Pages/About.cshtml*，然後以下列醒目提示的標記修改頁面：

[!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

瀏覽到 [https://localhost:5001/About](https://localhost:5001/About) 並驗證已顯示變更。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式專案。
> * 啟用本機 HTTPS。
> * 執行專案。
> * 進行變更。

若要深入了解 ASP.NET Core，請參閱簡介：

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> 我們正為規劃中的 ASP.NET Core 目錄新結構測試其可用性。 如果您有幾分鐘的時間可以嘗試在目前或建議的目錄中尋找七個不同主題，請[按一下這裡參加研究](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82)。
