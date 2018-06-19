---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduction to 偵錯 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 偵錯是尋找和修正錯誤字碼頁中的程序。 本章示範的一些工具和技巧可用來偵錯並 analyz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: c28d63acda6e585f4aa64f294049c1790faac850
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897501"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introduction to 偵錯 ASP.NET Web Pages (Razor) 站台
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明偵錯 ASP.NET Web Pages (Razor) 網站中的網頁的各種方法。 偵錯是尋找和修正錯誤字碼頁中的程序。
> 
> **您將學習：** 
> 
> - 如何顯示資訊可協助分析和偵錯網頁。
> - 如何使用偵錯工具 Visual Studio 中。
>   
> 
> 這些是發行項中所引進的 ASP.NET 功能：
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


疑難排解錯誤和問題，您的程式碼中的重要層面是避免發生在第一次。 您可以執行此動作將您的程式碼有可能造成錯誤的各節`try/catch`區塊。 如需詳細資訊，請參閱節上處理中的錯誤[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890)。

`ServerInfo`協助程式是一種診斷工具，可讓您裝載您的網頁之 web 伺服器環境的相關資訊的概觀。 它也會顯示在瀏覽器要求網頁時，會傳送的 HTTP 要求資訊。 `ServerInfo` Helper 會顯示目前的使用者身分識別，發出要求，瀏覽器的類型，依此類推。 此類資訊可協助您疑難排解常見的問題。

1. 建立名為的新網頁*ServerInfo.cshtml*。
2. 在頁面的結束時，只在關閉前`</body>`標記中加入`@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    您可以加入`ServerInfo`頁面中的任何位置的程式碼。 但是加入結尾會讓其輸出分開其他網頁內容，使其更容易閱讀。

    > [!NOTE] 
    > 
    > **重要**web 網頁移到實際伺服器之前，您應該從您網頁移除任何診斷程式碼。 這適用於`ServerInfo`協助程式，以及其他診斷技術在本文中包含程式碼加入至頁面。 您不想您的網站訪客在您的伺服器和類似的詳細資訊，請參閱您的伺服器名稱、 使用者名稱、 路徑的相關資訊，因為這種類型的資訊可能會被用於惡意用途的人士很有用。
3. 儲存頁面，並在瀏覽器中執行。

    ![偵錯 1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Helper 在頁面中顯示四個資料表的資訊：

   - 伺服器組態。 本節提供裝載的 web 伺服器，包括電腦名稱、 您正在執行的 ASP.NET、 網域名稱和伺服器時間的相關資訊。
   - ASP.NET 伺服器變數。 本節提供有關許多的 HTTP 通訊協定詳細資料 （稱為 HTTP 變數） 及值是每個網頁要求的一部分。
   - HTTP 執行階段資訊。 本章節提供詳細的 Microsoft.NET Framework 下執行您的網頁、 路徑、 詳細資料快取，等等的新版。 (因為您了解在[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890)、 使用的 Razor 語法在 Microsoft ASP.NET web 伺服器技術，也就是根據廣泛的軟體本身建立的 ASP.NET Web Pages開發程式庫呼叫.NET Framework。）
   - 環境變數。 本節提供在 web 伺服器上的所有本機的環境變數及其值的清單。

     所有伺服器及要求資訊的完整說明超出本文的範圍，但您可以看到`ServerInfo`協助程式傳回的診斷資訊很多。 如需值的詳細資訊，`ServerInfo`傳回時，請參閱[辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)Microsoft TechNet 網站上和[IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)MSDN 網站上。

## <a name="embedding-output-expressions-to-display-page-values"></a>內嵌的輸出運算式，以顯示頁面的值

若要查看您的程式碼中發生的另一個方法是將輸出運算式內嵌在網頁中。 您知道，您可以直接加入類似輸出變數的值`@myVariable`或`@(subTotal * 12)`至頁面。 偵錯時，您可以在程式碼中將這些輸出運算式放在策略的點。 這可讓您執行您的頁面時，請參閱重要變數或計算結果的值。 當您完成偵錯，您可以移除的運算式或排除這些註解。此程序說明使用內嵌的運算式，以協助偵錯網頁的典型方式。

1. 建立新的 WebMatrix 網頁名為*OutputExpression.cshtml*。
2. 取代內容以下列頁面：

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    此範例會使用`switch`陳述式來檢查值`weekday`變數，然後顯示它是根據一週的哪一天的不同的輸出訊息。 在範例中，`if`區塊的第一個程式碼區塊內的任意變更的一週天數加一天到目前的週間日值。 這是為了提供說明導入錯誤。
3. 儲存頁面，並在瀏覽器中執行。

    頁面會顯示錯誤星期幾的訊息。 任何一週的星期幾實際上是，您會看到訊息一天更新版本。 雖然在此情況下您知道為什麼訊息已關閉 （因為此程式碼刻意設定不正確的日期值），實際上通常很難了解即將在程式碼中錯誤項目。 若要偵錯，您必須找出發生什麼事的索引鍵的物件和變數值這類`weekday`。
4. 藉由將加入輸出運算式`@weekday`兩個程式碼中的註解所指出的位置中所示。 這些輸出運算式會在該點顯示變數的值，在執行程式碼。

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. 儲存並執行網頁瀏覽器中。

    頁面會顯示實際星期幾第一次，然後再更新一週的星期幾，這樣會產生一天，然後按一下 從產生的訊息中新增`switch`陳述式。 從兩個變數的運算式輸出 (`@weekday`) 具有天之間沒有空格，因為您沒有加入任何 HTML`<p>`輸出、 標記運算式是僅供測試。

    ![偵錯-2](introduction-to-debugging/_static/image2.jpg)

    現在您可以看到的錯誤時使用。 當您第一次顯示`weekday`變數在程式碼中，它會顯示正確的日期。 當您顯示它的第二個時間之後,`if`封鎖程式碼中，日期是關閉的其中一個。 因此您知道發生有問題之間的週間日變數的第一個和第二個外觀。 如果這是實際的錯誤，這種方法可協助您縮小問題的程式碼的位置。
6. 修正在頁面的程式碼，藉由移除您加入兩個輸出運算式及移除的程式碼變更的一週天數。 其餘的完成區塊的程式碼看起來像下列的範例：

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. 執行網頁瀏覽器中。 此時您會看到正確的訊息顯示實際的一週天數。

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>若要顯示物件的值使用 ObjectInfo Helper

`ObjectInfo` Helper 顯示類型和您傳遞給每個物件的值。 您可以使用它來檢視變數和物件的值，在您的程式碼 （例如您未使用在上述範例中的輸出運算式），加上您所見的資料型別物件的相關資訊。

1. 開啟的檔案命名*OutputExpression.cshtml*您稍早建立的。
2. 在頁面中的所有程式碼取代為下列程式碼區塊：

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. 儲存並執行網頁瀏覽器中。

    ![偵錯-4](introduction-to-debugging/_static/image3.jpg)

    在此範例中， `ObjectInfo` helper 會顯示兩個項目：

   - 類型。 第一個變數，型別是`DayOfWeek`。 針對第二個變數的類型是`String`。
   - 數值。 在此情況下，因為您已經在頁面中顯示的問候語變數的值，該值會再次顯示當您將變數傳遞給`ObjectInfo`。

     對於更複雜的物件，`ObjectInfo`協助程式可以顯示的詳細資訊&#8212;基本上，它可以顯示的類型和值的所有物件的屬性。

## <a name="using-debugging-tools-in-visual-studio"></a>使用 Visual Studio 中偵錯工具

如需更完整的偵錯經驗，使用 Visual Studio 2013 或免費[Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)。 使用 Visual Studio 中，您可以在您想要檢查的行程式碼中設定中斷點。

![設定中斷點](introduction-to-debugging/_static/image1.png)

當您測試網站時，執行程式碼會在中斷點中止。

![到達中斷點](introduction-to-debugging/_static/image2.png)

您可以檢查變數及逐步執行程式碼--逐行的目前值。

![請參閱值](introduction-to-debugging/_static/image3.png)

如需使用整合式偵錯工具 Visual Studio 中偵錯 ASP.NET Razor 頁面資訊，請參閱[程式設計 ASP.NET Web Pages (Razor) 使用 Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)。

## <a name="additional-resources"></a>其他資源

- [使用 Visual Studio 程式設計 ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS 伺服器變數](https://msdn.microsoft.com/library/ms524602(VS.90).aspx)(MSDN)
- [辨識環境變數](https://technet.microsoft.com/library/dd560744(WS.10).aspx)(TechNet)
