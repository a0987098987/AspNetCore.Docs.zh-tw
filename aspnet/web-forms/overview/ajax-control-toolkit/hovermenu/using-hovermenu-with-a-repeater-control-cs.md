---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: "使用 HoverMenu 與中繼器控制項 (C#) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在 specifi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aac5a26191cc633204549274c327e065578f4226
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>使用 HoverMenu 與中繼器控制項 (C#)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在指定的位置。 它也可使用這個控制項內的重複項。


## <a name="overview"></a>概觀

`HoverMenu` AJAX Control Toolkit 中的控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在指定的位置。 它也可使用這個控制項內的重複項。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

然後，將資料來源加入至頁面。 若要使用有限的資料，我們只選取前五個項目 Vendor 資料表的 AdventureWorks 資料庫中。 如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

接下來，加入工作面板會做為強制回應的快顯視窗：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

現在，`HoverMenuExtender`派上用場。 擴充項，以便在資料來源中的每個項目取得自己的快顯視窗，必須置於中繼器`<ItemTemplate>`> 一節。 標記如下：

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

現在每個資料來源中的項目會顯示快顯視窗右邊 (`PopupPosition`屬性) 50 毫秒的延遲之後 (`PopDelay`屬性)。


[![中繼器中每個項目的旁邊出現的停留功能表](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

中繼器中每個項目的旁邊出現的停留功能表 ([按一下以檢視完整大小的影像](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

>[!div class="step-by-step"]
[下一步](using-hovermenu-with-a-repeater-control-vb.md)
