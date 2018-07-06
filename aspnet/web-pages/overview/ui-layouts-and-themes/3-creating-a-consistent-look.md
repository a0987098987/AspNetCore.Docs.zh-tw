---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: 建立一致的版面配置，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 若要更有效率的方式建立您的網站的網頁，您可以建立可重複使用內容區塊 （例如頁首和頁尾） 為您的網站和您的 c...
ms.author: aspnetcontent
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: d27cdc70417f380d596f4d07384a615586427643
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821210"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web Pages (Razor) 網站中建立一致的版面配置
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何使用版面配置頁的 ASP.NET Web Pages (Razor) 網站中建立可重複使用的區塊的內容 （例如頁首和頁尾），以及在網站中建立的所有頁面一致的外觀。
> 
> **您將學到什麼：** 
> 
> - 如何建立可重複使用的區塊的內容，例如頁首和頁尾。
> - 如何在您的網站使用的版面配置建立一致的外觀的所有頁面。
> - 如何在執行階段的資料傳遞至版面配置頁。
> 
> 以下是所引進的發行項 ASP.NET 功能：
> 
> - 內容的區塊，也就是包含在多個頁面中插入 HTML 格式之內容的檔案。
> - 版面配置頁面： 包含 HTML 格式的內容可在網站上的頁面所共用的頁面。
> - `RenderPage`， `RenderBody`，和`RenderSection`方法，告知 ASP.NET 將頁面項目插入的位置。
> - `PageData`字典，可讓您共用內容區塊和版面配置頁之間的資料。
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

許多網站都有在每個頁面上，例如標頭和頁尾中或告知使用者已登入的方塊會顯示的內容。 ASP.NET 可讓您建立個別的檔案與內容的區塊可以包含文字、 標記和程式碼，就像標準網頁一樣。 然後，您就可以在您想要的資訊，才會出現在網站上的其他頁面中插入該內容區塊。 這樣一來，您不需要複製並貼到每個頁面的 相同的內容。 建立這類的一般內容也可讓您更輕鬆地更新您的網站。 如果您要變更內容，您可以只更新單一檔案，而且所做的變更會反映然後到處都已插入內容。

下圖顯示內容封鎖工作的方式。 當瀏覽器要求網頁從 web 伺服器時，ASP.NET 會在點插入內容區塊所在`RenderPage`方法會呼叫在主頁面。 完成 （合併） 頁面，然後會傳送至瀏覽器。

![概念的圖表，顯示 RenderPage 方法如何參考的頁插入目前的頁面。](3-creating-a-consistent-look/_static/image1.jpg)

在此程序中，您將建立會參考兩個內容區塊 （標頭和頁尾） 位於不同的檔案中的頁面。 在您的網站中的任何網頁中，您可以使用這些相同的內容區塊。 當您完成時，您會收到如下的頁面：

![在執行一個網頁，內含 RenderPage 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image2.jpg)

1. 在您的網站根資料夾中，建立名為*Index.cshtml*。
2. 以下列內容取代現有的標記：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. 在根資料夾中，建立名為*共用*。

    > [!NOTE]
    > 它是常見的作法是儲存在名為的資料夾中的 web 網頁之間共用的檔案*共用*。
4. 在  *Shared*資料夾中，建立名為 *\_Header.cshtml*。
5. 以下列內容取代任何現有的內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    請注意，檔案名稱是 *\_Header.cshtml*，以底線 (\_) 做為前置詞。 ASP.NET 不會將頁面傳送至瀏覽器，如果其名稱開頭為底線。 這可防止人員要求 （不小心或失敗） 這些網頁直接。 它是個不錯的主意，使用名稱頁具有內容區塊，前面加上的底線，因為您真的不希望使用者能夠要求這些頁面&#8212;絕對要插入至其他頁面存在。
6. 在 *共用*資料夾中，建立名為 *\_Footer.cshtml*並取代為下列內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. 在  *Index.cshtml*頁面上，新增兩個呼叫`RenderPage`方法，如下所示：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    這會顯示如何插入 web 網頁內容的區塊。 您呼叫`RenderPage`方法並將它傳遞您想要在該點插入其內容的檔案名稱。 在這裡，您要插入的內容 *\_Header.cshtml*並 *\_Footer.cshtml*檔案至*Index.cshtml*檔案。
8. 執行*Index.cshtml*瀏覽器中的頁面。 (在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**在瀏覽器中啟動**。)
9. 在瀏覽器中檢視網頁原始檔。 (例如，在 Internet Explorer 中，以滑鼠右鍵按一下  頁面上，然後按一下**檢視原始檔**。)

    這可讓您查看傳送至瀏覽器中，其結合了與內容區塊的索引頁面標記的 web 網頁標記。 下列範例顯示呈現頁面原始碼*Index.cshtml*。 若要呼叫`RenderPage`插入至*Index.cshtml*已取代為實際的頁首和頁尾的檔案內容。

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>建立一致的外觀，使用版面配置頁

到目前為止，您已了解，就可以輕鬆地包含多個頁面上相同的內容。 建立一致的外觀，站台的更具結構化的方式是使用版面配置頁。 版面配置頁定義網頁的結構，但不包含任何實際的內容。 建立版面配置頁之後，您可以建立包含內容，然後將它們連結至版面配置頁的網頁。 當顯示這些頁面時，它們會格式化根據版面配置頁中。 （這個意義來說，版面配置頁做為一種在其他頁面中定義的內容的範本。）

[配置] 頁面就像任何 HTML 頁面上，一樣，只不過它包含呼叫`RenderBody`方法。 位置`RenderBody`版面配置頁的方法會判斷會包含 [內容] 頁面中的資訊。

下圖顯示如何內容頁面和版面配置頁會結合以產生已完成的 web 網頁的執行階段。 瀏覽器要求內容的頁面。 [內容] 頁面中，指定要用於頁面結構的 [配置] 頁面，具有程式碼。 在 [配置] 頁面中，內容會插入點位置`RenderBody`呼叫方法。 內容區塊也可插入版面配置頁藉由呼叫`RenderPage`方法，就像在前一節中的方式。 網頁完成時，它會傳送至瀏覽器。

![在執行一個網頁，內含 RenderBody 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image3.jpg)

下列程序示範如何建立版面配置頁面和連結到它的內容頁面。

1. 在  *Shared*資料夾，您的網站，建立名為 *\_Layout1.cshtml*。
2. 以下列內容取代任何現有的內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    您使用`RenderPage`插入內容區塊的版面配置頁中的方法。 版面配置頁可以包含只有一個呼叫`RenderBody`方法。
3. 在  *Shared*資料夾中，建立名為 *\_Header2.cshtml*和任何現有的內容取代為下列：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. 在根資料夾中，建立新的資料夾並將它命名*樣式*。
5. 在 *樣式*資料夾中，建立名為*Site.css*並加入下列樣式定義：

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    這些樣式定義在此僅為顯示如何使用樣式表與版面配置頁。 如果您想，您可以定義這些項目的樣式。
6. 在根資料夾中，建立名為*Content1.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    這是將使用版面配置頁的頁面。 在頁面頂端的程式碼區塊會指出要用來格式化此內容的 [配置] 頁面。
7. 執行*Content1.cshtml*瀏覽器中。 呈現的網頁使用的格式和樣式表中定義 *\_Layout1.cshtml*中定義的文字 （內容） 及*Content1.cshtml*。

    ![[影像]](3-creating-a-consistent-look/_static/image4.jpg)

    您可以重複步驟 6 以建立其他的內容頁面，然後就可以共用相同的 [配置] 頁面。

    > [!NOTE]
    > 您可以設定您的網站，好讓您自動可以使用相同的 [配置] 頁面的資料夾中的所有內容頁面。 如需詳細資訊，請參閱 <<c0> [ 自訂全網站行為適用於 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202906)。

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>有多個內容區段的設計版面配置頁

內容頁面可以有多個區段，這很有用，如果您想要使用已使用可取代內容的多個區域的版面配置。 在 [內容] 頁面中，您提供每個區段的唯一名稱。 (預設區段保留未命名。)在 [配置] 頁面中，您會新增`RenderBody`方法，以指定 [未命名的 （預設值）] 區段應該出現的位置。 然後，您新增個別`RenderSection`方法，以便個別呈現具名的區段。

下圖顯示 ASP.NET 要如何處理分成多個區段的內容。 每個具名的區段會包含在 [內容] 頁面中的區段區塊。 (它們名為`Header`和`List`在範例中。)架構會插入點的 [配置] 頁面中的 [內容] 區段，`RenderSection`呼叫方法。 未命名的 （預設） 一節會插入點位置`RenderBody`呼叫方法，如您稍早所見。

![概念的圖表，顯示 RenderSection 方法如何參考區段插入目前的頁面。](3-creating-a-consistent-look/_static/image5.jpg)

此程序示範如何建立具有多個內容區段的內容頁面，以及如何支援多重內容區段的版面配置頁面進行轉譯。

1. 在  *Shared*資料夾中，建立名為 *\_Layout2.cshtml*。
2. 以下列內容取代任何現有的內容：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    您使用`RenderSection`方法以轉譯標頭和清單的區段。
3. 在根資料夾中，建立名為*Content2.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    此內容的頁面會包含在頁面頂端的程式碼區塊。 每個具名的區段包含區段區塊中。 頁面的其餘部分包含的預設 （未命名） 的內容一節。
4. 執行*Content2.cshtml*瀏覽器中。

    ![在執行一個網頁，內含 RenderSection 方法的呼叫而產生的瀏覽器中顯示頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>進行選擇性的內容區段

一般來說，您建立內容頁面中的區段，就一定要相符 [配置] 頁面中所定義的區段。 如果發生下列任一項動作，可能會收到錯誤：

- [內容] 頁面會包含在 [配置] 頁面中沒有對應的區段的區段。
- [配置] 頁面包含的區段，它沒有任何內容。
- [配置] 頁面包含方法呼叫，其會試圖來呈現該相同的區段，一次以上。

不過，您可以宣告為選擇性，在 [配置] 頁面中的區段來覆寫此行為的具名區段。 這可讓您定義多個內容頁面，可以共用版面配置頁，但可能會或可能沒有特定區段的內容。

1. 開啟*Content2.cshtml*及移除下列區段：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. 儲存頁面，然後執行它在瀏覽器中。 顯示一則錯誤訊息，因為 [內容] 頁面並不提供版面配置頁面，也就是標頭區段中定義的區段的內容。

    ![螢幕擷取畫面顯示如果您執行頁面，就會發生此錯誤的呼叫 RenderSection 方法，但未提供對應的章節。](3-creating-a-consistent-look/_static/image7.jpg)
3. 在  *Shared*資料夾中，開啟 *\_Layout2.cshtml*頁面上，然後取代這一行：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    以下列程式碼：

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    或者，您無法前的一行程式碼取代下列程式碼區塊，這會產生相同的結果：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. 執行*Content2.cshtml*頁面在瀏覽器中一次。 （如果您仍有瀏覽器中開啟此頁面，您可以只重新整理它。）這次頁面會不顯示任何錯誤，即使它有沒有標頭。

## <a name="passing-data-to-layout-pages"></a>將資料傳遞至版面配置頁

您可能必須在 [內容] 頁面，您需要在版面配置頁中參考中定義的資料。 如果是這樣，您需要將 [內容] 頁面中的資料傳遞至版面配置頁。 例如，您可能想要顯示使用者的登入狀態，或您可能想要顯示或隱藏內容區域根據使用者輸入。

若要將從內容頁面的資料傳遞至版面配置頁面中，您可以將值插入`PageData`內容頁面的屬性。 `PageData`屬性會保存您想要在頁面之間傳遞資料的名稱/值組的集合。 在 [配置] 頁面中，您可以接著讀取的值`PageData`屬性。

以下是另一個圖表。 這一個會顯示如何使用 ASP.NET`PageData`屬性來從內容頁面的值傳遞至版面配置頁。 當 ASP.NET 開始建置網頁時，它會建立`PageData`集合。 在 [內容] 頁面中，您可以撰寫程式碼，以將資料放在`PageData`集合。 中的值`PageData`集合也可以存取 [內容] 頁面中的其他章節，或其他內容的區塊。

![說明如何內容頁面可以填入 PageData 字典，並將該資訊傳遞至版面配置頁的概念圖。](3-creating-a-consistent-look/_static/image8.jpg)

下列程序示範如何將資料從內容頁面傳遞至版面配置頁。 當執行網頁時，它會顯示一個按鈕，可讓使用者隱藏或顯示版面配置頁面中定義的清單。 當使用者按一下按鈕時，會設定為 true/false （布林值） 值中`PageData`屬性。 版面配置頁讀取該值，，和如果運算式為 false，會隱藏清單。 值也會在 內容 頁面來判斷是否要顯示**隱藏清單** 按鈕或**顯示清單** 按鈕。

![[影像]](3-creating-a-consistent-look/_static/image9.jpg)

1. 在根資料夾中，建立名為*Content3.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    程式碼會儲存兩組中的資料`PageData`屬性&#8212;網頁和 true 或 false，指定是否要顯示清單的標題。

    請注意，ASP.NET 可讓您將 HTML 標記放在有條件地使用程式碼區塊的頁面。 例如，`if/else`區塊中頁面的主體會決定哪一個表單，以顯示取決於是否`PageData["ShowList"]`設為 true。
2. 在  *Shared*資料夾中，建立名為 *\_Layout3.cshtml*和任何現有的內容取代為下列：

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    [配置] 頁面包含的運算式`<title>`取得的標題值的項目`PageData`屬性。 它也會使用`ShowList`的值`PageData`屬性來判斷是否要顯示的清單內容的區塊。
3. 在  *Shared*資料夾中，建立名為 *\_List.cshtml*和任何現有的內容取代為下列：

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. 執行*Content3.cshtml*瀏覽器中的頁面。 頁面會顯示頁面的左側顯示的清單和**隱藏清單**底部的按鈕。

    ![顯示包含清單和一個按鈕，叫做 「 隱藏清單 」 頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image10.jpg)
5. 按一下 **隱藏清單**。 清單消失，按鈕會變成**顯示清單**。

    ![顯示不包含清單和一個按鈕，叫做 「 顯示清單 」 頁面的螢幕擷取畫面。](3-creating-a-consistent-look/_static/image11.jpg)
6. 按一下 **顯示清單**按鈕和清單就會再次顯示。

## <a name="additional-resources"></a>其他資源


[ASP.NET Web 網頁的自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
