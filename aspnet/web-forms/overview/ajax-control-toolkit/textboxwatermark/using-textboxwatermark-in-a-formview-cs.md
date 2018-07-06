---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
title: 在 FormView 中 (C#) 中使用 TextBoxWatermark |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。 當使用者在方塊中，按一下它我...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e6ee90bf-32a5-4987-a384-15cc7dd30c8a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e646b685680071501d45f7454920def2b15a7db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817034"
---
<a name="using-textboxwatermark-in-a-formview-c"></a>在 FormView 中 (C#) 中使用 TextBoxWatermark
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1CS.pdf)

> 在 AJAX Control Toolkit TextBoxWatermark 控制延伸的文字方塊，讓文字會顯示在方塊內。 當使用者按一下方塊時，它會清空。 如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。 這也可在 FormView 控制項內。


## <a name="overview"></a>總覽

`TextBoxWatermark`中 AJAX Control Toolkit 控制項擴充文字方塊中，使方塊內顯示的文字。 當使用者按一下方塊時，它會清空。 如果使用者離開不需要輸入文字的方塊中，預先填入的文字隨即再度出現。 這也可在`FormView`控制項。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。 如果您的設定不同，您必須調整資料庫的連接資訊。

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample1.aspx)]

然後，新增至 支援 頁面的 資料來源`DELETE`，`INSERT`和`UPDATE`SQL 陳述式。 如果您使用 Visual Studio 助理員來建立資料來源，請記住目前的版本中的錯誤不前置的資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample2.aspx)]

請記住名稱 (`ID`) 的資料來源，因為它將用於`DataSourceID`屬性`FormView`控制項。 `<InsertItemTemplate>`一節`FormView`包含擴充的 textbox`TextBoxWatermarkExtender`控制項。 請確定擴充項位於範本內和不之外。

[!code-aspx[Main](using-textboxwatermark-in-a-formview-cs/samples/sample3.aspx)]

現在當使用者變更的插入模式`FormView`控制項，在文字欄位的新廠商已預先填入要感謝`TextBoxWatermarkExtender`控制項。 在文字方塊按一下可讓消失了填入文字。


[![在欄位中的浮水印來自擴充項](using-textboxwatermark-in-a-formview-cs/_static/image2.png)](using-textboxwatermark-in-a-formview-cs/_static/image1.png)

在欄位中的浮水印來自擴充項 ([按一下以檢視完整大小的影像](using-textboxwatermark-in-a-formview-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](using-textboxwatermark-with-validation-controls-cs.md)
