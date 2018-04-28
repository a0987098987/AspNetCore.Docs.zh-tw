---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: 開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013 |Microsoft 文件
author: Erikre
description: 此逐步教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: a3527b54d1936bc14e32a1828ac3a2be625107ba
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此逐步教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 [ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> 測試您的知識，並利用 ASP.NET Web Form 測驗強調的重要概念。 這個小測驗已特別設計從包含在此教學課程系列的內容。 在測驗中的每個問題解釋以及其他指引連結。


## <a name="introduction"></a>簡介

這一系列的教學課程會引導您建立使用 Visual Studio Express 2013 for Web 和 ASP.NET 4.5 的 ASP.NET Web Form 應用程式所需的步驟。

應用程式，您會建立名為**Wingtip Toys**。 這是簡化的存放區前端網站購得線上項目範例。 此教學課程系列會反白顯示 ASP.NET 4.5 中新的功能。

註解是歡迎畫面中，我們將請致力將根據您的建議此教學課程系列的更新。

### <a name="download-completed-project"></a>下載完成的專案

您可以下載的 C# 專案，其中包含已完成本教學課程。

- [開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>擷取相關的 ASP.NET Web Form 測驗，藉此檢閱的內容

完成本教學課程之後，測試您的知識，並強化採取的重要概念[ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)。 這個小測驗已特別設計從包含在此教學課程系列的內容。 在測驗中的每個問題解釋以及其他指引連結。

- [ASP.NET Web Form 測驗](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>適用對象

此教學課程系列的對象為經驗豐富的開發人員不熟悉 ASP.NET Web Form。 此教學課程系列有興趣的開發人員應該具備下列技術：

- 熟悉的物件導向程式設計 (OOP) 語言
- 熟悉 Web 開發概念 (HTML、 CSS、 JavaScript)
- 熟悉關聯式資料庫概念
- 熟悉多層式架構概念

如果您有興趣檢視上面所列的區域，請考慮檢閱下列內容：

- [使用 Visual C# 使用者入門](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web 開發](https://msdn.microsoft.com/beginner/bb308760.aspx)， [HTML、 CSS、 JavaScript、 SQL、 PHP、 JQuery](http://w3schools.com/)
- [關聯式資料庫](http://en.wikipedia.org/wiki/Relational_database)
- [多層式架構](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>應用程式功能

顯示此數列中的 ASP.NET Web Form 功能包括：

- Web 應用程式專案 （不是網站專案）
- Web Form
- 主版頁面設定
- 啟動程序
- Entity Framework Code First，LocalDB
- 要求驗證
- 強型別資料控制項建立的模型繫結資料註解，且值提供者
- SSL 和 OAuth
- ASP.NET 識別、 組態和授權
- 不顯眼的驗證
- 路由
- ASP.NET 錯誤處理

### <a name="application-scenarios-and-tasks"></a>應用程式案例和工作

示範在這一系列的工作包括：

- 建立、 檢視和執行新的專案
- 建立資料庫結構
- 初始化並植入資料庫
- 自訂使用樣式、 圖形和主版頁面 UI
- 加入頁面和導覽
- 顯示功能表的詳細資料與產品資料
- 建立購物車
- 加入 SSL 和 OAuth 支援
- 新增付款方式
- 包括系統管理員角色和應用程式的使用者
- 限制存取特定頁面以及資料夾
- 將檔案上傳至 web 應用程式
- 實作的輸入的驗證
- 註冊 web 應用程式的路由
- 實作錯誤處理與錯誤記錄

## <a name="overview"></a>總覽

如果您不熟悉 ASP.NET Web Form 熟悉程式設計概念，但沒有，您有正確的教學課程。 如果您已經熟悉 ASP.NET Web Form，您可以受益於此教學課程系列由 ASP.NET 4.5 的新功能。 如果您不熟悉程式設計概念和 ASP.NET Web Form，請參閱 Web Form 中提供的其他教學課程[入門](../../../index.md)ASP.NET 網站上的一節。

特定**最新**ASP.NET 4.5 功能提供此 Web Form 中的教學課程包括下列：

- 建立的簡單 UI 專案該優惠[支援多個 ASP.NET 架構](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web Forms、 MVC、 及 Web 應用程式開發介面）。
- [啟動程序](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)，版面配置和佈景主題的架構，它提供回應式設計和主題功能。
- [ASP.NET Identity](../../../../identity/index.md)，新的 ASP.NET 成員資格系統，一樣會在所有 ASP.NET 架構和運作方式與 web 裝載 IIS 以外的軟體。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)、 更新至 Entity Framework 可讓您擷取和操作資料做為強型別的物件、 存取資料以非同步方式處理暫時性連接失敗，以及記錄的 SQL 陳述式。

如需完整的 ASP.NET 4.5 功能清單，請參閱[ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../../visual-studio/overview/2013/release-notes.md)。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys 範例應用程式

下列螢幕擷取畫面會提供您將建立此教學課程系列的 ASP.NET Web form 應用程式的快速檢視。 當您從 Visual Studio Express 2013 for Web 執行應用程式時，您會看到下列 web 首頁。

![Wingtip Toys-預設頁面](introduction-and-overview/_static/image1.png)

您可以註冊為新的使用者，或現有的使用者身分登入。 導覽是由從資料庫擷取可用的產品，在頂端提供每個產品類別目錄。

藉由選取 [產品] 連結，您將能夠看到一份所有可用產品。

![Wingtip Toys-產品](introduction-and-overview/_static/image2.png)

您也可以選取任何列出的產品，以查看個別的產品詳細資料。

![Wingtip Toys-產品詳細資料](introduction-and-overview/_static/image3.png)

身為使用者，您可以註冊及登入使用的 Web 表單範本的預設功能。 本教學課程也說明如何使用現有的 Gmail 帳戶登入。 此外，您可以登入為系統管理員新增和移除資料庫中的產品。

![Wingtip Toys-登入](introduction-and-overview/_static/image4.png)

一旦您的使用者身分登入，您可以將產品加入購物車和 PayPal 的簽出。 請注意此範例應用程式為了與 PayPal 的開發人員沙箱函式。 沒有實際 money 交易就會進行。

![Wingtip Toys-購物車](introduction-and-overview/_static/image5.png)

PayPal 將會確認您的帳戶、 順序和付款資訊。

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

傳回從 PayPal 之後, 您可以檢閱並完成您的訂單。

![Wingtip Toys-順序檢閱](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必要條件

開始之前，請確定您已在電腦上安裝下列軟體：

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)或[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)。 會自動安裝.NET Framework。

此教學課程的數列會使用 Microsoft Visual Studio Express 2013 for Web。 若要完成此教學課程系列，您可以使用 Microsoft Visual Studio Express 2013 for Web，或是 Microsoft Visual Studio 2013。

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 和 Microsoft Visual Studio Express 2013 for Web 將通常被稱為 Visual Studio 在此教學課程系列。


如果您已經有安裝 Visual Studio 版本，安裝程序會安裝 Visual Studio 2013 或 Microsoft Visual Studio Express 2013 for Web 旁邊的現有版本。 在舊版中所建立的站台可以在 Visual Studio 2013 中開啟，並繼續在先前版本中開啟。

> [!NOTE] 
> 
> 本逐步解說假設您已經選擇*Web 程式開發*設定的集合，您會啟動 Visual Studio 的第一次。 如需詳細資訊，請參閱[如何： 選取 Web 程式開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。


## <a name="download-the-sample-application"></a>下載範例應用程式

正在安裝的必要條件之後, 您就可以開始本教學課程系列建立新的 Web 專案所呈現項目。 如果您想要**選擇性**執行此教學課程建立範例應用程式，您可以從 MSDN 範例網站下載。 此下載包含下列內容：

- 中的範例應用程式*WingtipToys*資料夾。
- 若要建立範例應用程式中的使用的資源*WingtipToys 資產*資料夾中的*WingtipToys*資料夾。

#### <a name="download-the-file-from-msdn-samples-site"></a>從 MSDN 範例網站下載檔案：

[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

下載<em>.zip</em>檔案。 若要查看已完成此教學課程建立的專案，尋找並選取<em>C#</em>資料夾中的<em>.zip</em>檔案。 儲存<em>C#</em> folderto 您用於使用 Visual Studio 2013 專案的資料夾。 根據預設，Visual Studio 2013 專案資料夾，如下所示：

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

重新命名***C#*** 資料夾***WingtipToys***。

> [!NOTE]
> 如果您已經有一個名為資料夾*WingtipToys*在專案資料夾中，暫時重新命名該現有的資料夾重新命名之前*C#* 資料夾*WingtipToys*。


若要執行已完成的專案，開啟*WingtipToys*資料夾，然後按兩下*WingtipToys.sln*檔案。 Visual Studio 2013 將開啟的專案。 接下來，以滑鼠右鍵按一下*Default.aspx*檔案在 [方案總管] 視窗中，並從滑鼠右鍵功能表中按一下 瀏覽器中檢視。

### <a name="tutorial-support-and-comments"></a>教學課程的支援和註解

使用隨附於 Q AND A 區段[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例的任何問題或意見。

在此教學課程系列的註解是 褖畫惎，且致力此教學課程系列更新時進行納入帳戶更正或建議的教學課程的註解中所提供的增強功能。

在開發期間，就會發生錯誤時，或未正確執行的網站，錯誤訊息可能會提供問題的來源複雜線索，或可能不會說明如何修正此問題。 若要協助您解決一些常見的問題案例，您也可以使用[ASP.NET 論壇](https://forums.asp.net/)或隨附 Q AND A 區段[開始使用 ASP.NET 4.5 Web Form 和 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例。 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必查看上述位置。

> [!div class="step-by-step"]
> [下一步](create-the-project.md)
