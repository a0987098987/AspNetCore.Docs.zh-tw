---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: 使用 ModalPopup 與重複項控制項 (C#) |Microsoft Docs
author: wenz
description: AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。 此外，也可以使用此 contr....
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 72d0f16d22911d867f9a91faf273e236453e7b3a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833620"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="d6de9-104">使用 ModalPopup 與重複項控制項 (C#)</span><span class="sxs-lookup"><span data-stu-id="d6de9-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="d6de9-105">藉由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d6de9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d6de9-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6de9-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="d6de9-107">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="d6de9-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d6de9-108">它也可使用這個控制項內的重複項。</span><span class="sxs-lookup"><span data-stu-id="d6de9-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="d6de9-109">總覽</span><span class="sxs-lookup"><span data-stu-id="d6de9-109">Overview</span></span>

<span data-ttu-id="d6de9-110">AJAX Control Toolkit 之 ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應快顯。</span><span class="sxs-lookup"><span data-stu-id="d6de9-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d6de9-111">它也可使用這個控制項內的重複項。</span><span class="sxs-lookup"><span data-stu-id="d6de9-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="d6de9-112">步驟</span><span class="sxs-lookup"><span data-stu-id="d6de9-112">Steps</span></span>

<span data-ttu-id="d6de9-113">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="d6de9-113">First of all, a data source is required.</span></span> <span data-ttu-id="d6de9-114">此範例使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="d6de9-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="d6de9-115">資料庫 （包括 express edition） 的 Visual Studio 安裝的選擇性部分作業，因此也會提供個別下載底下[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="d6de9-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="d6de9-116">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (下載網址[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="d6de9-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="d6de9-117">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en))，並將附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="d6de9-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="d6de9-118">此範例中，我們假設 SQL Server 2005 Express Edition 的執行個體，會呼叫`SQLEXPRESS`位於與網頁伺服器; 相同的電腦上，這也是預設設定。</span><span class="sxs-lookup"><span data-stu-id="d6de9-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="d6de9-119">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="d6de9-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="d6de9-120">若要啟動的 ASP.NET AJAX Control Toolkit 中，功能`ScriptManager`控制項必須放置在任何位置上 (但在`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="d6de9-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="d6de9-121">然後，新增至頁面的資料來源。</span><span class="sxs-lookup"><span data-stu-id="d6de9-121">Then, add a data source to the page.</span></span> <span data-ttu-id="d6de9-122">若要使用的資料數量有限，我們只選取前五個項目在 Vendor 資料表的 AdventureWorks 資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d6de9-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="d6de9-123">如果您使用 Visual Studio 助理員來建立資料來源，請記住目前的版本中的錯誤不前置的資料表名稱 (`Vendor`) 與`Purchasing`。</span><span class="sxs-lookup"><span data-stu-id="d6de9-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="d6de9-124">下列標記會顯示正確的語法：</span><span class="sxs-lookup"><span data-stu-id="d6de9-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="d6de9-125">接下來，加入做為強制回應快顯面板。</span><span class="sxs-lookup"><span data-stu-id="d6de9-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="d6de9-126">它包含`Button`再次關閉快顯視窗的控制：</span><span class="sxs-lookup"><span data-stu-id="d6de9-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="d6de9-127">為了讓快顯功能表內重複項，運作`ModalPopupExtender`控制項必須放在`<ItemTemplate>`中繼器的區段。</span><span class="sxs-lookup"><span data-stu-id="d6de9-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="d6de9-128">讓面板超出重複項，但擴充項內。</span><span class="sxs-lookup"><span data-stu-id="d6de9-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="d6de9-129">以下是重複項的標記：</span><span class="sxs-lookup"><span data-stu-id="d6de9-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="d6de9-130">然後，它旁邊的按鈕觸發強制回應快顯視窗會顯示資料來源中的每個項目。</span><span class="sxs-lookup"><span data-stu-id="d6de9-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="d6de9-131">[![可針對每個資料來源項目觸發強制回應快顯視窗](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6de9-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="d6de9-132">可針對每個資料來源項目觸發強制回應快顯 ([按一下以檢視完整大小的影像](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d6de9-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d6de9-133">[上一頁](launching-a-modal-popup-window-from-server-code-cs.md)
> [下一頁](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d6de9-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
