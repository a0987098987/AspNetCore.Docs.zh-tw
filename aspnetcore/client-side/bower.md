---
title: 用戶端使用管理套件在 ASP.NET Core 中的 Bower
author: rick-anderson
description: 使用 Bower 管理用戶端套件。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 08e6daa537c6c6f92a1cf80d70745e8ef606f580
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665609"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>用戶端使用管理套件在 ASP.NET Core 中的 Bower

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)， [Noel Rice](https://twitter.com/noelrice1)，和[Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Bower 會維護，而其維護人員會建議使用不同的解決方案。 [程式庫管理員](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/)(簡稱 LibMan) 是 Visual Studio 的新用戶端程式庫取得的工具 (Visual Studio 15.8 或更新版本）。 如需詳細資訊，請參閱<xref:client-side/libman/index>。 Bower 是透過 15.5 版支援 Visual Studio 中。
>
> Webpack 使用 yarn 是一個常見的方法，為其[移轉指示](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)可用。

[Bower](https://bower.io/)呼叫其本身的 「 web 的套件管理員 」。 在.NET 生態系統，它會填滿 NuGet 無法傳遞靜態內容的檔案所留下的 void。 針對 ASP.NET Core 專案，這些靜態檔案會將用戶端程式庫，例如原本就[jQuery](http://jquery.com/)並[Bootstrap](http://getbootstrap.com/)。 針對.NET 程式庫，您仍然使用[NuGet](https://www.nuget.org/)封裝管理員。

以設定用戶端的 ASP.NET Core 專案範本建立的專案建置程序。 [jQuery](http://jquery.com/)並[Bootstrap](http://getbootstrap.com/)安裝之後，而且 Bower 支援。

用戶端套件都會列入*bower.json*檔案。 ASP.NET Core 專案範本會設定*bower.json* jQuery、 jQuery 驗證與啟動程序。

在本教學課程中，我們將新增的支援[Font Awesome](http://fontawesome.io)。 Bower 套件可以使用來安裝**管理 Bower 封裝**UI 或手動*bower.json*檔案。

### <a name="installation-via-manage-bower-packages-ui"></a>透過管理 Bower 封裝 UI 的安裝

* 建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)** 範本。 選取  **Web 應用程式**並**無驗證**。

* 以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理 Bower 封裝**(或者主功能表中，從**專案** > **管理 Bower 封裝**).

* 在  **Bower:\<專案名稱\>** 視窗中，按一下 瀏覽 索引標籤，並輸入，以篩選的套件清單`font-awesome`在搜尋方塊中：

  ![管理 bower 套件](bower/_static/manage-bower-packages.png)

* 確認 」 將變更儲存到*bower.json*」 核取方塊。 從下拉式清單中選取版本，然後按一下**安裝** 按鈕。 **輸出**視窗會顯示安裝詳細資料。

### <a name="manual-installation-in-bowerjson"></a>在 bower.json 的手動安裝

開啟*bower.json*檔案，並將 「 字型-絕佳 」 加入至相依性。 IntelliSense 會顯示可用的套件。 選取套件時，會顯示可用的版本。 下列影像還舊，並不會符合您所看到的內容。

![Bower 套件總管 的 IntelliSense](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

Bower 用法[語意版本設定](http://semver.org/)組織相依性。 語意版本設定，也就是 SemVer 識別套件的編號配置\<主要 >。\<次要 >。\<修補程式 >。 IntelliSense 會顯示只有幾個常見的選擇，以簡化語意版本設定。 在 [IntelliSense] 清單 (在上述範例中的 4.6.3) 頂端的項目會被視為封裝的最新穩定版本。 插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 比對的最新的次要版本。

儲存*bower.json*檔案。 Visual Studio 會監看*bower.json*變更的檔案。 在儲存時， *bower 安裝*執行命令。 請參閱 [輸出] 視窗**Bower/npm**檢視執行的確切命令。

開啟 *.bowerrc*下方的檔案*bower.json*。 `directory`屬性設定為*wwwroot/lib*表示位置 Bower 將安裝套件的資產。

```json
{
 "directory": "wwwroot/lib"
}
```

您可以使用 [方案總管] 中的 [搜尋] 方塊來尋找及顯示字型很棒的封裝。

開啟*Views\Shared\_Layout.cshtml*檔案，並將字型很棒的 CSS 檔案新增至環境[標籤協助程式](xref:mvc/views/tag-helpers/intro)如`Development`。 從 [方案總管] 拖*字型 awesome.css*內`<environment names="Development">`項目。

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

您可以在生產環境應用程式中加入*字型 awesome.min.css*環境標籤協助程式，如`Staging,Production`。

內容取代*Views\Home\About.cshtml* Razor 檔案，以下列標記：

[!code-html[](bower/sample/About.cshtml)]

執行應用程式並巡覽至 About 檢視來確認字型很棒的封裝可運作。

## <a name="exploring-the-client-side-build-process"></a>探索用戶端建置程序

大部分的 ASP.NET Core 專案範本已設定為使用 Bower。 這個下一個逐步解說開頭空白的 ASP.NET Core 專案，並手動新增每個片段，因此您可以取得了解如何在專案中使用 Bower。 您可以看到會發生什麼事的專案結構和執行階段輸出，如每個組態變更。

可搭配 Bower 的用戶端建置程序的一般步驟如下：

* 定義您專案中使用的封裝。 <!-- once defined, you don't need to download them, VS does -->
* 從您的網頁參考套件。

### <a name="define-packages"></a>定義套件

一旦您在清單中的套件*bower.json*檔案，Visual Studio 會下載它們。 下列範例會使用 Bower 載入 jQuery 和 Bootstrap *wwwroot*資料夾。

* 建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)** 範本。 選取 **空**專案範本，然後按一下**確定**。

* 在 [方案總管] 中，以滑鼠右鍵按一下專案 >**加入新項目**，然後選取**Bower 組態檔**。 注意:A *.bowerrc*也會加入檔案。

* 開啟*bower.json*，並新增 jquery 來啟動`dependencies`一節。 產生*bower.json*檔案看起來如下列範例所示。 版本會隨著時間改變，而不符合下列映像。

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* 儲存*bower.json*檔案。

  請確認專案包含*bootstrap*並*jQuery*中的目錄*wwwroot/lib*。 Bower 用法 *.bowerrc*檔案，以安裝中的資產*wwwroot/lib*。

  注意:[管理 Bower 封裝] UI 會提供替代檔案手動編輯。

### <a name="enable-static-files"></a>啟用靜態檔案

* 新增`Microsoft.AspNetCore.StaticFiles`NuGet 封裝加入專案。
* 啟用靜態檔案，以提供[靜態檔案中介軟體](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions)。 將呼叫加入[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions)要`Configure`方法`Startup`。

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>參照套件

在本節中，您將建立 HTML 網頁以確認它可以存取部署的封裝。

* 加入新的 HTML 網頁，名為*Index.html*要*wwwroot*資料夾。 注意:您必須新增至 HTML 檔案*wwwroot*資料夾。 根據預設，無法提供靜態內容之外*wwwroot*。 請參閱[靜態檔案](xref:fundamentals/static-files)如需詳細資訊。

  內容取代*Index.html*以下列標記：

[!code-html[](bower/sample/Index.html)]

* 執行應用程式，並瀏覽至`http://localhost:<port>/Index.html`。 或者，使用*Index.html*開啟，請按`Ctrl+Shift+W`。 請確認 jumbotron 樣式會套用，jQuery 程式碼會回應按一下按鈕時，並啟動程序的按鈕狀態變更。

  ![套用 jumbotron 樣式](bower/_static/jumbotron.png)
