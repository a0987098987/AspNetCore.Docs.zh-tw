---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: 使用 ColorPicker 控制項擴充項 (C#) |Microsoft 文件
author: microsoft
description: ColorPicker 是用戶端色彩挑選功能提供 UI，快顯功能表控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 4d44fc81305e668b545246cf044dce275563d81a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873845"
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="03dd0-104">使用 ColorPicker 控制項擴充項 (C#)</span><span class="sxs-lookup"><span data-stu-id="03dd0-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="03dd0-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="03dd0-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="03dd0-106">ColorPicker 是用戶端色彩挑選功能提供 UI，快顯功能表控制項中的 ASP.NET AJAX 擴充項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="03dd0-107">可以將它附加至任何 ASP.NET 文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="03dd0-108">它。</span><span class="sxs-lookup"><span data-stu-id="03dd0-108">It.</span></span>


<span data-ttu-id="03dd0-109">本教學課程的目標是要說明如何使用 AJAX 控制項 Toolkit ColorPicker 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="03dd0-110">ColorPicker 控制項擴充項會顯示快顯對話方塊中，可讓您選取的色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="03dd0-111">每當您想要提供直覺式使用者介面來選擇色彩的使用者，ColorPicker 很有用。</span><span class="sxs-lookup"><span data-stu-id="03dd0-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="03dd0-112">擴充 ColorPicker 控制項擴充項的 TextBox 控制項</span><span class="sxs-lookup"><span data-stu-id="03dd0-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="03dd0-113">例如，假設您想要建立網站，讓建立自訂的名片的訪客。</span><span class="sxs-lookup"><span data-stu-id="03dd0-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="03dd0-114">訪客可以商務名片輸入的文字，並選取色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="03dd0-115">程式碼範例 1 中的 ASP.NET 網頁包含名為 txtCardText 和 txtCardColor 的兩個文字方塊控制項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="03dd0-116">當您送出表單時，會顯示選取的值 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="03dd0-117">[![簡單的表單建立名片](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="03dd0-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="03dd0-118">**圖 01**： 簡單的表單建立名片 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="03dd0-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="03dd0-119">**Listing 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="03dd0-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="03dd0-120">表單的程式碼範例 1 是有效的但它不提供絕佳的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="03dd0-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="03dd0-121">使用者必須輸入的文字方塊中的色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="03dd0-122">如果使用者想要特製化的色彩： 例如，只要右陰影 pea 綠色-然後使用者必須找出 HTML 色彩代碼，而不需任何協助。</span><span class="sxs-lookup"><span data-stu-id="03dd0-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="03dd0-123">若要建立較佳使用者體驗，您可以使用 ColorPicker 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="03dd0-124">當您將焦點移至 TextBox 控制項 ColorPicker 會顯示色彩對話方塊 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="03dd0-125">[![ColorPicker 控制項擴充項](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="03dd0-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="03dd0-126">**圖 02**: ColorPicker 控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="03dd0-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="03dd0-127">您必須完成表單的程式碼範例 1 與使用 ColorPicker 控制項擴充項的兩個步驟：</span><span class="sxs-lookup"><span data-stu-id="03dd0-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="03dd0-128">ScriptManager 控制項加入網頁</span><span class="sxs-lookup"><span data-stu-id="03dd0-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="03dd0-129">頁面上新增 ColorPicker 控制項擴充項</span><span class="sxs-lookup"><span data-stu-id="03dd0-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="03dd0-130">您可以使用 ColorPicker 之前，您必須將 ScriptManager 加入您的頁面。</span><span class="sxs-lookup"><span data-stu-id="03dd0-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="03dd0-131">若要加入 ScriptManager 最好先開啟伺服器端的正下方&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="03dd0-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="03dd0-132">您可以拖曳到頁面的 ScriptManager，從工具箱 （ScriptManager 位於 AJAX 擴充功能 索引標籤）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="03dd0-133">或者，您可以輸入下列標記來源檢視下方開頭伺服器端表單的標記：</span><span class="sxs-lookup"><span data-stu-id="03dd0-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="03dd0-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="03dd0-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="03dd0-135">ColorPicker 控制項擴充項新增至頁面的最簡單方式是在設計檢視中。</span><span class="sxs-lookup"><span data-stu-id="03dd0-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="03dd0-136">如果您將滑鼠停留 txtCardColor 文字方塊中，智慧工作選項會顯示可讓您將擴充項 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="03dd0-137">如果您選擇此選項時，擴充性精靈 隨即出現 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="03dd0-138">[![新增 extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="03dd0-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="03dd0-139">**圖 03**： 加入擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="03dd0-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="03dd0-140">[![選取的控制項擴充項使用擴充性精靈](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="03dd0-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="03dd0-141">**圖 04**： 選取 使用 Extender 精靈控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="03dd0-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="03dd0-142">您可以挑選 ColorPicker extender 來延伸與 ColorPicker extender txtCardColor 文字方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="03dd0-143">按一下 [確定] 以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="03dd0-144">進行這些變更之後，頁面來源看起來像列表 2 中。</span><span class="sxs-lookup"><span data-stu-id="03dd0-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="03dd0-145">列出 2-CreateCard.aspx （具有 ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="03dd0-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="03dd0-146">請注意，頁面現在已包含 ColorPickerExtender 控制項 txtCardColor TextBox 控制項的正下方出現。</span><span class="sxs-lookup"><span data-stu-id="03dd0-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="03dd0-147">ColorPickerExtender 控制擴充 txtCardColor 控制項使其顯示色彩選擇器對話方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="03dd0-148">使用按鈕以啟動色彩選擇器對話方塊</span><span class="sxs-lookup"><span data-stu-id="03dd0-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="03dd0-149">ColorPicker extender 支援下列屬性：</span><span class="sxs-lookup"><span data-stu-id="03dd0-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="03dd0-150">PopupButtonId-會導致出現色彩選擇器對話方塊頁面上的按鈕的識別碼。</span><span class="sxs-lookup"><span data-stu-id="03dd0-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="03dd0-151">PopupPosition-的位置，相對於目標控制項的 [色彩選擇器] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="03dd0-152">可能的值為絕對、 中心、 BottomLeft、 BottomRight、 TopLeft、 TopRight 權限和左側 （預設為 BottomLeft）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="03dd0-153">SampleControlId-顯示所選取之色彩的控制項識別碼。</span><span class="sxs-lookup"><span data-stu-id="03dd0-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="03dd0-154">SelectedColor-ColorPicker 所選取的初始色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="03dd0-155">您可以使用這些屬性來自訂顯示色彩選擇器 對話方塊的方式，以及所選取之色彩的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="03dd0-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="03dd0-156">[] 頁面中列出的 3 說明您如何使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="03dd0-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="03dd0-157">**Listing 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="03dd0-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="03dd0-158">[] 頁面中列出的 3 包含挑選顏色按鈕 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="03dd0-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="03dd0-159">當您按一下這個按鈕時，色彩選擇器對話方塊上方的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="03dd0-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="03dd0-160">如果您選取色彩對話方塊中選取的色彩就會顯示為 lblSample Label 控制項的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="03dd0-161">ColorPicker PopupButtonID 屬性用來與 ColorPicker extender 挑選顏色 按鈕。</span><span class="sxs-lookup"><span data-stu-id="03dd0-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="03dd0-162">當您提供 PopupButtonID 屬性的值時，色彩選擇器對話方塊不會顯示目標控制項有焦點時。</span><span class="sxs-lookup"><span data-stu-id="03dd0-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="03dd0-163">您必須按一下按鈕以顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="03dd0-164">SampleControlID 屬性用來將會顯示所選取之色彩 ColorPicker 與控制項產生關聯。</span><span class="sxs-lookup"><span data-stu-id="03dd0-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="03dd0-165">ColorPicker 變更這個控制項的背景色彩為目前選取的色彩。</span><span class="sxs-lookup"><span data-stu-id="03dd0-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="03dd0-166">[![顯示色彩選擇器 對話方塊，使用按鈕](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="03dd0-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="03dd0-167">**圖 05**： 顯示色彩選擇器 對話方塊，使用按鈕 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="03dd0-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="03dd0-168">總結</span><span class="sxs-lookup"><span data-stu-id="03dd0-168">Summary</span></span>

<span data-ttu-id="03dd0-169">在本教學課程中，您將學會如何顯示快顯色彩選擇器對話方塊使用 ColorPicker 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="03dd0-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="03dd0-170">首先，我們會檢查當焦點移至 TextBox 控制項時，如何顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="03dd0-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="03dd0-171">接下來，您會學到如何建立顯示色彩選擇器對話方塊時按下按鈕的按鈕。</span><span class="sxs-lookup"><span data-stu-id="03dd0-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="03dd0-172">下一步</span><span class="sxs-lookup"><span data-stu-id="03dd0-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
