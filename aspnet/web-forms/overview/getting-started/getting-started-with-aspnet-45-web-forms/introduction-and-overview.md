---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Getting Started with ASP.NET 4.7 Web Form 和 Visual Studio 2017 |Microsoft Docs
author: Erikre
description: 此逐步教學課程系列將教導您建置使用 ASP.NET 4.7 和 Microsoft Visual Studio 的 ASP.NET Web Forms 應用程式的基本概念
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341546"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Getting Started with ASP.NET 4.5 Web Form 和 Visual Studio 2017
====================

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

本系列教學課程會示範如何建置 ASP.NET 4.5 和 Microsoft Visual Studio 2017 的 ASP.NET Web Forms 應用程式。 

## <a name="introduction"></a>簡介

本教學課程系列會引導您完成建立 ASP.NET Web Forms 應用程式使用 Visual Studio 2017 和 ASP.NET 4.5。 您將建立名為應用程式**Wingtip Toys** -簡化店面網站銷售線上項目。 在系列中，ASP.NET 4.5 的新功能會反白顯示。

### <a name="target-audience"></a>目標對象

ASP.NET Web Form 的新的開發人員是本教學課程系列的目標對象。

您應該具備一些知識，在下列區域：

- 物件導向程式設計 (OOP) 和語言
- Web 程式開發 (HTML、 CSS、 JavaScript)
- 關聯式資料庫
- 多層式架構

若要檢閱這些區域，請考慮研究下列內容：

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
- 強型別資料控制項
- 模型繫結
- 資料註釋
- 值提供者
- SSL 和 OAuth
- ASP.NET 身分識別、 組態和授權
- 低調驗證
- 路由
- ASP.NET 錯誤處理

### <a name="application-scenarios-and-tasks"></a>應用程式案例和工作

教學課程系列的工作包括：

- 建立、 檢視和執行新的專案
- 建立資料庫的結構
- 初始化並植入資料庫
- 自訂樣式、 圖形和主版頁面的 UI
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

本教學課程系列是為人熟悉的程式設計概念，但新的 ASP.NET Web form。 如果您已熟悉 ASP.NET Web Form，這一系列仍有助於您了解新的 ASP.NET 4.5 功能。 如讀者熟悉程式設計概念和 ASP.NET Web Form，請參閱其他的 Web Form 教學課程中提供[開始使用](../../../index.md)ASP.NET 網站上的一節。

在本教學課程系列中提供的 ASP.NET 4.5 包含下列功能：

- 提供用於建立專案的簡單 UI[許多 ASP.NET 架構支援](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)（Web Form、 MVC 和 Web API）。
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)、 配置、 佈景主題，以及回應式設計架構。
- [ASP.NET Identity](../../../../identity/index.md)，新的 ASP.NET 成員資格系統，與 web 裝載軟體，而不是 IIS 的運作方式在所有 ASP.NET framework 和運作方式相同。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Entity Framework，讓您更新：
  - 擷取與操作資料當做強型別物件
  - 以非同步方式存取資料
  - 處理暫時性連接失敗
  - 記錄 SQL 陳述式

如需完整的 ASP.NET 4.5 功能清單，請參閱 < [ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../../visual-studio/overview/2013/release-notes.md)。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys 範例應用程式

下列螢幕擷取畫面是來自您在本教學課程系列中建立 ASP.NET Web Forms 應用程式。 當您在 Visual Studio 中執行應用程式時，下列 web 首頁隨即出現。

![Wingtip Toys-預設頁面](introduction-and-overview/_static/image1.png)

您可以註冊為新的使用者，或現有的使用者身分登入。 頂端導覽列有產品類別和其產品以從資料庫中的連結。

如果您選取**產品**，會顯示所有可用的產品。 

![Wingtip Toys-產品](introduction-and-overview/_static/image2.png)

如果您選取特定產品時，會顯示產品詳細資料。


![Wingtip Toys-產品詳細資料](introduction-and-overview/_static/image3.png)

身為使用者，您可以註冊及登入 Web 表單範本的預設功能。 本教學課程也說明如何使用現有的 Gmail 帳戶登入。 此外，您可以新增和移除資料庫中的產品的系統管理員身分登入。

![Wingtip Toys-登入](introduction-and-overview/_static/image4.png)

一旦您的使用者身分登入，您可以新增產品至購物車及使用 PayPal 簽出。 範例應用程式被設計用於 PayPal 的開發人員沙箱中。 沒有實際的金錢交易將會發生。

![Wingtip Toys-購物車](introduction-and-overview/_static/image5.png)

PayPal 確認您的帳戶、 訂單和付款資訊。

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

傳回從 PayPal 之後, 您可以檢閱並完成您的訂單。

![Wingtip Toys-順序檢閱](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必要條件

在開始之前，請確定您的電腦上安裝下列軟體：

- [Microsoft Visual Studio 2017 或 Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)。

.NET Framework 會自動安裝。

本教學課程系列會使用 Microsoft Visual Studio Community 2017。 您可以使用其中一個，或 Microsoft Visual Studio 2017 才能完成本教學課程系列。

請注意下列有關 Visual Studio:

* Microsoft Visual Studio 2017 和 Microsoft Visual Studio Community 2017 指*Visual Studio*在此教學課程系列。

* Visual Studio 2017 安裝已安裝任何舊版旁邊。 在舊版中建立的網站可以在 Visual Studio 2017 中開啟，並繼續在舊版中開啟。

* 第一次啟動 Visual Studio，則會假設您已選取*Web 開發*設定。 如需詳細資訊，請參閱[＜How to：選取 Web 開發環境設定](https://msdn.microsoft.com/library/ff521558.aspx)。

安裝必要條件之後, 您就可以開始建立本教學課程系列中的 Web 專案項目。

## <a name="download-the-sample-application"></a>下載範例應用程式

 您可以從 MSDN 範例網站，隨時下載完整的範例 applicatiion 在：

[開始使用 ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 此下載有下列項目：

- 中的範例應用程式*WingtipToys*資料夾。
- 若要建立範例應用程式中的使用的資源*WingtipToys 資產*中的資料夾*WingtipToys*資料夾。

下載 *.zip*檔案。 若要查看已完成本教學課程系列所建立的專案，尋找並選取*C#* .zip 檔案的資料夾中。 儲存C#您用來處理與 Visual Studio 專案的資料夾。 根據預設，Visual Studio 2017 的 [專案] 資料夾是：

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

重新命名***C#*** 資料夾，以***WingtipToys***。

> [!NOTE]
> 如果您已經有一個名為資料夾*WingtipToys*在專案資料夾中，暫時重新命名該現有的資料夾重新命名之前*C#* 資料夾*WingtipToys*。

若要執行已完成的專案，開啟*WingtipToys*資料夾，然後按兩下*WingtipToys.sln*檔案。 Visual Studio 2017 中開啟專案。 接下來，以滑鼠右鍵按一下*Default.aspx*中的檔案**方案總管**，然後選取**瀏覽器中檢視**。

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>ASP.NET Web Form 測驗來檢閱內容

完成之後的教學課程系列，測驗測試您的知識，並強化的重要概念。 每個問題提供的說明和其他指引的連結。

 * [ASP.NET Web Form 測驗](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>教學課程的支援和註解

問題和註解，請使用包含在問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例頁面。

在本教學課程系列的註解是歡迎畫面。 本教學課程系列更新時，盡量考慮更正或建議的改進。

如果發生錯誤，對應的錯誤訊息可能會造成混淆，沒有很好的說明，如何修正此問題。 如需協助，您可以檢查[ASP.NET 論壇](https://forums.asp.net/)。 另一個不錯的來源是中的問與答一節[Getting Started with ASP.NET 4.5 Web Form 與 Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 範例頁面。 

> [!div class="step-by-step"]
> [下一步](create-the-project.md)
