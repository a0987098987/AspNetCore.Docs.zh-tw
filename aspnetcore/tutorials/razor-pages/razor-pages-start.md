---
title: 教學課程：開始使用 Razor ASP.NET Core 中的頁面
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 Razor ASP.NET Core 中的頁面。 瞭解如何建立模型、產生頁面的程式碼 Razor 、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證，以及使用遷移來更新模型。
ms.author: riande
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 77b230f14b4eef60d771daf8fe09288a9dd3c69c
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86212996"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>教學課程：開始使用 Razor ASP.NET Core 中的頁面

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"
這是一系列的第一個教學課程，會教您建立 ASP.NET Core Razor 頁面 web 應用程式的基本概念。

[!INCLUDE[](~/includes/advancedRP.md)]

在本系列結束時，您將會有一個可管理電影資料庫的應用程式。  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

在本教學課程中，您：

> [!div class="checklist"]
> * 建立 Razor 頁面 web 應用程式。
> * 執行應用程式。
> * 檢查專案檔。

在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a>先決條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a>建立 Razor 頁面 web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**[專案]** > **** 。
* 建立新的 ASP.NET Core Web 應用程式並選取 [下一步]****。
  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)
* 將專案命名為** Razor PagesMovie**。 請務必將專案命名為* Razor PagesMovie* ，如此一來，當您複製並貼上程式碼時，命名空間將會相符。
  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* 在下拉式清單 [ **Web 應用程式**] 中選取 [ **ASP.NET Core 3.1** ]，然後選取 [**建立**]。

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  下列起始專案會隨即建立：

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。

* 變更為將包含專案的目錄 (`cd`)。

* 執行下列命令：

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * `dotnet new`命令會 Razor 在* Razor PagesMovie*資料夾中建立新的頁面專案。
  * `code`命令會在 Visual Studio Code 的目前實例中開啟* Razor PagesMovie*資料夾。

* 狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' PagesMovie ' 中遺漏了 debug Razor 。要新增它們嗎？** 選取 [是]。

  *.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案]** [新增解決方案]** > ****。

  ![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* 在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。 在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。

  ![macOS web 應用程式範本選取專案](razor-pages-start/_static/web_app_template_vsmac.png)

* 在 [**設定您的新 Web 應用程式**] 對話方塊中：

  * 確認 [**驗證**] 已設為 [**無驗證**]。
  * 如果提供選取**目標 Framework**的選項，請選取最新的3.x 版。

  選取 [下一步]。

* 將專案命名為** Razor PagesMovie**，然後選取 [**建立**]。

  ![macOS 將專案命名為](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>執行應用程式

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a>檢查專案檔

以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。

### <a name="pages-folder"></a>Pages 資料夾

包含 Razor 頁面和支援檔案。 每個 Razor 頁面都是一對檔案：

* 包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。
* *.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。

支援檔案的名稱以底線開頭。 例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。 此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。 如需詳細資訊，請參閱 <xref:mvc/views/layout> 。

### <a name="wwwroot-folder"></a>wwwroot 資料夾

包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。 如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。

### <a name="appsettingsjson"></a>appSettings.json

包含組態資料，例如連接字串。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。

### <a name="programcs"></a>Program.cs

包含程式的進入點。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。

### <a name="startupcs"></a>Startup.cs

包含設定應用程式行為的程式碼。 如需詳細資訊，請參閱 <xref:fundamentals/startup> 。

## <a name="next-steps"></a>後續步驟

前進到系列中的下一個教學課程：

> [!div class="step-by-step"]
> [新增模型](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

這是本系列的第一個教學課程。 [本系列將](xref:tutorials/razor-pages/index)教您建立 ASP.NET Core Razor Pages web 應用程式的基本概念。

[!INCLUDE[](~/includes/advancedRP.md)]

在本系列結束時，您將會有一個可管理電影資料庫的應用程式。  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

在本教學課程中，您：

> [!div class="checklist"]
> * 建立 Razor 頁面 web 應用程式。
> * 執行應用程式。
> * 檢查專案檔。

在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a>先決條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a>建立 Razor 頁面 web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**[專案]** > **** 。

* 建立新的 ASP.NET Core Web 應用程式並選取 [下一步]****。

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* 將專案命名為** Razor PagesMovie**。 請務必將專案命名為* Razor PagesMovie* ，如此一來，當您複製並貼上程式碼時，命名空間將會相符。

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* 在下拉式清單中選取 [ASP.NET Core 2.2]****，然後選取 [Web 應用程式]**** 及 [建立]****。

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  下列起始專案會隨即建立：

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。

* 變更為將包含專案的目錄 (`cd`)。

* 執行下列命令：

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * `dotnet new`命令會 Razor 在* Razor PagesMovie*資料夾中建立新的頁面專案。
  * `code`命令會在 Visual Studio Code 的目前實例中開啟* Razor PagesMovie*資料夾。

* 狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' PagesMovie ' 中遺漏了 debug Razor 。要新增它們嗎？** 選取 [是]。

  *.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案]** [新增解決方案]** > ****。

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* 在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。 在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。

* 在 [**設定您的新 Web 應用程式**] 對話方塊中：

  * 確認 [**驗證**] 已設為 [**無驗證**]。
  * 如果提供選取**目標 Framework**的選項，請選取最新的2.x 版。

  選取 [下一步]。

* 將專案命名為** Razor PagesMovie**，然後選取 [**建立**]。

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>執行應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 按 Ctrl+F5 即可執行而不使用偵錯工具。

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。 當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。

* 在應用程式的首頁上，選取 [接受]**** 同意追蹤。

  此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  下圖顯示您同意追蹤之後的應用程式：

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* 按 **Ctrl-F5** 即可執行而不使用偵錯工具。

  Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。

* 在應用程式的首頁上，選取 [接受]**** 同意追蹤。

  此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  下圖顯示您同意追蹤之後的應用程式：

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* 按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。

  Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。

* 在應用程式的首頁上，選取 [接受]**** 同意追蹤。

  此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  下圖顯示您同意追蹤之後的應用程式：

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a>檢查專案檔

以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。

### <a name="pages-folder"></a>Pages 資料夾

包含 Razor 頁面和支援檔案。 每個 Razor 頁面都是一對檔案：

* 包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。
* *.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。

支援檔案的名稱以底線開頭。 例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。 此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。 如需詳細資訊，請參閱 <xref:mvc/views/layout> 。

### <a name="wwwroot-folder"></a>wwwroot 資料夾

包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。 如需詳細資訊，請參閱 <xref:fundamentals/static-files> 。

### <a name="appsettingsjson"></a>appSettings.json

包含組態資料，例如連接字串。 如需詳細資訊，請參閱 <xref:fundamentals/configuration/index> 。

### <a name="programcs"></a>Program.cs

包含程式的進入點。 如需詳細資訊，請參閱 <xref:fundamentals/host/generic-host> 。

### <a name="startupcs"></a>Startup.cs

包含設定應用程式行為的程式碼，例如是否需要同意使用 Cookie。 如需詳細資訊，請參閱 <xref:fundamentals/startup> 。

## <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a>後續步驟

前進到系列中的下一個教學課程：

> [!div class="step-by-step"]
> [新增模型](xref:tutorials/razor-pages/model)

::: moniker-end
