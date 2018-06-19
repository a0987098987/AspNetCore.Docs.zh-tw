---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: 在 FormView (VB) 中使用 TextBoxWatermark |Microsoft 文件
author: wenz
description: AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下 [到] 方塊，它我...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 5789ccc4b2c6385f3476857dce139a8b47c5e5e8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872529"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>在 FormView (VB) 中使用 TextBoxWatermark
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> AJAX Control Toolkit TextBoxWatermark 控制項擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下此方塊時，它會清空。 如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。 這也是可能在 FormView 控制項內。


## <a name="overview"></a>總覽

`TextBoxWatermark` AJAX Control Toolkit 中的控制會擴充文字方塊中，使方塊內會顯示文字。 當使用者按一下此方塊時，它會清空。 如果使用者離開 [] 方塊中，而不需輸入文字，其中已預先的文字會重新出現。 這也可能在`FormView`控制項。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

然後，將資料來源加入至 [支援] 頁面`DELETE`，`INSERT`和`UPDATE`SQL 陳述式。 如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

請記住的名稱 (`ID`) 的資料來源，因為它會用於`DataSourceID`屬性`FormView`控制項。 `<InsertItemTemplate>`區段`FormView`包含文字方塊中，後者由擴充`TextBoxWatermarkExtender`控制項。 請確定 extender 位於範本內且不該期間之外。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

現在當使用者變更的插入模式為`FormView`控制項，在文字欄位感謝來使新的供應商`TextBoxWatermarkExtender`控制項。 在文字方塊中按一下可讓您填入文字會消失。


[![在欄位中的浮水印來自擴充項](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

在欄位中的浮水印來自擴充項 ([按一下以檢視完整大小的影像](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [上一頁](using-textboxwatermark-with-validation-controls-cs.md)
> [下一頁](using-textboxwatermark-with-validation-controls-vb.md)
