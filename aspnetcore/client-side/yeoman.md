---
title: "與 Yeoman 中 ASP.NET Core 建置專案"
author: spboyer
description: "本文逐步建置 ASP.NET Core web 應用程式使用 Yeoman macOS 上的產生器。"
keywords: "ASP.NET Core、 Yeoman、 跨平台 yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>簡介與 Yeoman 中 ASP.NET Core 建置專案

[Yeoman](http://yeoman.io/)是用來建立許多種類的應用程式專案 scaffolding 系統。 Yeoman ASP.NET Core 的產生器包含各種不同的專案範本來啟動新的 web、 MVC 或主控台應用程式。

## <a name="install-nodejs-npm-and-yeoman"></a>安裝 Node.js、 npm 及 Yeoman

### <a name="prerequisites"></a>必要條件

Node.js 及 npm 所需的 Yeoman。 從下載[Node.js](https://nodejs.org/)。 安裝程式包含[Node.js](https://nodejs.org/)和[npm](https://www.npmjs.com/)。 Bower 也安裝是必要的 UI 架構，例如啟動程序。

若要安裝 Yeoman 和 Bower，執行下列命令：

```console
npm install -g yo bower
```

>[!Note]
>如果您收到錯誤`npm ERR! Please try running this command again as root/Administrator.`macOS，執行下列命令使用[sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

在命令提示字元中安裝 ASP.NET 產生器：

```console
npm install -g generator-aspnet
```

> [!NOTE]
> 如果您取得權限錯誤，請執行下的命令`sudo`上面所述。

`–g`旗標會安裝產生器的全域，使其可用於從任何路徑。

## <a name="create-an-aspnet-app"></a>建立 ASP.NET 應用程式

執行 Yeoman ASP.NET 產生器：

```console
yo aspnet
```

產生器會顯示功能表。 向下箭號**Web 應用程式基本 [沒有成員資格和授權]**專案，並點選**Enter**:

![命令視窗： 您要建立何種應用程式？ 應用程式類型的功能表](yeoman/_static/yeoman-yo-aspnet.png)

選取使用者介面架構為啟動程序，然後點選**Enter**。

使用 「**MyWebApp**」 應用程式名稱，然後點選**Enter**。

Yeoman 會 scaffold 專案和其支援的檔案。 表單的命令也會提供建議的後續步驟。

![命令視窗： ASP.NET 應用程式的名稱是什麼？ 命令提示字元](yeoman/_static/yeoman-yo-aspnet-created.png)

[ASP.NET 產生器](https://www.npmjs.com/package/generator-aspnet)建立 ASP.NET Core 專案中，可以先載入到 Visual Studio 程式碼中，Visual Studio 中，或是從命令列執行。

## <a name="restore-build-and-run"></a>還原、 建置及執行

藉由變更目錄至遵循建議的命令`MyWebApp`目錄。 然後執行`dotnet restore`。

![命令視窗](yeoman/_static/dotnet-restore.png)

建置並執行應用程式使用`dotnet build`和`dotnet run`:

![命令視窗](yeoman/_static/dotnet-build-run.png)

此時，您可以瀏覽至要測試新建立的 ASP.NET Core 應用程式顯示的 URL。

## <a name="client-side-packages"></a>用戶端封裝

範本所提供的前端資源 Yeoman 從產生器使用[Bower](xref:client-side/bower)用戶端封裝管理員 中，加入*bower.json*和*.bowerrc*還原使用 Bower 用戶端封裝的檔案。

[BundlerMinifier](xref:client-side/bundling-and-minification)元件也會包含預設輕鬆串連 （結合在一起） 和縮製的 CSS、 JavaScript 和 HTML。

## <a name="building-and-running-from-visual-studio"></a>從 Visual Studio 建置和執行

您可以直接將產生的 ASP.NET Core web 專案載入 Visual Studio 中，然後建置並從該處執行您的專案。 請遵循上述指示 scaffold 使用 Yeoman 的新 ASP.NET Core 應用程式。 此時，選擇**Web 應用程式**從功能表和應用程式名稱`MyWebApp`。

開啟 Visual Studio。 從 [檔案] 功能表中，選取 [開啟 ‣ 專案/方案]。

在 開啟專案 對話方塊中，瀏覽至*.csproj*檔案、 選取它，然後按一下**開啟** 按鈕。 在 方案總管專案看起來應該類似下面的螢幕擷取畫面。

![檔案和資料夾的 [方案總管] 中的新專案](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds MVC web 應用程式，同時使用完整伺服器和用戶端建置支援。 伺服器端的相依性會列在下**相依性/NuGet**節點，然後用戶端中的相依性**相依性/Bower**方案總管 節點。 載入專案時，會自動還原相依性。

![在 [方案總管] 樹狀結構檢視中的 [相依性] 節點中，Bower 資料夾是開啟列出其相依性。](yeoman/_static/yeoman-loading-dependencies.png)

當還原的所有相依性時，請按下**F5**執行專案。 預設的首頁會顯示在瀏覽器中。

![在 Microsoft Edge 中開啟的 web 應用程式](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>與還原、 建置、 裝載從命令列

您可以準備並裝載 web 應用程式使用.NET 核心 CLI。

在命令提示字元中，將目前目錄變更至包含專案的資料夾 (亦即，資料夾包含*.csproj*檔案):

```console
cd src\MyWebApp
```

還原專案的 NuGet 封裝相依性：

```console
dotnet restore
```

執行應用程式：

```console
dotnet run
```

跨平台[Kestrel](xref:fundamentals/servers/kestrel) web 伺服器會開始接聽通訊埠 5000。

開啟網頁瀏覽器，並導覽至`http://localhost:5000`。

![在 Microsoft Edge 中開啟的 web 應用程式](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Sub 產生器加入至專案

使用 Yeoman [sub 產生器](https://github.com/omnisharp/generator-aspnet)，您可以新增`nuget.config`或`web.config`建立專案之後。 比方說，從應該在其中建立檔案的目錄中執行下列命令：

```console
yo aspnet:nugetconfig
```

結果是名為的 NuGet 設定檔`nuget.config`具有下列內容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>其他資源

* [伺服器 （Kestrel 和 WebListener）](xref:fundamentals/servers/index)
* [基礎概念](xref:fundamentals/index)
