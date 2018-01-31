---
title: "Mac、Linux 或 Windows 上的 ASP.NET Core MVC 簡介"
author: rick-anderson
description: "Mac、Linux 和 Windows 上的 ASP.NET Core MVC 與 Visual Studio Code 使用者入門"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4771555b66f328a819f17a32eb3959f9ecf33d44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Mac、Linux 或 Windows 上的 ASP.NET Core MVC 使用者入門

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程會讓您了解使用 [Visual Studio Code](https://code.visualstudio.com) (VS Code) 建置 ASP.NET Core MVC Web 應用程式的基本知識。 本教學課程假設您熟悉 VS Code。 如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。 

[!INCLUDE[consider RP](../../includes/razor.md)]

本教學課程有 3 個版本：

* macOS：[使用 Visual Studio for Mac 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows：[使用 Visual Studio 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app/start-mvc)
* macOS、Linux 和 Windows：[使用 Visual Studio Code 建立 ASP.NET Core MVC 應用程式](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>安裝 VS Code 和 .NET Core

本教學課程需要 [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。 若要了解 ASP.NET Core 1.1 版，請參閱[此 PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf)。

安裝下列項目：

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更新版本。
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-web-app-with-dotnet"></a>使用 dotnet 建立 Web 應用程式

從終端機中，執行下列命令：

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

在 Visual Studio Code (VS Code) 中，開啟 *MvcMovie* 資料夾，然後選取 *Startup.cs* 檔案。

- 針對下列 [警告] 訊息選取 [是]：「'MvcMovie' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？」
- 針對下列 [資訊] 訊息選取 [還原]：「有未解析的相依性」。

![VS Code 與警告：'MvcMovie' 中遺漏了建置和偵錯的必要資產。 新增它們嗎？ 不要再詢問、現在不要、是以及資訊 - 有未解析的相依性 - 還原 - 關閉](../web-api-vsc/_static/vsc_restore.png)

按 [偵錯] (F5) 以建置並執行程式。

![執行中的應用程式](../first-mvc-app/start-mvc/_static/1.png)

VS Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel) 網頁伺服器並執行應用程式。 請注意，位址列會顯示 `localhost:5000`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。

預設範本提供您作用中的**首頁、關於**和**連絡人**連結。 上圖的瀏覽器不會顯示這些連結。 根據瀏覽器大小，您可能需要按一下巡覽圖示來顯示連結。

![右上角的瀏覽圖示](../first-mvc-app/start-mvc/_static/2.png)

在本教學課程的下一個部分中，我們會了解 MVC，並開始撰寫一些程式碼。

## <a name="visual-studio-code-help"></a>Visual Studio Code 說明

- [快速入門](https://code.visualstudio.com/docs)
- [偵錯](https://code.visualstudio.com/docs/editor/debugging)
- [整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [鍵盤快速鍵](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows 鍵盤快速鍵](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[下一步 - 新增控制器](adding-controller.md)
