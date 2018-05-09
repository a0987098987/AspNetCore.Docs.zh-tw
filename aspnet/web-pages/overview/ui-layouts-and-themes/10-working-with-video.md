---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: 視訊顯示在 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 本章節說明如何顯示視訊 ASP.NET Web Pages 中使用 Razor 語法的頁面。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>在 ASP.NET Web Pages (Razor) 網站中顯示視訊
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在 ASP.NET Web Pages (Razor) 網站中使用 （媒體） 視訊播放程式，讓使用者檢視儲存在站台的視訊。 含有 Razor 語法的 ASP.NET Web Pages 可讓您播放 Flash (*.swf*)，Media Player (*.wmv*)，和 Silverlight (*.xap*) 影片。
> 
> 您將學習：
> 
> - 如何選擇視訊播放程式。
> - 如何新增到網頁上的視訊。
> - 如何設定影片播放器屬性。
> 
> 這些是 ASP.NET Razor 頁面文件中所引進的功能：
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

您可以在您的網站上顯示視訊。 若要這樣做的方法之一是連結至已使用的視訊，像是 YouTube 站台。 如果您想要直接在自己的網頁中內嵌的影片，從這些站台，通常是從網站取得 HTML 標記，並再將它複製到您的頁面。 例如，下列範例示範如何內嵌的 YouTube 影片：

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

如果您想要播放的視訊，會在您自己的網站 （不在公用的視訊分享網站），您無法直接連結到它使用像這樣的內嵌的標記。 不過，使用從您的網站播放視訊`Video`helper，呈現媒體播放程式，直接在網頁中的。

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>選擇影片播放器

有許多的視訊檔案、 格式和每一個格式通常需要不同的播放程式，並設定播放程式的不同方法。 在 ASP.NET Razor 頁面中，您也可以在網頁中使用的視訊`Video`協助程式。 `Video`協助程式簡化程序的影片內嵌在網頁中，因為它會自動產生`object`和`embed`通常用來將視訊新增至網頁的 HTML 項目。

`Video`協助程式支援下列的媒體播放程式：

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash`玩家`Video`協助程式可讓您播放快閃視訊 (*.swf*檔案) 在網頁中。 最少，您必須提供視訊檔案的路徑。 如果您指定任何內容但路徑時，播放程式會使用最新版 Flash 所設定的預設值。 典型的預設設定如下：

- 視訊顯示其預設寬度和高度使用和不背景色彩。
- 載入頁面時，會自動播放視訊。
- 視訊迴圈持續直到明確停止為止。
- 視訊是以顯示所有的視訊，而非視訊時會裁剪以符合特定的大小調整。
- 在視窗中播放視訊。

### <a name="the-mediaplayer-player"></a>MediaPlayer 播放程式

`MediaPlayer`玩家`Video`協助程式可讓您播放 Windows Media 視訊 (*.wmv*檔案)，Windows Media audio (*.wma*檔案)，與 MP3 (*.mp3*在網頁中檔案）。 您必須包含要播放; 的媒體檔案的路徑所有其他參數是選擇性的。 如果您僅指定路徑，播放程式會使用預設設定的 MediaPlayer，目前的版本設定，例如：

- 使用其預設寬度和高度顯示視訊。
- 載入頁面時，會自動播放視訊。
- 該影片播放一次 （它不會執行迴圈）。
- 播放程式顯示使用者介面中的一組完整的控制項。
- 在視窗中播放視訊。

### <a name="the-silverlight-player"></a>Silverlight 播放程式

`Silverlight`玩家`Video`協助程式可讓您播放 Windows Media Video (*.wmv*檔案)，Windows Media Audio (*.wma*檔案)，與 MP3 (*.mp3*檔案）。 您必須設定 path 參數來指向 Silverlight 架構應用程式封裝 (*.xap*檔案)。 您也必須設定寬度和高度參數。 所有其他參數皆為選擇性使用。 當您使用 Silverlight 播放程式的視訊，如果您設定必要的參數時，Silverlight 播放程式會顯示沒有背景色彩的視訊。

> [!NOTE]
> 如果您不知道 Silverlight: *.xap*檔案是壓縮的檔案，其中包含中的版面配置指示 *.xaml*檔案，managed 程式碼的組件和選擇性的資源。 您可以建立 *.xap*為 Silverlight 應用程式專案的 Visual Studio 中的檔案。


`Silverlight`視訊播放程式會使用您提供的播放程式的設定以及中所提供的設定 *.xap*檔案。

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME 類型
> 
> 當瀏覽器下載檔案時，瀏覽器可確保檔案類型符合指定所呈現之文件的 MIME 類型。 MIME 類型會是檔案的內容型別或媒體類型。 `Video`協助程式會使用下列 MIME 類型：
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>播放視訊 Flash (.swf)

此程序會示範如何播放名為快閃視訊*sample.swf*。 此程序假設了名為的資料夾*媒體*您的網站，而且該 *.swf*檔案位於該資料夾中。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。
2. 在網站上加入頁面並將其命名*FlashVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. 執行網頁瀏覽器中。 (請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示，而且會自動播放視訊。 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

您可以設定`quality`快閃影片，以參數`low`， `autolow`， `autohigh`， `medium`， `high`，和`best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

您可以變更播放在特定的大小，使用快閃視訊`scale`參數，您可以將下列設定：

- `showall`. 這樣可讓整個視訊看到同時維持原始外觀比例。 不過，您最後可能會在每一端上的框線。
- `noorder`. 這會調整視訊同時維持原始外觀比例，但它可能會被裁剪。
- `exactfit`. 這樣可讓整個視訊顯示而不維持原始外觀比例，但是扭曲程度可能會發生。

如果您未指定`scale`參數時，整個視訊會顯示，不含任何裁剪會維持原始外觀比例。 下列範例示範如何使用`scale`參數：

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player 支援的視訊模式設定，稱為`windowMode`。 您可以設定為`window`， `opaque`，和`transparent`。 根據預設，`windowMode`設`window`，在網頁上的個別視窗中顯示視訊。 `opaque`設定隱藏在網頁上的視訊背後的所有項目。 `transparent`設定可讓透過視訊顯示網頁的背景假設任何部分視訊透明的。

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>播放 MediaPlayer (*.wmv*) 影片

下列程序會示範如何播放名為視窗 Media video *sample.wmv*位於*媒體*資料夾。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*MediaPlayerVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. 執行網頁瀏覽器中。 視訊載入，並會自動播放。 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

您可以設定`playCount`為整數，指出多少次自動播放視訊：

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode`參數可讓您指定的使用者介面中顯示的控制項。 您可以設定`uiMode`至`invisible`， `none`， `mini`，或`full`。 如果您未指定`uiMode`參數時，視訊會顯示狀態視窗中，搜尋列、 控制按鈕及音量控制項，除了 [視訊] 視窗。 如果您使用 media player 播放音訊檔案，也會顯示這些控制項。 以下是如何使用的範例`uiMode`參數：

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

根據預設，音訊會播放視訊時。 您可以藉由設定靜音音訊`mute`參數為 true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

您可以設定來控制 MediaPlayer 視訊的音量大小`volume`參數的值介於 0 到 100 之間。 預設值為 50。 以下為範例：

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>播放 Silverlight 視訊

此程序會示範如何播放包含在 Silverlight 中的視訊 *.xap*頁面的資料夾中名為*媒體*。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*SilverlightVideo.cshtml*。
3. 將下列標記加入頁面： 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. 執行網頁瀏覽器中。 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[Silverlight 概觀](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[快閃物件和內嵌標記屬性](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM 標記](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
