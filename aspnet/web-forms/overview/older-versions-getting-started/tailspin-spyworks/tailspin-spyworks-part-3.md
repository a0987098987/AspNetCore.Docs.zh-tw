---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 第 3 部分： 版面配置和類別目錄功能表 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 3 部分說明如何加入版面配置和類別目錄功能表。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-3-layout-and-category-menu"></a>第 3 部分： 版面配置和類別目錄功能表
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 3 部分說明如何加入版面配置和類別目錄功能表。


## <a id="_Toc260221669"></a>  加入一些版面配置和類別目錄功能表

在我們的站台主版頁面中，我們會將新增 div 左側資料行包含我們的產品類別目錄功能表。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

請注意，所要的對齊及其他格式會提供由我們新增至我們的 Style.css 檔案 CSS 類別。

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

[產品類別目錄] 功能表將以動態方式建立在執行階段查詢商務資料庫的現有產品類別和建立功能表項目和對應的連結。

若要完成此我們會使用 ASP 的兩個。網路的功能強大的資料控制項。 「 實體資料來源 」 控制項和 「 清單檢視 」 控制項。

![](tailspin-spyworks-part-3/_static/image1.jpg)

讓我們來切換至 [設計檢視]，並用於設定控制項，我們將協助程式。

![](tailspin-spyworks-part-3/_static/image2.jpg)

讓我們將 EntityDataSource ID 屬性設定為 EDS\_類別\_功能表，然後按一下上 「 設定資料來源 」。

![](tailspin-spyworks-part-3/_static/image3.jpg)

選取當我們建立實體資料來源模型商務資料庫時，我們所建立的 CommerceEntities 連接，然後按一下 下一步 」。

![](tailspin-spyworks-part-3/_static/image4.jpg)

選取 「 類別目錄 」 實體集名稱，然後讓其餘部分的選項，做為預設值。 按一下 [完成]。

現在讓我們設定我們置於我們 listview 的頁面的 ListView 控制項執行個體的 ID 屬性\_ProductsMenu 並啟用其協助程式。

![](tailspin-spyworks-part-3/_static/image5.jpg)

雖然我們可以使用控制選項，格式化資料的項目顯示和格式，我們功能表建立只需要簡單的標記，我們將輸入來源檢視中的程式碼。

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

請注意"Eval"陳述式： &lt;%# Eval("CategoryName") %&gt;

使用 ASP.NET 語法&lt;%# %&gt;會指示執行階段執行任何內含和輸出的結果"Line"的縮寫慣例。

陳述式 Eval("CategoryName") 指示，項目，此繫結集合中的目前項目擷取實體模型項目名稱的值"CatagoryName"。 這是非常強大的功能的精簡語法。

可讓應用程式現在需要執行。

![](tailspin-spyworks-part-3/_static/image6.jpg)

請注意，現在會顯示我們的產品類別目錄功能表以及當我們將滑鼠停留在一個類別目錄功能表項目，我們可以看到功能表項目連結指向的頁面，我們必須尚未實作名為 ProductsList.aspx，我們有建置動態查詢字串引數，包含 類別目錄識別碼。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-2.md)
> [下一頁](tailspin-spyworks-part-4.md)
