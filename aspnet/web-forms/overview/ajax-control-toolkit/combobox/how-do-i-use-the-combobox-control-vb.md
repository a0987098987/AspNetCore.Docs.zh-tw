---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: 如何使用下拉式方塊控制項？ (VB) | Microsoft Docs
author: microsoft
description: 下拉式方塊是一種 ASP.NET AJAX 控制項，結合的使用者可以從中選擇的選項清單中的文字方塊中的彈性。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e42844e326cb190502a51c5a85195b4752d7e827
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875405"
---
<a name="how-do-i-use-the-combobox-control-vb"></a><span data-ttu-id="07619-104">如何使用下拉式方塊控制項？</span><span class="sxs-lookup"><span data-stu-id="07619-104">How do I use the ComboBox Control?</span></span> <span data-ttu-id="07619-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="07619-105">(VB)</span></span>
====================
<span data-ttu-id="07619-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="07619-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="07619-107">下拉式方塊是一種 ASP.NET AJAX 控制項，結合的使用者可以從中選擇的選項清單中的文字方塊中的彈性。</span><span class="sxs-lookup"><span data-stu-id="07619-107">ComboBox is an ASP.NET AJAX control that combines the flexibility of a TextBox with a list of options from which users can choose.</span></span>


<span data-ttu-id="07619-108">本教學課程的目標是來說明 AJAX 控制項 Toolkit 下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-108">The goal of this tutorial is to explain the AJAX Control Toolkit ComboBox control.</span></span> <span data-ttu-id="07619-109">下拉式方塊的運作方式類似標準 ASP.NET DropDownList 控制項和文字方塊控制項之間的組合。</span><span class="sxs-lookup"><span data-stu-id="07619-109">The ComboBox works like a combination between a standard ASP.NET DropDownList control and a TextBox control.</span></span> <span data-ttu-id="07619-110">您可以從預先存在的項目清單中選取或輸入新的項目。</span><span class="sxs-lookup"><span data-stu-id="07619-110">You can either select from a pre-existing list of items or enter a new item.</span></span>

<span data-ttu-id="07619-111">下拉式方塊類似自動完成控制項擴充項，但會在不同案例中使用的控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-111">The ComboBox is similar to the AutoComplete control extender, but the controls are used in different scenarios.</span></span> <span data-ttu-id="07619-112">自動完成 extender 查詢 web 服務以取得相符的項目。</span><span class="sxs-lookup"><span data-stu-id="07619-112">The AutoComplete extender queries a web service to get matching entries.</span></span> <span data-ttu-id="07619-113">下拉式方塊控制項，相較之下，以初始化一組項目。</span><span class="sxs-lookup"><span data-stu-id="07619-113">The ComboBox control, in contrast, is initialized with a set of items.</span></span> <span data-ttu-id="07619-114">使用自動完成 extender 使意義上，當您正在使用的大量資料 （包含數百萬個汽車組件） 時使用下拉式方塊控制項合理使用較少的資料時 （數十個汽車組件）。</span><span class="sxs-lookup"><span data-stu-id="07619-114">Using the AutoComplete extender makes sense when you are working with a large set of data (millions of car parts) while using the ComboBox control makes sense when working with a small set of data (dozens of car parts).</span></span>

## <a name="selecting-from-a-static-list-of-items"></a><span data-ttu-id="07619-115">從靜態項目清單中選取</span><span class="sxs-lookup"><span data-stu-id="07619-115">Selecting from a Static List of Items</span></span>

<span data-ttu-id="07619-116">可讓 s 開頭使用下拉式方塊控制項的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="07619-116">Let�s start with a simple sample of using the ComboBox control.</span></span> <span data-ttu-id="07619-117">假設您想要顯示在下拉式清單中的靜態清單的項目。</span><span class="sxs-lookup"><span data-stu-id="07619-117">Imagine that you want to display a static list of items in a dropdown list.</span></span> <span data-ttu-id="07619-118">不過，您會想要保持開啟的清單不完整的可能性。</span><span class="sxs-lookup"><span data-stu-id="07619-118">However, you want to leave open the possibility that the list is not complete.</span></span> <span data-ttu-id="07619-119">您想要讓使用者輸入自訂值的清單中。</span><span class="sxs-lookup"><span data-stu-id="07619-119">You want to allow a user to enter a custom value into the list.</span></span>

<span data-ttu-id="07619-120">我們 ll 建立新的 ASP.NET Web Form 頁面和頁面中使用下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-120">We�ll create a new ASP.NET Web Forms page and use the ComboBox control in the page.</span></span> <span data-ttu-id="07619-121">專案中加入新的 ASP.NET 網頁，並切換至 [設計] 檢視中。</span><span class="sxs-lookup"><span data-stu-id="07619-121">Add the new ASP.NET page to your project and switch to Design view.</span></span>

<span data-ttu-id="07619-122">如果您想要使用下拉式方塊控制項頁面中您必須將 ScriptManager 控制項加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="07619-122">If you want to use the ComboBox control in the page then you must add a ScriptManager control to the page.</span></span> <span data-ttu-id="07619-123">將 [AJAX 擴充功能] 索引標籤 from beneath ScriptManager 控制項拖曳至設計工具介面。</span><span class="sxs-lookup"><span data-stu-id="07619-123">Drag the ScriptManager control from beneath the AJAX Extensions tab onto the Designer surface.</span></span> <span data-ttu-id="07619-124">您應該在頁面頂端的; 加入 ScriptManager 控制項您可以將它立即開啟伺服器端下方&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="07619-124">You should add the ScriptManager control at the top of the page; you can add it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="07619-125">下一步，拖曳到頁面的下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-125">Next, drag the ComboBox control onto the page.</span></span> <span data-ttu-id="07619-126">您可以使用其他 AJAX Control Toolkit 控制項和控制項擴充項 （請參閱圖 1） 的 [工具箱] 中找到 ComboBox 控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-126">You can find the ComboBox control in the Toolbox with the other AJAX Control Toolkit controls and control extenders (see figure1).</span></span>


<span data-ttu-id="07619-127">[![簡單的表單建立名片](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="07619-127">[![Simple form for creating a business card](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="07619-128">**圖 01**： 從 [工具箱] 中選取 ComboBox 控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="07619-128">**Figure 01**: Selecting the ComboBox control from the toolbox ([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="07619-129">我們 ll 用於 ComboBox 控制項顯示靜態選項清單。</span><span class="sxs-lookup"><span data-stu-id="07619-129">We�ll use the ComboBox control to display a static list of choices.</span></span> <span data-ttu-id="07619-130">使用者可以從三個選項的清單中選取食物的 spiciness 特定層級： 不雅、 中型和作用 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="07619-130">The user can select a particular level of spiciness for their food from a list of three choices: Mild, Medium, and Hot (see Figure 2).</span></span>


<span data-ttu-id="07619-131">[![從靜態項目清單中選取](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="07619-131">[![Selecting from a static list of items](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="07619-132">**圖 02**： 從靜態項目清單中選取 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="07619-132">**Figure 02**: Selecting from a static list of items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="07619-133">有兩種方式，您可以將這些選擇加入的下拉式方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-133">There are two ways that you can add these choices to the ComboBox control.</span></span> <span data-ttu-id="07619-134">首先，當滑鼠停留在 [設計] 檢視中的控制項選取編輯選項工作選項，並開啟項目編輯器 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="07619-134">First, you select the Edit Options task option when hovering your mouse over the control in Design view and open the Item Editor (see Figure 3).</span></span>


<span data-ttu-id="07619-135">[![編輯下拉式方塊項目](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="07619-135">[![Editing ComboBox items](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="07619-136">**圖 03**： 編輯下拉式方塊項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="07619-136">**Figure 03**: Editing ComboBox items([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="07619-137">第二個選項是加入在開頭和結尾之間的項目清單&lt;asp: ComboBox&gt;來源檢視中的標籤。</span><span class="sxs-lookup"><span data-stu-id="07619-137">The second option is to add the list of items in between the opening and closing &lt;asp:ComboBox&gt; tags in Source view.</span></span> <span data-ttu-id="07619-138">程式碼範例 1 中的頁面包含更新具有項目清單的下拉式方塊。</span><span class="sxs-lookup"><span data-stu-id="07619-138">The page in Listing 1 contains the updated ComboBox that has the list of items.</span></span>

<span data-ttu-id="07619-139">**Listing 1 - Static.aspx**</span><span class="sxs-lookup"><span data-stu-id="07619-139">**Listing 1 - Static.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

<span data-ttu-id="07619-140">當您開啟的頁面清單 1 中時，您可以從下拉式方塊選取其中一個預先存在的選項。</span><span class="sxs-lookup"><span data-stu-id="07619-140">When you open the page in Listing 1, you can select one of the pre-existing options from the ComboBox.</span></span> <span data-ttu-id="07619-141">換句話說，下拉式方塊的運作方式類似 DropDownList 控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-141">In other words, the ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="07619-142">不過，您也可以輸入選項 （例如超級辣味） 不在現有的清單中的選擇。</span><span class="sxs-lookup"><span data-stu-id="07619-142">However, you also have the option of entering a new choice (for example, Super Spicy) that is not in the existing list.</span></span> <span data-ttu-id="07619-143">因此，下拉式方塊也運作方式類似的 TextBox 控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-143">So, the ComboBox also works like a TextBox control.</span></span>

<span data-ttu-id="07619-144">不論您是否選擇既存的項目，或輸入自訂的項目，當您送出表單，您的選擇會出現在 label 控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-144">Regardless of whether you pick a pre-existing item or you enter a custom item, when you submit the form, your choice appears in the label control.</span></span> <span data-ttu-id="07619-145">當您提交表單，btnSubmit\_按一下 執行處理常式，並更新標籤 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="07619-145">When you submit the form, the btnSubmit\_Click handler executes and updates the label (see Figure 4).</span></span>


<span data-ttu-id="07619-146">[![顯示選取的項目](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="07619-146">[![Displaying the selected item](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="07619-147">**圖 04**： 顯示選取的項目 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="07619-147">**Figure 04**: Displaying the selected item([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="07619-148">下拉式方塊支援 DropDownList 控制項相同的內容之後提交表單時，擷取選取的項目：</span><span class="sxs-lookup"><span data-stu-id="07619-148">The ComboBox supports the same properties as the DropDownList control for retrieving the selected item after a form is submitted:</span></span>

- <span data-ttu-id="07619-149">SelectedItem.Text-顯示選取之項目的文字屬性的值。</span><span class="sxs-lookup"><span data-stu-id="07619-149">SelectedItem.Text - Displays the value of the Text property of the selected item.</span></span>
- <span data-ttu-id="07619-150">SelectedItem.Value-顯示選取之項目的值屬性的值，或顯示下拉式方塊中輸入的文字。</span><span class="sxs-lookup"><span data-stu-id="07619-150">SelectedItem.Value - Displays the value of the Value property of the selected item or displays the text typed into the ComboBox.</span></span>
- <span data-ttu-id="07619-151">SelectedValue-相同 SelectedItem.Value 不同之處在於此屬性可讓您指定預設值 （初始） 選取的項目。</span><span class="sxs-lookup"><span data-stu-id="07619-151">SelectedValue - Same as SelectedItem.Value except that this property enables you to specify the default (initial) selected item.</span></span>

<span data-ttu-id="07619-152">如果您輸入至下拉式方塊然後自訂選項的自訂選項會指派給 SelectedItem.Text 和 SelectedItem.Value 屬性。</span><span class="sxs-lookup"><span data-stu-id="07619-152">If you type a custom choice into the ComboBox then the custom choice is assigned to both the SelectedItem.Text and SelectedItem.Value properties.</span></span>

## <a name="selecting-the-list-of-items-from-the-database"></a><span data-ttu-id="07619-153">從資料庫中選取項目的清單</span><span class="sxs-lookup"><span data-stu-id="07619-153">Selecting the List of Items from the Database</span></span>

<span data-ttu-id="07619-154">您可以從資料庫擷取下拉式方塊會顯示的項目的清單。</span><span class="sxs-lookup"><span data-stu-id="07619-154">You can retrieve the list of items that the ComboBox displays from a database.</span></span> <span data-ttu-id="07619-155">例如，您可以繫結下拉式方塊 SqlDataSource 控制項、 ObjectDataSource 控制項、 LinqDataSource 或 EntityDataSource。</span><span class="sxs-lookup"><span data-stu-id="07619-155">For example, you can bind the ComboBox to a SqlDataSource control, an ObjectDataSource control, a LinqDataSource, or an EntityDataSource.</span></span>

<span data-ttu-id="07619-156">假設您想要在下拉式方塊中顯示的電影清單。</span><span class="sxs-lookup"><span data-stu-id="07619-156">Imagine that you want to display a list of movies in a ComboBox.</span></span> <span data-ttu-id="07619-157">您想要從電影資料庫資料表中擷取的電影清單。</span><span class="sxs-lookup"><span data-stu-id="07619-157">You want to retrieve the list of movies from the Movies database table.</span></span> <span data-ttu-id="07619-158">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="07619-158">Follow these steps:</span></span>

1. <span data-ttu-id="07619-159">建立名為 Movies.aspx 的頁面</span><span class="sxs-lookup"><span data-stu-id="07619-159">Create a page named Movies.aspx</span></span>
2. <span data-ttu-id="07619-160">加入至頁面 ScriptManager 控制項拖曳到頁面的 [工具箱] 中拖曳 [AJAX 擴充功能] 索引標籤底下 ScriptManager。</span><span class="sxs-lookup"><span data-stu-id="07619-160">Add a ScriptManager control to the page by dragging the ScriptManager from under the AJAX Extensions tab in the Toolbox onto the page.</span></span>
3. <span data-ttu-id="07619-161">加入頁面的下拉式方塊控制項拖曳到頁面的下拉式方塊。</span><span class="sxs-lookup"><span data-stu-id="07619-161">Add a ComboBox control to the page by dragging the ComboBox onto the page.</span></span>
4. <span data-ttu-id="07619-162">在設計檢視中，滑鼠停留下拉式方塊控制項，然後選取**選擇資料來源**工作選項 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="07619-162">In Design view, hover your mouse over the ComboBox control and select the **Choose Data Source** task option (see Figure 5).</span></span> <span data-ttu-id="07619-163">資料來源組態精靈會啟動。</span><span class="sxs-lookup"><span data-stu-id="07619-163">The Data Source Configuration Wizard is launched.</span></span>
5. <span data-ttu-id="07619-164">在**選擇資料來源**步驟中，選取&lt;新的資料來源&gt;選項。</span><span class="sxs-lookup"><span data-stu-id="07619-164">In the **Choose a Data Source** step, select the &lt;New data source�&gt; option.</span></span>
6. <span data-ttu-id="07619-165">在**選擇資料來源類型**步驟中，選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="07619-165">In the **Choose a Data Source Type** step, select Database.</span></span>
7. <span data-ttu-id="07619-166">在**選擇資料連接**步驟中，選取您的資料庫 (例如，MoviesDB.mdf)。</span><span class="sxs-lookup"><span data-stu-id="07619-166">In the **Choose Your Data Connection** step, select your database (for example, MoviesDB.mdf).</span></span>
8. <span data-ttu-id="07619-167">在**連接字串儲存到應用程式組態檔**步驟中，選取要儲存您的連接字串選項。</span><span class="sxs-lookup"><span data-stu-id="07619-167">In the **Save the Connection String to the Application Configuration File** step, select the option to save your connection string.</span></span>
9. <span data-ttu-id="07619-168">在**設定 Select 陳述式**步驟、 選取影片資料庫資料表，再選取所有資料行。</span><span class="sxs-lookup"><span data-stu-id="07619-168">In the **Configure the Select Statement** step, select the Movies database table and select all of the columns.</span></span>
10. <span data-ttu-id="07619-169">在**測試查詢**步驟中，按一下 [完成] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="07619-169">In the **Test Query** step, click the Finish button.</span></span>
11. <span data-ttu-id="07619-170">回到**選擇資料來源**步驟中，選取要顯示之欄位的標題資料行和資料的識別碼資料行欄位 （請參閱圖）。</span><span class="sxs-lookup"><span data-stu-id="07619-170">Back in the **Choose Data Source** step, select the Title column for the field to display and the Id column for the data field (see Figure).</span></span>
12. <span data-ttu-id="07619-171">按一下 [確定] 按鈕以關閉精靈。</span><span class="sxs-lookup"><span data-stu-id="07619-171">Click the OK button to close the wizard.</span></span>


<span data-ttu-id="07619-172">[![選擇資料來源](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="07619-172">[![Choosing a data source](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="07619-173">**圖 05**： 選擇資料來源 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="07619-173">**Figure 05**: Choosing a data source([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="07619-174">[![選擇資料的文字和值欄位](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="07619-174">[![Choosing the data text and value fields](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)</span></span>

<span data-ttu-id="07619-175">**圖 06**： 選擇資料的文字和值欄位 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="07619-175">**Figure 06**: Choosing the data text and value fields([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image12.png))</span></span>


<span data-ttu-id="07619-176">完成上述步驟之後，下拉式方塊都代表電影電影資料庫資料表中的 SqlDataSource 控制項繫結。</span><span class="sxs-lookup"><span data-stu-id="07619-176">After you complete the steps above, the ComboBox is bound to a SqlDataSource control that represents the movies from the Movies database table.</span></span> <span data-ttu-id="07619-177">來源頁面看起來像列出 2 （我清除有點格式）。</span><span class="sxs-lookup"><span data-stu-id="07619-177">The source for the page looks like Listing 2 (I cleaned up the formatting a little bit).</span></span>

<span data-ttu-id="07619-178">**列出 2-Movies.aspx**</span><span class="sxs-lookup"><span data-stu-id="07619-178">**Listing 2 - Movies.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

<span data-ttu-id="07619-179">請注意下拉式方塊控制項有指向 SqlDataSource 控制項 DataSourceID 屬性。</span><span class="sxs-lookup"><span data-stu-id="07619-179">Notice that the ComboBox control has a DataSourceID property that points to the SqlDataSource control.</span></span> <span data-ttu-id="07619-180">當您開啟的網頁瀏覽器中時，會顯示的影片，從資料庫清單 （請參閱圖 7）。</span><span class="sxs-lookup"><span data-stu-id="07619-180">When you open the page in a browser, the list of movies from the database is displayed (see Figure 7).</span></span> <span data-ttu-id="07619-181">您可以挑選其中一個電影從清單或下拉式方塊中輸入電影中輸入新的電影。</span><span class="sxs-lookup"><span data-stu-id="07619-181">You can either a pick a movie from the list or enter a new movie by typing the movie into the ComboBox.</span></span>


<span data-ttu-id="07619-182">[![顯示電影的清單](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="07619-182">[![Displaying a list of movies](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)</span></span>

<span data-ttu-id="07619-183">**圖 07**： 顯示的電影清單 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="07619-183">**Figure 07**: Displaying a list of movies([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image14.png))</span></span>


## <a name="setting-the-dropdownstyle"></a><span data-ttu-id="07619-184">設定 DropDownStyle</span><span class="sxs-lookup"><span data-stu-id="07619-184">Setting the DropDownStyle</span></span>

<span data-ttu-id="07619-185">您可以使用下拉式方塊 DropDownStyle 屬性，變更下拉式方塊的行為。</span><span class="sxs-lookup"><span data-stu-id="07619-185">You can use the ComboBox DropDownStyle property to change the behavior of the ComboBox.</span></span> <span data-ttu-id="07619-186">這個屬性可以接受那里可能的值：</span><span class="sxs-lookup"><span data-stu-id="07619-186">This property accepts there possible values:</span></span>

- <span data-ttu-id="07619-187">在下拉式清單中的 （預設值） 的下拉式方塊會顯示下拉式清單，當您按一下箭號，而且您可以輸入自訂值。</span><span class="sxs-lookup"><span data-stu-id="07619-187">DropDown - (default value) The ComboBox displays a dropdown list when you click the arrow and you can enter a custom value.</span></span>
- <span data-ttu-id="07619-188">簡單-下拉式方塊會自動顯示下拉式清單中，您可以輸入自訂值。</span><span class="sxs-lookup"><span data-stu-id="07619-188">Simple - The ComboBox displays a dropdown list automatically and you can enter a custom value.</span></span>
- <span data-ttu-id="07619-189">DropDownList-下拉式方塊的運作方式類似 DropDownList 控制項。</span><span class="sxs-lookup"><span data-stu-id="07619-189">DropDownList - The ComboBox works just like a DropDownList control.</span></span>

<span data-ttu-id="07619-190">不同之間下拉式清單中且簡單時，會顯示的項目清單。</span><span class="sxs-lookup"><span data-stu-id="07619-190">The different between DropDown and Simple is when the list of items is displayed.</span></span> <span data-ttu-id="07619-191">在簡單的情況下立即將焦點移到下拉式方塊會顯示清單。</span><span class="sxs-lookup"><span data-stu-id="07619-191">In the case of Simple, the list is displayed immediately when you move focus to the ComboBox.</span></span> <span data-ttu-id="07619-192">在下拉式清單中，您必須按一下箭頭來查看的項目清單。</span><span class="sxs-lookup"><span data-stu-id="07619-192">In the case of DropDown, you must click the arrow to see the list of items.</span></span>

<span data-ttu-id="07619-193">DropDownList 值導致 ComboBox 控制項就像標準的 DropDownList 控制項一樣運作。</span><span class="sxs-lookup"><span data-stu-id="07619-193">The DropDownList value causes the ComboBox control to work just like a standard DropDownList control.</span></span> <span data-ttu-id="07619-194">不過，是一個重要的差異。</span><span class="sxs-lookup"><span data-stu-id="07619-194">However, there is an important difference here.</span></span> <span data-ttu-id="07619-195">舊版 Internet Explorer 會顯示具有無限 z-索引 DropDownList 控制項，控制項就會出現任何控制項放置在它前面的前面。</span><span class="sxs-lookup"><span data-stu-id="07619-195">Older versions of Internet Explorer display a DropDownList control with an infinite z-index so the control will appear in front of any control placed in front of it.</span></span> <span data-ttu-id="07619-196">因為下拉式方塊可呈現 HTML &lt;div&gt;標記，而不是 HTML&lt;選取&gt;標記，下拉式方塊正確尊重疊置順序。</span><span class="sxs-lookup"><span data-stu-id="07619-196">Because the ComboBox renders an HTML &lt;div&gt; tag instead of an HTML &lt;select&gt; tag, the ComboBox correctly respects z-ordering.</span></span>

## <a name="setting-the-autocompletemode"></a><span data-ttu-id="07619-197">設定 AutoCompleteMode</span><span class="sxs-lookup"><span data-stu-id="07619-197">Setting the AutoCompleteMode</span></span>

<span data-ttu-id="07619-198">您可以使用下拉式方塊 AutoCompleteMode 屬性來指定有人類型到下拉式方塊的文字時，會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="07619-198">You use the ComboBox AutoCompleteMode property to specify what happens when someone types text into the ComboBox.</span></span> <span data-ttu-id="07619-199">這個屬性可以接受下列可能的值：</span><span class="sxs-lookup"><span data-stu-id="07619-199">This property accepts the following possible values:</span></span>

- <span data-ttu-id="07619-200">無-（預設值） 下拉式方塊不會提供任何自動完成行為。</span><span class="sxs-lookup"><span data-stu-id="07619-200">None - (default value) The ComboBox does not provide any auto-complete behavior.</span></span>
- <span data-ttu-id="07619-201">建議的下拉式方塊會顯示清單，它會反白顯示清單中的符合項目 （請參閱圖 8）。</span><span class="sxs-lookup"><span data-stu-id="07619-201">Suggest - The ComboBox displays the list and it highlights the matching item in the list (see Figure 8).</span></span>
- <span data-ttu-id="07619-202">附加-下拉式方塊不會顯示清單並附加到您輸入的內容 （請參閱圖 9） 清單中的符合項目。</span><span class="sxs-lookup"><span data-stu-id="07619-202">Append - The ComboBox does not display the list and it appends the matching item from the list onto what you have typed (see Figure 9).</span></span>
- <span data-ttu-id="07619-203">SuggestAppend-下拉式方塊會顯示清單並將附加到您輸入的內容 （請參閱圖 10） 清單中的符合項目。</span><span class="sxs-lookup"><span data-stu-id="07619-203">SuggestAppend - The ComboBox both displays the list and appends the matching item from the list onto what you have typed (see Figure 10).</span></span>


<span data-ttu-id="07619-204">[![下拉式方塊可讓提供的建議](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="07619-204">[![The ComboBox makes a suggestion](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)</span></span>

<span data-ttu-id="07619-205">**圖 08**: 下拉式方塊會建議 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="07619-205">**Figure 08**: The ComboBox makes a suggestion([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image16.png))</span></span>


<span data-ttu-id="07619-206">[![下拉式方塊將相符的文字附加](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="07619-206">[![ComboBox appends matching text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)</span></span>

<span data-ttu-id="07619-207">**圖 09**： 下拉式方塊將相符的文字附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="07619-207">**Figure 09**: ComboBox appends matching text([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image18.png))</span></span>


<span data-ttu-id="07619-208">[![下拉式方塊的建議，並將附加](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="07619-208">[![The ComboBox suggests and appends](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)</span></span>

<span data-ttu-id="07619-209">**圖 10**: ComboBox 建議，並將附加 ([按一下以檢視完整大小的影像](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span><span class="sxs-lookup"><span data-stu-id="07619-209">**Figure 10**: The ComboBox suggests and appends([Click to view full-size image](how-do-i-use-the-combobox-control-vb/_static/image20.png))</span></span>


## <a name="summary"></a><span data-ttu-id="07619-210">總結</span><span class="sxs-lookup"><span data-stu-id="07619-210">Summary</span></span>

<span data-ttu-id="07619-211">在本教學課程中，您學會如何使用下拉式方塊控制項來顯示一組固定的項目。</span><span class="sxs-lookup"><span data-stu-id="07619-211">In this tutorial, you learned how to use the ComboBox control to display a fixed set of items.</span></span> <span data-ttu-id="07619-212">我們繫結下拉式方塊控制項，同時為靜態設定的項目與資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="07619-212">We bound the ComboBox control both to a static set of items and to a database table.</span></span> <span data-ttu-id="07619-213">最後，您會學到如何藉由設定其 DropDownStyle 而且 AutoCompleteMode 屬性修改下拉式方塊的行為。</span><span class="sxs-lookup"><span data-stu-id="07619-213">Finally, you learned how to modify the behavior of the ComboBox by setting its DropDownStyle and AutoCompleteMode properties.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="07619-214">上一步</span><span class="sxs-lookup"><span data-stu-id="07619-214">Previous</span></span>](how-do-i-use-the-combobox-control-cs.md)
