---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: 使用 ASP.NET Web Pages (Razor) 網站中的映像 |Microsoft Docs
author: tfitzmac
description: 本章會示範如何新增、 顯示和操作的映像 （調整大小、 翻轉，再加入浮水印） 在您的網站。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: e609cd1c6ab74b5b40d28bde353501dbacb5d544
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824732"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web Pages (Razor) 網站中的映像
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何新增、 顯示和操作的映像 （調整大小、 翻轉，再加入浮水印），ASP.NET Web Pages (Razor) 網站中。
> 
> 您將學到什麼：
> 
> - 如何以動態方式新增至頁面的影像。
> - 如何讓使用者上傳影像。
> - 如何調整影像大小。
> - 若要翻轉或旋轉影像的方式。
> - 如何將浮水印加入至映像。
> - 如何使用做為浮水印的影像。
> 
> 這些是 ASP.NET 程式設計文章中所引進的功能：
> 
> - `WebImage`協助程式。
> - `Path`物件，提供方法，讓您管理的路徑和檔案名稱。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教學課程也適用於 WebMatrix 3。


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>以動態方式將影像加入至網頁

您可以新增映像至您的網站和個別頁面時您正在開發的網站。 您也可以讓使用者上傳映像，這可能是適用於工作，像是讓他們新增個人檔案相片。

如果映像上已有您的網站，而且只想要顯示在頁面上，您會使用 HTML`<img>`像這樣的項目：

[!code-html[Main](9-working-with-images/samples/sample1.html)]

有時候，不過，您必須是能夠以動態方式顯示映像&#8212;也就是您不知道哪些要顯示的頁面會執行直到映像。

在本節中的程序示範如何讓使用者指定的映像名稱清單的映像檔案名稱的即時顯示影像。 他們從下拉式清單中，選取影像的名稱，並送出頁面上，顯示他們選取的映像。

![[影像]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. 在 WebMatrix 中，建立新的網站。
2. 新增名為頁面*DynamicImage.cshtml*。
3. 在網站的根資料夾中，加入新的資料夾並將它命名*映像*。
4. 新增四個映像*映像*您剛才建立的資料夾。 （任何映像有好用的就可以不過它們應該放置於頁面上）。重新命名映像*Photo1.jpg*， *Photo2.jpg*， *Photo3.jpg*，以及*Photo4.jpg*。 (您不會使用*Photo4.jpg*在此程序中，但您將使用它在本文稍後。)
5. 請確認四個映像會不會標示為唯讀。
6. 以下列內容取代現有的內容頁面中：

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    頁面的主體具有下拉式清單 (`<select>`項目)，名稱為`photoChoice`。 清單中有三種選項，而`value`每個清單選項的屬性具有名稱的其中一個映像，可放入*映像*資料夾。 基本上，清單可讓使用者選取易記的名稱，例如&quot;相片 1&quot;，然後將傳遞 *.jpg*時送出頁面的檔案名稱。

    在程式碼中，您可以取得使用者的選取範圍 （也就是說，影像檔名稱） 從清單中，請閱讀`Request["photoChoice"]`。 您先查看是否有選取項目完全。 如果沒有，您會建構映像檔的資料夾名稱和使用者的映像檔案名稱所組成之影像的路徑。 (如果您嘗試建構的路徑，但沒有任何在`Request["photoChoice"]`，您會收到錯誤。)這會導致這類的相對路徑：

    *映像/Photo1.jpg*

    路徑儲存在名為變數`imagePath`，您將需要更新版本中的頁面。

    在本文中，另外還有`<img>`項目，用來顯示使用者選取的映像。 `src`屬性是否設定為 檔案名稱或 URL，像您一樣顯示靜態項目。 相反地，它會設定為`@imagePath`，這表示，它會從您在程式碼中設定的路徑取得其值。

    第一次執行頁面，不過，沒有任何影像顯示，因為使用者尚未選取任何項目。 這通常表示，`src`屬性會是空白，並將映像會顯示為紅色&quot;x&quot; （或任何瀏覽器呈現時找不到映像）。 若要避免這個問題，您將放`<img>`中的項目`if`區塊，來測試是否`imagePath`變數中有任何項目。 如果使用者所做的選擇，`imagePath`包含的路徑。 如果使用者不選擇映像，或如果這是第一次顯示頁面，`<img>`甚至不呈現項目。
7. 儲存檔案，並在瀏覽器中執行的頁面。 (請確定中選取頁面**檔案**才能執行這個工作區。)
8. 從下拉式清單中選取映像，然後按一下**範例映像**。 請確定您會看到不同的映像，如需不同的選項。

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>上傳影像

前一個範例會示範如何以動態方式，顯示影像，但它只會使用已經在網站的映像。 此程序示範如何讓使用者上傳的影像，接著會顯示在頁面上。 在 ASP.NET 中，您可以操作影像使用`WebImage`協助程式，有方法可讓您建立、 操作和儲存影像。 `WebImage`協助程式支援所有常見 web 映像檔案類型，包括 *.jpg*， *.png*，和 *.bmp*。 在本文中，您將使用 *.jpg*映像，但是您可以使用任何映像類型。

![[影像]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. 加入新的頁面並將它命名*UploadImage.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    在文字本文有`<input type="file">`元素，其可讓使用者選取要上傳的檔案。 當使用者按下**送出**，以及表單送出他們所選擇的檔案。

    若要取得已上傳的映像，您使用`WebImage`協助程式，有各式各樣的有用的方法來處理映像。 具體來說，您可以使用`WebImage.GetImageFromRequest`取得已上傳的映像 （如果有的話），並將其儲存在變數中名為`photo`。

    取得和設定檔案和路徑的名稱，則牽涉到很多工作，在此範例中。 問題是您想要取得使用者上傳，映像的名稱 （與只是名稱），然後再建立您要儲存映像的新路徑。 因為使用者可能無法上傳多個具有相同名稱的映像，您可以使用一些額外的程式碼來建立唯一的名稱，並確定使用者不覆寫現有的圖片。

    如果實際上已上傳影像 (測試`if (photo != null)`)，您取得的映像的映像名稱`FileName`屬性。 當使用者上傳映像，`FileName`包含使用者的原始名稱，其中包含從使用者的電腦的路徑。 它可能會看起來像這樣：

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    您不想該路徑資訊，不過 &#8212;您只想實際的檔案名稱 (*SamplePhoto1.jpg*)。 您可以使用去除只從路徑檔案`Path.GetFileName`方法，就像這樣：

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    然後，您會建立新的唯一檔案名稱將 GUID 加入至原始的名稱。 (如需有關 Guid 的詳細資訊，請參閱[有關 Guid](#SB_AboutGUIDs)本文稍後。)然後，您會建構用來儲存影像，您可以使用的完整路徑。 在儲存路徑組成新的檔案名稱、 資料夾 （映像），以及目前的網站位置。

    > [!NOTE]
    > 為了讓您儲存檔案中的程式碼*映像*資料夾中，應用程式需要讀取-寫入該資料夾的權限。 在您的開發電腦上不通常發生問題。 不過，當您發行您的網站以裝載提供者的 web 伺服器時，您可能需要明確地設定這些權限。 如果您裝載提供者的伺服器上執行此程式碼，而且發生錯誤，請與主機服務提供者，了解如何設定這些權限。

    最後，您將傳遞儲存通往`Save`方法的`WebImage`協助程式。 這會儲存已上傳的映像的新名稱。 儲存方法看起來像這樣： `photo.Save(@"~\" + imagePath)`。 完整路徑會附加至`@"~\"`，這是目前的網站位置。 (如需`~`運算子，請參閱[ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)。)

    如同先前的範例中，包含頁面的主體`<img>`來顯示影像的項目。 如果`imagePath`已設定，`<img>`項目會呈現及其`src`屬性設為`imagePath`值。
3. 執行網頁瀏覽器中。
4. 上傳的映像，並確定它會顯示在頁面上。
5. 在您的網站，開啟*映像*資料夾。 您會看到，新的檔案已加入的檔案名稱看起來像這樣： 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    這是您使用 GUID 做為前置詞的名稱上傳的映像。 (您自己的檔案會有不同的 GUID，並可能名為以外*MyPhoto.png*。)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>有關 Guid
> 
> GUID （全域唯一識別碼） 是識別碼，通常會轉譯成的格式，就像這樣： `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`。 針對每個 GUID，不同的數字和字母 （A-F)，不過都會遵循使用一組 8-4-4-4 到 12 個字元的模式。 （技術上來說，GUID 是 16 位元組/128 位元數字）。當您需要一個 GUID 時，您可以呼叫特定的程式碼會為您產生的 GUID。 Guid 背後的概念在於之間的數字的極大的大小 (3.4 x 10<sup>38</sup>) 及產生它的演算法，產生的數字實際上保證是其中一種。 Guid，因此是當您必須保證您將不會使用相同的名稱兩次，產生的項目名稱的好方法。 它的缺點當然是，Guid 不特別是使用者易記的因此它們通常用於只能在程式碼中使用名稱。


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>調整影像大小

如果您的網站會接受來自使用者的映像，您可能想要調整影像的大小，先顯示，或將它們儲存。 您可以再次使用`WebImage`這個協助程式。

此程序示範如何調整已上傳的映像建立縮圖，然後儲存在網站的 縮圖和原始的映像的大小。 您的頁面上顯示縮圖，並使用重新導向使用者到完整大小的影像的超連結。

![[影像]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. 新增名為頁面*Thumbnail.cshtml*。
2. 在 *映像*資料夾中，建立名為*個大拇指朝*。
3. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    此程式碼是類似上一個範例中的程式碼。 差別在於，此程式碼會將影像儲存兩次，一次通常一次之後建立縮圖影像的複本。 首先取得已上傳的映像，並將它儲存在*映像*資料夾。 然後，您會建構的縮圖影像的新路徑。 若要實際建立縮圖，請呼叫`WebImage`協助程式的`Resize`建立 60 像素 x 60 像素映像的方法。 此範例會示範保留長寬比的方式以及如何避免映像 （如果新的大小會實際放大影像） 放大。 調整大小的影像則會儲存在*個大拇指朝*子資料夾。

    在結束標記時，您可使用`<img>`具有動態項目`src`您看過先前的範例中有條件地顯示影像的屬性。 在此情況下，您還會顯示縮圖。 您也使用`<a>`建立巨量版本的映像的超連結的項目。 如同`src`的屬性`<img>`設定項目`href`屬性`<a>`動態中的項目`imagePath`。 若要確定路徑可做為 URL，您將傳遞`imagePath`至`Html.AttributeEncode`方法，然後將路徑中的保留的字元轉換為 [確定]，在 URL 中的字元。
4. 執行網頁瀏覽器中。
5. 將相片上傳，並確認會顯示縮圖。
6. 按一下以查看完整大小的影像縮圖。
7. 在*映像*並*映像/個大拇指朝*，請注意，已新增新的檔案。

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>旋轉和翻轉影像

`WebImage`協助程式也可讓您翻轉和旋轉的映像。 此程序示範如何從伺服器取得映像、 翻轉影像面朝下 （垂直），並儲存它，然後在頁面上顯示翻轉的影像。 在此範例中，您只使用您已在伺服器的檔案 (*Photo2.jpg*)。 在實際的應用程式中，您可能會翻轉影像像在先前的範例，以動態方式取得其名稱。

![[影像]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. 新增名為頁面*FlipImage.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    此程式碼使用`WebImage`協助程式 」 從伺服器取得映像。 您建立使用相同的技巧，您使用先前範例中，來儲存映像，映像的路徑，而且當您建立的映像使用時，會傳遞該路徑`WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    如果找到映像，您會建構新路徑和檔案名稱，像在先前的範例。 若要翻轉影像，請呼叫`FlipVertical`方法，然後將影像儲存一次。

    頁面上，將映像使用要再次顯示`<img>`具有項目`src`屬性設為`imagePath`。
3. 執行網頁瀏覽器中。 影像*Photo2.jpg*面朝下顯示。
4. 重新整理頁面，或要求頁面，再次以查看映像已翻轉的右端啟動一次。

若要旋轉影像時，您會使用相同的程式碼，不過，而不是呼叫`FlipVertical`或是`FlipHorizontal`，您呼叫`RotateLeft`或`RotateRight`。

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>將浮水印加入至映像

當您將影像新增至您的網站時，您可能想要將浮水印加入至映像，才能將它儲存或顯示頁面上。 人們經常會使用浮水印將著作權資訊加入至映像，或通告其商務名稱。

![[影像]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. 新增名為頁面*Watermark.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    此程式碼就像中的程式碼*FlipImage.cshtml*稍早的頁面 (雖然這次會使用*Photo3.jpg*檔案)。 若要新增浮水印，請呼叫`WebImage`協助程式的`AddTextWatermark`儲存映像之前的方法。 在呼叫`AddTextWatermark`，傳遞文字&quot;我的浮水印&quot;設定字型色彩為黃色，且新細明體設定的字型系列。 (雖然它未顯示在這裡，`WebImage`協助程式也可讓您指定不透明度、 字型家族和字型的大小和浮水印文字的位置。)當您儲存映像時它不能唯讀狀態。

    如您所見之前，以您使用輸入頁面上顯示的映像`<img>`元素的 src 屬性設定為`@imagePath`。
3. 執行網頁瀏覽器中。 右下角的 映像，請注意 「 我的浮水印 」 的文字。

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>使用做為浮水印的影像

而不是使用浮水印文字，您可以使用另一個映像。 人們有時會使用公司標誌等影像做為浮水印，或它們而不是文字浮水印影像用於著作權資訊。

![[影像]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. 新增名為頁面*ImageWatermark.cshtml*。
2. 新增映像*映像*資料夾，您可以用來作為標誌，並重新命名映像*MyCompanyLogo.jpg*。 此映像應該是您可以清楚地查看當它設定為 80 個像素寬且 20 像素高的映像。
3. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    這是從先前的範例程式碼的另一種變化。 在此情況下，您呼叫`AddImageWatermark`將浮水印影像新增至目標映像 (*Photo3.jpg*) 才能儲存映像。 當您呼叫`AddImageWatermark`，您將其寬度設定為 80 的像素、 高度設為 20 個像素。 *MyCompanyLogo.jpg*映像會水平置中對齊和垂直靠下對齊的目標映像。 不透明度設定為 100%和邊框距離設定為 10 個像素。 如果浮水印影像大於目標映像，會發生任何事。 如果浮水印影像大於目標映像，您將設定為零的映像浮水印的邊框距離浮水印會被忽略。

    因為您之前，顯示映像使用`<img>`項目，並動態`src`屬性。
4. 執行網頁瀏覽器中。 浮水印影像會出現在主要映像底部的通知。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[使用 ASP.NET Web Pages 網站中的檔案](https://go.microsoft.com/fwlink/?LinkId=202896)

[程式設計使用 Razor 語法的 ASP.NET Web Pages 簡介](https://go.microsoft.com/fwlink/?LinkID=251587)
