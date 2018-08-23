---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: 驗證與資料註解驗證器 (C#) |Microsoft Docs
author: microsoft
description: 善用資料註釋模型繫結，若要執行的 ASP.NET MVC 應用程式內的驗證。 了解如何使用不同類型的驗證程式...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 203e49e67d8a9c6eb9dbf605a8d7d860737de073
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832571"
---
<a name="validation-with-the-data-annotation-validators-c"></a>驗證與資料註解驗證器 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 善用資料註釋模型繫結，若要執行的 ASP.NET MVC 應用程式內的驗證。 了解如何使用不同類型的驗證程式屬性，並在 Microsoft Entity Framework 中使用它們。


在本教學課程中，您將了解如何執行驗證的 ASP.NET MVC 應用程式中使用資料註解驗證程式。 使用資料註解驗證程式的優點是，它們可讓您執行驗證，只要新增一或多個屬性 – 例如 Required 或 StringLength 屬性 – 至類別內容。

您可以使用資料註解驗證程式之前，您必須下載資料註釋的模型繫結。 您可以從 CodePlex 網站下載的資料註釋的模型繫結器範例，依序按一下[此處](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)。


請務必了解資料註釋的模型繫結不是官方 Microsoft ASP.NET MVC 架構一部分。 雖然資料註釋的模型繫結 Microsoft ASP.NET MVC 團隊所建立的但是 Microsoft 不提供官方的產品支援的資料註釋的模型繫結所述，並在本教學課程中使用。


## <a name="using-the-data-annotation-model-binder"></a>使用資料註釋的模型繫結

若要使用資料註釋的模型繫結的 ASP.NET MVC 應用程式中，您首先要加入 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件的參考。 選取功能表選項**專案中，加入參考**。 按一下 下一步**瀏覽**索引標籤，然後瀏覽至您下載 （並解壓縮） 資料註釋模型繫結器範例的位置 (請參閱**圖 1**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**圖 1**： 加入資料註釋的模型繫結的參考 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-cs/_static/image3.png))

選取 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件，然後按一下**確定** 按鈕。


您無法使用 System.ComponentModel.DataAnnotations.dll 組件隨附於.NET Framework Service Pack 1 與資料註釋的模型繫結。 您必須使用隨附於下載資料註釋模型繫結器範例 System.ComponentModel.DataAnnotations.dll 組件的版本。


最後，您需要在 Global.asax 檔案中註冊 DataAnnotations 模型繫結器。 將下列程式碼行新增至應用程式\_start （） 事件處理常式，讓應用程式\_start （） 方法看起來像這樣：

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

這行程式碼會註冊 ataAnnotationsModelBinder 為整個 ASP.NET MVC 應用程式的預設模型繫結器。

## <a name="using-the-data-annotation-validator-attributes"></a>使用資料註解驗證程式屬性

當您使用資料註釋的模型繫結時，您會使用驗證程式屬性，來執行驗證。 System.ComponentModel.DataAnnotations 命名空間包含下列的驗證程式屬性：

- 範圍 – 可讓您驗證是否屬性的值介於指定的值範圍之間。
- RegularExpression – 可讓您驗證是否屬性的值符合指定的規則運算式模式。
- 所需 – 可讓您將標記所需的屬性。
- StringLength – 可讓您指定的字串屬性的最大長度。
- 驗證-驗證程式的所有屬性的基底類別。

> [!NOTE] 
> 
> 如果您的驗證需求不符合任何的標準驗證然後您一律可以選擇建立自訂驗證程式屬性，藉由繼承自基底的驗證屬性的 新的驗證程式屬性。


中的產品類別**列表 1**說明如何使用這些驗證程式屬性。 名稱、 描述和 UnitPrice 屬性會標示為必要。 Name 屬性必須是少於 10 個字元的字串長度。 最後，[UnitPrice] 屬性必須比對規則運算式模式，表示貨幣金額。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**列表 1**: Models\Product.cs

Product 類別說明如何使用一個額外的屬性： DisplayName 屬性。 DisplayName 屬性可讓您修改的屬性名稱時顯示錯誤訊息中的屬性。 您可以不會顯示錯誤訊息 「 [UnitPrice] 欄位必要的 「 顯示錯誤訊息 「 [價格] 欄位是必要的 」。

> [!NOTE] 
> 
> 如果您想要完全自訂驗證程式所顯示的錯誤訊息然後您就可以指派自訂錯誤訊息給驗證程式的錯誤訊息屬性，像這樣： `<Required(ErrorMessage:="This field needs a value!")>`


您可以使用中的產品類別**列表 1** create （） 的控制器動作中使用**列表 2**。 模型狀態中包含的任何錯誤時，此控制器動作會重新顯示建立檢視。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**列表 2**: Controllers\ProductController.vb

最後，您可以在其中建立中的檢視**列表 3**以滑鼠右鍵按一下 create （） 動作，然後選取功能表選項**加入檢視**。 建立與產品類別的強型別檢視，做為模型類別。 選取  **Create**從檢視內容的下拉式清單 (請參閱**圖 2**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**圖 2**： 加入建立檢視

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**列表 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> 移除所產生的建立表單中的 [識別碼] 欄位**加入檢視**功能表選項。 由於 [識別碼] 欄位會對應到識別欄位，您不想要允許使用者輸入此欄位的值。


如果您送出的表單建立產品，而且您未輸入值為必要的欄位，則驗證錯誤訊息**圖 3**會顯示。

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**圖 3**： 遺漏必要的欄位

如果您輸入不正確的貨幣金額，則中的錯誤訊息**圖 4**隨即出現。

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**圖 4**： 無效的貨幣金額

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>使用 Entity Framework 中的資料註解驗證程式

如果您使用 Microsoft Entity Framework 產生的資料模型類別然後您無法驗證程式屬性直接套用到您的類別。 Entity Framework 設計工具產生模型類別，因為您對模型類別的任何變更將會被覆寫下一次您在設計工具中進行任何變更。

如果您想要使用 Entity Framework 所產生的類別中的驗證程式您需要建立中繼資料類別。 您可以套用驗證至中繼資料類別，而不是實際的類別中套用的驗證程式。

例如，假設您已建立使用 Entity Framework 的電影類別 (請參閱**圖 5**)。 此外，假設您想要讓電影標題和主管屬性所需的內容。 在此情況下，您可以在其中建立部分類別和中的中繼資料類別**列表 4**。

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**圖 5**: Entity Framework 所產生的影片類別

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**列表 4**: Models\Movie.cs

中的檔案**列表 4**包含名為影片和 MovieMetaData 的兩個類別。 影片類別是部分類別。 它會對應至 Entity Framework DataModel.Designer.vb 檔案中包含所產生的部分類別。

目前，.NET framework 不支援部分的屬性。 因此，沒有任何方法所套用的驗證程式屬性的定義檔中的電影類別屬性定義 DataModel.Designer.vb 檔案中的電影類別的屬性套用的驗證程式屬性**列表 4**.

請注意影片的部分類別附有指向 MovieMetaData 類別 MetadataType 屬性。 MovieMetaData 類別包含屬性的電影類別的 proxy 屬性。

驗證程式屬性會套用至 MovieMetaData 類別的屬性。 所有標示為必要屬性的標題、 導演和 DateReleased 屬性。 總監屬性必須被指派包含 5 個字元的字串。 最後，DisplayName 屬性會套用至 DateReleased 屬性以顯示錯誤訊息 「 日期發行欄位是必要。 」 而不是錯誤 」 DateReleased 為必要欄位。 」

> [!NOTE] 
> 
> 請注意，proxy 類別中的屬性 MovieMetaData 不需要代表影片類別中的對應屬性相同的類型。 比方說，經理屬性是電影類別中的字串屬性和 MovieMetaData 類別中的物件屬性。


中的網頁**圖 6**說明當您輸入的影片內容無效的值時，傳回的錯誤訊息。

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**圖 6**： 使用 Entity Framework 中的驗證程式 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>總結

在本教學課程中，您已了解如何善用資料註釋模型繫結，若要執行的 ASP.NET MVC 應用程式內的驗證。 您已了解如何使用不同類型的驗證程式屬性，例如 Required 和 StringLength 屬性。 您也學到如何使用 Microsoft Entity Framework 時，使用這些屬性。

> [!div class="step-by-step"]
> [上一頁](validating-with-a-service-layer-cs.md)
> [下一頁](creating-model-classes-with-the-entity-framework-vb.md)
