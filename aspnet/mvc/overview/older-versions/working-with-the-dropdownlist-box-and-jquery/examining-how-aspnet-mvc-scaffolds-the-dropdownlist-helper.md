---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: 檢查 ASP.NET MVC 如何 scaffold DropDownList 協助程式 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 8e407d5426ed93388b3c6f7bdb125be5356c002c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837126"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>檢查 ASP.NET MVC 如何 scaffold DropDownList 協助程式
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

在 **方案總管**，以滑鼠右鍵按一下*控制站*資料夾，然後選取**新增控制器**。 控制器**StoreManagerController**。 設定的選項**新增控制器**如下圖所示的對話方塊。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

編輯*StoreManager\Index.cshtml*檢視和移除`AlbumArtUrl`。 移除`AlbumArtUrl`會讓呈現更容易閱讀。 已完成的程式碼如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

開啟*Controllers\StoreManagerController.cs*檔案，並尋找`Index`方法。 新增`OrderBy`子句，將價格依排序的相簿。 完整的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

價格依排序會讓您更輕鬆地測試資料庫的變更。 當您要測試的編輯，並建立方法時，您可以使用較低的價格，因此已儲存的資料會出現在第一次。

開啟*StoreManager\Edit.cshtml*檔案。 下面這一行只之後加入圖例標籤。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

下列程式碼顯示這項變更的內容：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

`AlbumId` ，才可變更的專輯記錄。

按 CTRL+F5 執行應用程式。 選取即可**系統管理員**連結，然後選取**建立新**連結，以建立新的相簿。 請確認已儲存的專輯資訊。 編輯專輯，並確認您所做的變更會保存。

### <a name="the-album-schema"></a>專輯結構描述

`StoreManager` MVC scaffolding 機制所建立的控制站將允許相簿的 CRUD （Create、 Read、 Update、 Delete） 存取 「 音樂市集 」 資料庫中。 專輯資訊的結構描述如下所示：

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

`Albums`相簿的內容類型和描述不會儲存資料表，其中存放了外部索引鍵`Genres`資料表。 `Genres`資料表包含的內容類型名稱和描述。 同樣地，`Albums`專輯演出者名稱，但的外部索引鍵，不包含資料表`Artists`資料表。 `Artists`資料表包含演出者的名稱。 如果您檢查中的資料`Albums`資料表中，您可以看到每個資料列包含外部索引鍵`Genres`資料表和外部索引鍵`Artists`資料表。 下圖顯示一些資料表資料從`Albums`資料表。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>HTML 標記

HTML`<select>`項目 (由 HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)協助程式) 用來顯示值 （例如內容類型清單） 的完整清單。 編輯表單時已知的目前值，則選取清單可以顯示目前的值。 我們先前所見此當我們選取的值設定為**喜劇**。 選取的清單很適合用於顯示類別目錄或外部索引鍵資料。 `<select>`內容類型的外部索引鍵的項目會顯示可能的內容類型名稱的清單，但當您儲存表單的內容類型屬性已包含內容類型外部索引鍵值，而不是顯示的內容類型名稱。 下圖中所選取的內容類型是**Disco**演出者，而且**Donna 夏天**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>檢查 ASP.NET MVC 包含 Scaffold 的程式碼

開啟*Controllers\StoreManagerController.cs*檔案，並尋找`HTTP GET Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

`Create`方法會將兩個[SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx)物件至`ViewBag`，其中包含內容類型的資訊，另一種包含演出者資訊。 [SelectList](https://msdn.microsoft.com/library/dd505286.aspx)上面使用的建構函式多載會採用三個引數：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含在清單中的項目。 在上述範例中，內容類型的清單傳回`db.Genres`。
2. *dataValueField*： 在屬性名稱**IEnumerable**清單，其中包含索引鍵的值。 在上述範例`GenreId`和`ArtistId`。
3. *dataTextField*： 在屬性名稱**IEnumerable**清單，其中包含要顯示的資訊。 演出者和內容類型表格中`name`使用欄位。

開啟*Views\StoreManager\Create.cshtml*檔案，並檢查`Html.DropDownList`協助程式類型欄位的標記。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

第一行會顯示建立檢視採用`Album`模型。 在 `Create`方法如上所示，不傳遞任何模型，讓檢視取得**null** `Album`模型。 此時，我們會建立新專輯因此我們沒有任何`Album`為它的資料。

[Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)如上所示的多載會採用要繫結至模型之欄位的名稱。 它也會使用此名稱來尋找**ViewBag**物件，包含[SelectList](https://msdn.microsoft.com/library/dd505286.aspx)物件。 使用這個多載，您都必須名稱**ViewBag SelectList**物件`GenreId`。 第二個參數 (`String.Empty`) 是用來在不選取任何項目時顯示文字。 這正是我們要建立新專輯時。 如果您移除第二個參數，並使用下列程式碼：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

選取清單會預設為第一個項目或 Rock，在我們的範例。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

檢查`HTTP POST Create`方法。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

這個多載`Create`方法會採用`album`張貼的表單值的 ASP.NET MVC 模型繫結系統所建立的物件。 當您送出新的相簿中，如果模型狀態無效，而且沒有任何資料庫錯誤時，新的專輯新增資料庫。 下圖顯示建立新的相簿。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

您可以使用[fiddler 工具](http://www.fiddler2.com/fiddler2/)來檢查已張貼的表單值 ASP.NET MVC 模型繫結會使用建立專輯物件。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>重構 ViewBag SelectList 建立

這兩個`Edit`方法和`HTTP POST Create`方法有相同的程式碼，以設定**SelectList**中**ViewBag**。 精神[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself)，我們將會重構此程式碼。 我們要使用這個重構程式碼更新版本。

建立新的方法來新增內容類型與演出者**SelectList**要**ViewBag**。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

取代設定的兩個線條`ViewBag`中每個`Create`並`Edit`方法呼叫`SetGenreArtistViewBag`方法。 已完成的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

建立新的專輯和編輯以確認工作變更專輯。

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>明確地將 SelectList 傳遞至 DropDownList

建立 ASP.NET MVC scaffolding 使用下列的建立] 和 [編輯檢視**DropDownList**多載：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

`DropDownList`標記建立檢視如下所示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

因為`ViewBag`屬性`SelectList`稱為`GenreId`，則**DropDownList**協助程式會使用`GenreId` **SelectList**中**ViewBag**. 在下列**DropDownList**多載，`SelectList`明確傳遞。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

開啟*Views\StoreManager\Edit.cshtml*檔案，然後變更**DropDownList**呼叫中明確地傳遞**SelectList**，使用上述的多載。 執行這項操作的內容類型分類。 已完成的程式碼如下所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

執行應用程式，然後按一下**系統管理員**連結，然後瀏覽至 Jazz 專輯，並選取**編輯**連結。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

而是顯示 Jazz 目前選取的內容類型為 Rock 隨即出現。 當字串引數 （要繫結的屬性） 和**SelectList**物件具有相同的名稱，不會使用選取的值。 中的第一個項目未提供的選取的值時，預設瀏覽器**SelectList**(即**Rock**在上述範例中)。 這是已知的限制**DropDownList**協助程式。

開啟*Controllers\StoreManagerController.cs*檔案，並變更**SelectList**物件名稱來`Genres`和`Artists`。 已完成的程式碼如下所示：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

內容類型和演出者名稱是類別，更好的名稱，因為它們包含不只是每個類別目錄的識別碼。 重構的稍早執行過了成果。 而不是變更**ViewBag**四種方法，在我們的變更已隔離至`SetGenreArtistViewBag`方法。

變更**DropDownList**呼叫中建立和編輯檢視，以使用新**SelectList**名稱。 [編輯] 檢視的新標記如下所示：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

建立檢視需要空字串，以防止 SelectList 中的第一個項目顯示。

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

建立新的專輯和編輯以確認工作變更專輯。 測試編輯程式碼，藉由選取與內容類型 Rock 以外的專輯。

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>使用 DropDownList 協助程式中的檢視模型

建立新的類別中名為的 Viewmodel 資料夾`AlbumSelectListViewModel`。 中的程式碼取代`AlbumSelectListViewModel`取代為下列類別：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

`AlbumSelectListViewModel`建構函式會專輯、 演出者和內容類型的清單，並建立物件，其中包含專輯和`SelectList`的內容類型和演出者名稱。

建置專案因此`AlbumSelectListViewModel`時才可以使用我們在下一個步驟中建立檢視。

新增`EditVM`方法，以`StoreManagerController`。 已完成的程式碼如下所示。

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

以滑鼠右鍵按一下`AlbumSelectListViewModel`，選取**解決**，然後**使用 MvcMusicStore.ViewModels;**。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

或者，您可以在其中新增下列 using 陳述式：

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

以滑鼠右鍵按一下`EditVM`，然後選取**加入檢視**。 使用如下所示的選項。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

選取 **新增**，然後取代內容*Views\StoreManager\EditVM.cshtml*以下列檔案：

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

`EditVM`標記是非常類似於原始`Edit`標記，但有下列例外狀況。

- 模型中的屬性`Edit`檢視會在表單`model.property`(比方說， `model.Title` )。 模型中的屬性`EditVm`檢視會在表單`model.Album.property`(比方說， `model.Album.Title`)。 這是因為`EditVM`檢視會傳遞一個容器進行`Album`，而非`Album`示`Edit`檢視。
- **DropDownList**第二個參數不是檢視模型中，來自**ViewBag**。
- **BeginForm**中的 helper`EditVM`檢視明確回傳至`Edit`動作方法。 藉由公佈回到`Edit`動作，我們不需要撰寫`HTTP POST EditVM`動作，並可以重複使用`HTTP POST``Edit`動作。

執行應用程式，並編輯 album。 將 URL 變更為使用`EditVM`。 變更的欄位，然後按**儲存** 按鈕，確認程式碼是否正常運作。

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>您應該使用哪種方法？

顯示的三個方法都是不得。 許多開發人員偏好 explictily pass`SelectList`要`DropDownList`使用`ViewBag`。 這個方法有新增的好處，是讓您彈性地使用更適當的名稱集合。 一個需要注意的是您不能命名`ViewBag SelectList`物件模型的屬性名稱相同。

有些開發人員偏好使用 ViewModel 方法。 其他人請考慮更詳細的標記，並產生 HTML ViewModel 方法的缺點。

在本節中我們已了解使用三種方法**DropDownList**與類別目錄資料。 在下一步 區段中，我們將示範如何新增新的類別。

> [!div class="step-by-step"]
> [上一頁](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [下一頁](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
