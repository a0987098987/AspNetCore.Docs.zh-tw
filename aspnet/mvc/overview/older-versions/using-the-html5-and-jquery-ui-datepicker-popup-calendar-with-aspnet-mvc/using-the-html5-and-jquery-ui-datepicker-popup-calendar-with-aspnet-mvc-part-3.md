---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: 使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 3 篇 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869461"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-3 部分
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


## <a name="working-with-complex-types"></a>使用複雜型別

本節中您會建立位址類別，並了解如何建立範本以顯示它。

在*模型*資料夾中，建立新的類別檔案命名為*Person.cs*會在其中放置兩種類型：`Person`類別和`Address`類別。 `Person`類別所包含的屬性，其類型為`Address`。 `Address`類型是複雜類型，這表示它不其中一個內建型別，例如`int`， `string`，或`double`。 相反地，它會有數個屬性。 新類別的程式碼看起來像這樣：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

在`Movie`控制站，加入下列`PersonDetail`動作以顯示人員執行個體：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

然後加入下列程式碼加入`Movie`控制站，以填入`Person`以特定範例資料模型：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

開啟*Views\Movies\PersonDetail.cshtml*檔案，然後加入下列標記`PersonDetail`檢視。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

按 Ctrl + F5 執行應用程式，並瀏覽到*電影/PersonDetail*。

`PersonDetail`檢視不包含`Address`複雜類型，為您可以看到在這個螢幕擷取畫面。 （沒有位址會顯示）。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

`Address`模型並不會因為它是複雜類型。 若要顯示的地址資訊，請開啟*Views\Movies\PersonDetail.cshtml*檔案一次，然後加入下列標記。

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

完整標記`PersonDetail`現在檢視看起來像這樣：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

再次執行應用程式，並顯示`PersonDetail`檢視。 現在會顯示位址資訊：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>建立複雜類型的範本

在本節中，您將建立的範本，將會用來呈現`Address`複雜型別。 當您建立的範本`Address`型別，ASP.NET MVC 可以自動使用它來設定應用程式中的任何位置的位址模型的格式。 這可讓您控制的轉譯的方式`Address`從一個位置的應用程式中的型別。

在*Views\Shared\DisplayTemplates*資料夾中，建立名為強型別部分檢視**位址**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

按一下**新增**，然後開啟 新*Views\Shared\DisplayTemplates\Address.cshtml*檔案。 新的檢視包含下列產生的標記：

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

執行應用程式，並顯示`PersonDetail`檢視。 這次`Address`您剛才建立的範本用來顯示`Address`複雜類型，所以顯示類似下列所示：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>指定模型的顯示格式和範本的方式摘要：

您所見，您可以使用下列方法指定的格式或模型屬性的範本：

- 套用`DisplayFormat`屬性設定為模型中的屬性。 例如，下列程式碼會導致顯示不含時間的日期：

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- 套用[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性中的模型和指定的資料類型的屬性。 例如，下列程式碼會導致顯示不含時間日期。

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    如果應用程式包含*date.cshtml*中的範本*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾中，該範本將用來呈現`DateTime`屬性。 否則內建 ASP.NET 樣板化系統會顯示為日期屬性。
- 建立顯示範本中的*Views\Shared\DisplayTemplates*資料夾或*Views\Movies\DisplayTemplates*資料夾名稱符合您想要格式化的資料類型。 例如，您看到*Views\Shared\DisplayTemplates\DateTime.cshtml*用於呈現`DateTime`模型，但不會加入至模型的屬性，而不需要任何標記加入至檢視中的屬性。
- 使用[UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx)屬性上指定的範本，以顯示模型屬性的模型。
- 明確地加入要的顯示範本名稱[Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx)呼叫在檢視中。

您使用的方法，取決於您要在您的應用程式中的項目。 是很常見混用這些方法，以取得所需要的格式，您需要的位置。

在下一節中，您將切換一下話題位元，從自訂向自訂輸入的方式顯示資料的方式移動。 您將會連結到編輯檢視中的應用程式，以便提供靈活的方式來指定日期的 jQuery 日期選擇器。

> [!div class="step-by-step"]
> [上一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [下一頁](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
