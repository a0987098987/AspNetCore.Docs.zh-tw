---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: "使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 1 部分 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MV 中的基本概念..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 4b5507021af47d96c29809c9830d0558f5501f87
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>使用 HTML5 與 jQuery UI 日期選擇器快顯行事曆搭配 ASP.NET MVC-第 1 部分
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程將告訴您如何使用編輯器範本、 顯示範本與 jQuery UI 日期選擇器快顯行事曆，ASP.NET MVC Web 應用程式中的基本概念。


本教學課程將告訴您如何使用編輯器範本、 顯示範本和 jQuery 的基本概念[UI 日期選擇器快顯行事曆](http://plugins.jquery.com/project/datepicker)ASP.NET MVC Web 應用程式中。 此教學課程中，您可以使用 Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;)，這是免費版的 Microsoft Visual Studio，或您可以使用 Visual Studio 2010 SP1，如果您已經有的。

開始之前，請確定您已安裝下面所列的必要條件。 您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。 或者，您可以個別安裝所需的軟體，使用下列連結：

- [Visual Studio Web Developer Express SP1 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）

如果您使用 Visual Studio 2010 而不 Visual Web Developer 中，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。

本教學課程假設您已經完成[入門 MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程中，或您已熟悉 ASP.NET MVC 開發。 本教學課程開頭已完成的專案，從[入門 MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程。

本教學課程會示範 C# 中的程式碼。 不過，[入門專案](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)和已完成的專案也會提供在 Visual Basic 中。

本主題隨附了 C# 和 Visual Basic 原始程式碼的 Visual Studio 專案：[下載](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。

### <a name="what-youll-build"></a>您將建置

您會加入樣板 （具體而言，編輯和顯示範本） 中建立簡單的電影清單應用程式[入門 MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)教學課程。 您也會加入[jQuery UI 日期選擇器](http://jqueryui.com/demos/datepicker/)簡化的程序的輸入日期的快顯行事曆。 下列螢幕擷取畫面會顯示已修改的應用程式與 jQuery UI 日期選擇器快顯行事曆顯示。

![完成的 jQuery](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>您將學習到的技術

以下是您將學習：

- 如何使用屬性從[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間來控制資料的格式顯示時，並處於編輯模式。
- 如何建立範本 （編輯和顯示範本） 來控制資料的格式。
- 如何新增[jQuery UI 日期選擇器](http://jqueryui.com/demos/datepicker/)做為輸入的日期欄位的方式。

### <a name="getting-started"></a>快速入門

如果您還沒有起始專案的電影清單應用程式，下載會使用下列連結：[下載](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800)。 然後在 Windows 檔案總管中，以滑鼠右鍵按一下*MvcMovie.zip*檔案，然後選取**屬性**。 在**MvcMovie.zip 屬性**對話方塊中，選取**解除封鎖**。 (解除封鎖可避免安全性警告，當您嘗試使用時，就會發生*.zip*您已經從 web 下載的檔案。)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

以滑鼠右鍵按一下*MvcMovie.zip*檔案，然後選取**全部解壓縮**來解壓縮檔案。 在 Visual Web Developer 或 Visual Studio 2010 中開啟*MvcMovieCS\_TU.sln*檔案。

在**方案總管 中**，連按兩下*_layout.cshtml\\_Layout.cshtml*加以開啟。 變更`H1`標頭從**MVC 電影應用程式**至**影片 jQuery**。 按下 CTRL + F5 執行應用程式，然後按一下**首頁**索引標籤上，會帶您前往`Index`影片控制器方法。 若要試用應用程式，選取**編輯**連結和**詳細資料**影片的其中一個連結。 請注意，在索引中，編輯，並妥善格式化檢視詳細資料、 發行日期和價格：

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

格式的日期和價格是使用結果[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)上的內容屬性`Movie`類別。

開啟*Movie.cs*檔案，並標記為註解`DisplayFormat`屬性`ReleaseDate`和`Price`屬性。 產生`Movie`類別看起來像這樣：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

按 CTRL + F5 以執行應用程式並選取**首頁**索引標籤，檢視電影清單。 這次發行日期顯示日期和時間，和 [價格] 欄位不會再顯示的貨幣符號。 在您變更`Movie`類別已復原的不錯格式稍早所見，但將在不久後修正這個問題。

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>使用 DataAnnotations DataType 屬性來指定資料類型

取代標記為註解`DisplayFormat`屬性`ReleaseDate`屬性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性，使用`Date`列舉型別。 取代`DisplayFormat`屬性`Price`屬性[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性同樣地，這次使用`Currency`列舉型別。 這是已完成的程式碼看起來像：

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

執行應用程式。 現在的發行日期和價格屬性的格式正確 （亦即，使用適當的日期和貨幣格式）。 [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性提供型別中繼資料內建的 ASP.NET MVC 範本，以便呈現格式正確的欄位。 使用`DataType`屬性時，最好使用`DisplayFormat`原本在程式碼，因為屬性`DataType`屬性可讓模型較為簡潔且更有彈性的國際化這類目的。

下一節中，您會看到如何進行自訂範本來顯示日期的欄位。

>[!div class="step-by-step"]
[下一步](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
