---
title: ASP.NET Core 類別庫中的可重複使用 Razor UI
author: Rick-Anderson
description: 說明如何在ASP.NET Core 中的類庫中使用部分檢視創建可重用的 Razor UI。
ms.author: riande
ms.date: 01/25/2020
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: f24dc62eba345a8a3d35143805b4966cb51832fa
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667563"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>使用 ASP.NET Core 中的 Razor 函式庫專案建立可重用的 UI

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Razor 檢視、頁面、控制器、頁面模型[、Razor 元件](xref:blazor/class-libraries)、[檢視元件](xref:mvc/views/view-components)和資料模型可以內置到 Razor 類庫 (RCL) 中。 RCL 可以封裝和重複使用。 應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。 在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>建立包含 Razor UI 的類別庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從視覺化工作室選擇**建立新專案**。
* 選擇**剃刀類別庫**>**下一個**。
* 命名庫(例如,"RazorClassLib"),>**建立**。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。
* 如果需要支援檢視,請選擇 **「支援」 頁面與檢視**。 默認情況下,僅支援剃刀頁。 選取 [建立]  。

默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。 支援**頁面和檢視**「選項支援頁面和檢視。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行 `dotnet new razorclasslib`。 例如：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

默認情況下,Razor 類庫 (RCL) 範本預設為 Razor 元件開發。 通過`--support-pages-and-views`選項`dotnet new razorclasslib --support-pages-and-views`() 以提供對頁面和檢視的支援。

如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。

---

新增 Razor 檔案至 RCL。

ASP.NET核心範本假定 RCL 內容位於 *「區域」* 資料夾中。 請參閱[RCL 頁面佈局](#rcl-pages-layout)以`~/Pages``~/Areas/Pages`創建在 而不是公開 內容的 RCL。

## <a name="reference-rcl-content"></a>參考 RCL 內容

RCL 可以由下列各項參考：

* NuGet 套件。 請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。
* *{ProjectName}.csproj*。 請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。

## <a name="override-views-partial-views-and-pages"></a>覆寫檢視、部分檢視和頁面

在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。 例如,將*WebApp1/區域/我的功能/頁面/Page1.cshtml*添加到 WebApp1,WebApp1 中的第 1 頁將優先於 RCL 中的第 1 頁。

在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。

將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。 更新標記來指示新的位置。 建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。

### <a name="rcl-pages-layout"></a>RCL 頁面配置

要參考 RCL 內容,就好像它是 Web 應用的*Pages*資料夾一樣,請建立具有以下檔案結構的 RCL 專案:

* *RazorUIClassLib/頁面*
* *RazorUIClassLib/頁面/共用*

假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔 *:_Header.cshtml*和 *_Footer.cshtml*。 標記`<partial>`可以加入 *_Layout.cshtml*檔案中:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>使用靜態資產建立 RCL

RCL 可能需要可供 RCL 或 RCL 使用應用引用的配套靜態資產。 ASP.NET核心允許創建包含可供使用應用的靜態資產的 LPL。

要將配套資產包含在 RCL 中,請在類庫中創建一個*wwwroot*資料夾,並在該資料夾中包含任何必需的檔。

打包 RCL 時 *,wwwroot*資料夾中的所有配套資產將自動包含在包中。

### <a name="exclude-static-assets"></a>排除靜態資產

要排除靜態資產,將所需的排除路徑添加到專案檔中`$(DefaultItemExcludes)`的屬性組。 分號 (`;`的單獨條目)。

在下面的範例中 *,wwwroot*資料夾中的*lib.css*樣式表不被視爲靜態資產,並且不包括在已發佈的 RCL 中:

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>打字文本整合

在 RCL 中包含 TypeScript 檔,請進行以下操作:

1. 將 TypeScript 檔 *(.ts*) 放在*wwwroot*資料夾之外。 例如,將檔案放在*客戶端*資料夾中。

1. 為*wwwroot*資料夾配置 TypeScript 生成輸出。 在`TypescriptOutDir`專案檔中的`PropertyGroup`中 設定 屬性:

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. 以專案檔案中 新增`ResolveCurrentProjectStaticWebAssets``PropertyGroup`以下目標,將 TypeScript 目標作為目標的相依項:

   ```xml
   <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
     CompileTypeScript;
     $(ResolveCurrentProjectStaticWebAssetsInputs)
   </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>使用參考的 RCL 中的內容

包含在 RCL 的*wwwroot*資料夾中的檔案會`_content/{LIBRARY NAME}/`公開到前綴 下的 RCL 或使用的應用程式。 例如,名為*Razor.Class.Lib*的庫會導致`_content/Razor.Class.Lib/`在處的靜態內容路徑。 生成 NuGet 套件且程式集名稱與包 ID 不同時`{LIBRARY NAME}`,請使用的包 ID。

使用的應用程式引用函式庫提供的靜態`<script>``<style>`資源,`<img>`以及、和其他 HTML 標籤。 使用要使用靜`Startup.Configure`[態檔案支援](xref:fundamentals/static-files):

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

從生成輸出 ()`dotnet run`運行使用應用時,靜態 Web 資產預設在開發環境中啟用。 要支援其他環境中的資產,請調用`UseStaticWebAssets` *Program.cs*中的主機生成器:

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

從`UseStaticWebAssets`已發佈輸出 () 運行應用`dotnet publish`時,不需要調用。

### <a name="multi-project-development-flow"></a>多項目開發流程

使用應用程式執行時:

* RCL 中的資產保留在其原始資料夾中。 資產不會移動到使用的應用。
* 重建 RCL 後,RCL 的*wwwroot*資料夾中的任何更改都將反映在使用應用中,而不重新生成使用的應用程式。

生成 RCL 時,將生成描述靜態 Web 資產位置的清單。 使用的應用程式在運行時讀取清單以使用引用的專案和包中的資產。 將新資產添加到 RCL 時,必須重新生成 RCL 以更新其清單,然後才能使用的應用訪問新資產。

### <a name="publish"></a>發佈

發佈應用時,所有引用的專案和包的配套資源將複製到`_content/{LIBRARY NAME}/`下 已發佈應用的*wwwroot*資料夾中。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Razor 檢視、頁面、控制器、頁面模型[、Razor 元件](xref:blazor/class-libraries)、[檢視元件](xref:mvc/views/view-components)和資料模型可以內置到 Razor 類庫 (RCL) 中。 RCL 可以封裝和重複使用。 應用程式可以包括 RCL，以及覆寫它所包含的檢視和頁面。 在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)([如何下載](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>建立包含 Razor UI 的類別庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**[專案]** > **** 。
* 選取 **ASP.NET Core Web 應用程式**。
* 命名程式庫 (例如，"RazorClassLib") > [確定]****。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Razor 類別庫]**[確定]** > ****。

RCL 具有以下項目檔:

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行 `dotnet new razorclasslib`。 例如：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

如需詳細資訊，請參閱 [dotnet new](/dotnet/core/tools/dotnet-new)。 若要避免與產生的檢視程式庫發生檔案名稱衝突，程式庫名稱結尾請務必不要使用 `.Views`。

---

新增 Razor 檔案至 RCL。

ASP.NET核心範本假定 RCL 內容位於 *「區域」* 資料夾中。 請參閱[RCL 頁面佈局](#rcl-pages-layout)以`~/Pages``~/Areas/Pages`創建在 而不是公開 內容的 RCL。

## <a name="reference-rcl-content"></a>參考 RCL 內容

RCL 可以由下列各項參考：

* NuGet 套件。 請參閱[建立 NuGet 套件](/nuget/create-packages/creating-a-package)和 [dotnet add package](/dotnet/core/tools/dotnet-add-package) 和[建立和發佈 NuGet 套件](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)。
* *{ProjectName}.csproj*。 請參閱 [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference)。

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>演練:建立 RCL 專案並從 Razor 頁面專案使用

您可以下載[完整專案](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples)並測試它，而不是建立它。 下載範例包含額外的程式碼和連結，讓您輕鬆地測試專案。 您可以在[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/6098)留下意見反應以及您對下載範例與逐步指示的評論。

### <a name="test-the-download-app"></a>測試下載應用程式

如果您尚未下載已完成的應用程式，而是要建立逐步解說專案，請跳至[下一節](#create-an-rcl)。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio 中開啟 *.sln* 檔案。 執行應用程式。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

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

在本節中,將創建一個 RCL。 會有 Razor 檔案新增至 RCL。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 RCL 專案：

* 從 Visual Studio 的 [檔案]**** 功能表中，選取 [新增]**[專案]** > **** 。
* 選取 **ASP.NET Core Web 應用程式**。
* 命名應用程式**RazorUClassLib** > **OK**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Razor 類別庫]**[確定]** > ****。
* 新增名為 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 的 Razor 部分檢視檔。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令列執行下列命令：

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

上述命令：

* 創建`RazorUIClassLib`RCL。
* 建立 Razor _Message 頁面，並將它新增至 RCL。 `-np` 參數會建立頁面而不使用 `PageModel`。
* 建立[_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view)檔並將其添加到 RCL。

*_ViewStart.cshtml*檔需要使用 Razor Pages 專案的佈局(下一節將添加)。

---

### <a name="add-razor-files-and-folders-to-the-project"></a>將 Razor 檔案與資料夾加入項目中

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 中的標記取代為下列程式碼：

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* 將 *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* 中的標記取代為下列程式碼：

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  需要 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 才能使用部分檢視 (`<partial name="_Message" />`)。 您可以新增 *_ViewImports.cshtml* 檔案，而不要包含 `@addTagHelper` 指示詞。 例如：

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  有關 *_ViewImports.cshtml*的詳細資訊,請參閱[匯入分享指令](xref:mvc/views/layout#importing-shared-directives)

* 建置類別庫，以確認沒有任何編譯器錯誤：

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

建置輸出包含 *RazorUIClassLib.dll* 和 *RazorUIClassLib.Views.dll*。 *RazorUIClassLib.Views.dll* 包含已編譯的 Razor 內容。

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>從 Razor 頁面專案使用 Razor UI 程式庫

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

建立 Razor 頁面 Web 應用程式：

* 在 [方案總管]**** 中，以滑鼠右鍵按一下解決方案 > [新增]**[新增專案]** >  ****。
* 選取 **ASP.NET Core Web 應用程式**。
* 將應用程式命名為 **WebApp1**。
* 確認已選取 **ASP.NET Core 2.1** 或更新版本。
* 選取 [Web 應用程式]**[確定]** > ****。

* 從 [方案總管]****，以滑鼠右鍵按一下 **WebApp1**，然後選取 [設定為啟始專案]****。
* 從方案總管****，以滑鼠右鍵按一下 **WebApp1**，然後選取 [建置相依性]**[專案相依性]** > ****。
* 核取 **RazorUIClassLib** 作為 **WebApp1** 的相依性。
* 從 [方案總管]****，以滑鼠右鍵按一下 **WebApp1**，然後選取 [新增]**[參考]** > ****。
* 在 [參考管理員]**** 對話方塊中，核取 **RazorUIClassLib[確定]** > ****。

執行應用程式。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

建立 Razor 網頁網頁 應用程式與包含 Razor 網頁應用和 RCL 的解決方案檔:

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

瀏覽`/MyFeature/Page1`以 驗證 Razor UI 類庫是否正在使用中。

## <a name="override-views-partial-views-and-pages"></a>覆寫檢視、部分檢視和頁面

在 Web 應用程式和 RCL 中發現檢視、部分檢視，或是 Razor 頁面時，以 Web 應用程式中的 Razor 標記 (*.cshtml* 檔案) 為優先。 例如,將*WebApp1/區域/我的功能/頁面/Page1.cshtml*添加到 WebApp1,WebApp1 中的第 1 頁將優先於 RCL 中的第 1 頁。

在下載範例中，將 *WebApp1/Areas/MyFeature2* 重新命名為 *WebApp1/Areas/MyFeature* 以測試優先順序。

將 *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* 部分檢視複製到 *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*。 更新標記來指示新的位置。 建置並執行應用程式，以確認正在使用部分檢視的應用程式版本。

### <a name="rcl-pages-layout"></a>RCL 頁面配置

要參考 RCL 內容,就好像它是 Web 應用的*Pages*資料夾一樣,請建立具有以下檔案結構的 RCL 專案:

* *RazorUIClassLib/頁面*
* *RazorUIClassLib/頁面/共用*

假設*RazorUIClassLib/Pages/Shared*包含兩個部分檔 *:_Header.cshtml*和 *_Footer.cshtml*。 標記`<partial>`可以加入 *_Layout.cshtml*檔案中:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:blazor/class-libraries>
