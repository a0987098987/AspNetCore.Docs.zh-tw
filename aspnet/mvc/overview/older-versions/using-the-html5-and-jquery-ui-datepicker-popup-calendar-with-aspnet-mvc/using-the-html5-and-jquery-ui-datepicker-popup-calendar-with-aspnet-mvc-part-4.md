---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 4 部分 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MV 中的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2b76d2e449d491fd8d808343065b22ba267f1152
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 4 部分
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


### <a name="adding-a-template-for-editing-dates"></a>新增範本進行編輯日期

在本節中，您將建立範本進行編輯時，ASP.NET MVC 會顯示 UI 編輯標示為的模型屬性會套用的日期**日期**列舉[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)relationshipend。 範本會呈現的日期。不會顯示時間。 在範本中，您將使用[jQuery UI 日期選擇器](http://jqueryui.com/demos/datepicker/)提供編輯日期的方式快顯行事曆。

若要開始，請開啟*Movie.cs*檔案，然後加入[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx)屬性附帶**日期**列舉`ReleaseDate`屬性，如下列程式碼所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

此程式碼會造成`ReleaseDate`欄位要顯示的時間，以同時顯示範本，然後編輯範本。 如果您的應用程式包含*date.cshtml*中的範本*Views\Shared\EditorTemplates*資料夾或*Views\Movies\EditorTemplates*資料夾中，該範本將用來呈現任何`DateTime`時編輯的屬性。 否則內建 ASP.NET 樣板化系統會顯示為日期的屬性。

按 CTRL+F5 執行應用程式。 選取 [編輯] 連結來確認發行日期的輸入的欄位會顯示只以日期。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

在**方案總管] 中**，依序展開*檢視*資料夾中，展開 [*共用*資料夾，然後再以滑鼠右鍵按一下*Views\Shared\EditorTemplates*資料夾。

按一下**新增**，然後按一下 **檢視**。 **加入檢視**對話方塊隨即出現。

在**檢視名稱**方塊中，輸入&quot;日期&quot;。

選取**建立成局部檢視**核取方塊。 請確定**使用版面配置頁或主版頁面**和**建立強型別檢視**未選取核取方塊。

按一下 [加入] 。 *Views\Shared\EditorTemplates\Date.cshtml*建立範本。

將下列程式碼加入*Views\Shared\EditorTemplates\Date.cshtml*範本。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

第一行會宣告為模型`DateTime`型別。 雖然您不需要在編輯的模型型別宣告，並顯示範本，最佳作法是，讓您了編譯時間檢查傳遞至檢視的模型。 （另一項優點是在 Visual Studio 中檢視模型然後取得 IntelliSense）。如果未宣告的模型型別，ASP.NET MVC 會將它視為[動態](https://msdn.microsoft.com/en-us/library/dd264741.aspx)輸入，而且沒有任何編譯時間類型檢查。 如果您宣告為模型`DateTime`類型，它會成為強型別。

第二行是常值只會顯示的 HTML 標記&quot;使用日期範本&quot;之前的日期欄位。 若要確認正在使用此日期範本，將暫時使用這一行。

下一行是[Html.TextBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.inputextensions.textbox.aspx) helper 呈現`input`是文字方塊中的欄位。 協助專家的第三個參數設定文字方塊中的類別使用的匿名型別`datefield`並輸入要`date`。 (因為`class`已保留在 C# 中，您必須使用`@`要逸出字元`class`C# 剖析器中的屬性。)

`date`類型是 HTML5 輸入的類型，可讓感知 HTML5 的瀏覽器來呈現 HTML5 行事曆控制項。 稍後您將在其中加入一些 JavaScript 連結至 jQuery 日期選擇器`Html.TextBox`項目使用`datefield`類別。

按 CTRL+F5 執行應用程式。 您可以確認`ReleaseDate`編輯檢視中的屬性會使用編輯範本，因為該範本會顯示&quot;使用日期範本&quot;之前`ReleaseDate`文字輸入方塊中，此映像中所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

在瀏覽器中檢視網頁的來源。 (例如，頁面上按一下滑鼠右鍵，然後選取**檢視原始檔**。)下列範例顯示網頁的標記的部分說明`class`和`type`中呈現的 HTML 屬性。

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

返回*Views\Shared\EditorTemplates\Date.cshtml*範本並移除&quot;使用日期範本&quot;標記。 現在已完成的範本看起來像這樣：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>新增 jQuery UI 日期選擇器快顯行事曆使用 NuGet

本節中您要加入[jQuery UI 日期選擇器](http://jqueryui.com/demos/datepicker/)快顯行事曆日期編輯範本。 [JQuery UI](http://jqueryui.com/)程式庫提供支援動畫，進階的影響，且可自訂的 widget。 它是之上 jQuery JavaScript 程式庫。 日期選擇器快顯行事曆可簡單和自然來輸入日期，而不輸入字串中使用的曆法。 快顯行事曆也會限制使用者合法日期-日期的一般文字項目可讓您輸入類似`2/33/1999`(年 2 月 33rd，1999年)，但[jQuery UI 日期選擇器](http://jqueryui.com/demos/datepicker/)快顯行事曆不允許的。

首先，您必須安裝 jQuery UI 程式庫。 若要這樣做，您將使用 NuGet，是包含在 SP1 版本的 Visual Studio 2010 和 Visual Web Developer 中的封裝管理員。

在 Visual Web Developer 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取 **管理 NuGet 封裝**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

注意： 如果**工具**不會顯示功能表**程式庫套件管理員**命令時，您需要遵循指示安裝 NuGet[安裝 NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)頁面NuGet 網站。   
  
如果您使用 Visual Studio，而不是 Visual Web Developer 中，從**工具**功能表上，選取**程式庫套件管理員**，然後選取 **新增程式庫封裝參考**。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

在**MVCMovie-管理 NuGet 封裝**對話方塊中，按一下 **線上**左側索引標籤，然後輸入&quot;jQuery.UI&quot; 搜尋 方塊中。 選取 j**查詢 UI Widget： 日期選擇器**，然後選取**安裝** 按鈕。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet 會將這些偵錯版本和 jQuery UI 核心和 jQuery UI 日期選擇器縮短的版本加入至您的專案中：

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

注意： 偵錯版本 (不含任何檔案*。 min.js*延伸模組) 適合進行偵錯，但在生產網站中，您會加入縮短的版本。

若要實際使用 jQuery 日期選擇器，您要建立 [行事曆] widget 中編輯範本將連結的 jQuery 指令碼。 在**方案總管 中**，以滑鼠右鍵按一下*指令碼*資料夾，然後選取**新增**，然後**新項目**，然後**JScript檔案**。 將檔案命名*DatePickerReady.js*。

將下列程式碼加入*DatePickerReady.js*檔案：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

如果您不熟悉 jQuery，以下是這個動作的簡短說明： 第一行是&quot;jQuery 準備&quot;函式，當已載入的網頁中的所有 DOM 項目時呼叫。 第二行中選取所有具有類別名稱的 DOM 項目`datefield`，再叫用`datepicker`為每個函式。 (請記住您加入`datefield`類別*Views\Shared\EditorTemplates\Date.cshtml*稍早在本教學課程中的範本。)

接下來，開啟*_layout.cshtml\\_Layout.cshtml*檔案。 您必須將參考加入至下列檔案，也就是所有必要的好讓您可以使用日期選擇器：

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

下列範例顯示的實際程式碼應新增在底部`head`中的項目*_layout.cshtml\\_Layout.cshtml*檔案。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

完整`head`區段如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

[URL 內容的 helper](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.content.aspx)方法會將資源路徑轉換成絕對路徑。 您必須使用`@URL.Content`正確參考這些資源，當應用程式在 IIS 上執行。

按 CTRL+F5 執行應用程式。 選取 [編輯] 連結，然後放到插入點**ReleaseDate**欄位。 會顯示 jQuery UI 快顯行事曆。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

如同大部分的 jQuery 控制項、 日期選擇器可讓您廣泛地加以自訂。 如需資訊，請參閱[視覺自訂： 設計 jQuery UI 佈景主題](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme)上[jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/)站台。

### <a name="supporting-the-html5-date-input-control"></a>支援 HTML5 日期輸入的控制項

如需詳細的瀏覽器支援 HTML5 時，您會想要使用原生的 HTML5 輸入，例如`date`輸入項目，並使用 jQuery UI 行事曆。 您可以將邏輯加入您的應用程式會自動使用 HTML5 控制項，在瀏覽器支援它們。 若要這樣做，取代內容*DatePickerReady.js*以下列檔案：

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

此指令碼的第一行會確認支援 HTML5 日期輸入使用 Modernizr。 如果不受支援，jQuery UI 日期選擇器是改為連結。 ([Modernizr](http://www.modernizr.com/docs/)是偵測到的原生實作的 HTML5 和 CSS3 可用性的開放原始碼 JavaScript 程式庫。 Modernizr 包含在您建立任何新 ASP.NET MVC 專案中。）

進行這項變更之後，您可以使用支援 HTML5，例如 Opera 11 瀏覽器來進行測試。 執行使用 HTML5 相容的瀏覽器的應用程式，並編輯電影項目。 HTML5 日期控制項使用 jQuery UI 快顯行事曆取代：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

因為瀏覽器的新版本以累加方式實作 HTML5，現在，最佳方法是支援的將程式碼加入您的網站，可容納各種 HTML5。 例如，更強固*DatePickerReady.js*指令碼如下所示，可讓您僅部分支援 HTML5 日期控制項的站台支援瀏覽器。

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

此指令碼會選取 HTML5`input`類型的項目`date`未完全支援 HTML5 日期控制項。 這些項目，該連結 jQuery UI 快顯行事曆，然後變更`type`屬性從`date`至`text`。 藉由變更`type`屬性從`date`至`text`，因此可以排除 HTML5 日期的部分支援。 更強固*DatePickerReady.js*指令碼，請參閱[JSFIDDLE](http://jsfiddle.net/XSTK8/15/)。

### <a name="adding-nullable-dates-to-the-templates"></a>加入範本中的 Null 的日期

如果您使用其中一個現有日期範本，並傳遞 null 的日期，您會收到執行階段錯誤。 若要讓更強固的日期範本，您要變更其處理 null 值。 若要支援可為 null 的日期，在變更程式碼*Views\Shared\DisplayTemplates\DateTime.cshtml*如下：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

程式碼會傳回空字串，此模型之後**null**。

在變更程式碼*Views\Shared\EditorTemplates\Date.cshtml*下列檔案：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

當此程式碼執行時，如果模型不是 null，此模型`DateTime`會使用值。 如果模型是 null，就會改為使用目前的日期。

### <a name="wrapup"></a>簡便

本教學課程已涵蓋 ASP.NET 樣板化 helper 的基本概念，並顯示如何在 ASP.NET MVC 應用程式中使用 jQuery UI 日期選擇器快顯行事曆。 如需詳細資訊，請嘗試這些資源：

- 如需當地語系化資訊，請參閱 Rajeesh 的部落格[ASP.NET MVC 中的日期選擇器 JQueryUI](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/)。
- JQuery UI 的相關資訊，請參閱[jQuery UI](http://docs.jquery.com/UI)。
- 如需如何當地語系化日期選擇器控制項的詳細資訊，請參閱[當地語系化/日期選擇器UI/](http://docs.jquery.com/UI/Datepicker/Localization)。
- 如需 ASP.NET MVC 範本的詳細資訊，請參閱 Brad Wilson 的部落格系列[ASP.NET MVC 2 範本](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 雖然 ASP.NET MVC 2 寫入數列時，所需的資料仍適用於目前版本的 ASP.NET MVC。

>[!div class="step-by-step"]
[上一步](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
