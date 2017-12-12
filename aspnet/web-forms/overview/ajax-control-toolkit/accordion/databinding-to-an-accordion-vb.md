---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: "資料繫結至 Accordion (VB) |Microsoft 文件"
author: wenz
description: "AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告 w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: aeff732e4daed6ed22fd5f3b6adcdeb6082aae53
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-vb"></a>資料繫結至 Accordion (VB)
====================
由[Christian Wenz](https://github.com/wenz)

[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面內，但繫結至資料來源提供更大的彈性。


## <a name="overview"></a>概觀

AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告本身，在頁面內，但繫結至資料來源提供更大的彈性。

## <a name="steps"></a>步驟

首先，資料來源是必要的。 這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。 資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064)。 AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。 若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。

此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。 如果您的設定不同，您必須調整資料庫的連接資訊。

為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

然後，將資料來源加入至頁面。 若要使用有限的資料，我們只選取前五個項目 Vendor 資料表的 AdventureWorks 資料庫中。 如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。 下列標記會顯示正確的語法：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

記住資料來源的名稱 (ID)。 這個非常識別必須用於`DataSourceID`accordion 設定控制項的屬性：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

Accordion 設定在控制項內，您可以提供範本的不同部分的控制項，包括標頭 (`<HeaderTemplate>`) 及內容 (`<ContentTemplate>`)。 在這些項目，只要輸出中資料的資料來源，使用`DataBinder.Eval()`方法：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

載入頁面時，資料來源必須繫結至這個伺服器端程式碼 accordion 設定：

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

若要完成此範例中，您需要定義 accordion 設定控制項中的兩個參考的 CSS 類別 (在其屬性中`HeaderCssClass`和`ContentCssClass`)。 將下列標記放在`<head>`頁面區段：

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Accordion 設定中的資料直接來自資料來源](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Accordion 設定中的資料直接來自資料來源 ([按一下以檢視完整大小的影像](databinding-to-an-accordion-vb/_static/image3.png))

>[!div class="step-by-step"]
[上一頁](dynamically-adding-an-accordion-pane-cs.md)
[下一頁](dynamically-adding-an-accordion-pane-vb.md)
