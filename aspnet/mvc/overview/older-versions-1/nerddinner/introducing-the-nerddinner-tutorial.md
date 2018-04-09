---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: 簡介 NerdDinner 教學課程 |Microsoft 文件
author: shanselman
description: 若要深入了解新的架構，最好是建立它的項目。 本教學課程逐步解說如何建置使用 ASP.NE 很小，但完整的應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a>簡介 NerdDinner 教學課程
====================
由[Scott Hanselman](https://github.com/shanselman)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 若要深入了解新的架構，最好是建立它的項目。 本教學課程會逐步解說如何建置小，但是完成，應用程式使用 ASP.NET MVC 1，並介紹一些背後的核心概念。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-tutorial"></a>NerdDinner 教學課程

若要深入了解新的架構，最好是建立它的項目。 本教學課程會逐步解說如何建置小，但是完成，請使用 ASP.NET MVC 應用程式，並介紹一些背後的核心概念。

我們即將建置的應用程式稱為 「 NerdDinner"。 NerdDinner 提供簡單的方法來尋找和組織線上 dinners 障礙人士：

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner 可讓您建立、 編輯和刪除 dinners 註冊的使用者。 它會強制一致的驗證和商務規則集執行整個應用程式：

![](introducing-the-nerddinner-tutorial/_static/image2.png)

訪客可以使用以 AJAX 為基礎的對應搜尋即將 dinners 持有附近它們：

![](introducing-the-nerddinner-tutorial/_static/image3.png)

按一下 dinner 會移到詳細資料頁面它們可以在哪裡取得詳細資訊，請參閱：

![](introducing-the-nerddinner-tutorial/_static/image4.png)

如果他們想要參加的 dinner 他們可以登入，或在網站上註冊：

![](introducing-the-nerddinner-tutorial/_static/image5.png)

然後，他們就可以按一下參加事件以 AJAX 為基礎的 RSVP 連結：

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>實作 NerdDinner

我們即將開始我們 NerdDinner 應用程式使用的檔案-&gt;Visual Studio 中建立新的 ASP.NET MVC 專案的新專案命令。 我們會再以累加方式加入功能與特性。 在過程中，我們將討論：

1. [如何建立新的 ASP.NET MVC 專案](# "建立新的 ASP.NET MVC 專案")
2. [如何建立資料庫](# "建立資料庫")
3. [如何建立使用商務規則驗證模型](# "建立使用商務規則驗證模型")
4. [如何使用控制器和檢視來實作清單/詳細資料 UI](# "使用控制器和檢視來實作詳細資料清單/UI")
5. [如何提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援](# "提供 CRUD （建立、 讀取、 更新、 刪除） 資料表單項目支援")
6. [如何使用別的 ViewData 及實作 ViewModel 類別](# "使用別的 ViewData 和實作 ViewModel 類別")
7. [如何重複使用 UI 使用主版頁面和 partials](# "重複使用 UI 使用主版頁面和 Partials")
8. [如何實作有效率的資料分頁](# "實作有效率的資料分頁")
9. [如何保護應用程式使用驗證和授權](# "安全的應用程式使用驗證和授權")
10. [如何動態更新使用 AJAX](# "使用 AJAX 傳送動態更新")
11. [如何實作對應實例中使用 AJAX](# "使用 AJAX 實作對應案例")
12. [如何啟用自動化的單元測試](# "啟用自動化單元測試")

您可以建立自己的 NerdDinner 從頭完成每個步驟我們在本章中的逐步解說。 或者，您可以下載完整的版的原始碼： [GitHub 上的 NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)。 您也可以選擇性地也可以[下載免費的 PDF 版本，本教學課程的](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)如果您想要讀取的離線教學課程。

您可以使用 Visual Studio 2008 或免費 Visual Web Developer 2008 Express 建置應用程式。 您可以使用 SQL Server 或免費的 SQL Server Express 資料庫。

您可以安裝 ASP.NET MVC、 Visual Web Developer 2008 Express，以及 SQL Server Express （免費） 使用的 V2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>現在讓我們開始吧...

現在我們已經探討 NerdDinner 是什麼，讓我們來彙總我們套筒它們套並撰寫一些程式碼。

我們一開始會使用檔案-&gt;建立 NerdDinner 應用程式的 Visual Studio 中的新專案。

> [!div class="step-by-step"]
> [下一步](create-a-new-aspnet-mvc-project.md)
