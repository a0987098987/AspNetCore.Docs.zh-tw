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
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="8e001-103">使用 ASP.NET Web Pages (Razor) 網站中的影像</span><span class="sxs-lookup"><span data-stu-id="8e001-103">Working with Images in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="8e001-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8e001-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8e001-105">本文將說明如何新增、 顯示及操控映像 （調整大小、 翻轉，和新增浮水印） 中 ASP.NET Web Pages (Razor) 網站。</span><span class="sxs-lookup"><span data-stu-id="8e001-105">This article shows you how to add, display, and manipulate images (resize, flip, and add watermarks) in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="8e001-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="8e001-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8e001-107">如何以動態方式將影像加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="8e001-107">How to add an image to a page dynamically.</span></span>
> - <span data-ttu-id="8e001-108">如何讓使用者上傳的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-108">How to let users upload an image.</span></span>
> - <span data-ttu-id="8e001-109">如何調整影像大小。</span><span class="sxs-lookup"><span data-stu-id="8e001-109">How to resize an image.</span></span>
> - <span data-ttu-id="8e001-110">若要翻轉或旋轉影像的方式。</span><span class="sxs-lookup"><span data-stu-id="8e001-110">How to flip or rotate an image.</span></span>
> - <span data-ttu-id="8e001-111">如何新增浮水印影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-111">How to add a watermark to an image.</span></span>
> - <span data-ttu-id="8e001-112">如何使用做為浮水印的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-112">How to use an image as a watermark.</span></span>
> 
> <span data-ttu-id="8e001-113">這些是 ASP.NET 程式設計文件中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="8e001-113">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="8e001-114">`WebImage`協助程式。</span><span class="sxs-lookup"><span data-stu-id="8e001-114">The `WebImage` helper.</span></span>
> - <span data-ttu-id="8e001-115">`Path`物件，提供可讓您操作路徑和檔案名稱的方法。</span><span class="sxs-lookup"><span data-stu-id="8e001-115">The `Path` object, which provides methods that let you manipulate path and file names.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e001-116">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="8e001-116">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8e001-117">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="8e001-117">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="8e001-118">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="8e001-118">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="8e001-119">本教學課程也適用於 WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="8e001-119">This tutorial also works with WebMatrix 3.</span></span>


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a><span data-ttu-id="8e001-120">以動態方式將影像加入至 Web 網頁</span><span class="sxs-lookup"><span data-stu-id="8e001-120">Adding an Image to a Web Page Dynamically</span></span>

<span data-ttu-id="8e001-121">您可以將影像加入至您的網站和個別頁面雖然您正在開發的網站。</span><span class="sxs-lookup"><span data-stu-id="8e001-121">You can add images to your website and to individual pages while you're developing the website.</span></span> <span data-ttu-id="8e001-122">您也可以讓使用者上傳映像，這可能是適用於工作，例如但是讓他們新增設定檔照片。</span><span class="sxs-lookup"><span data-stu-id="8e001-122">You can also let users upload images, which might be useful for tasks like letting them add a profile photo.</span></span>

<span data-ttu-id="8e001-123">如果影像是已經在網站上，而且您只想要顯示在頁面上，您會使用 HTML`<img>`像這樣的項目：</span><span class="sxs-lookup"><span data-stu-id="8e001-123">If an image is already available on your site and you just want to display it on a page, you use an HTML `<img>` element like this:</span></span>

[!code-html[Main](9-working-with-images/samples/sample1.html)]

<span data-ttu-id="8e001-124">有時候，不過，您需要能夠以動態方式顯示影像 & #8212;也就是說，您不知道何種映像之前的頁面，顯示 正在執行。</span><span class="sxs-lookup"><span data-stu-id="8e001-124">Sometimes, though, you need to be able to display images dynamically &#8212; that is, you don't know what image to display until the page is running.</span></span>

<span data-ttu-id="8e001-125">本節中的程序示範如何讓使用者指定影像的檔案名稱，從映像名稱的清單，立即顯示影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-125">The procedure in this section shows how to display an image on the fly where users specify the image file name from a list of image names.</span></span> <span data-ttu-id="8e001-126">他們從下拉式清單中，選取影像的名稱，當它們送出此頁面，會顯示在選取的映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-126">They select the name of the image from a drop-down list, and when they submit the page, the image they selected is displayed.</span></span>

<span data-ttu-id="8e001-127">![[影像]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-127">![[image]](9-working-with-images/_static/image1.jpg "ch9images-1.jpg")</span></span>

1. <span data-ttu-id="8e001-128">在 WebMatrix 中，建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="8e001-128">In WebMatrix, create a new website.</span></span>
2. <span data-ttu-id="8e001-129">加入新的頁面名稱為*DynamicImage.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-129">Add a new page named *DynamicImage.cshtml*.</span></span>
3. <span data-ttu-id="8e001-130">在網站的根資料夾中，加入新的資料夾並將其命名*映像*。</span><span class="sxs-lookup"><span data-stu-id="8e001-130">In the root folder of the website, add a new folder and name it *images*.</span></span>
4. <span data-ttu-id="8e001-131">新增四個映像以*映像*您剛才建立的資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e001-131">Add four images to the *images* folder you just created.</span></span> <span data-ttu-id="8e001-132">（任何映像具有方便將執行作業，但它們應該符合網頁）。重新命名圖像*Photo1.jpg*， *Photo2.jpg*， *Photo3.jpg*，和*Photo4.jpg*。</span><span class="sxs-lookup"><span data-stu-id="8e001-132">(Any images you have handy will do, but they should fit onto a page.) Rename the images *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, and *Photo4.jpg*.</span></span> <span data-ttu-id="8e001-133">(您不會使用*Photo4.jpg*在這個程序，但您會用到它在本文稍後。)</span><span class="sxs-lookup"><span data-stu-id="8e001-133">(You won't use *Photo4.jpg* in this procedure, but you'll use it later in the article.)</span></span>
5. <span data-ttu-id="8e001-134">請確認，四個映像未標示為唯讀。</span><span class="sxs-lookup"><span data-stu-id="8e001-134">Verify that the four images are not marked as read-only.</span></span>
6. <span data-ttu-id="8e001-135">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-135">Replace the existing content in the page with the following:</span></span>

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    <span data-ttu-id="8e001-136">頁面主體具有下拉式清單 (`<select>`項目)，名稱為`photoChoice`。</span><span class="sxs-lookup"><span data-stu-id="8e001-136">The body of the page has a drop-down list (a `<select>` element) that's named `photoChoice`.</span></span> <span data-ttu-id="8e001-137">清單中有三個選項，而`value`每個清單選項的屬性具有名稱的其中一個您放在映像*映像*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e001-137">The list has three options, and the `value` attribute of each list option has the name of one of the images that you put in the *images* folder.</span></span> <span data-ttu-id="8e001-138">基本上，清單可讓使用者選取的好記的名稱，例如&quot;相片 1&quot;，然後將傳遞 *.jpg*時送出頁面的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="8e001-138">Essentially, the list lets the user select a friendly name like &quot;Photo 1&quot;, and it then passes the *.jpg* file name when the page is submitted.</span></span>

    <span data-ttu-id="8e001-139">在程式碼中，您可以取得使用者的選取範圍 （亦即，映像檔案名稱） 從清單讀取`Request["photoChoice"]`。</span><span class="sxs-lookup"><span data-stu-id="8e001-139">In the code, you can get the user's selection (in other words, the image file name) from the list by reading `Request["photoChoice"]`.</span></span> <span data-ttu-id="8e001-140">您先查看是否有選取項目完全。</span><span class="sxs-lookup"><span data-stu-id="8e001-140">You first see if there's a selection at all.</span></span> <span data-ttu-id="8e001-141">如果沒有，您會建構為映像的資料夾名稱和使用者的映像檔案名稱所組成的映像的路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-141">If there is, you construct a path for the image that consists of the name of the folder for the images and the user's image file name.</span></span> <span data-ttu-id="8e001-142">(如果您嘗試建構的路徑，但沒有任何在`Request["photoChoice"]`，就會收到錯誤。)像這樣的相對路徑的結果：</span><span class="sxs-lookup"><span data-stu-id="8e001-142">(If you tried to construct a path but there was nothing in `Request["photoChoice"]`, you'd get an error.) This results in a relative path like this:</span></span>

    <span data-ttu-id="8e001-143">*映像/Photo1.jpg*</span><span class="sxs-lookup"><span data-stu-id="8e001-143">*images/Photo1.jpg*</span></span>

    <span data-ttu-id="8e001-144">路徑儲存在名為變數`imagePath`，您必須稍後在頁面中。</span><span class="sxs-lookup"><span data-stu-id="8e001-144">The path is stored in variable named `imagePath` that you'll need later in the page.</span></span>

    <span data-ttu-id="8e001-145">在本文中，另外還有`<img>`用來顯示影像，為使用者選取的項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-145">In the body, there's also an `<img>` element that's used to display the image that the user picked.</span></span> <span data-ttu-id="8e001-146">`src`屬性未設定為 檔案名稱或 URL，像是您一樣顯示靜態項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-146">The `src` attribute isn't set to a file name or URL, like you'd do to display a static element.</span></span> <span data-ttu-id="8e001-147">相反地，若要設定`@imagePath`，這表示它從您在程式碼中設定的路徑，取得其值。</span><span class="sxs-lookup"><span data-stu-id="8e001-147">Instead, it's set to `@imagePath`, meaning that it gets its value from the path you set in code.</span></span>

    <span data-ttu-id="8e001-148">第一次執行網頁時，不過，沒有任何影像顯示，因為使用者尚未選取任何項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-148">The first time that the page runs, though, there's no image to display, because the user hasn't selected anything.</span></span> <span data-ttu-id="8e001-149">這通常表示，`src`屬性會為空白，且影像會顯示為紅色&quot;x&quot; （或任何瀏覽器呈現時找不到映像）。</span><span class="sxs-lookup"><span data-stu-id="8e001-149">This would normally mean that the `src` attribute would be empty and the image would show up as a red &quot;x&quot; (or whatever the browser renders when it can't find an image).</span></span> <span data-ttu-id="8e001-150">若要避免這個問題，您將放`<img>`中的項目`if`區塊，請參閱程序會測試是否`imagePath`變數中有任何項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-150">To prevent this, you put the `<img>` element in an `if` block that tests to see whether the `imagePath` variable has anything in it.</span></span> <span data-ttu-id="8e001-151">如果使用者所做的選擇，`imagePath`包含路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-151">If the user made a selection, `imagePath` contains the path.</span></span> <span data-ttu-id="8e001-152">如果使用者沒有挑選一個映像，或如果這是第一次顯示頁面，`<img>`根本不呈現項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-152">If the user didn't pick an image or if this is the first time the page is displayed, the `<img>` element isn't even rendered.</span></span>
7. <span data-ttu-id="8e001-153">儲存檔案，並在瀏覽器中執行網頁。</span><span class="sxs-lookup"><span data-stu-id="8e001-153">Save the file and run the page in a browser.</span></span> <span data-ttu-id="8e001-154">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="8e001-154">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
8. <span data-ttu-id="8e001-155">從下拉式清單中選取映像，然後按一下**範例影像**。</span><span class="sxs-lookup"><span data-stu-id="8e001-155">Select an image from the drop-down list and then click **Sample Image**.</span></span> <span data-ttu-id="8e001-156">請確定您會看到不同的選擇不同的映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-156">Make sure that you see different images for different choices.</span></span>

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a><span data-ttu-id="8e001-157">上傳映像</span><span class="sxs-lookup"><span data-stu-id="8e001-157">Uploading an Image</span></span>

<span data-ttu-id="8e001-158">前一個範例會示範如何以動態方式，顯示影像，但它只能使用已經在網站上的映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-158">The previous example showed you how to display an image dynamically, but it worked only with images that were already on your website.</span></span> <span data-ttu-id="8e001-159">此程序示範如何讓使用者上傳映像，即會顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="8e001-159">This procedure shows how to let users upload an image, which is then displayed on the page.</span></span> <span data-ttu-id="8e001-160">在 ASP.NET 中，您可以上操控映像使用即時`WebImage`helper，有方法可讓您建立、 操作及儲存映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-160">In ASP.NET, you can manipulate images on the fly using the `WebImage` helper, which has methods that let you create, manipulate, and save images.</span></span> <span data-ttu-id="8e001-161">`WebImage`協助程式支援所有常見 web 影像檔案類型，包括 *.jpg*， *.png*，和 *.bmp*。</span><span class="sxs-lookup"><span data-stu-id="8e001-161">The `WebImage` helper supports all the common web image file types, including *.jpg*, *.png*, and *.bmp*.</span></span> <span data-ttu-id="8e001-162">在本文中，您將使用 *.jpg*映像，但是您可以使用任何影像類型。</span><span class="sxs-lookup"><span data-stu-id="8e001-162">Throughout this article, you'll use *.jpg* images, but you can use any of the image types.</span></span>

<span data-ttu-id="8e001-163">![[影像]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-163">![[image]](9-working-with-images/_static/image2.jpg "ch9images-2.jpg")</span></span>

1. <span data-ttu-id="8e001-164">加入新的頁面並將其命名*UploadImage.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-164">Add a new page and name it *UploadImage.cshtml*.</span></span>
2. <span data-ttu-id="8e001-165">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-165">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    <span data-ttu-id="8e001-166">文字本文有`<input type="file">`元素，其可讓使用者選取要上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e001-166">The body of the text has an `<input type="file">` element, which lets users select a file to upload.</span></span> <span data-ttu-id="8e001-167">當使用者按**送出**，以及表單送出它們挑選的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e001-167">When they click **Submit**, the file they picked is submitted along with the form.</span></span>

    <span data-ttu-id="8e001-168">若要取得已上傳的映像，您使用`WebImage`helper，有各式各樣的有用的方法以處理映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-168">To get the uploaded image, you use the `WebImage` helper, which has all sorts of useful methods for working with images.</span></span> <span data-ttu-id="8e001-169">具體而言，使用`WebImage.GetImageFromRequest`取得上傳的影像 （如果有的話），並將其儲存在名為`photo`。</span><span class="sxs-lookup"><span data-stu-id="8e001-169">Specifically, you use `WebImage.GetImageFromRequest` to get the uploaded image (if any) and store it in a variable named `photo`.</span></span>

    <span data-ttu-id="8e001-170">取得和設定檔案及路徑名稱，牽涉到許多在此範例中工作。</span><span class="sxs-lookup"><span data-stu-id="8e001-170">A lot of the work in this example involves getting and setting file and path names.</span></span> <span data-ttu-id="8e001-171">問題在於您想要取得使用者上傳，映像的名稱 （和只有名稱），然後再建立用於您要儲存映像的新路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-171">The issue is that you want to get the name (and just the name) of the image that the user uploaded, and then create a new path for where you're going to store the image.</span></span> <span data-ttu-id="8e001-172">因為使用者可能無法上傳多個具有相同名稱的映像，您可以使用額外的程式碼進行一些建立唯一的名稱，以確保使用者不覆寫現有的圖片。</span><span class="sxs-lookup"><span data-stu-id="8e001-172">Because users could potentially upload multiple images that have the same name, you use a bit of extra code to create unique names and make sure that users don't overwrite existing pictures.</span></span>

    <span data-ttu-id="8e001-173">如果影像實際上已上傳 (測試`if (photo != null)`)，就從映像的映像名稱`FileName`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e001-173">If an image actually has been uploaded (the test `if (photo != null)`), you get the image name from the image's `FileName` property.</span></span> <span data-ttu-id="8e001-174">當使用者上傳映像，`FileName`包含使用者的原始名稱，其中包含從使用者的電腦的路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-174">When the user uploads the image, `FileName` contains the user's original name, which includes the path from the user's computer.</span></span> <span data-ttu-id="8e001-175">它看起來可能像這樣：</span><span class="sxs-lookup"><span data-stu-id="8e001-175">It might look like this:</span></span>

    <span data-ttu-id="8e001-176">*C:\Users\Joe\Pictures\SamplePhoto1.jpg*</span><span class="sxs-lookup"><span data-stu-id="8e001-176">*C:\Users\Joe\Pictures\SamplePhoto1.jpg*</span></span>

    <span data-ttu-id="8e001-177">您不想該路徑資訊，不過 & #8212;您只想實際的檔案名稱 (*SamplePhoto1.jpg*)。</span><span class="sxs-lookup"><span data-stu-id="8e001-177">You don't want all that path information, though &#8212; you just want the actual file name (*SamplePhoto1.jpg*).</span></span> <span data-ttu-id="8e001-178">您可以使用帶狀出該檔案的路徑`Path.GetFileName`方法，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="8e001-178">You can strip out just the file from a path by using the `Path.GetFileName` method, like this:</span></span>

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    <span data-ttu-id="8e001-179">然後，您會建立新的唯一檔案名稱加入 GUID 的原始名稱。</span><span class="sxs-lookup"><span data-stu-id="8e001-179">You then create a new unique file name by adding a GUID to the original name.</span></span> <span data-ttu-id="8e001-180">(如需詳細資訊的 Guid，請參閱[有關 Guid](#SB_AboutGUIDs)本文稍後。)然後您建構可讓您儲存的映像的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-180">(For more about GUIDs, see [About GUIDs](#SB_AboutGUIDs) later in this article.) Then you construct a complete path that you can use to save the image.</span></span> <span data-ttu-id="8e001-181">儲存在由新的檔案名稱的資料夾 （影像） 及目前的網站位置的路徑所組成。</span><span class="sxs-lookup"><span data-stu-id="8e001-181">The save path is made up of the new file name, the folder (images), and the current website location.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8e001-182">為了讓您的程式碼儲存在檔案*映像*資料夾中，應用程式需要讀取-寫入該資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="8e001-182">In order for your code to save files in the *images* folder, the application needs read-write permissions for that folder.</span></span> <span data-ttu-id="8e001-183">在開發電腦上這不通常是發生問題。</span><span class="sxs-lookup"><span data-stu-id="8e001-183">On your development computer this is not typically an issue.</span></span> <span data-ttu-id="8e001-184">不過，當您將您的網站發行至裝載提供者的 web 伺服器時，您可能需要明確設定這些權限。</span><span class="sxs-lookup"><span data-stu-id="8e001-184">However, when you publish your site to a hosting provider's web server, you might need to explicitly set those permissions.</span></span> <span data-ttu-id="8e001-185">如果您在裝載提供者的伺服器上執行此程式碼，並發生錯誤，請洽詢主機服務提供者了解如何設定這些權限。</span><span class="sxs-lookup"><span data-stu-id="8e001-185">If you run this code on a hosting provider's server and get errors, check with the hosting provider to find out how to set those permissions.</span></span>

    <span data-ttu-id="8e001-186">最後，您將傳遞儲存路徑`Save`方法`WebImage`協助程式。</span><span class="sxs-lookup"><span data-stu-id="8e001-186">Finally, you pass the save path to the `Save` method of the `WebImage` helper.</span></span> <span data-ttu-id="8e001-187">這會儲存已上傳的映像，新名稱。</span><span class="sxs-lookup"><span data-stu-id="8e001-187">This stores the uploaded image under its new name.</span></span> <span data-ttu-id="8e001-188">儲存方法看起來像這樣： `photo.Save(@"~\" + imagePath)`。</span><span class="sxs-lookup"><span data-stu-id="8e001-188">The save method looks like this: `photo.Save(@"~\" + imagePath)`.</span></span> <span data-ttu-id="8e001-189">完整路徑會附加至`@"~\"`，這是目前的網站位置。</span><span class="sxs-lookup"><span data-stu-id="8e001-189">The complete path is appended to `@"~\"`, which is the current website location.</span></span> <span data-ttu-id="8e001-190">(如需有關資訊`~`運算子，請參閱[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)。)</span><span class="sxs-lookup"><span data-stu-id="8e001-190">(For information about the `~` operator, see [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)</span></span>

    <span data-ttu-id="8e001-191">如同先前的範例，頁面主體包含`<img>`来顯示的影像項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-191">As in the previous example, the body of the page contains an `<img>` element to display the image.</span></span> <span data-ttu-id="8e001-192">如果`imagePath`已設定，`<img>`項目會呈現及其`src`屬性設為`imagePath`值。</span><span class="sxs-lookup"><span data-stu-id="8e001-192">If `imagePath` has been set, the `<img>` element is rendered and its `src` attribute is set to the `imagePath` value.</span></span>
3. <span data-ttu-id="8e001-193">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="8e001-193">Run the page in a browser.</span></span>
4. <span data-ttu-id="8e001-194">上傳映像，並確定它會顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="8e001-194">Upload an image and make sure it's displayed in the page.</span></span>
5. <span data-ttu-id="8e001-195">在您的網站，開啟*映像*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e001-195">In your site, open the *images* folder.</span></span> <span data-ttu-id="8e001-196">您會看到，新的檔案已加入的檔案名稱看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="8e001-196">You see that a new file has been added whose file name looks something like this::</span></span> 

    <span data-ttu-id="8e001-197">*45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*</span><span class="sxs-lookup"><span data-stu-id="8e001-197">*45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*</span></span>

    <span data-ttu-id="8e001-198">這是您上傳具有 GUID 的名稱前置詞的映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-198">This is the image that you uploaded with a GUID prefixed to the name.</span></span> <span data-ttu-id="8e001-199">(您自己的檔案會有不同的 GUID，並可能名為以外*MyPhoto.png*。)</span><span class="sxs-lookup"><span data-stu-id="8e001-199">(Your own file will have a different GUID and probably is named something different than *MyPhoto.png*.)</span></span>

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a><span data-ttu-id="8e001-200">有關 Guid</span><span class="sxs-lookup"><span data-stu-id="8e001-200">About GUIDs</span></span>
> 
> <span data-ttu-id="8e001-201">GUID （全域唯一識別碼） 是識別碼，通常會轉譯格式如下： `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`。</span><span class="sxs-lookup"><span data-stu-id="8e001-201">A GUID (globally-unique ID) is an identifier that's usually rendered in a format like this: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`.</span></span> <span data-ttu-id="8e001-202">針對每個 GUID，不同的數字和字母 （A-F)，但是它們都依照使用群組的 8-4-4-4-12 個字元的模式。</span><span class="sxs-lookup"><span data-stu-id="8e001-202">The numbers and letters (from A-F) differ for each GUID, but they all follow the pattern of using groups of 8-4-4-4-12 characters.</span></span> <span data-ttu-id="8e001-203">（技術上來說，GUID 是 16 位元組/128 位元數目）。當您需要 GUID 時，您可以呼叫會產生一個 GUID，為您的特定程式碼。</span><span class="sxs-lookup"><span data-stu-id="8e001-203">(Technically, a GUID is a 16-byte/128-bit number.) When you need a GUID, you can call specialized code that generates a GUID for you.</span></span> <span data-ttu-id="8e001-204">Guid 背後的概念在於之間的數字龐大大小 (3.4 x 10<sup>38</sup>)，而且產生的演算法，產生的數字幾乎保證是其中一種。</span><span class="sxs-lookup"><span data-stu-id="8e001-204">The idea behind GUIDs is that between the enormous size of the number (3.4 x 10<sup>38</sup>) and the algorithm for generating it, the resulting number is virtually guaranteed to be one of a kind.</span></span> <span data-ttu-id="8e001-205">Guid，因此是產生的項目名稱，當您必須保證您將不會使用相同的名稱是兩次的好方法。</span><span class="sxs-lookup"><span data-stu-id="8e001-205">GUIDs therefore are a good way to generate names for things when you must guarantee that you won't use the same name twice.</span></span> <span data-ttu-id="8e001-206">這個選項的缺點當然是 Guid 是的特別使用者易記的因此它們通常用於只能在程式碼中使用名稱。</span><span class="sxs-lookup"><span data-stu-id="8e001-206">The downside, of course, is that GUIDs aren't particularly user friendly, so they tend to be used when the name is used only in code.</span></span>


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a><span data-ttu-id="8e001-207">調整影像大小</span><span class="sxs-lookup"><span data-stu-id="8e001-207">Resizing an Image</span></span>

<span data-ttu-id="8e001-208">如果您的網站會接受來自使用者的映像，您可能要調整影像的大小，然後才顯示，或將它們儲存。</span><span class="sxs-lookup"><span data-stu-id="8e001-208">If your website accepts images from a user, you might want to resize the images before you display or save them.</span></span> <span data-ttu-id="8e001-209">您可以再次使用`WebImage`這個協助程式。</span><span class="sxs-lookup"><span data-stu-id="8e001-209">You can again use the `WebImage` helper for this.</span></span>

<span data-ttu-id="8e001-210">此程序顯示如何調整大小建立縮圖，然後將縮圖和原始的映像儲存在網站上傳映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-210">This procedure shows how to resize an uploaded image to create a thumbnail and then save the thumbnail and original image in the website.</span></span> <span data-ttu-id="8e001-211">您的頁面上顯示縮圖，並將使用者重新導向至全尺寸的映像使用超連結。</span><span class="sxs-lookup"><span data-stu-id="8e001-211">You display the thumbnail on the page and use a hyperlink to redirect users to the full-sized image.</span></span>

<span data-ttu-id="8e001-212">![[影像]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-212">![[image]](9-working-with-images/_static/image3.jpg "ch9images-3.jpg")</span></span>

1. <span data-ttu-id="8e001-213">加入新的頁面名稱為*Thumbnail.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-213">Add a new page named *Thumbnail.cshtml*.</span></span>
2. <span data-ttu-id="8e001-214">在*映像*資料夾中，建立名稱為的子*使*。</span><span class="sxs-lookup"><span data-stu-id="8e001-214">In the *images* folder, create a subfolder named *thumbs*.</span></span>
3. <span data-ttu-id="8e001-215">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-215">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    <span data-ttu-id="8e001-216">此程式碼是從先前的範例類似的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8e001-216">This code is similar to the code from the previous example.</span></span> <span data-ttu-id="8e001-217">兩者的差異在於，此程式碼會將影像儲存兩次，一次通常一次之後您建立縮圖影像的複本。</span><span class="sxs-lookup"><span data-stu-id="8e001-217">The difference is that this code saves the image twice, once normally and once after you create a thumbnail copy of the image.</span></span> <span data-ttu-id="8e001-218">先取得已上傳映像，並儲存在*映像*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e001-218">First you get the uploaded image and save it in the *images* folder.</span></span> <span data-ttu-id="8e001-219">然後，您可以建構在縮圖影像的新路徑。</span><span class="sxs-lookup"><span data-stu-id="8e001-219">You then construct a new path for the thumbnail image.</span></span> <span data-ttu-id="8e001-220">若要實際建立縮圖，請呼叫`WebImage`協助專家的`Resize`方法來建立 60 像素 x 60 像素的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-220">To actually create the thumbnail, you call the `WebImage` helper's `Resize` method to create a 60-pixel by 60-pixel image.</span></span> <span data-ttu-id="8e001-221">這個範例顯示如何您保留外觀比例如何防止影像 （如果新的大小會實際設定更大的映像） 放大。</span><span class="sxs-lookup"><span data-stu-id="8e001-221">The example shows how you preserve the aspect ratio and how you can prevent the image from being enlarged (in case the new size would actually make the image larger).</span></span> <span data-ttu-id="8e001-222">調整大小的影像則儲存在*使*子資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e001-222">The resized image is then saved in the *thumbs* subfolder.</span></span>

    <span data-ttu-id="8e001-223">在結束標記時，您使用的相同`<img>`具有動態項目`src`您所見前述範例中，有條件地顯示影像的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e001-223">At the end of the markup, you use the same `<img>` element with the dynamic `src` attribute that you've seen in the previous examples to conditionally show the image.</span></span> <span data-ttu-id="8e001-224">在此情況下，您可以顯示縮圖。</span><span class="sxs-lookup"><span data-stu-id="8e001-224">In this case, you display the thumbnail.</span></span> <span data-ttu-id="8e001-225">您也使用`<a>`建立大版本的映像的超連結的項目。</span><span class="sxs-lookup"><span data-stu-id="8e001-225">You also use an `<a>` element to create a hyperlink to the big version of the image.</span></span> <span data-ttu-id="8e001-226">如同`src`屬性`<img>`設定項目，`href`屬性`<a>`項目，以動態方式來指定任何處於`imagePath`。</span><span class="sxs-lookup"><span data-stu-id="8e001-226">As with the `src` attribute of the `<img>` element, you set the `href` attribute of the `<a>` element dynamically to whatever is in `imagePath`.</span></span> <span data-ttu-id="8e001-227">若要確定路徑可做為 URL，您將傳遞`imagePath`至`Html.AttributeEncode`方法，然後將路徑中的保留的字元轉換為 [確定]，在 URL 中的字元。</span><span class="sxs-lookup"><span data-stu-id="8e001-227">To make sure that the path can work as a URL, you pass `imagePath` to the `Html.AttributeEncode` method, which converts reserved characters in the path to characters that are ok in a URL.</span></span>
4. <span data-ttu-id="8e001-228">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="8e001-228">Run the page in a browser.</span></span>
5. <span data-ttu-id="8e001-229">上傳相片，並確認已顯示縮圖。</span><span class="sxs-lookup"><span data-stu-id="8e001-229">Upload a photo and verify that the thumbnail is shown.</span></span>
6. <span data-ttu-id="8e001-230">按一下以查看完整的影像縮圖。</span><span class="sxs-lookup"><span data-stu-id="8e001-230">Click the thumbnail to see the full-size image.</span></span>
7. <span data-ttu-id="8e001-231">在*映像*和*映像/使*，請注意，已加入新的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e001-231">In the *images* and *images/thumbs*, note that new files have been added.</span></span>

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a><span data-ttu-id="8e001-232">輪替與翻轉影像</span><span class="sxs-lookup"><span data-stu-id="8e001-232">Rotating and Flipping an Image</span></span>

<span data-ttu-id="8e001-233">`WebImage` Helper 也可讓您翻轉和旋轉的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-233">The `WebImage` helper also lets you flip and rotate images.</span></span> <span data-ttu-id="8e001-234">此程序示範如何從伺服器取得映像、 翻轉影像面朝下 （垂直）、 加以儲存，並接著顯示在頁面上的 翻轉的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-234">This procedure shows how to get an image from the server, flip the image upside down (vertically), save it, and then display the flipped image on the page.</span></span> <span data-ttu-id="8e001-235">在此範例中，您只會使用您已經在伺服器的檔案 (*Photo2.jpg*)。</span><span class="sxs-lookup"><span data-stu-id="8e001-235">In this example, you're just using a file you already have on the server (*Photo2.jpg*).</span></span> <span data-ttu-id="8e001-236">在實際應用中，您可能會翻轉影像如同上述範例中，以動態方式取得其名稱。</span><span class="sxs-lookup"><span data-stu-id="8e001-236">In a real application, you'd probably flip an image whose name you get dynamically, like you did in previous examples.</span></span>

<span data-ttu-id="8e001-237">![[影像]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-237">![[image]](9-working-with-images/_static/image4.jpg "ch9images-4.jpg")</span></span>

1. <span data-ttu-id="8e001-238">加入新的頁面名稱為*FlipImage.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-238">Add a new page named *FlipImage.cshtml*.</span></span>
2. <span data-ttu-id="8e001-239">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-239">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    <span data-ttu-id="8e001-240">程式碼會使用`WebImage`從伺服器取得映像的協助程式。</span><span class="sxs-lookup"><span data-stu-id="8e001-240">The code uses the `WebImage` helper to get an image from the server.</span></span> <span data-ttu-id="8e001-241">您建立使用相同的技巧，先前範例中使用儲存映像的映像的路徑並建立映像使用時，會傳遞該路徑`WebImage`:</span><span class="sxs-lookup"><span data-stu-id="8e001-241">You create the path to the image using the same technique you used in earlier examples for saving images, and you pass that path when you create an image using `WebImage`:</span></span>

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    <span data-ttu-id="8e001-242">如果找到映像，您可以建構新路徑和檔案名稱，如同先前範例中。</span><span class="sxs-lookup"><span data-stu-id="8e001-242">If an image is found, you construct a new path and file name, like you did in earlier examples.</span></span> <span data-ttu-id="8e001-243">若要翻轉影像，請呼叫`FlipVertical`方法，然後將映像儲存一次。</span><span class="sxs-lookup"><span data-stu-id="8e001-243">To flip the image, you call the `FlipVertical` method, and then you save the image again.</span></span>

    <span data-ttu-id="8e001-244">映像再次顯示在頁面上使用`<img>`具有項目`src`屬性設為`imagePath`。</span><span class="sxs-lookup"><span data-stu-id="8e001-244">The image is again displayed on the page by using the `<img>` element with the `src` attribute set to `imagePath`.</span></span>
3. <span data-ttu-id="8e001-245">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="8e001-245">Run the page in a browser.</span></span> <span data-ttu-id="8e001-246">映像*Photo2.jpg*面朝下顯示。</span><span class="sxs-lookup"><span data-stu-id="8e001-246">The image for *Photo2.jpg* is shown upside down.</span></span>
4. <span data-ttu-id="8e001-247">重新整理頁面，或要求頁面，再次以查看映像已翻轉的右側，即啟動一次。</span><span class="sxs-lookup"><span data-stu-id="8e001-247">Refresh the page or request the page again to see the image is flipped right side up again.</span></span>

<span data-ttu-id="8e001-248">若要旋轉影像，您可以使用相同的程式碼，不過，而不是呼叫`FlipVertical`或`FlipHorizontal`，您呼叫`RotateLeft`或`RotateRight`。</span><span class="sxs-lookup"><span data-stu-id="8e001-248">To rotate an image, you use the same code, except that instead of calling the `FlipVertical` or `FlipHorizontal`, you call `RotateLeft` or `RotateRight`.</span></span>

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a><span data-ttu-id="8e001-249">新增浮水印影像</span><span class="sxs-lookup"><span data-stu-id="8e001-249">Adding a Watermark to an Image</span></span>

<span data-ttu-id="8e001-250">當您將影像加入至您的網站時，您可能要先儲存它，或將它顯示在頁面上，新增浮水印影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-250">When you add images to your website, you might want to add a watermark to the image before you save it or display it on a page.</span></span> <span data-ttu-id="8e001-251">若要將著作權資訊加入至映像或通告其商務名稱人們經常會使用浮水印。</span><span class="sxs-lookup"><span data-stu-id="8e001-251">People often use watermarks to add copyright information to an image or to advertise their business name.</span></span>

<span data-ttu-id="8e001-252">![[影像]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-252">![[image]](9-working-with-images/_static/image5.jpg "ch9images-5.jpg")</span></span>

1. <span data-ttu-id="8e001-253">加入新的頁面名稱為*Watermark.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-253">Add a new page named *Watermark.cshtml*.</span></span>
2. <span data-ttu-id="8e001-254">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-254">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    <span data-ttu-id="8e001-255">此程式碼就像中的程式碼*FlipImage.cshtml*從先前的頁面 (雖然這次使用*Photo3.jpg*檔案)。</span><span class="sxs-lookup"><span data-stu-id="8e001-255">This code is like the code in the *FlipImage.cshtml* page from earlier (although this time it uses the *Photo3.jpg* file).</span></span> <span data-ttu-id="8e001-256">若要新增浮水印，請呼叫`WebImage`協助專家的`AddTextWatermark`方法之前儲存的映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-256">To add the watermark, you call the `WebImage` helper's `AddTextWatermark` method before you save the image.</span></span> <span data-ttu-id="8e001-257">在呼叫`AddTextWatermark`，傳送文字&quot;我浮水印&quot;、 設定字型色彩為黃色，並設定新細明體字型家族。</span><span class="sxs-lookup"><span data-stu-id="8e001-257">In the call to `AddTextWatermark`, you pass the text &quot;My Watermark&quot;, set the font color to yellow, and set the font family to Arial.</span></span> <span data-ttu-id="8e001-258">(雖然它未顯示在這裡， `WebImage` helper 也可讓您指定的不透明度、 字型系列和字型大小和浮水印文字的位置。)當您儲存映像時它不得為唯讀。</span><span class="sxs-lookup"><span data-stu-id="8e001-258">(Although it's not shown here, the `WebImage` helper also lets you specify opacity, font family and font size, and the position of the watermark text.) When you save the image it must not be read-only.</span></span>

    <span data-ttu-id="8e001-259">映像如您所見之前，會顯示在頁面上使用`<img>`元素具有的 src 屬性設定為`@imagePath`。</span><span class="sxs-lookup"><span data-stu-id="8e001-259">As you've seen before, the image is displayed on the page by using the `<img>` element with the src attribute set to `@imagePath`.</span></span>
3. <span data-ttu-id="8e001-260">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="8e001-260">Run the page in a browser.</span></span> <span data-ttu-id="8e001-261">右下角的 映像，請注意 「 我的配額上限 」 的文字。</span><span class="sxs-lookup"><span data-stu-id="8e001-261">Notice the text "My Watermark" at the bottom-right corner of the image.</span></span>

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a><span data-ttu-id="8e001-262">使用做為浮水印的影像</span><span class="sxs-lookup"><span data-stu-id="8e001-262">Using an Image As a Watermark</span></span>

<span data-ttu-id="8e001-263">而不是使用浮水印文字，您可以使用另一個映像。</span><span class="sxs-lookup"><span data-stu-id="8e001-263">Instead of using text for a watermark, you can use another image.</span></span> <span data-ttu-id="8e001-264">人員有時使用像公司標誌的影像做為浮水印，或者使用浮水印影像而不是文字的著作權資訊。</span><span class="sxs-lookup"><span data-stu-id="8e001-264">People sometimes use images like a company logo as a watermark, or they use a watermark image instead of text for copyright information.</span></span>

<span data-ttu-id="8e001-265">![[影像]](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")</span><span class="sxs-lookup"><span data-stu-id="8e001-265">![[image]](9-working-with-images/_static/image6.jpg "ch9images-6.jpg")</span></span>

1. <span data-ttu-id="8e001-266">加入新的頁面名稱為*ImageWatermark.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="8e001-266">Add a new page named *ImageWatermark.cshtml*.</span></span>
2. <span data-ttu-id="8e001-267">將映像加入*映像*資料夾，您可以做為標誌，並將影像重新命名*MyCompanyLogo.jpg*。</span><span class="sxs-lookup"><span data-stu-id="8e001-267">Add an image to the *images* folder that you can use as a logo, and rename the image *MyCompanyLogo.jpg*.</span></span> <span data-ttu-id="8e001-268">此映像應該是您可以清楚地查看當其設為 80 個像素寬和高 20 像素的影像。</span><span class="sxs-lookup"><span data-stu-id="8e001-268">This image should be an image that you can see clearly when it's set to 80 pixels wide and 20 pixels high.</span></span>
3. <span data-ttu-id="8e001-269">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="8e001-269">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    <span data-ttu-id="8e001-270">這是從先前的範例程式碼的另一種變異。</span><span class="sxs-lookup"><span data-stu-id="8e001-270">This is another variation on the code from earlier examples.</span></span> <span data-ttu-id="8e001-271">在此情況下，您呼叫`AddImageWatermark`浮水印影像新增至目標映像 (*Photo3.jpg*) 儲存映像之前。</span><span class="sxs-lookup"><span data-stu-id="8e001-271">In this case, you call `AddImageWatermark` to add the watermark image to the target image (*Photo3.jpg*) before you save the image.</span></span> <span data-ttu-id="8e001-272">當您呼叫`AddImageWatermark`，您將其寬度設定為 80 的像素並到 20 像素的高度。</span><span class="sxs-lookup"><span data-stu-id="8e001-272">When you call `AddImageWatermark`, you set its width to 80 pixels and the height to 20 pixels.</span></span> <span data-ttu-id="8e001-273">*MyCompanyLogo.jpg*映像會水平置中對齊和目標映像底部垂直對齊。</span><span class="sxs-lookup"><span data-stu-id="8e001-273">The *MyCompanyLogo.jpg* image is horizontally aligned in the center and vertically aligned at the bottom of the target image.</span></span> <span data-ttu-id="8e001-274">不透明度設定為 100%，填補設定為 10 個像素為單位。</span><span class="sxs-lookup"><span data-stu-id="8e001-274">The opacity is set to 100% and the padding is set to 10 pixels.</span></span> <span data-ttu-id="8e001-275">浮水印影像大於目標映像時，會發生任何事。</span><span class="sxs-lookup"><span data-stu-id="8e001-275">If the watermark image is bigger than the target image, nothing will happen.</span></span> <span data-ttu-id="8e001-276">如果浮水印影像大於目標映像，並且設定為零的映像浮水印的填補，會忽略浮水印。</span><span class="sxs-lookup"><span data-stu-id="8e001-276">If the watermark image is bigger than the target image and you set the padding for the image watermark to zero, the watermark is ignored.</span></span>

    <span data-ttu-id="8e001-277">因為之前，您可以顯示映像使用`<img>`項目和動態`src`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e001-277">As before, you display the image using the `<img>` element and a dynamic `src` attribute.</span></span>
4. <span data-ttu-id="8e001-278">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="8e001-278">Run the page in a browser.</span></span> <span data-ttu-id="8e001-279">浮水印影像會出現在主要映像底部的通知。</span><span class="sxs-lookup"><span data-stu-id="8e001-279">Notice that the watermark image appears at the bottom of the main image.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="8e001-280">其他資源</span><span class="sxs-lookup"><span data-stu-id="8e001-280">Additional Resources</span></span>


[<span data-ttu-id="8e001-281">使用中的 ASP.NET Web Pages 站台中的檔案</span><span class="sxs-lookup"><span data-stu-id="8e001-281">Working with Files in an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkId=202896)

[<span data-ttu-id="8e001-282">介紹程式設計使用 Razor 語法的 ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="8e001-282">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
