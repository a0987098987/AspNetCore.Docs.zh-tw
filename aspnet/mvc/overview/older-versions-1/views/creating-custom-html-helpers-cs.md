---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: 建立自訂的 HTML 協助程式 (C#) |Microsoft Docs
author: microsoft
description: 本教學課程的目標在於示範如何建立自訂的 HTML 協助程式，您可以使用 MVC 檢視中。 利用 HTML 協助程式...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839405"
---
<a name="creating-custom-html-helpers-c"></a>建立自訂的 HTML 協助程式 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> 本教學課程的目標在於示範如何建立自訂的 HTML 協助程式，您可以使用 MVC 檢視中。 利用 HTML 協助程式，您可以減少您必須執行才能建立標準的 HTML 網頁的 HTML 標記的 tedious 打字的量。


本教學課程的目標在於示範如何建立自訂的 HTML 協助程式，您可以使用 MVC 檢視中。 利用 HTML 協助程式，您可以減少您必須執行才能建立標準的 HTML 網頁的 HTML 標記的 tedious 打字的量。

在本教學課程的第一個部分中，我會說明一些現有 ASP.NET MVC framework 隨附的 HTML 協助程式。 接下來，我將說明兩種建立自訂的 HTML 協助程式方法： 我說明如何建立自訂的 HTML 協助程式，藉由建立靜態方法，並藉由建立擴充方法。

## <a name="understanding-html-helpers"></a>了解 HTML 協助程式

HTML 協助程式是只會傳回字串的方法。 字串可以代表任一種您想要的內容。 比方說，您可以使用 HTML Helper 來呈現標準的 HTML 標記，例如 HTML`<input>`和`<img>`標記。 您也可以使用 HTML Helper 來呈現更複雜的內容，例如索引標籤帶或資料庫資料的 HTML 資料表。

ASP.NET MVC 架構包括下列設定中 （這不是完整的清單） 的標準 HTML 協助程式：

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

例如，請考慮在列表 1 中的表單。 此表單會轉譯兩個標準的 HTML 協助程式 （請參閱 圖 1） 的協助。 這個表單用`Html.BeginForm()`和`Html.TextBox()`轉譯簡單的 HTML 表單的 Helper 方法。


[![頁面呈現與 HTML 協助程式](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**圖 01**： 頁面呈現與 HTML 協助程式 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-cs/_static/image3.png))


**列表 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Html.BeginForm() 協助程式方法用來建立的開頭和結尾的 HTML`<form>`標記。 請注意，`Html.BeginForm()`方法稱為在 using 陳述式。 使用陳述式可以確保`<form>`標記取得關閉使用結尾區塊。

如果您偏好，而不是建立 using 區塊中，您可以呼叫 Html.EndForm() 協助程式方法，以關閉`<form>`標記。 使用任何方法，來建立開啟和關閉`<form>`似乎最直覺的標記。

`Html.TextBox()` Helper 方法會在 列表 1 中用來呈現 HTML`<input>`標記。 如果您選取檢視原始檔瀏覽器中您會看到列表 2 中的 HTML 原始程式碼。 請注意來源包含標準的 HTML 標記。

> [!IMPORTANT]
> 請注意， `Html.TextBox()`HTML 協助程式以呈現`<%= %>`而不是標記`<% %>`標記。 如果您未包含等號，然後執行任何動作取得呈現在瀏覽器。

ASP.NET MVC 架構會包含較少的協助程式。 最有可能，您必須擴充 MVC 架構使用自訂 HTML 協助程式。 在本教學課程的其餘部分，您將了解建立自訂的 HTML 協助程式的兩個方法。

**列表 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>建立 HTML 協助程式搭配靜態方法

若要建立新的 HTML 協助程式的最簡單方式是建立靜態方法會傳回字串。 想像一下，比方說，您決定建立新的 HTML Helper 呈現 HTML`<label>`標記。 您也可以在 列表 2 中使用類別來呈現`<label>`。

**列表 2 – `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

沒有特別關於列表 2 中的類別。 `Label()`方法只會傳回字串。

在 列表 3 中修改過的 索引 檢視會使用`LabelHelper`呈現 HTML`<label>`標記。 請注意，此檢視包含`<%@ imports %>`匯入的指示詞`Application1.Helpers`命名空間。

**列表 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>使用擴充方法建立 HTML 協助程式

如果您想要建立只是工作的 HTML 協助程式，例如標準包含 ASP.NET MVC 架構中，則您需要建立擴充方法的 HTML 協助程式。 擴充方法可讓您將新方法新增至現有的類別。 在建立時的 HTML Helper 方法，您會將新方法加入至檢視的 Html 屬性所表示的 HtmlHelper 類別中。

列表 3 中的類別加入至擴充方法`HtmlHelper`名為類別`Label()`。 有幾個您應該注意到關於此類別的項目。 首先，請注意，類別的靜態類別。 您必須定義靜態類別的延伸方法。

其次，請注意，第一個參數`Label()`方法的前面是關鍵字`this`。 擴充方法的第一個參數指出的擴充方法要擴充的類別。

**列表 3 – `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

擴充方法建立擴充方法，並成功建置您的應用程式之後，會出現在 Visual Studio Intellisense，如同所有其他方法的類別 （請參閱 圖 2）。 唯一的差別是該方法會出現特殊符號旁邊 （向下箭號圖示） 的延伸模組。


[![使用 Html.Label() 擴充方法](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**圖 02**： 使用 Html.Label() 擴充方法 ([按一下以檢視完整大小的影像](creating-custom-html-helpers-cs/_static/image6.png))


在 列表 4 中已修改的 索引 檢視使用 Html.Label(); 擴充方法來呈現所有其`<label>`標記。

**列表 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>總結

在本教學課程中，您已了解建立自訂的 HTML 協助程式的兩個方法。 首先，您已了解如何建立自訂`Label()`藉由建立靜態方法的 HTML 協助程式傳回的字串。 接下來，您已了解如何建立自訂`Label()`上建立擴充方法的 HTML Helper 方法`HtmlHelper`類別。

在本教學課程中，我會著重於建置非常簡單的 HTML Helper 方法。 請注意，HTML 協助程式可以是任意的複雜。 您可以建置 HTML 協助程式呈現豐富的內容，例如樹狀檢視、 功能表或資料庫資料的資料表。

> [!div class="step-by-step"]
> [上一頁](asp-net-mvc-views-overview-cs.md)
> [下一頁](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
