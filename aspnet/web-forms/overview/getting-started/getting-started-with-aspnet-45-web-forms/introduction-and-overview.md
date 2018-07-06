---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013 |Microsoft Docs
author: Erikre
description: 此逐步教學課程系列將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: cda0c6239e2f10186641ab315837440d83b2fda6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841392"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2013
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此逐步教學課程系列將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 [ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> 測試您的知識並藉由採取 ASP.NET Web Form 測驗加強重要概念。 這項測驗被專從本教學課程系列中所包含的內容。 在測驗中的每個問題時提供說明以及其他指引的連結。


## <a name="introduction"></a>簡介

這一系列的教學課程會引導您完成建立 ASP.NET Web Forms 應用程式使用 Visual Studio Express 2013 for Web 和 ASP.NET 4.5 所需的步驟。

應用程式，您將建立名為**Wingtip Toys**。 這是簡化的範例銷售項目線上存放區的前端 web 站台。 本教學課程系列會反白顯示 ASP.NET 4.5 中所提供的新功能。

註解是歡迎畫面中，我們會盡所有努力更新本教學課程系列，根據您的建議。

### <a name="download-completed-project"></a>下載已完成專案

您可以下載 C# 專案，其中包含已完成本教學課程。

- [開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>檢閱的內容採取相關的 ASP.NET Web Form 測驗

完成本教學課程之後，測試您的知識，並強化採取的重要概念[ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)。 這項測驗被專從本教學課程系列中所包含的內容。 在測驗中的每個問題時提供說明以及其他指引的連結。

- [ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>適用對象

本教學課程系列的對象是有經驗的開發人員不熟悉 ASP.NET Web Form。 在本教學課程系列中，您想要的開發人員應該具備下列技術：

- 熟悉物件導向程式設計 (OOP) 語言
- 熟悉 Web 程式開發概念 (HTML、 CSS、 JavaScript)
- 熟悉關聯式資料庫概念
- 熟悉多層式架構概念

如果您有興趣檢閱上面所列的區域，請考慮檢閱下列內容：

- [Getting Started with Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web 開發](https://msdn.microsoft.com/beginner/bb308760.aspx)， [HTML、 CSS、 JavaScript、 SQL、 PHP、 JQuery](http://w3schools.com/)
- [關聯式資料庫](http://en.wikipedia.org/wiki/Relational_database)
- [多層式架構](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>應用程式功能

在這一系列提供的 ASP.NET Web Form 功能包括：

- Web 應用程式專案 （不是網站專案）
- Web Form
- 主版頁面設定
- 啟動程序
- Entity Framework Code First，LocalDB
- 要求驗證
- 強型別資料控制項中，模型繫結，資料註解，以及值提供者
- SSL 和 OAuth
- ASP.NET 身分識別、 組態和授權
- 低調驗證
- 路由
- ASP.NET 錯誤處理

### <a name="application-scenarios-and-tasks"></a>應用程式案例和工作

本系列中所示範的工作包括：

- 建立、 檢閱和執行新的專案
- 建立資料庫結構
- 初始化並植入資料庫
- 自訂使用樣式、 圖形和主版頁面 UI
- 加入頁面和導覽
- 顯示功能表的詳細資訊和產品資料
- 建立購物車
- 新增 SSL 和 OAuth 支援
- 新增付款方法
- 包括系統管理員角色和使用者使用應用程式
- 限制存取特定頁面以及資料夾
- 將檔案上傳至 web 應用程式
- 實作輸入的驗證
- 註冊 web 應用程式的路由
- 實作錯誤處理和錯誤記錄

## <a name="overview"></a>總覽

如果您不熟悉 ASP.NET Web Form，但沒有熟悉程式設計概念，您有正確的教學課程。 如果您已熟悉 ASP.NET Web Form，您可以直接從本教學課程系列 ASP.NET 4.5 中所提供的新功能。 如果您不熟悉程式設計概念和 ASP.NET Web Form，請參閱 Web Form 中提供的其他教學課程[開始使用](../../../index.md)ASP.NET 網站上的一節。

特定**最新**ASP.NET 4.5 功能提供在此 Web Form 中的教學課程系列包括下列：

- 建立簡單的 UI 專案該供應項目[支援多個 ASP.NET 架構](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web Form、 MVC 和 Web API）。
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)，版面配置和佈景主題的架構，可提供回應式設計和佈景主題的功能。
- [ASP.NET Identity](../../../../identity/index.md)，新的 ASP.NET 成員資格系統，與 web 裝載軟體，而不是 IIS 的運作方式在所有 ASP.NET framework 和運作方式相同。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)、 更新 Entity Framework 可讓您擷取和操作資料做為強型別的物件、 存取資料以非同步的方式，處理暫時性連接失敗，以及記錄 SQL 陳述式。

ASP.NET 4.5 功能的完整清單，請參閱 < [ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../../visual-studio/overview/2013/release-notes.md)。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys 範例應用程式

下列螢幕擷取畫面會提供您將在本教學課程系列中建立 ASP.NET Web forms 應用程式的快速檢視。 當您從 Visual Studio Express 2013 for Web 中執行應用程式時，您會看到下列 web 首頁。

![Wingtip Toys-預設頁面](introduction-and-overview/_static/image1.png)

您可以註冊為新的使用者，或現有的使用者身分登入。 瀏覽是由從資料庫擷取可用的產品，在頂端提供每個產品類別。

藉由選取 [產品] 連結，您將能夠看到所有可用產品清單。

![Wingtip Toys-產品](introduction-and-overview/_static/image2.png)

您也可以選取任何列出的產品，以查看個別的產品詳細資料。

![Wingtip Toys-產品詳細資料](introduction-and-overview/_static/image3.png)

身為使用者，您可以註冊及登入使用 Web 表單範本的預設功能。 本教學課程也說明如何使用現有的 Gmail 帳戶登入。 此外，您可以登入為系統管理員新增和移除資料庫中的產品。

![Wingtip Toys-登入](introduction-and-overview/_static/image4.png)

一旦您已登入的使用者身分，您可以新增至購物車及結帳使用 PayPal 的產品。 請注意，此範例應用程式設計來透過 PayPal 的開發人員沙箱函式。 沒有實際的金錢交易就會進行。

![Wingtip Toys-購物車](introduction-and-overview/_static/image5.png)

PayPal 會確認您的帳戶、 訂單和付款資訊。

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

傳回從 PayPal 之後, 您可以檢閱並完成您的訂單。

![Wingtip Toys-順序檢閱](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必要條件

在開始之前，請確定您已在電腦上安裝下列軟體：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或是[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 .NET Framework 會自動安裝。

本教學課程系列會使用 Microsoft Visual Studio Express 2013 for Web。 您可以使用 Microsoft Visual Studio Express 2013 for Web 或 Microsoft Visual Studio 2013，才能完成本教學課程系列。

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常稱為 Visual Studio 在此教學課程系列。


如果您已經安裝 Visual Studio 版本，安裝程序會安裝 Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web 的現有版本旁邊。 您在舊版中建立的站台可以在 Visual Studio 2013 中開啟，並繼續在舊版中開啟。

> [!NOTE] 
> 
> 本逐步解說假設您已經選擇*Web 開發*設定集合，第一次您啟動 Visual Studio。 如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。


## <a name="download-the-sample-application"></a>下載範例應用程式

安裝必要條件之後, 您已準備好開始本教學課程系列中建立新的 Web 專案所呈現項目。 如果您想要**選擇性地**執行本教學課程系列會建立範例應用程式，您可以下載從 MSDN 範例網站。 此下載包含下列內容：

- 中的範例應用程式*WingtipToys*資料夾。
- 若要建立範例應用程式中的使用的資源*WingtipToys 資產*中的資料夾*WingtipToys*資料夾。

#### <a name="download-the-file-from-msdn-samples-site"></a>從 MSDN 範例網站下載檔案：

[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

下載<em>.zip</em>檔案。 若要查看已完成本教學課程系列所建立的專案，尋找並選取<em>C#</em>中的資料夾<em>.zip</em>檔案。 儲存<em>C#</em> folderto 您使用以使用 Visual Studio 2013 專案的資料夾。 根據預設，Visual Studio 2013 的 [專案] 資料夾如下：

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

重新命名***C#*** 資料夾，以***WingtipToys***。

> [!NOTE]
> 如果您已經有一個名為資料夾*WingtipToys*在專案資料夾中，暫時重新命名該現有的資料夾重新命名之前*C#* 資料夾*WingtipToys*。


若要執行已完成的專案，開啟*WingtipToys*資料夾，然後按兩下*WingtipToys.sln*檔案。 Visual Studio 2013 將開啟的專案。 接下來，以滑鼠右鍵按一下*Default.aspx*檔案在 [方案總管] 視窗中，並從滑鼠右鍵功能表中按一下 瀏覽器中檢視。

### <a name="tutorial-support-and-comments"></a>教學課程的支援和註解

使用隨附的問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例的任何問題或意見。

在此教學課程系列的註解是 褖畫惎，且致力此教學課程系列更新時進行納入帳戶更正或建議的教學課程的註解中所提供的增強功能。

在開發期間，發生錯誤時，或如果網站不會正確執行，可能讓問題的來源複雜線索的錯誤訊息，或可能不會說明如何修正此問題。 為了協助您利用一些常見的問題案例，您也可以使用[ASP.NET 論壇](https://forums.asp.net/)或隨附的 [問與答] 區段[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必查看上述位置。

> [!div class="step-by-step"]
> [下一步](create-the-project.md)
