---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 2 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 2818e69f912509a6392333bad8f5a1afa55884f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375285"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 2 部分
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


## <a name="adding-an-automatic-datetime-template"></a>新增自動的 DateTime 範本

在本教學課程的第一個部分，您會看到如何將屬性加入至模型，以明確指定的格式，以及如何明確地指定用來呈現模型的範本。 例如， [Displayformat.{0](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性，在下列程式碼明確指定的格式`ReleaseDate`屬性。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

在下列範例中， [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性，使用`Date`列舉型別，可讓您指定日期範本，應該用來呈現模型。 如果您的專案中沒有任何日期範本，則會使用內建日期範本。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

不過，ASP。MVC 可以執行類型比對使用慣例優於組態，藉由尋找符合型別名稱的範本。 這可讓您建立的範本，而不需完全使用任何屬性或程式碼，自動格式化資料。 此部分的教學課程中，您將建立的範本，就會自動套用至模型屬性的型別[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 您不需要使用屬性或其他設定來指定範本，應該用來呈現所有模型屬性的型別[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。

您也將了解自訂個別屬性或甚至是個別欄位的顯示方式。

若要開始，讓我們移除現有的格式資訊，並在應用程式中顯示完整的日期。

開啟*Movie.cs*檔案，並標記為註解`DataType`屬性`ReleaseDate`屬性：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

按 CTRL+F5 執行應用程式。

請注意，`ReleaseDate`屬性現在會顯示日期和時間，因為這是預設值時不提供的任何格式的資訊。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>新增的 CSS 樣式，用於測試新的範本

建立用於格式化日期的範本之前，您將新增一些您可以套用至新範本的 CSS 樣式規則。 這些功能會協助您確認呈現的網頁使用新的範本。

開啟*Content\Site.cs*s 檔案，並將下列 CSS 規則新增至檔案最下方：

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>新增日期時間的顯示範本

現在您可以建立新的範本。 在  *Views\Movies*資料夾中，建立*DisplayTemplates*資料夾。

在  *Views\Shared*資料夾中，建立*DisplayTemplates*資料夾以及*EditorTemplates*資料夾。

中的顯示範本*Views\Shared\DisplayTemplates*資料夾可供所有控制站。 中的顯示範本*Views\Movie\DisplayTemplates*資料夾將僅供`Movie`控制站。 (如果具有相同名稱的範本會出現在這兩個資料夾中的範本*Views\Movie\DisplayTemplates*資料夾 — 也就是更特定的範本，會針對檢視所傳回的優先`Movie`控制器。)

在 [**方案總管] 中**，展開*檢視*資料夾中，展開*共用*資料夾，然後再以滑鼠右鍵按一下*Views\Shared\DisplayTemplates*資料夾。

按一下 **新增**，然後按一下**檢視**。 **加入檢視**對話方塊隨即出現。

在 **檢視表名稱**方塊中，輸入`DateTime`。 （您必須使用此名稱以符合類型的名稱）。

選取 **建立成局部檢視**核取方塊。 請確定**使用版面配置頁或主版頁面**並**建立強型別檢視**未選取核取方塊。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

按一下 [加入] 。 A *DateTime.cshtml*中建立範本*Views\Shared\DisplayTemplates*。

下圖顯示*檢視*中的資料夾**方案總管**之後`DateTime`建立顯示和編輯程式的範本。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

開啟*Views\Shared\DisplayTemplates\DateTime.cshtml*檔案，並新增下列標記中，它會使用[String.Format](https://msdn.microsoft.com/library/system.string.format.aspx)可設定為不包含時間的日期格式之屬性的方法。 (`{0:d}`格式指定簡短日期格式。)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

重複此步驟，建立`DateTime`中的範本*Views\Movie\DisplayTemplates*資料夾。 使用中的下列程式碼*Views\Movie\DisplayTemplates\DateTime.cshtml*檔案。

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

`loud-1` CSS 類別會將日期是以紅色粗體文字顯示。 您加入`loud-1`就如同暫時的措施，讓您可以輕鬆地查看使用這個特殊範本中時的 CSS 類別。

您已完成建立和自訂的 ASP.NET 用來顯示日期的範本。 更多一般範本 (在*Views\Shared\DisplayTemplates*資料夾) 會顯示簡單的簡短日期。 範本特別`Movie`控制器 (在*Views\Movies\DisplayTemplates*資料夾) 會也格式化為簡短日期顯示為紅色粗體文字。

按 CTRL+F5 執行應用程式。 瀏覽器呈現應用程式的 [索引] 檢視。

`ReleaseDate`現在以紅色粗體字不包含的時間顯示日期的屬性。這可協助您確認`DateTime`中的樣板化 helper *Views\Movies\DisplayTemplates*透過選取資料夾`DateTime`共用資料夾中的樣板化 helper (*Views\Shared\DisplayTemplates*)。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

現在可以重新命名*Views\Movies\DisplayTemplates\DateTime.cshtml*的檔案*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*。

按 CTRL+F5 執行應用程式。

這次`ReleaseDate`屬性來顯示日期不包含的時間而紅色粗體字。 這說明了具有資料的名稱的範本類型 (在此情況下`DateTime`) 自動用來顯示該類型的所有模型屬性。 重新命名之後*DateTime.cshtml*的檔案*LoudDateTime.cshtml*，ASP.NET 導致找不到範本中的*Views\Movies\DisplayTemplates*資料夾，所以使用*DateTime.cshtml*範本，從 * Views\Movies\Shared\*資料夾。

（範本比對是區分大小寫，因此您無法建立範本檔案名稱具有任何大小寫。 例如， *DATETIME.chstml、 datetime.cshtml*，並*DaTeTiMe.cshtml*所有符合`DateTime`型別。)

若要檢閱： 到目前為止，`ReleaseDate`欄位會顯示使用*Views\Movies\DisplayTemplates\DateTime.cshtml*範本，使用簡短日期格式顯示資料，但否則加入沒有特殊的格式。

### <a name="using-uihint-to-specify-a-display-template"></a>使用 UIHint 指定顯示範本

如果您的 web 應用程式有許多`DateTime`欄位和您想要僅限日期的格式，顯示它們的全部或大部分的預設*DateTime.cshtml*範本是不錯的方法。 但是，如果您有幾個日期您想要顯示完整日期和時間的地方嗎？ 沒問題。 您可以建立額外的範本，並使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性來指定格式的完整日期和時間。 您接著可以選擇性地套用該範本。 您可以使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性在模型層級，或者您可以指定在檢視內的範本。 在本節中，您會看到如何使用`UIHint`屬性選擇性地變更某些情況下的日期時間欄位的格式。

開啟*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*檔案，並以下列內容取代現有的程式碼：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

這樣會導致要顯示的完整日期和時間，並新增讓文字，綠色和大型的 CSS 類別。

開啟*Movie.cs*檔案，並新增[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性設定為`ReleaseDate`屬性，如下列範例所示：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

這會告訴 ASP.NET MVC，它會顯示當`ReleaseDate`屬性 (具體而言，並不是任何`DateTime`物件)，它應該使用*LoudDateTime.cshtml*範本。

按 CTRL+F5 執行應用程式。

請注意，`ReleaseDate`屬性現在會顯示日期和時間的大型的綠色字型。

返回`UIHint`屬性中*Movie.cs*檔案，並註解化，因此*LoudDateTime.cshtml*不會使用範本。 再次執行應用程式。 發行日期時，不會顯示大型和綠色。 這會驗證*Views\Shared\DisplayTemplates\DateTime.cshtml*範本會在 索引 和 詳細資料檢視。

如先前所述，您也可以套用範本，以在檢視中，它可讓您將範本套用到個別的執行個體的一些資料。 開啟*Views\Movies\Details.cshtml*檢視。 新增`"LoudDateTime"`做為第二個參數的[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)呼叫`ReleaseDate`欄位。 完成的程式碼如下所示：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

這會指定`LoudDateTime`範本應該用來顯示模型屬性，不論何種屬性套用至模型。

按 CTRL+F5 執行應用程式。

驗證是否使用影片索引頁*Views\Shared\DisplayTemplates\DateTime.cshtml*範本 （紅色粗體） 和*Movie\Details*網頁使用*Views\Movies\DisplayTemplates\LoudDateTime.cshtml*範本 （大型和綠色）。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

在下一步 區段中，您將建立複雜類型的範本。

> [!div class="step-by-step"]
> [上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
