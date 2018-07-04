---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: 使用 ViewData 和實作 ViewModel 類別 |Microsoft Docs
author: microsoft
description: 步驟 6 顯示如何啟用更豐富的格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: edc62246cdc5e5df51c369a70b47dab92c9ecc1c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365119"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>使用 ViewData 和實作 ViewModel 類別
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 6 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 6 顯示如何啟用更豐富的格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法： ViewData 和 ViewModel。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner 步驟 6: ViewData 和 ViewModel

我們已涵蓋了數個表單張貼案例，並討論如何實作建立、 更新和刪除 (CRUD) 支援。 現在，我們會採取進一步的 DinnersController 實作，並啟用更豐富的格式編輯案例的支援。 執行這個操作時，我們將討論可用來將資料從控制器傳遞至檢視的兩種方法： ViewData 和 ViewModel。

### <a name="passing-data-from-controllers-to-view-templates"></a>將資料從控制器傳遞至檢視範本

MVC 模式的定義特性之一，是嚴格 「 關注點分離 」 可協助應用程式的不同元件之間強制執行。 模型、 控制器和檢視每個具有良好定義的角色和責任，並以妥善定義的方式彼此通訊。 這有助於升級可測試性和重複使用程式碼。

當控制器類別決定要呈現 HTML 回應傳回給用戶端時，它會負責明確傳送的所有資料來呈現回應所需的檢視範本。 檢視範本應該永遠不會執行任何資料擷取或應用程式邏輯，而且應該改為限制本身只會有驅動傳遞給它，控制器模型/資料的轉譯程式碼。

目前模型的資料由我們 DinnersController 傳遞至我們的檢視範本的類別很簡單，而且簡單 – index （），在 Dinner 物件的清單及單一物件在 Details()、 edit （）、 create （） 和 delete （） 的情況下。 因為我們將更多的 UI 功能新增至我們的應用程式時，通常我們需要傳遞不只是這項資料來呈現 HTML 回應，在我們的檢視範本。 比方說，我們可能要變更 「 國家/地區 」 欄位，在我們的編輯和建立檢視從 dropdownlist 進行 HTML 文字方塊。 與其硬式編碼在檢視範本中的國家/地區名稱的下拉式清單，我們可能會想要產生從支援的國家，我們會以動態方式填入的清單。 我們將會需要想辦法將 Dinner 物件傳遞*和*支援國家/地區從控制器到我們的檢視範本的清單。

讓我們看看我們可以達成這個目的的兩種方式。

### <a name="using-the-viewdata-dictionary"></a>使用 ViewData 字典

控制器的基底類別會公開 「 ViewData"字典屬性，可用來將其他資料的項目從控制器傳遞至檢視。

比方說，若要支援的案例，我們想要防止 dropdownlist 進行 HTML 文字方塊變更 「 國家/地區 」 文字方塊中，我們的編輯檢視中，我們可以更新我們的 edit （） 動作方法，傳遞電子郵件 （除了 Dinner 物件） 可用來當做 m SelectList 物件odel 的國家/地區 dropdownlist。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

上述 SelectList 的建構函式會接受郡來填入下拉式 downlist，以及目前選取的值的清單。

然後，我們可以更新 Edit.aspx 檢視範本使用 Html.DropDownList() helper 方法，而不是我們先前使用 Html.TextBox() helper 方法：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

上述的 Html.DropDownList() helper 方法會採用兩個參數。 第一個是要輸出 HTML 表單項目的名稱。 第二個是我們透過 ViewData 字典傳遞"SelectList 」 模型。 我們會使用 C#"關鍵字 as"轉換為 SelectList 字典內的型別。

現在，當我們執行我們的應用程式和存取並 */Dinners/Edit/1* URL 在我們的瀏覽器中所見，我們編輯 UI 已更新為顯示的國家/地區，而不是 textbox dropdownlist 進行：

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

因為我們也會呈現編輯檢視範本，從 HTTP POST 編輯方法 （在錯誤發生時的情況下），我們會想要確定我們也更新用戶端時在錯誤狀況中呈現的檢視範本，將 SelectList ViewData 加入這個方法：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

而且我們 DinnersController 編輯案例現在支援 DropDownList。

### <a name="using-a-viewmodel-pattern"></a>使用 ViewModel 模式

ViewData 字典方法的好處是能夠相當快速且輕鬆地實作。 有些開發人員不喜歡使用以字串為基礎的字典，不過，因為錯字可能會導致不會攔截在編譯時期的錯誤。 不具型別的的 ViewData 字典也需要使用"，"運算子或轉換時使用強型別 C# 等語言中檢視範本。

我們可以使用另一個方法是通常稱為 「 ViewModel 」 模式。 使用此模式時我們會建立強型別的類別，適用於我們的特定檢視案例，以及其公開屬性，動態值/內容所需的我們的檢視範本。 然後填入並將這些檢視最佳化的類別傳遞至我們的檢視範本，若要使用我們的控制器類別。 這可讓類型安全、 編譯時間檢查，以及檢視範本中的編輯器 intellisense。

例如，若要啟用編輯案例，我們可以建立 「 DinnerFormViewModel"像下面類別會公開兩個強型別屬性 dinner 表單： Dinner 物件，並填入國家/地區 dropdownlist 需 SelectList 模型：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

我們可以接著更新來建立使用您從我們的存放庫中，我們擷取 Dinner 物件 DinnerFormViewModel 我們 edit （） 動作方法，並再將它傳遞至我們的檢視範本：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

我們會接著更新我們檢視範本，因此它必須要有 「 DinnerFormViewModel"而不是 「 Dinner"物件變更 edit.aspx 頁面頂端的 「 繼承 」 屬性就像這樣：

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

一旦我們這樣做，我們檢視範本內的 「 模型 」 屬性的 intellisense 將會更新以反映我們傳遞 DinnerFormViewModel 類型的物件模型：

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

然後，我們可以更新我們的檢視程式碼，才能關閉。 請注意下面如何我們不會變更之輸入項目的名稱，我們建立 （表單項目將仍然會命名為"Title"，"Country"） – 但我們正在更新的 HTML Helper 方法來擷取使用 DinnerFormViewModel 類別的值：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

我們也會更新時要使用 DinnerFormViewModel 類別的轉譯錯誤我們編輯 post 方法：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

我們也可以更新我們的 create （） 動作方法，以便重複使用的確切相同*DinnerFormViewModel*類別以啟用的國家/地區 DropDownList 其內以及。 以下是 HTTP GET 實作：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

以下是 HTTP POST 建立方法的實作：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

以及我們編輯和建立的畫面現在支援拖放在此挑選 國家/地區。

### <a name="custom-shaped-viewmodel-classes"></a>自訂形狀 ViewModel 類別

在上述案例中，我們 DinnerFormViewModel 類別直接公開 Dinner 模型物件為屬性，以及支援的 SelectList 模型屬性。 這種方法可正常運作的案例中我們想要建立檢視範本的 HTML UI 對應相當接近我們的領域模型物件。

這不是適合的情況下，您可以使用的其中一個選項是建立自訂形狀 ViewModel 類別的物件模型更適合用於耗用量的檢視 – 和可能看起來完全不同的基礎網域模型物件。 比方說，它可能會公開不同的屬性名稱及/或從多個模型物件收集到的彙總屬性。

自訂形狀 ViewModel 類別都可以同時用來將資料從控制器傳遞至轉譯，以及協助處理回傳至控制器動作方法的表單資料的檢視。 此更新的案例中，您可能必須更新表單張貼的資料的 ViewModel 物件，然後使用 ViewModel 執行個體對應，或擷取實際的網域模型物件的動作方法。

自訂形狀 ViewModel 類別可提供超強的彈性，且要調查的每當你檢視範本或表單張貼程式碼內找到轉譯程式碼在您開始變得太複雜的動作方法內的項目。 這通常是領域模型完全不對應至您要產生的 UI 和中繼自訂形狀 ViewModel 類別可以幫助。

### <a name="next-step"></a>下一個步驟

現在來看看如何我們可以使用部分和主版頁面要重複使用及共用跨應用程式的 UI。

> [!div class="step-by-step"]
> [上一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [下一頁](re-use-ui-using-master-pages-and-partials.md)
