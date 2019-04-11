---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何建立可重複使用 Razor UI，使用 ASP.NET Core 中的類別庫中的部分檢視。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
ms.custom: seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dc7db9483f2d75fe79ed9a9806f944e4f2a05a9b
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265338"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>建立可重複使用 UI 在 ASP.NET Core 中使用 Razor 類別庫專案

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

Razor 檢視、頁面、控制器、頁面模型、[檢視元件](xref:mvc/views/view-components)和資料模型可以建置到 Razor 類別庫 (RCL) 中。 RCL 可以封裝和重複使用。 應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。 在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。

這項功能需要 [!INCLUDE[](~/includes/2.1-SDK.md)]

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>建立包含 Razor UI 的類別庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 選取 [ASP.NET Core Web 應用程式]。
* 命名程式庫 (例如，"RazorClassLib") > [確定]。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Razor 類別庫] > [確定]。

Razor 類別庫包含下列專案檔：

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行 `dotnet new razorclasslib`。 例如: 

```console
dotnet new razorclasslib -o RazorUIClassLib
```

如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。

---

新增 Razor 檔案至 RCL。

ASP.NET Core 範本假設 RCL 內容位於*領域*資料夾。 請參閱[RCL 網頁版面配置](#afs)建立中的內容會公開 RCL`~/Pages`而非`~/Areas/Pages`。

## <a name="referencing-razor-class-library-content"></a>參考 Razor 類別庫內容

RCL 可以由下列各項參考：

* NuGet 套件。 請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。
* *{ProjectName}.csproj*。 請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>逐步解說：建立 Razor 類別庫專案，並使用從 Razor 頁面專案

您可以下載[完整專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。 下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。 您可以在[此 GitHub 問題](https://github.com/aspnet/Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。

### <a name="test-the-download-app"></a>測試下載應用程式

如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-a-razor-class-library)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio 中開啟 *.sln* 檔案。 執行應用程式。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。

```console
dotnet build
```

移至 *WebApp1* 目錄並執行應用程式：

```console
dotnet run
```

---

遵循[測試 WebApp1](#test) 中的指示執行。

## <a name="create-a-razor-class-library"></a>建立 Razor 類別庫

在本節中，會建立 Razor 類別庫 (RCL)。 會有 Razor 檔案新增至 RCL。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 RCL 專案：

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 選取 [ASP.NET Core Web 應用程式]。
* 將應用程式命名**RazorUIClassLib** > **確定**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Razor 類別庫] > [確定]。
* 新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行下列命令：

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

上述命令：

* 建立 `RazorUIClassLib` Razor 類別庫 (RCL)。
* 建立 Razor _Message 頁面，並將它新增至 RCL。 `-np` 參數會建立頁面而不使用 `PageModel`。
* 會建立[_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view)檔案，並將它新增至 RCL。

*_ViewStart.cshtml*檔案，才能使用 Razor 頁面專案 （這會新增下一節） 的配置。

---

### <a name="add-razor-files-and-folders-to-the-project"></a>加入專案中的 Razor 檔案和資料夾

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。 您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。 例如: 

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

如需詳細資訊 *_ViewImports.cshtml*，請參閱[匯入共用指示詞](xref:mvc/views/layout#importing-shared-directives)

* 建置類別庫，以確認沒有任何編譯器錯誤：

```console
dotnet build RazorUIClassLib
```

建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。 *RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>從 Razor 頁面專案使用 Razor UI 程式庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 Razor 頁面 Web 應用程式：

* 在 [方案總管] 中，以滑鼠右鍵按一下解決方案 > [新增] >  [新增專案]。
* 選取 [ASP.NET Core Web 應用程式]。
* 將應用程式命名為 **WebApp1**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Web 應用程式] > [確定]。

* 從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]。
* 從方案總管，以滑鼠右鍵按一下 **WebApp1**，然後選取 [建置相依性] > [專案相依性]。
* 核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。
* 從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [新增] > [參考]。
* 在 [參考管理員] 對話方塊中，核取 **RazorUIClassLib** > [確定]。

執行應用程式。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

建立 Razor 頁面的 Web 應用程式和方案檔，其中包含 Razor 頁面應用程式和 Razor 類別庫：

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

建置和執行 Web 應用程式：

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>測試 WebApp1

確認正在使用 Razor UI 類別庫。

* 瀏覽至 `/MyFeature/Page1`。

## <a name="override-views-partial-views-and-pages"></a>覆寫檢視、部分檢視和頁面

在 Web 應用程式和 Razor 類別庫中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。 例如，新增*WebApp1/Areas/MyFeature/Pages/Page1.cshtml* WebApp1，以和 Page1 WebApp1 中的將會優先於 Razor 類別庫中的 Page1。

在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。

將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。 更新標記來指示新的位置。 建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>RCL 網頁版面配置

參考 RCL 內容，就好像它是 web 應用程式的一部分*頁*資料夾中，建立 RCL 專案檔案結構如下：

* *RazorUIClassLib/頁面*
* *RazorUIClassLib/網頁/共用*

假設*RazorUIClassLib/網頁/Shared*包含兩個部分的檔案： *_Header.cshtml*並 *_Footer.cshtml*。 `<partial>`標記新增到 *_Layout.cshtml*檔案：

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```
