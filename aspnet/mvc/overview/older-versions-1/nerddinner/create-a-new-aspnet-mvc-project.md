---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: 建立新的 ASP.NET MVC 專案 |Microsoft 文件
author: microsoft
description: 步驟 1 會示範如何將基本 NerdDinner 應用程式結構放在位置。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-a-new-aspnet-mvc-project"></a>建立新的 ASP.NET MVC 專案
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 1 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 1 會示範如何將基本 NerdDinner 應用程式結構放在位置。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner 步驟 1： 檔案-&gt;新專案

我們一開始我們 NerdDinner 應用程式會藉由選取**檔案-&gt;新專案**Visual Studio 2008 或免費 Visual Web Developer 2008 Express 中的功能表項目。

這會顯示 [新增專案] 對話方塊。 若要建立新的 ASP.NET MVC 應用程式，我們會選取對話方塊左側的"Web"節點，然後選擇 ["ASP.NET MVC Web 應用程式] 專案範本，在右邊：

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*重要事項： 請確定您已下載並安裝 ASP.NET MVC-否則它不會顯示在 [新增專案] 對話方塊。您可以使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)如果您尚未尚未安裝 (ASP.NET MVC 內可供使用 」 Web 平台-&gt;架構和執行階段 」 一節)。*

我們會將新的專案，我們將建立 「 NerdDinner"，然後按一下 「 確定 」 按鈕來建立它。

當我們按一下 [確定] Visual Studio 會顯示其他對話方塊，提示我們選擇建立新應用程式的單元測試專案。 此單元測試專案可讓我們來建立自動化的測試來驗證功能和行為的應用程式 (項目仍會詳細說明如何在此教學課程後面的待辦事項)。

![](create-a-new-aspnet-mvc-project/_static/image2.png)

上述的對話方塊中的 「 測試架構 」 下拉式清單中填入所有使用 ASP.NET MVC 單元測試專案範本在電腦上安裝。 您可以下載版本 NUnit、 MBUnit，和 XUnit。 也支援內建的 Visual Studio 單元測試架構。

*注意： Visual Studio 單元測試架構才可使用 Visual Studio 2008 專業版和更新版本。如果您使用 VS 2008 Standard Edition 或 Visual Web Developer 2008 Express，您必須下載並安裝 ASP.NET MVC 的 NUnit、 MBUnit 或 XUnit 延伸，以便顯示此對話方塊。如果沒有安裝任何測試架構，將不會顯示對話方塊。*

我們將使用預設 「 NerdDinner.Tests 」 名稱，我們建立時，測試專案，並使用 「 Visual Studio 單元測試 」 架構選項。 當我們按一下 Visual Studio 的 「 確定 」 按鈕將使用它的其中一個 web 應用程式，另一個用於我們的單元測試中的兩個專案就讓我們建立方案：

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>檢查 NerdDinner 目錄結構

當您使用 Visual Studio 建立新的 ASP.NET MVC 應用程式時，它會自動將檔案和目錄的數字加入專案：

![](create-a-new-aspnet-mvc-project/_static/image4.png)

預設的 ASP.NET MVC 專案有六個最上層目錄：

| **Directory** | **目的** |
| --- | --- |
| **/Controllers** | 您在其中放置處理 URL 要求的控制器類別 |
| **/Models** | 您在其中放置代表和操作資料的類別 |
| **/Views** | 您在其中放置所負責轉譯輸出 UI 範本檔案 |
| **/Scripts** | 您可在此將 JavaScript 程式庫檔案和指令碼 (.js) |
| **/Content** | 您在其中放置 CSS 和映像檔，以及其他非-動態/非-JavaScript 內容 |
| **/App\_Data** | 將資料檔案儲存在您要讀取/寫入。 |

ASP.NET MVC 不需要這個結構。 事實上，大型應用程式開發人員將通常分割應用程式設定跨多個專案，使其更易管理 (例如： 資料模型類別通常從移至個別的類別庫專案中的 web 應用程式)。 不過，預設專案結構，並提供好的預設目錄慣例我們可以用來保留全新我們的應用程式考量。

當我們展開 /Controllers 目錄，我們會發現，Visual Studio 兩個控制器類別 – HomeController 和 AccountController – 依預設加入至專案：

![](create-a-new-aspnet-mvc-project/_static/image5.png)

當我們展開 /Views 目錄時，我們會發現三個子目錄 – /Home、 /Account 和 /Shared – 以及數個範本，其中檔案預設也加入至專案：

![](create-a-new-aspnet-mvc-project/_static/image6.png)

當我們展開的 /Content 和 /Scripts 目錄時，我們也可以找到用來設定所有 HTML 都樣式，於網站上的 Site.css 檔案，以及可以啟用 ASP.NET AJAX 和 jQuery 的 JavaScript 程式庫支援應用程式中：

![](create-a-new-aspnet-mvc-project/_static/image7.png)

當我們展開 NerdDinner.Tests 專案時，我們也可以找到包含我們控制器類別的單元測試的兩個類別：

![](create-a-new-aspnet-mvc-project/_static/image8.png)

這些加入的 Visual Studio 的預設檔案工作應用程式層完成，但有關頁面、 帳戶登入/登出註冊頁面，以及未處理的錯誤頁面 （所有有線向上和現成提供的工作） 的首頁上，提供我們的基本結構。

### <a name="running-the-nerddinner-application"></a>執行 NerdDinner 應用程式

我們可以執行專案，選擇 **偵錯-&gt;開始偵錯**或**偵錯-&gt;啟動但不偵錯**功能表項目：

![](create-a-new-aspnet-mvc-project/_static/image9.png)

這會啟動內建 ASP.NET Web 伺服器隨附於 Visual Studio 中，並執行應用程式：

![](create-a-new-aspnet-mvc-project/_static/image10.png)

以下是我們的新專案的首頁 (URL:"/") 在執行時：

![](create-a-new-aspnet-mvc-project/_static/image11.png)

按一下 [關於] 索引標籤會顯示有關頁面 (URL:"/ 家用/關於 」):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

按一下右上方的 「 登入 」 連結會帶我們前往登入頁面 (URL:"/ 帳戶/登入 」)

![](create-a-new-aspnet-mvc-project/_static/image13.png)

如果我們沒有登入帳戶，我們可以按一下暫存器連結 (URL:"/ 帳戶/暫存器 」) 來建立一個：

![](create-a-new-aspnet-mvc-project/_static/image14.png)

程式碼來實作上述的首頁、 關於和登出 / 註冊當我們建立新專案時依預設加入的功能。 我們將使用它做為我們的應用程式的起點。

### <a name="testing-the-nerddinner-application"></a>測試 NerdDinner 應用程式

如果我們使用的專業版或更高版本的 Visual Studio 2008，我們可以使用的內建的單元測試 Visual Studio 中的 IDE 支援測試專案：

![](create-a-new-aspnet-mvc-project/_static/image15.png)

選擇其中一個上述選項會開啟 「 測試結果 」 窗格中的，在 IDE 中，並提供涵蓋的內建功能的 27 單元測試包含在新專案中的成功/失敗狀態：

![](create-a-new-aspnet-mvc-project/_static/image16.png)

稍後在本教學課程中我們將討論更多關於自動化測試，並新增其他單元測試，其中涵蓋應用程式功能，我們會實作。

### <a name="next-step"></a>下一個步驟

我們現在有基本應用程式結構中的位置。 我們現在[建立資料庫來儲存應用程式資料](create-a-database.md)。

> [!div class="step-by-step"]
> [上一頁](introducing-the-nerddinner-tutorial.md)
> [下一頁](create-a-database.md)
