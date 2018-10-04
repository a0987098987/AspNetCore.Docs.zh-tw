---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: 使用 DropDownList 協助程式搭配 ASP.NET MVC |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 11b874d2d07c84631c6c5c266c22c6de49d40cf2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578064"
---
<a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>使用 DropDownList 協助程式搭配 ASP.NET MVC
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

本教學課程將教導您使用的基本概念[DropDownList](https://msdn.microsoft.com/library/dd492948.aspx)協助程式和[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) ASP.NET MVC Web 應用程式中的協助程式。 您可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是要遵循本教學課程的 Microsoft Visual Studio 的免費版本。 在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝的必要條件，使用下列連結：

- [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack) <a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）

如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。 本教學課程假設您已完成[ASP.NET MVC 簡介](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程或[ASP.NET MVC Music 市集](../mvc-music-store/mvc-music-store-part-1.md)教學課程中，或您已熟悉 ASP.NET MVC 開發。 本教學課程開頭修改過的專案，從[ASP.NET MVC Music 市集](../mvc-music-store/mvc-music-store-part-1.md)教學課程。 您可以下載入門專案，使用下列連結[下載 C# 版本](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)。

使用本主題隨附了 Visual Web Developer 專案已完成本教學課程 C# 原始程式碼。 [下載](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d)。

### <a name="what-youll-build"></a>您將建置

您將建立動作方法和使用的檢視[DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)選取類別目錄的協助程式。 您也會使用**jQuery**來加入插入類別目錄 對話方塊中，可以在需要新的類別 （例如內容類型或演出者） 時使用。 以下是顯示連結來加入新的內容類型以及加入新的演出者的 [建立] 檢視的螢幕擷取畫面。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- 如何使用[DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx)選取類別目錄資料的協助程式。
- 如何新增**jQuery**對話方塊，即可加入新的類別。

### <a name="getting-started"></a>快速入門

下載入門專案，使用下列連結來啟動[下載](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829)。 在 Windows 檔案總管中，以滑鼠右鍵按一下*DDL\_Starter.zip*檔案，然後選取 [屬性]。 在  **DDL\_Starter.zip 屬性** 對話方塊中，選取 解除封鎖。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

以滑鼠右鍵按一下 DDL\_Starter.zip 檔案，然後選取**全部解壓縮**來解壓縮檔案。 開啟*StartMusicStore.sln* Visual Web Developer 2010 Express （"Visual Web Developer 」 或 「 VWD 」，簡稱） 或 Visual Studio 2010 的檔案。

按 CTRL + F5 執行應用程式，然後按一下**測試**連結。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

選取 **選取影片類別 （簡單）** 連結。 將顯示 Movie 型別選取清單喜劇選取的值。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

以滑鼠右鍵按一下選取 檢視來源與瀏覽器中。 HTML 頁面會顯示。 下列程式碼顯示的 HTML select 元素。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

您可以看到，在選取清單中的每個項目都有值 （動作為 0，1 表示 Drama、 喜劇 2，3 是科幻小說的產物） 和 （動作、 劇本、 喜劇和科幻小說的產物） 的顯示名稱。 上述程式碼是標準 HTML 選取清單。

將選取的清單變更為 Drama 並點擊**送出** 按鈕。 在瀏覽器的 URL 是`http://localhost:2468/Home/CategoryChosen?MovieType=1`和頁面會顯示**您選取： 1**。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

開啟*Controllers\HomeController.cs*檔案，並檢查`SelectCategory`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)協助程式用來建立 HTML 選取清單需要**IEnumerable&lt;SelectListItem &gt;** ，明確或隱含。 也就是說，您可以傳遞**IEnumerable&lt;SelectListItem &gt;** 明確地[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)協助程式，或者您可以新增**IEnumerable&lt;SelectListItem &gt;** 要[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx)使用的相同名稱**SelectListItem**作為模型屬性。 傳入**SelectListItem**隱含和明確涵蓋在本教學課程的下一個部分。 上述程式碼示範最簡單的可能方式，來建立**IEnumerable&lt;SelectListItem &gt;** 並填入文字和值。 附註`Comedy` [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)具有[Selected](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx)屬性設為**true;** 這會導致轉譯的選取清單，顯示**喜劇**清單中選取項目。

**IEnumerable&lt;SelectListItem &gt;** 建立更新版本會新增至[ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) MovieType 同名。 這是我們將傳遞**IEnumerable&lt;SelectListItem &gt;** 隱含為[DropDownList](https://msdn.microsoft.com/library/dd492738.aspx)協助程式如下所示。

開啟*Views\Home\SelectCategory.cshtml*檔案，並檢查的標記。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

在第三行中，我們將設定配置為 Views/Shared/\_簡單\_Layout.cshtml，也就是標準的版面配置檔的簡化的版本。 我們這麼做可以讓顯示，並呈現 HTML 更簡單。

在此範例中我們會不變更狀態的應用程式，讓我們將會提交資料使用**HTTP GET**，而非**HTTP POST**。 請參閱 W3C 一節[快速檢查清單選擇 HTTP GET 或 POST](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist)。 因為我們不會變更應用程式，並張貼的表單，我們使用[Html.BeginForm](https://msdn.microsoft.com/library/dd460344.aspx)多載，可讓我們指定的動作方法、 控制器和表單方法 (**HTTP POST** 或**HTTP GET**)。 檢視通常包含[Html.BeginForm](https://msdn.microsoft.com/library/dd505244.aspx)不接受任何參數的多載。 沒有參數的版本預設為張貼的新版用相同的動作方法和控制器之間的 POST 表單資料。

下面這行

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

傳遞的字串引數**DropDownList**協助程式。 這個字串中，「 MovieType 」，在本例中，會執行兩件事：

- 它提供的索引鍵**DropDownList**協助程式來尋找**IEnumerable&lt;SelectListItem &gt;** 中**ViewBag**。
- 它是資料繫結至 MovieType 表單項目。 送出方法是否**HTTP GET**，`MovieType`會是查詢字串。 送出方法是否**HTTP POST**，`MovieType`將加入訊息主體中。 下圖顯示查詢字串值是 1。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

下列程式碼示範`CategoryChosen`表單提交給方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

瀏覽回到 [測試] 頁面，然後選取**HTML SelectList**連結。 HTML 網頁會呈現類似於簡單的 ASP.NET MVC 測試頁面選取項目。 以滑鼠右鍵按一下 瀏覽器視窗中，選取**檢視原始檔**。 選取清單的 HTML 標記是基本上完全相同。 測試 HTML 頁面上，其運作方式類似的 ASP.NET MVC 動作方法和檢視我們先前所執行的測試。

### <a name="improving-the-movie-select-list-with-enums"></a>提升影片選取清單列舉

如果您的應用程式中的類別固定的而且不會變更，您可以利用更加穩定且更容易擴充，讓您的程式碼的列舉。 當您新增新的類別時，會產生正確的類別目錄值。 當您新增新的類別，但忘記更新類別目錄值時，可避免複製和貼上的錯誤。

開啟*Controllers\HomeController.cs*檔案，並檢查下列程式碼：

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

[Enum](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories`擷取四個 movie 型別。 `SetViewBagMovieType`方法會建立**IEnumerable&lt;SelectListItem &gt;** 從`eMovieCategories`**列舉**，並設定`Selected`屬性`selectedMovie`參數。 `SelectCategoryEnum`動作方法會使用相同的檢視為`SelectCategory`動作方法。

瀏覽至 [測試] 頁面，然後按一下`Select Movie Category (Enum)`連結。 這次，而不是所顯示的值 （數字），表示列舉的字串會顯示。

### <a name="posting-enum-values"></a>張貼的列舉值

HTML 表單通常用來將資料公佈至伺服器。 下列程式碼示範`HTTP GET`並`HTTP POST`新版`SelectCategoryEnumPost`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

藉由傳遞`eMovieCategories`列舉可`POST`方法中，我們可以擷取列舉值和列舉字串。 執行範例，並瀏覽至 [測試] 頁面。 按一下`Select Movie Category(Enum Post)`連結。 選取 movie 型別，然後按下 提交 按鈕。 顯示數值和 movie 型別名稱。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>建立多個區段選取項目

[ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) HTML 協助程式的 HTML 呈現`<select>`項目`multiple`屬性，可讓使用者進行多重選取。 瀏覽至 [測試] 連結，然後選取**多重選取國家/地區**連結。 轉譯的 UI 可讓您選取多個國家/地區。 在下圖中，會選取加拿大和中國。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>檢查 MultiSelectCountry 程式碼

檢查下列程式碼*Controllers\HomeController.cs*檔案。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

`GetCountries`方法會建立一份國家/地區，然後將它傳遞給`MultiSelectList`建構函式。 `MultiSelectList`中使用的建構函式多載`GetCountries`上述方法接受四個參數：

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *項目*: [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx)包含在清單中的項目。 在範例中，清單上方的國家/地區。
2. *dataValueField*： 在屬性名稱**IEnumerable**包含值的清單。 在上面的範例，`ID`屬性。
3. *dataTextField*： 在屬性名稱**IEnumerable**清單，其中包含要顯示的資訊。 在上面的範例，`name`屬性。
4. *selectedValues*： 所選取之值的清單。

在上述範例`MultiSelectCountry`方法傳遞`null`選取國家/地區，就在 UI 顯示時不會選取任何國家/地區的值。 下列程式碼顯示用來呈現的 Razor 標記`MultiSelectCountry`檢視。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

HTML helper [ListBox](https://msdn.microsoft.com/library/dd470200.aspx)方法採用上面兩個參數，為模型繫結屬性的名稱使用， [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx)包含選取的選項和值。 `ViewBag.YouSelected`上述程式碼用來顯示您送出表單時選取的國家/地區的值。 檢查的 HTTP POST 多載`MultiSelectCountry`方法。

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

`ViewBag.YouSelected`動態屬性包含所選的國家/地區，為取得`Countries`表單集合中的項目。 在這一版 GetCountries 方法會傳遞一份所選的國家/地區，因此當`MultiSelectCountry`檢視隨即顯示，在 UI 中選取所選的國家/地區。

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>讓 選取項目易記與 Harvest 選擇 jQuery 外掛程式

Harvest[所選](http://harvesthq.github.com/chosen/)jQuery 外掛程式可以加入至 HTML&lt;選取&gt;元素，來建立使用者易記 UI。 下列影像示範 Harvest[所選](http://harvesthq.github.com/chosen/)jQuery 外掛程式與`MultiSelectCountry`檢視。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

在下列兩個映像**加拿大**已選取。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

在上圖中，選取 加拿大，且其中包含**x**您可以按一下以移除選取項目。 下圖顯示中國、 加拿大和日本選取。

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>連結 Harvest 選擇 jQuery 外掛程式

下一節是如果您有一些經驗，與 jQuery 時需遵循的工作變得更容易。 如果您從未使用過 jQuery 之前，您可以嘗試下列 jQuery 教學課程中的其中一個。

- [JQuery 的運作方式](http://docs.jquery.com/Tutorials:How_jQuery_Works)由[John Resig](http://ejohn.org/)
- [開始使用 jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery)由[Jörn Zaefferer](http://bassistance.de/)
- [即時範例的 jQuery](http://codylindley.com/blogstuff/js/jquery/#)由[Cody Lindley](http://codylindley.com/)

所選外掛程式納入的 starter 和已完成的範例專案隨附此教學課程。 本教學課程中您只需要使用 jQuery UI 將它連結。 若要使用的 Harvest 選擇 jQuery 外掛程式在 ASP.NET MVC 專案中，您必須：

1. 下載從所選外掛程式[github](https://github.com/harvesthq/chosen/)。 此步驟中已為您已完成。
2. ASP.NET MVC 專案中加入所選資料夾。 新增從您在上一個步驟所選資料夾中下載的所選外掛程式的資產。 此步驟中已為您已完成。
3. 連結至所選外掛程式**DropDownList**或是**ListBox** HTML 協助程式。

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>連結至 MultiSelectCountry 檢視所選外掛程式。

開啟*Views\Home\MultiSelectCountry.cshtml*檔案，並新增`htmlAttributes`參數來`Html.ListBox`。 您將加入的參數包含選取清單的類別名稱 (`@class = "chzn-select"`)。 已完成的程式碼如下所示：

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

在上述程式碼，我們新增的 HTML 屬性和屬性值`class = "chzn-select"`。 \@上述的類別具有與 Razor 檢視引擎無關的字元。 `class` 已[C# 關鍵字](https://msdn.microsoft.com/library/x53a06bb.aspx)。 不能當做識別項使用 C# 關鍵字，除非它們包含\@做為前置詞。 在上述範例`@class`有效的識別項，但**類別**不是因為**類別**是關鍵字。

將參考加入至*Chosen/chosen.jquery.js*並*Chosen/chosen.css*檔案。 *Chosen/chosen.jquery.js*並實作所選外掛程式的功能。 *Chosen/chosen.css*檔案提供的樣式。 新增這些參考到底部*Views\Home\MultiSelectCountry.cshtml*檔案。 下列程式碼示範如何參考所選外掛程式。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

啟用使用類別名稱中使用的所選外掛程式**Html.ListBox**程式碼。 在上述範例中，類別名稱是`chzn-select`。 將下行新增至底部*Views\Home\MultiSelectCountry.cshtml*檢視檔案。 這一行就會啟動所選外掛程式。

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

下面這一行會呼叫 jQuery 就緒功能，選取 使用類別名稱的 DOM 項目語法`chzn-select`。

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

已包裝的設定從上述的呼叫傳回則適用於所選的方法 (`.chosen();`)，所選外掛程式的連結。

下列程式碼顯示已完成*Views\Home\MultiSelectCountry.cshtml*檢視檔案。

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

執行應用程式，並瀏覽至`MultiSelectCountry`檢視。 嘗試新增和刪除國家/地區。 提供的範例下載也包含`MultiCountryVM`方法，並實作 MultiSelectCountry 功能使用檢視的檢視模型而非**ViewBag**。

在下一節中您會看到與 ASP.NET MVC scaffolding 機制的運作方式**DropDownList**協助程式。

> [!div class="step-by-step"]
> [下一步](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
