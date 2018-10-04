---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 4 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7ecd180b7608e82ea143575c6590574b92843dcf
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577492"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 4 部分
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


### <a name="adding-a-template-for-editing-dates"></a>新增範本進行編輯日期

在本節中，您將建立範本，以進行編輯時，ASP.NET MVC 會顯示 UI，來編輯模型屬性以標記會套用的日期**日期**列舉[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性。 範本會呈現的日期;不會顯示時間。 在範本中，您將使用[jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/)地編輯日期的快顯行事曆。

若要開始，開啟*Movie.cs*檔案，並新增[資料類型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性搭配**日期**列舉`ReleaseDate`屬性，如下列程式碼所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

此程式碼會造成`ReleaseDate`的時間，以同時顯示範本，並編輯範本沒有要顯示的欄位。 如果您的應用程式含有*date.cshtml*中的範本*Views\Shared\EditorTemplates*資料夾或*Views\Movies\EditorTemplates*資料夾中，該範本將用來呈現任何`DateTime`時編輯的屬性。 否則內建的 ASP.NET 範本化系統會顯示為日期的屬性。

按 CTRL+F5 執行應用程式。 選取 [編輯] 連結來確認發行日期輸入的欄位會顯示只有表示日期。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

在 [**方案總管] 中**，展開*檢視*資料夾中，展開*共用*資料夾，然後再以滑鼠右鍵按一下*Views\Shared\EditorTemplates*資料夾。

按一下 **新增**，然後按一下**檢視**。 **加入檢視**對話方塊隨即出現。

在 **檢視表名稱**方塊中，輸入&quot;日期&quot;。

選取 **建立成局部檢視**核取方塊。 請確定**使用版面配置頁或主版頁面**並**建立強型別檢視**未選取核取方塊。

按一下 [加入] 。 *Views\Shared\EditorTemplates\Date.cshtml*建立範本。

將下列程式碼加入*Views\Shared\EditorTemplates\Date.cshtml*範本。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

第一行會宣告為模型`DateTime`型別。 雖然您不需要編輯中的模型型別宣告，並顯示範本，它是最佳的作法，讓您取得編譯時間檢查傳遞至檢視的模型。 （另一個優點是您再取得在 Visual Studio 中檢視模型的 IntelliSense）。如果未宣告的模型型別，ASP.NET MVC 會將它視為[動態](https://msdn.microsoft.com/library/dd264741.aspx)輸入，而且沒有任何編譯時期類型檢查。 如果您宣告模型`DateTime`型別，它會成為強型別。

第二行是常值只會顯示的 HTML 標記&quot;使用的日期範本&quot;之前的日期欄位。 您將暫時使用這一行，來確認正在使用此日期範本。

下一行是[Html.TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx)呈現的協助程式`input`是文字方塊中的欄位。 協助程式的第三個參數會使用匿名型別來設定文字方塊中的類別`datefield`並輸入要`date`。 (因為`class`會保留在 C# 中，您需要使用`@`字元來逸出`class`C# 剖析器中的屬性。)

`date`型別是 HTML5 輸入的類型，可讓感知 HTML5 的瀏覽器來呈現 HTML5 行事曆控制項。 稍後您將在其中加入一些 JavaScript，以連結到 jQuery datepicker`Html.TextBox`項目使用`datefield`類別。

按 CTRL+F5 執行應用程式。 您可以確認`ReleaseDate`編輯檢視中的屬性使用編輯範本，因為範本會顯示&quot;使用日期範本&quot;之前`ReleaseDate`文字輸入方塊中，此映像所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

在瀏覽器中檢視頁面的來源。 (例如，以滑鼠右鍵按一下  頁面上，並選取**檢視原始檔**。)下列範例示範一些標記頁面，說明`class`和`type`中轉譯的 HTML 屬性。

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

返回*Views\Shared\EditorTemplates\Date.cshtml*範本並移除&quot;使用日期範本&quot;標記。 現在已完成的範本看起來像這樣：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>新增 jQuery UI Datepicker 快顯行事曆使用 NuGet

在本節中，您將新增[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)快顯行事曆日期編輯範本。 [JQuery UI](http://jqueryui.com/)動畫，進階的效果，以及可自訂的小工具程式庫提供支援。 它是以 jQuery JavaScript 程式庫所建置的。 Datepicker 快顯行事曆能夠以簡單且自然的輸入而不輸入字串中使用的行事曆的日期。 快顯行事曆也會限制使用者合法的日期，日期的一般文字項目可讓您輸入類似`2/33/1999`(年 2 月 33rd，1999年)，但[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)快顯行事曆不允許的。

首先，您必須安裝的 jQuery UI 程式庫。 若要這樣做，您將使用 NuGet，也就是包含在 Visual Studio 2010 和 Visual Web Developer 的 SP1 版本的套件管理員。

在 Visual Web Developer 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**管理 NuGet 套件**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

注意： 如果**工具**不會顯示功能表**程式庫套件管理員**命令，您必須依照的指示安裝 NuGet[安裝 NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)頁面NuGet 網站。   
  
如果您使用 Visual Studio，而不是 Visual Web Developer 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取**新增程式庫封裝參考**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

在 [ **MVCMovie-管理 NuGet 套件**] 對話方塊中，按一下**線上**左側索引標籤，然後輸入&quot;jQuery.UI&quot;在搜尋方塊中。 選取 [j**查詢 UI Widget: Datepicker**，然後選取**安裝**] 按鈕。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet 會將這些偵錯版本和縮製的版本的 jQuery UI 核心和 jQuery UI 日期選擇器新增至您的專案：

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

注意： 偵錯版本 (不含任何檔案 *。 min.js*擴充功能) 適合進行偵錯，但在生產網站中，您會在加入縮製的版本。

若要實際使用 jQuery 日期選擇器，您需要建立行事曆小工具，來編輯範本將會連結 jQuery 指令碼。 在 [**方案總管] 中**，以滑鼠右鍵按一下*指令碼*資料夾，然後選取**新增**，然後**新項目**，，然後**JScript檔案**。 將檔案命名*DatePickerReady.js*。

將下列程式碼加入*DatePickerReady.js*檔案：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

如果您不熟悉 jQuery，以下是這項功能的簡短說明： 第一行是&quot;jQuery 準備&quot;函式，在網頁中的所有 DOM 項目已都載入時呼叫。 第二行中選取所有具有類別名稱的 DOM 項目`datefield`，然後叫用`datepicker`為每個函式。 (請記住，您會加入`datefield`類別，即可*Views\Shared\EditorTemplates\Date.cshtml*稍早在本教學課程中的範本。)

接下來，開啟*Views\Shared\\_Layout.cshtml*檔案。 您需要將參考加入至下列檔案，也就是所有必要的好讓您可以使用日期選擇器：

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

下列範例顯示的實際程式碼，您應該新增底部`head`中的項目*Views\Shared\\_Layout.cshtml*檔案。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

完整`head`區段如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL 內容 helper](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx)方法會將資源路徑轉換為絕對路徑。 您必須使用`@URL.Content`正確地參考這些資源在 IIS 上執行應用程式。

按 CTRL+F5 執行應用程式。 選取 [編輯] 連結，然後將插入點放**ReleaseDate**欄位。 JQuery UI 快顯行事曆會顯示。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

大部分的 jQuery 控制項，例如 datepicker 可讓您廣泛地加以自訂。 如需資訊，請參閱[視覺自訂： 設計的 jQuery UI 佈景主題](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上[的 jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)站台。

### <a name="supporting-the-html5-date-input-control"></a>支援 HTML5 的日期輸入的控制項

更多瀏覽器支援 HTML5，您會想使用原生 HTML5 輸入，例如`date`輸入項目，並使用 jQuery UI 行事曆。 您可以將邏輯加入您的應用程式會自動使用 HTML5 控制項，如果瀏覽器支援它們。 若要這樣做，請將取代的內容*DatePickerReady.js*以下列檔案：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

此指令碼的第一行會使用 Modernizr，確認支援 HTML5 日期輸入。 如果不受支援，jQuery UI 日期選擇器會改為連結。 ([Modernizr](http://www.modernizr.com/docs/)一個開放原始碼 JavaScript 程式庫，可偵測的 HTML5 和 CSS3 的原生實作的可用性。 Modernizr 已隨附於您建立任何新 ASP.NET MVC 專案中）。

您進行這項變更之後，您可以使用瀏覽器支援 HTML5，例如 Opera 11 來進行測試。 執行使用 HTML5 相容的瀏覽器的應用程式，並編輯電影項目。 會使用 HTML5 日期控制項，而不是 jQuery UI 快顯行事曆：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

因為瀏覽器的新版本以累加方式實作 HTML5，現在是不錯的方法是將程式碼新增至您所能容納的 HTML5 支援各種不同的網站。 比方說，更強固*DatePickerReady.js*指令碼如下所示，可讓您僅部分支援 HTML5 日期控制項的站台支援瀏覽器。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

此指令碼會選取 HTML5`input`類型的項目`date`未完全支援 HTML5 日期控制項。 針對這些項目，其連結的 jQuery UI 快顯行事曆，然後變更`type`屬性從`date`至`text`。 藉由變更`type`屬性從`date`到`text`，也會刪除 HTML5 日期的部分支援。 更加強固*DatePickerReady.js*指令碼，請參閱[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)。

### <a name="adding-nullable-dates-to-the-templates"></a>加入範本中的可為 Null 的日期

如果您使用其中一個現有的日期範本，並傳遞 null 的日期，您會收到執行階段錯誤。 若要進行更穩固的日期範本，您會將它們來處理 null 值。 若要支援可為 null 的日期，將變更中的程式碼*Views\Shared\DisplayTemplates\DateTime.cshtml*如下：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

程式碼會傳回空字串，當模型**null**。

變更中的程式碼*Views\Shared\EditorTemplates\Date.cshtml*下列檔案：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

此程式碼執行時，該模型不是 null，如果模型的`DateTime`會使用值。 如果模型是 null，就會改為使用目前的日期。

### <a name="wrapup"></a>簡便

本教學課程涵蓋 ASP.NET 樣板化 helper 的基本概念，並示範如何使用 ASP.NET MVC 應用程式中的 jQuery UI datepicker 快顯行事曆。 如需詳細資訊，請嘗試這些資源：

- 如需當地語系化資訊，請參閱 Rajeesh 的部落格[ASP.NET MVC 中的 JQueryUI Datepicker](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)。
- JQuery UI 的相關資訊，請參閱[jQuery UI](http://docs.jquery.com/UI)。
- 如需如何當地語系化 datepicker 控制項的詳細資訊，請參閱[UI/Datepicker/當地語系化](http://docs.jquery.com/UI/Datepicker/Localization)。
- 如需有關 ASP.NET MVC 範本的詳細資訊，請參閱 Brad Wilson 的部落格系列[ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 雖然系列撰寫 ASP.NET MVC 2，資料仍適用於目前版本的 ASP.NET MVC。

> [!div class="step-by-step"]
> [上一步](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
