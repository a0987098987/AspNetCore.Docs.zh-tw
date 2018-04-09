---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: 使用別的 ViewData 和實作 ViewModel 類別 |Microsoft 文件
author: microsoft
description: 步驟 6 顯示如何啟用更豐富格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>使用別的 ViewData 和實作 ViewModel 類別
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 6 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 6 顯示如何啟用更豐富格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法： 別的 ViewData 和 ViewModel。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner 步驟 6： 別的 ViewData 和 ViewModel

我們已涵蓋了數個表單張貼案例，並討論如何實作建立、 更新和刪除 (CRUD) 支援。 我們現在將會採取進一步的 DinnersController 實作，並啟用更豐富格式編輯案例的支援。 執行此動作時，我們將討論可用來將資料從控制器傳遞至檢視的兩種方法： 別的 ViewData 和 ViewModel。

### <a name="passing-data-from-controllers-to-view-templates"></a>將資料從控制器傳遞至檢視範本

MVC 模式的定義特性的其中一個是 strict"的重要性分離 」 可協助應用程式的不同元件之間強制執行。 模型、 控制器和檢視每個已正確定義的角色和責任，並以妥善定義的方式彼此通訊。 這有助於升級可測試性並重複使用程式碼。

當控制器類別會決定要呈現的 HTML 回應傳回給用戶端時，它會負責明確傳遞至檢視範本的所有資料需要回應。 檢視範本應該永遠不會執行任何的資料擷取或應用程式邏輯 –，而且應該改為限制只會有驅動開控制器所收到的模型/資料的轉譯程式碼本身。

現在模型的資料傳入我們 DinnersController 我們檢視範本的類別是簡單明瞭 – index （），在 Dinner 物件的清單以及單一 Dinner 物件在 Details()、 Edit()、 create （） 和 delete 的情況下。 因為我們將更多的 UI 功能加入至我們的應用程式時，我們通常會需要傳遞不只是這項資料來呈現 HTML 回應中我們檢視範本。 比方說，我們可能會想要變更的 「 國家/地區 」 欄位中在我們的編輯，並防止 dropdownlist 以 HTML 文字方塊中建立檢視。 而不是硬式編碼檢視範本中的國家 （地區） 名稱的下拉式清單，我們可能會想要產生從支援國家 （地區），我們以動態方式填入的清單。 我們將會需要能夠傳遞 Dinner 物件*和*支援國家/地區從 controller 我們檢視範本清單。

讓我們看看我們可以完成這兩種方式。

### <a name="using-the-viewdata-dictionary"></a>使用別的 ViewData 字典

控制器的基底類別會公開 「 別的 ViewData"字典屬性可以用來將其他資料項目控制站從傳遞至檢視。

例如，若要支援的案例，我們要變更 「 國家/地區 」 文字方塊中，我們的編輯檢視中進行 dropdownlist 以 HTML 文字方塊中，我們可以更新來傳送電子郵件 （除了 Dinner 物件） SelectList 物件，可用來當作 m 我們 Edit() 動作方法odel 的國家/地區 dropdownlist。

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

上述 SelectList 的建構函式可接受郡來填入下拉式 downlist，為目前選取的值的清單。

然後，我們可以更新使用 Html.DropDownList() helper 方法，而不是我們先前使用 Html.TextBox() helper 方法我們 Edit.aspx 檢視範本：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

上述的 Html.DropDownList() helper 方法會採用兩個參數。 第一個是輸出將 HTML 表單項目的名稱。 第二個是我們透過別的 ViewData 字典傳遞"SelectList 」 模型。 我們使用 C#"關鍵字 as"轉型為 SelectList 字典內的型別。

現在，當我們執行我們的應用程式，以及存取和*/Dinners/Edit/1* URL 我們瀏覽器中我們會看到我們編輯 UI，已更新以顯示國家/地區，而不是 textbox 的 dropdownlist:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

我們也會呈現編輯檢視範本，從編輯 HTTP POST 方法 （在案例中發生錯誤時），因為我們會想要確定我們也更新這個方法時可加入 SelectList 別的 ViewData 在錯誤狀況中呈現的檢視表範本：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

和我們 DinnersController 編輯案例現在支援 DropDownList。

### <a name="using-a-viewmodel-pattern"></a>使用 ViewModel 模式

別的 ViewData 字典方法有優點就是相當快速而輕鬆地實作。 有些開發人員不喜歡使用字串為基礎的字典，不過，因為拼字錯誤可能會導致不會攔截在編譯時期的錯誤。 不具型別的別的 ViewData 字典也需要使用"為"運算子或轉換時使用強型別 C# 等語言中檢視範本。

我們可以使用替代方法是其中一個通常稱為 「 ViewModel 」 模式。 使用此模式時我們建立強型別的類別，最佳化的特定檢視案例中，以及其公開之動態值/內容我們檢視範本所需的屬性。 我們控制器類別可以填入並將這些檢視最佳化類別傳遞給我們要使用的檢視範本。 這可讓類型安全、 編譯時期檢查，以及編輯器 intellisense 中檢視範本。

例如，若要啟用編輯案例，我們可以建立 「 DinnerFormViewModel"像下面類別會公開兩個強型別屬性 dinner 表單： Dinner 物件，並填入國家 （地區) dropdownlist 需 SelectList 模型：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

我們可以然後更新 若要建立使用 Dinner 物件我們從我們的儲存機制擷取 DinnerFormViewModel 我們 Edit() 動作方法，並將它傳遞至我們檢視範本：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

我們會接著我們檢視範本，因此它必須要有 「 DinnerFormViewModel"而不是 「 Dinner"物件"inherits"屬性變更 edit.aspx 頁面頂端的更新，就像這樣：

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

一旦我們這樣做時，「 模型 」 屬性，我們檢視範本中的 intellisense 將會更新以反映我們傳遞 DinnerFormViewModel 類型的物件模型：

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

然後，我們可以更新檢視程式碼中使用它的索引。 請注意以下方式我們並不會變更輸入項目的名稱，我們會建立 （form 項目將仍被命名為"Title"，「 國家/地區 」） – 但我們正在更新的 HTML Helper 方法來擷取使用 DinnerFormViewModel 類別的值：

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

我們也會將更新的轉譯錯誤時，使用 DinnerFormViewModel 類別我們編輯 post 方法：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

我們也可以更新重複使用的確切我們 create （） 動作方法相同*DinnerFormViewModel*類別，讓的國家/地區 DropDownList 內那些以及。 以下是 HTTP GET 實作：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

以下是建立 HTTP POST 方法的實作：

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

以及我們編輯和建立的畫面現在支援拖放在此選擇國家/地區。

### <a name="custom-shaped-viewmodel-classes"></a>自訂形狀 ViewModel 類別

在上述案例中，我們 DinnerFormViewModel 類別直接公開 Dinner 模型物件為屬性，以及支援 SelectList 模型屬性。 這種方法其中我們想要建立我們檢視範本的 HTML UI 對應相當接近我們網域模型物件的情況下運作正常。

這不是其中的案例的情況下，您可以使用的其中一個選項是建立自訂形狀 ViewModel 類別的物件模型更適合耗用量檢視 – 所且這看起來可能完全不同的基礎網域模型物件。 例如，它可能無法公開不同的屬性名稱和 （或） 從多個模型物件收集到的彙總屬性。

您可以為自訂形狀 ViewModel 類別用於兩者中控制站的資料傳遞至轉譯，以及協助處理回傳至控制器動作方法的表單資料的檢視。 此更新的案例中，您可能必須表單張貼的資料，更新的 ViewModel 物件，然後使用 ViewModel 執行個體對應，或擷取實際的網域模型物件的動作方法。

自訂形狀 ViewModel 類別可以提供極大的彈性，而且調查隨時檢視範本或表單張貼程式碼中尋找轉譯程式碼在您開始得太複雜的動作方法內的項目。 這通常是正負號的網域模型不完全地對應至您產生的 UI 中繼自訂形狀 ViewModel 類別可以幫助。

### <a name="next-step"></a>下一個步驟

讓我們現在看看如何我們可以使用 partials 和主版頁面來重複使用，並在我們的應用程式之間共用 UI。

> [!div class="step-by-step"]
> [上一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [下一頁](re-use-ui-using-master-pages-and-partials.md)
