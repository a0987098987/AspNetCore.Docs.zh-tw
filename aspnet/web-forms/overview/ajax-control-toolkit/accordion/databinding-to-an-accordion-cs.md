---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: 資料繫結至 Accordion (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常宣告 w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 05adc7158725bd5a6ba276b81222de04158d3c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833536"
---
<a name="databinding-to-an-accordion-c"></a>資料繫結至 Accordion (C#)
====================
藉由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> 在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面中，但繫結至資料來源提供更大的彈性。


## <a name="overview"></a>總覽

在 AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面中，但繫結至資料來源提供更大的彈性。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。 如果您的設定不同，您必須調整資料庫的連接資訊。

若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

然後，新增至頁面的資料來源。 若要使用的資料數量有限，我們只選取前五個項目在 Vendor 資料表的 AdventureWorks 資料庫中。 如果您使用 Visual Studio 助理員來建立資料來源，請記住目前的版本中的錯誤不前置的資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

請記住的資料來源名稱 (ID)。 這個非常的識別必須再用於`DataSourceID`Accordion 控制項的屬性：

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Accordion 在控制項內，您可以提供範本的控制項，包括標頭的各個組件 (`<HeaderTemplate>`) 和內容 (`<ContentTemplate>`)。 這些項目，只輸出資料的資料來源，使用`DataBinder.Eval()`方法：

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

載入頁面時，資料來源必須繫結至 accordion 伺服器端程式碼：

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

若要結束此範例中，您需要 Accordion 控制項中定義兩個參考的 CSS 類別 (在其屬性`HeaderCssClass`和`ContentCssClass`)。 將下列標記放在`<head>`頁面的區段：

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Accordion 中的資料直接來自資料來源](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Accordion 中的資料直接來自資料來源 ([按一下以檢視完整大小的影像](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [下一步](dynamically-adding-an-accordion-pane-cs.md)
