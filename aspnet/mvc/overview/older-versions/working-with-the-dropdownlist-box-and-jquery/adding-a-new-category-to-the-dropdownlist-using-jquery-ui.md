---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: 將新類別新增至 DropDownList 使用 jQuery UI |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802522"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>將新類別新增至 DropDownList 使用 jQuery UI
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

HTML`Select`標記是適合用來呈現一份固定的類別目錄資料，但通常您需要新增新的類別。 假設我們想要在資料庫中的類別中加入內容類型 」 Opera 」？ 在本節中，我們將使用的 jQuery UI 加入對話方塊中，我們可以使用來新增新的類別。 下圖顯示如何在 UI 將會顯示在瀏覽器中。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

當使用者選取**加入新的內容類型**連結，快顯對話方塊提示使用者輸入新的內容類型名稱 （並選擇性輸入說明）。 下的圖顯示**新增的內容類型**快顯對話方塊。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

輸入新的內容類型名稱時，**儲存**按鈕推送時，會發生下列情況：

1. AJAX 呼叫張貼的內容類型的控制站，這會將新的內容類型儲存至資料庫，並傳回新內容類型的資訊 （「 內容類型名稱 」 和 「 識別碼 」） 為 JSON 的 Create 方法的資料。
2. JavaScript 會將選取清單中的新內容類型的資料。
3. JavaScript 可讓新的內容類型選取的項目。

   在下圖中， **Opera**已加入至資料庫，而在中選取**內容類型**下拉式清單。 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

開啟*Views\StoreManager\Create.cshtml*檔案，並取代為下列內容類型標記的下列程式碼：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

`_ChooseGenre`部分檢視會包含連結的 JavaScript 和 jQuery 用來實作新增新的內容類型功能的所有邏輯。 當我們完成後就會輕鬆地執行相同的演出者 UI 的程式碼。

在 [方案總管] 中，以滑鼠右鍵按一下*Views\StoreManager*資料夾，然後選取**新增**，然後**檢視**。 在 **檢視表名稱**輸入、 輸入`_ChooseGenre`然後選取**新增**。 取代中的標記*Views\StoreManager\\_ChooseGenre.cshtml*以下列檔案：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

第一行會宣告我們傳入`Album`為我們的模型，完全相同模型中的 Create view 陳述式。 接下來的幾行會都**標籤**標記協助程式。 下一行是**DropDownList**協助程式呼叫時，原始的 Create view 完全相同。 下一行加入名為連結`Add New Genre`，並設定它像按鈕的樣式。 包含的列`ValidationMessageFor`複製直接來自建立檢視。 下列幾行：

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

會隱藏的 div 中，建立識別碼為`genreDialog`。 我們會使用 jQuery 來連結我們**新增的內容類型**對話方塊中，識別碼`genreDialog`中這個 div。 最後兩個指令碼標記包含我們將用來實作新增新的內容類型功能的 JavaScript 檔案連結。 */Scripts/chooseGenre.js*檔案是提供為您專案中，我們將檢視在教學課程稍後它。

執行應用程式，然後按一下**加入新的內容類型** 按鈕。 在 **新增的內容類型**對話方塊方塊中，輸入**Opera**中**名稱**輸入的方塊。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

按一下 [儲存] 按鈕。 AJAX 呼叫建立 Opera 類別然後填入下拉式清單和 Opera，並將 Opera 設定為所選取的內容類型。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

輸入演出者、 標題和價格，然後選取**建立** 按鈕。 如果您輸入少於 8.99 美金的價格，新的相簿會出現在頂端的 [索引] 檢視中。 確認新的專輯項目已儲存在資料庫中。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

嘗試建立新的內容類型只能有一個字母。 下列程式碼中*Models\Genre.cs*檔案設定的內容類型名稱的最小和最大長度。

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

用戶端驗證會報告您必須輸入介於 2 到 20 個字元的字串。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>檢查方式的新內容類型加入至資料庫，以及選取清單中。

開啟*Scripts\chooseGenre.js*檔案，並檢查程式碼。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

第二行則會使用識別碼`genreDialog`若要建立對話方塊上的 div 標記*Views\StoreManager\\_ChooseGenre.cshtml*檔案。 具名參數的大部分都是自我說明。 `autoOpen`參數設為 false，選取**建立的內容類型** 按鈕會明確地開啟的對話方塊 （這說明後者上）。 對話方塊有兩個按鈕**儲存**並**取消**。 **取消**按鈕關閉對話方塊。 下列程式碼示範**儲存**按鈕函式。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

`var createGenreForm`從選取`createGenreForm`識別碼。 `createGenreForm`中找到下列程式碼中設定的識別碼*Views\Genre\\_CreateGenre.cshtml*檔案。

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx)中所使用的 helper 多載*Views\Genre\\_CreateGenre.cshtml*檔案會產生與包含 URL 來送出表單的 action 屬性的 HTML。 您可以看到這個瀏覽器中顯示建立的 專輯 頁面，然後選取 瀏覽器中的 顯示來源。 下列標記會顯示包含表單標記產生的 HTML。

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

JQuery`$.post`一行可讓動作屬性的 AJAX 呼叫 (`/StoreManager/Create`)，並傳入的資料**建立的內容類型** 對話方塊。 資料是由新的內容類型和選擇性的描述名稱所組成。 AJAX 呼叫成功時，新的內容類型名稱和值加入選取的標記，與新的內容類型設定為選取的值。 因為這是動態產生的標記，您無法看到新選取的選項，藉由瀏覽器中檢視的來源。 您可以看到新的 HTML 與 IE 9 F12 開發人員工具。 若要檢視新的選取選項，在 Internet Explorer 9 中，按 F12 鍵以啟動 F12 開發人員工具。 瀏覽至 [建立] 頁面，並加入新的內容類型，因此在內容類型選取清單中選取新的內容類型。 在 F12 開發人員工具：

1. 選取 [HTML] 索引標籤。
2. 叫用重新整理圖示。  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. 在 [搜尋] 方塊中，輸入 GenreID。
4. 使用 [下一步] 圖示，   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   瀏覽至下列選取的標記：

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. 展開的最後一個選項值。

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

下列程式碼中*Scripts\chooseGenre.js*檔案會顯示如何**新增新的內容類型** 按鈕取得連線至 click 事件，以及如何**加入新的內容類型**對話方塊建立。

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

第一行會建立附加至按一下函式**加入新的內容類型** 按鈕。 下列標記從 Views\StoreManager\\_ChooseGenre.cshtml 檔案顯示如何**加入新的內容類型**按鈕建立：

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Load 方法會建立並開啟 [新增內容類型] 對話方塊並呼叫 jQuery`parse`方法，讓用戶端驗證，就會發生在對話方塊中輸入的資料。

在本節中，您已經學會如何建立可用來在選取清單中加入新的類別目錄資料 對話方塊。 您可以依照相同的程序建立的演出者選取清單中加入新的演出者的 UI。 本教學課程提供使用 ASP.NET MVC 的 HTML helper 的總覽**DropDownList**。 如需有關使用所需的詳細資訊**DropDownList**，請參閱 [新增參考] 區段下方。 請讓我們知道是否已有幫助本教學課程。

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>其他參考資料

- [ASP.NET MVC – 階層式下拉式清單中列出的教學課程](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx)由[Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [所選](http://harvesthq.github.com/chosen/)JavaScript 外掛程式，可支援多重選取和篩選。

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>檢閱者

- Jean Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike 主教
- Tom Dykstra

> [!div class="step-by-step"]
> [上一步](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
