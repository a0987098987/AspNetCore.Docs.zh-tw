---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何在 ASP.NET Core 的類別庫中，使用部分視圖建立可重複使用的 Razor UI。
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 420cc54701394673e2b442b1fdf999e421820fd5
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809116"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>在 ASP.NET Core 中使用 Razor 類別庫專案建立可重複使用的 UI

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Razor 視圖、頁面、控制器、頁面模型、 [razor 元件](xref:blazor/class-libraries)、[視圖元件](xref:mvc/views/view-components)和資料模型可以內建于 RAZOR 類別庫（RCL）中。 RCL 可以封裝和重複使用。 應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。 在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>建立包含 Razor UI 的類別庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 選取 [**建立新專案**]。
* 選取  **Razor 類別庫**  **> 下一步**。
* 為程式庫命名（例如，"RazorClassLib"），>**建立**。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。
* 如果您需要支援視圖，請選取 [**支援頁面和視圖**]。 根據預設，只支援 Razor Pages。 選取 [建立]。

根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。 **支援頁面和 views**選項支援頁面和視圖。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行 `dotnet new razorclasslib`。 例如：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

根據預設，Razor 類別庫（RCL）範本預設為 Razor 元件開發。 傳遞 `--support-pages-and-views` 選項（`dotnet new razorclasslib --support-pages-and-views`），以提供頁面和視圖的支援。

如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。

---

新增 Razor 檔案至 RCL。

ASP.NET Core 範本會假設 RCL 內容位於 [*區域*] 資料夾中。 請參閱[RCL 頁面配置](#rcl-pages-layout)，以建立會在 `~/Pages` 中公開內容的 RCL，而不是 `~/Areas/Pages`。

## <a name="reference-rcl-content"></a>參考 RCL 內容

RCL 可以由下列各項參考：

* NuGet 套件。 請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。
* *{ProjectName}.csproj*。 請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。

## <a name="override-views-partial-views-and-pages"></a>覆寫檢視、部分檢視和頁面

在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。 例如，將*WebApp1/Areas/MyFeature/Pages/Page1. cshtml*新增到 WebApp1，WebApp1 中的 page1 會優先于 RCL 中的 page1。

在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。

將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。 更新標記來指示新的位置。 建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。

### <a name="rcl-pages-layout"></a>RCL 頁面配置

若要參考 RCL 內容，如同它是 web 應用程式*Pages*資料夾的一部分，請使用下列檔案結構來建立 RCL 專案：

* *RazorUIClassLib/Pages*
* *RazorUIClassLib/Pages/Shared*

假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔案： *_Header. cshtml*和 *_Footer. cshtml*。 `<partial>` 標記可以新增至 _Layout 的*cshtml*檔案：

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>建立具有靜態資產的 RCL

RCL 可能需要隨附的靜態資產，由 RCL 或 RCL 的取用應用程式來參考。 ASP.NET Core 可讓您建立包含可供取用應用程式使用之靜態資產的 RCLs。

若要將隨附的資產納入為 RCL 的一部分，請在類別庫中建立*wwwroot*資料夾，並在該資料夾中包含任何必要的檔案。

封裝 RCL 時， *wwwroot*資料夾中的所有附屬資產都會自動包含在套件中。

### <a name="exclude-static-assets"></a>排除靜態資產

若要排除靜態資產，請將所需的排除路徑新增至專案檔中的 `$(DefaultItemExcludes)` 屬性群組。 以分號（`;`）分隔專案。

在下列範例中， *wwwroot*資料夾中的*lib .css*樣式不會被視為靜態資產，而且不會包含在已發行的 RCL 中：

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>Typescript 整合

若要在 RCL 中包含 TypeScript 檔案：

1. 將 TypeScript 檔案（ *. ts*）放在*wwwroot*資料夾外部。 例如，將檔案放在*用戶端*資料夾中。

1. 設定 [ *wwwroot* ] 資料夾的 TypeScript 組建輸出。 在專案檔的 `PropertyGroup` 內設定 `TypescriptOutDir` 屬性：

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. 將 TypeScript 目標納入為 `ResolveCurrentProjectStaticWebAssets` 目標的相依性，方法是在專案檔的 `PropertyGroup` 內新增下列目標：

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>從參考的 RCL 取用內容

RCL 的*wwwroot*資料夾中包含的檔案會公開給 RCL 或 `_content/{LIBRARY NAME}/`前置詞底下的取用應用程式。 例如，名為 Razor. *.lib*的程式庫會產生 `_content/Razor.Class.Lib/`的靜態內容路徑。 當產生 NuGet 套件，且元件名稱與套件識別碼不同時，請使用 `{LIBRARY NAME}`的封裝識別碼。

取用應用程式會參考程式庫所提供的靜態資產，其具有 `<script>`、`<style>`、`<img>`和其他 HTML 標籤。 取用應用程式必須在中`Startup.Configure`啟用[靜態檔案支援](xref:fundamentals/static-files)：

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

從組建輸出（`dotnet run`）執行取用應用程式時，預設會在開發環境中啟用靜態 web 資產。 若要在從組建輸出執行時支援其他環境中的資產，請在*Program.cs*中的主機產生器上呼叫 `UseStaticWebAssets`：

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

從已發行的輸出（`dotnet publish`）執行應用程式時，不需要呼叫 `UseStaticWebAssets`。

### <a name="multi-project-development-flow"></a>多專案開發流程

當使用中的應用程式執行時：

* RCL 中的資產會保留在其原始檔案夾中。 資產不會移至取用應用程式。
* 重建 RCL 之後，RCL *wwwroot*資料夾內的任何變更都會反映在取用應用程式中，而不需要重建取用應用程式。

建立 RCL 時，會產生描述靜態 web 資產位置的資訊清單。 取用應用程式會在執行時間讀取資訊清單，以從參考的專案和套件取用資產。 將新資產新增至 RCL 時，必須重建 RCL 以更新其資訊清單，取用應用程式才能存取新資產。

### <a name="publish"></a>發行

當應用程式發佈時，所有參考專案和套件中的附屬資產都會複製到 `_content/{LIBRARY NAME}/`底下已發佈應用程式的*wwwroot*資料夾。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor 視圖、頁面、控制器、頁面模型、 [razor 元件](xref:blazor/class-libraries)、[視圖元件](xref:mvc/views/view-components)和資料模型可以內建于 RAZOR 類別庫（RCL）中。 RCL 可以封裝和重複使用。 應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。 在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>建立包含 Razor UI 的類別庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**Visual Studio 的**[檔案] 功能表中，選取 [**新增**>**專案**]。
* 選取 [ASP.NET Core Web 應用程式]。
* 命名程式庫 (例如，"RazorClassLib") > [確定]。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [ **Razor 類別庫** **] > [確定]** 。

RCL 具有下列專案檔：

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行 `dotnet new razorclasslib`。 例如：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。

---

新增 Razor 檔案至 RCL。

ASP.NET Core 範本會假設 RCL 內容位於 [*區域*] 資料夾中。 請參閱[RCL 頁面配置](#rcl-pages-layout)，以建立會在 `~/Pages` 中公開內容的 RCL，而不是 `~/Areas/Pages`。

## <a name="reference-rcl-content"></a>參考 RCL 內容

RCL 可以由下列各項參考：

* NuGet 套件。 請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。
* *{ProjectName}.csproj*。 請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>逐步解說：建立 RCL 專案並從 Razor Pages 專案使用

您可以下載[完整專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。 下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。 您可以在[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。

### <a name="test-the-download-app"></a>測試下載應用程式

如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-an-rcl)。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio 中開啟 *.sln* 檔案。 執行應用程式。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令提示字元中在 *cli* 目錄中，建置 RCL 和 Web 應用程式。

```dotnetcli
dotnet build
```

移至 *WebApp1* 目錄並執行應用程式：

```dotnetcli
dotnet run
```

---

遵循[測試 WebApp1](#test-webapp1) 中的指示執行。

## <a name="create-an-rcl"></a>建立 RCL

在本節中，會建立 RCL。 會有 Razor 檔案新增至 RCL。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 RCL 專案：

* 從**Visual Studio 的**[檔案] 功能表中，選取 [**新增**>**專案**]。
* 選取 [ASP.NET Core Web 應用程式]。
* 將應用程式命名為**RazorUIClassLib** >**確定**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [ **Razor 類別庫** **] > [確定]** 。
* 新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行下列命令：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

上述命令：

* 建立 `RazorUIClassLib` RCL。
* 建立 Razor _Message 頁面，並將它新增至 RCL。 `-np` 參數會建立頁面而不使用 `PageModel`。
* 建立[_ViewStart 的 cshtml](xref:mvc/views/layout#running-code-before-each-view)檔案，並將它新增至 RCL。

需要 *_ViewStart. cshtml*檔案才能使用 Razor Pages 專案的配置（在下一節中加入）。

---

### <a name="add-razor-files-and-folders-to-the-project"></a>將 Razor 檔案和資料夾加入至專案

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。 您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。 例如：

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  如需 *_ViewImports*的詳細資訊，請參閱匯[入共用](xref:mvc/views/layout#importing-shared-directives)指示詞

* 建置類別庫，以確認沒有任何編譯器錯誤：

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。 *RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>從 Razor 頁面專案使用 Razor UI 程式庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 Razor 頁面 Web 應用程式：

* 在**方案總管**中，以滑鼠右鍵按一下方案 **> 新增**>**新增專案**。
* 選取 [ASP.NET Core Web 應用程式]。
* 將應用程式命名為 **WebApp1**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [ **Web 應用程式** **] > [確定]** 。

* 從 [方案總管]，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]。
* 在**方案總管**中，以滑鼠右鍵按一下 [WebApp1 **]** ，然後選取 [組建相依性] >**專案**相依性。
* 核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。
* 在**方案總管**中，以滑鼠右鍵按一下**WebApp1** ，然後選取 [**新增**>**參考**]。
* 在 **參考管理員** 對話方塊中，檢查**RazorUIClassLib** >**確定**。

執行應用程式。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

建立 Razor Pages web 應用程式，以及包含 Razor Pages 應用程式和 RCL 的方案檔：

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

建置和執行 Web 應用程式：

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>測試 WebApp1

流覽至 `/MyFeature/Page1`，確認 Razor UI 類別庫正在使用中。

## <a name="override-views-partial-views-and-pages"></a>覆寫檢視、部分檢視和頁面

在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 ( *.cshtml* 檔案) 為優先。 例如，將*WebApp1/Areas/MyFeature/Pages/Page1. cshtml*新增到 WebApp1，WebApp1 中的 page1 會優先于 RCL 中的 page1。

在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。

將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。 更新標記來指示新的位置。 建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。

### <a name="rcl-pages-layout"></a>RCL 頁面配置

若要參考 RCL 內容，如同它是 web 應用程式*Pages*資料夾的一部分，請使用下列檔案結構來建立 RCL 專案：

* *RazorUIClassLib/Pages*
* *RazorUIClassLib/Pages/Shared*

假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔案： *_Header. cshtml*和 *_Footer. cshtml*。 `<partial>` 標記可以新增至 _Layout 的*cshtml*檔案：

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
