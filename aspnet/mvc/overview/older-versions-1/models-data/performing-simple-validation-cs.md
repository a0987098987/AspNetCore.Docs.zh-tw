---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: 執行簡單的驗證 (C#) |Microsoft 文件
author: StephenWalther
description: 了解如何在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，作者： Stephen Walther 導入您模型狀態，以及驗證 HTML helper...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fc1dcc6935841382215f67a519cd241ac68931a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869656"
---
<a name="performing-simple-validation-c"></a>執行簡單的驗證 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 了解如何在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，作者： Stephen Walther 介紹您模型狀態，以及驗證 HTML helper。


本教學課程的目標是以說明如何執行 ASP.NET MVC 應用程式中的驗證。 比方說，您會了解如何防止某人送出表單不包含必要的欄位的值。 您了解如何使用模型的狀態和驗證 HTML helper。

## <a name="understanding-model-state"></a>了解模型的狀態

您使用模型狀態-或更精確地說，位於模型狀態字典中-來表示驗證錯誤。 比方說，create （） 中的動作程式碼範例 1 會先將加入資料庫中的產品類別中，驗證產品類別的屬性。


我不建議將您的資料庫或驗證邏輯加入至控制器。 控制器應該包含應用程式流程控制相關的邏輯。 我們會執行簡單的捷徑。


**Listing 1 - Controllers\ProductController.cs**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

在清單 1 中，產品類別的名稱、 描述和 UnitsInStock 屬性會進行驗證。 如果任一這些屬性無法通過驗證測試錯誤將加入模型狀態字典 （控制器類別的 ModelState 屬性所表示）。

如果模型狀態中有任何錯誤 ModelState.IsValid 屬性傳回 false。 在此情況下，會重新顯示的 HTML 表單建立新的產品。 否則，如果有任何驗證錯誤，新的產品加入至資料庫。

## <a name="using-the-validation-helpers"></a>使用驗證 Helper

ASP.NET MVC 架構包括兩個驗證 helper: Html.ValidationMessage() helper 和 Html.ValidationSummary() 協助程式。 您可以在檢視中使用這些兩個 helper 顯示驗證錯誤訊息。

由 ASP.NET MVC scaffolding 自動產生的建立與編輯檢視中，會使用 Html.ValidationMessage() 和 Html.ValidationSummary() helper。 請遵循這些步驟來產生建立檢視：

1. 以滑鼠右鍵按一下 create （） 中的動作 Product 控制器，然後選取功能表選項**加入檢視**（請參閱圖 1）。
2. 在**加入檢視** 對話方塊中，選取核取方塊標示為**建立強型別檢視**（請參閱圖 2）。
3. 從**檢視資料類別**下拉式清單中選取產品類別。
4. 從**檢視內容**下拉式清單中，選取建立。
5. 按一下 [新增] 按鈕。


請確定您建置應用程式，再加入的檢視。 否則，類別的清單不會在顯示**檢視資料類別**下拉式清單中。


[![新增專案 對話方塊](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**圖 01**： 加入的檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image2.png))


[![新增專案 對話方塊](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**圖 02**： 建立強型別檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image4.png))


完成這些步驟之後，您會取得列表 2 中建立檢視。

**Listing 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

在列出 2，Html.ValidationSummary() helper 稱為立即 HTML 表單上方。 此協助程式用來顯示驗證錯誤訊息的清單。 Html.ValidationSummary() helper 呈現項目符號清單中的錯誤。

Html.ValidationMessage() helper 會呼叫每個 HTML 表單欄位旁邊。 此協助程式用來顯示表單欄位旁邊的錯誤訊息。 如果列出的 2 是 Html.ValidationMessage() 協助程式發生錯誤時顯示一個星號。

圖 3 中的頁面說明驗證 helper 呈現表單送出遺漏的欄位與無效的值時的錯誤訊息。


[![新增專案 對話方塊](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**圖 03**： 問題與提交的 Create view ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image6.png))


請注意外觀的 html 輸入驗證錯誤時，也會修改欄位。 Html.TextBox() helper 呈現*類別 ="輸入的驗證錯誤的 「* Html.TextBox() 協助程式 」 所呈現的屬性相關聯的驗證錯誤時的屬性。

有三個階層式樣式表類別，用於控制驗證錯誤的外觀：

- 輸入驗證錯誤-套用至&lt;輸入&gt;Html.TextBox() 協助程式 」 所呈現的標記。
- 欄位驗證錯誤-套用至&lt;跨越&gt;Html.ValidationMessage() 協助程式 」 所呈現的標記。
- 若要套用驗證摘要錯誤- &lt;ul&gt; Html.ValidationSumamry() 協助程式 」 所呈現的標記。

您可以修改這些階層式樣式表類別中，並因此修改外觀的驗證錯誤，方法是修改 Site.css 檔案內容資料夾中。

> [!NOTE] 
> 
> HtmlHelper 類別包含唯讀的靜態屬性，擷取驗證的名稱相關 CSS 的類別。 這些靜態屬性是名為 ValidationInputCssClassName、 ValidationFieldCssClassName 和 ValidationSummaryCssClassName。


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding 驗證和 Postbinding 驗證

如果您送出 HTML 表單，建立產品，價格 欄位並沒有 UnitsInStock 欄位的值中輸入無效的值，您會得到在圖 4 顯示驗證訊息。 這些驗證錯誤訊息來自何處？


[![新增專案 對話方塊](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**圖 04**: Prebinding 驗證錯誤 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image8.png))


沒有實際上有兩種類型的驗證錯誤訊息的產生之前為 HTML 表單欄位繫結至類別和表單欄位繫結至類別之後，所產生。 換句話說，沒有 prebinding 驗證錯誤和 postbinding 驗證錯誤。

產品中的控制站列表 1 所公開的 create （） 動作接受產品類別的執行個體。 建立方法的簽章看起來像這樣：

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

建立表單的 HTML 表單欄位的值會繫結至 productToCreate 類別所謂的模型繫結器。 預設模型繫結器錯誤訊息至模型狀態會自動新增當它無法將表單欄位繫結至表單的屬性。

預設模型繫結器無法將字串"apple"繫結至產品類別的價格屬性。 您無法將字串指派給小數點的內容。 因此，模型繫結器會將錯誤加入至模型狀態。

預設模型繫結器也無法將 null 值指派給不接受 null 值的屬性。 特別是，模型繫結器無法將 null 值指派給 [UnitsInStock] 屬性。 同樣地，模型繫結器放棄，並將錯誤訊息加入至模型狀態。

如果您想要自訂的外觀，其中 prebinding 錯誤訊息您需要建立這些訊息的資源字串。

## <a name="summary"></a>總結

本教學課程的目標是驗證的為了說明基本的 ASP.NET MVC 架構中機制。 您已學習如何使用模型狀態和驗證 HTML helper。 我們也將討論 prebinding 和 postbinding 驗證之間的差別。 在其他教學課程中，我們將討論各種不同的策略來移動驗證程式碼從您的控制站移至您的模型類別。

> [!div class="step-by-step"]
> [上一頁](displaying-a-table-of-database-data-cs.md)
> [下一頁](validating-with-the-idataerrorinfo-interface-cs.md)
