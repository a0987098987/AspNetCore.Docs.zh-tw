---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: "使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 2 部分 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MV 中的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 69fbaa7761c97895ffee770f6feb9ce6b745d186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 2 部分
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


## <a name="adding-an-automatic-datetime-template"></a>新增自動的日期時間範本

在本教學課程的第一個部分，您已看到如何將屬性加入至模型，以明確指定的格式，以及如何明確地指定用來呈現模型的範本。 例如， [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性，在下列程式碼明確指定的格式`ReleaseDate`屬性。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

在下列範例中， [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性，使用`Date`列舉型別，可讓您指定日期範本，應該用來呈現模型。 如果您的專案沒有日期範本，則會使用內建日期範本。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

不過，ASP。MVC 可以執行型別比對使用慣例移轉組態，藉由尋找符合型別名稱的範本。 這可讓您建立的範本，而不需要完全使用任何屬性或程式碼，自動格式化資料。 此部分的教學課程中，您將建立會自動套用至類型的模型內容的範本[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 您不需要使用屬性或其他設定來指定應該使用的範本來呈現所有類型的模型內容[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。

您也將學習來自訂個別屬性或甚至個別欄位的顯示方式。

若要開始，讓我們先移除現有的格式資訊，並在應用程式中顯示完整日期。

開啟*Movie.cs*檔案，並標記為註解`DataType`屬性`ReleaseDate`屬性：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

按 CTRL+F5 執行應用程式。

請注意，`ReleaseDate`屬性現在會顯示日期和時間，因為這是預設值，不提供任何格式設定資訊時。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>加入 CSS 樣式，用於測試新的範本

您建立的範本來格式化日期之前，您要加入幾個 CSS 樣式規則，您可以套用到新的範本。 這些會協助您確認呈現的網頁使用新的範本。

開啟*Content\Site.cs*s 檔案，並將下列 CSS 規則加入至檔案最下方：

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>加入日期時間的顯示範本

現在您可以建立新的範本。 在*Views\Movies*資料夾中，建立*DisplayTemplates*資料夾。

在*_layout.cshtml*資料夾中，建立*DisplayTemplates*資料夾和*EditorTemplates*資料夾。

中的顯示範本*Views\Shared\DisplayTemplates*資料夾將會由所有控制器。 中的顯示範本*Views\Movie\DisplayTemplates*資料夾將只使用`Movie`控制站。 (如果具有相同名稱的範本會出現在這兩個的資料夾中的範本*Views\Movie\DisplayTemplates*資料夾-也就是更具體的範本 — 檢視所傳回的優先`Movie`控制站。)

在**方案總管] 中**，依序展開*檢視*資料夾中，展開 [*共用*資料夾，然後再以滑鼠右鍵按一下*Views\Shared\DisplayTemplates*資料夾。

按一下**新增**，然後按一下 **檢視**。 **加入檢視**對話方塊隨即出現。

在**檢視名稱**方塊中，輸入`DateTime`。 （您必須使用此名稱才能符合型別名稱）。

選取**建立成局部檢視**核取方塊。 請確定**使用版面配置頁或主版頁面**和**建立強型別檢視**未選取核取方塊。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

按一下 [加入] 。 A *DateTime.cshtml*中建立範本*Views\Shared\DisplayTemplates*。

下圖顯示*檢視*資料夾中的**方案總管 中**之後`DateTime`建立顯示和編輯程式的範本。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

開啟*Views\Shared\DisplayTemplates\DateTime.cshtml*檔案，然後加入下列標記，會使用[String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)屬性格式化為不含時間日期的方法。 (`{0:d}`格式指定簡短日期格式。)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

重複此步驟，建立`DateTime`中的範本*Views\Movie\DisplayTemplates*資料夾。 使用中的下列程式碼*Views\Movie\DisplayTemplates\DateTime.cshtml*檔案。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS 類別會將日期會以紅色粗體字顯示。 您加入`loud-1`一樣暫時的措施，以便輕鬆地查看使用此特定的範本時的 CSS 類別。

您已完成建立和自訂的 ASP.NET 用來顯示日期的範本。 一般範本 (在*Views\Shared\DisplayTemplates*資料夾) 會顯示簡單的簡短日期。 是特別針對範本`Movie`控制站 (在*Views\Movies\DisplayTemplates*資料夾) 以紅色粗體文字顯示簡短日期也格式化。

按 CTRL+F5 執行應用程式。 瀏覽器來呈現應用程式的索引檢視。

`ReleaseDate`屬性現在以紅色粗體字不含時間顯示的日期。這可協助您確認`DateTime`樣板化 helper 在*Views\Movies\DisplayTemplates*資料夾選取透過`DateTime`樣板化 helper 的共用資料夾中 (*Views\Shared\DisplayTemplates*)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

現在可以重新命名*Views\Movies\DisplayTemplates\DateTime.cshtml*檔案*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*。

按 CTRL+F5 執行應用程式。

這次`ReleaseDate`屬性會顯示日期，而不需要的時間以及粗體紅色字型。 下列說明處理輸入資料的名稱的範本 (在此情況下`DateTime`) 自動用來顯示該類型的所有模型屬性。 重新命名之後*DateTime.cshtml*檔案*LoudDateTime.cshtml*，ASP.NET 導致找不到範本中的*Views\Movies\DisplayTemplates*資料夾，所以使用*DateTime.cshtml*範本從 * Views\Movies\Shared\*資料夾。

（範本比對是不區分大小寫，因此您無法建立範本的檔案名稱具有任何大小寫。 例如， *DATETIME.chstml、 datetime.cshtml*，和*DaTeTiMe.cshtml*會全部都會相符`DateTime`型別。)

若要檢閱： 此時，`ReleaseDate`欄位會顯示使用*Views\Movies\DisplayTemplates\DateTime.cshtml*範本，它使用簡短日期格式顯示資料但否則加入沒有特殊的格式。

### <a name="using-uihint-to-specify-a-display-template"></a>使用 UIHint 指定顯示範本

如果您的 web 應用程式有許多`DateTime`欄位和您想要顯示所有或大部分的這些日期的格式，預設*DateTime.cshtml*範本是最佳方法。 但如果您有幾個日期您想要顯示完整日期和時間的地方嗎？ 沒問題。 您可以建立額外的範本，並使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性來指定完整日期和時間的格式。 您接著可以選擇性地套用該範本。 您可以使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性在模型層級，或者您可以指定內檢視的範本。 在本節中，您會看到如何使用`UIHint`屬性選擇性地變更某些日期時間欄位的執行個體的格式。

開啟*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*檔案，然後以下列取代現有的程式碼：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

這樣會導致顯示完整日期和時間，並將會使文字綠色和大型的 CSS 類別。

開啟*Movie.cs*檔案，然後加入[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性`ReleaseDate`屬性，如下列範例所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

這會告知 ASP.NET MVC，它會顯示當`ReleaseDate`屬性 (具體而言，並不是任何`DateTime`物件)，它應該使用*LoudDateTime.cshtml*範本。

按 CTRL+F5 執行應用程式。

請注意，`ReleaseDate`屬性現在會顯示日期和時間以大字型綠色。

返回`UIHint`屬性*Movie.cs*檔案，並加以註解化因此*LoudDateTime.cshtml*將不會使用範本。 再次執行應用程式。 發行日期時，不會顯示大型和綠色。 這會驗證*Views\Shared\DisplayTemplates\DateTime.cshtml* 索引 和 詳細資料檢視會使用範本。

如先前所述，您也可以套用範本中的檢視，可讓您將範本套用到個別的執行個體的一些資料。 開啟*Views\Movies\Details.cshtml*檢視。 新增`"LoudDateTime"`做為第二個參數的[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)呼叫`ReleaseDate`欄位。 已完成的程式碼看起來像這樣：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

這會指定`LoudDateTime`範本應該用來顯示模型屬性，無論何種屬性會套用至模型。

按 CTRL+F5 執行應用程式。

驗證是否使用電影索引頁面*Views\Shared\DisplayTemplates\DateTime.cshtml*範本 （紅色粗體顯示） 和*Movie\Details*網頁使用*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*範本 （大型和綠色）。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

在下一步 區段中，您將建立複雜類型的範本。

>[!div class="step-by-step"]
[上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
[下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
