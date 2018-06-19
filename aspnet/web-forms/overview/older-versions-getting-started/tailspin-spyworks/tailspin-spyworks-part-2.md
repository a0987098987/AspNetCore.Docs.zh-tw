---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 第 2 部分： 資料存取層 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 2 部分涵蓋新增資料存取層。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890485"
---
<a name="part-2-data-access-layer"></a>第 2 部分： 資料存取層
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 2 部分涵蓋新增資料存取層。


## <a id="_Toc260221668"></a>  新增資料存取層

我們的電子商務應用程式將取決於兩個資料庫。

客戶資訊，我們將使用標準 ASP.NET 成員資格資料庫。 我們的購物車和產品類別目錄中，我們會實作 SQL Express 資料庫執行，如下所示。

![](tailspin-spyworks-part-2/_static/image1.jpg)

在應用程式的應用程式中建立資料庫 (Commerce.mdf)\_我們可以繼續建立我們使用.NET Entity Framework 的資料存取層的 Data 資料夾。

我們將建立資料夾，名為 「 資料\_存取 」 和它們該資料夾上按一下滑鼠右鍵，然後選取 加入新項目 」。

在 「 已安裝的範本 」 項目，然後選取 「 ADO.NET 實體資料模型 」 輸入 EDM\_Commerce.edmx 做為名稱，然後按一下 [新增] 按鈕。

![](tailspin-spyworks-part-2/_static/image2.jpg)

選擇 [從資料庫產生]。

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

儲存並建置。

現在我們已經準備好新增我們的第一項功能 – 產品類別目錄功能表。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-1.md)
> [下一頁](tailspin-spyworks-part-3.md)
