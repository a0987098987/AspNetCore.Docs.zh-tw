---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: 在中繼器 (VB) 中使用 ConfirmButton |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。 只有當是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>在中繼器 (VB) 中使用 ConfirmButton
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。 只当按一下 [是]，按鈕會執行動作，否則為已取消。 這也是在中繼器中。


## <a name="overview"></a>總覽

AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。 只当按一下 [是]，按鈕會執行動作，否則為已取消。 這也是在中繼器中。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

然後，資料來源是必要的。 為了簡單起見，已擷取只有前五個項目 AdventureWorks 的供應商資料表中。 請注意，當使用 Visual Studio 精靈來建立資料來源、 資料表名稱 (`Vendors`) 目前不正確加上`Purchasing`。 下列標記會是正確的其中一個：

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

此資料來源可用中繼器中。 像往常一樣，`DataBinder.Eval()`方法會從資料來源擷取資料。 `ConfirmButtonExtender`控制項必須再放置內`<ItemTemplate>`中繼器使它顯示資料來源中的每個項目的一節。

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![[確認] 按鈕旁邊的資料來源的每個項目](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

資料來源的每個項目旁邊的 [確認] 按鈕會出現 ([按一下以檢視完整大小的影像](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](using-a-confirmbutton-in-a-repeater-cs.md)
