---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "將新的分類加入至使用 jQuery UI DropDownList |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: de661616ff3ca83052ae74d3ae6810d014aff764
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>將新的分類加入至使用 jQuery UI 的 DropDownList
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

HTML`Select`標記非常適合用來呈現一份固定的類別目錄資料，但通常您需要加入新的類別。 假設我們想要在資料庫中的類別中加入內容類型 」 Opera 」？ 在本節中，我們將使用 jQuery UI，加入對話方塊中，我們可以使用來新增新的類別。 下圖顯示 UI 會出現在瀏覽器內的方式。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

當使用者選取**加入新的內容類型**連結，快顯對話方塊提示使用者輸入新的內容類型名稱 （以及選擇性描述）。 下面顯示的映像**新增內容類型**快顯對話方塊。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

輸入新的內容類型名稱時，**儲存**按鈕推入，會發生下列狀況：

1. AJAX 呼叫會建立方法的內容類型的控制站，會將新的內容類型儲存至資料庫，並傳回新類型的資訊 （內容類型名稱和 ID） 做為 JSON 資料。
2. JavaScript 會將新的內容類型資料加入至選取的清單。
3. JavaScript 可讓新的內容類型選取項目。

 在下圖， **Opera**已加入至資料庫，而在中選取**類型**下拉式清單。 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

開啟*Views\StoreManager\Create.cshtml*檔案，並取代為下列內容類型標記的下列程式碼：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre`部分檢視將包含所有的邏輯以 JavaScript 和 jQuery 用來實作加入新的內容類型功能相連結。 我們完成之後會很簡單，執行相同的演出者 UI 的程式碼。

在 [方案總管] 中，以滑鼠右鍵按一下*Views\StoreManager*資料夾，然後選取**新增**，然後**檢視**。 在**檢視名稱**輸入、 輸入`_ChooseGenre`然後選取**新增**。 取代中的標記*Views\StoreManager\\_ChooseGenre.cshtml*以下列檔案：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

第一行會宣告我們中傳遞`Album`我們的模型，以完全相同模型中建立檢視中找到的陳述式。 接下來的幾個幾行是**標籤**helper 標記。 下一行是**DropDownList**呼叫 helper，與原始建立檢視完全相同。 下一行會加入名稱連結`Add New Genre`，和樣式和按鈕相同。 列包含`ValidationMessageFor`複製直接從建立檢視。 下列幾行：

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

建立隱藏的 div，識別碼為`genreDialog`。 我們將使用 jQuery 連結我們**新增內容類型**對話方塊識別碼`genreDialog`中此 div. 最後兩個指令碼標記包含我們會使用實作加入新的內容類型功能的 JavaScript 檔案連結。 */Scripts/chooseGenre.js*檔案是為了讓您在專案中，我們會檢查稍後在本教學課程中提供。

執行應用程式，然後按一下**加入新的內容類型** 按鈕。 在**新增內容類型**對話方塊方塊中，輸入**Opera**中**名稱**輸入的方塊。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

按一下**儲存** 按鈕。 AJAX 呼叫會建立 作業類別目錄，然後填入 Opera 的下拉式清單，然後將 Opera 設定為選取的內容類型。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

輸入演出者、 標題和價格，然後選取**建立** 按鈕。 如果您輸入價格不超過 $8.99，新的相簿會出現在索引檢視的頂端。 確認新的專輯項目已儲存在資料庫中。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

請先嘗試建立新的內容類型只能有一個字母。 下列程式碼中*Models\Genre.cs*檔案設定內容類型名稱的最小和最大長度。

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

用戶端驗證報告您必須輸入介於 2 到 20 個字元的字串。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>檢查方式的新內容類型加入至資料庫和選取清單中。

開啟*Scripts\chooseGenre.js*檔案，並檢查程式碼。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

第二行則會使用識別碼`genreDialog`上的 div 標記中建立對話方塊*Views\StoreManager\\_ChooseGenre.cshtml*檔案。 大部分的具名參數都是本身的說明。 `autoOpen`參數設定為 false，選取**建立內容類型**按鈕將會明確地開啟對話方塊 （這會描述後者上）。 對話方塊有兩個按鈕，**儲存**和**取消**。 **取消**按鈕關閉對話方塊。 下列程式碼會示範**儲存**按鈕函式。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm`選取從`createGenreForm`識別碼。 `createGenreForm`中找到下列程式碼中設定的識別碼*Views\Genre\\_CreateGenre.cshtml*檔案。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx)協助程式中使用的多載*Views\Genre\\_CreateGenre.cshtml*檔案產生 HTML，以包含 URL 送出表單的 action 屬性。 您可以看到此瀏覽器中顯示建立專輯頁面，然後在瀏覽器中選取顯示來源。 下列標記會顯示產生包含表單標記的 HTML。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery`$.post`一行可讓動作屬性的 AJAX 呼叫 (`/StoreManager/Create`)，並將資料中傳遞**建立內容類型** 對話方塊。 資料是由新的內容類型以及選擇性的描述名稱所組成。 AJAX 呼叫成功時，新的內容類型名稱和值加入至選取的標記，以及新的內容類型設定為選取的值。 因為這是動態產生的標記，您看新的選取選項藉由瀏覽器中檢視來源。 您可以看到新的 HTML 使用 IE 9 F12 開發人員工具。 若要檢視新的選取選項，在 Internet Explorer 9 中，按 F12 鍵，即可啟動 F12 開發人員工具。 巡覽至 [建立] 頁面，並加入新的內容類型，以便在內容類型選取清單中選取新的內容類型。 在 F12 開發人員工具：

1. 選取 [HTML] 索引標籤。
2. 叫用重新整理圖示。  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. 在 [搜尋] 方塊中，輸入 GenreID。
4. 使用下一個圖示，   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 瀏覽至下列選取的標記：

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. 展開的最後一個選項值。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

下列程式碼中*Scripts\chooseGenre.js*檔會示範如何**加入新的內容類型** 按鈕取得連接至 click 事件，以及如何**加入新的內容類型**對話方塊建立。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

第一行會建立附加至按一下函數**加入新的內容類型** 按鈕。 下列標記的來源 Views\StoreManager\\_ChooseGenre.cshtml 檔案顯示如何**加入新的內容類型**按鈕建立：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load 方法會建立並開啟 [新增內容類型] 對話方塊，呼叫 jQuery`parse`方法，讓用戶端驗證，就會發生在對話方塊中輸入的資料。

本節中，您已經學會如何建立可用於選取清單中加入新的類別目錄資料 對話方塊。 您可以遵循相同的程序來建立新的演出者加入演出者選取清單的 UI。 本教學課程已授與使用 ASP.NET MVC 的 HTML helper 的概觀**DropDownList**。 如需有關使用**DropDownList**，請參閱 [加入參考] 區段下方。 請讓我們知道是否本教學課程已幫助。

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>其他參考資料

- [ASP.NET MVC – 階層式下拉式清單中列出的教學課程](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)由[Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [所選](http://harvesthq.github.com/chosen/)JavaScript 外掛程式支援多重選取和篩選。

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>檢閱者

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike 教宗
- Tom Dykstra

>[!div class="step-by-step"]
[上一步](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
