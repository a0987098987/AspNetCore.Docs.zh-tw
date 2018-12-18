---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: 這一系列的教學課程會示範如何使用 ASP.NET Core 中的 Razor Pages。 了解如何建立模型、產生 Razor Pages 的程式碼、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證以及使用移轉來更新模型。
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861624"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>教學課程：開始使用 ASP.NET Core 中的 Razor Pages

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。

此應用程式會管理電影標題的資料庫。 您將學習如何：

> [!div class="checklist"]
> * 建立 Razor Pages Web 應用程式。
> * 新增並建構模型。
> * 使用資料庫。
> * 新增搜尋和驗證。

結束時，您將會有一個可管理及顯示電影標題項目的應用程式。

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **RazorPagesMovie**。 請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。
 ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* 在下拉式清單中選取 [ASP.NET Core 2.2]，然後選取 [Web 應用程式]。

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  下列起始專案會隨即建立：

  ![底下提供說明，包括方案總管](razor-pages-start/_static/se2.2.png)

* 按 **Ctrl-F5** 即可執行而不使用偵錯工具。

  Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。 在上述影像中，連接埠編號為 5001。 當您執行應用程式時，會看到不同的連接埠編號。

  使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為其中包含專案的資料夾。
* 執行下列命令：

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * 這會顯示對話方塊並指出：**'RazorPagesMovie' 中遺漏了建置和偵錯的必要資產。新增它們嗎？**  選取 [是]

  * `dotnet new webapp -o RazorPagesMovie`：在 *RazorPagesMovie* 資料夾中建立新的 Razor Pages 專案。
  * `code -r RazorPagesMovie`：在 Visual Studio Code 中載入 *RazorPagesMovie.csproj* 專案檔。

### <a name="launch-the-app"></a>啟動應用程式

* 按 **Ctrl-F5** 即可執行而不使用偵錯工具。

  Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後巡覽至 `http://localhost:5001`。 位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。

  使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

從終端機中，執行下列命令：

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

上述命令使用 [.NET Core CLI](/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。 將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。

## <a name="open-the-project"></a>開啟專案

按 Ctrl + C 來關閉應用程式。

從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。

### <a name="launch-the-app"></a>啟動應用程式

選取 [執行] > [啟動但不偵錯] 來啟動應用程式。 Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。

<!-- End of VS tabs -->

---

* 選取 [接受] 同意追蹤。 此應用程式不會追踪個人資訊。 範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  下圖顯示接受追蹤之後的應用程式：

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>專案檔和資料夾

下表列出專案中的檔案和資料夾。 在這部分的教學課程中，*Startup.cs* 檔案是需要了解的最重要項目。 您不需要檢閱以下提供的每個連結。 當您需要專案中的檔案或資料夾的詳細資訊時，便會提供連結作為參考。

| 檔案或資料夾              | 用途 |
| ----------------- | ------------ |
| *wwwroot* | 包含靜態檔案。 請參閱[靜態檔案](xref:fundamentals/static-files)。 |
| *頁面* | [Razor 頁面](xref:razor-pages/index)的資料夾。 |
| *appsettings.json* | [組態](xref:fundamentals/configuration/index) |
| *Program.cs* | [裝載](xref:fundamentals/host/index) ASP.NET Core 應用程式。|
| *Startup.cs* | 設定服務和要求管線。 請參閱 [Startup](xref:fundamentals/startup)。|

### <a name="the-pages-folder"></a>Pages 資料夾

*_Layout.cshtml* 檔案包含通用的 HTML 元素 (指令碼和樣式表)，並設定應用程式的配置。 例如，當您按一下 **RazorPagesMovie**、**Home** 或 **Privacy** 時，會看到相同的元素。 通用元素包含頂端的導覽功能表和視窗底部的標頭。 如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。

*_ViewImports.cshtml* 檔案包含匯入至每個 Razor 頁面的 Razor 指示詞。 如需詳細資訊，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)。

*_ViewStart.cshtml* 會設定 Razor 頁面 `Layout` 屬性，以使用 *_Layout.cshtml* 檔案。 如需詳細資訊，請參閱 [Layout](xref:mvc/views/layout)。

*_ValidationScriptsPartial.cshtml* 檔案提供 [jQuery](https://jquery.com/) 驗證指令碼的參考。 稍後在教學課程中新增 `Create` 和 `Edit` 頁面時，會使用 *_ValidationScriptsPartial.cshtml* 檔案。

`Index`、`Error` 和 `Privacy` 頁面的提供目的如下：

* `Index`：啟動應用程式。
* `Error`：顯示錯誤資訊。
* `Privacy`：指定網站隱私權原則的詳細資料。

在本教學課程中，不會使用上述頁面。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>使用 F7 鍵在 Razor 頁面與 PageModel 之間進行切換

F7 鍵可在 Razor 頁面 (*\*.cshtml* 檔案) 與 C# 檔案 (*\*.cshtml.cs*) 之間進行切換。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

依照慣例，Razor 頁面 (*\*.cshtml* 檔案) 與相關聯的 `PageModel` 具有相同的根檔案名稱。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

依照慣例，Razor 頁面 (*\*.cshtml* 檔案) 與相關聯的 `PageModel` 具有相同的根檔案名稱。

---

> [!div class="step-by-step"]
> [下一步：新增模型](xref:tutorials/razor-pages/model)