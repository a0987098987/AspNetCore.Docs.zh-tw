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
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>如何使用 HTML 編輯器控制項？ (VB)
====================
by [Microsoft](https://github.com/microsoft)

> HTMLEditor 是 ASP.NET AJAX 控制項，可讓您輕鬆地建立和編輯 HTML 內容，透過在工具列中的按鈕。


本教學課程的目標是為您提供具有 AJAX Control Toolkit 包含的 HTML 編輯器控制項的概觀。 HTML 編輯器包括變更字型大小、 選取的字型、 變更背景色彩、 修改前景色彩，選項新增連結，加入影像，變更文字對齊方式，並執行剪下、 複製和貼上的作業 （請參閱圖 1）。


[![HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**圖 01**: HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


HTML 編輯器可讓您輸入使用設計模式的內容，或您可以直接輸入 HTML。 您也可以用預覽 HTML 內容的選項 （請參閱圖 2）。


[![設計、 HTML 和預覽按鈕](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**圖 02**： 設計、 HTML、 和 [預覽] 按鈕 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


在此教學課程中，您將學會如何顯示 HTML 編輯器、 如何自訂工具列按鈕出現在 HTML 編輯器中，以及如何避免跨網站指令碼攻擊。

## <a name="displaying-the-html-editor"></a>顯示 HTML 編輯器

您可以使用 HTML 編輯器中的 ASP.NET 網頁之前，您必須先加入 ScriptManager 控制項的頁面。 ScriptManager 控制項位於 Visual Studio/Visual Web Developer Express 工具箱中的 [AJAX 擴充功能] 索引標籤下方。

您應該將 ScriptManager 控制項放在頁面頂端的任何其他網頁上的控制項之前。 例如，您可以將它放立即之下開啟伺服器端&lt;表單&gt;標記。

HTML 編輯器控制項位於工具箱 中的 AJAX Control Toolkit 控制項的其餘部分。 它的名稱是編輯器控制項 （請參閱圖 3）。


[![HTML 編輯器控制項](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**圖 03**: HTML 編輯器控制項 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


您拖曳到頁面的 HTML 編輯器之後，您可以在屬性工作表設定其屬性。 例如，您通常想要設定寬度和高度屬性。 清單 1 包含 ASP.NET 網頁，其中包含 HTML 編輯器的來源。

**列出 1-SimpleEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

程式碼範例 1 中的頁面包含 HTML 編輯器控制項、 按鈕控制項，以及常值的控制項。 當您按一下按鈕時，內容的 HTML 編輯器中會出現在常值的控制項 （請參閱圖 4）。


[![送出表單使用 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**圖 04**： 提交表單與 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


HTML 編輯器內容屬性用來擷取輸入到 HTML 編輯器中的 HTML 內容。 請注意，此 HTML 內容可包含 JavaScript。 在下一步 區段中，我們會討論如何防止 JavaScript 插入式攻擊。

## <a name="customizing-the-html-editor-toolbar"></a>自訂 HTML 編輯器工具列

您可以自訂完全的按鈕會出現在編輯器中。 例如，您可能想要移除 [HTML] 索引標籤，以防止使用者在 HTML 編輯器中切換至 HTML 模式。 或者，您可能想要移除字型大小下拉式清單可防止使用者建立極大的文字在論壇中張貼的訊息 （請參閱圖 5）。


[![自訂的 HTML 編輯器](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**圖 05**： 的自訂 HTML 編輯器 ([按一下以檢視完整大小的影像](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


您可以衍生自基底的編輯器類別的新的 HTML 編輯器自訂工具列按鈕。 例如，列出 2 中的自訂編輯器只會包含粗體和斜體的工具列按鈕。 已移除所有其他工具列按鈕。 此外，[HTML] 索引標籤已經移除了編輯器底部 （但的設計和預覽索引標籤仍然會有）。

**Listing 2 - App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

您必須列出 2 中將類別加入您的應用程式\_程式碼資料夾，讓類別將會自動編譯。 如果應用程式\_程式碼資料夾不存在於您的網站，則您只可以將資料夾。

建立自訂編輯器之後，您可以將它加入 ASP.NET 網頁中的相同方式加入標準 HTML 編輯器中 （請參閱列出 3）。

**列出 3-ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>避免跨網站指令碼 (XSS) 攻擊

每當您接受使用者輸入，並重新該輸入顯示在您的網站，您可能會開啟您的網站以跨網站指令碼 (XSS) 攻擊。 理論上，為惡意駭客無法提交時輸入會重新顯示執行的 JavaScript 程式碼。 JavaScript 無法用於竊取使用者密碼或其他機密資訊。

一般來說，您可以對抗 XSS 攻擊 HTML 編碼使用者擷取顯示在網頁中之前的任何輸入。 不過，進行 HTML 編碼的 HTML 編輯器中的輸出會不只會編碼&lt;指令碼&gt;標記，它也會編碼所有的 HTML 標記。 換句話說，您會遺失所有的格式設定，例如字型、 字型大小和背景色彩。

如果您要收集機密資訊從您的使用者-例如密碼、 信用卡卡號和社會保險號碼-您應該不顯示您使用 HTML 編輯器中的使用者從擷取的未編碼內容。 您應該只有在您不會重新顯示 HTML 內容，或正在提交的 HTML 內容的情況下使用 HTML 編輯器，您信任的合作對象的網站。

例如，假設您要建立的部落格應用程式。 在此情況下，它有意義時要使用 HTML 編輯器中撰寫部落格文章。 唯一送出的部落格文章的人，假定，您可以信任您自己未送出惡意的 JavaScript。 不過，並不合理時要使用 HTML 編輯器可讓匿名使用者張貼評論。 您應該在使用者送出密碼等機密資訊的情況下特別小心。 可能需要的惡意使用者可以張貼在註解，其中包含右邊的 JavaScript 的竊取的密碼。

## <a name="summary"></a>總結

在本教學課程中，您已提供包含 AJAX Control Toolkit 中的 HTML 編輯器控制項的簡短概觀。 您已學習如何使用 HTML 編輯器，以接受來自使用者的豐富內容並提交到伺服器的內容。 我們也將討論您如何自訂顯示 HTML 編輯器的工具列按鈕。 最後，您學會了如何以避免跨網站指令碼處理攻擊來接受潛在惡意的輸入使用 HTML 編輯器時。

> [!div class="step-by-step"]
> [上一步](how-do-i-use-the-html-editor-control-cs.md)
