---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "檢查 ASP.NET MVC 如何 scaffolds DropDownList Helper |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>檢查 ASP.NET MVC 如何 scaffolds DropDownList Helper
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

在**方案總管 中**，以滑鼠右鍵按一下*控制器*資料夾，然後選取**加入控制器**。 控制器**StoreManagerController**。 設定的選項**加入控制器**下圖所示的對話方塊。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

編輯*StoreManager\Index.cshtml*檢視並移除`AlbumArtUrl`。 移除`AlbumArtUrl`會讓呈現更容易閱讀。 已完成的程式碼如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

開啟*Controllers\StoreManagerController.cs*檔案，並尋找`Index`方法。 新增`OrderBy`子句，因此價格會依專輯。 完整的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

排序價格會請您更輕鬆地測試資料庫的變更。 當您要測試的編輯，並建立方法時，您可以使用最低價，因此會出現在第一個儲存的資料。

開啟*StoreManager\Edit.cshtml*檔案。 加入下列行後方圖例標籤。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

下列程式碼顯示這項變更的內容：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` ，才能變更專輯記錄。

按 CTRL+F5 執行應用程式。 選取即可**Admin**連結，然後選取 **建立新**連結，以建立新的相簿。 請確認已儲存的專輯資訊。 編輯專輯，並確認您所做的變更會保存。

### <a name="the-album-schema"></a>專輯結構描述

`StoreManager`音樂存放區資料庫中控制站建立的 MVC scaffolding 機制可讓您的相簿 CRUD （建立、 讀取、 更新、 刪除） 存取。 專輯資訊的結構描述如下所示：

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums`資料表不會儲存專輯內容類型和描述，它會儲存的外部索引鍵`Genres`資料表。 `Genres`資料表包含的內容類型名稱和描述。 同樣地，`Albums`資料表未包含專輯演出者名稱，但的外部索引鍵`Artists`資料表。 `Artists`資料表包含的演出者名稱。 如果您檢查中的資料`Albums`資料表中，您可以看到每個資料列包含的外部索引鍵`Genres`資料表和外部索引鍵`Artists`資料表。 下圖顯示的某些資料表資料`Albums`資料表。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML 選取標記

HTML`<select>`元素 (由 HTML 建立[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)協助程式) 用來顯示值 （例如內容類型清單） 的完整清單。 編輯表單時已知的目前值，則選取清單可以顯示的目前值。 我們了解此先前當我們選取的值設定為**喜劇**。 Select 清單是適合用來顯示類別目錄或外部索引鍵資料。 `<select>`內容類型的外部索引鍵的項目會顯示可能的內容類型名稱的清單，但當您儲存的表單類型外部索引鍵值，而不是顯示的內容類型名稱與更新的內容類型屬性。 在下面的影像中，選取內容類型是**Disco**和演出者是**Donna Summer**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>檢查 ASP.NET MVC 建立結構的程式碼

開啟*Controllers\StoreManagerController.cs*檔案，並尋找`HTTP GET Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create`方法會將兩個[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)物件加入至`ViewBag`，一個包含內容類型的資訊，一個包含演出者資訊。 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上面使用的建構函式多載會採用三個引數：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含在清單中的項目。 在上述範例中，內容類型的清單傳回`db.Genres`。
2. *dataValueField*： 中屬性的名稱**IEnumerable**清單，其中包含的索引鍵值。 在上述範例`GenreId`和`ArtistId`。
3. *dataTextField*： 中屬性的名稱**IEnumerable**清單，其中包含要顯示的資訊。 演出者和類型表格中`name`欄位使用。

開啟*Views\StoreManager\Create.cshtml*檔案，並檢查`Html.DropDownList`協助程式類型欄位的標記。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

第一行會顯示建立檢視所花費的`Album`模型。 在`Create`方法如上所示，已通過沒有模型，因此檢視取得**null** `Album`模型。 此時我們會建立新的相簿因此我們不需要任何`Album`為它的資料。

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)如上所示的多載會採用要繫結至模型之欄位的名稱。 它也會使用此名稱尋找**ViewBag**物件，包含[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)物件。 使用這個多載，您都必須名稱**ViewBag SelectList**物件`GenreId`。 第二個參數 (`String.Empty`) 是要在選取的項目時顯示的文字。 這正是我們想要建立新專輯時。 如果您移除第二個參數，並使用下列程式碼：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

選取清單會預設為第一個元素或 Rock，在我們的範例。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

檢查`HTTP POST Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

這個多載`Create`方法會採用`album`張貼的表單值的 ASP.NET MVC 模型繫結系統所建立的物件。 當您送出新的專輯、 模型狀態有效，且沒有資料庫錯誤時，新的相簿就會加入資料庫。 下圖顯示建立新的相簿。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

您可以使用[fiddler 工具](http://www.fiddler2.com/fiddler2/)來檢查的已張貼的表單值建立專輯物件會使用 ASP.NET MVC 模型繫結。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>重構 ViewBag SelectList 建立

這兩個`Edit`方法和`HTTP POST Create`方法有相同的程式碼來設定**SelectList**中**ViewBag**。 中的精神[乾](http://en.wikipedia.org/wiki/Don't_repeat_yourself)，我們將會重構此程式碼。 我們要使用這個重構程式碼之後。

建立新的方法來新增內容類型與演出者**SelectList**至**ViewBag**。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

取代為兩行設定`ViewBag`中每個`Create`和`Edit`方法呼叫`SetGenreArtistViewBag`方法。 已完成的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

建立新的相簿和編輯以確認所做的變更都會相簿。

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>明確地將 SelectList 傳遞至 DropDownList

建立與編輯檢視建立 ASP.NET MVC scaffold 利用下列**DropDownList**多載：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList`建立檢視的標記如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

因為`ViewBag`屬性`SelectList`名為`GenreId`、 **DropDownList**協助程式將會使用`GenreId` **SelectList**中**ViewBag**. 在下列**DropDownList**負擔過重、`SelectList`明確傳遞。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

開啟*Views\StoreManager\Edit.cshtml*檔案，然後變更**DropDownList**呼叫中明確傳遞**SelectList**，使用上述的多載。 執行這項操作的內容類型分類。 已完成的程式碼如下所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

執行應用程式，然後按一下**Admin**連結，然後瀏覽至 Jazz 專輯，並選取**編輯**連結。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

而不顯示 Jazz 目前選取的內容類型為 Rock 隨即出現。 當字串引數 （若要繫結屬性） 和**SelectList**物件具有相同的名稱，不會使用選取的值。 中的第一個項目未提供所選的值時，預設瀏覽器**SelectList**(也就是**Rock**上述範例中)。 這是已知的限制**DropDownList**協助程式。

開啟*Controllers\StoreManagerController.cs*檔案，並且變更**SelectList**物件名稱來`Genres`和`Artists`。 已完成的程式碼如下所示：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

內容類型和演出者名稱是更好的名稱對於分類，因為它們包含不只是每個類別目錄的識別碼。 我們稍早的重構成效。 而不是變更**ViewBag**中四種方法，我們所做的變更與隔離`SetGenreArtistViewBag`方法。

變更**DropDownList**呼叫在建立和編輯檢視，以使用新**SelectList**名稱。 編輯檢視的新標記如下所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

建立檢視需要空字串，以防止 SelectList 中的第一個項目顯示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

建立新的相簿和編輯以確認所做的變更都會相簿。 選取內容類型 Rock 以外的相簿測試編輯程式碼。

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>使用 DropDownList Helper 的檢視模型

建立新的類別在名為 ViewModels 資料夾`AlbumSelectListViewModel`。 中的程式碼取代`AlbumSelectListViewModel`具有下列類別：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel`建構函式會將專輯、 演出者與內容類型的清單，並建立物件，其中包含專輯和`SelectList`的內容類型和演出者名稱。

建置專案而`AlbumSelectListViewModel`時可以使用我們在下一個步驟中建立檢視。

新增`EditVM`方法`StoreManagerController`。 已完成的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

以滑鼠右鍵按一下`AlbumSelectListViewModel`，選取**解決**，然後**使用 MvcMusicStore.ViewModels;**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

或者，您可以在其中新增下列 using 陳述式：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

以滑鼠右鍵按一下`EditVM`選取**加入檢視**。 使用下面所顯示的選項。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

選取**新增**，然後將內容取代*Views\StoreManager\EditVM.cshtml*以下列檔案：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM`標記是非常類似於原始`Edit`標記，但有下列例外狀況。

- 模型中的屬性`Edit`檢視是表單的`model.property`(例如， `model.Title` )。 模型中的屬性`EditVm`檢視是表單的`model.Album.property`(例如， `model.Album.Title`)。 這是因為`EditVM`檢視針對傳遞容器`Album`，而非`Album`中`Edit`檢視。
- **DropDownList**第二個參數不是檢視模型， **ViewBag**。
- **BeginForm** helper`EditVM`檢視明確回傳至`Edit`動作方法。 藉由公佈回`Edit`動作，我們不需要撰寫`HTTP POST EditVM`動作也可以重複使用`HTTP POST``Edit`動作。

執行應用程式，並編輯專輯。 將 URL 變更為使用`EditVM`。 變更的欄位，然後點擊**儲存**按鈕，以驗證程式碼可以運作。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>您應該使用哪種方法？

顯示所有的三種方法是 acceptible。 許多開發人員偏好使用至 explictily 階段`SelectList`至`DropDownList`使用`ViewBag`。 這個方法有優點，提供使用更適合的名稱集合的彈性。 一個需要注意的是無法命名`ViewBag SelectList`物件同名的模型屬性。

有些開發人員偏好 ViewModel 方法。 其他考量更詳細的資訊標記和產生的 HTML 的 ViewModel 方法的缺點。

這一節中，我們已經學會使用三種方法**DropDownList**與類別目錄資料。 在下一步 區段中，我們將示範如何新增新的類別。

>[!div class="step-by-step"]
[上一頁](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[下一頁](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
