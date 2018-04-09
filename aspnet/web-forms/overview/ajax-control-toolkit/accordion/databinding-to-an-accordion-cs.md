---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: 資料繫結至 Accordion (C#) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。 面板通常會宣告 w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a3ec242c4d5312026ddbc8282ef1b4c3142915a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="d92d0-104">資料繫結至 Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="d92d0-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="d92d0-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d92d0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d92d0-106">[下載程式碼](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d92d0-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="d92d0-107">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="d92d0-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d92d0-108">面板通常會宣告本身，在頁面內，但繫結至資料來源提供更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="d92d0-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="d92d0-109">總覽</span><span class="sxs-lookup"><span data-stu-id="d92d0-109">Overview</span></span>

<span data-ttu-id="d92d0-110">AJAX Control Toolkit Accordion 控制項提供多個窗格，並可讓使用者一次顯示其中一個。</span><span class="sxs-lookup"><span data-stu-id="d92d0-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d92d0-111">面板通常會宣告本身，在頁面內，但繫結至資料來源提供更大的彈性。</span><span class="sxs-lookup"><span data-stu-id="d92d0-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="d92d0-112">步驟</span><span class="sxs-lookup"><span data-stu-id="d92d0-112">Steps</span></span>

<span data-ttu-id="d92d0-113">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="d92d0-113">First of all, a data source is required.</span></span> <span data-ttu-id="d92d0-114">這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="d92d0-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="d92d0-115">資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="d92d0-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="d92d0-116">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="d92d0-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="d92d0-117">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="d92d0-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="d92d0-118">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="d92d0-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="d92d0-119">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="d92d0-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="d92d0-120">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="d92d0-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="d92d0-121">然後，將資料來源加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="d92d0-121">Then, add a data source to the page.</span></span> <span data-ttu-id="d92d0-122">若要使用有限的資料，我們只選取前五個項目 Vendor 資料表的 AdventureWorks 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d92d0-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="d92d0-123">如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。</span><span class="sxs-lookup"><span data-stu-id="d92d0-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="d92d0-124">下列標記會顯示正確的語法：</span><span class="sxs-lookup"><span data-stu-id="d92d0-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="d92d0-125">記住資料來源的名稱 (ID)。</span><span class="sxs-lookup"><span data-stu-id="d92d0-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="d92d0-126">這個非常識別必須用於`DataSourceID`accordion 設定控制項的屬性：</span><span class="sxs-lookup"><span data-stu-id="d92d0-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="d92d0-127">Accordion 設定在控制項內，您可以提供範本的不同部分的控制項，包括標頭 (`<HeaderTemplate>`) 及內容 (`<ContentTemplate>`)。</span><span class="sxs-lookup"><span data-stu-id="d92d0-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="d92d0-128">在這些項目，只要輸出中資料的資料來源，使用`DataBinder.Eval()`方法：</span><span class="sxs-lookup"><span data-stu-id="d92d0-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="d92d0-129">載入頁面時，資料來源必須繫結至這個伺服器端程式碼 accordion 設定：</span><span class="sxs-lookup"><span data-stu-id="d92d0-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="d92d0-130">若要完成此範例中，您需要定義 accordion 設定控制項中的兩個參考的 CSS 類別 (在其屬性中`HeaderCssClass`和`ContentCssClass`)。</span><span class="sxs-lookup"><span data-stu-id="d92d0-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="d92d0-131">將下列標記放在`<head>`頁面區段：</span><span class="sxs-lookup"><span data-stu-id="d92d0-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="d92d0-132">[![Accordion 設定中的資料直接來自資料來源](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d92d0-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="d92d0-133">Accordion 設定中的資料直接來自資料來源 ([按一下以檢視完整大小的影像](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d92d0-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d92d0-134">下一步</span><span class="sxs-lookup"><span data-stu-id="d92d0-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)
