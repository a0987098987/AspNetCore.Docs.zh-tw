---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: 如何使用下拉式方塊控制項？ (VB) | Microsoft Docs
author: microsoft
description: 下拉式方塊會是結合使用者可以從中選擇的選項清單中的文字方塊中的彈性以 ASP.NET AJAX 控制項。
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ec00a58581f36f87ecdca2fbd96fcea645f75f48
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831218"
---
<a name="how-do-i-use-the-combobox-control-vb"></a><span data-ttu-id="440a7-104">如何使用下拉式方塊控制項？</span><span class="sxs-lookup"><span data-stu-id="440a7-104">How do I use the ComboBox Control?</span></span> <span data-ttu-id="440a7-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="440a7-105">(VB)</span></span>
====================
<span data-ttu-id="440a7-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="440a7-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="440a7-107">下拉式方塊會是結合使用者可以從中選擇的選項清單中的文字方塊中的彈性以 ASP.NET AJAX 控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-107">ComboBox is an ASP.NET AJAX control that combines the flexibility of a TextBox with a list of options from which users can choose.</span></span>


<span data-ttu-id="440a7-108">本教學課程的目標在於說明 AJAX 控制項工具組下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-108">The goal of this tutorial is to explain the AJAX Control Toolkit ComboBox control.</span></span> <span data-ttu-id="440a7-109">下拉式方塊的運作方式類似標準的 ASP.NET DropDownList 控制項和文字方塊控制項的結合。</span><span class="sxs-lookup"><span data-stu-id="440a7-109">The ComboBox works like a combination between a standard ASP.NET DropDownList control and a TextBox control.</span></span> <span data-ttu-id="440a7-110">您可以從預先存在的項目清單中選取，或輸入新的項目。</span><span class="sxs-lookup"><span data-stu-id="440a7-110">You can either select from a pre-existing list of items or enter a new item.</span></span>

<span data-ttu-id="440a7-111">下拉式方塊類似於 AutoComplete 控制項擴充項，但不同的案例中使用的控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-111">The ComboBox is similar to the AutoComplete control extender, but the controls are used in different scenarios.</span></span> <span data-ttu-id="440a7-112">AutoComplete 擴充項會查詢 web 服務，以取得相符的項目。</span><span class="sxs-lookup"><span data-stu-id="440a7-112">The AutoComplete extender queries a web service to get matching entries.</span></span> <span data-ttu-id="440a7-113">下拉式方塊控制項，相較之下，會使用一組項目初始化。</span><span class="sxs-lookup"><span data-stu-id="440a7-113">The ComboBox control, in contrast, is initialized with a set of items.</span></span> <span data-ttu-id="440a7-114">使用自動完成擴充項會適合您正在使用大量 （數百萬計的汽車組件） 的資料時使用下拉式方塊控制項有意義時使用較少的資料部分 （幾十個汽車部分）。</span><span class="sxs-lookup"><span data-stu-id="440a7-114">Using the AutoComplete extender makes sense when you are working with a large set of data (millions of car parts) while using the ComboBox control makes sense when working with a small set of data (dozens of car parts).</span></span>

## <a name="selecting-from-a-static-list-of-items"></a><span data-ttu-id="440a7-115">從靜態項目清單中選取</span><span class="sxs-lookup"><span data-stu-id="440a7-115">Selecting from a Static List of Items</span></span>

<span data-ttu-id="440a7-116">可讓 s 使用下拉式方塊控制項的簡單範例開始。</span><span class="sxs-lookup"><span data-stu-id="440a7-116">Let�s start with a simple sample of using the ComboBox control.</span></span> <span data-ttu-id="440a7-117">假設您想要顯示在下拉式清單中的靜態項目清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-117">Imagine that you want to display a static list of items in a dropdown list.</span></span> <span data-ttu-id="440a7-118">不過，您會想要保持開啟的清單不完整的可能性。</span><span class="sxs-lookup"><span data-stu-id="440a7-118">However, you want to leave open the possibility that the list is not complete.</span></span> <span data-ttu-id="440a7-119">您想要允許使用者清單中輸入自訂值。</span><span class="sxs-lookup"><span data-stu-id="440a7-119">You want to allow a user to enter a custom value into the list.</span></span>

<span data-ttu-id="440a7-120">我們將建立新的 ASP.NET Web Form 頁面和頁面中，使用下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-120">We�ll create a new ASP.NET Web Forms page and use the ComboBox control in the page.</span></span> <span data-ttu-id="440a7-121">專案中加入新的 ASP.NET 頁面，並切換至 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="440a7-121">Add the new ASP.NET page to your project and switch to Design view.</span></span>

<span data-ttu-id="440a7-122">如果您想要使用 ComboBox 控制項頁面中您必須將 ScriptManager 控制項加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="440a7-122">If you want to use the ComboBox control in the page then you must add a ScriptManager control to the page.</span></span> <span data-ttu-id="440a7-123">將 ScriptManager 控制項來 [AJAX 延伸模組] 索引標籤拖曳至設計工具介面。</span><span class="sxs-lookup"><span data-stu-id="440a7-123">Drag the ScriptManager control from beneath the AJAX Extensions tab onto the Designer surface.</span></span> <span data-ttu-id="440a7-124">您應該在頁面的頂端加入 ScriptManager 控制項您可以將它立即下方開啟伺服器端&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="440a7-124">You should add the ScriptManager control at the top of the page; you can add it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="440a7-125">接下來，拖曳到頁面的下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-125">Next, drag the ComboBox control onto the page.</span></span> <span data-ttu-id="440a7-126">您可以在 [工具箱] 中與其他 AJAX Control Toolkit 控制項及控制項擴充項 （請參閱 [圖 1]） 中找到下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-126">You can find the ComboBox control in the Toolbox with the other AJAX Control Toolkit controls and control extenders (see figure1).</span></span>


<span data-ttu-id="440a7-127">[![簡單的表單建立名片](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-127">[![Simple form for creating a business card](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="440a7-128">**圖 01**： 從 [工具箱] 中選取下拉式方塊控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-128">**Figure 01**: Selecting the ComboBox control from the toolbox ([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="440a7-129">我們將使用下拉式方塊控制項來顯示靜態選項清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-129">We�ll use the ComboBox control to display a static list of choices.</span></span> <span data-ttu-id="440a7-130">使用者可以從三種選項的清單中選取食物的 spiciness 的特定層級： 輕微、 中型和經常性存取 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="440a7-130">The user can select a particular level of spiciness for their food from a list of three choices: Mild, Medium, and Hot (see Figure 2).</span></span>


<span data-ttu-id="440a7-131">[![從靜態項目清單中選取](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-131">[![Selecting from a static list of items](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="440a7-132">**圖 02**： 選取靜態清單的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-132">**Figure 02**: Selecting from a static list of items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="440a7-133">有兩種方式，您可以加入這些選項的下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-133">There are two ways that you can add these choices to the ComboBox control.</span></span> <span data-ttu-id="440a7-134">首先，您將滑鼠停留在設計檢視中的控制項時，請選取 [編輯選項] 工作選項，並開啟項目編輯器 （請參閱 [圖 3]）。</span><span class="sxs-lookup"><span data-stu-id="440a7-134">First, you select the Edit Options task option when hovering your mouse over the control in Design view and open the Item Editor (see Figure 3).</span></span>


<span data-ttu-id="440a7-135">[![編輯下拉式方塊項目](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-135">[![Editing ComboBox items](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="440a7-136">**圖 03**： 編輯下拉式方塊項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-136">**Figure 03**: Editing ComboBox items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="440a7-137">第二個選項是新增的開頭和結尾之間的項目清單&lt;asp: ComboBox&gt;來源檢視中的標籤。</span><span class="sxs-lookup"><span data-stu-id="440a7-137">The second option is to add the list of items in between the opening and closing &lt;asp:ComboBox&gt; tags in Source view.</span></span> <span data-ttu-id="440a7-138">[列表 1] 頁面包含 [更新] 下拉式方塊具有項目清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-138">The page in Listing 1 contains the updated ComboBox that has the list of items.</span></span>

<span data-ttu-id="440a7-139">**列表 1-Static.aspx**</span><span class="sxs-lookup"><span data-stu-id="440a7-139">**Listing 1 - Static.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

<span data-ttu-id="440a7-140">當您在 列表 1 中開啟頁面時，您可以從下拉式方塊中選取其中一個預先存在的選項。</span><span class="sxs-lookup"><span data-stu-id="440a7-140">When you open the page in Listing 1, you can select one of the pre-existing options from the ComboBox.</span></span> <span data-ttu-id="440a7-141">換句話說，下拉式方塊的運作方式一樣 DropDownList 控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-141">In other words, the ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="440a7-142">不過，您也可以選擇輸入新選擇 （例如，超級辣味） 不在現有的清單中。</span><span class="sxs-lookup"><span data-stu-id="440a7-142">However, you also have the option of entering a new choice (for example, Super Spicy) that is not in the existing list.</span></span> <span data-ttu-id="440a7-143">因此，下拉式方塊也的運作方式類似文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-143">So, the ComboBox also works like a TextBox control.</span></span>

<span data-ttu-id="440a7-144">不論您是否選取既存的項目，或輸入自訂的項目中，當您提交表單時，您的選擇會顯示在標籤控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-144">Regardless of whether you pick a pre-existing item or you enter a custom item, when you submit the form, your choice appears in the label control.</span></span> <span data-ttu-id="440a7-145">當您提交表單，btnSubmit\_Click 處理常式會執行，並更新標籤 （請參閱 圖 4）。</span><span class="sxs-lookup"><span data-stu-id="440a7-145">When you submit the form, the btnSubmit\_Click handler executes and updates the label (see Figure 4).</span></span>


<span data-ttu-id="440a7-146">[![顯示選取的項目](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-146">[![Displaying the selected item](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="440a7-147">**圖 04**： 顯示選取的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-147">**Figure 04**: Displaying the selected item([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="440a7-148">下拉式方塊來擷取選取的項目表單送出之後，支援 DropDownList 控制項相同的屬性：</span><span class="sxs-lookup"><span data-stu-id="440a7-148">The ComboBox supports the same properties as the DropDownList control for retrieving the selected item after a form is submitted:</span></span>

- <span data-ttu-id="440a7-149">SelectedItem.Text-顯示選取項目的 Text 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="440a7-149">SelectedItem.Text - Displays the value of the Text property of the selected item.</span></span>
- <span data-ttu-id="440a7-150">SelectedItem.Value-顯示選取的項目的 Value 屬性的值，或顯示下拉式方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="440a7-150">SelectedItem.Value - Displays the value of the Value property of the selected item or displays the text typed into the ComboBox.</span></span>
- <span data-ttu-id="440a7-151">SelectedValue-相同 SelectedItem.Value 不同之處在於此屬性可讓您指定預設值 （初始） 選取的項目。</span><span class="sxs-lookup"><span data-stu-id="440a7-151">SelectedValue - Same as SelectedItem.Value except that this property enables you to specify the default (initial) selected item.</span></span>

<span data-ttu-id="440a7-152">如果您輸入到下拉式方塊然後自訂的選擇是自訂的選擇會指派給 SelectedItem.Text] 和 [SelectedItem.Value 屬性中。</span><span class="sxs-lookup"><span data-stu-id="440a7-152">If you type a custom choice into the ComboBox then the custom choice is assigned to both the SelectedItem.Text and SelectedItem.Value properties.</span></span>

## <a name="selecting-the-list-of-items-from-the-database"></a><span data-ttu-id="440a7-153">從資料庫中選取項目的清單</span><span class="sxs-lookup"><span data-stu-id="440a7-153">Selecting the List of Items from the Database</span></span>

<span data-ttu-id="440a7-154">您可以從資料庫擷取下拉式方塊會顯示的項目的清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-154">You can retrieve the list of items that the ComboBox displays from a database.</span></span> <span data-ttu-id="440a7-155">例如，您可以將下拉式方塊 SqlDataSource 控制項、 ObjectDataSource 控制項、 LinqDataSource，或 EntityDataSource 繫結。</span><span class="sxs-lookup"><span data-stu-id="440a7-155">For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.</span></span>

<span data-ttu-id="440a7-156">假設您想要顯示在下拉式方塊的電影清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-156">Imagine that you want to display a list of movies in a ComboBox.</span></span> <span data-ttu-id="440a7-157">您想要從電影資料庫資料表中擷取的電影清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-157">You want to retrieve the list of movies from the Movies database table.</span></span> <span data-ttu-id="440a7-158">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="440a7-158">Follow these steps:</span></span>

1. <span data-ttu-id="440a7-159">建立名為 Movies.aspx 的頁面</span><span class="sxs-lookup"><span data-stu-id="440a7-159">Create a page named Movies.aspx</span></span>
2. <span data-ttu-id="440a7-160">在拖曳到頁面的 [工具箱] 拖曳 [AJAX 延伸模組] 索引標籤底下 ScriptManager 加入網頁 ScriptManager 控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-160">Add a ScriptManager control to the page by dragging the ScriptManager from under the AJAX Extensions tab in the Toolbox onto the page.</span></span>
3. <span data-ttu-id="440a7-161">下拉式方塊拖曳到頁面，頁面將下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-161">Add a ComboBox control to the page by dragging the ComboBox onto the page.</span></span>
4. <span data-ttu-id="440a7-162">在 [設計] 檢視中，將滑鼠移下拉式方塊控制項，然後選取**選擇資料來源**工作選項 （請參閱 [圖 5]）。</span><span class="sxs-lookup"><span data-stu-id="440a7-162">In Design view, hover your mouse over the ComboBox control and select the **Choose Data Source** task option (see Figure 5).</span></span> <span data-ttu-id="440a7-163">資料來源組態精靈會啟動。</span><span class="sxs-lookup"><span data-stu-id="440a7-163">The Data Source Configuration Wizard is launched.</span></span>
5. <span data-ttu-id="440a7-164">在 **選擇資料來源**步驟中，選取&lt;新的資料來源&gt;選項。</span><span class="sxs-lookup"><span data-stu-id="440a7-164">In the **Choose a Data Source** step, select the &lt;New data source�&gt; option.</span></span>
6. <span data-ttu-id="440a7-165">在 **選擇資料來源類型**步驟中，選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="440a7-165">In the **Choose a Data Source Type** step, select Database.</span></span>
7. <span data-ttu-id="440a7-166">在 **選擇資料連接**步驟中，選取您的資料庫 (例如 MoviesDB.mdf)。</span><span class="sxs-lookup"><span data-stu-id="440a7-166">In the **Choose Your Data Connection** step, select your database (for example, MoviesDB.mdf).</span></span>
8. <span data-ttu-id="440a7-167">在 **儲存連接字串儲存到應用程式組態檔**步驟中，選取要儲存您的連接字串的選項。</span><span class="sxs-lookup"><span data-stu-id="440a7-167">In the **Save the Connection String to the Application Configuration File** step, select the option to save your connection string.</span></span>
9. <span data-ttu-id="440a7-168">在 **設定 Select 陳述式**步驟中，選取的電影資料庫資料表，再選取所有資料行。</span><span class="sxs-lookup"><span data-stu-id="440a7-168">In the **Configure the Select Statement** step, select the Movies database table and select all of the columns.</span></span>
10. <span data-ttu-id="440a7-169">在 **測試查詢**步驟中，按一下 完成 按鈕。</span><span class="sxs-lookup"><span data-stu-id="440a7-169">In the **Test Query** step, click the Finish button.</span></span>
11. <span data-ttu-id="440a7-170">回到**選擇資料來源**步驟中，選取要顯示之欄位的標題資料行和資料的識別碼資料行欄位 （請參閱圖）。</span><span class="sxs-lookup"><span data-stu-id="440a7-170">Back in the **Choose Data Source** step, select the Title column for the field to display and the Id column for the data field (see Figure).</span></span>
12. <span data-ttu-id="440a7-171">按一下 [確定] 按鈕，即可關閉精靈。</span><span class="sxs-lookup"><span data-stu-id="440a7-171">Click the OK button to close the wizard.</span></span>


<span data-ttu-id="440a7-172">[![選擇資料來源](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-172">[![Choosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="440a7-173">**圖 05**： 選擇資料來源 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-173">**Figure 05**: Choosing a data source([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="440a7-174">[![選擇資料的文字和值欄位](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-174">[![Choosing the data text and value fields](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span></span>

<span data-ttu-id="440a7-175">**圖 06**： 選擇資料的文字和值欄位 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-175">**Figure 06**: Choosing the data text and value fields([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span></span>


<span data-ttu-id="440a7-176">完成上述步驟之後，下拉式方塊繫結代表影片的電影資料庫資料表從 SqlDataSource 控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-176">After you complete the steps above, the ComboBox is bound to a SqlDataSource control that represents the movies from the Movies database table.</span></span> <span data-ttu-id="440a7-177">頁面的來源是由 （我已清除的格式有點） 的列表 2 所示。</span><span class="sxs-lookup"><span data-stu-id="440a7-177">The source for the page looks like Listing 2 (I cleaned up the formatting a little bit).</span></span>

<span data-ttu-id="440a7-178">**列表 2-Movies.aspx**</span><span class="sxs-lookup"><span data-stu-id="440a7-178">**Listing 2 - Movies.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

<span data-ttu-id="440a7-179">請注意下拉式方塊控制項都有指向 SqlDataSource 控制項 DataSourceID 屬性。</span><span class="sxs-lookup"><span data-stu-id="440a7-179">Notice that the ComboBox control has a DataSourceID property that points to the SqlDataSource control.</span></span> <span data-ttu-id="440a7-180">當您開啟網頁瀏覽器中時，會顯示的資料庫中的電影清單 （請參閱 圖 7）。</span><span class="sxs-lookup"><span data-stu-id="440a7-180">When you open the page in a browser, the list of movies from the database is displayed (see Figure 7).</span></span> <span data-ttu-id="440a7-181">您可以挑選其中一個影片，以從清單或下拉式方塊中輸入電影中輸入新的電影。</span><span class="sxs-lookup"><span data-stu-id="440a7-181">You can either a pick a movie from the list or enter a new movie by typing the movie into the ComboBox.</span></span>


<span data-ttu-id="440a7-182">[![顯示電影的清單](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-182">[![Displaying a list of movies](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span></span>

<span data-ttu-id="440a7-183">**圖 07**： 顯示的電影清單 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-183">**Figure 07**: Displaying a list of movies([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span></span>


## <a name="setting-the-dropdownstyle"></a><span data-ttu-id="440a7-184">設定 DropDownStyle</span><span class="sxs-lookup"><span data-stu-id="440a7-184">Setting the DropDownStyle</span></span>

<span data-ttu-id="440a7-185">您可以使用下拉式方塊 DropDownStyle 屬性來變更下拉式方塊的行為。</span><span class="sxs-lookup"><span data-stu-id="440a7-185">You can use the ComboBox DropDownStyle property to change the behavior of the ComboBox.</span></span> <span data-ttu-id="440a7-186">這個屬性會接受那里可能的值：</span><span class="sxs-lookup"><span data-stu-id="440a7-186">This property accepts there possible values:</span></span>

- <span data-ttu-id="440a7-187">下拉式清單-（預設值） 的下拉式方塊會顯示在下拉式清單中列出當您按一下箭號，您可以輸入自訂值。</span><span class="sxs-lookup"><span data-stu-id="440a7-187">DropDown - (default value) The ComboBox displays a dropdown list when you click the arrow and you can enter a custom value.</span></span>
- <span data-ttu-id="440a7-188">簡單-下拉式方塊會自動顯示下拉式清單中，您可以輸入自訂值。</span><span class="sxs-lookup"><span data-stu-id="440a7-188">Simple - The ComboBox displays a dropdown list automatically and you can enter a custom value.</span></span>
- <span data-ttu-id="440a7-189">DropDownList-下拉式方塊的運作方式類似 DropDownList 控制項。</span><span class="sxs-lookup"><span data-stu-id="440a7-189">DropDownList - The ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="440a7-190">不同之間下拉式清單中，並且會簡單時，會顯示的項目清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-190">The different between DropDown and Simple is when the list of items is displayed.</span></span> <span data-ttu-id="440a7-191">在簡單的情況下，請立即將焦點移至 ComboBox，會顯示清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-191">In the case of Simple, the list is displayed immediately when you move focus to the ComboBox.</span></span> <span data-ttu-id="440a7-192">在下拉式清單中，您必須按一下箭號，若要查看的項目清單。</span><span class="sxs-lookup"><span data-stu-id="440a7-192">In the case of DropDown, you must click the arrow to see the list of items.</span></span>

<span data-ttu-id="440a7-193">DropDownList 值會造成下拉式方塊控制項，就像標準的 DropDownList 控制項一樣運作。</span><span class="sxs-lookup"><span data-stu-id="440a7-193">The DropDownList value causes the ComboBox control to work just like a standard DropDownList control.</span></span> <span data-ttu-id="440a7-194">不過，還有一個重要的差異。</span><span class="sxs-lookup"><span data-stu-id="440a7-194">However, there is an important difference here.</span></span> <span data-ttu-id="440a7-195">舊版 Internet Explorer 會顯示無限的 z 軸索引的 DropDownList 控制項，控制項便會出現在其前面放置任何控制項之前。</span><span class="sxs-lookup"><span data-stu-id="440a7-195">Older versions of Internet Explorer display a DropDownList control with an infinite z-index so the control will appear in front of any control placed in front of it.</span></span> <span data-ttu-id="440a7-196">因為下拉式方塊會呈現 HTML &lt;div&gt;而不是 HTML 標記&lt;選取&gt;標記，下拉式方塊正確尊重疊置順序。</span><span class="sxs-lookup"><span data-stu-id="440a7-196">Because the ComboBox renders an HTML &lt;div&gt; tag instead of an HTML &lt;select&gt; tag, the ComboBox correctly respects z-ordering.</span></span>

## <a name="setting-the-autocompletemode"></a><span data-ttu-id="440a7-197">設定 AutoCompleteMode</span><span class="sxs-lookup"><span data-stu-id="440a7-197">Setting the AutoCompleteMode</span></span>

<span data-ttu-id="440a7-198">您可以使用下拉式方塊 AutoCompleteMode 屬性來指定當有人下拉式方塊中輸入文字時，會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="440a7-198">You use the ComboBox AutoCompleteMode property to specify what happens when someone types text into the ComboBox.</span></span> <span data-ttu-id="440a7-199">這個屬性接受下列可能的值：</span><span class="sxs-lookup"><span data-stu-id="440a7-199">This property accepts the following possible values:</span></span>

- <span data-ttu-id="440a7-200">無-（預設值） 下拉式方塊不會提供任何自動完成行為。</span><span class="sxs-lookup"><span data-stu-id="440a7-200">None - (default value) The ComboBox does not provide any auto-complete behavior.</span></span>
- <span data-ttu-id="440a7-201">建議-下拉式方塊會顯示清單，它會反白顯示清單中的符合項目 （請參閱 圖 8）。</span><span class="sxs-lookup"><span data-stu-id="440a7-201">Suggest - The ComboBox displays the list and it highlights the matching item in the list (see Figure 8).</span></span>
- <span data-ttu-id="440a7-202">附加-下拉式方塊不會顯示清單和附加相符的項目，從清單中拖曳至您輸入的內容 （請參閱 圖 9）。</span><span class="sxs-lookup"><span data-stu-id="440a7-202">Append - The ComboBox does not display the list and it appends the matching item from the list onto what you have typed (see Figure 9).</span></span>
- <span data-ttu-id="440a7-203">同時 SuggestAppend-下拉式方塊會顯示的清單，並將附加相符的項目，從清單中拖曳至您輸入的內容 （請參閱 圖 10）。</span><span class="sxs-lookup"><span data-stu-id="440a7-203">SuggestAppend - The ComboBox both displays the list and appends the matching item from the list onto what you have typed (see Figure 10).</span></span>


<span data-ttu-id="440a7-204">[![下拉式方塊可讓建議](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-204">[![The ComboBox makes a suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span></span>

<span data-ttu-id="440a7-205">**圖 08**: 下拉式方塊會建議 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-205">**Figure 08**: The ComboBox makes a suggestion([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span></span>


<span data-ttu-id="440a7-206">[![下拉式方塊將相符的文字附加](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-206">[![ComboBox appends matching text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span></span>

<span data-ttu-id="440a7-207">**圖 09**： 下拉式方塊將相符的文字附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-207">**Figure 09**: ComboBox appends matching text([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span></span>


<span data-ttu-id="440a7-208">[![下拉式方塊的建議，並將附加](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="440a7-208">[![The ComboBox suggests and appends](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span></span>

<span data-ttu-id="440a7-209">**圖 10**： 建議的下拉式方塊，並附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="440a7-209">**Figure 10**: The ComboBox suggests and appends([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span></span>


## <a name="summary"></a><span data-ttu-id="440a7-210">總結</span><span class="sxs-lookup"><span data-stu-id="440a7-210">Summary</span></span>

<span data-ttu-id="440a7-211">在本教學課程中，您已了解如何使用下拉式方塊控制項來顯示一組固定的項目。</span><span class="sxs-lookup"><span data-stu-id="440a7-211">In this tutorial, you learned how to use the ComboBox control to display a fixed set of items.</span></span> <span data-ttu-id="440a7-212">我們繫結下拉式方塊控制項都設為靜態設定的項目和資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="440a7-212">We bound the ComboBox control both to a static set of items and to a database table.</span></span> <span data-ttu-id="440a7-213">最後，您已了解如何藉由設定其 DropDownStyle 和 AutoCompleteMode 屬性修改下拉式方塊的行為。</span><span class="sxs-lookup"><span data-stu-id="440a7-213">Finally, you learned how to modify the behavior of the ComboBox by setting its DropDownStyle and AutoCompleteMode properties.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="440a7-214">上一步</span><span class="sxs-lookup"><span data-stu-id="440a7-214">Previous</span></span>](how-do-i-use-the-combobox-control-cs.md)
