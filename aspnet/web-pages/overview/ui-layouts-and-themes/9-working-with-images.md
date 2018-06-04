---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: 使用 ASP.NET Web Pages (Razor) 網站中的映像 |Microsoft 文件
author: tfitzmac
description: 本章節將說明如何新增、 顯示及操控映像 （調整大小、 翻轉，和新增浮水印） 在您的網站。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530147"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web Pages (Razor) 網站中的影像
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文將說明如何新增、 顯示及操控映像 （調整大小、 翻轉，和新增浮水印） 中 ASP.NET Web Pages (Razor) 網站。
> 
> 您將學習：
> 
> - 如何以動態方式將影像加入至頁面。
> - 如何讓使用者上傳的影像。
> - 如何調整影像大小。
> - 若要翻轉或旋轉影像的方式。
> - 如何新增浮水印影像。
> - 如何使用做為浮水印的影像。
> 
> 這些是 ASP.NET 程式設計文件中所引進的功能：
> 
> - `WebImage`協助程式。
> - `Path`物件，提供可讓您操作路徑和檔案名稱的方法。
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
## <a name="adding-an-image-to-a-web-page-dynamically"></a>以動態方式將影像加入至 Web 網頁

您可以將影像加入至您的網站和個別頁面雖然您正在開發的網站。 您也可以讓使用者上傳映像，這可能是適用於工作，例如但是讓他們新增設定檔照片。

如果影像是已經在網站上，而且您只想要顯示在頁面上，您會使用 HTML`<img>`像這樣的項目：

[!code-html[Main](9-working-with-images/samples/sample1.html)]

有時候，不過，您需要能夠以動態方式顯示影像 & #8212;也就是說，您不知道何種映像之前的頁面，顯示 正在執行。

本節中的程序示範如何讓使用者指定影像的檔案名稱，從映像名稱的清單，立即顯示影像。 他們從下拉式清單中，選取影像的名稱，當它們送出此頁面，會顯示在選取的映像。

![[影像]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. 在 WebMatrix 中，建立新的網站。
2. 加入新的頁面名稱為*DynamicImage.cshtml*。
3. 在網站的根資料夾中，加入新的資料夾並將其命名*映像*。
4. 新增四個映像以*映像*您剛才建立的資料夾。 （任何映像具有方便將執行作業，但它們應該符合網頁）。重新命名圖像*Photo1.jpg*， *Photo2.jpg*， *Photo3.jpg*，和*Photo4.jpg*。 (您不會使用*Photo4.jpg*在這個程序，但您會用到它在本文稍後。)
5. 請確認，四個映像未標示為唯讀。
6. 以下列內容取代現有的內容頁面中：

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    頁面主體具有下拉式清單 (`<select>`項目)，名稱為`photoChoice`。 清單中有三個選項，而`value`每個清單選項的屬性具有名稱的其中一個您放在映像*映像*資料夾。 基本上，清單可讓使用者選取的好記的名稱，例如&quot;相片 1&quot;，然後將傳遞 *.jpg*時送出頁面的檔案名稱。

    在程式碼中，您可以取得使用者的選取範圍 （亦即，映像檔案名稱） 從清單讀取`Request["photoChoice"]`。 您先查看是否有選取項目完全。 如果沒有，您會建構為映像的資料夾名稱和使用者的映像檔案名稱所組成的映像的路徑。 (如果您嘗試建構的路徑，但沒有任何在`Request["photoChoice"]`，就會收到錯誤。)像這樣的相對路徑的結果：

    *映像/Photo1.jpg*

    路徑儲存在名為變數`imagePath`，您必須稍後在頁面中。

    在本文中，另外還有`<img>`用來顯示影像，為使用者選取的項目。 `src`屬性未設定為 檔案名稱或 URL，像是您一樣顯示靜態項目。 相反地，若要設定`@imagePath`，這表示它從您在程式碼中設定的路徑，取得其值。

    第一次執行網頁時，不過，沒有任何影像顯示，因為使用者尚未選取任何項目。 這通常表示，`src`屬性會為空白，且影像會顯示為紅色&quot;x&quot; （或任何瀏覽器呈現時找不到映像）。 若要避免這個問題，您將放`<img>`中的項目`if`區塊，請參閱程序會測試是否`imagePath`變數中有任何項目。 如果使用者所做的選擇，`imagePath`包含路徑。 如果使用者沒有挑選一個映像，或如果這是第一次顯示頁面，`<img>`根本不呈現項目。
7. 儲存檔案，並在瀏覽器中執行網頁。 (請確定中選取頁面**檔案**才能執行這個工作區。)
8. 從下拉式清單中選取映像，然後按一下**範例影像**。 請確定您會看到不同的選擇不同的映像。

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>上傳映像

前一個範例會示範如何以動態方式，顯示影像，但它只能使用已經在網站上的映像。 此程序示範如何讓使用者上傳映像，即會顯示在頁面上。 在 ASP.NET 中，您可以上操控映像使用即時`WebImage`helper，有方法可讓您建立、 操作及儲存映像。 `WebImage`協助程式支援所有常見 web 影像檔案類型，包括 *.jpg*， *.png*，和 *.bmp*。 在本文中，您將使用 *.jpg*映像，但是您可以使用任何影像類型。

![[影像]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. 加入新的頁面並將其命名*UploadImage.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    文字本文有`<input type="file">`元素，其可讓使用者選取要上傳的檔案。 當使用者按**送出**，以及表單送出它們挑選的檔案。

    若要取得已上傳的映像，您使用`WebImage`helper，有各式各樣的有用的方法以處理映像。 具體而言，使用`WebImage.GetImageFromRequest`取得上傳的影像 （如果有的話），並將其儲存在名為`photo`。

    取得和設定檔案及路徑名稱，牽涉到許多在此範例中工作。 問題在於您想要取得使用者上傳，映像的名稱 （和只有名稱），然後再建立用於您要儲存映像的新路徑。 因為使用者可能無法上傳多個具有相同名稱的映像，您可以使用額外的程式碼進行一些建立唯一的名稱，以確保使用者不覆寫現有的圖片。

    如果影像實際上已上傳 (測試`if (photo != null)`)，就從映像的映像名稱`FileName`屬性。 當使用者上傳映像，`FileName`包含使用者的原始名稱，其中包含從使用者的電腦的路徑。 它看起來可能像這樣：

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    您不想該路徑資訊，不過 & #8212;您只想實際的檔案名稱 (*SamplePhoto1.jpg*)。 您可以使用帶狀出該檔案的路徑`Path.GetFileName`方法，就像這樣：

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    然後，您會建立新的唯一檔案名稱加入 GUID 的原始名稱。 (如需詳細資訊的 Guid，請參閱[有關 Guid](#SB_AboutGUIDs)本文稍後。)然後您建構可讓您儲存的映像的完整路徑。 儲存在由新的檔案名稱的資料夾 （影像） 及目前的網站位置的路徑所組成。

    > [!NOTE]
    > 為了讓您的程式碼儲存在檔案*映像*資料夾中，應用程式需要讀取-寫入該資料夾的權限。 在開發電腦上這不通常是發生問題。 不過，當您將您的網站發行至裝載提供者的 web 伺服器時，您可能需要明確設定這些權限。 如果您在裝載提供者的伺服器上執行此程式碼，並發生錯誤，請洽詢主機服務提供者了解如何設定這些權限。

    最後，您將傳遞儲存路徑`Save`方法`WebImage`協助程式。 這會儲存已上傳的映像，新名稱。 儲存方法看起來像這樣： `photo.Save(@"~\" + imagePath)`。 完整路徑會附加至`@"~\"`，這是目前的網站位置。 (如需有關資訊`~`運算子，請參閱[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)。)

    如同先前的範例，頁面主體包含`<img>`来顯示的影像項目。 如果`imagePath`已設定，`<img>`項目會呈現及其`src`屬性設為`imagePath`值。
3. 執行網頁瀏覽器中。
4. 上傳映像，並確定它會顯示在頁面上。
5. 在您的網站，開啟*映像*資料夾。 您會看到，新的檔案已加入的檔案名稱看起來像這樣： 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    這是您上傳具有 GUID 的名稱前置詞的映像。 (您自己的檔案會有不同的 GUID，並可能名為以外*MyPhoto.png*。)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>有關 Guid
> 
> GUID （全域唯一識別碼） 是識別碼，通常會轉譯格式如下： `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`。 針對每個 GUID，不同的數字和字母 （A-F)，但是它們都依照使用群組的 8-4-4-4-12 個字元的模式。 （技術上來說，GUID 是 16 位元組/128 位元數目）。當您需要 GUID 時，您可以呼叫會產生一個 GUID，為您的特定程式碼。 Guid 背後的概念在於之間的數字龐大大小 (3.4 x 10<sup>38</sup>)，而且產生的演算法，產生的數字幾乎保證是其中一種。 Guid，因此是產生的項目名稱，當您必須保證您將不會使用相同的名稱是兩次的好方法。 這個選項的缺點當然是 Guid 是的特別使用者易記的因此它們通常用於只能在程式碼中使用名稱。


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>調整影像大小

如果您的網站會接受來自使用者的映像，您可能要調整影像的大小，然後才顯示，或將它們儲存。 您可以再次使用`WebImage`這個協助程式。

此程序顯示如何調整大小建立縮圖，然後將縮圖和原始的映像儲存在網站上傳映像。 您的頁面上顯示縮圖，並將使用者重新導向至全尺寸的映像使用超連結。

![[影像]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. 加入新的頁面名稱為*Thumbnail.cshtml*。
2. 在*映像*資料夾中，建立名稱為的子*使*。
3. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    此程式碼是從先前的範例類似的程式碼。 兩者的差異在於，此程式碼會將影像儲存兩次，一次通常一次之後您建立縮圖影像的複本。 先取得已上傳映像，並儲存在*映像*資料夾。 然後，您可以建構在縮圖影像的新路徑。 若要實際建立縮圖，請呼叫`WebImage`協助專家的`Resize`方法來建立 60 像素 x 60 像素的影像。 這個範例顯示如何您保留外觀比例如何防止影像 （如果新的大小會實際設定更大的映像） 放大。 調整大小的影像則儲存在*使*子資料夾。

    在結束標記時，您使用的相同`<img>`具有動態項目`src`您所見前述範例中，有條件地顯示影像的屬性。 在此情況下，您可以顯示縮圖。 您也使用`<a>`建立大版本的映像的超連結的項目。 如同`src`屬性`<img>`設定項目，`href`屬性`<a>`項目，以動態方式來指定任何處於`imagePath`。 若要確定路徑可做為 URL，您將傳遞`imagePath`至`Html.AttributeEncode`方法，然後將路徑中的保留的字元轉換為 [確定]，在 URL 中的字元。
4. 執行網頁瀏覽器中。
5. 上傳相片，並確認已顯示縮圖。
6. 按一下以查看完整的影像縮圖。
7. 在*映像*和*映像/使*，請注意，已加入新的檔案。

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>輪替與翻轉影像

`WebImage` Helper 也可讓您翻轉和旋轉的影像。 此程序示範如何從伺服器取得映像、 翻轉影像面朝下 （垂直）、 加以儲存，並接著顯示在頁面上的 翻轉的影像。 在此範例中，您只會使用您已經在伺服器的檔案 (*Photo2.jpg*)。 在實際應用中，您可能會翻轉影像如同上述範例中，以動態方式取得其名稱。

![[影像]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. 加入新的頁面名稱為*FlipImage.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    程式碼會使用`WebImage`從伺服器取得映像的協助程式。 您建立使用相同的技巧，先前範例中使用儲存映像的映像的路徑並建立映像使用時，會傳遞該路徑`WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    如果找到映像，您可以建構新路徑和檔案名稱，如同先前範例中。 若要翻轉影像，請呼叫`FlipVertical`方法，然後將映像儲存一次。

    映像再次顯示在頁面上使用`<img>`具有項目`src`屬性設為`imagePath`。
3. 執行網頁瀏覽器中。 映像*Photo2.jpg*面朝下顯示。
4. 重新整理頁面，或要求頁面，再次以查看映像已翻轉的右側，即啟動一次。

若要旋轉影像，您可以使用相同的程式碼，不過，而不是呼叫`FlipVertical`或`FlipHorizontal`，您呼叫`RotateLeft`或`RotateRight`。

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>新增浮水印影像

當您將影像加入至您的網站時，您可能要先儲存它，或將它顯示在頁面上，新增浮水印影像。 若要將著作權資訊加入至映像或通告其商務名稱人們經常會使用浮水印。

![[影像]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. 加入新的頁面名稱為*Watermark.cshtml*。
2. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    此程式碼就像中的程式碼*FlipImage.cshtml*從先前的頁面 (雖然這次使用*Photo3.jpg*檔案)。 若要新增浮水印，請呼叫`WebImage`協助專家的`AddTextWatermark`方法之前儲存的映像。 在呼叫`AddTextWatermark`，傳送文字&quot;我浮水印&quot;、 設定字型色彩為黃色，並設定新細明體字型家族。 (雖然它未顯示在這裡， `WebImage` helper 也可讓您指定的不透明度、 字型系列和字型大小和浮水印文字的位置。)當您儲存映像時它不得為唯讀。

    映像如您所見之前，會顯示在頁面上使用`<img>`元素具有的 src 屬性設定為`@imagePath`。
3. 執行網頁瀏覽器中。 右下角的 映像，請注意 「 我的配額上限 」 的文字。

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>使用做為浮水印的影像

而不是使用浮水印文字，您可以使用另一個映像。 人員有時使用像公司標誌的影像做為浮水印，或者使用浮水印影像而不是文字的著作權資訊。

![[影像]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. 加入新的頁面名稱為*ImageWatermark.cshtml*。
2. 將映像加入*映像*資料夾，您可以做為標誌，並將影像重新命名*MyCompanyLogo.jpg*。 此映像應該是您可以清楚地查看當其設為 80 個像素寬和高 20 像素的影像。
3. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    這是從先前的範例程式碼的另一種變異。 在此情況下，您呼叫`AddImageWatermark`浮水印影像新增至目標映像 (*Photo3.jpg*) 儲存映像之前。 當您呼叫`AddImageWatermark`，您將其寬度設定為 80 的像素並到 20 像素的高度。 *MyCompanyLogo.jpg*映像會水平置中對齊和目標映像底部垂直對齊。 不透明度設定為 100%，填補設定為 10 個像素為單位。 浮水印影像大於目標映像時，會發生任何事。 如果浮水印影像大於目標映像，並且設定為零的映像浮水印的填補，會忽略浮水印。

    因為之前，您可以顯示映像使用`<img>`項目和動態`src`屬性。
4. 執行網頁瀏覽器中。 浮水印影像會出現在主要映像底部的通知。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[使用中的 ASP.NET Web Pages 站台中的檔案](https://go.microsoft.com/fwlink/?LinkId=202896)

[介紹程式設計使用 Razor 語法的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=251587)
