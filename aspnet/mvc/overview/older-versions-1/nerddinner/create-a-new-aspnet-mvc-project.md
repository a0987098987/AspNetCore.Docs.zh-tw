---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 建立新的 ASP.NET MVC 專案 |Microsoft Docs
author: microsoft
description: 步驟 1 會示範如何將基本 NerdDinner 應用程式的結構放在位置。
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 88ef850503725cc57c92a11952729b4bd2205a69
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835076"
---
<a name="create-a-new-aspnet-mvc-project"></a>建立新的 ASP.NET MVC 專案
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 1 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 1 會示範如何將基本 NerdDinner 應用程式的結構放在位置。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner 步驟 1： 檔案-&gt;新專案

我們一開始我們 NerdDinner 的應用程式，選取**檔案&gt;新的專案**Visual Studio 2008 或免費 Visual Web Developer 2008 Express 中的功能表項目。

這會顯示 [新增專案] 對話方塊。 若要建立新的 ASP.NET MVC 應用程式，我們會選取對話方塊左側的 網站 節點，然後選擇 在右側 ASP.NET MVC Web 應用程式 」 專案範本：

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*重要事項： 請確定您已下載並安裝 ASP.NET MVC-否則它不會顯示在 [新增專案] 對話方塊。您可以使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)如果您尚未安裝它 (ASP.NET MVC 是可以在"Web 平台-&gt;架構和執行階段 」 一節)。*

我們會將新的專案，我們將建立 「 NerdDinner"，然後按一下 [確定] 按鈕來建立它。

當我們按一下 [確定] Visual Studio 會顯示其他對話方塊，提示我們選擇建立新應用程式的單元測試專案。 此單元測試專案可讓我們將建立自動化的測試來驗證的功能和我們的應用程式行為 (我們將討論的項目如何待辦事項，稍後在本教學課程)。

![](create-a-new-aspnet-mvc-project/_static/image2.png)

在上述的對話方塊中的 「 測試架構 」 下拉式清單中會填入所有使用 ASP.NET MVC 單元測試專案範本安裝在電腦上。 適用於 NUnit、 2.0、mbunit 和 XUnit，就可以下載的版本。 也支援內建的 Visual Studio 單元測試架構。

*注意： Visual Studio 單元測試架構只是適用於 Visual Studio 2008 Professional 或更新版本。如果您使用 VS 2008 Standard Edition 或 Visual Web Developer 2008 Express，您必須下載並安裝 ASP.NET mvc 的 NUnit、 MBUnit 或 XUnit 的延伸模組，以便顯示此對話方塊。如果沒有安裝任何測試架構，將不會顯示對話方塊。*

我們將使用我們建立時，測試專案的預設 「 NerdDinner.Tests 」 名稱，並使用 Visual Studio 單元測試架構選項。 當我們按一下 Visual Studio 的 確定 按鈕會包含兩個專案，一個用於 web 應用程式，一個針對我們的單元測試就讓我們建立一個方案：

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>檢查 NerdDinner 目錄結構

當您使用 Visual Studio 中建立新的 ASP.NET MVC 應用程式時，它會自動加入幾個檔案和目錄的專案：

![](create-a-new-aspnet-mvc-project/_static/image4.png)

預設的 ASP.NET MVC 專案會有六個最上層目錄：

| **目錄** | **目的** |
| --- | --- |
| **/ 控制站** | 您在其中放置處理 URL 要求的控制器類別 |
| **/ 模型** | 您在其中放置代表和操作資料的類別 |
| **/ 檢視表** | 您在其中放置負責轉譯輸出的 UI 範本檔案 |
| **/Scripts** | 您可在此將 JavaScript 程式庫檔案和指令碼 (.js) |
| **/Content** | 您放置 CSS 和映像檔，以及其他非-動態/非-JavaScript 內容 |
| **/ 應用程式\_資料** | 儲存資料檔案，您會想要讀取/寫入。 |

ASP.NET MVC 不需要此結構。 事實上，處理大型應用程式的開發人員將通常分散到應用程式設定以讓它更容易管理的多個專案 (例如： 資料模型類別通常進入個別的類別庫專案的 web 應用程式)。 不過，預設的專案結構，並提供我們可以使用為了讓我們的應用程式考量全新的好用的預設目錄慣例。

當我們展開 /Controllers 目錄，我們會發現，Visual Studio 兩個控制器類別 – HomeController 和 AccountController – 預設新增至專案：

![](create-a-new-aspnet-mvc-project/_static/image5.png)

當我們展開 /Views 目錄時，我們會發現三個子目錄 – /Home、 /Account、 /Shared – 以及數個範本中的檔案也預設會加入至專案：

![](create-a-new-aspnet-mvc-project/_static/image6.png)

當我們展開 /Content 和 /scripts 複製目錄中時，我們會發現用來設定樣式，於網站上的所有 HTML Site.css 檔案，以及可 ASP.NET AJAX 和 jQuery JavaScript 程式庫支援在應用程式中：

![](create-a-new-aspnet-mvc-project/_static/image7.png)

當我們展開 NerdDinner.Tests 專案，我們會發現包含我們的控制器類別的單元測試的兩個類別：

![](create-a-new-aspnet-mvc-project/_static/image8.png)

這些加入的 Visual Studio 的預設檔案提供我們一個基本結構如可用應用程式-完整的首頁上，有關頁面、 帳戶登入/登出/註冊頁面，以及未處理的錯誤頁面 （所有連接和現成的工作）。

### <a name="running-the-nerddinner-application"></a>執行 NerdDinner 應用程式

我們可以執行專案，選擇**偵錯-&gt;啟動偵錯**或是**偵錯-&gt;啟動但不偵錯**功能表項目：

![](create-a-new-aspnet-mvc-project/_static/image9.png)

這會啟動內建 ASP.NET Web 伺服器隨附於 Visual Studio 中，並執行應用程式：

![](create-a-new-aspnet-mvc-project/_static/image10.png)

以下是我們新的專案首頁 (URL:"/") 執行時：

![](create-a-new-aspnet-mvc-project/_static/image11.png)

按一下 [關於] 索引標籤會顯示關於頁面 (URL:"/ Home /"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

按一下右上方的 「 登入 」 連結會帶我們前往登入頁面 (URL:"/ 帳戶/登入 」)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

如果我們沒有登入帳戶，我們可以按一下 [註冊] 連結 (URL:"/ 帳戶/註冊 」) 來建立一個：

![](create-a-new-aspnet-mvc-project/_static/image14.png)

程式碼來實作上述的首頁，和登出 / 註冊功能已新增根據預設，當我們建立我們的新專案時。 我們將使用它作為我們的應用程式的起點。

### <a name="testing-the-nerddinner-application"></a>測試 NerdDinner 應用程式

如果我們使用 Professional Edition 或更高版本的 Visual Studio 2008，我們可以使用內建單元測試的 IDE 支援，在 Visual Studio 測試專案：

![](create-a-new-aspnet-mvc-project/_static/image15.png)

選擇其中一個以上的選項，將會開啟 [測試結果] 窗格中的，在 IDE 中，並提供我們 27 的單元測試包含在我們新的專案，其中涵蓋內建的功能上的通過/失敗狀態：

![](create-a-new-aspnet-mvc-project/_static/image16.png)

本教學課程稍後我們將深入說明自動化測試，並新增其他單元測試，其中涵蓋我們實作應用程式的功能。

### <a name="next-step"></a>下一個步驟

我們現在有基本的應用程式結構就緒。 讓我們現在[建立資料庫來儲存我們的應用程式資料](create-a-database.md)。

> [!div class="step-by-step"]
> [上一頁](introducing-the-nerddinner-tutorial.md)
> [下一頁](create-a-database.md)
