---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 第 1 部分： 檔案]-> [新增專案 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案/新增的專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 8c346e88277b6cf43676f7c6b58f9679a591d634
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388207"
---
<a name="part-1-file--new-project"></a>第 1 部分： 檔案]-> [新增專案
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案/新增的專案。


## <a id="_Toc260221666"></a>  概觀

本教學課程是 ASP.NET WebForms 的簡介。 我們會慢慢開始，因此層級的 web 開發初學者體驗也沒關係。

我們將建置的應用程式是簡單的線上商店。

![](tailspin-spyworks-part-1/_static/image1.jpg)


訪客可以瀏覽產品分類：

![](tailspin-spyworks-part-1/_static/image2.jpg)

他們可以檢視單一產品，並將它新增至購物車：

![](tailspin-spyworks-part-1/_static/image3.jpg)

它們可以檢閱其購物車，並移除不再需要的任何項目：

![](tailspin-spyworks-part-1/_static/image4.jpg)

繼續簽出將會提示他們

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

之後排序，就會看到簡單的確認畫面：

![](tailspin-spyworks-part-1/_static/image7.jpg)


我們一開始先在 Visual Studio 2010 中，建立新的 ASP.NET WebForms 專案，我們會以累加方式新增功能來建立完整的運作應用程式。 過程中，我們將討論資料庫存取權、 清單和格線檢視、 資料更新頁面、 資料驗證、 主版頁面使用一致的頁面配置、 AJAX、 驗證、 使用者成員資格，等等。

您可以照著逐步進行，或者您可以下載從完成的應用程式 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

您可以使用 Visual Studio 2010 或從可用 Visual Web Developer 2010 [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/)。 若要建置應用程式，您可以使用 SQL Server 或免費的 SQL Server Express 來裝載資料庫。

## <a id="_Toc260221667"></a>  檔案 / 新增專案

我們一開始是從 Visual Studio 中的 [檔案] 功能表中選取新的專案。 這會顯示 [新增專案] 對話方塊。

![](tailspin-spyworks-part-1/_static/image8.jpg)

我們將選取的 Visual C# / Web 範本左側的群組，然後選擇 在中間欄中的 「 ASP.NET Web 應用程式 」 範本。 您的專案 TailspinSpyworks 命名，然後按 [確定] 按鈕。

![](tailspin-spyworks-part-1/_static/image9.jpg)

這會建立我們的專案。 讓我們看看我們在右邊的 [方案總管] 中的應用程式中包含的資料夾。

![](tailspin-spyworks-part-1/_static/image10.jpg)

空的方案不是完全空白-它會新增一個基本的資料夾結構：

![](tailspin-spyworks-part-1/_static/image1.png)

請注意 ASP.NET 4 預設專案範本所實作的慣例。

- 「 帳戶 」 資料夾 ASP 實作基本的使用者介面。NET 的成員資格的子系統。
- 「 指令碼 」 資料夾做為用戶端 JavaScript 檔案的儲存機制和核心 jQuery.js 檔案所能使用預設值。
- 「 樣式 」 資料夾用來組織我們網站上的視覺效果 （CSS 樣式表）

當我們按下 f5 鍵執行應用程式，並呈現 default.aspx 頁面時看到下列項目。

![](tailspin-spyworks-part-1/_static/image11.jpg)

我們的第一個應用程式增強功能是要取代的 CSS 類別和相關聯的映像檔，會呈現 visual asthetics，我們想我們的 Tailspin Spyworks 應用程式的預設 WebForms 範本中的 Style.css 檔案。

完成之後，我們的 default.aspx 網頁會轉譯如下。

![](tailspin-spyworks-part-1/_static/image12.jpg)

請注意頂端的影像連結權限的頁面和已加入主版頁面的功能表項目。 只有 登入 」 和 「 帳戶 」 連結指向存在 （預設範本所產生） 的頁面和我們建置的應用程式，我們會實作頁面的其餘部分。

我們也要將主版頁面重新放置到樣式目錄。 但這是喜好設定它可能會讓有點輕鬆地如果我們決定讓我們的應用程式 「 skinable"未來使用。

之後這樣我們就必須變更主版頁面中所有的.aspx 檔案的參考所產生的預設 ASP.NET WebForms 頁面。

> [!div class="step-by-step"]
> [下一步](tailspin-spyworks-part-2.md)
