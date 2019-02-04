---
title: 教學課程：開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: bec5838c2efaffb933828260eaf1a840ff202140
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667761"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>教學課程：開始使用 ASP.NET Core 中的 Razor Pages

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

這是本系列的第一個教學課程。 [本系列](xref:tutorials/razor-pages/index)將教導您建置 ASP.NET Core Razor Pages Web 應用程式的基本概念。

[!INCLUDE[](~/includes/advancedRP.md)]

在本系列結束時，您將會有一個可管理電影資料庫的應用程式。  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

在本教學課程中，您將：

> [!div class="checklist"]
> * 建立 Razor Pages Web 應用程式。
> * 執行應用程式。
> * 檢查專案檔。

在本教學課程結束時，您將會有一個運作正常的 Razor Pages Web 應用程式，並將在稍後的教學課程中以此為建置基礎。

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>建立 Razor 頁面 Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。

* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，以便在您複製並貼上程式碼時，名稱空間會相符。

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* 在下拉式清單中選取 [ASP.NET Core 2.2]，然後選取 [Web 應用程式]。

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  下列起始專案會隨即建立：

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。

* 將目錄 (`cd`) 變更為其中包含專案的資料夾。

* 執行下列命令：

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * `dotnet new` 命令會在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。
  * `code` 命令會在新的 Visual Studio Code 執行個體中開啟 *RazorPagesMovie* 資料夾。

  這會顯示對話方塊並指出：**'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**

* 選取 [是]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

從終端機中，執行下列命令：

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立 Razor Pages 專案。

## <a name="open-the-project"></a>開啟專案

從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。

<!-- End of VS tabs -->

---

## <a name="run-the-web-app"></a>執行 Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 按 Ctrl+F5 即可執行而不使用偵錯工具。

  Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5001。 當您執行應用程式時，會看到不同的連接埠編號。
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 按 **Ctrl-F5** 即可執行而不使用偵錯工具。

  Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後巡覽至 `http://localhost:5001`。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

選取 [執行] > [啟動但不偵錯] 來啟動應用程式。 Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。

<!-- End of VS tabs -->

---

* 在應用程式的首頁上，選取 [接受] 同意追蹤。

  此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  下圖顯示您同意追蹤之後的應用程式：

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a>檢查專案檔

以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。

### <a name="pages-folder"></a>Pages 資料夾

包含 Razor 頁面和支援檔案。 每個 Razor 頁面都是一組檔案：

* *.cshtml* 檔案，其中包含 C# 程式碼的 HTML 標記 (使用 Razor 語法)。
* *.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。

支援檔案的名稱以底線開頭。 例如，*_Layout.cshtml* 檔案會設定所有頁面通用的 UI 元素。 此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。 如需詳細資訊，請參閱<xref:mvc/views/layout>。


### <a name="wwwroot-folder"></a>wwwroot 資料夾

包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。 如需詳細資訊，請參閱<xref:fundamentals/static-files>。

### <a name="appsettingsjson"></a>appSettings.json

包含組態資料，例如連接字串。 如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。

### <a name="programcs"></a>Program.cs

包含程式的進入點。 如需詳細資訊，請參閱<xref:fundamentals/host/web-host>。

### <a name="startupcs"></a>Startup.cs

包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。 如需詳細資訊，請參閱<xref:fundamentals/startup>。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 Razor Pages Web 應用程式。
> * 執行應用程式。
> * 檢查專案檔。

前進到系列中的下一個教學課程：

> [!div class="step-by-step"]
> [新增模型](xref:tutorials/razor-pages/model)
