---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: "反覆項目 #3-加入表單驗證 (C#) |Microsoft 文件"
author: microsoft
description: "第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 我們也會驗證 emai..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 06c7fc31e138e9009640d20202e4745a61b68eeb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="iteration-3--add-form-validation-c"></a>反覆項目 #3-加入表單驗證 (C#)
====================
由[Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>建立連絡人管理 ASP.NET MVC 應用程式 (C#)
  

在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 與每個反覆項目，我們會逐漸改善應用程式。 這個多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-請看起來很棒的應用程式。 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。

- 反覆項目 #3-加入表單驗證。 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-請鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。

- 反覆項目 #6-使用測試為導向的開發。 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。

- 反覆項目 #7： 加入 Ajax 功能。 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。


## <a name="this-iteration"></a>這個反覆項目

連絡人管理員應用程式的這個第二個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交連絡人，而不需輸入必要的表單欄位的值。 此外，我們也會驗證電話號碼和電子郵件地址 （請參閱圖 1）。


[![新增專案 對話方塊](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**圖 01**： 驗證表單 ([按一下以檢視完整大小的影像](iteration-3-add-form-validation-cs/_static/image2.png))


在這個反覆項目，我們將驗證邏輯直接加入至控制器的動作。 一般情況下，這不是建議用來將驗證加入至 ASP.NET MVC 應用程式。 更好的方法是將應用程式的驗證邏輯放在個別[服務層](http://martinfowler.com/eaaCatalog/serviceLayer.html)。 中的下一個反覆項目中，我們可以重構連絡人管理員應用程式，讓應用程式更容易維護。

在這個反覆項目，為了讓事情變簡單，我們撰寫所有驗證程式碼以手動方式。 而不必自行撰寫驗證程式碼，我們無法利用驗證架構。 例如，您可以使用 Microsoft 企業程式庫驗證應用程式區塊 (VAB) 來實作 ASP.NET MVC 應用程式的驗證邏輯。 若要深入了解驗證應用程式區塊，請參閱：

[*http://msdn.microsoft.com/en-us/library/dd203099.aspx*](https://msdn.microsoft.com/en-us/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>將驗證加入至建立檢視

可讓 s 開始將驗證邏輯加入至建立檢視。 幸運的是，由於我們產生與 Visual Studio 建立檢視時，建立檢視已經包含所有必要的使用者介面邏輯，顯示驗證訊息。 建立檢視包含在程式碼範例 1。

**列出 1-\Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

請注意，就會立即顯示 HTML 表單上方 Html.ValidationSummary() helper 方法的呼叫。 如果有驗證錯誤訊息，這個方法會顯示驗證訊息中項目符號清單。

請注意，此外，每個表單欄位旁出現 Html.ValidationMessage() 呼叫。 ValidationMessage() helper 顯示個別的驗證錯誤訊息。 在程式碼範例 1，發生驗證錯誤時，會顯示一個星號。

最後，Html.TextBox() helper 會自動轉譯的階層式樣式表類別協助程式 」 所顯示的屬性相關聯的驗證錯誤時。 Html.TextBox() helper 呈現類別的**輸入驗證錯誤**。

當您建立新的 ASP.NET MVC 應用程式時，名為 Site.css 樣式表會自動建立的內容資料夾中。 這個樣式表包含驗證錯誤訊息的外觀與相關的 CSS 類別的下列定義：

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

欄位驗證錯誤類別用於 Html.ValidationMessage() 協助程式 」 所呈現的輸出樣式。 輸入驗證錯誤類別用來設定文字方塊中 （輸入） Html.TextBox() 協助程式 」 所呈現的樣式。 驗證摘要錯誤類別用來設定樣式 Html.ValidationSummary() 協助程式 」 所呈現的未排序的清單。

> [!NOTE] 
> 
> 您可以修改自訂的驗證錯誤訊息的外觀，本節所描述的樣式表類別。


## <a name="adding-validation-logic-to-the-create-action"></a>加入驗證邏輯，以建立動作

現在，建立檢視永遠不會顯示驗證錯誤訊息，因為我們沒有產生任何訊息的邏輯寫入。 若要顯示驗證錯誤訊息，您要加入 ModelState 錯誤訊息。

> [!NOTE] 
> 
> UpdateModel() 方法將錯誤訊息加入 ModelState 自動指派給屬性的表單欄位的值發生錯誤時。 例如，如果您嘗試將字串"apple"指派給 BirthDate 屬性可接受的日期時間值，然後 UpdateModel() 方法將錯誤加入至 ModelState。


修改 create （） 方法中列出的 2 包含新的區段，新的連絡人插入資料庫之前會驗證連絡人類別的屬性。

**列出 2-Controllers\ContactController.cs （建立與驗證）**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

[驗證] 區段中，會強制執行四個不同的驗證規則：

- FirstName 屬性長度必須小於或等於零 （而且它不能只包含空格）
- LastName 屬性長度必須小於或等於零 （而且它不能只包含空格）
- 如果電話屬性的值 （長度大於 0） 則電話屬性必須符合規則運算式。
- 如果電子郵件屬性的值 （長度大於 0） 則電子郵件屬性必須符合規則運算式。

違反驗證規則時，錯誤訊息會加入到 ModelState AddModelError() 方法的說明。 當您將訊息加入 ModelState 時，您可以提供屬性的名稱和驗證錯誤訊息文字。 這則錯誤訊息會顯示在檢視中 Html.ValidationSummary() 和 Html.ValidationMessage() helper 方法。

驗證規則會執行後，會檢查 ModelState 的 IsValid 屬性。 當任何驗證錯誤訊息已加入至 ModelState IsValid 屬性會傳回 false。 如果驗證失敗，錯誤訊息重新顯示建立表單。

> [!NOTE] 
> 
> 我收到的驗證電話號碼和電子郵件地址的規則運算式儲存機制從規則運算式[ *http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>將驗證邏輯加入至 編輯動作

Edit() 動作來更新連絡人。 Edit() 動作需要執行 create （） 動作完全相同的驗證。 而不必重複相同的驗證程式碼，我們應該重構連絡控制器，以便在 create （） 和 Edit() 動作呼叫相同的驗證方法。

修改過的連絡人控制器類別包含在列出的 3。 這個類別具有新 ValidateContact() 方法呼叫內的 create （） 」 和 「 Edit() 動作。

**列出 3-Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>總結

在這個反覆項目，我們加入的基本表單驗證連絡人管理員應用程式。 我們將驗證邏輯可防止使用者提交新的連絡人或編輯現有連絡人，而不需要提供 FirstName 和 LastName 屬性的值。 此外，使用者必須提供有效的電話號碼和電子郵件地址。

在這個反覆項目，我們加入驗證邏輯為連絡人管理員應用程式中最簡單的方式。 不過，我們控制器邏輯至混合我們驗證邏輯會產生問題為我們長期來看。 我們的應用程式將會更難維護及修改一段時間。

中的下一個反覆項目中，我們將我們控制器重構我們的驗證邏輯和資料庫存取邏輯。 我們將利用數種軟體設計原則，讓我們來建立更多彈性，且更易於維護，應用程式。

>[!div class="step-by-step"]
[上一頁](iteration-2-make-the-application-look-nice-cs.md)
[下一頁](iteration-4-make-the-application-loosely-coupled-cs.md)
