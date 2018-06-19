---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: 如何使用 HTML 編輯器控制項？ (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor 是 ASP.NET AJAX 控制項，可讓您輕鬆地建立和編輯 HTML 內容，透過在工具列中的按鈕。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873296"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a><span data-ttu-id="d355b-104">如何使用 HTML 編輯器控制項？</span><span class="sxs-lookup"><span data-stu-id="d355b-104">How do I use the HTML Editor Control?</span></span> <span data-ttu-id="d355b-105">(VB)</span><span class="sxs-lookup"><span data-stu-id="d355b-105">(VB)</span></span>
====================
<span data-ttu-id="d355b-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d355b-106">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d355b-107">HTMLEditor 是 ASP.NET AJAX 控制項，可讓您輕鬆地建立和編輯 HTML 內容，透過在工具列中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="d355b-107">HTMLEditor is an ASP.NET AJAX Control that allows you to easily create and edit HTML content via buttons in a toolbar.</span></span>


<span data-ttu-id="d355b-108">本教學課程的目標是為您提供具有 AJAX Control Toolkit 包含的 HTML 編輯器控制項的概觀。</span><span class="sxs-lookup"><span data-stu-id="d355b-108">The goal of this tutorial is to provide you with an overview of the HTML Editor control included with the AJAX Control Toolkit.</span></span> <span data-ttu-id="d355b-109">HTML 編輯器包括變更字型大小、 選取的字型、 變更背景色彩、 修改前景色彩，選項新增連結，加入影像，變更文字對齊方式，並執行剪下、 複製和貼上的作業 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="d355b-109">The HTML Editor includes options for changing font size, selecting a font, changing background color, modifying the foreground color, adding links, adding images, changing text alignment, and performing cut, copy, and paste operations (see Figure 1).</span></span>


<span data-ttu-id="d355b-110">[![HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d355b-110">[![The HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="d355b-111">**圖 01**: HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="d355b-111">**Figure 01**: The HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image2.png))</span></span>


<span data-ttu-id="d355b-112">HTML 編輯器可讓您輸入使用設計模式的內容，或您可以直接輸入 HTML。</span><span class="sxs-lookup"><span data-stu-id="d355b-112">The HTML editor enables you to enter content using a design mode or you can enter HTML directly.</span></span> <span data-ttu-id="d355b-113">您也可以用預覽 HTML 內容的選項 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="d355b-113">You also are provided with the option to preview your HTML content (see Figure 2).</span></span>


<span data-ttu-id="d355b-114">[![設計、 HTML 和預覽按鈕](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d355b-114">[![Design, HTML, and Preview buttons](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)</span></span>

<span data-ttu-id="d355b-115">**圖 02**： 設計、 HTML、 和 [預覽] 按鈕 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="d355b-115">**Figure 02**: Design, HTML, and Preview buttons([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image4.png))</span></span>


<span data-ttu-id="d355b-116">在此教學課程中，您將學會如何顯示 HTML 編輯器、 如何自訂工具列按鈕出現在 HTML 編輯器中，以及如何避免跨網站指令碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="d355b-116">In this tutorial, you learn how to display the HTML Editor, how to customize the toolbar buttons that appear in the HTML Editor, and how to avoid Cross-Site Scripting Attacks.</span></span>

## <a name="displaying-the-html-editor"></a><span data-ttu-id="d355b-117">顯示 HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="d355b-117">Displaying the HTML Editor</span></span>

<span data-ttu-id="d355b-118">您可以使用 HTML 編輯器中的 ASP.NET 網頁之前，您必須先加入 ScriptManager 控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="d355b-118">Before you can use the HTML Editor in an ASP.NET page, you must first add a ScriptManager control to the page.</span></span> <span data-ttu-id="d355b-119">ScriptManager 控制項位於 Visual Studio/Visual Web Developer Express 工具箱中的 [AJAX 擴充功能] 索引標籤下方。</span><span class="sxs-lookup"><span data-stu-id="d355b-119">The ScriptManager control is located beneath the AJAX Extensions tab in the Visual Studio/Visual Web Developer Express toolbox.</span></span>

<span data-ttu-id="d355b-120">您應該將 ScriptManager 控制項放在頁面頂端的任何其他網頁上的控制項之前。</span><span class="sxs-lookup"><span data-stu-id="d355b-120">You should place the ScriptManager control at the top of the page before any other controls on the page.</span></span> <span data-ttu-id="d355b-121">例如，您可以將它放立即之下開啟伺服器端&lt;表單&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="d355b-121">For example, you can place it immediately below the opening server-side &lt;form&gt; tag.</span></span>

<span data-ttu-id="d355b-122">HTML 編輯器控制項位於工具箱 中的 AJAX Control Toolkit 控制項的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="d355b-122">The HTML Editor control is located in the toolbox with the rest of the AJAX Control Toolkit controls.</span></span> <span data-ttu-id="d355b-123">它的名稱是編輯器控制項 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="d355b-123">It is named the Editor control (see Figure 3).</span></span>


<span data-ttu-id="d355b-124">[![HTML 編輯器控制項](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d355b-124">[![The HTML Editor control](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)</span></span>

<span data-ttu-id="d355b-125">**圖 03**: HTML 編輯器控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d355b-125">**Figure 03**: The HTML Editor control([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image6.png))</span></span>


<span data-ttu-id="d355b-126">您拖曳到頁面的 HTML 編輯器之後，您可以在屬性工作表設定其屬性。</span><span class="sxs-lookup"><span data-stu-id="d355b-126">After you drag the HTML Editor onto a page, you can set its properties in the property sheet.</span></span> <span data-ttu-id="d355b-127">例如，您通常想要設定寬度和高度屬性。</span><span class="sxs-lookup"><span data-stu-id="d355b-127">For example, you normally want to set the Width and Height properties.</span></span> <span data-ttu-id="d355b-128">清單 1 包含 ASP.NET 網頁，其中包含 HTML 編輯器的來源。</span><span class="sxs-lookup"><span data-stu-id="d355b-128">Listing 1 contains the source for an ASP.NET page that contains an HTML editor.</span></span>

<span data-ttu-id="d355b-129">**列出 1-SimpleEditor.aspx**</span><span class="sxs-lookup"><span data-stu-id="d355b-129">**Listing 1 - SimpleEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

<span data-ttu-id="d355b-130">程式碼範例 1 中的頁面包含 HTML 編輯器控制項、 按鈕控制項，以及常值的控制項。</span><span class="sxs-lookup"><span data-stu-id="d355b-130">The page in Listing 1 contains an HTML Editor control, a Button control, and a Literal control.</span></span> <span data-ttu-id="d355b-131">當您按一下按鈕時，內容的 HTML 編輯器中會出現在常值的控制項 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="d355b-131">When you click the button, the contents of the HTML Editor appear in the Literal control (see Figure 4).</span></span>


<span data-ttu-id="d355b-132">[![送出表單使用 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d355b-132">[![Submitting a form with an HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)</span></span>

<span data-ttu-id="d355b-133">**圖 04**： 提交表單與 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="d355b-133">**Figure 04**: Submitting a form with an HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image8.png))</span></span>


<span data-ttu-id="d355b-134">HTML 編輯器內容屬性用來擷取輸入到 HTML 編輯器中的 HTML 內容。</span><span class="sxs-lookup"><span data-stu-id="d355b-134">The HTML Editor Content property is used to retrieve the HTML content entered into the HTML Editor.</span></span> <span data-ttu-id="d355b-135">請注意，此 HTML 內容可包含 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="d355b-135">Be aware that this HTML content can contain JavaScript.</span></span> <span data-ttu-id="d355b-136">在下一步 區段中，我們會討論如何防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="d355b-136">In the next section, we discuss how you can prevent JavaScript Injection Attacks.</span></span>

## <a name="customizing-the-html-editor-toolbar"></a><span data-ttu-id="d355b-137">自訂 HTML 編輯器工具列</span><span class="sxs-lookup"><span data-stu-id="d355b-137">Customizing the HTML Editor Toolbar</span></span>

<span data-ttu-id="d355b-138">您可以自訂完全的按鈕會出現在編輯器中。</span><span class="sxs-lookup"><span data-stu-id="d355b-138">You can customize exactly which buttons appear in the editor.</span></span> <span data-ttu-id="d355b-139">例如，您可能想要移除 [HTML] 索引標籤，以防止使用者在 HTML 編輯器中切換至 HTML 模式。</span><span class="sxs-lookup"><span data-stu-id="d355b-139">For example, you might want to remove the HTML tab to prevent users from switching the HTML Editor into HTML mode.</span></span> <span data-ttu-id="d355b-140">或者，您可能想要移除字型大小下拉式清單可防止使用者建立極大的文字在論壇中張貼的訊息 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="d355b-140">Or, you might want to remove the font size dropdown list to prevent users from creating overly large text in a forum message post (see Figure 5).</span></span>


<span data-ttu-id="d355b-141">[![自訂的 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d355b-141">[![A customized HTML Editor](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)</span></span>

<span data-ttu-id="d355b-142">**圖 05**： 的自訂 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="d355b-142">**Figure 05**: A customized HTML Editor([Click to view full-size image](how-do-i-use-the-html-editor-control-vb/_static/image10.png))</span></span>


<span data-ttu-id="d355b-143">您可以衍生自基底的編輯器類別的新的 HTML 編輯器自訂工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="d355b-143">You customize the toolbar buttons by deriving a new HTML Editor from the base Editor class.</span></span> <span data-ttu-id="d355b-144">例如，列出 2 中的自訂編輯器只會包含粗體和斜體的工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="d355b-144">For example, the custom editor in Listing 2 only contains toolbar buttons for bold and italic.</span></span> <span data-ttu-id="d355b-145">已移除所有其他工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="d355b-145">All other toolbar buttons have been removed.</span></span> <span data-ttu-id="d355b-146">此外，[HTML] 索引標籤已經移除了編輯器底部 （但的設計和預覽索引標籤仍然會有）。</span><span class="sxs-lookup"><span data-stu-id="d355b-146">Furthermore, the HTML tab has been removed from the bottom of the editor (but the Design and Preview tabs are still there).</span></span>

<span data-ttu-id="d355b-147">**Listing 2 - App\_Code\CustomEditor.vb**</span><span class="sxs-lookup"><span data-stu-id="d355b-147">**Listing 2 - App\_Code\CustomEditor.vb**</span></span>

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

<span data-ttu-id="d355b-148">您必須列出 2 中將類別加入您的應用程式\_程式碼資料夾，讓類別將會自動編譯。</span><span class="sxs-lookup"><span data-stu-id="d355b-148">You must add the class in Listing 2 to your App\_Code folder so that the class will be compiled automatically.</span></span> <span data-ttu-id="d355b-149">如果應用程式\_程式碼資料夾不存在於您的網站，則您只可以將資料夾。</span><span class="sxs-lookup"><span data-stu-id="d355b-149">If the App\_Code folder does not exist in your website then you can simply add the folder.</span></span>

<span data-ttu-id="d355b-150">建立自訂編輯器之後，您可以將它加入 ASP.NET 網頁中的相同方式加入標準 HTML 編輯器中 （請參閱列出 3）。</span><span class="sxs-lookup"><span data-stu-id="d355b-150">After you create a custom editor, you can add it to an ASP.NET page in the same way as you add the normal HTML Editor (see Listing 3).</span></span>

<span data-ttu-id="d355b-151">**列出 3-ShowCustomEditor.aspx**</span><span class="sxs-lookup"><span data-stu-id="d355b-151">**Listing 3 - ShowCustomEditor.aspx**</span></span>

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a><span data-ttu-id="d355b-152">避免跨網站指令碼 (XSS) 攻擊</span><span class="sxs-lookup"><span data-stu-id="d355b-152">Avoiding Cross-Site Scripting (XSS) Attacks</span></span>

<span data-ttu-id="d355b-153">每當您接受使用者輸入，並重新該輸入顯示在您的網站，您可能會開啟您的網站以跨網站指令碼 (XSS) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="d355b-153">Whenever you accept input from a user, and redisplay that input on your website, you potentially open your website to Cross-Site Scripting (XSS) Attacks.</span></span> <span data-ttu-id="d355b-154">理論上，為惡意駭客無法提交時輸入會重新顯示執行的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="d355b-154">In theory, a malicious hacker could submit JavaScript code that gets executed when the input is redisplayed.</span></span> <span data-ttu-id="d355b-155">JavaScript 無法用於竊取使用者密碼或其他機密資訊。</span><span class="sxs-lookup"><span data-stu-id="d355b-155">The JavaScript could be used to steal user passwords or other sensitive information.</span></span>

<span data-ttu-id="d355b-156">一般來說，您可以對抗 XSS 攻擊 HTML 編碼使用者擷取顯示在網頁中之前的任何輸入。</span><span class="sxs-lookup"><span data-stu-id="d355b-156">Normally, you can defeat XSS attacks by HTML encoding whatever input you retrieve from a user before displaying it in a web page.</span></span> <span data-ttu-id="d355b-157">不過，進行 HTML 編碼的 HTML 編輯器中的輸出會不只會編碼&lt;指令碼&gt;標記，它也會編碼所有的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="d355b-157">However, HTML encoding the output of the HTML Editor would not only encode &lt;script&gt; tags, it would also encode all HTML tags.</span></span> <span data-ttu-id="d355b-158">換句話說，您會遺失所有的格式設定，例如字型、 字型大小和背景色彩。</span><span class="sxs-lookup"><span data-stu-id="d355b-158">In other words, you would lose all of the formatting such as the font type, font size, and background color.</span></span>

<span data-ttu-id="d355b-159">如果您要收集機密資訊從您的使用者-例如密碼、 信用卡卡號和社會保險號碼-您應該不顯示您使用 HTML 編輯器中的使用者從擷取的未編碼內容。</span><span class="sxs-lookup"><span data-stu-id="d355b-159">If you are collecting sensitive information from your users -- such as passwords, credit-card numbers, and social security numbers - then you should not display un-encoded content that you retrieve from a user with the HTML Editor.</span></span> <span data-ttu-id="d355b-160">您應該只有在您不會重新顯示 HTML 內容，或正在提交的 HTML 內容的情況下使用 HTML 編輯器，您信任的合作對象的網站。</span><span class="sxs-lookup"><span data-stu-id="d355b-160">You should use the HTML Editor only in situations in which you are not redisplaying the HTML content, or the HTML content is being submitted to your website by a trusted party.</span></span>

<span data-ttu-id="d355b-161">例如，假設您要建立的部落格應用程式。</span><span class="sxs-lookup"><span data-stu-id="d355b-161">Imagine, for example, that you are creating a blog application.</span></span> <span data-ttu-id="d355b-162">在此情況下，它有意義時要使用 HTML 編輯器中撰寫部落格文章。</span><span class="sxs-lookup"><span data-stu-id="d355b-162">In this situation, it makes sense to use the HTML Editor when composing blog posts.</span></span> <span data-ttu-id="d355b-163">唯一送出的部落格文章的人，假定，您可以信任您自己未送出惡意的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="d355b-163">You are the only one who submits a blog post and, presumably, you can trust yourself not to submit malicious JavaScript.</span></span> <span data-ttu-id="d355b-164">不過，並不合理時要使用 HTML 編輯器可讓匿名使用者張貼評論。</span><span class="sxs-lookup"><span data-stu-id="d355b-164">However, it does not make sense to use the HTML Editor when allowing anonymous users to post comments.</span></span> <span data-ttu-id="d355b-165">您應該在使用者送出密碼等機密資訊的情況下特別小心。</span><span class="sxs-lookup"><span data-stu-id="d355b-165">You should be especially careful in situations in which users submit sensitive information such as passwords.</span></span> <span data-ttu-id="d355b-166">可能需要的惡意使用者可以張貼在註解，其中包含右邊的 JavaScript 的竊取的密碼。</span><span class="sxs-lookup"><span data-stu-id="d355b-166">Potentially, a malicious user could post a comment that contains the right JavaScript for stealing a password.</span></span>

## <a name="summary"></a><span data-ttu-id="d355b-167">總結</span><span class="sxs-lookup"><span data-stu-id="d355b-167">Summary</span></span>

<span data-ttu-id="d355b-168">在本教學課程中，您已提供包含 AJAX Control Toolkit 中的 HTML 編輯器控制項的簡短概觀。</span><span class="sxs-lookup"><span data-stu-id="d355b-168">In this tutorial, you were provided with a brief overview of the HTML Editor control included in the AJAX Control Toolkit.</span></span> <span data-ttu-id="d355b-169">您已學習如何使用 HTML 編輯器，以接受來自使用者的豐富內容並提交到伺服器的內容。</span><span class="sxs-lookup"><span data-stu-id="d355b-169">You learned how to use the HTML Editor to accept rich content from a user and submit the content to the server.</span></span> <span data-ttu-id="d355b-170">我們也將討論您如何自訂顯示 HTML 編輯器的工具列按鈕。</span><span class="sxs-lookup"><span data-stu-id="d355b-170">We also discussed how you can customize the toolbar buttons that are displayed by the HTML Editor.</span></span> <span data-ttu-id="d355b-171">最後，您學會了如何以避免跨網站指令碼處理攻擊來接受潛在惡意的輸入使用 HTML 編輯器時。</span><span class="sxs-lookup"><span data-stu-id="d355b-171">Finally, you learned how to avoid Cross-Site Scripting Attacks when using the HTML Editor to accept potentially malicious input.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d355b-172">上一步</span><span class="sxs-lookup"><span data-stu-id="d355b-172">Previous</span></span>](how-do-i-use-the-html-editor-control-cs.md)
