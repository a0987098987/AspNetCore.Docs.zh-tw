---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 第 3 部分： 版面配置與分類 功能表 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 3 部分涵蓋如何加入版面配置及類別目錄功能表項目。
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: f55b29a271dbdb72d3e2249ed74517b77d78cf5e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823634"
---
<a name="part-3-layout-and-category-menu"></a>第 3 部分： 版面配置與分類 功能表
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 3 部分涵蓋如何加入版面配置及類別目錄功能表項目。


## <a id="_Toc260221669"></a>  新增一些版面配置和類別目錄功能表

在我們網站的主版頁面中，我們將新增包含我們的產品類別目錄 功能表左邊資料行的 div。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

請注意，所需的對齊和其他格式會由我們加入我們的 Style.css 檔案的 CSS 類別。

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

[產品類別目錄] 功能表將會以動態方式建立在執行階段藉由查詢商務資料庫的現有產品類別目錄以及建立功能表項目和對應的連結之用。

若要這麼做，我們將使用兩個 ASP。NET 的功能強大的資料控制項。 「 實體資料來源 」 控制項和"ListView"控制項。

![](tailspin-spyworks-part-3/_static/image1.jpg)

讓我們切換到 [設計檢視]，並設定控制項中使用協助程式。

![](tailspin-spyworks-part-3/_static/image2.jpg)

讓我們將 EntityDataSource 識別碼屬性設定為 EDS\_分類\_功能表，然後按一下 「 設定資料來源 」。

![](tailspin-spyworks-part-3/_static/image3.jpg)

選取我們建立我們商務資料庫的實體資料來源模型時，為我們所建立的 CommerceEntities 連接，然後按一下 下一步 」。

![](tailspin-spyworks-part-3/_static/image4.jpg)

選取 「 類別 」 實體集名稱，然後將其餘的選項為預設值。 按一下 [完成]。

現在讓我們設定我們放在我們的頁面，即可 ListView 的 ListView 控制項執行個體的 ID 屬性\_ProductsMenu 並啟用其協助程式。

![](tailspin-spyworks-part-3/_static/image5.jpg)

不過我們可以使用 控制選項來格式化資料的項目顯示和格式化，我們功能表建立只需要簡單的標記讓我們將輸入來源檢視中的程式碼。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

請注意"Eval"陳述式： &lt;# Eval("CategoryName") %&gt;

使用 ASP.NET 語法&lt;%# %&gt;是指示執行階段執行任何內含和輸出的結果"Line"的縮寫慣例。

陳述式 Eval("CategoryName")，指示目前項目的繫結集合中的資料項目時，擷取實體模型項目名稱的值"CatagoryName 」。 這是簡潔的語法非常強大的功能。

讓我們執行應用程式現在。

![](tailspin-spyworks-part-3/_static/image6.jpg)

請注意，現在會顯示我們的產品類別目錄 功能表，並當我們將滑鼠停留在其中一個類別目錄功能表項目，我們可以看到此功能表項目連結指向頁面，我們必須尚未實作名為 ProductsList.aspx，且我們已建置動態查詢字串引數，包含 類別目錄識別碼。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-2.md)
> [下一頁](tailspin-spyworks-part-4.md)
