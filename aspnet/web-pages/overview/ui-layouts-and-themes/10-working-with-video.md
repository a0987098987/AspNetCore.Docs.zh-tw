---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: "視訊顯示在 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本章節說明如何顯示視訊 ASP.NET Web Pages 中使用 Razor 語法的頁面。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 0e1849fb780908b55520d8108e2227d046759987
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="03364-103">在 ASP.NET Web Pages (Razor) 網站中顯示視訊</span><span class="sxs-lookup"><span data-stu-id="03364-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="03364-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="03364-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="03364-105">本文說明如何在 ASP.NET Web Pages (Razor) 網站中使用 （媒體） 視訊播放程式，讓使用者檢視儲存在站台的視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="03364-106">含有 Razor 語法的 ASP.NET Web Pages 可讓您播放 Flash (*.swf*)，Media Player (*.wmv*)，和 Silverlight (*.xap*) 影片。</span><span class="sxs-lookup"><span data-stu-id="03364-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="03364-107">您將學習：</span><span class="sxs-lookup"><span data-stu-id="03364-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="03364-108">如何選擇視訊播放程式。</span><span class="sxs-lookup"><span data-stu-id="03364-108">How to choose a video player.</span></span>
> - <span data-ttu-id="03364-109">如何新增到網頁上的視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="03364-110">如何設定影片播放器屬性。</span><span class="sxs-lookup"><span data-stu-id="03364-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="03364-111">這些是 ASP.NET Razor 頁面文件中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="03364-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="03364-112">`Video`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03364-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="03364-113">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="03364-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="03364-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="03364-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="03364-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="03364-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="03364-116">本教學課程也適用於 WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="03364-116">This tutorial also works with WebMatrix 3.</span></span>


## <a name="introduction"></a><span data-ttu-id="03364-117">簡介</span><span class="sxs-lookup"><span data-stu-id="03364-117">Introduction</span></span>

<span data-ttu-id="03364-118">您可以在您的網站上顯示視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-118">You might want to display a video on your site.</span></span> <span data-ttu-id="03364-119">若要這樣做的方法之一是連結至已使用的視訊，像是 YouTube 站台。</span><span class="sxs-lookup"><span data-stu-id="03364-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="03364-120">如果您想要直接在自己的網頁中內嵌的影片，從這些站台，通常是從網站取得 HTML 標記，並再將它複製到您的頁面。</span><span class="sxs-lookup"><span data-stu-id="03364-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="03364-121">例如，下列範例示範如何內嵌的 YouTube 影片：</span><span class="sxs-lookup"><span data-stu-id="03364-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="03364-122">如果您想要播放的視訊，會在您自己的網站 （不在公用的視訊分享網站），您無法直接連結到它使用像這樣的內嵌的標記。</span><span class="sxs-lookup"><span data-stu-id="03364-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="03364-123">不過，使用從您的網站播放視訊`Video`helper，呈現媒體播放程式，直接在網頁中的。</span><span class="sxs-lookup"><span data-stu-id="03364-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="03364-124">選擇影片播放器</span><span class="sxs-lookup"><span data-stu-id="03364-124">Choosing a Video Player</span></span>

<span data-ttu-id="03364-125">有許多的視訊檔案、 格式和每一個格式通常需要不同的播放程式，並設定播放程式的不同方法。</span><span class="sxs-lookup"><span data-stu-id="03364-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="03364-126">在 ASP.NET Razor 頁面中，您也可以在網頁中使用的視訊`Video`協助程式。</span><span class="sxs-lookup"><span data-stu-id="03364-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="03364-127">`Video`協助程式簡化程序的影片內嵌在網頁中，因為它會自動產生`object`和`embed`通常用來將視訊新增至網頁的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="03364-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="03364-128">`Video`協助程式支援下列的媒體播放程式：</span><span class="sxs-lookup"><span data-stu-id="03364-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="03364-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="03364-129">Adobe Flash</span></span>
- <span data-ttu-id="03364-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="03364-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="03364-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="03364-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="03364-132">Flash Player</span><span class="sxs-lookup"><span data-stu-id="03364-132">The Flash Player</span></span>

<span data-ttu-id="03364-133">`Flash`玩家`Video`協助程式可讓您播放快閃視訊 (*.swf*檔案) 在網頁中。</span><span class="sxs-lookup"><span data-stu-id="03364-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="03364-134">最少，您必須提供視訊檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="03364-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="03364-135">如果您指定任何內容但路徑時，播放程式會使用最新版 Flash 所設定的預設值。</span><span class="sxs-lookup"><span data-stu-id="03364-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="03364-136">典型的預設設定如下：</span><span class="sxs-lookup"><span data-stu-id="03364-136">Typical default settings are:</span></span>

- <span data-ttu-id="03364-137">視訊顯示其預設寬度和高度使用和不背景色彩。</span><span class="sxs-lookup"><span data-stu-id="03364-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="03364-138">載入頁面時，會自動播放視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="03364-139">視訊迴圈持續直到明確停止為止。</span><span class="sxs-lookup"><span data-stu-id="03364-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="03364-140">視訊是以顯示所有的視訊，而非視訊時會裁剪以符合特定的大小調整。</span><span class="sxs-lookup"><span data-stu-id="03364-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="03364-141">在視窗中播放視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="03364-142">MediaPlayer 播放程式</span><span class="sxs-lookup"><span data-stu-id="03364-142">The MediaPlayer Player</span></span>

<span data-ttu-id="03364-143">`MediaPlayer`玩家`Video`協助程式可讓您播放 Windows Media 視訊 (*.wmv*檔案)，Windows Media audio (*.wma*檔案)，與 MP3 (*.mp3*在網頁中檔案）。</span><span class="sxs-lookup"><span data-stu-id="03364-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="03364-144">您必須包含要播放; 的媒體檔案的路徑所有其他參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="03364-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="03364-145">如果您僅指定路徑，播放程式會使用預設設定的 MediaPlayer，目前的版本設定，例如：</span><span class="sxs-lookup"><span data-stu-id="03364-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="03364-146">使用其預設寬度和高度顯示視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="03364-147">載入頁面時，會自動播放視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="03364-148">該影片播放一次 （它不會執行迴圈）。</span><span class="sxs-lookup"><span data-stu-id="03364-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="03364-149">播放程式顯示使用者介面中的一組完整的控制項。</span><span class="sxs-lookup"><span data-stu-id="03364-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="03364-150">在視窗中，在播放視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-150">The video plays in in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="03364-151">Silverlight 播放程式</span><span class="sxs-lookup"><span data-stu-id="03364-151">The Silverlight Player</span></span>

<span data-ttu-id="03364-152">`Silverlight`玩家`Video`協助程式可讓您播放 Windows Media Video (*.wmv*檔案)，Windows Media Audio (*.wma*檔案)，與 MP3 (*.mp3*檔案）。</span><span class="sxs-lookup"><span data-stu-id="03364-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="03364-153">您必須設定 path 參數來指向 Silverlight 架構應用程式封裝 (*.xap*檔案)。</span><span class="sxs-lookup"><span data-stu-id="03364-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="03364-154">您也必須設定寬度和高度參數。</span><span class="sxs-lookup"><span data-stu-id="03364-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="03364-155">所有其他參數皆為選擇性使用。</span><span class="sxs-lookup"><span data-stu-id="03364-155">All other parameters are optional.</span></span> <span data-ttu-id="03364-156">當您使用 Silverlight 播放程式的視訊，如果您設定必要的參數時，Silverlight 播放程式會顯示沒有背景色彩的視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="03364-157">如果您不知道 Silverlight: *.xap*檔案是壓縮的檔案，其中包含中的版面配置指示*.xaml*檔案，managed 程式碼的組件和選擇性的資源。</span><span class="sxs-lookup"><span data-stu-id="03364-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="03364-158">您可以建立*.xap*為 Silverlight 應用程式專案的 Visual Studio 中的檔案。</span><span class="sxs-lookup"><span data-stu-id="03364-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>


<span data-ttu-id="03364-159">`Silverlight`視訊播放程式會使用您提供的播放程式的設定以及中所提供的設定*.xap*檔案。</span><span class="sxs-lookup"><span data-stu-id="03364-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="03364-160">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="03364-160">MIME Types</span></span>
> 
> <span data-ttu-id="03364-161">當瀏覽器下載檔案時，瀏覽器可確保檔案類型符合指定所呈現之文件的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="03364-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="03364-162">MIME 類型會是檔案的內容型別或媒體類型。</span><span class="sxs-lookup"><span data-stu-id="03364-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="03364-163">`Video`協助程式會使用下列 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="03364-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="03364-164">播放視訊 Flash (.swf)</span><span class="sxs-lookup"><span data-stu-id="03364-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="03364-165">此程序會示範如何播放名為快閃視訊*sample.swf*。</span><span class="sxs-lookup"><span data-stu-id="03364-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="03364-166">此程序假設了名為的資料夾*媒體*您的網站，而且該*.swf*檔案位於該資料夾中。</span><span class="sxs-lookup"><span data-stu-id="03364-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="03364-167">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。</span><span class="sxs-lookup"><span data-stu-id="03364-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="03364-168">在網站上加入頁面並將其命名*FlashVideo.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="03364-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="03364-169">將下列標記加入頁面：</span><span class="sxs-lookup"><span data-stu-id="03364-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="03364-170">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="03364-170">Run the page in a browser.</span></span> <span data-ttu-id="03364-171">(請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示，而且會自動播放視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="03364-172">![[影像]] (10-working-with-video/_static/image1.jpg "ch08_video 1.jpg")</span><span class="sxs-lookup"><span data-stu-id="03364-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="03364-173">您可以設定`quality`快閃影片，以參數`low`， `autolow`， `autohigh`， `medium`， `high`，和`best`:</span><span class="sxs-lookup"><span data-stu-id="03364-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="03364-174">您可以變更播放在特定的大小，使用快閃視訊`scale`參數，您可以將下列設定：</span><span class="sxs-lookup"><span data-stu-id="03364-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="03364-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="03364-175">`showall`.</span></span> <span data-ttu-id="03364-176">這樣可讓整個視訊看到同時維持原始外觀比例。</span><span class="sxs-lookup"><span data-stu-id="03364-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="03364-177">不過，您最後可能會在每一端上的框線。</span><span class="sxs-lookup"><span data-stu-id="03364-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="03364-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="03364-178">`noorder`.</span></span> <span data-ttu-id="03364-179">這會調整視訊同時維持原始外觀比例，但它可能會被裁剪。</span><span class="sxs-lookup"><span data-stu-id="03364-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="03364-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="03364-180">`exactfit`.</span></span> <span data-ttu-id="03364-181">這樣可讓整個視訊顯示而不維持原始外觀比例，但是扭曲程度可能會發生。</span><span class="sxs-lookup"><span data-stu-id="03364-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="03364-182">如果您未指定`scale`參數時，整個視訊會顯示，不含任何裁剪會維持原始外觀比例。</span><span class="sxs-lookup"><span data-stu-id="03364-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="03364-183">下列範例示範如何使用`scale`參數：</span><span class="sxs-lookup"><span data-stu-id="03364-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="03364-184">Flash player 支援的視訊模式設定，稱為`windowMode`。</span><span class="sxs-lookup"><span data-stu-id="03364-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="03364-185">您可以設定為`window`， `opaque`，和`transparent`。</span><span class="sxs-lookup"><span data-stu-id="03364-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="03364-186">根據預設，`windowMode`設`window`，在網頁上的個別視窗中顯示視訊。</span><span class="sxs-lookup"><span data-stu-id="03364-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="03364-187">`opaque`設定隱藏在網頁上的視訊背後的所有項目。</span><span class="sxs-lookup"><span data-stu-id="03364-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="03364-188">`transparent`設定可讓透過視訊顯示網頁的背景假設任何部分視訊透明的。</span><span class="sxs-lookup"><span data-stu-id="03364-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="03364-189">播放 MediaPlayer (*.wmv*) 影片</span><span class="sxs-lookup"><span data-stu-id="03364-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="03364-190">下列程序會示範如何播放名為視窗 Media video *sample.wmv*位於*媒體*資料夾。</span><span class="sxs-lookup"><span data-stu-id="03364-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="03364-191">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="03364-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="03364-192">建立名為的新頁面*MediaPlayerVideo.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="03364-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="03364-193">將下列標記加入頁面：</span><span class="sxs-lookup"><span data-stu-id="03364-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="03364-194">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="03364-194">Run the page in a browser.</span></span> <span data-ttu-id="03364-195">視訊載入，並會自動播放。</span><span class="sxs-lookup"><span data-stu-id="03364-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="03364-196">![[影像]] (10-working-with-video/_static/image2.jpg "ch08_video 2.jpg")</span><span class="sxs-lookup"><span data-stu-id="03364-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="03364-197">您可以設定`playCount`為整數，指出多少次自動播放視訊：</span><span class="sxs-lookup"><span data-stu-id="03364-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="03364-198">`uiMode`參數可讓您指定的使用者介面中顯示的控制項。</span><span class="sxs-lookup"><span data-stu-id="03364-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="03364-199">您可以設定`uiMode`至`invisible`， `none`， `mini`，或`full`。</span><span class="sxs-lookup"><span data-stu-id="03364-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="03364-200">如果您未指定`uiMode`參數時，視訊會顯示狀態視窗中，搜尋列、 控制按鈕及音量控制項，除了 [視訊] 視窗。</span><span class="sxs-lookup"><span data-stu-id="03364-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="03364-201">如果您使用 media player 播放音訊檔案，也會顯示這些控制項。</span><span class="sxs-lookup"><span data-stu-id="03364-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="03364-202">以下是如何使用的範例`uiMode`參數：</span><span class="sxs-lookup"><span data-stu-id="03364-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="03364-203">根據預設，音訊會播放視訊時。</span><span class="sxs-lookup"><span data-stu-id="03364-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="03364-204">您可以藉由設定靜音音訊`mute`參數為 true:</span><span class="sxs-lookup"><span data-stu-id="03364-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="03364-205">您可以設定來控制 MediaPlayer 視訊的音量大小`volume`參數的值介於 0 到 100 之間。</span><span class="sxs-lookup"><span data-stu-id="03364-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="03364-206">預設值為 50。</span><span class="sxs-lookup"><span data-stu-id="03364-206">The default value is 50.</span></span> <span data-ttu-id="03364-207">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="03364-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="03364-208">播放 Silverlight 視訊</span><span class="sxs-lookup"><span data-stu-id="03364-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="03364-209">此程序會示範如何播放包含在 Silverlight 中的視訊*.xap*頁面的資料夾中名為*媒體*。</span><span class="sxs-lookup"><span data-stu-id="03364-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="03364-210">中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="03364-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="03364-211">建立名為的新頁面*SilverlightVideo.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="03364-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="03364-212">將下列標記加入頁面：</span><span class="sxs-lookup"><span data-stu-id="03364-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="03364-213">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="03364-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="03364-214">![[影像]] (10-working-with-video/_static/image3.jpg "ch08_video 3.jpg")</span><span class="sxs-lookup"><span data-stu-id="03364-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="03364-215">其他資源</span><span class="sxs-lookup"><span data-stu-id="03364-215">Additional Resources</span></span>


<span data-ttu-id="03364-216">[Silverlight 概觀](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="03364-216">[Silverlight Overview](https://msdn.microsoft.com/en-us/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="03364-217">快閃物件和內嵌標記屬性</span><span class="sxs-lookup"><span data-stu-id="03364-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="03364-218">[Windows Media Player 11 SDK PARAM 標記](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="03364-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/en-us/library/aa392321(VS.85).aspx)</span></span>
