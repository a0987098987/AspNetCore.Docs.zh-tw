---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 3 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832028"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>ASP.NET MVC： 第 3 部分中使用 HTML5 與 jQuery UI Datepicker 快顯行事曆
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


## <a name="working-with-complex-types"></a>使用複雜型別

在本節中，您會建立位址類別及了解如何建立樣板來顯示它。

在 *模型*資料夾中，建立新的類別檔案，名為*Person.cs*會在其中放置兩種類型：`Person`類別和`Address`類別。 `Person`類別將包含屬性的類型為`Address`。 `Address`類型是複雜類型，這表示它不其中一個內建的型別，例如`int`， `string`，或`double`。 相反地，它有數個屬性。 新的類別的程式碼看起來像這樣：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

在 `Movie`控制站，新增下列`PersonDetail`動作以顯示 person 執行個體：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

然後新增下列程式碼`Movie`控制器來填入`Person`具有一些範例資料的模型：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

開啟*Views\Movies\PersonDetail.cshtml*檔案，並新增下列標記`PersonDetail`檢視。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

按 Ctrl + F5 執行應用程式，並瀏覽到*電影/PersonDetail*。

`PersonDetail`檢視不包含`Address`複雜型別，您可以看到此螢幕擷取畫面中。 （沒有位址會顯示）。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address`模型並不會因為它是複雜型別。 若要顯示的地址資訊，請開啟*Views\Movies\PersonDetail.cshtml*檔案一次，並新增下列標記。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

完整標記`PersonDetail`現在檢視看起來像這樣：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

再次執行應用程式，並顯示`PersonDetail`檢視。 現在會顯示的位址資訊：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>建立範本的複雜型別

在本節中，您將建立的範本，將會用來呈現`Address`複雜型別。 當您建立的範本`Address`型別，ASP.NET MVC 可以自動利用它來設定應用程式中的任何位置的位址模型的格式。 這可讓您控制呈現的方式`Address`從應用程式中的一個位置的型別。

在  *Views\Shared\DisplayTemplates*資料夾中，建立名為強型別部分檢視**地址**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

按一下 **新增**，然後開啟 新*Views\Shared\DisplayTemplates\Address.cshtml*檔案。 新的檢視包含下列產生的標記：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

執行應用程式，並顯示`PersonDetail`檢視。 此時`Address`您剛才建立的範本用來顯示`Address`複雜型別，所以顯示看起來如下所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>指定的模型顯示格式和範本的方式摘要：

您已了解您可以使用下列方法指定的格式或模型屬性的範本：

- 套用`DisplayFormat`屬性加入模型中的屬性。 例如，下列程式碼會導致顯示不包含時間的日期：

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- 套用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性中的模型和指定的資料類型的屬性。 例如，下列程式碼會導致顯示不包含時間的日期。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    如果應用程式含有*date.cshtml*中的範本*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾中，該範本將用來呈現`DateTime`屬性。 否則內建的 ASP.NET 範本化系統會顯示為日期的屬性。
- 建立顯示範本中的*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾名稱符合您想要格式化的資料類型。 例如，您看到*Views\Shared\DisplayTemplates\DateTime.cshtml*用來呈現`DateTime`在模型中，加入至模型屬性而不將任何標記新增至檢視的屬性。
- 使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)來指定要顯示的 模型屬性的範本模型的屬性。
- 明確地將顯示的範本名稱，以[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)在檢視中呼叫。

您使用的方法取決於您要在您的應用程式中執行動作。 是不常見混用這些方法來取得所需要的格式，您需要的位置。

在下一步 區段中，您將切換有點一下話題，並將從自訂向自訂輸入的方式顯示資料的方式移動。 您將會連接到應用程式，以便提供靈活的方式，來指定日期中的 [編輯] 檢視的 jQuery 日期選擇器。

> [!div class="step-by-step"]
> [上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
