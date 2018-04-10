---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: 使用 AJAX 控制項 Toolkit 控制項和控制項擴充項 (VB) |Microsoft 文件
author: microsoft
description: 了解如何將 AJAX Control Toolkit 控制項和擴充項加入 ASP.NET 網頁。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 080dd65677d80fb75ab37a20f6c385a38af4e353
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="97400-103">使用 AJAX 控制項 Toolkit 控制項和控制項擴充項 (VB)</span><span class="sxs-lookup"><span data-stu-id="97400-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="97400-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="97400-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="97400-105">了解如何將 AJAX Control Toolkit 控制項和擴充項加入 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="97400-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="97400-106">AJAX Control Toolkit 包含一組控制項和控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="97400-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="97400-107">在這個簡短的教學課程中，您會學習如何加入 ASP.NET 網頁上的控制項和控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="97400-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="97400-108">如需安裝 AJAX Control Toolkit，然後在 [Visual Studio/Visual Web Developer 工具箱] 中，將 AJAX Control Toolkit 的指示，請參閱本教學課程[開始使用 AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md)。</span><span class="sxs-lookup"><span data-stu-id="97400-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="97400-109">使用 AJAX 控制項 Toolkit 控制項</span><span class="sxs-lookup"><span data-stu-id="97400-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="97400-110">AJAX Control Toolkit 控制的運作方式類似標準的 ASP.NET 控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="97400-111">您可以將控制項從 [工具箱] 拖曳到 ASP.NET 頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="97400-112">您可以將控制項加入至 設計檢視 或 原始碼檢視中的頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="97400-113">使用 AJAX Control Toolkit 從控制項時，沒有一項特殊需求。</span><span class="sxs-lookup"><span data-stu-id="97400-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="97400-114">頁面必須包含 ScriptManager 控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="97400-115">ScriptManager 控制項負責包括所有必要的 AJAX Control Toolkit 控制項所需的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="97400-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="97400-116">例如，[AJAX Control Toolkit] 索引標籤包含控制項，名為編輯器控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="97400-117">此控制項顯示豐富的 HTML 編輯器。</span><span class="sxs-lookup"><span data-stu-id="97400-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="97400-118">請遵循下列步驟來加入網頁中的編輯器控制項：</span><span class="sxs-lookup"><span data-stu-id="97400-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="97400-119">建立新的 ASP.NET 網頁，名為 ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="97400-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="97400-120">在 [工具箱] 中選取 [AJAX 擴充功能] 索引標籤 from beneath ScriptManager 控制項，並將控制項拖曳到頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="97400-121">在 工具箱 選取 from beneath AJAX Control Toolkit 索引標籤的編輯器控制項，然後將控制項拖曳到頁面 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="97400-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="97400-122">在設計工具應該如下圖 2。</span><span class="sxs-lookup"><span data-stu-id="97400-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="97400-123">選取功能表選項來執行網站**偵錯，開始偵錯**或按下 F5 鍵。</span><span class="sxs-lookup"><span data-stu-id="97400-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="97400-124">您應該會看到圖 3 中的頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="97400-125">[![HTML 編輯器控制項選取](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="97400-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="97400-126">**圖 01**: HTML 編輯器控制項選取 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="97400-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="97400-127">[![Visual Studio 設計工具與 ScriptManager 和編輯控制項](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="97400-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="97400-128">**圖 02**: ScriptManager 和編輯控制項的 Visual Studio 設計工具 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="97400-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="97400-129">[![DisplayEditor.aspx 頁面](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="97400-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="97400-130">**圖 03**: DisplayEditor.aspx 頁面 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="97400-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="97400-131">使用 AJAX 控制項 Toolkit 控制項擴充項</span><span class="sxs-lookup"><span data-stu-id="97400-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="97400-132">AJAX Control Toolkit 也包含控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="97400-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="97400-133">正如其名，控制項擴充項會擴充現有控制項的功能。</span><span class="sxs-lookup"><span data-stu-id="97400-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="97400-134">例如，ConfirmButton 控制項擴充項會擴充標準的 ASP.NET 按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="97400-135">擴充項變更按鈕控制項的行為，所以按鈕會顯示確認對話方塊中，當您按一下它。</span><span class="sxs-lookup"><span data-stu-id="97400-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="97400-136">控制項擴充項，就像 AJAX Control Toolkit 控制項需要 ScriptManager 控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="97400-137">開始使用網頁中的控制項擴充項之前，您必須將 ScriptManager 控制項加入頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="97400-138">請遵循下列步驟使用 ConfirmButton 控制項擴充項：</span><span class="sxs-lookup"><span data-stu-id="97400-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="97400-139">建立新的 ASP.NET 網頁，名為 ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="97400-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="97400-140">ScriptManager 將控制項加入至頁面控制項拖曳到 from beneath [AJAX 擴充功能] 索引標籤頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="97400-141">在工具箱 拖曳至設計工具介面拖曳 from beneath 標準索引標籤按鈕加入頁面標準的按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="97400-142">按一下**加入擴充項**工作選項 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="97400-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="97400-143">在 選擇的擴充性 對話方塊中，選取 ConfirmButtonExtender （請參閱圖 5），然後按一下 確定 按鈕。</span><span class="sxs-lookup"><span data-stu-id="97400-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="97400-144">在設計工具選取按鈕控制項，然後展開 Extender Button1\_ConfirmButtonExtender 節點，在 [屬性] 視窗中的 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="97400-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="97400-145">將值指派*真的？* ConfirmText 屬性。</span><span class="sxs-lookup"><span data-stu-id="97400-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="97400-146">選取功能表選項執行頁面**偵錯，開始偵錯**或按 F5 鍵。</span><span class="sxs-lookup"><span data-stu-id="97400-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="97400-147">[![加入擴充項工作選項](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="97400-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="97400-148">**圖 04**: 加入擴充項工作的選項 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="97400-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="97400-149">[![選取 ConfirmButton 控制項擴充項](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="97400-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="97400-150">**圖 05**： 選取 ConfirmButton 控制項擴充項 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="97400-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="97400-151">[![設定 ConfirmButton 屬性](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="97400-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="97400-152">**圖 06**: ConfirmButton 屬性設定 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="97400-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="97400-153">當頁面開啟時，您應該會看到一個按鈕。</span><span class="sxs-lookup"><span data-stu-id="97400-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="97400-154">當您按一下按鈕時，您會取得濆迶 7 的確認對話方塊。</span><span class="sxs-lookup"><span data-stu-id="97400-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="97400-155">[![顯示 [確認] 對話方塊](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="97400-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="97400-156">**圖 07**： 顯示 [確認] 對話方塊 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="97400-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="97400-157">請注意，您通常請勿拖曳控制項擴充項拖曳到頁面。</span><span class="sxs-lookup"><span data-stu-id="97400-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="97400-158">相反地，您使用**加入擴充項**工作選項，將擴充項加入至已加入至網頁的控制項。</span><span class="sxs-lookup"><span data-stu-id="97400-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="97400-159">此外，請注意，您將控制項擴充項屬性開啟要擴充控制項的屬性工作表。</span><span class="sxs-lookup"><span data-stu-id="97400-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="97400-160">在單一 ASP.NET 控制項可以由多個控制項擴充項擴充。</span><span class="sxs-lookup"><span data-stu-id="97400-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="97400-161">擴充控制項的屬性工作表會列出所有與控制項關聯的控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="97400-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97400-162">[上一頁](get-started-with-the-ajax-control-toolkit-vb.md)
> [下一頁](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="97400-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
