---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: 簡介偵錯 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 偵錯是尋找和修正錯誤，在字碼頁中的程序。 本章示範一些工具和技術可用來偵錯和分析...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: fb9a69d568e10ddafd71e2b9600b3dae21576807
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794985"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>簡介偵錯 ASP.NET Web Pages (Razor) 網站
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章會說明各種偵錯 ASP.NET Web Pages (Razor) 網站中的網頁。 偵錯是尋找和修正錯誤，在字碼頁中的程序。
>
> **您將學到什麼：**
>
> - 如何顯示資訊可協助分析和偵錯網頁。
> - 如何使用偵錯工具在 Visual Studio 中。
>
>
> 以下是所引進的發行項 ASP.NET 功能：
>
> - `ServerInfo`協助程式。
> - `ObjectInfo` 協助程式。
>
>
> ## <a name="software-versions"></a>軟體版本
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>
>
> 本教學課程也適用於 ASP.NET Web Pages 2。 您可以使用 WebMatrix 3，但不是支援整合式偵錯工具。


疑難排解錯誤和您的程式碼中的問題很重要的層面是在第一時間避免使用它們。 您可以這麼做將您的程式碼有可能造成錯誤的各節`try/catch`區塊。 如需詳細資訊，請參閱一節上的錯誤處理[ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890)。

`ServerInfo`協助程式是一種診斷工具，可讓您裝載您的網頁之 web 伺服器環境的相關資訊的概觀。 它也會顯示您的瀏覽器要求網頁時，會傳送的 HTTP 要求資訊。 `ServerInfo`協助程式會顯示目前的使用者身分識別，發出要求，瀏覽器的類型，依此類推。 這類資訊可協助您針對常見問題進行疑難排解。

1. 建立名為的新網頁*ServerInfo.cshtml*。
2. 在網頁結束時，只是在關閉前`</body>`標記中加入`@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    您可以新增`ServerInfo`頁面中的任何位置的程式碼。 但是加入結尾會區隔其輸出與您其他的網頁內容，這會讓您更容易閱讀。

    > [!NOTE]
    >
    > **重要**網頁移到生產環境伺服器之前，您應該從您的網頁移除任何診斷程式碼。 這適用於`ServerInfo`協助程式，以及其他診斷技巧這篇文章中的包含程式碼加入至頁面。 因為這類資訊可能有助於不懷好意的人，您不希望您的網站訪客在您的伺服器和類似的詳細資訊，查看您的伺服器名稱、 使用者名稱、 路徑的相關資訊。
3. 儲存頁面，並在瀏覽器中執行它。

    ![偵錯-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo`協助程式會顯示在頁面中的四個資料表的資訊：

   - 伺服器設定。 本節提供裝載的 web 伺服器，包括電腦名稱、 您正在執行的 ASP.NET、 網域名稱和伺服器時間的相關資訊。
   - ASP.NET 伺服器變數。 本節提供許多的 HTTP 通訊協定詳細資料 （稱為 HTTP 變數） 的詳細資料和值是每一個網頁要求的一部分。
   - HTTP 執行階段資訊。 本章節提供詳細資料，Microsoft.NET Framework 下執行您的網頁、 路徑、 快取和等等的相關詳細資料的版本。 (當您了解[ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890)、 使用的 Razor 語法技術為建置基礎 Microsoft ASP.NET web 伺服器，也就是本身上豐富的軟體建置的 ASP.NET Web Pages開發程式庫呼叫.NET Framework。）
   - 環境變數。 本節提供在 web 伺服器上的所有本機環境變數及其值的清單。

     所有的伺服器和要求資訊的完整描述超出本文的範圍，但您可以看到`ServerInfo`協助程式會傳回大量的診斷資訊。 如需值的詳細資訊，`ServerInfo`傳回時，請參閱[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)Microsoft TechNet 網站上並[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN 網站上。

## <a name="embedding-output-expressions-to-display-page-values"></a>內嵌的輸出運算式，以顯示頁面的值

若要查看您的程式碼中的情況的另一個方法是將輸出運算式內嵌在網頁中。 如您所知，您可以藉由新增類似的內容，直接輸出變數的值`@myVariable`或`@(subTotal * 12)`至頁面。 進行偵錯，您可以在程式碼中將這些輸出運算式放在策略性的點。 這可讓您執行您的頁面時，請參閱重要變數或計算結果的值。 當您完成偵錯，您可以移除的運算式或註解出。此程序說明使用內嵌的運算式，以協助偵錯 頁面的典型方式。

1. 建立新的 WebMatrix 網頁，稱為*OutputExpression.cshtml*。
2. 將內容取代為下列頁面：

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    此範例會使用`switch`陳述式來檢查值`weekday`變數，然後顯示它是根據一週的哪一天的不同的輸出訊息。 在範例中，`if`區塊的第一個程式碼區塊內任意變更的一週天數加一天到目前星期幾的值。 這是為了方便說明導入錯誤。
3. 儲存頁面，並在瀏覽器中執行它。

    頁面會顯示錯誤的一天，一週的訊息。 任何星期幾實際上是，您會看到訊息一天更新版本。 雖然在此情況下您知道為什麼訊息已關閉 （因為此程式碼中刻意設定不正確的日期值），但實際上通常很難了解即將在程式碼中錯誤項目。 若要偵錯，您需要了解這類情況的索引鍵的物件和變數值`weekday`。
4. 新增輸出運算式插入`@weekday`程式碼中的註解來表示兩個位置中所示。 這些輸出運算式會在此時顯示變數的值在執行程式碼。

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 儲存並執行網頁瀏覽器中。

    此頁面會顯示實際的一天一週的第一次，然後，會將一天，然後按一下 從產生的訊息更新星期幾`switch`陳述式。 兩個變數運算式的輸出 (`@weekday`) 有天之間沒有空格，因為您未新增任何 HTML`<p>`輸出; 標記運算式是只用於測試。

    ![偵錯-2](introduction-to-debugging/_static/image2.jpg)

    現在您可以看到的錯誤時使用。 當您第一次顯示`weekday`變數在程式碼，它會顯示正確的日期。 當您顯示它的第二個的時間之後,`if`在程式碼區塊中，一天已關閉。 讓您了解，告訴它發生了第一個和第二個的外觀工作日變數之間。 如果這是實際的 bug，這種方法會幫助您縮小問題的程式碼的位置。
6. 在頁面中修正程式碼，請移除您已新增，兩個輸出運算式並移除變更的當週日期的程式碼。 程式碼的剩餘、 完整區塊看起來如下列範例所示：

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. 執行網頁瀏覽器中。 此時您會看到正確顯示實際的當週日期的訊息。

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>若要顯示物件的值使用 ObjectInfo Helper

`ObjectInfo`協助程式會顯示類型和您傳遞給它的每個物件的值。 您可以使用它來檢視變數和物件的值，在您的程式碼 （如同在上述範例中的輸出運算式），再加上您所見的資料型別物件的相關資訊。

1. 開啟名為的檔案*OutputExpression.cshtml*您稍早建立。
2. 取代下列程式碼區塊中的頁面中的所有程式碼：

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 儲存並執行網頁瀏覽器中。

    ![偵錯-4](introduction-to-debugging/_static/image3.jpg)

    在此範例中，`ObjectInfo`協助程式會顯示兩個項目：

   - 類型。 第一個變數中，型別是`DayOfWeek`。 第二個變數中，型別是`String`。
   - 數值。 在此情況下，由於您已在頁面中顯示的問候語變數的值，該值會再次顯示當您將變數傳遞給`ObjectInfo`。

     對於更複雜的物件，`ObjectInfo`協助程式可以顯示更多的資訊&#8212;基本上，它可以在此對話方塊中顯示的類型和值的所有物件的屬性。

## <a name="using-debugging-tools-in-visual-studio"></a>使用 Visual Studio 中偵錯工具

使用更完整的偵錯體驗[Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)。 使用 Visual Studio 中，您可以在您想要檢查的行程式碼中設定中斷點。

![設定中斷點](introduction-to-debugging/_static/image1.png)

當您測試網站時，在中斷點暫止執行的程式碼。

![到達中斷點](introduction-to-debugging/_static/image2.png)

您可以檢查變數及逐步執行程式碼--行的目前值。

![請參閱值](introduction-to-debugging/_static/image3.png)

如需在 Visual Studio 中使用整合式偵錯工具，來偵錯 ASP.NET Razor 頁面的資訊，請參閱[Programming ASP.NET Web Pages (Razor) 使用 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)。

## <a name="additional-resources"></a>其他資源

- [程式設計使用 Visual Studio 的 ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)
- [辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)
