---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: 使用 HoverMenu 與重複項控制項 (VB) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在 specifi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c0a59b20fa9649472a2cd9bf9368b17bb7b8619c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376290"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>使用 HoverMenu 與重複項控制項 (VB)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> 在 AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會出現在指定的位置。 它也可使用這個控制項內的重複項。


## <a name="overview"></a>總覽

`HoverMenu` AJAX Control Toolkit 中的控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會出現在指定的位置。 它也可使用這個控制項內的重複項。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。 如果您的設定不同，您必須調整資料庫的連接資訊。

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

然後，新增至頁面的資料來源。 若要使用的資料數量有限，我們只選取前五個項目在 Vendor 資料表的 AdventureWorks 資料庫中。 如果您使用 Visual Studio 助理員來建立資料來源，請記住目前的版本中的錯誤不前置的資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

接下來，新增一個面板作為強制回應快顯視窗：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

現在，`HoverMenuExtender`派上用場。 擴充項，讓資料來源中的每個項目取得自己的快顯視窗，必須將 repeater 內`<ItemTemplate>`一節。 標記如下：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

現在的右邊資料來源中的每個項目顯示快顯視窗 (`PopupPosition`屬性) 的 50 毫秒的延遲之後 (`PopDelay`屬性)。


[![中繼器中每個項目旁邊會出現的停留功能表](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

中繼器中每個項目旁邊會出現的停留功能表 ([按一下以檢視完整大小的影像](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一步](using-hovermenu-with-a-repeater-control-cs.md)
