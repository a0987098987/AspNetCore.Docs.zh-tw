---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: ASP.NET Web Pages 2 中的最上層功能 |Microsoft 文件
author: microsoft
description: 本主題概略說明 ASP.NET Web Pages 2，隨附於 WebMatr 的輕量型 web 程式設計架構中的最上層的新功能...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="the-top-features-in-aspnet-web-pages-2"></a><span data-ttu-id="e6e63-103">在 ASP.NET Web Pages 2 上的功能</span><span class="sxs-lookup"><span data-stu-id="e6e63-103">The Top Features in ASP.NET Web Pages 2</span></span>
====================
<span data-ttu-id="e6e63-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e6e63-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e6e63-105">本文章提供在 ASP.NET Web Pages 2 RC 中，隨附於的輕量型 web 程式設計架構最上層的新功能的概觀[Microsoft WebMatrix 2 Rc<](https://www.microsoft.com/web/)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-105">This article provides an overview of the top new features in the ASP.NET Web Pages 2 RC, a lightweight web programming framework that is included with [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).</span></span>
> 
> <span data-ttu-id="e6e63-106">**包含的內容：**</span><span class="sxs-lookup"><span data-stu-id="e6e63-106">**What's included:**</span></span> 
> 
> - [<span data-ttu-id="e6e63-107">安裝 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e6e63-107">Installing WebMatrix</span></span>](#install)
> - [<span data-ttu-id="e6e63-108">新功能和增強功能</span><span class="sxs-lookup"><span data-stu-id="e6e63-108">New and enhanced features</span></span>](#New_and_Enhanced_Features)
> 
>     - [<span data-ttu-id="e6e63-109">RC 版本變更</span><span class="sxs-lookup"><span data-stu-id="e6e63-109">Changes for the RC release</span></span>](#Changes_for_the_RC_Version)
>     - [<span data-ttu-id="e6e63-110">測試版的變更</span><span class="sxs-lookup"><span data-stu-id="e6e63-110">Changes for the Beta release</span></span>](#Changes_for_the_Beta_Version)
>     - [<span data-ttu-id="e6e63-111">使用新的和更新的網站範本</span><span class="sxs-lookup"><span data-stu-id="e6e63-111">Using the New and Updated Site Templates</span></span>](#templates)
>     - [<span data-ttu-id="e6e63-112">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="e6e63-112">Validating User Input</span></span>](#validation)
>     - [<span data-ttu-id="e6e63-113">啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入</span><span class="sxs-lookup"><span data-stu-id="e6e63-113">Enabling logins from Facebook and other sites using OAuth and OpenID</span></span>](#oauthsetup)
>     - [<span data-ttu-id="e6e63-114">新增對應使用對應 Helper</span><span class="sxs-lookup"><span data-stu-id="e6e63-114">Adding Maps using the Maps Helper</span></span>](#maphelper)
>     - [<span data-ttu-id="e6e63-115">執行網頁應用程式並排顯示</span><span class="sxs-lookup"><span data-stu-id="e6e63-115">Running Web Pages Applications Side by Side</span></span>](#sidebyside)
>     - [<span data-ttu-id="e6e63-116">行動裝置的轉譯頁面</span><span class="sxs-lookup"><span data-stu-id="e6e63-116">Rendering Pages for Mobile Devices</span></span>](#mobile)
> - [<span data-ttu-id="e6e63-117">其他資源</span><span class="sxs-lookup"><span data-stu-id="e6e63-117">Additional Resources</span></span>](#resources)
> 
> > [!NOTE]
> > <span data-ttu-id="e6e63-118">本主題假設您使用 WebMatrix 來處理您的 ASP.NET Web Pages 2 程式碼。</span><span class="sxs-lookup"><span data-stu-id="e6e63-118">This topic assumes that you are using WebMatrix to work with your ASP.NET Web Pages 2 code.</span></span> <span data-ttu-id="e6e63-119">不過，如 Web Pages 1 中，您也可以建立使用 Visual Studio Web Pages 2 網站，可讓您強化 IntelliSense 功能，以及偵錯。</span><span class="sxs-lookup"><span data-stu-id="e6e63-119">However, as with Web Pages 1, you can also create Web Pages 2 websites using Visual Studio, which gives you enhanced IntelliSense capabilities and debugging.</span></span> <span data-ttu-id="e6e63-120">若要使用 Visual Studio 中的網頁，您必須先安裝 Visual Studio 2010 SP1、 Visual Web Developer Express 2010 SP1 或 Visual Studio 11 Beta。</span><span class="sxs-lookup"><span data-stu-id="e6e63-120">To work with Web Pages in Visual Studio, you must first install Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1, or Visual Studio 11 Beta.</span></span> <span data-ttu-id="e6e63-121">接著，安裝 ASP.NET MVC 4 Beta，其中包含用於 Visual Studio 中建立 ASP.NET MVC 4 和 Web Pages 2 應用程式範本和工具。</span><span class="sxs-lookup"><span data-stu-id="e6e63-121">Then install the ASP.NET MVC 4 Beta, which includes templates and tools for creating ASP.NET MVC 4 and Web Pages 2 applications in Visual Studio.</span></span>
> 
> 
> <span data-ttu-id="e6e63-122">*上次更新： 18 年 6 月 2012年*</span><span class="sxs-lookup"><span data-stu-id="e6e63-122">*Last update: 18 June 2012*</span></span>


<a id="install"></a>
## <a name="installing-webmatrix"></a><span data-ttu-id="e6e63-123">安裝 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e6e63-123">Installing WebMatrix</span></span>

<span data-ttu-id="e6e63-124">若要安裝 Web 網頁，您可以使用 Microsoft Web Platform Installer 中，這是免費的應用程式，可讓您輕鬆安裝及設定 web 相關的技術。</span><span class="sxs-lookup"><span data-stu-id="e6e63-124">To install Web Pages, you can use the Microsoft Web Platform Installer, which is a free application that makes it easy to install and configure web-related technologies.</span></span> <span data-ttu-id="e6e63-125">您將安裝 WebMatrix 2 beta 版，包括 Web Pages 2 Beta。</span><span class="sxs-lookup"><span data-stu-id="e6e63-125">You will install the WebMatrix 2 Beta, which includes Web Pages 2 Beta.</span></span>

1. <span data-ttu-id="e6e63-126">瀏覽至 Web Platform Installer 的最新版本的安裝頁面：</span><span class="sxs-lookup"><span data-stu-id="e6e63-126">Browse to the installation page for the latest version of the Web Platform Installer:</span></span>

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > <span data-ttu-id="e6e63-127">如果您已經安裝 WebMatrix 1，這項安裝會更新它到 WebMatrix 2 Beta。</span><span class="sxs-lookup"><span data-stu-id="e6e63-127">If you already have WebMatrix 1 installed, this installation updates it to WebMatrix 2 Beta.</span></span> <span data-ttu-id="e6e63-128">您可以執行相同的電腦上使用 1 或 2 的版本所建立的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-128">You can run websites that were created using version 1 or 2 on the same computer.</span></span> <span data-ttu-id="e6e63-129">如需詳細資訊，請參閱的章節[執行網頁應用程式並存](#sidebyside)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-129">For more information, see the section on [Running Web Pages Applications Side by Side](#sidebyside).</span></span>
2. <span data-ttu-id="e6e63-130">選擇**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="e6e63-130">Choose **Install Now**.</span></span> 

    <span data-ttu-id="e6e63-131">如果您使用 Internet Explorer，請移至下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="e6e63-131">If you use Internet Explorer, go to the next step.</span></span> <span data-ttu-id="e6e63-132">如果您使用不同的瀏覽器，如 Mozilla Firefox 或 Google Chrome，提示您儲存*Webmatrix.exe*檔案儲存到電腦。</span><span class="sxs-lookup"><span data-stu-id="e6e63-132">If you use a different browser like Mozilla Firefox or Google Chrome, you are prompted to save the *Webmatrix.exe* file to your computer.</span></span> <span data-ttu-id="e6e63-133">儲存檔案，然後按一下以啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-133">Save the file and then click it to launch the installer.</span></span>
3. <span data-ttu-id="e6e63-134">執行安裝程式，並選擇**安裝** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-134">Run the installer and choose the **Install** button.</span></span> <span data-ttu-id="e6e63-135">這會安裝 WebMatrix 及網頁。</span><span class="sxs-lookup"><span data-stu-id="e6e63-135">This installs WebMatrix and Web Pages.</span></span>

## <a id="New_and_Enhanced_Features"></a>  <span data-ttu-id="e6e63-136">新功能和增強功能</span><span class="sxs-lookup"><span data-stu-id="e6e63-136">New and Enhanced Features</span></span>

### <a id="Changes_for_the_RC_Version"></a>  <span data-ttu-id="e6e63-137">RC 版本 (年 6 月 2012) 的變更</span><span class="sxs-lookup"><span data-stu-id="e6e63-137">Changes for the RC Version (June 2012)</span></span>

<span data-ttu-id="e6e63-138">2012 年 6 月的 RC 版本版有已於 2012 年 3 月發行的 Beta 版本重新整理的一些變更。</span><span class="sxs-lookup"><span data-stu-id="e6e63-138">The RC version release in June 2012 has a few changes from the Beta version refresh that was released in March 2012.</span></span> <span data-ttu-id="e6e63-139">這些變更包括：</span><span class="sxs-lookup"><span data-stu-id="e6e63-139">These changes are:</span></span>

- <span data-ttu-id="e6e63-140">A`Validation.AddFormError`方法已加入至`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-140">A `Validation.AddFormError` method was added to the `Validation` helper.</span></span> <span data-ttu-id="e6e63-141">這非常有用，如果您以手動方式執行的驗證 （例如，您驗證傳遞查詢字串中的值） 和您想要新增錯誤訊息，可以顯示`Html.ValidationSummary`方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-141">This is useful if you perform validation manually (for example, you validate a value that is passed in the query string) and you want to add an error message that can be displayed by the `Html.ValidationSummary` method.</span></span> <span data-ttu-id="e6e63-142">如需詳細資訊，請參閱下節[驗證資料，不會直接從使用者](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)中[ASP.NET Web Pages (Razor) 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-142">For more information, see the section [Validating Data That Doesn't Come Directly From Users](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) in [Validating User Input in ASP.NET Web Pages (Razor) Sites](https://go.microsoft.com/fwlink/?LinkId=253002).</span></span>
- <span data-ttu-id="e6e63-143">統合及縮製的功能已經移除了核心 ASP.NET Web Pages 2 組件。</span><span class="sxs-lookup"><span data-stu-id="e6e63-143">The functionality for bundling and minification has been removed from the core ASP.NET Web Pages 2 assemblies.</span></span> <span data-ttu-id="e6e63-144">因此，`Assets`不提供本文件稍後所列的協助程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-144">As a consequence, the `Assets` helper listed later in this document is not available.</span></span> <span data-ttu-id="e6e63-145">相反地，您必須安裝[ASP.NET 最佳化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="e6e63-145">Instead, you must install the [ASP.NET Optimization](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet package.</span></span> <span data-ttu-id="e6e63-146">如需詳細資訊，請參閱[組合和 ASP.NET Web Pages (Razor) 網站中的 用資產](https://go.microsoft.com/fwlink/?LinkId=255373)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-146">For more information, see [Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkId=255373).</span></span>
- <span data-ttu-id="e6e63-147">已新增支援 ASP.NET Web Pages 2 的其他組件。</span><span class="sxs-lookup"><span data-stu-id="e6e63-147">Additional assemblies to support ASP.NET Web Pages 2 have been added.</span></span> <span data-ttu-id="e6e63-148">只有明顯的影響，這項變更是您可能會看到多個組件中的站台*bin*資料夾在建立站台，或將網站部署至主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="e6e63-148">The only noticeable effect of this change is that you might see more assemblies in a site's *bin* folder after you create a site or deploy a site to a hosting provider.</span></span>

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a><span data-ttu-id="e6e63-149">Beta 版中 (2 月版 2012) 的變更</span><span class="sxs-lookup"><span data-stu-id="e6e63-149">Changes for the Beta Version (February 2012)</span></span>

<span data-ttu-id="e6e63-150">在 2012 年 2 月發行的 Beta 版有只有少數變更從 2011 年 12 月發行的 Beta 版本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-150">The Beta version released in February 2012 has only a few changes from the Beta version that was released in December 2011.</span></span> <span data-ttu-id="e6e63-151">這些變更包括：</span><span class="sxs-lookup"><span data-stu-id="e6e63-151">These changes are:</span></span>

- <span data-ttu-id="e6e63-152">Razor 現在支援條件式屬性。</span><span class="sxs-lookup"><span data-stu-id="e6e63-152">Razor now supports conditional attributes.</span></span> <span data-ttu-id="e6e63-153">HTML 項目，如果您設定屬性的值，會在伺服器程式碼中解析`false`或`null`，ASP.NET 不會完全呈現的屬性。</span><span class="sxs-lookup"><span data-stu-id="e6e63-153">In an HTML element, if you set an attribute to a value that resolves in server code to `false` or `null`, ASP.NET does not render the attribute at all.</span></span> <span data-ttu-id="e6e63-154">例如，假設您有下列核取方塊標記：</span><span class="sxs-lookup"><span data-stu-id="e6e63-154">For example, imagine you have the following markup for a check box:</span></span>

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    <span data-ttu-id="e6e63-155">如果值`checked1`解析成`false`或`null`、`checked`屬性不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="e6e63-155">If the value of `checked1` resolves to `false` or to `null`, the `checked` attribute is not rendered.</span></span> <span data-ttu-id="e6e63-156">這是一項重大變更。</span><span class="sxs-lookup"><span data-stu-id="e6e63-156">This is a breaking change.</span></span>
- <span data-ttu-id="e6e63-157">`Validation.GetHtml`方法重新命名為`Validation.For`。</span><span class="sxs-lookup"><span data-stu-id="e6e63-157">The `Validation.GetHtml` method has been renamed to `Validation.For`.</span></span> <span data-ttu-id="e6e63-158">這是一項重大變更。`Validation.GetHtml`在 Beta 版中無法運作。</span><span class="sxs-lookup"><span data-stu-id="e6e63-158">This is a breaking change; `Validation.GetHtml` will not work in the Beta release.</span></span>
- <span data-ttu-id="e6e63-159">您現在可以包含`~`運算子標記中的，而不使用參考網站根目錄`Href`函式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-159">You can now include the `~` operator in markup to reference the site root without using the `Href` function.</span></span> <span data-ttu-id="e6e63-160">(亦即，Razor 剖析器現在可以找出並解決`~`運算子，而不需要的明確方法呼叫`Href`。)`Href`方法仍能運作，因此這不是一項重大變更。</span><span class="sxs-lookup"><span data-stu-id="e6e63-160">(That is, the Razor parser can now find and resolve the `~` operator without requiring an explicit method call to `Href`.) The `Href` method still works, so this is not a breaking change.</span></span>

    <span data-ttu-id="e6e63-161">例如，如果您先前已標記如下：</span><span class="sxs-lookup"><span data-stu-id="e6e63-161">For example, if you previously had markup like this:</span></span>

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    <span data-ttu-id="e6e63-162">您現在可以使用標記如下：</span><span class="sxs-lookup"><span data-stu-id="e6e63-162">You can now use markup like this:</span></span>

    `<a href="~/Default.cshtml">Home</a>`
- <span data-ttu-id="e6e63-163">`Scripts`已取代為資產 （資源） 管理的協助程式`Assets`helper，有稍微不同的方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e6e63-163">The `Scripts` helper for assets (resource) management has been replaced with the `Assets` helper, which has slightly different methods, such as the following:</span></span>

  - <span data-ttu-id="e6e63-164">如`Scripts.Add`，使用 `Assets.AddScript`</span><span class="sxs-lookup"><span data-stu-id="e6e63-164">For `Scripts.Add`, use `Assets.AddScript`</span></span>
  - <span data-ttu-id="e6e63-165">如`Scripts.GetScriptTags`，使用 `Assets.GetScripts`</span><span class="sxs-lookup"><span data-stu-id="e6e63-165">For `Scripts.GetScriptTags`, use `Assets.GetScripts`</span></span>

    <span data-ttu-id="e6e63-166">這是一項重大變更。`Scripts`類別不是 Beta 版中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-166">This is a breaking change; the `Scripts` class is not available in the Beta release.</span></span> <span data-ttu-id="e6e63-167">這項變更已更新此文件的程式碼範例使用資產管理。</span><span class="sxs-lookup"><span data-stu-id="e6e63-167">The code examples in this document that use asset management have been updated with this change.</span></span>

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a><span data-ttu-id="e6e63-168">使用新的和更新的網站範本</span><span class="sxs-lookup"><span data-stu-id="e6e63-168">Using the New and Updated Site Templates</span></span>

<span data-ttu-id="e6e63-169">**入門網站**範本已更新以便在 Web Pages 2 上執行，預設值。</span><span class="sxs-lookup"><span data-stu-id="e6e63-169">The **Starter Site** template has been updated so that it runs on Web Pages 2 by default.</span></span> <span data-ttu-id="e6e63-170">它也包含下列新功能：</span><span class="sxs-lookup"><span data-stu-id="e6e63-170">It also includes the following new capabilities:</span></span>

- <span data-ttu-id="e6e63-171">行動設備友善網頁呈現。</span><span class="sxs-lookup"><span data-stu-id="e6e63-171">Mobile-friendly page rendering.</span></span> <span data-ttu-id="e6e63-172">透過使用 CSS 樣式和`@media`選取器**入門網站**提供改善的呈現頁面的較小的螢幕，包括行動裝置螢幕上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-172">Through the use of CSS styles and the `@media` selector, the **Starter Site** provides improved rendering of pages on smaller screens, including mobile device screens.</span></span>
- <span data-ttu-id="e6e63-173">改良的成員資格和驗證選項。</span><span class="sxs-lookup"><span data-stu-id="e6e63-173">Improved membership and authentication options.</span></span> <span data-ttu-id="e6e63-174">您可以讓使用者登入您從其他社交網路網站，例如 Twitter、 Facebook 和 Windows Live 使用其帳戶的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-174">You can let users log into your site using their accounts from other social networking sites, such as Twitter, Facebook, and Windows Live.</span></span> <span data-ttu-id="e6e63-175">如需詳細資訊，請參閱[啟用登入的 Facebook 和其他站台使用 OAuth 和 OpenID](#oauthsetup) > 一節。</span><span class="sxs-lookup"><span data-stu-id="e6e63-175">For more information, see the [Enabling Logins from Facebook and Other Sites using OAuth and OpenID](#oauthsetup) section.</span></span>
- <span data-ttu-id="e6e63-176">HTML5 項目。</span><span class="sxs-lookup"><span data-stu-id="e6e63-176">HTML5 elements.</span></span>

<span data-ttu-id="e6e63-177">新**個人網站**範本可讓您建立包含個人部落格、 相片頁面和 Twitter 網頁的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-177">The new **Personal Site** template lets you create a website that contains a personal blog, a photos page, and a Twitter page.</span></span> <span data-ttu-id="e6e63-178">您可以自訂為基礎的站台**個人網站**範本，方法如下：</span><span class="sxs-lookup"><span data-stu-id="e6e63-178">You can customize a site based on the **Personal Site** template by doing the following:</span></span>

- <span data-ttu-id="e6e63-179">藉由編輯配置檔案中變更網站的外觀 (*\_SiteLayout.cshtml*) 和樣式檔案 (*Site.css*)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-179">Change the look of the site by editing the layout file (*\_SiteLayout.cshtml*) and the styles file (*Site.css*).</span></span>
- <span data-ttu-id="e6e63-180">安裝 NuGet 封裝會將功能加入至您的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-180">Install NuGet packages that add functionality to your site.</span></span> <span data-ttu-id="e6e63-181">如需如何安裝封裝，包括 ASP.NET Web Helpers Library，請參閱本教學課程[安裝 helper](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-181">For information about how to install packages, including the ASP.NET Web Helpers Library, see the tutorial about [installing helpers](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).</span></span>

<span data-ttu-id="e6e63-182">若要存取**個人網站**範本中，選擇**範本**上 WebMatrix**快速入門**螢幕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-182">To access the **Personal Site** template, choose **Templates** on the WebMatrix **Quick Start** screen.</span></span>

<span data-ttu-id="e6e63-183">[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-183">[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)</span></span>

<span data-ttu-id="e6e63-184">在**範本**對話方塊方塊中，選擇**個人網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-184">In the **Templates** dialog box, choose the **Personal Site** template.</span></span>

<span data-ttu-id="e6e63-185">[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-185">[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)</span></span>

<span data-ttu-id="e6e63-186">登陸頁面**個人網站**範本可讓您設定您的部落格連結 Twitter 網頁和相片頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-186">The landing page of the **Personal Site** template lets you follow links to set up your blog, Twitter page, and photos page.</span></span>

<span data-ttu-id="e6e63-187">[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-187">[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)</span></span>

<a id="validation"></a>
### <a name="validating-user-input"></a><span data-ttu-id="e6e63-188">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="e6e63-188">Validating User Input</span></span>

<span data-ttu-id="e6e63-189">在 Web Pages 1，來驗證使用者輸入，在提交表單中，您使用`System.Web.WebPages.Html.ModelState`類別。</span><span class="sxs-lookup"><span data-stu-id="e6e63-189">In Web Pages 1, to validate user input on submitted forms, you use the `System.Web.WebPages.Html.ModelState` class.</span></span> <span data-ttu-id="e6e63-190">(說明這種情形有幾個程式碼範例在 Web Pages 1 教學課程中標題為[使用資料](../data/5-working-with-data.md)。)您仍然可以使用這種方法，在 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="e6e63-190">(This is illustrated in several of the code samples in the Web Pages 1 tutorial titled [Working with Data](../data/5-working-with-data.md).) You can still use this approach in Web Pages 2.</span></span> <span data-ttu-id="e6e63-191">不過，Web Pages 2 也提供改良的工具，驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="e6e63-191">However, Web Pages 2 also offers improved tools for validating user input:</span></span>

- <span data-ttu-id="e6e63-192">新的驗證類別，包括`System.Web.WebPages.ValidationHelper`和`System.Web.WebPages.Validator`，它可讓您執行幾行程式碼的功能強大的驗證工作。</span><span class="sxs-lookup"><span data-stu-id="e6e63-192">New validation classes, including `System.Web.WebPages.ValidationHelper` and `System.Web.WebPages.Validator`, that let you do powerful validation tasks with a few lines of code.</span></span>
- <span data-ttu-id="e6e63-193">（選擇性） 用戶端驗證，而不需要往返伺服器使用者，若要檢查立即回應提供驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="e6e63-193">Optionally, client-side validation, which provides immediate feedback to the user instead of requiring a round trip to the server to check for validation errors.</span></span> <span data-ttu-id="e6e63-194">（基於安全性理由，驗證伺服器上執行則即使事先在用戶端已執行檢查。）</span><span class="sxs-lookup"><span data-stu-id="e6e63-194">(For security reasons, validation is performed on the server even if the checks have been performed in the client beforehand.)</span></span>

<span data-ttu-id="e6e63-195">若要使用新的驗證功能，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e6e63-195">To use the new validation features, do the following:</span></span>

<span data-ttu-id="e6e63-196">在頁面的程式碼，註冊項目以進行驗證所使用的方法`Validation`helper: `Validation.RequireField`， `Validation.RequireFields` （若要註冊為必要的多個項目），或`Validation.Add`。</span><span class="sxs-lookup"><span data-stu-id="e6e63-196">In the page's code, register an element to be validated by using methods of the `Validation` helper: `Validation.RequireField`, `Validation.RequireFields` (to register multiple elements to be required), or `Validation.Add`.</span></span> <span data-ttu-id="e6e63-197">`Add`方法可讓您指定其他類型的驗證檢查，例如資料型別檢查、 比較不同的欄位，字串長度檢查中的項目，以及模式 （使用規則運算式）。</span><span class="sxs-lookup"><span data-stu-id="e6e63-197">The `Add` method lets you specify other types of validation checks, like data-type checking, comparing entries in different fields, string-length checks, and patterns (using regular expressions).</span></span> <span data-ttu-id="e6e63-198">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="e6e63-198">Here are some examples:</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

<span data-ttu-id="e6e63-199">若要顯示特定欄位的錯誤，請呼叫`Html.ValidationMessage`中驗證每個項目的標記：</span><span class="sxs-lookup"><span data-stu-id="e6e63-199">To display a field-specific error, call `Html.ValidationMessage` in the markup for each element being validated:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

<span data-ttu-id="e6e63-200">若要顯示的摘要 (`<ul>`清單) 的頁面中的所有錯誤`Html.ValidationSummary`標記中：</span><span class="sxs-lookup"><span data-stu-id="e6e63-200">To display a summary (`<ul>` list) of all the errors in the page, `Html.ValidationSummary` in the markup:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

<span data-ttu-id="e6e63-201">這些步驟便足以實作伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="e6e63-201">These steps are enough to implement server-side validation.</span></span> <span data-ttu-id="e6e63-202">如果您想要新增用戶端驗證，請執行下列此外。</span><span class="sxs-lookup"><span data-stu-id="e6e63-202">If you want to add client-side validation, do the following in addition.</span></span>

<span data-ttu-id="e6e63-203">加入下列指令碼檔參考內部`<head>`web 頁面的區段。</span><span class="sxs-lookup"><span data-stu-id="e6e63-203">Add the following script file references inside the `<head>` section of a web page.</span></span> <span data-ttu-id="e6e63-204">前兩個指令碼參考會指向內容傳遞網路 (CDN) 伺服器上的遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-204">The first two script references point to remote files on a content delivery network (CDN) server.</span></span> <span data-ttu-id="e6e63-205">第三個參考會指向本機指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-205">The third reference points to a local script file.</span></span> <span data-ttu-id="e6e63-206">無法使用 CDN 時，實際執行應用程式應該實作後援。</span><span class="sxs-lookup"><span data-stu-id="e6e63-206">Production apps should implement a fallback when the CDN is unavailable.</span></span> <span data-ttu-id="e6e63-207">測試此後援。</span><span class="sxs-lookup"><span data-stu-id="e6e63-207">Test the fallback.</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

<span data-ttu-id="e6e63-208">若要取得的本機副本最簡單的方式*jquery.validate.unobtrusive.min.js*程式庫是建立新的 Web Pages 站台中，根據其中一個網站範本 （例如入門網站）。</span><span class="sxs-lookup"><span data-stu-id="e6e63-208">The easiest way to get a local copy of the *jquery.validate.unobtrusive.min.js* library is to create a new Web Pages site based on one of the site templates (such as Starter Site).</span></span> <span data-ttu-id="e6e63-209">在範本所建立的站台包含*jquery.validate.unobtrusive.js*資料夾中檔案的指令碼，從中您可以將它複製到您的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-209">The site created by the template includes *jquery.validate.unobtrusive.js* file in its Scripts folder, from which you can copy it to your site.</span></span>

<span data-ttu-id="e6e63-210">如果您的網站使用<em>\_SiteLayout</em>來控制頁面版面配置頁面上，您可以包含這些指令碼參考在該頁面，以便驗證是否能夠使用所有的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-210">If your website uses a<em>\_SiteLayout</em> page to control the page layout, you can include these script references in that page so that validation is available to all content pages.</span></span> <span data-ttu-id="e6e63-211">如果您想要執行驗證，只在特定的頁面上，您可以使用的資產管理員註冊在只有這些頁面上的指令碼。</span><span class="sxs-lookup"><span data-stu-id="e6e63-211">If you want to perform validation only on particular pages, you can use the assets manager to register the scripts on only those pages.</span></span> <span data-ttu-id="e6e63-212">若要這樣做，請呼叫`Assets.AddScript(path)`在頁面中您想要驗證，並參考每個指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-212">To do this, call `Assets.AddScript(path)` in the page that you want to validate and reference each of the script files.</span></span> <span data-ttu-id="e6e63-213">然後將呼叫加入`Assets.GetScripts`中 <em>\_SiteLayout</em>頁面呈現註冊所`<script>`標記。</span><span class="sxs-lookup"><span data-stu-id="e6e63-213">Then add a call to `Assets.GetScripts` in the <em>\_SiteLayout</em> page in order to render the registered `<script>` tags.</span></span> <span data-ttu-id="e6e63-214">如需詳細資訊，請參閱下節[與資產管理員註冊的指令碼](#resmanagement)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-214">For more information, see the section [Registering Scripts with the Assets Manager](#resmanagement).</span></span>

<span data-ttu-id="e6e63-215">在標記中的個別項目，呼叫`Validation.For`方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-215">In the markup for an individual element, call the `Validation.For` method.</span></span> <span data-ttu-id="e6e63-216">這個方法會發出屬性該 jQuery 可以攔截以便提供用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e6e63-216">This method emits attributes that jQuery can hook in order to provide client-side validation.</span></span> <span data-ttu-id="e6e63-217">例如: </span><span class="sxs-lookup"><span data-stu-id="e6e63-217">For example:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

<span data-ttu-id="e6e63-218">下列範例會驗證使用者輸入，在表單上的頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-218">The following example shows a page that validates user input on a form.</span></span> <span data-ttu-id="e6e63-219">若要執行測試此驗證程式碼，請執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="e6e63-219">To run and test this validation code, do this:</span></span>

1. <span data-ttu-id="e6e63-220">建立新的網站使用其中一個 WebMatrix 2 的網站範本，其中包含*指令碼*資料夾，例如**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-220">Create a new web site using one of the WebMatrix 2 site templates that includes a *Scripts* folder, such as the **Starter Site** template.</span></span>
2. <span data-ttu-id="e6e63-221">在新的站台建立新*.cshtml*頁面，然後以下列程式碼取代頁面內容。</span><span class="sxs-lookup"><span data-stu-id="e6e63-221">In the new site, create a new *.cshtml* page, and replace the contents of the page with the following code.</span></span>
3. <span data-ttu-id="e6e63-222">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-222">Run the page in a browser.</span></span> <span data-ttu-id="e6e63-223">輸入有效和無效的值，以查看上驗證的效果。</span><span class="sxs-lookup"><span data-stu-id="e6e63-223">Enter valid and invalid values to see the effects on validation.</span></span> <span data-ttu-id="e6e63-224">例如，將必要的欄位保留空白，或輸入中的字母**信用額度**欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-224">For example, leave a required field blank or enter a letter in the **Credits** field.</span></span>


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

<span data-ttu-id="e6e63-225">當使用者提交有效的輸入，以下是頁面：</span><span class="sxs-lookup"><span data-stu-id="e6e63-225">Here is the page when a user submits valid input:</span></span>

<span data-ttu-id="e6e63-226">[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-226">[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)</span></span>

<span data-ttu-id="e6e63-227">當使用者提交它具有必要的欄位保留空白，以下是頁面：</span><span class="sxs-lookup"><span data-stu-id="e6e63-227">Here is the page when a user submits it with a required field left empty:</span></span>

<span data-ttu-id="e6e63-228">[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-228">[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)</span></span>

<span data-ttu-id="e6e63-229">當使用者將它提交使用中的整數以外的項目時，以下是頁面**信用額度**欄位：</span><span class="sxs-lookup"><span data-stu-id="e6e63-229">Here is the page when a user submits it with something other than an integer in the **Credits** field:</span></span>

<span data-ttu-id="e6e63-230">[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-230">[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)</span></span>

<span data-ttu-id="e6e63-231">如需詳細資訊，請參閱下列部落格文章：</span><span class="sxs-lookup"><span data-stu-id="e6e63-231">For more information, see the following blog posts:</span></span>

- <span data-ttu-id="e6e63-232">[更新網頁 v2 驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)新增驗證使用的基本概念`Validation`helper （只有伺服器端）</span><span class="sxs-lookup"><span data-stu-id="e6e63-232">[Updated validation in Web Pages v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Basics of adding validation using the `Validation` helper (server-side only)</span></span>
- <span data-ttu-id="e6e63-233">[更新網頁 v2、 第 2 部分中的驗證](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)加入用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e6e63-233">[Updated validation in Web Pages v2, Part 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) Adding client-side validation.</span></span>
- <span data-ttu-id="e6e63-234">[更新網頁 v2、 第 3 中的驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)格式驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="e6e63-234">[Updated validation in Web Pages v2, Part 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) Formatting validation errors.</span></span>

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a><span data-ttu-id="e6e63-235">註冊指令碼使用的資產管理員</span><span class="sxs-lookup"><span data-stu-id="e6e63-235">Registering Scripts Using the Assets Manager</span></span>

<span data-ttu-id="e6e63-236">資產管理員是新的功能，您可以使用伺服端程式碼來註冊及轉譯用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="e6e63-236">The assets manager is a new feature that you can use in server code to register and render client scripts.</span></span> <span data-ttu-id="e6e63-237">當您正在使用中 （例如版面配置頁、 內容頁面，協助程式 」 等） 會結合成單一頁面上，在執行階段的多個檔案的程式碼時，此功能很實用。</span><span class="sxs-lookup"><span data-stu-id="e6e63-237">This feature is helpful when you are working with code from multiple files (such as layout pages, content pages, helpers, etc.) that are combined into a single page at run time.</span></span> <span data-ttu-id="e6e63-238">資產管理員協調之來源檔案，請確定指令碼檔案的參考都正確並有效率地在轉譯頁面上，不論哪一個程式碼檔案從呼叫或所呼叫的次數。</span><span class="sxs-lookup"><span data-stu-id="e6e63-238">The assets manager coordinates the source files to make sure that script files are referenced correctly and efficiently on the rendered page, regardless of which code files they are called from or how many times they are called.</span></span> <span data-ttu-id="e6e63-239">資產管理員也會呈現`<script>`標記在正確的位置，以便快速 （而不需要下載指令碼時轉譯） 可以載入的頁面和避免錯誤，可能會發生，如果在呈現之前呼叫的指令碼已完成。</span><span class="sxs-lookup"><span data-stu-id="e6e63-239">The assets manager also renders `<script>` tags in the right place so that the page can load quickly (without downloading scripts while rendering) and to avoid errors that can occur if scripts are called before rendering is complete.</span></span>

<span data-ttu-id="e6e63-240">例如，假設您建立自訂的 helper 可呼叫的 JavaScript 檔案，並於三個不同的地方呼叫此 helper 內容頁面上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-240">For example, suppose that you create a custom helper that calls a JavaScript file, and you call this helper at three different places in your content page code.</span></span> <span data-ttu-id="e6e63-241">如果您不使用的資產管理員來註冊指令碼會呼叫中的協助程式，有三個不同`<script>`所有指向相同的指令碼檔案會都出現在轉譯頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="e6e63-241">If you don't use the assets manager to register the script calls in the helper, three different `<script>` tags that all point to the same script file will appear in your rendered page.</span></span> <span data-ttu-id="e6e63-242">此外，根據 where`<script>`標記會插入在呈現的頁面、 指令碼嘗試存取頁面完全載入之前的特定頁面項目可能會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="e6e63-242">Plus, depending on where the `<script>` tags are inserted in the rendered page, errors may occur if the script tries to access certain page elements before the page fully loads.</span></span> <span data-ttu-id="e6e63-243">如果您使用的資產管理員註冊的指令碼時，會避免這些問題。</span><span class="sxs-lookup"><span data-stu-id="e6e63-243">If you use the assets manager to register the script, you avoid these problems.</span></span>

<span data-ttu-id="e6e63-244">您可以執行此動作，與資產管理員註冊指令碼：</span><span class="sxs-lookup"><span data-stu-id="e6e63-244">You can register a script with the assets manager by doing this:</span></span>

- <span data-ttu-id="e6e63-245">需要參考指令碼的程式碼，在呼叫`Assets.AddScript`方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-245">In the code that needs to reference the script, call the `Assets.AddScript` method.</span></span>
- <span data-ttu-id="e6e63-246">在 *\_SiteLayout*頁面上，呼叫`Assets.GetScripts`方法以呈現`<script>`標記。</span><span class="sxs-lookup"><span data-stu-id="e6e63-246">In a *\_SiteLayout* page, call the `Assets.GetScripts` method to render the `<script>` tags.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e6e63-247">將呼叫`Assets.GetScripts`中的最後一個項目為`<body>`元素 *\_SiteLayout*頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-247">Put calls to `Assets.GetScripts` as the very last item in the `<body>` element of the *\_SiteLayout* page.</span></span> <span data-ttu-id="e6e63-248">這有助於頁面更快載入，而且可以協助避免指令碼錯誤。</span><span class="sxs-lookup"><span data-stu-id="e6e63-248">This helps the page load faster and can help avoid script errors.</span></span>

<span data-ttu-id="e6e63-249">下列範例會顯示資產管理員如何運作。</span><span class="sxs-lookup"><span data-stu-id="e6e63-249">The following example shows how the assets manager works.</span></span> <span data-ttu-id="e6e63-250">程式碼包含下列項目：</span><span class="sxs-lookup"><span data-stu-id="e6e63-250">The code contains the following items:</span></span>

- <span data-ttu-id="e6e63-251">名為的自訂 helper `MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="e6e63-251">A custom helper named `MakeNote`.</span></span> <span data-ttu-id="e6e63-252">此 helper 來呈現的方塊內的字串換行`div`項目周圍的框線及新增具有樣式&quot;附註：&quot;它。</span><span class="sxs-lookup"><span data-stu-id="e6e63-252">This helper renders a string inside a box by wrapping a `div` element around it that's styled with a border and by adding &quot;Note:&quot; to it.</span></span> <span data-ttu-id="e6e63-253">Helper 也會呼叫的 JavaScript 檔案新增至便箋的執行階段行為。</span><span class="sxs-lookup"><span data-stu-id="e6e63-253">The helper also calls a JavaScript file that adds run-time behavior to the note.</span></span> <span data-ttu-id="e6e63-254">而不是使用指令碼的參考`<script>`標記，協助專家登錄的指令碼，藉由呼叫`Assets.AddScript`。</span><span class="sxs-lookup"><span data-stu-id="e6e63-254">Rather than reference the script with a `<script>` tag, the helper registers the script by calling `Assets.AddScript` .</span></span>
- <span data-ttu-id="e6e63-255">JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-255">A JavaScript file.</span></span> <span data-ttu-id="e6e63-256">這是檔案所呼叫的協助程式，而且會暫時增加附註項目時的字型大小`mouseover`事件。</span><span class="sxs-lookup"><span data-stu-id="e6e63-256">This is the file that's called by the helper, and it temporarily increases the font size of note items during a `mouseover` event.</span></span>
- <span data-ttu-id="e6e63-257">內容頁面上，即在參考<em>\_SiteLayout</em>  頁面上，呈現在主體中，某些內容，然後呼叫`MakeNote`協助程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-257">A content page, which references the<em>\_SiteLayout</em> page, renders some content in the body, and then calls the `MakeNote` helper.</span></span>
- <span data-ttu-id="e6e63-258">A  *\_SiteLayout*頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-258">A *\_SiteLayout* page.</span></span> <span data-ttu-id="e6e63-259">此頁面提供的通用標頭和頁面配置結構。</span><span class="sxs-lookup"><span data-stu-id="e6e63-259">This page provides a common header and a page layout structure.</span></span> <span data-ttu-id="e6e63-260">它也包含呼叫`Assets.GetScripts`，即資產管理員如何轉譯指令碼會呼叫在網頁中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-260">It also includes a call to `Assets.GetScripts`, which is how the assets manager renders script calls in a page.</span></span>

<span data-ttu-id="e6e63-261">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="e6e63-261">To run the sample:</span></span>

1. <span data-ttu-id="e6e63-262">建立空白 Web Pages 2 網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-262">Create an empty Web Pages 2 website.</span></span> <span data-ttu-id="e6e63-263">您可以使用 WebMatrix**空白網站**這個範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-263">You can use the WebMatrix **Empty Site** template for this.</span></span>
2. <span data-ttu-id="e6e63-264">建立名為的資料夾*指令碼*站台中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-264">Create a folder named *Scripts* in the site.</span></span>
3. <span data-ttu-id="e6e63-265">在*指令碼*資料夾中，建立名為*Test.js*，複製*Test.js*範例中，從內容到它，然後儲存檔案...</span><span class="sxs-lookup"><span data-stu-id="e6e63-265">In the *Scripts* folder, create a file named *Test.js*, copy the *Test.js* content into it from the example, and save the file..</span></span>
4. <span data-ttu-id="e6e63-266">建立名為的資料夾*應用程式\_程式碼*站台中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-266">Create a folder named *App\_Code* in the site.</span></span>
5. <span data-ttu-id="e6e63-267">在*應用程式\_程式碼*資料夾中，建立名為*Helpers.cshtml*、 將範例程式碼複製到其中，並將它儲存在名為的資料夾中*應用程式\_程式碼*根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-267">In the *App\_Code* folder, create a file named *Helpers.cshtml*, copy the example code into it, and save it in a folder named *App\_Code* in the root folder.</span></span>
6. <span data-ttu-id="e6e63-268">在站台的根資料夾中，建立名為 *\_SiteLayout.cshtml，*範例複製到它，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-268">In the site's root folder, create a file named *\_SiteLayout.cshtml,* copy the example into it, and save the file.</span></span>
7. <span data-ttu-id="e6e63-269">在站台根目錄中，建立名為*ContentPage.cshtml*、 加入範例程式碼，並將它儲存。</span><span class="sxs-lookup"><span data-stu-id="e6e63-269">In the site root, create a file named *ContentPage.cshtml*, add the example code, and save it.</span></span>
8. <span data-ttu-id="e6e63-270">執行*ContentPage*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="e6e63-270">Run *ContentPage* in a browser.</span></span> <span data-ttu-id="e6e63-271">您傳遞給字串`MakeNote`helper 會呈現為 boxed 的附註。</span><span class="sxs-lookup"><span data-stu-id="e6e63-271">The string you passed to the `MakeNote` helper is rendered as a boxed note.</span></span>
9. <span data-ttu-id="e6e63-272">滑鼠指標通過附註。</span><span class="sxs-lookup"><span data-stu-id="e6e63-272">Pass the mouse pointer over the note.</span></span> <span data-ttu-id="e6e63-273">指令碼會暫時增加便箋的字型大小。</span><span class="sxs-lookup"><span data-stu-id="e6e63-273">The script temporarily increases the font size of the note.</span></span>
10. <span data-ttu-id="e6e63-274">檢視所呈現頁面的來源。</span><span class="sxs-lookup"><span data-stu-id="e6e63-274">View the source of the rendered page.</span></span> <span data-ttu-id="e6e63-275">由於您放置呼叫`Assets.GetScripts`，呈現`<script>`呼叫的標記*Test.js*是網頁主體中的最後一個項目。</span><span class="sxs-lookup"><span data-stu-id="e6e63-275">Because of where you placed the call to `Assets.GetScripts`, the rendered `<script>` tag that calls *Test.js* is the very last item in the body of the page.</span></span>

<span data-ttu-id="e6e63-276">*Test.js*</span><span class="sxs-lookup"><span data-stu-id="e6e63-276">*Test.js*</span></span>

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

<span data-ttu-id="e6e63-277">*Helpers.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6e63-277">*Helpers.cshtml*</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

<span data-ttu-id="e6e63-278">*\_SiteLayout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6e63-278">*\_SiteLayout.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

<span data-ttu-id="e6e63-279">*ContentPage.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6e63-279">*ContentPage.cshtml*</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

<span data-ttu-id="e6e63-280">下列螢幕擷取畫面顯示*ContentPage.cshtml*當您透過便箋滑鼠指標在瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="e6e63-280">The following screenshot shows *ContentPage.cshtml* in a browser when you hold the mouse pointer over the note:</span></span>

<span data-ttu-id="e6e63-281">[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-281">[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)</span></span>

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a><span data-ttu-id="e6e63-282">啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入</span><span class="sxs-lookup"><span data-stu-id="e6e63-282">Enabling Logins from Facebook and Other Sites Using OAuth and OpenID</span></span>

<span data-ttu-id="e6e63-283">Web Pages 2 提供增強的成員資格和驗證的選項。</span><span class="sxs-lookup"><span data-stu-id="e6e63-283">Web Pages 2 provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="e6e63-284">主要增強功能是在新[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供者。</span><span class="sxs-lookup"><span data-stu-id="e6e63-284">The main enhancement is that there are new [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="e6e63-285">使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證從 Facebook、 Twitter、 Windows Live、 Google 和 Yahoo。</span><span class="sxs-lookup"><span data-stu-id="e6e63-285">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Windows Live, Google, and Yahoo.</span></span> <span data-ttu-id="e6e63-286">例如，使用 Facebook 帳戶進行登入，使用者就可以選擇一個 Facebook 圖示，重新導向至在其中輸入他們的使用者資訊的 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-286">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="e6e63-287">他們可將其網站上的帳戶產生關聯 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-287">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="e6e63-288">網頁的成員資格功能相關的增強功能是，使用者可將多個登入 （包括從社交網路網站的登入） 與您的網站上的單一帳戶。</span><span class="sxs-lookup"><span data-stu-id="e6e63-288">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="e6e63-289">下圖顯示登入頁面**入門網站**範本，其中使用者可以選擇要啟用外部的帳戶登入的 Facebook、 Twitter、 或 Windows Live 圖示：</span><span class="sxs-lookup"><span data-stu-id="e6e63-289">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, or Windows Live icon to enable logging in with an external account:</span></span>

<span data-ttu-id="e6e63-290">[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-290">[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)</span></span>

<span data-ttu-id="e6e63-291">您可以啟用 OAuth 和 OpenID 使用幾行程式碼的成員資格。</span><span class="sxs-lookup"><span data-stu-id="e6e63-291">You can enable OAuth and OpenID membership using just a few lines of code.</span></span> <span data-ttu-id="e6e63-292">您的方法和屬性用來處理 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。</span><span class="sxs-lookup"><span data-stu-id="e6e63-292">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span>

<span data-ttu-id="e6e63-293">不過，而不是使用程式碼來啟用登入從其他站台，若要開始使用新的提供者的建議的方式是使用新**入門網站**隨附 WebMatrix 2 beta 版的範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-293">However, instead of using code to enable logins from other sites, a recommended way to get started with the new providers is to use the new **Starter Site** template that is included with WebMatrix 2 Beta.</span></span> <span data-ttu-id="e6e63-294">**入門網站**範本包含完整的成員資格基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成與登入頁面、 成員資格資料庫和所有的程式碼.</span><span class="sxs-lookup"><span data-stu-id="e6e63-294">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a><span data-ttu-id="e6e63-295">如何啟用使用 OAuth 和 OpenID 提供者的登入</span><span class="sxs-lookup"><span data-stu-id="e6e63-295">How to Enable Logins using the OAuth and OpenID Providers</span></span>

<span data-ttu-id="e6e63-296">本節提供如何讓使用者從外部網站 （Facebook、 Twitter、 Windows Live、 Google 或 Yahoo） 登入的範例為基礎的站台**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-296">This section provides an example of how to let users log in from external sites (Facebook, Twitter, Windows Live, Google, or Yahoo) to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="e6e63-297">建立入門網站之後, 您這樣做，（詳細資料）：</span><span class="sxs-lookup"><span data-stu-id="e6e63-297">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="e6e63-298">針對使用 OAuth 提供者 （Facebook、 Twitter 和 Windows Live） 的網站，建立外部站台上的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-298">For the sites that use an OAuth provider (Facebook, Twitter, and Windows Live), create an application on the external site.</span></span> <span data-ttu-id="e6e63-299">這讓您將需要以叫用這些站台的登入功能的應用程式金鑰。</span><span class="sxs-lookup"><span data-stu-id="e6e63-299">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span> <span data-ttu-id="e6e63-300">使用 OpenID 提供者 （Google、 Yahoo） 的網站，您沒有建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-300">For sites that use an OpenID provider (Google, Yahoo), you do not have to create an application.</span></span> <span data-ttu-id="e6e63-301">針對所有這些站台，您必須有帳戶才能登入，並建立開發人員應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-301">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e6e63-302">Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-302">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="e6e63-303">若要指定適當的驗證提供者，以及提交的登入您想要使用的站台，請編輯網站中的幾個檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-303">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="e6e63-304">**若要啟用 Google 和 Yahoo 登入**:</span><span class="sxs-lookup"><span data-stu-id="e6e63-304">**To enable Google and Yahoo logins**:</span></span>

1. <span data-ttu-id="e6e63-305">在您的網站編輯 *\_AppStart.cshtml*頁面上，並在呼叫之後，新增 Razor 程式碼區塊中的下列兩行程式碼`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-305">In your website, edit the *\_AppStart.cshtml* page and add the following two lines of code in the Razor code block after the call to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e6e63-306">此程式碼可讓 Google 和 Yahoo OpenID 提供者。</span><span class="sxs-lookup"><span data-stu-id="e6e63-306">This code enables both the Google and Yahoo OpenID providers.</span></span> 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. <span data-ttu-id="e6e63-307">在*~/Account/Login.cshtml*頁面上，移除註解下列`<fieldset>`接近頁面的結束標記的區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-307">In the *~/Account/Login.cshtml* page, remove the comments from the following `<fieldset>` block of markup near the end of the page.</span></span> <span data-ttu-id="e6e63-308">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-308">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="e6e63-309">產生的程式碼區塊看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="e6e63-309">The resulting code block looks like this:</span></span>

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. <span data-ttu-id="e6e63-310">新增`<input>`Google 或 Yahoo 提供者的項目`<fieldset>`群組*~/Account/Login.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-310">Add an `<input>` element for the Google or Yahoo provider to the `<fieldset>` group in the *~/Account/Login.cshtml* page.</span></span> <span data-ttu-id="e6e63-311">已更新`<fieldset>`群組與`<input>`元素 Google 和 Yahoo 看起來類似下列的範例：</span><span class="sxs-lookup"><span data-stu-id="e6e63-311">The updated `<fieldset>` group with `<input>` elements for both Google and Yahoo looks like the following example:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. <span data-ttu-id="e6e63-312">在*~/Account/AssociateServiceAccount.cshtml*頁面上，新增`<input>`Google 或 Yahoo 至的項目`<fieldset>`群組接近檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="e6e63-312">In the *~/Account/AssociateServiceAccount.cshtml* page, add `<input>` elements for Google or Yahoo to the `<fieldset>` group near the end of the file.</span></span> <span data-ttu-id="e6e63-313">您可以複製相同`<input>`剛加入的項目`<fieldset>`一節中*~/Account/Login.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-313">You can copy the same `<input>` elements that you just added to the `<fieldset>` section in the *~/Account/Login.cshtml* page.</span></span> 

    <span data-ttu-id="e6e63-314">*~/Account/AssociateServiceAccount.cshtml*入門網站範本中的頁面可以使用，如果您想要建立的使用者可以與您的網站上的單一帳戶聯想從其他站台的多個登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-314">The *~/Account/AssociateServiceAccount.cshtml* page in the Starter Site template can be used if you want to create a page on which users can associate multiple logins from other sites with a single account on your website.</span></span>

<span data-ttu-id="e6e63-315">現在您可以測試 Google 和 Yahoo 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-315">Now you can test Google and Yahoo logins.</span></span>

1. <span data-ttu-id="e6e63-316">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-316">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="e6e63-317">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇  **Google**或**Yahoo**送出按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-317">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="e6e63-318">這個範例會使用 Google 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-318">This example uses the Google login.</span></span> 

    <span data-ttu-id="e6e63-319">網頁上將要求重新導向至 Google 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-319">The web page redirects the request to the Google login page.</span></span>

    <span data-ttu-id="e6e63-320">[![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-320">[![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)</span></span>
3. <span data-ttu-id="e6e63-321">現有的 Google 帳戶輸入認證。</span><span class="sxs-lookup"><span data-stu-id="e6e63-321">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="e6e63-322">如果 Google 會詢問您是否要允許使用來自帳戶的資訊，請按一下 Localhost**允許**。</span><span class="sxs-lookup"><span data-stu-id="e6e63-322">If Google asks you whether you want to allow Localhost to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="e6e63-323">程式碼來驗證使用者使用 Google 語彙基元，並會返回此頁面，在您的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-323">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="e6e63-324">此頁面可讓使用者將其 Google 登入與現有的帳戶，在您的網站產生關聯，或他們可以註冊新的帳戶產生關聯的外部登入您的站台上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-324">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    <span data-ttu-id="e6e63-325">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-325">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)</span></span>
5. <span data-ttu-id="e6e63-326">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-326">Choose the **Associate** button.</span></span> <span data-ttu-id="e6e63-327">瀏覽器會返回應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="e6e63-327">The browser returns to your application's home page.</span></span>

    <span data-ttu-id="e6e63-328">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-328">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)</span></span>

    <span data-ttu-id="e6e63-329">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-329">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)</span></span>

<span data-ttu-id="e6e63-330">**若要啟用 Facebook 登入**:</span><span class="sxs-lookup"><span data-stu-id="e6e63-330">**To enable Facebook logins**:</span></span>

1. <span data-ttu-id="e6e63-331">移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您未登入）。</span><span class="sxs-lookup"><span data-stu-id="e6e63-331">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="e6e63-332">選擇**建立新的應用程式**按鈕，然後再遵循提示來命名和建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-332">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="e6e63-333">一節中**選取與 Facebook 整合您的應用程式將會如何**，選擇**網站**> 一節。</span><span class="sxs-lookup"><span data-stu-id="e6e63-333">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="e6e63-334">填寫**網站 URL**欄位與您的網站 URL (例如， [ `http://www.example.com` ](http://www.example.com))。</span><span class="sxs-lookup"><span data-stu-id="e6e63-334">Fill in the **Site URL** field with the URL of your site (for example, [`http://www.example.com`](http://www.example.com)).</span></span> <span data-ttu-id="e6e63-335">**網域**欄位是選擇性的; 您可以使用此提供驗證整個網域 (例如*example.com*)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-335">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e6e63-336">如果您正在執行網站的 url 在本機電腦上要`http://localhost:12345`（其中數目是本機連接埠號碼），您可以將此值以**網站 URL**欄位以供測試您的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-336">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="e6e63-337">不過，任何時候您本機站台的變更的通訊埠編號，您將需要更新**網站 URL**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-337">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="e6e63-338">選擇**儲存變更** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-338">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="e6e63-339">選擇**應用程式**同樣地，索引標籤，然後再檢視應用程式的 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-339">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="e6e63-340">複製**應用程式識別碼**和**應用程式秘鑰**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="e6e63-340">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="e6e63-341">在網站上的程式碼中，您將會 Facebook 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="e6e63-341">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="e6e63-342">結束 Facebook 開發人員網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-342">Exit the Facebook developer site.</span></span>

<span data-ttu-id="e6e63-343">現在您變更兩個頁面中您的網站，讓使用者將能夠登入使用其 Facebook 帳戶的站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-343">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="e6e63-344">在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Facebook OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="e6e63-344">In your website, edit the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="e6e63-345">取消註解的程式碼區塊看起來如下：</span><span class="sxs-lookup"><span data-stu-id="e6e63-345">The uncommented code block looks like the following:</span></span> 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. <span data-ttu-id="e6e63-346">複製**應用程式識別碼**從 Facebook 應用程式做為值的值`consumerKey`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="e6e63-346">Copy the **App ID** value from the Facebook application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
3. <span data-ttu-id="e6e63-347">複製**應用程式秘鑰**從 Facebook 應用程式做為值`consumerSecret`參數值。</span><span class="sxs-lookup"><span data-stu-id="e6e63-347">Copy **App Secret** value from the Facebook application as the `consumerSecret` parameter value.</span></span>
4. <span data-ttu-id="e6e63-348">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-348">Save and close the file.</span></span>
5. <span data-ttu-id="e6e63-349">編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-349">Edit the *~/Account/Login.cshtml* page and remove the comments from the `<fieldset>` block near the end of the page.</span></span> <span data-ttu-id="e6e63-350">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-350">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="e6e63-351">使用註解的程式碼區塊中移除看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="e6e63-351">The code block with comments removed looks like the following:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. <span data-ttu-id="e6e63-352">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-352">Save and close the file.</span></span>

<span data-ttu-id="e6e63-353">現在您可以測試 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-353">Now you can test the Facebook login.</span></span>

1. <span data-ttu-id="e6e63-354">執行站台的*default.cshtml*頁面上，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-354">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="e6e63-355">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Facebook**圖示。</span><span class="sxs-lookup"><span data-stu-id="e6e63-355">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="e6e63-356">網頁上將要求重新導向至 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-356">The web page redirects the request to the Facebook login page.</span></span>

    <span data-ttu-id="e6e63-357">[![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-357">[![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)</span></span>
3. <span data-ttu-id="e6e63-358">登入 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="e6e63-358">Log into a Facebook account.</span></span> 

    <span data-ttu-id="e6e63-359">程式碼來驗證您使用 Facebook 語彙基元，然後傳回頁面您可以在其中將與您的網站登入您的 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-359">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="e6e63-360">使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-360">Your user name or email address is filled into the **Email** field on the form.</span></span>

    <span data-ttu-id="e6e63-361">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-361">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)</span></span>
4. <span data-ttu-id="e6e63-362">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-362">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="e6e63-363">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-363">The browser returns to the home page and you are logged in.</span></span>

    <span data-ttu-id="e6e63-364">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-364">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)</span></span>

<span data-ttu-id="e6e63-365">**若要啟用 Twitter 登入：**</span><span class="sxs-lookup"><span data-stu-id="e6e63-365">**To enable Twitter logins:**</span></span> 

1. <span data-ttu-id="e6e63-366">瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-366">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="e6e63-367">選擇**建立應用程式**連結，然後再登入網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-367">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="e6e63-368">在**建立應用程式**表單中，填寫**名稱**和**描述**欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-368">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="e6e63-369">在**網站**欄位中，輸入您網站的 URL (例如， [ `http://www.example.com` ](http://www.example.com))。</span><span class="sxs-lookup"><span data-stu-id="e6e63-369">In the **WebSite** field, enter the URL of your site (for example, [`http://www.example.com`](http://www.example.com)).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e6e63-370">如果您要測試您的網站，在本機 (使用的 URL，例如`http://localhost:12345`)，Twitter 可能無法接受 URL。</span><span class="sxs-lookup"><span data-stu-id="e6e63-370">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="e6e63-371">不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-371">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="e6e63-372">這樣可簡化測試您的應用程式在本機的程序。</span><span class="sxs-lookup"><span data-stu-id="e6e63-372">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="e6e63-373">不過，每次變更您的本機網站的通訊埠編號，您必須先更新**網站**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-373">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="e6e63-374">在**回呼 URL**欄位中，輸入您的網站，您要讓使用者在後返回記錄到 Twitter 中頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="e6e63-374">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="e6e63-375">例如，要傳送給使用者 （這會將其登入的狀態） 的入門網站的首頁，輸入您在輸入的相同 URL**網站**欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-375">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="e6e63-376">接受條款，然後選擇 [**建立應用程式 Twitter** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-376">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="e6e63-377">在**我的應用程式**登陸頁面上，選擇您所建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-377">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="e6e63-378">在**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-378">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="e6e63-379">上**詳細資料**索引標籤上，複製**取用者索引鍵**和**取用者秘密**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="e6e63-379">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="e6e63-380">在網站上的程式碼中，您將 Twitter 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="e6e63-380">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="e6e63-381">結束 Twitter 站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-381">Exit the Twitter site.</span></span>

<span data-ttu-id="e6e63-382">現在您變更兩個頁面中您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。</span><span class="sxs-lookup"><span data-stu-id="e6e63-382">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="e6e63-383">在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Twitter OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="e6e63-383">In your website, edit the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="e6e63-384">取消註解的程式碼區塊看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="e6e63-384">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. <span data-ttu-id="e6e63-385">複製**取用者索引鍵**從 Twitter 應用程式做為值的值`consumerKey`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="e6e63-385">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
3. <span data-ttu-id="e6e63-386">複製**取用者秘密**從 Twitter 應用程式做為值的值`consumerSecret`參數。</span><span class="sxs-lookup"><span data-stu-id="e6e63-386">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
4. <span data-ttu-id="e6e63-387">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-387">Save and close the file.</span></span>
5. <span data-ttu-id="e6e63-388">編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-388">Edit the *~/Account/Login.cshtml* page and remove the comments from the `<fieldset>` block near the end of the page.</span></span> <span data-ttu-id="e6e63-389">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="e6e63-389">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="e6e63-390">使用註解的程式碼區塊中移除看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="e6e63-390">The code block with comments removed looks like the following:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. <span data-ttu-id="e6e63-391">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-391">Save and close the file.</span></span>

<span data-ttu-id="e6e63-392">現在您可以測試 Twitter 登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-392">Now you can test the Twitter login.</span></span>

1. <span data-ttu-id="e6e63-393">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-393">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="e6e63-394">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Twitter**圖示。</span><span class="sxs-lookup"><span data-stu-id="e6e63-394">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="e6e63-395">網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-395">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    <span data-ttu-id="e6e63-396">[![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-396">[![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)</span></span>
3. <span data-ttu-id="e6e63-397">登入 Twitter 帳戶。</span><span class="sxs-lookup"><span data-stu-id="e6e63-397">Log into a Twitter account.</span></span>
4. <span data-ttu-id="e6e63-398">程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您的頁面您可在關聯與您網站的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-398">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="e6e63-399">您的名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-399">Your name or email address is filled into the **Email** field on the form.</span></span>

    <span data-ttu-id="e6e63-400">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-400">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)</span></span>
5. <span data-ttu-id="e6e63-401">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-401">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="e6e63-402">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="e6e63-402">The browser returns to the home page and you are logged in.</span></span>

    <span data-ttu-id="e6e63-403">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-403">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)</span></span>

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a><span data-ttu-id="e6e63-404">新增對應使用對應 Helper</span><span class="sxs-lookup"><span data-stu-id="e6e63-404">Adding Maps Using the Maps Helper</span></span>

<span data-ttu-id="e6e63-405">Web Pages 2 包括附加的 ASP.NET Web Helpers Library，這個 Web Pages 站台中的增益集的封裝。</span><span class="sxs-lookup"><span data-stu-id="e6e63-405">Web Pages 2 includes additions to the ASP.NET Web Helpers Library, which is a package of add-ins for a Web Pages site.</span></span> <span data-ttu-id="e6e63-406">下列其中一種是所提供的對應元件`Microsoft.Web.Helpers.Maps`類別。</span><span class="sxs-lookup"><span data-stu-id="e6e63-406">One of these is a mapping component provided by the `Microsoft.Web.Helpers.Maps` class.</span></span> <span data-ttu-id="e6e63-407">您可以使用`Maps`類別來產生地圖根據位址或一組經度和緯度座標。</span><span class="sxs-lookup"><span data-stu-id="e6e63-407">You can use the `Maps` class to generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="e6e63-408">`Maps`類別可讓您直接呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 熱門地圖引擎。</span><span class="sxs-lookup"><span data-stu-id="e6e63-408">The `Maps` class lets you call directly into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="e6e63-409">若要使用新`Maps`類別在您的網站，您必須先安裝了第 2 版 Web Helpers Library。</span><span class="sxs-lookup"><span data-stu-id="e6e63-409">To use the new `Maps` class in your website, you must first install the version 2 of the Web Helpers Library.</span></span> <span data-ttu-id="e6e63-410">若要這樣做，請移至安裝的目前發行的版本的指示[ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)並安裝第 2 版。</span><span class="sxs-lookup"><span data-stu-id="e6e63-410">To do this, go to the instructions for installing the currently released version of the [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) and install version 2.</span></span>

<span data-ttu-id="e6e63-411">將對應新增至網頁的步驟都相同不論哪一對應引擎呼叫。</span><span class="sxs-lookup"><span data-stu-id="e6e63-411">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="e6e63-412">您只要加入您的對應頁面的 JavaScript 檔案參考，然後再新增呈現呼叫`<script>`標記放在您的頁面上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-412">You just add a JavaScript file reference to your mapping page and then add a call that renders the `<script>` tags on your page.</span></span> <span data-ttu-id="e6e63-413">然後在您的對應頁面上，呼叫您想要使用的對應引擎。</span><span class="sxs-lookup"><span data-stu-id="e6e63-413">Then on your mapping page, call the map engine you want to use.</span></span>

<span data-ttu-id="e6e63-414">下列範例會示範如何建立會根據位址，將模式轉譯的頁面及呈現地圖經度和緯度座標為基礎的另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-414">The following example shows how to create a page that renders a map based on an address, and another page that renders a map based on longitude and latitude coordinates.</span></span> <span data-ttu-id="e6e63-415">位址對應範例會使用 Google 地圖和座標的對應範例會使用 Bing 地圖服務。</span><span class="sxs-lookup"><span data-stu-id="e6e63-415">The address mapping example uses Google Maps, and the coordinate mapping example uses Bing Maps.</span></span> <span data-ttu-id="e6e63-416">請注意程式碼中的下列元素：</span><span class="sxs-lookup"><span data-stu-id="e6e63-416">Note the following elements in the code:</span></span>

- <span data-ttu-id="e6e63-417">若要呼叫`Assets.AddScript`頂端的兩個對應頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-417">The call to `Assets.AddScript` at the top of the two mapping pages.</span></span> <span data-ttu-id="e6e63-418">這個方法會將參考加入*jquery 1.6.2.min.js*檔案所包含的**入門網站**範本，而且需要透過`Maps`類別。</span><span class="sxs-lookup"><span data-stu-id="e6e63-418">This method adds a reference to the *jquery-1.6.2.min.js* file that is included with the **Starter Site** template and that's required by the `Maps` class.</span></span>
- <span data-ttu-id="e6e63-419">若要呼叫`Assets.GetScripts`配置檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-419">The call to the `Assets.GetScripts` method in the layout file.</span></span> <span data-ttu-id="e6e63-420">這個方法會呈現`<script>`標記的兩個對應頁面。</span><span class="sxs-lookup"><span data-stu-id="e6e63-420">This method renders the `<script>` tag on the two mapping pages.</span></span>
- <span data-ttu-id="e6e63-421">若要呼叫`@Maps.GetGoogleHtml`和`@Maps.GetBingHtml`中的對應頁面的方法。</span><span class="sxs-lookup"><span data-stu-id="e6e63-421">The call to the `@Maps.GetGoogleHtml` and the `@Maps.GetBingHtml` methods in the mapping pages.</span></span> <span data-ttu-id="e6e63-422">若要對應的位址，您必須傳遞位址字串。</span><span class="sxs-lookup"><span data-stu-id="e6e63-422">To map an address, you must pass an address string.</span></span> <span data-ttu-id="e6e63-423">若要在地圖座標中，您必須傳遞經度和緯度座標。</span><span class="sxs-lookup"><span data-stu-id="e6e63-423">To map coordinates, you must pass longitude and latitude coordinates.</span></span> <span data-ttu-id="e6e63-424">Bing 地圖服務引擎，您還必須傳遞的索引鍵 (在註冊免費取得[Bing 地圖服務開發人員網站](https://www.microsoft.com/maps/developers/web.aspx))。</span><span class="sxs-lookup"><span data-stu-id="e6e63-424">For the Bing Maps engine, you must also pass a key (which you get for free by signing up at the [Bing Maps Developers site](https://www.microsoft.com/maps/developers/web.aspx)).</span></span> <span data-ttu-id="e6e63-425">其他對應引擎方法的運作方式類似 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-425">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>

<span data-ttu-id="e6e63-426">若要建立對應頁面：</span><span class="sxs-lookup"><span data-stu-id="e6e63-426">To create mapping pages:</span></span>

1. <span data-ttu-id="e6e63-427">建立網站，根據**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-427">Create a website based on the **Starter Site** template.</span></span>
2. <span data-ttu-id="e6e63-428">建立名為*MapAddress.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-428">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="e6e63-429">此頁面將會產生傳遞給它的位址為基礎的對應。</span><span class="sxs-lookup"><span data-stu-id="e6e63-429">This page will generate a map based on an address that you pass to it.</span></span>
3. <span data-ttu-id="e6e63-430">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="e6e63-430">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. <span data-ttu-id="e6e63-431">建立名為 *\_MapLayout.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-431">Create a file named *\_MapLayout.cshtml* in the root of the site.</span></span> <span data-ttu-id="e6e63-432">此頁面將兩個對應頁面的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="e6e63-432">This page will be the layout page for the two mapping pages.</span></span>
5. <span data-ttu-id="e6e63-433">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="e6e63-433">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. <span data-ttu-id="e6e63-434">建立名為*MapCoordinates.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-434">Create a file named *MapCoordinates.cshtml* in the root of the site.</span></span> <span data-ttu-id="e6e63-435">此頁面將會產生一組您傳遞給它的座標為基礎的對應。</span><span class="sxs-lookup"><span data-stu-id="e6e63-435">This page will generate a map based on a set of coordinates that you pass to it.</span></span>
7. <span data-ttu-id="e6e63-436">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="e6e63-436">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

<span data-ttu-id="e6e63-437">若要測試您的對應頁面：</span><span class="sxs-lookup"><span data-stu-id="e6e63-437">To test your mapping pages:</span></span>

1. <span data-ttu-id="e6e63-438">執行網頁*MapAddress.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="e6e63-438">Run the page *MapAddress.cshtml* file.</span></span>
2. <span data-ttu-id="e6e63-439">輸入街道地址、 狀態或市和郵遞區號，包括完整位址字串，然後選擇**Map It**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-439">Enter a full address string including a street address, state or province, and postal code, and then choose the **Map It** button.</span></span> <span data-ttu-id="e6e63-440">頁面會從 Google 地圖呈現對應：</span><span class="sxs-lookup"><span data-stu-id="e6e63-440">The page renders a map from Google Maps:</span></span> 

    <span data-ttu-id="e6e63-441">[![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-441">[![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)</span></span>
3. <span data-ttu-id="e6e63-442">尋找特定位置的緯度和經度座標的集合。</span><span class="sxs-lookup"><span data-stu-id="e6e63-442">Find a set of latitude and longitude coordinates for a specific location.</span></span>
4. <span data-ttu-id="e6e63-443">執行網頁*MapCoordinates.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e6e63-443">Run the page *MapCoordinates.cshtml*.</span></span> <span data-ttu-id="e6e63-444">輸入座標，然後選擇**Map It**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="e6e63-444">Enter the coordinates and then choose the **Map It** button.</span></span> <span data-ttu-id="e6e63-445">頁面會引導模式轉譯來自 Bing 地圖服務：</span><span class="sxs-lookup"><span data-stu-id="e6e63-445">The page renders a map from Bing Maps:</span></span> 

    <span data-ttu-id="e6e63-446">[![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-446">[![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)</span></span>

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a><span data-ttu-id="e6e63-447">執行網頁應用程式並排顯示</span><span class="sxs-lookup"><span data-stu-id="e6e63-447">Running Web Pages Applications Side by Side</span></span>

<span data-ttu-id="e6e63-448">Web Pages 2 還新增了可執行應用程式並存。</span><span class="sxs-lookup"><span data-stu-id="e6e63-448">Web Pages 2 adds the ability to run applications side by side.</span></span> <span data-ttu-id="e6e63-449">這可讓您繼續執行應用程式 Web Pages 1、 建立新的 Web Pages 2 應用程式，並在相同電腦上執行所有程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-449">This lets you continue to run your Web Pages 1 applications, build new Web Pages 2 applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="e6e63-450">以下是要記住，當您使用 WebMatrix 安裝 Web Pages 2 beta 版的一些事項：</span><span class="sxs-lookup"><span data-stu-id="e6e63-450">Here are some things to remember when you install the Web Pages 2 Beta with WebMatrix:</span></span>

- <span data-ttu-id="e6e63-451">根據預設，現有的 Web Pages 應用程式會在電腦上執行做為第 2 版應用程式。</span><span class="sxs-lookup"><span data-stu-id="e6e63-451">By default, existing Web Pages applications will run as version 2 applications on your computer.</span></span> <span data-ttu-id="e6e63-452">（第 2 版的組件安裝在 GAC 和會自動使用）。</span><span class="sxs-lookup"><span data-stu-id="e6e63-452">(The assemblies for version 2 are installed in the GAC and will be used automatically.)</span></span>
- <span data-ttu-id="e6e63-453">如果您想要執行站台使用網頁版本 1 （而不是預設值，如同先前的點），您可以設定站台執行此作業。</span><span class="sxs-lookup"><span data-stu-id="e6e63-453">If you want to run a site using Web Pages version 1 (instead of the default, as in the previous point), you can configure the site to do that.</span></span> <span data-ttu-id="e6e63-454">如果還沒有網站*web.config*檔根目錄中的站台、 建立一個新並將下列 XML 複製到其中，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="e6e63-454">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="e6e63-455">如果網站已包含*web.config* file、 add`<appSettings>`元素類似下列的其中一個來`<configuration>`> 一節。</span><span class="sxs-lookup"><span data-stu-id="e6e63-455">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  <span data-ttu-id="e6e63-456">'-如果您未指定的版本中*web.config*檔案，網站會部署為第 2 版的站台。</span><span class="sxs-lookup"><span data-stu-id="e6e63-456">\`- If you do not specify a version in the *web.config* file, a site is deployed as a version 2 site.</span></span> <span data-ttu-id="e6e63-457">(第 2 版組件會複製到*bin*資料夾中已部署的站台。)</span><span class="sxs-lookup"><span data-stu-id="e6e63-457">(The version 2 assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="e6e63-458">您使用 Web Matrix 2 Beta 包含在網站的網頁版本 2 組件的版本中的網站範本所建立的新應用程式*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e6e63-458">New applications that you create using the site templates in Web Matrix version 2 Beta include the Web Pages version 2 assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="e6e63-459">一般情況下，您一律可以控制哪個版本的網頁使用與您的網站使用 NuGet 將適當的組件安裝到站台的*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e6e63-459">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="e6e63-460">若要尋找的封裝，請瀏覽[NuGet.org](http://NuGet.org)。</span><span class="sxs-lookup"><span data-stu-id="e6e63-460">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a><span data-ttu-id="e6e63-461">行動裝置的轉譯頁面</span><span class="sxs-lookup"><span data-stu-id="e6e63-461">Rendering Pages for Mobile Devices</span></span>

<span data-ttu-id="e6e63-462">Web Pages 2 可讓您建立來呈現內容的自訂顯示行動裝置或其他裝置上。</span><span class="sxs-lookup"><span data-stu-id="e6e63-462">Web Pages 2 lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="e6e63-463">`System.Web.WebPages`命名空間包含下列類別，可讓您使用的顯示模式： `DefaultDisplayMode`， `DisplayInfo`，和`DisplayModes`。</span><span class="sxs-lookup"><span data-stu-id="e6e63-463">The `System.Web.WebPages` namespace contains the following classes that let you work with display modes: `DefaultDisplayMode`, `DisplayInfo`, and `DisplayModes`.</span></span> <span data-ttu-id="e6e63-464">您可以直接使用這些類別，並撰寫呈現特定裝置的正確輸出的程式碼。</span><span class="sxs-lookup"><span data-stu-id="e6e63-464">You can use these classes directly and write code that renders the right output for specific devices.</span></span>

<span data-ttu-id="e6e63-465">或者，您可以建立裝置的特定頁面所使用的檔案命名模式如下：<em>檔名。</em><em>行動</em><em>.cshtml</em>。</span><span class="sxs-lookup"><span data-stu-id="e6e63-465">Alternatively, you can create device-specific pages by using a file-naming pattern like this: <em>FileName.</em><em>Mobile</em><em>.cshtml</em>.</span></span> <span data-ttu-id="e6e63-466">例如，您可以建立兩個版本的頁面上，一個名為<em>MyFile.cshtml</em> ，而另一個名為<em>MyFile.Mobile.cshtml</em>。</span><span class="sxs-lookup"><span data-stu-id="e6e63-466">For example, you can create two versions of a page, one named <em>MyFile.cshtml</em> and one named <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="e6e63-467">在執行的階段，當行動裝置的要求<em>MyFile.cshtml</em>，網頁呈現內容從<em>MyFile.Mobile.cshtml</em>。</span><span class="sxs-lookup"><span data-stu-id="e6e63-467">At run time, when a mobile device requests <em>MyFile.cshtml</em>, Web Pages renders the content from <em>MyFile.Mobile.cshtml</em>.</span></span> <span data-ttu-id="e6e63-468">否則， <em>MyFile.cshtml</em>轉譯。</span><span class="sxs-lookup"><span data-stu-id="e6e63-468">Otherwise, <em>MyFile.cshtml</em> is rendered.</span></span>

<span data-ttu-id="e6e63-469">下列範例會示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。</span><span class="sxs-lookup"><span data-stu-id="e6e63-469">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="e6e63-470">*Page1.cshtml*包含內容加上導覽提要欄位。</span><span class="sxs-lookup"><span data-stu-id="e6e63-470">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="e6e63-471">*Page1.Mobile.cshtml*包含相同的內容，但會省略 [資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="e6e63-471">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

<span data-ttu-id="e6e63-472">若要建置並執行程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="e6e63-472">To build and run the code sample:</span></span>

1. <span data-ttu-id="e6e63-473">在網頁上，建立名為*Page1.cshtml*並複製*Page1.cshtml*到其中的內容範例。</span><span class="sxs-lookup"><span data-stu-id="e6e63-473">In a Web Pages site, create a file named *Page1.cshtml* and copy the *Page1.cshtml* content into it from the example.</span></span>
2. <span data-ttu-id="e6e63-474">建立名為*Page1.Mobile.cshtml*並複製*Page1.Mobile.cshtml*到其中的內容範例。</span><span class="sxs-lookup"><span data-stu-id="e6e63-474">Create a file named *Page1.Mobile.cshtml* and copy the *Page1.Mobile.cshtml* content into it from the example.</span></span> <span data-ttu-id="e6e63-475">請注意行動版的頁面會省略更好的呈現較小螢幕上的瀏覽區段。</span><span class="sxs-lookup"><span data-stu-id="e6e63-475">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>
3. <span data-ttu-id="e6e63-476">執行桌面瀏覽器並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e6e63-476">Run a desktop browser and browse to *Page1.cshtml*.</span></span>
4. <span data-ttu-id="e6e63-477">執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e6e63-477">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="e6e63-478">請注意，目前網頁呈現頁面的行動裝置的版本。</span><span class="sxs-lookup"><span data-stu-id="e6e63-478">Notice that this time Web Pages renders the mobile version of the page.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e6e63-479">若要測試行動頁面，您可以使用行動裝置模擬器，桌面的電腦上執行。</span><span class="sxs-lookup"><span data-stu-id="e6e63-479">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="e6e63-480">此工具可讓您測試網頁，因為它們會在行動裝置上看起來 （也就是通常具有較小顯示區域）。</span><span class="sxs-lookup"><span data-stu-id="e6e63-480">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="e6e63-481">模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox，這可讓您模擬 Firefox 桌面版本從各種行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e6e63-481">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<span data-ttu-id="e6e63-482">*Page1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6e63-482">*Page1.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

<span data-ttu-id="e6e63-483">*Page1.Mobile.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e6e63-483">*Page1.Mobile.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

<span data-ttu-id="e6e63-484">*Page1.cshtml*桌面瀏覽器中呈現：</span><span class="sxs-lookup"><span data-stu-id="e6e63-484">*Page1.cshtml* rendered in a desktop browser:</span></span>

<span data-ttu-id="e6e63-485">[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-485">[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)</span></span>

<span data-ttu-id="e6e63-486">*Page1.Mobile.cshtml* Apple iPhone 模擬器檢視中 Firefox 瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="e6e63-486">*Page1.Mobile.cshtml* displayed in an Apple iPhone simulator view in the Firefox browser.</span></span> <span data-ttu-id="e6e63-487">即使該要求是*Page1.cshtml*，應用程式呈現*Page1.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e6e63-487">Even though the request is to *Page1.cshtml*, the application renders *Page1.Mobile.cshtml*.</span></span>

<span data-ttu-id="e6e63-488">[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="e6e63-488">[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)</span></span>

<a id="resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="e6e63-489">其他資源</span><span class="sxs-lookup"><span data-stu-id="e6e63-489">Additional Resources</span></span>

### <a name="aspnet-web-pages-1-resources"></a><span data-ttu-id="e6e63-490">ASP.NET Web Pages 1 資源</span><span class="sxs-lookup"><span data-stu-id="e6e63-490">ASP.NET Web Pages 1 Resources</span></span>

> [!NOTE]
> <span data-ttu-id="e6e63-491">大部分的網頁 1 程式設計和應用程式開發介面資源仍然會套用到 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="e6e63-491">Most Web Pages 1 programming and API resources still apply to Web Pages 2.</span></span>

- [<span data-ttu-id="e6e63-492">ASP.NET Web Pages 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="e6e63-492">Introduction to ASP.NET Web Pages Programming</span></span>](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a><span data-ttu-id="e6e63-493">WebMatrix 資源</span><span class="sxs-lookup"><span data-stu-id="e6e63-493">WebMatrix Resources</span></span>

- [<span data-ttu-id="e6e63-494">WebMatrix 2 最新消息</span><span class="sxs-lookup"><span data-stu-id="e6e63-494">WebMatrix 2 What's New</span></span>](http://webmatrix.com/next)
- [<span data-ttu-id="e6e63-495">Microsoft WebMatrix Site</span><span class="sxs-lookup"><span data-stu-id="e6e63-495">Microsoft WebMatrix Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=195076)
- <span data-ttu-id="e6e63-496">[從 Microsoft WebMatrix Web 程式開發](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)（包括完整的範例網頁應用程式）</span><span class="sxs-lookup"><span data-stu-id="e6e63-496">[Starting Web Development with Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(includes a full-length sample Web Pages application)</span></span>
