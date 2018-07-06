---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: 執行簡單的驗證 (C#) |Microsoft Docs
author: StephenWalther
description: 了解如何在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，Stephen Walther 會介紹您至模型狀態，並驗證 HTML 協助程式...
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 2a8f21a3950eb7793fd76458799ab4f6a6b5fa09
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816466"
---
<a name="performing-simple-validation-c"></a>執行簡單的驗證 (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 了解如何在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，Stephen Walther 會介紹您模型狀態，以及驗證 HTML 協助程式。


本教學課程的目標是要說明如何執行驗證的 ASP.NET MVC 應用程式內。 比方說，您會了解如何防止有人提交表單不包含必要的欄位的值。 您了解如何使用模型的狀態和驗證 HTML 協助程式。

## <a name="understanding-model-state"></a>了解模型狀態

您可以使用模型狀態-或更精確地說，位於模型狀態字典中-來表示驗證錯誤。 比方說，列表 1 中的 create （） 動作會加入資料庫中的 Product 類別之前驗證產品類別的屬性。


我不建議將您的資料庫或驗證邏輯加入至控制器。 控制器應該包含應用程式流程控制相關的邏輯。 我們正在採取捷徑，為了簡單起見。


**列表 1-Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

在 [列表 1] 的產品類別的名稱、 描述和 UnitsInStock 屬性會進行驗證。 如果其中任何一個屬性的驗證測試失敗的錯誤會加入 （控制器類別的 ModelState 屬性所表示） 的模型狀態字典。

如果模型狀態中有任何錯誤則 ModelState.IsValid 屬性會傳回 false。 在此情況下，會重新顯示的 HTML 表單建立新的產品。 否則，如果不有任何驗證錯誤，新的產品被加入至資料庫。

## <a name="using-the-validation-helpers"></a>使用驗證協助程式

ASP.NET MVC 架構包括兩個驗證協助程式： Html.ValidationMessage() helper 和 Html.ValidationSummary() 協助程式。 您可以在檢視中，使用這些兩個協助程式來顯示驗證錯誤訊息。

Html.ValidationMessage() 和 Html.ValidationSummary() 的協助程式由 ASP.NET MVC scaffolding 自動產生的建立與編輯檢視中使用。 請遵循下列步驟來產生建立檢視：

1. 以滑鼠右鍵按一下 create （） 動作，在 Product 控制器，然後選取功能表選項**加入檢視**（請參閱 圖 1）。
2. 在**加入檢視**] 對話方塊中，選取標示的核取方塊**建立強型別檢視**（請參閱 [圖 2）。
3. 從**檢視資料類別**下拉式清單中選取產品類別。
4. 從**檢視內容**下拉式清單中，選取建立。
5. 按一下 [新增] 按鈕。


請確定您建置應用程式，再加入的檢視。 否則，清單中的類別不會出現在**檢視資料類別**下拉式清單中。


[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**圖 01**： 新增檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image2.png))


[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**圖 02**： 建立強型別檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image4.png))


完成這些步驟之後，您會取得列表 2 中建立檢視。

**列表 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

在列表 2 中，Html.ValidationSummary() helper 稱為立即上方的 HTML 表單。 此協助程式用來顯示驗證錯誤訊息的清單。 Html.ValidationSummary() helper 會呈現項目符號清單中的錯誤。

Html.ValidationMessage() 協助程式會呼叫每個 HTML 表單欄位旁邊。 此協助程式用來顯示錯誤訊息的表單欄位的旁邊。 在列表 2 的情況下 Html.ValidationMessage() 協助程式會顯示星號，當發生錯誤。

[圖 3] 頁面會說明遺漏的欄位與無效的值送出表單時，驗證協助程式所呈現的錯誤訊息。


[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**圖 03**: 建立檢視提交問題 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image6.png))


請注意外觀的 HTML 輸入驗證錯誤時，也會修改欄位。 Html.TextBox() helper 轉譯*類別 ="輸入的驗證錯誤的 「* Html.TextBox() 協助程式所呈現屬性相關聯的驗證錯誤時的屬性。

有三個階層式的樣式表類別用來控制驗證錯誤的外觀：

- 輸入驗證錯誤-套用至&lt;輸入&gt;Html.TextBox() 協助程式所呈現的標記。
- 欄位驗證錯誤-套用至&lt;跨越&gt;Html.ValidationMessage() 協助程式所呈現的標記。
- 驗證摘要錯誤-套用至&lt;ul&gt; Html.ValidationSumamry() 協助程式所呈現的標記。

您可以修改這些階層式樣式表類別，並因此修改藉由修改 Site.css 檔案內容的資料夾中的 驗證錯誤的外觀。

> [!NOTE] 
> 
> HtmlHelper 類別包含唯讀的靜態屬性，如需擷取驗證的名稱相關的 CSS 類別。 這些靜態屬性會命名為 ValidationInputCssClassName、 ValidationFieldCssClassName 和 ValidationSummaryCssClassName。


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding 驗證和 Postbinding 驗證

如果您送出的 HTML 表單，建立一項產品，且 [價格] 欄位和 [UnitsInStock] 欄位沒有值輸入了無效的值時，您就會得到 [圖 4] 中顯示的驗證訊息。 這些驗證錯誤訊息來自何處？


[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**圖 04**: Prebinding 驗證錯誤 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image8.png))


有實際上有兩種類型的驗證錯誤訊息-產生之前的 HTML 表單欄位，繫結至類別和表單欄位繫結至類別之後，所產生。 換句話說，有 prebinding 驗證錯誤和 postbinding 驗證錯誤。

產品中的控制站列表 1 所公開的 create （） 動作接受產品類別的執行個體。 建立方法的簽章看起來像這樣：

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

建立表單的 HTML 表單欄位的值會繫結至 productToCreate 類別，所謂的模型繫結器。 預設模型繫結器錯誤訊息至模型狀態會自動新增當它無法將表單欄位繫結至表單的屬性。

預設模型繫結器無法將字串"apple"繫結至 Product 類別中的 [價格] 屬性。 您無法將字串指派給十進位屬性。 因此，模型繫結會將錯誤加入至模型狀態。

預設模型繫結器也無法將 null 值指派給不接受 null 值的屬性。 特別是，模型繫結器無法將 null 值指派給 [UnitsInStock] 屬性。 同樣地，模型繫結會放棄，並將錯誤訊息加入至模型狀態。

如果您想要自訂這些外觀 prebinding 錯誤訊息您需要建立這些訊息的資源字串。

## <a name="summary"></a>總結

本教學課程的目標是驗證的要說明基本的 ASP.NET MVC 架構中機制。 您已了解如何使用模型狀態和驗證 HTML 協助程式。 我們也會討論 prebinding 和 postbinding 驗證之間的差異。 在其他教學課程中，我們將討論各種策略可用來移動驗證程式碼出您的控制器並進入您的模型類別。

> [!div class="step-by-step"]
> [上一頁](displaying-a-table-of-database-data-cs.md)
> [下一頁](validating-with-the-idataerrorinfo-interface-cs.md)
