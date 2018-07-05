---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: 使用 ColorPicker 控制項擴充項 (C#) |Microsoft Docs
author: microsoft
description: ColorPicker 是提供 UI 中的用戶端色彩挑選功能，在快顯視窗控制項中的 ASP.NET AJAX 擴充項。 可以將它附加至任何 ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: f20928099e2b4db477705cd1634fd28745a328ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383900"
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="4be93-104">使用 ColorPicker 控制項擴充項 (C#)</span><span class="sxs-lookup"><span data-stu-id="4be93-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="4be93-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4be93-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4be93-106">ColorPicker 是提供 UI 中的用戶端色彩挑選功能，在快顯視窗控制項中的 ASP.NET AJAX 擴充項。</span><span class="sxs-lookup"><span data-stu-id="4be93-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="4be93-107">可以將它附加至任何 ASP.NET TextBox 控制項。</span><span class="sxs-lookup"><span data-stu-id="4be93-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="4be93-108">它。</span><span class="sxs-lookup"><span data-stu-id="4be93-108">It.</span></span>


<span data-ttu-id="4be93-109">本教學課程的目標是要說明如何使用 AJAX 控制項工具組 ColorPicker 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="4be93-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="4be93-110">ColorPicker 控制項擴充項會顯示快顯對話方塊中，可讓您選取色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="4be93-111">ColorPicker 適合，每當您想要提供直覺式使用者介面來選擇色彩的使用者。</span><span class="sxs-lookup"><span data-stu-id="4be93-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="4be93-112">擴充 ColorPicker 控制項擴充項與 TextBox 控制項</span><span class="sxs-lookup"><span data-stu-id="4be93-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="4be93-113">例如，假設您想要建立網站，可讓訪客建立自訂的名片。</span><span class="sxs-lookup"><span data-stu-id="4be93-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="4be93-114">訪客可以輸入文字的名片，挑選色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="4be93-115">列表 1 中的 [ASP.NET] 頁面包含兩個名為 txtCardText 和 txtCardColor 的 TextBox 控制項。</span><span class="sxs-lookup"><span data-stu-id="4be93-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="4be93-116">當您提交表單時，會顯示選取的值 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="4be93-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="4be93-117">[![簡單的表單建立名片](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4be93-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="4be93-118">**圖 01**： 建立名片的簡單表單 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4be93-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="4be93-119">**列表 1-CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="4be93-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="4be93-120">表單的程式碼範例 1 的運作方式，但它不提供絕佳的使用者體驗。</span><span class="sxs-lookup"><span data-stu-id="4be93-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="4be93-121">使用者必須在文字方塊中輸入一種色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="4be93-122">如果使用者想要的特製化的色彩-比方說，只是右陰影的尖峰綠-然後使用者必須找出 HTML 色彩代碼，而不需任何協助。</span><span class="sxs-lookup"><span data-stu-id="4be93-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="4be93-123">若要建立較佳使用者體驗，您可以使用 ColorPicker 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="4be93-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="4be93-124">當您將焦點移至 TextBox 控制項，ColorPicker 顯示色彩對話方塊 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="4be93-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="4be93-125">[![ColorPicker 控制項擴充項](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4be93-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="4be93-126">**圖 02**: ColorPicker 控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4be93-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="4be93-127">您需要完成 ColorPicker 控制項擴充項搭配列表 1 中的表單的兩個步驟：</span><span class="sxs-lookup"><span data-stu-id="4be93-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="4be93-128">ScriptManager 控制項加入頁面</span><span class="sxs-lookup"><span data-stu-id="4be93-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="4be93-129">ColorPicker 控制項擴充項新增至頁面</span><span class="sxs-lookup"><span data-stu-id="4be93-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="4be93-130">您可以使用 ColorPicker 之前，必須以您的頁面加入 ScriptManager。</span><span class="sxs-lookup"><span data-stu-id="4be93-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="4be93-131">開啟伺服器端權限低於將 ScriptManager 的好地方&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="4be93-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="4be93-132">您可以拖曳到頁面的 ScriptManager，從工具箱 （ScriptManager 位於 AJAX 延伸模組 索引標籤下）。</span><span class="sxs-lookup"><span data-stu-id="4be93-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="4be93-133">或者，您可以輸入下列標記原始碼檢視下開啟伺服器端表單標記：</span><span class="sxs-lookup"><span data-stu-id="4be93-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="4be93-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="4be93-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="4be93-135">ColorPicker 控制項擴充項新增至頁面的最簡單方式是在設計檢視中。</span><span class="sxs-lookup"><span data-stu-id="4be93-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="4be93-136">如果您將滑鼠停留 txtCardColor 文字方塊時，智慧工作選項，將會出現可讓您將新增 extender （請參閱 [圖 3]）。</span><span class="sxs-lookup"><span data-stu-id="4be93-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="4be93-137">如果您選擇此選項時，擴充性精靈] 隨即出現 （請參閱 [圖 4）。</span><span class="sxs-lookup"><span data-stu-id="4be93-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="4be93-138">[![新增 extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4be93-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="4be93-139">**圖 03**： 加入擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4be93-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="4be93-140">[![選取 使用 Extender 精靈控制項擴充項](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4be93-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="4be93-141">**圖 04**： 選取使用 Extender 精靈控制項擴充項 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4be93-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="4be93-142">您可以挑選 ColorPicker 擴充項，來擴充 txtCardColor 文字方塊與 ColorPicker 擴充項。</span><span class="sxs-lookup"><span data-stu-id="4be93-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="4be93-143">按一下 [確定] 以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="4be93-144">進行這些變更之後，來源頁面看起來像列表 2 中。</span><span class="sxs-lookup"><span data-stu-id="4be93-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="4be93-145">列表 2-CreateCard.aspx （含 ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="4be93-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="4be93-146">請注意頁面現在包含 ColorPickerExtender 控制項 txtCardColor TextBox 控制項的正下方顯示。</span><span class="sxs-lookup"><span data-stu-id="4be93-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="4be93-147">ColorPickerExtender 控制延伸 txtCardColor 控制項，讓它會顯示色彩選擇器對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="4be93-148">使用按鈕來啟動色彩選擇器對話方塊</span><span class="sxs-lookup"><span data-stu-id="4be93-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="4be93-149">ColorPicker extender 支援下列屬性：</span><span class="sxs-lookup"><span data-stu-id="4be93-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="4be93-150">可利用 PopupButtonId-會導致出現色彩選擇器對話方塊頁面上的按鈕的識別碼。</span><span class="sxs-lookup"><span data-stu-id="4be93-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="4be93-151">PopupPosition-的位置，相對於目標控制項的 [色彩選擇器] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="4be93-152">可能值為 Absolute、 中心、 BottomLeft、 BottomRight、 TopLeft、 右上方和左側 （預設為 BottomLeft）。</span><span class="sxs-lookup"><span data-stu-id="4be93-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="4be93-153">SampleControlId-顯示所選取之的色彩的控制項的 ID。</span><span class="sxs-lookup"><span data-stu-id="4be93-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="4be93-154">SelectedColor-ColorPicker 所選取的初始色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="4be93-155">您可以使用這些屬性來自訂顯示色彩選擇器對話方塊的方式，以及所選取之的色彩的顯示方式。</span><span class="sxs-lookup"><span data-stu-id="4be93-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="4be93-156">[列表 3] 頁面會說明您如何使用數個這些屬性。</span><span class="sxs-lookup"><span data-stu-id="4be93-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="4be93-157">**列表 3-CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="4be93-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="4be93-158">[列表 3] 頁面包含挑選色彩按鈕 （請參閱 [圖 5]）。</span><span class="sxs-lookup"><span data-stu-id="4be93-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="4be93-159">當您按一下此按鈕時，色彩選擇器對話方塊上方文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="4be93-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="4be93-160">如果您選取色彩對話方塊中選取的色彩就會顯示為 lblSample Label 控制項的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="4be93-161">ColorPicker 可利用 PopupButtonID 屬性用來關聯 ColorPicker 擴充項中的 [挑選顏色] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="4be93-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="4be93-162">當您提供可利用 PopupButtonID 屬性的值時，色彩選擇器對話方塊不會再出現目標控制項有焦點時。</span><span class="sxs-lookup"><span data-stu-id="4be93-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="4be93-163">您必須按一下按鈕以顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="4be93-164">SampleControlID 屬性用來將會顯示所選取的色彩與 ColorPicker 控制項產生關聯。</span><span class="sxs-lookup"><span data-stu-id="4be93-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="4be93-165">ColorPicker 會將此控制項的背景色彩變成目前選取的色彩。</span><span class="sxs-lookup"><span data-stu-id="4be93-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="4be93-166">[![顯示具有按鈕的色彩選擇器對話方塊](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4be93-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="4be93-167">**圖 05**： 顯示具有按鈕的色彩選擇器對話方塊 ([按一下以檢視完整大小的影像](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="4be93-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="4be93-168">總結</span><span class="sxs-lookup"><span data-stu-id="4be93-168">Summary</span></span>

<span data-ttu-id="4be93-169">在本教學課程中，您已了解如何使用 ColorPicker 控制項擴充項顯示快顯色彩選擇器對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="4be93-170">首先，我們會檢查焦點會移至 TextBox 控制項時，如何顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="4be93-171">接下來，您已了解如何建立按鈕，按一下按鈕時，會顯示 [色彩選擇器] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="4be93-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4be93-172">下一步</span><span class="sxs-lookup"><span data-stu-id="4be93-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
