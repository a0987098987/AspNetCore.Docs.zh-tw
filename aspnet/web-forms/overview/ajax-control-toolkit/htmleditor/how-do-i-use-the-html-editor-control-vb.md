---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: 如何使用 HTML 編輯器控制項？ (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor 是 ASP.NET AJAX 控制項，可讓您輕鬆地建立和編輯工具列上按鈕的 HTML 內容。
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 11d9251644f1daf4257e1bfa3c9405fc0c46a5d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831438"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>如何使用 HTML 編輯器控制項？ (VB)
====================
by [Microsoft](https://github.com/microsoft)

> HTMLEditor 是 ASP.NET AJAX 控制項，可讓您輕鬆地建立和編輯工具列上按鈕的 HTML 內容。


本教學課程的目標是為您提供包含使用 AJAX Control Toolkit 的 HTML 編輯器控制項的概觀。 HTML 編輯器包括變更字型大小、 選取字型、 變更背景色彩、 修改前景色彩，選項新增連結，加入影像，變更文字對齊方式，並執行剪下、 複製和貼上 （請參閱 圖 1） 的作業。


[![HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**圖 01**: HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


HTML 編輯器可讓您輸入使用的設計模式的內容，或者您可以直接輸入 HTML。 系統也會提供選項，即可預覽您的 HTML 內容 （請參閱 圖 2）。


[![設計、 HTML 和預覽按鈕](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**圖 02**： 設計、 HTML 和預覽按鈕 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


在本教學課程中，您將學習如何顯示 HTML 編輯器、 如何自訂工具列按鈕出現在 HTML 編輯器中，以及如何避免跨網站指令碼攻擊。

## <a name="displaying-the-html-editor"></a>顯示 HTML 編輯器

您可以使用 HTML 編輯器中的 ASP.NET 頁面之前，您必須先將 ScriptManager 控制項加入頁面中。 ScriptManager 控制項位於 Visual Studio/Visual Web Developer Express 工具箱中的 [AJAX 延伸模組] 索引標籤底下。

您應該先在頁面上的任何其他控制項在頁面頂端將 ScriptManager 控制項。 例如，您可以將它放立即之下開啟伺服器端&lt;表單&gt;標記。

HTML 編輯器控制項位於工具箱 中的 AJAX Control Toolkit 控制項的其餘部分。 它會命名為編輯器控制項 （請參閱 [圖 3]）。


[![HTML 編輯器控制項](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**圖 03**: HTML 編輯器控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


您拖曳到頁面的 HTML 編輯器之後，您可以設定其屬性，屬性工作表中。 例如，您通常想要設定寬度和高度屬性。 列表 1 包含 ASP.NET 網頁，其中包含 HTML 編輯器的來源。

**列表 1-SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

[列表 1] 頁面包含 HTML 編輯器控制項、 按鈕控制項和常值的控制項。 當您按一下按鈕時，內容的 HTML 編輯器中會出現在常值控制項 （請參閱 圖 4）。


[![提交表單使用 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**圖 04**： 提交表單使用 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


HTML 編輯器內容屬性用來擷取輸入到 [HTML 編輯器] 中的 HTML 內容。 請注意，此 HTML 內容可以包含 JavaScript。 在下一步 區段中，我們會討論如何防止 JavaScript 插入式攻擊。

## <a name="customizing-the-html-editor-toolbar"></a>自訂 HTML 編輯器工具列

您可以自訂完全的按鈕會出現在編輯器中。 例如，您可能想要移除以防止使用者切換至 HTML 模式的 HTML 編輯器的 [HTML] 索引標籤。 或者，您可能想要移除字型的大小下拉式清單，以防止使用者在論壇中建立極大的文字張貼訊息 （請參閱 [圖 5]）。


[![自訂的 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**圖 05**： 的自訂 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


您可以衍生自基底的編輯器類別的新的 HTML 編輯器自訂工具列按鈕。 例如，列表 2 中的自訂編輯器只會包含粗體和斜體的工具列按鈕。 已移除其他所有的工具列按鈕。 此外，[HTML] 索引標籤已經移除了編輯器的底部 （但仍然會有的設計和預覽索引標籤）。

**列表 2-應用程式\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

您必須在 列表 2 中將類別加入您的應用程式\_，因此會自動編譯類別程式碼的資料夾。 如果應用程式\_程式碼資料夾不存在於您的網站，則您可以直接將該資料夾。

建立自訂編輯器之後，您可以將它新增至 ASP.NET 頁面相同的方式新增標準的 HTML 編輯器 （請參閱列表 3）。

**列表 3-ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>避免跨網站指令碼 (XSS) 攻擊

只要接受來自使用者的輸入，並且重新該輸入顯示在您的網站，可能會開啟您的網站以跨網站指令碼 (XSS) 攻擊。 理論上，為惡意駭客無法提交時輸入會重新顯示時，取得執行的 JavaScript 程式碼。 JavaScript 可用於竊取使用者的密碼或其他機密資訊。

一般來說，您可以打敗 HTML 編碼您再顯示在網頁上，從使用者擷取任何輸入的 XSS 的攻擊。 不過，進行 HTML 編碼的 HTML 編輯器中的輸出會不只編碼&lt;指令碼&gt;標記，它也會編碼所有的 HTML 標記。 換句話說，您會遺失所有的格式設定，例如字型、 字型大小和背景色彩。

如果您要收集機密資訊從您的使用者，例如密碼、 信用卡號碼和身分證號碼-您應該不顯示您的使用者在 HTML 編輯器具有從擷取的未編碼內容。 您應該只在您不選擇 HTML 內容，或正在提交的 HTML 內容的情況下使用 HTML 編輯器，您受信任的合作對象的網站。

例如，假設您要建立的部落格應用程式。 在此情況下，合理時要使用 HTML 編輯器撰寫部落格文章。 您唯一一個提交的部落格文章的人，而且您能夠信任自己，不以提交惡意的 JavaScript。 不過，它不會毫無意義時要使用 HTML 編輯器可讓匿名使用者張貼註解。 您應該在使用者送出密碼等機密資訊的情況下特別小心。 甚至是惡意使用者可以張貼在註解，其中包含正確的 JavaScript 竊取的密碼。

## <a name="summary"></a>總結

在本教學課程中，提供給您的 HTML 編輯器控制項包含在 AJAX Control Toolkit 中的簡短概觀。 您已了解如何使用 HTML 編輯器，接受來自使用者的豐富內容，並提交到伺服器的內容。 我們也會討論如何自訂工具列按鈕所顯示的 HTML 編輯器中。 最後，您已了解如何避免跨網站指令攻擊時使用 HTML 編輯器，以接受潛在的惡意輸入。

> [!div class="step-by-step"]
> [上一步](how-do-i-use-the-html-editor-control-cs.md)
