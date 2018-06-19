---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: 使用 HoverMenu 與中繼器控制項 (VB) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在 specifi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868395"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="c0dd9-103">使用 HoverMenu 與中繼器控制項 (VB)</span><span class="sxs-lookup"><span data-stu-id="c0dd9-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="c0dd9-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c0dd9-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c0dd9-105">[下載程式碼](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c0dd9-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="c0dd9-106">AJAX Control Toolkit HoverMenu 控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在指定的位置。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="c0dd9-107">它也可使用這個控制項內的重複項。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="c0dd9-108">總覽</span><span class="sxs-lookup"><span data-stu-id="c0dd9-108">Overview</span></span>

<span data-ttu-id="c0dd9-109">`HoverMenu` AJAX Control Toolkit 中的控制項提供簡單的快顯效果： 當滑鼠指標停留在項目上方時，快顯視窗會顯示在指定的位置。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="c0dd9-110">它也可使用這個控制項內的重複項。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="c0dd9-111">步驟</span><span class="sxs-lookup"><span data-stu-id="c0dd9-111">Steps</span></span>

<span data-ttu-id="c0dd9-112">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-112">First of all, a data source is required.</span></span> <span data-ttu-id="c0dd9-113">這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="c0dd9-114">資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="c0dd9-115">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="c0dd9-116">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="c0dd9-117">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="c0dd9-118">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="c0dd9-119">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="c0dd9-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="c0dd9-120">然後，將資料來源加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-120">Then, add a data source to the page.</span></span> <span data-ttu-id="c0dd9-121">若要使用有限的資料，我們只選取前五個項目 Vendor 資料表的 AdventureWorks 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="c0dd9-122">如果您使用 Visual Studio 小幫手來建立資料來源，請記住，請在目前版本中的 bug 不前置資料表名稱 (`Vendor`) 與`Purchasing`。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="c0dd9-123">下列標記會顯示正確的語法：</span><span class="sxs-lookup"><span data-stu-id="c0dd9-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="c0dd9-124">接下來，加入工作面板會做為強制回應的快顯視窗：</span><span class="sxs-lookup"><span data-stu-id="c0dd9-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="c0dd9-125">現在，`HoverMenuExtender`派上用場。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="c0dd9-126">擴充項，以便在資料來源中的每個項目取得自己的快顯視窗，必須置於中繼器`<ItemTemplate>`> 一節。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="c0dd9-127">標記如下：</span><span class="sxs-lookup"><span data-stu-id="c0dd9-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="c0dd9-128">現在每個資料來源中的項目會顯示快顯視窗右邊 (`PopupPosition`屬性) 50 毫秒的延遲之後 (`PopDelay`屬性)。</span><span class="sxs-lookup"><span data-stu-id="c0dd9-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="c0dd9-129">[![中繼器中每個項目的旁邊出現的停留功能表](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c0dd9-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="c0dd9-130">中繼器中每個項目的旁邊出現的停留功能表 ([按一下以檢視完整大小的影像](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c0dd9-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c0dd9-131">上一步</span><span class="sxs-lookup"><span data-stu-id="c0dd9-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
