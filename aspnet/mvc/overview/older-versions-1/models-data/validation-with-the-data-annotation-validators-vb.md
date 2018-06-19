---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: 使用資料註解驗證程式 (VB) 驗證 |Microsoft 文件
author: microsoft
description: 利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。 了解如何使用不同類型的驗證程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870163"
---
<a name="validation-with-the-data-annotation-validators-vb"></a>使用資料註解驗證程式 (VB) 驗證
====================
by [Microsoft](https://github.com/microsoft)

> 利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。 了解如何使用不同類型的驗證程式屬性，並在 Microsoft Entity Framework 中使用它們。


在本教學課程中，您會學習如何使用資料註解驗證程式在 ASP.NET MVC 應用程式中執行驗證。 使用資料註解驗證器的優點是可讓您執行驗證，只要新增一或多個屬性 – 例如 Required 或 StringLength 屬性 – 至類別內容。

您可以使用資料註解驗證器之前，您必須下載資料註解模型繫結器。 您可以從 CodePlex 網站下載資料註解的模型繫結器範例，依序按一下[這裡](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)。


請務必了解資料註解模型繫結器不是官方 Microsoft ASP.NET MVC framework 的一部分。 雖然資料註解模型繫結器 Microsoft ASP.NET MVC 團隊所建立，但是 Microsoft 不提供資料註解模型繫結器提供官方產品支援描述，在本教學課程中使用。


## <a name="using-the-data-annotation-model-binder"></a>使用資料註解模型繫結器

若要在 ASP.NET MVC 應用程式中使用資料註解模型繫結器，您需要新增 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件的參考。 選取功能表選項**專案中，加入參考**。 按一下 下一步**瀏覽**索引標籤上，瀏覽至您下載 （並解壓縮） 資料註解模型繫結器範例的位置 (請參閱**圖 1**)。

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**圖 1**： 將參考加入至資料註解模型繫結器 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-vb/_static/image3.png))

選取 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件，然後按一下**確定** 按鈕。


您無法使用 System.ComponentModel.DataAnnotations.dll 組件隨附於.NET Framework Service Pack 1 及資料註解模型繫結器。 您必須使用隨附的資料附註模型繫結器範例下載 System.ComponentModel.DataAnnotations.dll 組件的版本。


最後，您要在 Global.asax 檔案中註冊 DataAnnotations 模型繫結器。 將下列程式碼行加入至應用程式\_start （） 事件處理常式，讓應用程式\_start （） 方法看起來像這樣：

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

這行程式碼會註冊 DataAnnotationsModelBinder 為整個 ASP.NET MVC 應用程式的預設模型繫結器。

## <a name="using-the-data-annotation-validator-attributes"></a>使用資料註解驗證程式屬性

當您使用資料註解模型繫結器時，您可以使用驗證程式屬性來執行驗證。 System.ComponentModel.DataAnnotations 命名空間包含下列的驗證程式屬性：

- 範圍-可讓您驗證是否屬性的值介於指定的值範圍。
- ReqularExpression – 可讓您以驗證是否屬性的值符合指定的規則運算式模式。
- 所需 – 可讓您將屬性標記為必要。
- StringLength – 可讓您指定為字串屬性的最大長度。
- 驗證-所有驗證程式屬性的基底類別。

> [!NOTE] 
> 
> 如果您的驗證需求不符合任何的標準驗證然後一定繼承自基底的驗證屬性的 新的驗證程式屬性，以建立自訂驗證程式屬性的選項。


中的產品類別**列出 1**說明如何使用這些驗證程式屬性。 名稱、 描述和 UnitPrice 屬性會標示為必要。 Name 屬性必須是少於 10 個字元的字串長度。 最後，UnitPrice 屬性必須符合規則運算式模式，表示貨幣金額。

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**清單 1**: Models\Product.vb

產品類別將說明如何使用一個額外的屬性： DisplayName 屬性。 DisplayName 屬性可讓您修改的屬性名稱時顯示錯誤訊息中的屬性。 而不是顯示錯誤訊息"UnitPrice 欄位必要的 「 您可以顯示錯誤訊息 「 [價格] 欄位是必要的 」。

> [!NOTE] 
> 
> 如果您想要自訂驗證程式所顯示的錯誤訊息然後您就可以指派自訂錯誤訊息給驗證程式的錯誤訊息屬性，就像這樣： `<Required(ErrorMessage:="This field needs a value!")>`


您可以使用中的產品類別**列出 1** create （） 的控制器動作中**列出 2**。 模型狀態中包含的任何錯誤時，此控制器動作重新顯示建立檢視。

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**表 2**: Controllers\ProductController.vb

最後，您可以建立的檢視**列出 3** create （） 動作上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。 建立強型別檢視與產品類別，為模型類別。 選取**建立**從檢視內容的下拉式清單 (請參閱**圖 2**)。

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**圖 2**： 加入建立檢視

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**列出 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> 移除所產生的建立表單的 [識別碼] 欄位**加入檢視**功能表選項。 由於 [識別碼] 欄位會對應到 Identity 資料行，您不想允許使用者輸入此欄位的值。


如果您送出的表單建立產品，而且您沒有輸入值為必要欄位，則驗證錯誤訊息中**圖 3**會顯示。

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**圖 3**： 遺漏必要的欄位

如果您輸入無效的貨幣金額，則錯誤訊息中的**圖 4**隨即出現。

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**圖 4**： 無效的貨幣金額

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Entity Framework 搭配使用資料註解驗證器

如果您使用 Microsoft Entity Framework 來產生資料模型類別然後您就無法套用的驗證程式屬性直接加入您的類別。 因為 Entity Framework Designer 產生的模型類別，您對模型類別進行任何變更將會被覆寫的下次您在設計工具中進行任何變更。

如果您想要使用 Entity Framework 所產生的類別中的驗證程式您需要建立中繼資料類別。 您套用中繼資料類別，而不是實際的類別中套用的驗證程式驗證程式。

例如，假設您已建立使用 Entity Framework 的電影類別 (請參閱**圖 5**)。 此外，假設您想要將電影標題和主管屬性的必要的屬性。 在此情況下，您可以在其中建立部分類別和中的中繼資料類別**列出 4**。

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**圖 5**: Entity Framework 所產生的電影類別

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**列出 4**: Models\Movie.vb

中的檔案**列出 4**包含名為電影和 MovieMetaData 的兩個類別。 影片類別是部分類別。 它會對應至 DataModel.Designer.vb 檔案中包含的 Entity Framework 所產生的部分類別。

目前，.NET framework 不支援部分屬性。 因此，沒有任何方法來驗證程式屬性會套用到透過將驗證程式屬性套用至電影類別檔中定義的屬性定義 DataModel.Designer.vb 檔案中的影片類別屬性**列出 4**.

請注意，以指向 MovieMetaData 類別 MetadataType 屬性裝飾影片部分類別。 MovieMetaData 類別包含內容的影片類別的 proxy 屬性。

驗證程式屬性套用至 MovieMetaData 類別的屬性。 所有標記為必要的屬性標題、 導演和 DateReleased 屬性。 主管屬性必須被指派包含 5 個字元的字串。 最後，DisplayName 屬性會套用到 DateReleased 屬性來顯示錯誤訊息一樣 」 此日期發行為必要欄位。 」 而不是錯誤 「 DateReleased 為必要欄位。 」

> [!NOTE] 
> 
> 請注意，proxy 類別中的屬性 MovieMetaData 不需要代表影片類別中的對應屬性相同的類型。 例如，主管屬性是電影類別中的字串屬性和 MovieMetaData 類別中的物件屬性。


在頁面**圖 6**說明影片屬性中輸入無效值時，傳回的錯誤訊息。

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**圖 6**: Entity Framework 搭配使用的驗證程式 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>總結

在本教學課程中，您學會如何充分利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。 您已學習如何使用不同的驗證程式屬性，例如 Required 和 StringLength 屬性類型。 您也學到如何使用 Microsoft Entity Framework 時，使用這些屬性。

> [!div class="step-by-step"]
> [上一步](validating-with-a-service-layer-vb.md)
