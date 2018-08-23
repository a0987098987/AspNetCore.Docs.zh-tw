---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: ASP.NET Web Pages (Razor) 網站的自訂全網站行為 |Microsoft Docs
author: tfitzmac
description: 本章說明如何對您的整個網站或整個資料夾，而不是單一頁面的設定。
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: a6737c05d3326a2cd7535f32e99482b0de394575
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826232"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="9c087-103">ASP.NET Web Pages (Razor) 網站的自訂全網站行為</span><span class="sxs-lookup"><span data-stu-id="9c087-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="9c087-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9c087-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="9c087-105">這篇文章說明如何在 ASP.NET Web Pages (Razor) 網站建立站台後端設定頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="9c087-106">您將學到什麼：</span><span class="sxs-lookup"><span data-stu-id="9c087-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="9c087-107">如何執行程式碼，可讓您設定的值 （全域值或協助程式設定） 中站台的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="9c087-108">如何執行程式碼，讓您設定的資料夾中的所有頁面的值。</span><span class="sxs-lookup"><span data-stu-id="9c087-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="9c087-109">如何執行程式碼之前和之後的頁面載入。</span><span class="sxs-lookup"><span data-stu-id="9c087-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="9c087-110">如何將錯誤傳送至中央的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="9c087-111">如何將驗證新增至資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9c087-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="9c087-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9c087-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="9c087-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="9c087-114">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="9c087-114">WebMatrix 3</span></span>
> - <span data-ttu-id="9c087-115">ASP.NET Web Helpers Library （NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="9c087-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="9c087-116">本教學課程也適用於使用 ASP.NET Web Pages 3 和 Visual Studio 2013 （或 Visual Studio Express 2013 for Web），但您無法使用 ASP.NET Web Helpers Library。</span><span class="sxs-lookup"><span data-stu-id="9c087-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="9c087-117">新增 ASP.NET Web Pages 網站啟動程式碼</span><span class="sxs-lookup"><span data-stu-id="9c087-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="9c087-118">對於大部分您撰寫 ASP.NET Web Pages 中的程式碼，個別的頁面可以包含具有該頁面所需的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c087-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="9c087-119">例如，如果頁面傳送電子郵件訊息時，就能夠將該作業的所有程式都碼放在單一頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="9c087-120">這可能包括初始化設定傳送電子郵件的程式碼 (也就是 SMTP 伺服器) 和傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="9c087-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="9c087-121">不過，在某些情況下，您可能想要在網站上的任何頁面執行前執行的一些程式碼。</span><span class="sxs-lookup"><span data-stu-id="9c087-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="9c087-122">這可用於設定可在網站中任何地方使用的值 (稱為*全域值*。)例如，一些協助程式會要求您提供的值，例如電子郵件設定或帳戶金鑰。</span><span class="sxs-lookup"><span data-stu-id="9c087-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="9c087-123">它可以方便地將這些設定保留在全域值。</span><span class="sxs-lookup"><span data-stu-id="9c087-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="9c087-124">您可以藉由建立名為的頁面 *\_AppStart.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="9c087-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="9c087-125">如果此頁面存在，它會執行第一次要求在站台的任何頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="9c087-126">因此，它是執行程式碼來設定全域值的好地方。</span><span class="sxs-lookup"><span data-stu-id="9c087-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="9c087-127">(因為 *\_AppStart.cshtml*具有底線的前置詞，ASP.NET 不會傳送頁面給瀏覽器即使使用者直接要求。)</span><span class="sxs-lookup"><span data-stu-id="9c087-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="9c087-128">下圖顯示如何 *\_AppStart.cshtml*頁面上運作。</span><span class="sxs-lookup"><span data-stu-id="9c087-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="9c087-129">當要求傳入頁面中，和如果這是第一個要求任何頁面在網站中，ASP.NET 會先檢查是否 *\_AppStart.cshtml*頁面存在。</span><span class="sxs-lookup"><span data-stu-id="9c087-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="9c087-130">如果是的話，任何程式碼中 *\_AppStart.cshtml*頁面上執行，並執行要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[影像]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="9c087-132">為您的網站設定的全域值</span><span class="sxs-lookup"><span data-stu-id="9c087-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="9c087-133">在 WebMatrix 網站根資料夾中，建立名為 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="9c087-134">檔案必須是網站的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="9c087-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="9c087-135">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="9c087-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="9c087-136">此程式碼將值儲存在`AppState`字典，會自動提供給網站中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="9c087-137">請注意，  *\_AppStart.cshtml*檔案中沒有任何標記。</span><span class="sxs-lookup"><span data-stu-id="9c087-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="9c087-138">頁面會執行程式碼，並再重新導向至原先要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9c087-139">將程式碼放在時要特別小心 *\_AppStart.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="9c087-140">如果在程式碼中發生任何錯誤 *\_AppStart.cshtml*檔案，將無法啟動網站。</span><span class="sxs-lookup"><span data-stu-id="9c087-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="9c087-141">在根資料夾中，建立名為的新網頁*AppName.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="9c087-142">以下列內容取代預設標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="9c087-143">此程式碼會擷取的值`AppState`中設定的物件 *\_AppStart.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="9c087-144">執行*AppName.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="9c087-145">(請確定中選取頁面**檔案**才能執行這個工作區。)此頁面會顯示全域值。</span><span class="sxs-lookup"><span data-stu-id="9c087-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[影像]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="9c087-147">協助程式的設定值</span><span class="sxs-lookup"><span data-stu-id="9c087-147">Setting Values for Helpers</span></span>

<span data-ttu-id="9c087-148">為充分利用 *\_AppStart.cshtml*檔案之設定的協助程式，您的環境中使用，且具有初始化值。</span><span class="sxs-lookup"><span data-stu-id="9c087-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="9c087-149">典型的範例包括電子郵件設定`WebMail`協助程式 」 和 「 私用和公用金鑰`ReCaptcha`協助程式。</span><span class="sxs-lookup"><span data-stu-id="9c087-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="9c087-150">在這類的情況下，您可以設定中的一次的值 *\_AppStart.cshtml* ，然後已設定的所有頁面在您的網站。</span><span class="sxs-lookup"><span data-stu-id="9c087-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="9c087-151">此程序會示範如何設定`WebMail`設定全域。</span><span class="sxs-lookup"><span data-stu-id="9c087-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="9c087-152">(如需使用詳細資訊`WebMail`協助程式，請參閱 <<c2> [ 新增至 ASP.NET Web Pages 網站的電子郵件](../getting-started/11-adding-email-to-your-web-site.md)。)</span><span class="sxs-lookup"><span data-stu-id="9c087-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="9c087-153">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有新增它。</span><span class="sxs-lookup"><span data-stu-id="9c087-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="9c087-154">如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="9c087-155">新增下列`WebMail`設定，以 *\_AppStart.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c087-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="9c087-156">修改下列電子郵件的程式碼中的相關的設定：</span><span class="sxs-lookup"><span data-stu-id="9c087-156">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="9c087-157">設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="9c087-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="9c087-158">設定`your-user-name-here`SMTP 伺服器帳戶的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="9c087-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="9c087-159">設定`your-account-password`SMTP 伺服器帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="9c087-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="9c087-160">設定`your-email-address-here`您自己的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="9c087-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="9c087-161">這是從訊息傳送的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="9c087-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="9c087-162">(某些電子郵件提供者不讓您指定不同`From`位址，並使用您的使用者名稱，做為`From`地址。)</span><span class="sxs-lookup"><span data-stu-id="9c087-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     <span data-ttu-id="9c087-163">如需有關 SMTP 設定的詳細資訊，請參閱 <<c0> [ 設定電子郵件設定](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)一文中[從 ASP.NET Web Pages (Razor) 網站的 傳送電子郵件](https://go.microsoft.com/fwlink/?LinkID=202899)和[問題傳送電子郵件](https://go.microsoft.com/fwlink/?LinkId=253001#email)中[ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)。</span><span class="sxs-lookup"><span data-stu-id="9c087-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
4. <span data-ttu-id="9c087-164">儲存 *\_AppStart.cshtml*檔案，並將它關閉。</span><span class="sxs-lookup"><span data-stu-id="9c087-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
5. <span data-ttu-id="9c087-165">在根資料夾中的網站，建立名為的新頁面*TestEmail.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
6. <span data-ttu-id="9c087-166">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="9c087-166">Replace the existing content with the following:</span></span> 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. <span data-ttu-id="9c087-167">執行*TestEmail.cshtml*瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
8. <span data-ttu-id="9c087-168">填寫欄位，以將電子郵件訊息傳送給自己，然後按一下**傳送**。</span><span class="sxs-lookup"><span data-stu-id="9c087-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
9. <span data-ttu-id="9c087-169">請檢查您的電子郵件，藉此確定您已收到的訊息。</span><span class="sxs-lookup"><span data-stu-id="9c087-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="9c087-170">此範例中的重要部分是，您通常不會變更的設定 — 喜歡您的 SMTP 伺服器和您的電子郵件認證的名稱 — 中設定 *\_AppStart.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="9c087-171">這樣一來，您不需要再次設定它們，每個頁面中您用來傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="9c087-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="9c087-172">（雖然如果基於某些原因，您需要變更這些設定，您可以設定它們個別頁面中。）在頁面中，您只會設定的值，通常會變更每個時間，例如收件者和電子郵件訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="9c087-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="9c087-173">執行程式碼之前和之後的資料夾中的檔案</span><span class="sxs-lookup"><span data-stu-id="9c087-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="9c087-174">您可以使用，如同 *\_AppStart.cshtml*網站中的執行之前，請撰寫程式碼，您可以撰寫程式碼執行之前 （和之後） 執行的特定資料夾中的任何頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="9c087-175">這可用於使用之類的設定相同版面配置頁中的所有頁面的資料夾，或檢查使用者已登入，再執行資料夾中的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="9c087-176">頁面的特別資料夾，您可以在程式碼檔案名 *\_PageStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="9c087-177">下圖顯示如何 *\_PageStart.cshtml*頁面上運作。</span><span class="sxs-lookup"><span data-stu-id="9c087-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="9c087-178">當要求傳入網頁時，ASP.NET 先檢查 *\_AppStart.cshtml*頁面上，並執行的。</span><span class="sxs-lookup"><span data-stu-id="9c087-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="9c087-179">則 ASP.NET 會檢查是否有 *\_PageStart.cshtml*頁面，然後如果是的話，會執行。</span><span class="sxs-lookup"><span data-stu-id="9c087-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="9c087-180">然後，它會執行要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-180">It then runs the requested page.</span></span>

<span data-ttu-id="9c087-181">內部 *\_PageStart.cshtml*頁面上，您可以在您想要執行包含要求的頁面處理期間指定的位置`RunPage`方法。</span><span class="sxs-lookup"><span data-stu-id="9c087-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="9c087-182">這可讓您要求的頁面執行前執行的程式碼，然後再次之後。</span><span class="sxs-lookup"><span data-stu-id="9c087-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="9c087-183">如果您未加`RunPage`中的所有程式碼 *\_PageStart.cshtml*執行，然後按一下 要求的頁面會自動執行。</span><span class="sxs-lookup"><span data-stu-id="9c087-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[影像]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="9c087-185">ASP.NET 可讓您建立階層 *\_PageStart.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="9c087-186">您可以將放 *\_PageStart.cshtml*根目錄中的站台和任何子資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="9c087-187">要求頁面時，  *\_PageStart.cshtml*檔案 （最接近的站台根目錄） 的最高的層級執行，後面接著 *\_PageStart.cshtml*檔案中的下一步子資料夾，等下的子資料夾結構，直到在要求到達的資料夾，包含要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="9c087-188">之後所有適用 *\_PageStart.cshtml*檔案已執行、 執行要求的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="9c087-189">比方說，您可能會有下列的組合 *\_PageStart.cshtml*檔案並*Default.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="9c087-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="9c087-190">當您執行 */myfolder/default.cshtml*，您會看到下列訊息：</span><span class="sxs-lookup"><span data-stu-id="9c087-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="9c087-191">執行初始化程式碼的資料夾中的所有頁面</span><span class="sxs-lookup"><span data-stu-id="9c087-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="9c087-192">為充分利用 *\_PageStart.cshtml*檔案是初始化的單一資料夾中的所有檔案相同的 [配置] 頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="9c087-193">在根資料夾中，建立名為的新資料夾*InitPages*。</span><span class="sxs-lookup"><span data-stu-id="9c087-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="9c087-194">在  *InitPages*資料夾，您的網站，建立名為 *\_PageStart.cshtml*並以下列內容取代預設標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="9c087-195">在網站根目錄中，建立名為*共用*。</span><span class="sxs-lookup"><span data-stu-id="9c087-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="9c087-196">在 *共用*資料夾中，建立名為 *\_Layout1.cshtml*並以下列內容取代預設標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="9c087-197">在  *InitPages*資料夾中，建立名為*Content1.cshtml*並以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="9c087-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="9c087-198">在  *InitPages*資料夾中，建立另一個名為的檔案*Content2.cshtml*並以下列內容取代預設標記：</span><span class="sxs-lookup"><span data-stu-id="9c087-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="9c087-199">執行*Content1.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9c087-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[影像]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="9c087-201">當*Content1.cshtml*頁面上執行時，  *\_PageStart.cshtml*檔中設定`Layout`也設`PageData["MyBackground"]`色彩。</span><span class="sxs-lookup"><span data-stu-id="9c087-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="9c087-202">在  *Content1.cshtml*，版面配置和色彩會套用。</span><span class="sxs-lookup"><span data-stu-id="9c087-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="9c087-203">顯示*Content2.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9c087-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="9c087-204">版面配置是一樣的因為這兩個頁面使用相同的版面配置頁和色彩，初始化 *\_PageStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="9c087-205">使用\_PageStart.cshtml 來處理錯誤</span><span class="sxs-lookup"><span data-stu-id="9c087-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="9c087-206">另一個適用於 *\_PageStart.cshtml*檔案是建立來處理任何可能發生的程式設計錯誤 （例外狀況） 的方式 *.cshtml*資料夾中的頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="9c087-207">這個範例會示範一個方式來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="9c087-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="9c087-208">在根資料夾中，建立名為*InitCatch*。</span><span class="sxs-lookup"><span data-stu-id="9c087-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="9c087-209">在  *InitCatch*資料夾，您的網站，建立名為 *\_PageStart.cshtml*並以下列內容取代現有的標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="9c087-210">在此程式碼中，您嘗試明確執行要求的頁面，藉由呼叫`RunPage`方法內`try`區塊。</span><span class="sxs-lookup"><span data-stu-id="9c087-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="9c087-211">如果在要求中發生程式設計錯誤頁面內的程式碼`catch`封鎖執行。</span><span class="sxs-lookup"><span data-stu-id="9c087-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="9c087-212">在此情況下，程式碼會重新導向至網頁 (*Error.cshtml*) 並將 URL 的過程中發生錯誤的檔案名稱傳遞。</span><span class="sxs-lookup"><span data-stu-id="9c087-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="9c087-213">（您將建立頁面很快就。）</span><span class="sxs-lookup"><span data-stu-id="9c087-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="9c087-214">在  *InitCatch*資料夾，您的網站，建立名為*Exception.cshtml*並以下列內容取代現有的標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="9c087-215">基於此範例的詳細資訊，您所要做的此頁面是刻意錯誤，即可建立嘗試開啟不存在的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="9c087-216">在根資料夾中，建立名為*Error.cshtml*並以下列內容取代現有的標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="9c087-217">在此頁面中，運算式`@Request["source"]`取得在 url 的值，並顯示它。</span><span class="sxs-lookup"><span data-stu-id="9c087-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="9c087-218">在工具列中，按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="9c087-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="9c087-219">執行*Exception.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9c087-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[影像]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="9c087-221">因為發生錯誤，所以*Exception.cshtml*，則 *\_PageStart.cshtml*頁面重新導向至*Error.cshtml*檔案，其中會顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="9c087-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="9c087-222">如需例外狀況的詳細資訊，請參閱[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkID=251587)。</span><span class="sxs-lookup"><span data-stu-id="9c087-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="9c087-223">使用\_PageStart.cshtml 限制資料夾存取權</span><span class="sxs-lookup"><span data-stu-id="9c087-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="9c087-224">您也可以使用 *\_PageStart.cshtml*來限制存取的資料夾中的所有檔案的檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="9c087-225">在 WebMatrix 中，建立新的網站使用**站台從範本**選項。</span><span class="sxs-lookup"><span data-stu-id="9c087-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="9c087-226">從可用範本中，選取**入門網站**。</span><span class="sxs-lookup"><span data-stu-id="9c087-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="9c087-227">在根資料夾中，建立名為*AuthenticatedContent*。</span><span class="sxs-lookup"><span data-stu-id="9c087-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="9c087-228">在  *AuthenticatedContent*資料夾中，建立名為 *\_PageStart.cshtml*並以下列內容取代現有的標記和程式碼：</span><span class="sxs-lookup"><span data-stu-id="9c087-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="9c087-229">程式碼一開始會防止快取資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="9c087-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="9c087-230">（這是適用案例如公用電腦，不需要一位使用者的快取的頁面，可供下一位使用者的情況）。接下來，程式碼會判斷是否使用者已登入站台之前，他們可以檢視任何頁面的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="9c087-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="9c087-231">如果使用者未登入中，程式碼會重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="9c087-232">登入頁面可以讓使用者返回包含名為查詢字串值，如果原始要求的頁面`ReturnUrl`。</span><span class="sxs-lookup"><span data-stu-id="9c087-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="9c087-233">建立新的頁面中*AuthenticatedContent*名為資料夾*Page.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9c087-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="9c087-234">以下列內容取代預設標記：</span><span class="sxs-lookup"><span data-stu-id="9c087-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="9c087-235">執行*Page.cshtml*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="9c087-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="9c087-236">程式碼會將您導向登入頁面。</span><span class="sxs-lookup"><span data-stu-id="9c087-236">The code redirects you to a login page.</span></span> <span data-ttu-id="9c087-237">您必須註冊才能登入。</span><span class="sxs-lookup"><span data-stu-id="9c087-237">You must register before logging in.</span></span> <span data-ttu-id="9c087-238">您已註冊並登入之後，您可以瀏覽的頁面，並檢視其內容。</span><span class="sxs-lookup"><span data-stu-id="9c087-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="9c087-239">其他資源</span><span class="sxs-lookup"><span data-stu-id="9c087-239">Additional Resources</span></span>

[<span data-ttu-id="9c087-240">程式設計使用 Razor 語法的 ASP.NET Web Pages 簡介</span><span class="sxs-lookup"><span data-stu-id="9c087-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
