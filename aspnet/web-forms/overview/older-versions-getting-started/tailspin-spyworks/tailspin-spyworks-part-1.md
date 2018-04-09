---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 第 1 部分： 檔案]-> [新增專案 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案/新增的專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-1-file--new-project"></a>第 1 部分： 檔案]-> [新增專案
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案/新增的專案。


## <a id="_Toc260221666"></a>  概觀

本教學課程是 ASP.NET WebForms 的簡介。 我們將會啟動慢，因此初學者層級的 web 程式開發體驗也沒關係。

我們會建置應用程式是一個簡單的線上存放區。

![](tailspin-spyworks-part-1/_static/image1.jpg)


訪客可以瀏覽依分類的產品：

![](tailspin-spyworks-part-1/_static/image2.jpg)

他們可以檢視單一產品，並將它加入至購物車：

![](tailspin-spyworks-part-1/_static/image3.jpg)

它們可以檢閱其購物車，並移除不再需要的任何項目：

![](tailspin-spyworks-part-1/_static/image4.jpg)

繼續簽出將會提示使用者

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

排序之後，他們會看到簡單確認畫面：

![](tailspin-spyworks-part-1/_static/image7.jpg)


我們一開始會藉由在 Visual Studio 2010 中，建立新的 ASP.NET WebForms 專案，我們將以累加方式新增至建立的完整運作應用程式的功能。 過程中，我們將討論資料庫存取權、 清單和格線檢視、 資料更新頁面、 資料驗證、 主版頁面使用一致的頁面配置、 AJAX、 驗證、 使用者成員資格，等等。

您可以展開冒險逐步解說，或者，您可以下載從完成的應用程式 [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

您可以使用 Visual Studio 2010 或從可用 Visual Web Developer 2010 [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/)。 若要建置應用程式，您可以使用 SQL Server 或免費 SQL Server Express 來裝載資料庫。

## <a id="_Toc260221667"></a>  檔案 / 新增專案

我們會先從 Visual Studio 中的 [檔案] 功能表中選取新的專案。 這會開啟 [新增專案] 對話方塊。

![](tailspin-spyworks-part-1/_static/image8.jpg)

我們將選取的 Visual C# / 網站範本左邊的群組，然後選擇 中心資料行中的 「 ASP.NET Web 應用程式 」 範本。 TailspinSpyworks 您專案的名稱，然後按 [確定] 按鈕。

![](tailspin-spyworks-part-1/_static/image9.jpg)

這會建立專案。 讓我們看看我們在右邊的 [方案總管] 中的應用程式中包含的資料夾。

![](tailspin-spyworks-part-1/_static/image10.jpg)

空白的方案不是完全空白 – 它會加入基本資料夾結構：

![](tailspin-spyworks-part-1/_static/image1.png)

請注意在 ASP.NET 4 預設專案範本所實作的慣例。

- 「 帳戶 」 資料夾 ASP 實作基本的使用者介面。網路的成員資格的子系統。
- 「 指令碼 」 資料夾做為用戶端 JavaScript 檔案的儲存機制並核心 jQuery.js 檔案依預設會提供。
- 「 樣式 」 資料夾用來組織在網站上的視覺效果 （CSS 樣式表）

當我們按下 F5 執行應用程式，並呈現 default.aspx 頁面上看到下列項目。

![](tailspin-spyworks-part-1/_static/image11.jpg)

我們第一個應用程式增強功能會以 CSS 類別和相關聯的映像檔，在我們想要我們 Tailspin Spyworks 應用程式 visual asthetics 取代預設 WebForms 範本中的 Style.css 檔案。

這樣做之後我們 default.aspx 頁面上呈現像這樣。

![](tailspin-spyworks-part-1/_static/image12.jpg)

請注意上方的影像連結方的頁面已加入至主版頁面的功能表項目。 只有 「 登入 」 及 「 帳戶 」 連結指向存在的頁面 （預設範本所產生） 和建置應用程式，我們會實作網頁的其餘部分。

我們也要將主版頁面重新放置到樣式目錄。 雖然這是喜好設定它可能方便的方式稍有如果我們決定要在未來執行我們的應用程式"skinable"。

之後這樣我們就必須變更主版頁面中所有的.aspx 檔案參考產生預設的 ASP.NET WebForms 網頁。

> [!div class="step-by-step"]
> [下一步](tailspin-spyworks-part-2.md)
