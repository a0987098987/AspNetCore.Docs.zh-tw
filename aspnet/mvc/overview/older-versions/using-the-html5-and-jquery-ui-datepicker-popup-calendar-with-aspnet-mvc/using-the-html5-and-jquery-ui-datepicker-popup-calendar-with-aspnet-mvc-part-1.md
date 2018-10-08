---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: 使用 HTML5 與 jQuery UI Datepicker 快顯行事曆搭配 ASP.NET MVC-第 1 部分 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MV 中的基本概念...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: a9a373a54458faa21199019a4adbe69c0b94cb60
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577341"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>ASP.NET MVC： 第 1 部分中使用 HTML5 與 jQuery UI Datepicker 快顯行事曆
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 本教學課程將教導您如何使用編輯器範本、 顯示範本與 jQuery UI datepicker 快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


本教學課程將教導您如何使用編輯器範本、 顯示範本和 jQuery 的基本概念[UI datepicker 快顯行事曆](http://plugins.jquery.com/project/datepicker)ASP.NET MVC Web 應用程式中。 本教學課程中，您可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;)，這是免費版本的 Microsoft Visual Studio，或如果您已具備，您可以使用 Visual Studio 2010 SP1。

在開始之前，請確定您已安裝符合下列先決條件。 您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝所需的軟體，使用下列連結：

- [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）

如果您使用 Visual Studio 2010 而不 Visual Web Developer 中，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。

本教學課程假設您已完成[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程中，或您已熟悉 ASP.NET MVC 開發。 本教學課程開頭中完成的專案[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程。

本教學課程會示範在 C# 程式碼。 不過，[入門專案](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)和已完成的專案也會提供在 Visual Basic 中。

C# 和 Visual Basic 原始程式碼的 Visual Studio 專案位於本主題隨附了：[下載](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。

### <a name="what-youll-build"></a>您將建置

您將新增範本 （具體而言，編輯和顯示範本） 中建立簡單的電影清單應用程式[Getting Started with MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程。 您也會加入[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)簡化程序的輸入日期的快顯行事曆。 下列螢幕擷取畫面會顯示與 jQuery UI datepicker 快顯行事曆顯示修改過的應用程式。

![已完成的 jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>您將學習到的技能

以下是您將學到什麼：

- 如何使用屬性從[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)來控制資料的格式，顯示時，並處於編輯模式的命名空間。
- 如何建立範本 （編輯和顯示範本） 來控制資料的格式。
- 如何新增[jQuery UI datepicker](http://jqueryui.com/demos/datepicker/)做為輸入的日期欄位的方式。

### <a name="getting-started"></a>快速入門

如果您還沒有將入門專案的電影清單應用程式，下載，使用下列連結：[下載](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx? https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。 然後在 Windows 檔案總管中，以滑鼠右鍵按一下*MvcMovie.zip*檔案，然後選取**屬性**。 在  **MvcMovie.zip 屬性**對話方塊中，選取**解除封鎖**。 (解除封鎖可避免安全性警告，當您嘗試使用時，就會發生 *.zip*您已經從 web 下載的檔案。)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

以滑鼠右鍵按一下*MvcMovie.zip*檔案，然後選取**全部解壓縮**來解壓縮檔案。 在 Visual Web Developer] 或 [Visual Studio 2010 中，開啟*MvcMovieCS\_TU.sln*檔案。

在 [**方案總管] 中**，按兩下*Views\Shared\\_Layout.cshtml*加以開啟。 變更`H1`標頭**MVC 電影應用程式**要**電影 jQuery**。 按 CTRL + F5 執行應用程式，然後按一下**首頁**索引標籤上，會帶您前往`Index`電影控制器方法。 若要試用應用程式，請選取**編輯**連結並**詳細資料**電影的其中一個連結。 請注意，在索引中，編輯，並適當格式化的詳細資料檢視、 發行日期和價格：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

格式化的日期和價格是使用結果[Displayformat.{0](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)上的內容屬性`Movie`類別。

開啟*Movie.cs*檔案，並標記為註解`DisplayFormat`屬性`ReleaseDate`和`Price`屬性。 產生`Movie`類別看起來像這樣：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

按 CTRL + F5 以執行應用程式，然後選取**首頁**索引標籤，檢視的電影清單。 這次發行日期顯示日期和時間，以及 [價格] 欄位不會再顯示貨幣符號。 在您變更`Movie`類別已復原不錯的格式，您稍早所見，但您將在稍後修正該問題。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>使用 DataAnnotations DataType 屬性來指定資料類型

取代標記為註解`DisplayFormat`屬性`ReleaseDate`屬性[資料類型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性，使用`Date`列舉型別。 取代`DisplayFormat`屬性`Price`屬性[資料類型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性同樣地，這次使用`Currency`列舉型別。 這是完整的程式碼看起來像：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

執行應用程式。 現在在發行日期和價格屬性的格式正確 （也就利用適當的日期和貨幣格式）。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性提供型別中繼資料內建的 ASP.NET MVC 範本，以便呈現格式正確的欄位。 使用`DataType`屬性時，最好使用`DisplayFormat`屬性，因為原本是在程式碼的`DataType`屬性可讓模型更簡潔且更有彈性國際化等用途。

下一節中，您會看到如何進行自訂的範本，來顯示日期欄位。

> [!div class="step-by-step"]
> [下一步](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
