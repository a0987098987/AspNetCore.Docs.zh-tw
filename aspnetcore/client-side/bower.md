---
title: "使用 ASP.NET Core 中的 Bower"
author: rick-anderson
description: "Bower 管理用戶端封裝。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: ee628ee14aa38969cdb4443718c378fd36192596
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>管理用戶端封裝，以在 ASP.NET Core Bower

由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)，和[Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> 雖然仍會保留 Bower，其維護程式會建議使用不同的解決方案。 與 Webpack yarn 是一種常用的替代方法，[移轉指示](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)可用。

[Bower](https://bower.io/)呼叫其本身"web 版封裝管理員 」。 在.NET 生態系統中，填滿由 NuGet 的傳遞靜態內容的檔案無法驗證為 void。 針對 ASP.NET Core 專案，這些靜態檔案會於用戶端程式庫，例如固有[jQuery](http://jquery.com/)和[Bootstrap](http://getbootstrap.com/)。 .NET 程式庫，您仍然使用[NuGet](https://www.nuget.org/)封裝管理員。

設定用戶端之 ASP.NET Core 專案範本建立的新專案建置程序。 [jQuery](http://jquery.com/)和[啟動程序](http://getbootstrap.com/)安裝，而且受到 Bower。

用戶端封裝中所列*bower.json*檔案。 ASP.NET Core 專案範本會設定*bower.json* jQuery、 jQuery 驗證與啟動程序。

在本教學課程中，我們會將新增的支援[字型臻](http://fontawesome.io)。 可以使用安裝 bower 封裝**管理 Bower 封裝**UI 或以手動方式在*bower.json*檔案。

### <a name="installation-via-manage-bower-packages-ui"></a>透過管理 UI 的 Bower 封裝安裝

* 建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。 選取**Web 應用程式**和**不驗證**。

* 以滑鼠右鍵按一下方案總管 中的專案，然後選取**管理 Bower 封裝**(或者從主功能表**專案** > **管理 Bower 封裝**).

* 在**Bower:\<專案名稱\>**視窗中，按一下 [瀏覽] 索引標籤中，並輸入，以篩選封裝清單`font-awesome`[搜尋] 方塊中：

 ![管理 bower 封裝](bower/_static/manage-bower-packages.png)

* 確認 」 將變更儲存*bower.json*」 核取方塊。 從下拉式清單中選取版本，然後按一下**安裝** 按鈕。 **輸出** 視窗會顯示安裝詳細資料。

### <a name="manual-installation-in-bowerjson"></a>手動安裝在 bower.json

開啟*bower.json*檔案，然後加入 「 字型-實用 」 的相依性。 IntelliSense 會顯示可用的封裝。 選取封裝時，會顯示可用的版本。 下列映像和都不會符合您所看到的內容。

![Bower 封裝總管 的 IntelliSense](bower/_static/add-package.png)

![bower 版本 IntelliSense](bower/_static/version-intelliSense.png)

Bower 使用[語意版本設定](http://semver.org/)將組織的相依性。 語意版本設定，也稱為 SemVer 編號方式識別封裝\<主要 >。\<次要 >。\<修補程式 >。 IntelliSense 會顯示只有幾個常見的選項，以簡化語意版本設定。 IntelliSense 清單 (在上述範例 4.6.3) 中的最上層項目會被視為封裝的最新穩定版本。 插入號 (^) 符號符合最新的主要版本和波狀符號 （~） 符合最新的次要版本。

儲存*bower.json*檔案。 Visual Studio 會監看*bower.json*檔是否有變更。 在儲存時*bower 安裝*執行命令。 請參閱 [輸出] 視窗**Bower/npm**檢視執行的確切命令。

開啟*.bowerrc*底下*bower.json*。 `directory`屬性設定為*wwwroot/lib*表示 Bower 的位置將會安裝封裝資產。

```json
{
 "directory": "wwwroot/lib"
}
```

您可以使用 [方案總管] 中的 [搜尋] 方塊尋找並顯示字型實用的封裝。

開啟*_layout.cshtml\_Layout.cshtml*檔案，然後加入至環境字型實用的 CSS 檔案[標記協助程式](xref:mvc/views/tag-helpers/intro)如`Development`。 從 [方案總管] 中，拖曳和卸除*字型 awesome.css*內`<environment names="Development">`項目。

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

您可以在生產環境應用程式中加入*字型 awesome.min.css*環境標記協助程式，如`Staging,Production`。

取代內容*Views\Home\About.cshtml* Razor 檔案，以下列標記：

[!code-html[Main](bower/sample/About.cshtml)]

執行應用程式，並瀏覽至 [關於] 檢視，確認字型實用的封裝可運作。

## <a name="exploring-the-client-side-build-process"></a>探索用戶端建置程序

大部分的 ASP.NET Core 專案範本已設定為使用 Bower。 這個下一個逐步解說開頭空白的 ASP.NET Core 專案，並將手動加入每一項，讓您可以感受 Bower 專案中的使用方式。 您可以看到所發生的專案結構和執行階段輸出每個組態變更的狀況。

使用 Bower 用戶端建置程序的一般步驟如下：

* 定義您的專案中使用的封裝。 <!-- once defined, you don't need to download them, VS does -->
* 從您網頁參考封裝。

### <a name="define-packages"></a>定義封裝

一旦您在清單中的封裝*bower.json*檔案，Visual Studio 會下載它們。 下列範例會使用 Bower jQuery 和啟動程序來載入*wwwroot*資料夾。

* 建立新的 ASP.NET Core Web 應用程式與**ASP.NET Core Web 應用程式 (.NET Core)**範本。 選取**空**專案範本，然後按一下**確定**。

* 在 [方案總管] 中，以滑鼠右鍵按一下專案 >**加入新項目**選取**Bower 的組態檔**。 注意： A *.bowerrc*也會加入檔案。

* 開啟*bower.json*，加入 jquery 和要啟動`dependencies`> 一節。 產生*bower.json*檔案看起來像下列的範例。 版本會隨時間而改變，而不符合下方的影像。

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* 儲存*bower.json*檔案。

 請確認專案包含*啟動程序*和*jQuery*中的目錄*wwwroot/lib*。 Bower 使用*.bowerrc*檔案，以安裝中的資產*wwwroot/lib*。

 注意: < 管理 Bower 封裝 > UI 提供替代檔案手動編輯。

### <a name="enable-static-files"></a>啟用靜態檔案

* 新增`Microsoft.AspNetCore.StaticFiles`NuGet 封裝加入專案。
* 啟用靜態檔案來提供[靜態檔案中介軟體](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)。 將呼叫加入[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)至`Configure`方法`Startup`。

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>參考封裝

在本節中，您將建立 HTML 網頁，確認它可以存取部署的封裝。

* 加入名為新的 HTML 頁面*Index.html*至*wwwroot*資料夾。 注意： 您必須加入至 HTML 檔*wwwroot*資料夾。 根據預設，無法提供靜態內容之外*wwwroot*。 請參閱[靜態檔案處理](xref:fundamentals/static-files)如需詳細資訊。

 取代內容*Index.html*以下列標記：

[!code-html[Main](bower/sample/Index.html)]

* 執行應用程式，並瀏覽至`http://localhost:<port>/Index.html`。 或者，使用*Index.html*開啟，請按`Ctrl+Shift+W`。 請確認 jumbotron 樣式會套用、 jQuery 程式碼可以回應在按一下按鈕時，以及啟動程序的按鈕狀態變更。

 ![套用 jumbotron 樣式](bower/_static/jumbotron.png)
