---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: 使用 ConfirmButton 中 (C#) |Microsoft Docs
author: wenz
description: 在 AJAX Control Toolkit ConfirmButton 擴充項會建立 是/當使用者按一下按鈕上的任何快顯 （包含 LinkButton 控制項）。 只有當是...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 614b5b1edaa164cca30b2142d1e0c02771153403
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830154"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="41a86-104">使用 ConfirmButton 中 (C#)</span><span class="sxs-lookup"><span data-stu-id="41a86-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="41a86-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="41a86-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="41a86-106">[下載程式碼](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="41a86-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="41a86-107">在 AJAX Control Toolkit ConfirmButton 擴充項會建立 是/當使用者按一下按鈕上的任何快顯 （包含 LinkButton 控制項）。</span><span class="sxs-lookup"><span data-stu-id="41a86-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="41a86-108">只是按一下時，如果按鈕的執行動作，否則已取消。</span><span class="sxs-lookup"><span data-stu-id="41a86-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="41a86-109">這也是可能的重複項中。</span><span class="sxs-lookup"><span data-stu-id="41a86-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="41a86-110">總覽</span><span class="sxs-lookup"><span data-stu-id="41a86-110">Overview</span></span>

<span data-ttu-id="41a86-111">在 AJAX Control Toolkit ConfirmButton 擴充項會建立 是/當使用者按一下按鈕上的任何快顯 （包含 LinkButton 控制項）。</span><span class="sxs-lookup"><span data-stu-id="41a86-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="41a86-112">只是按一下時，如果按鈕的執行動作，否則已取消。</span><span class="sxs-lookup"><span data-stu-id="41a86-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="41a86-113">這也是可能的重複項中。</span><span class="sxs-lookup"><span data-stu-id="41a86-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="41a86-114">步驟</span><span class="sxs-lookup"><span data-stu-id="41a86-114">Steps</span></span>

<span data-ttu-id="41a86-115">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="41a86-115">First of all, a data source is required.</span></span> <span data-ttu-id="41a86-116">此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="41a86-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="41a86-117">資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="41a86-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="41a86-118">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="41a86-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="41a86-119">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="41a86-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="41a86-120">此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。</span><span class="sxs-lookup"><span data-stu-id="41a86-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="41a86-121">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="41a86-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="41a86-122">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="41a86-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="41a86-123">然後，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="41a86-123">Then, a data source is required.</span></span> <span data-ttu-id="41a86-124">為了簡單起見，會擷取僅 AdventureWorks 的供應商資料表的前五個項目。</span><span class="sxs-lookup"><span data-stu-id="41a86-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="41a86-125">請注意，當使用 Visual Studio 精靈來建立資料來源時，資料表名稱 (`Vendors`) 目前未正確前面會加上`Purchasing`。</span><span class="sxs-lookup"><span data-stu-id="41a86-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="41a86-126">下列標記正確：</span><span class="sxs-lookup"><span data-stu-id="41a86-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="41a86-127">此資料來源可用內設有重複項。</span><span class="sxs-lookup"><span data-stu-id="41a86-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="41a86-128">像往常一樣，`DataBinder.Eval()`方法會從資料來源擷取資料。</span><span class="sxs-lookup"><span data-stu-id="41a86-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="41a86-129">`ConfirmButtonExtender`控制項則必須放在`<ItemTemplate>`區段的重複項，使它顯示的資料來源中的每個項目。</span><span class="sxs-lookup"><span data-stu-id="41a86-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="41a86-130">[![從資料來源的每個項目旁邊的 [確認] 按鈕會出現](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="41a86-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="41a86-131">從資料來源的每個項目旁邊的 [確認] 按鈕會出現 ([按一下以檢視完整大小的影像](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="41a86-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="41a86-132">下一步</span><span class="sxs-lookup"><span data-stu-id="41a86-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
