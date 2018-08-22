---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: 顯示視訊在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章說明如何顯示視訊 ASP.NET Web Pages 中使用 Razor 語法的頁面。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 28884d8c4998ea5b00a60bf094f41b915b565bd8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826609"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>在 ASP.NET Web Pages (Razor) 網站中顯示影片
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何在 ASP.NET Web Pages (Razor) 網站中使用 （媒體） 的影片播放程式，讓使用者檢視儲存在站台的影片。 含有 Razor 語法的 ASP.NET Web Pages 可讓您進行此遊戲 Flash (*.swf*)，Media Player (*.wmv*)，和 Silverlight (*.xap*) 影片。
> 
> 您將學到什麼：
> 
> - 如何選擇的視訊播放器。
> - 如何將影片新增至網頁。
> - 如何設定視訊播放程式屬性。
> 
> 這些是 ASP.NET Razor pages 文件中所引進的功能：
> 
> - `Video`協助程式。
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


## <a name="introduction"></a>簡介

您可能想要顯示在您的網站上的影片。 方法之一是連結到已經有段影片中的，例如 YouTube 的站台。 如果您想要直接在自己的網頁中內嵌來自這些網站的影片，您可以通常是從網站取得 HTML 標記，並再將它複製到您的頁面。 例如，下列範例示範如何內嵌的 YouTube 影片：

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

如果您想要播放的視訊，會在您自己的網站 （不在公用的視訊分享網站） 上，您無法直接連結使用像這樣的內嵌的標記。 不過，使用從您的網站播放影片`Video`helper 會呈現的媒體播放程式，直接在網頁中。

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>選擇的視訊播放器

有許多格式的視訊檔案，以及每個格式通常需要不同的播放程式，然後設定播放程式的不同方法。 在 ASP.NET Razor 頁面中，您可以播放視訊時在網頁中使用`Video`協助程式。 `Video`協助程式簡化在網頁中內嵌影片，因為它會自動產生的程序`object`和`embed`通常用來將影片新增至頁面的 HTML 項目。

`Video`協助程式支援下列的媒體播放程式：

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash`的 player`Video`協助程式可讓您播放 Flash 視訊 (*.swf*檔案) 在網頁中。 至少，您必須提供視訊檔案的路徑。 如果您指定只一個路徑，播放程式會使用目前版本的 Flash 所設定的預設值。 典型的預設設定如下：

- 在視訊顯示時使用它的預設寬度和高度，而不需要的背景色彩。
- 在頁面載入時，會自動播放視訊。
- 影片迴圈持續直到明確停止為止。
- 視訊是以顯示所有的影片中，而不是裁剪視訊以符合特定的大小調整。
- 在視窗中播放視訊。

### <a name="the-mediaplayer-player"></a>MediaPlayer 播放程式

`MediaPlayer`的 player`Video`協助程式可讓您播放 Windows Media 視訊 (*.wmv*檔案)，Windows Media audio (*.wma*檔案)，和 MP3 (*.mp3*在網頁中檔案）。 您必須包含播放; 的媒體檔案的路徑所有其他參數是選擇性的。 如果您僅指定路徑，播放程式會使用這類設定的 MediaPlayer，最新版的預設設定：

- 在視訊顯示時使用它的預設寬度和高度。
- 在頁面載入時，會自動播放視訊。
- 視訊播放一次 （它不會執行迴圈）。
- 播放程式顯示使用者介面中的一組完整的控制項。
- 在視窗中播放視訊。

### <a name="the-silverlight-player"></a>Silverlight 播放程式

`Silverlight`的 player`Video`協助程式可讓您播放 Windows Media Video (*.wmv*檔案)，Windows Media Audio (*.wma*檔案)，和 MP3 (*.mp3*檔案）。 您必須設定 path 參數來指向 Silverlight 應用程式封裝 (*.xap*檔案)。 您也必須設定寬度和高度的參數。 所有其他參數皆為選擇性使用。 當您使用 Silverlight 播放程式的影片中，如果您設定必要的參數時，Silverlight 播放程式就會顯示沒有背景色彩的影片。

> [!NOTE]
> 如果您還不知道 Silverlight: *.xap*檔案會壓縮的檔案，包含配置中的指示 *.xaml*的組件和選擇性資源的 managed 程式碼檔。 您可以建立 *.xap*為 Silverlight 應用程式專案的 Visual Studio 中的檔案。


`Silverlight`視訊播放程式會使用您提供的播放程式的設定和中提供的設定都 *.xap*檔案。

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME 類型
> 
> 當瀏覽器下載檔案時，瀏覽器確保檔案類型符合指定所呈現的文件的 MIME 類型。 內容類型或媒體檔案類型的 MIME 類型。 `Video`協助程式會使用下列 MIME 類型：
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>播放視訊 Flash (.swf)

此程序會示範如何名為 Flash 視訊*sample.swf*。 此程序假設您有一個名為資料夾*媒體*您的網站，而且該 *.swf*檔案位在該資料夾。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。
2. 在網站中，加入頁面並將它命名*FlashVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)頁面隨即顯示，並會自動播放視訊。 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

您可以設定`quality`Flash 影片的參數`low`， `autolow`， `autohigh`， `medium`， `high`，和`best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

您可以變更播放在特定大小，使用 Flash 視訊`scale`參數，您可以將下列設定：

- `showall`. 這樣可讓整個視訊看到同時維持原始外觀比例。 不過，您可能會得到每一端上的框線。
- `noorder`. 這樣可以調整視訊同時維持原始外觀比例，但它可能會被裁剪。
- `exactfit`. 這樣可讓整個視訊看到不含維持原始外觀比例，但允許進行扭曲。

如果您未指定`scale`參數，整個視訊會顯示，而不需要任何裁剪會維持原始外觀比例。 下列範例示範如何使用`scale`參數：

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player 支援的視訊模式設定，稱為`windowMode`。 您可以設定為`window`， `opaque`，和`transparent`。 根據預設，`windowMode`設為`window`，其中顯示在不同的視窗，在網頁上的影片。 `opaque`設定會隱藏在網頁上的視訊背後的所有項目。 `transparent`設定可讓影片中，透過顯示網頁的背景假設任何部分視訊透明的。

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>播放 MediaPlayer (*.wmv*) 影片

下列程序會示範如何播放名為視窗 Media video *sample.wmv*位於*媒體*資料夾。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*MediaPlayerVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. 執行網頁瀏覽器中。 載入影片，並會自動播放。 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

您可以設定`playCount`設為整數，表示要自動播放影片多少次：

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode`參數可讓您指定的使用者介面中顯示哪些控制項。 您可以設定`uiMode`要`invisible`， `none`， `mini`，或`full`。 如果您未指定`uiMode`參數，影片將會顯示狀態視窗中，搜尋列中，控制按鈕及音量控制項，加上 [視訊] 視窗。 如果您使用 media player 播放音訊檔案，也會顯示這些控制項。 以下是如何使用的範例`uiMode`參數：

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

根據預設，音訊是上播放視訊時。 您可以藉由設定靜音音訊`mute`參數設為 true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

您可以設定來控制 MediaPlayer 視訊的音訊層級`volume`參數的值介於 0 到 100 之間。 預設值為 50。 以下為範例：

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>播放 Silverlight 影片

此程序會示範如何播放包含 Silverlight 中的影片 *.xap*資料夾中名為頁面*媒體*。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*SilverlightVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. 執行網頁瀏覽器中。 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[Silverlight 的概觀](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash 物件和內嵌的標記屬性](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM 標記](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
