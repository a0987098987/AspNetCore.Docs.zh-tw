---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: 建立一致的版面配置中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 若要更有效率，以建立您的網站的網頁，您可以建立可重複使用內容區塊 （如頁首和頁尾） 您的網站及您的 c...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530237"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web Pages (Razor) 網站中建立一致的版面配置
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用版面配置頁面在 ASP.NET Web Pages (Razor) 的網站中，建立可重複使用的內容 （例如頁首和頁尾） 的區塊，並在站台中建立外觀一致的所有頁面。
> 
> **您將學習：** 
> 
> - 如何建立可重複使用的內容，如頁首和頁尾的區塊。
> - 如何建立網站使用的版面配置中的所有頁面一致的外觀。
> - 如何將資料在執行階段傳遞至版面配置頁。
> 
> 這些是發行項中所引進的 ASP.NET 功能：
> 
> - 內容區塊，就是包含在多個頁面中插入 HTML 格式之內容的檔案。
> - 版面配置頁頁包含 HTML 格式之內容可在網站上的頁面所共用。
> - `RenderPage`， `RenderBody`，和`RenderSection`方法會通知 ASP.NET 頁面項目插入的位置。
> - `PageData`字典，其中可讓您共用內容的區塊和版面配置頁之間的資料。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="about-layout-pages"></a>關於版面配置頁

許多網站有在每個頁面上，例如頁首和頁尾或告知使用者已登入的方塊會顯示的內容。 ASP.NET 可讓您建立個別的檔案與內容的區塊可以包含文字、 標記和程式碼，就像標準網頁。 然後，您就可以插入該內容區塊中所要顯示的資訊站台上的其他頁面。 這樣一來，您不需要複製並貼到每個頁面的 相同的內容。 建立這樣的一般內容也可讓您更輕鬆地更新您的網站。 如果您要變更的內容時，您可以只更新單一檔案，而且所做的變更會反映然後 everywhere 已插入內容。

下圖顯示內容封鎖工作的方式。 當瀏覽器從 web 伺服器要求頁面時，ASP.NET 會在點插入內容區塊所在`RenderPage`主頁面中呼叫方法。 完成 （合併） 頁面，然後會傳送至瀏覽器。

![顯示如何 RenderPage 方法插入至目前的頁面參考的頁面的概念性圖表。](3-creating-a-consistent-look/_static/image1.jpg)

在此程序，您會建立參考兩個內容區塊 （頁首和頁尾） 位於不同的檔案中的網頁。 您可以使用這些相同的內容區塊，在您的網站中的任何分頁。 當您完成時，您會取得如下的頁面：

![在執行一個網頁，內含 RenderPage 方法呼叫而產生的瀏覽器中顯示網頁的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image2.jpg)

1. 在您的網站的根資料夾中，建立名為*Index.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. 在根資料夾中，建立名為資料夾*共用*。

    > [!NOTE]
    > 常見的做法，來儲存名為的資料夾中的網頁所共用的檔案是*共用*。
4. 在*共用*資料夾中，建立名為 *\_Header.cshtml*。
5. 以下列內容取代任何現有內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    請注意，檔案名稱是 *\_Header.cshtml*，以底線 (\_) 做為前置詞。 ASP.NET 不會將頁面傳送至瀏覽器，如果其名稱開頭為底線。 這可避免使用者要求 （不小心或失敗） 這些頁面直接。 最好使用名稱網頁內容區塊，前面加上的底線，您實際上不想讓使用者可以要求這些頁面 &#8212;其嚴格存在要插入至其他頁面。
6. 在*共用*資料夾中，建立名為 *\_Footer.cshtml*並取代為下列內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. 在*Index.cshtml*頁面上，加入兩個呼叫`RenderPage`方法，如下所示：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    這會顯示如何插入 web 網頁內容的區塊。 您呼叫`RenderPage`方法並傳遞您想要在該點插入其內容的檔案名稱。 在這裡，您要插入的內容 *\_Header.cshtml*和 *\_Footer.cshtml*檔案至*Index.cshtml*檔案。
8. 執行*Index.cshtml*瀏覽器中的。 (在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**瀏覽器中啟動**。)
9. 在瀏覽器中檢視網頁原始檔。 (例如，在 Internet Explorer 中，以滑鼠右鍵按一下頁面，然後按一下**檢視原始檔**。)

    這可讓您查看傳送至瀏覽器，結合在內容區塊的索引頁面標記的網頁標記。 下列範例顯示呈現頁面來源*Index.cshtml*。 若要呼叫`RenderPage`插入*Index.cshtml*已取代為頁首和頁尾檔案的實際內容。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>建立一致的外觀，使用版面配置頁

到目前為止所見，所以可以輕鬆地包含多個頁面上相同的內容。 建立站台的一致的外觀更結構化的方法是使用版面配置頁。 版面配置頁面定義的網頁，結構，但不包含任何實際內容。 建立版面配置頁面之後，您可以建立包含的內容，然後將它們連結至版面配置頁的網頁。 當顯示這些頁面時，它們會格式化根據 [配置] 頁面中。 （這個意義而言，版面配置頁做為一種內容中的其他頁面定義的範本。）

[配置] 頁面就如同任何 HTML 頁面上，從不同之處在於它包含呼叫`RenderBody`方法。 位置`RenderBody`版面配置頁面中的方法會判斷會包含從內容頁面的資訊。

下圖顯示如何在內容頁和版面配置頁會結合以產生已完成的 web 網頁的執行階段。 瀏覽器要求內容的頁面。 [內容] 頁面中，指定要用於頁面結構的 [配置] 頁面中有程式碼。 在 [配置] 頁面中，內容會插入在點其中`RenderBody`方法呼叫。 內容區塊也可插入版面配置頁藉由呼叫`RenderPage`方法上, 一節中的方式。 網頁上完成時，它會傳送至瀏覽器。

![在執行一個網頁，內含 RenderBody 方法呼叫而產生的瀏覽器中顯示網頁的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image3.jpg)

下列程序示範如何建立版面配置頁面和連結到它的內容頁面。

1. 在*共用*資料夾，您的網站，建立名為 *\_Layout1.cshtml*。
2. 以下列內容取代任何現有內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    您使用`RenderPage`版面配置頁面，即可將內容的區塊中的方法。 版面配置頁可以包含只有一個呼叫`RenderBody`方法。
3. 在*共用*資料夾中，建立名為 *\_Header2.cshtml*和任何現有的內容取代為下列：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. 在根資料夾中，建立新的資料夾並將其命名*樣式*。
5. 在*樣式*資料夾中，建立名為*Site.css*並加入下列樣式定義：

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    這些樣式定義會在此處僅為顯示樣式表如何搭配版面配置頁。 如果您想，您可以定義自己的樣式，用於下列項目。
6. 在根資料夾中，建立名為*Content1.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    這是一個網頁，將會使用版面配置頁。 在頁面頂端的程式碼區塊會指出要用來格式化此內容的版面配置頁。
7. 執行*Content1.cshtml*瀏覽器中。 轉譯的頁面使用的格式和樣式表中定義 *\_Layout1.cshtml*中定義的文字 （內容） 和*Content1.cshtml*。

    ![[影像]](3-creating-a-consistent-look/_static/image4.jpg)

    您可以重複步驟 6 來建立其他的內容頁面，然後可以共用相同的 [配置] 頁面。

    > [!NOTE]
    > 使您可以自動使用相同的版面配置頁資料夾中的所有內容頁面，可以設定您的網站。 如需詳細資訊，請參閱[自訂全站台的行為的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)。

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>設計版面配置頁面中有多個內容區段

內容頁面可以有多個區段，會很有用，如果您想要使用具有多個可取代內容區域的配置。 在 [內容] 頁面中，您為每個區段指定唯一名稱。 (保留的預設區段命名。)在 [配置] 頁面中，您將`RenderBody`方法，以指定應未命名的 （預設） 一節。 然後，您將個別`RenderSection`方法，才能個別呈現具名的區段。

下圖顯示如何 ASP.NET 處理分成多個區段的內容。 每個具名的區段包含在一節中的區塊內容的頁面。 (其名稱`Header`和`List`在範例中。)架構會將內容區段插入點的 [配置] 頁面其中`RenderSection`方法呼叫。 未命名的 （預設） 一節會插入在點其中`RenderBody`呼叫方法，如稍早所見。

![顯示如何 RenderSection 方法插入至目前的頁面參考章節的概念性圖表。](3-creating-a-consistent-look/_static/image5.jpg)

此程序示範如何建立具有多個內容區段的內容頁面以及如何使用支援多個內容區段的版面配置頁面呈現。

1. 在*共用*資料夾中，建立名為 *\_Layout2.cshtml*。
2. 以下列內容取代任何現有內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    您使用`RenderSection`轉譯標頭 和 清單 區段的方法。
3. 在根資料夾中，建立名為*Content2.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    此內容的頁面包含在頁面頂端的程式碼區塊。 每個具名的區段包含在區段區塊。 網頁的其餘部分包含預設 （未命名） 的內容 > 一節。
4. 執行*Content2.cshtml*瀏覽器中。

    ![在執行一個網頁，內含 RenderSection 方法呼叫而產生的瀏覽器中顯示網頁的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>進行選擇性的內容區段

一般來說，您建立內容頁面中的章節一定要相符 [配置] 頁面中所定義的區段。 如果發生下列任何動作，可能會收到錯誤：

- [內容] 頁面包含 [配置] 頁面中沒有對應的區段的區段。
- [配置] 頁面包含的區段，它沒有任何內容。
- [配置] 頁面包含嘗試一次以上呈現該相同區段的方法呼叫。

不過，您可以宣告為選擇性，在 [配置] 頁面中的區段來覆寫具名區段的這個行為。 這可讓您定義多個內容頁面，可以共用版面配置頁面，但可能會或可能不會有特定區段的內容。

1. 開啟*Content2.cshtml*及移除下列區段：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. 儲存網頁，然後在瀏覽器中執行它。 會顯示錯誤訊息，因為內容的頁面不會定義在版面配置頁面中，也就是標頭區段的區段提供內容。

    ![螢幕擷取畫面顯示如果您執行頁面，就會發生此錯誤的呼叫 RenderSection 方法，但未提供的對應區段。](3-creating-a-consistent-look/_static/image7.jpg)
3. 在*共用*資料夾中，開啟 *\_Layout2.cshtml*頁面上，並取代這一行：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    下列程式碼：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    或者，您無法前的一行程式碼取代下列程式碼區塊，會產生相同結果：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. 執行*Content2.cshtml*在瀏覽器中的頁面。 （如果仍有瀏覽器中開啟此頁面，您可以只重新整理。）這次頁面會不顯示任何錯誤，即使有沒有標頭。

## <a name="passing-data-to-layout-pages"></a>將資料傳遞至版面配置頁

您可能會有您需要在版面配置頁面中參考的內容頁面中定義的資料。 如果是這樣，您需要將內容頁面中的資料傳遞至版面配置頁面。 例如，您可能想要顯示的使用者，登入狀態，或您可能想要顯示或隱藏根據使用者輸入的內容區域。

若要將內容頁中的資料傳遞至版面配置頁面中，您可以將值插入`PageData`內容頁的屬性。 `PageData`屬性是包含您想要在頁面之間傳遞資料的名稱/值組的集合。 在 [配置] 頁面中，然後可以讀取值超出`PageData`屬性。

以下是另一個圖表。 這會顯示如何使用 ASP.NET`PageData`將從內容頁面的值傳遞至版面配置頁的屬性。 當 ASP.NET 開始建置網頁時，它會建立`PageData`集合。 在 [內容] 頁面中，您撰寫程式碼以將資料放入`PageData`集合。 中的值`PageData`集合也可以存取內容頁面中的其他章節，或其他內容的區塊。

![說明如何內容頁面可以擴展 PageData 字典，並將該資訊傳遞至版面配置頁的概念性圖表。](3-creating-a-consistent-look/_static/image8.jpg)

下列程序示範如何將資料從內容頁面傳遞至版面配置頁。 執行網頁時，它會顯示一個按鈕，可讓使用者隱藏或顯示 [配置] 頁面中定義的清單。 當使用者按一下按鈕時，會設定為 true/false （布林值） 值中`PageData`屬性。 版面配置頁讀取該值，和如果為 false，則會隱藏清單。 值也內容頁面中用來決定是否要顯示**隱藏清單**按鈕或**顯示清單** 按鈕。

![[影像]](3-creating-a-consistent-look/_static/image9.jpg)

1. 在根資料夾中，建立名為*Content3.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    程式碼會儲存兩個資料片段`PageData`屬性 &#8212; 網頁，true 或 false，指定是否要顯示清單的標題。

    請注意，ASP.NET 可讓您進入有條件地使用程式碼區塊的網頁的 HTML 標記。 例如，`if/else`網頁主體中的區塊會決定哪一個表單，以顯示取決於是否`PageData["ShowList"]`設為 true。
2. 在*共用*資料夾中，建立名為 *\_Layout3.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    [配置] 頁面包含的運算式中`<title>`取得的標題值的項目`PageData`屬性。 它也會使用`ShowList`值`PageData`屬性來判斷是否要顯示的清單內容的區塊。
3. 在*共用*資料夾中，建立名為 *\_List.cshtml*和任何現有的內容取代為下列：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. 執行*Content3.cshtml*瀏覽器中的。 頁面會顯示清單顯示在頁面的左邊和**隱藏清單**底部的按鈕。

    ![顯示網頁，內含清單和按鈕的時候隱藏清單的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image10.jpg)
5. 按一下**隱藏清單**。 清單會消失，按鈕會變成**顯示清單**。

    ![顯示的頁面不包括清單和按鈕的時候顯示清單的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image11.jpg)
6. 按一下**顯示清單**按鈕和清單會顯示一次。

## <a name="additional-resources"></a>其他資源


[ASP.NET Web 網頁自訂全站台的行為](https://go.microsoft.com/fwlink/?LinkId=202906)
