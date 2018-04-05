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
ms.openlocfilehash: e8fc758936953970ff3e9ba289516925dee9ef45
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="the-top-features-in-aspnet-web-pages-2"></a><span data-ttu-id="0af3c-103">在 ASP.NET Web Pages 2 上的功能</span><span class="sxs-lookup"><span data-stu-id="0af3c-103">The Top Features in ASP.NET Web Pages 2</span></span>
====================
<span data-ttu-id="0af3c-104">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0af3c-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="0af3c-105">本文章提供在 ASP.NET Web Pages 2 RC 中，隨附於的輕量型 web 程式設計架構最上層的新功能的概觀[Microsoft WebMatrix 2 Rc<](https://www.microsoft.com/web/)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-105">This article provides an overview of the top new features in the ASP.NET Web Pages 2 RC, a lightweight web programming framework that is included with [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).</span></span>
> 
> <span data-ttu-id="0af3c-106">**包含的內容：**</span><span class="sxs-lookup"><span data-stu-id="0af3c-106">**What's included:**</span></span> 
> 
> - [<span data-ttu-id="0af3c-107">安裝 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0af3c-107">Installing WebMatrix</span></span>](#install)
> - [<span data-ttu-id="0af3c-108">新功能和增強功能</span><span class="sxs-lookup"><span data-stu-id="0af3c-108">New and enhanced features</span></span>](#New_and_Enhanced_Features)
> 
>     - [<span data-ttu-id="0af3c-109">RC 版本變更</span><span class="sxs-lookup"><span data-stu-id="0af3c-109">Changes for the RC release</span></span>](#Changes_for_the_RC_Version)
>     - [<span data-ttu-id="0af3c-110">測試版的變更</span><span class="sxs-lookup"><span data-stu-id="0af3c-110">Changes for the Beta release</span></span>](#Changes_for_the_Beta_Version)
>     - [<span data-ttu-id="0af3c-111">使用新的和更新的網站範本</span><span class="sxs-lookup"><span data-stu-id="0af3c-111">Using the New and Updated Site Templates</span></span>](#templates)
>     - [<span data-ttu-id="0af3c-112">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="0af3c-112">Validating User Input</span></span>](#validation)
>     - [<span data-ttu-id="0af3c-113">啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入</span><span class="sxs-lookup"><span data-stu-id="0af3c-113">Enabling logins from Facebook and other sites using OAuth and OpenID</span></span>](#oauthsetup)
>     - [<span data-ttu-id="0af3c-114">新增對應使用對應 Helper</span><span class="sxs-lookup"><span data-stu-id="0af3c-114">Adding Maps using the Maps Helper</span></span>](#maphelper)
>     - [<span data-ttu-id="0af3c-115">執行網頁應用程式並排顯示</span><span class="sxs-lookup"><span data-stu-id="0af3c-115">Running Web Pages Applications Side by Side</span></span>](#sidebyside)
>     - [<span data-ttu-id="0af3c-116">行動裝置的轉譯頁面</span><span class="sxs-lookup"><span data-stu-id="0af3c-116">Rendering Pages for Mobile Devices</span></span>](#mobile)
> - [<span data-ttu-id="0af3c-117">其他資源</span><span class="sxs-lookup"><span data-stu-id="0af3c-117">Additional Resources</span></span>](#resources)
> 
> > [!NOTE]
> > <span data-ttu-id="0af3c-118">本主題假設您使用 WebMatrix 來處理您的 ASP.NET Web Pages 2 程式碼。</span><span class="sxs-lookup"><span data-stu-id="0af3c-118">This topic assumes that you are using WebMatrix to work with your ASP.NET Web Pages 2 code.</span></span> <span data-ttu-id="0af3c-119">不過，如 Web Pages 1 中，您也可以建立使用 Visual Studio Web Pages 2 網站，可讓您強化 IntelliSense 功能，以及偵錯。</span><span class="sxs-lookup"><span data-stu-id="0af3c-119">However, as with Web Pages 1, you can also create Web Pages 2 websites using Visual Studio, which gives you enhanced IntelliSense capabilities and debugging.</span></span> <span data-ttu-id="0af3c-120">若要使用 Visual Studio 中的網頁，您必須先安裝 Visual Studio 2010 SP1、 Visual Web Developer Express 2010 SP1 或 Visual Studio 11 Beta。</span><span class="sxs-lookup"><span data-stu-id="0af3c-120">To work with Web Pages in Visual Studio, you must first install Visual Studio 2010 SP1, Visual Web Developer Express 2010 SP1, or Visual Studio 11 Beta.</span></span> <span data-ttu-id="0af3c-121">接著，安裝 ASP.NET MVC 4 Beta，其中包含用於 Visual Studio 中建立 ASP.NET MVC 4 和 Web Pages 2 應用程式範本和工具。</span><span class="sxs-lookup"><span data-stu-id="0af3c-121">Then install the ASP.NET MVC 4 Beta, which includes templates and tools for creating ASP.NET MVC 4 and Web Pages 2 applications in Visual Studio.</span></span>
> 
> 
> <span data-ttu-id="0af3c-122">*上次更新： 18 年 6 月 2012年*</span><span class="sxs-lookup"><span data-stu-id="0af3c-122">*Last update: 18 June 2012*</span></span>


<a id="install"></a>
## <a name="installing-webmatrix"></a><span data-ttu-id="0af3c-123">安裝 WebMatrix</span><span class="sxs-lookup"><span data-stu-id="0af3c-123">Installing WebMatrix</span></span>

<span data-ttu-id="0af3c-124">若要安裝 Web 網頁，您可以使用 Microsoft Web Platform Installer 中，這是免費的應用程式，可讓您輕鬆安裝及設定 web 相關的技術。</span><span class="sxs-lookup"><span data-stu-id="0af3c-124">To install Web Pages, you can use the Microsoft Web Platform Installer, which is a free application that makes it easy to install and configure web-related technologies.</span></span> <span data-ttu-id="0af3c-125">您將安裝 WebMatrix 2 beta 版，包括 Web Pages 2 Beta。</span><span class="sxs-lookup"><span data-stu-id="0af3c-125">You will install the WebMatrix 2 Beta, which includes Web Pages 2 Beta.</span></span>

1. <span data-ttu-id="0af3c-126">瀏覽至 Web Platform Installer 的最新版本的安裝頁面：</span><span class="sxs-lookup"><span data-stu-id="0af3c-126">Browse to the installation page for the latest version of the Web Platform Installer:</span></span>

    [<span data-ttu-id="0af3c-127">https://go.microsoft.com/fwlink/?LinkId=226883</span><span class="sxs-lookup"><span data-stu-id="0af3c-127">https://go.microsoft.com/fwlink/?LinkId=226883</span></span>](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > <span data-ttu-id="0af3c-128">如果您已經安裝 WebMatrix 1，這項安裝會更新它到 WebMatrix 2 Beta。</span><span class="sxs-lookup"><span data-stu-id="0af3c-128">If you already have WebMatrix 1 installed, this installation updates it to WebMatrix 2 Beta.</span></span> <span data-ttu-id="0af3c-129">您可以執行相同的電腦上使用 1 或 2 的版本所建立的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-129">You can run websites that were created using version 1 or 2 on the same computer.</span></span> <span data-ttu-id="0af3c-130">如需詳細資訊，請參閱的章節[執行網頁應用程式並存](#sidebyside)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-130">For more information, see the section on [Running Web Pages Applications Side by Side](#sidebyside).</span></span>
2. <span data-ttu-id="0af3c-131">選擇**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="0af3c-131">Choose **Install Now**.</span></span> 

    <span data-ttu-id="0af3c-132">如果您使用 Internet Explorer，請移至下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="0af3c-132">If you use Internet Explorer, go to the next step.</span></span> <span data-ttu-id="0af3c-133">如果您使用不同的瀏覽器，如 Mozilla Firefox 或 Google Chrome，提示您儲存*Webmatrix.exe*檔案儲存到電腦。</span><span class="sxs-lookup"><span data-stu-id="0af3c-133">If you use a different browser like Mozilla Firefox or Google Chrome, you are prompted to save the *Webmatrix.exe* file to your computer.</span></span> <span data-ttu-id="0af3c-134">儲存檔案，然後按一下以啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-134">Save the file and then click it to launch the installer.</span></span>
3. <span data-ttu-id="0af3c-135">執行安裝程式，並選擇**安裝** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-135">Run the installer and choose the **Install** button.</span></span> <span data-ttu-id="0af3c-136">這會安裝 WebMatrix 及網頁。</span><span class="sxs-lookup"><span data-stu-id="0af3c-136">This installs WebMatrix and Web Pages.</span></span>

## <a id="New_and_Enhanced_Features"></a><span data-ttu-id="0af3c-137">新功能和增強功能</span><span class="sxs-lookup"><span data-stu-id="0af3c-137">New and Enhanced Features</span></span>

### <a id="Changes_for_the_RC_Version"></a><span data-ttu-id="0af3c-138">RC 版本 (年 6 月 2012) 的變更</span><span class="sxs-lookup"><span data-stu-id="0af3c-138">Changes for the RC Version (June 2012)</span></span>

<span data-ttu-id="0af3c-139">2012 年 6 月的 RC 版本版有已於 2012 年 3 月發行的 Beta 版本重新整理的一些變更。</span><span class="sxs-lookup"><span data-stu-id="0af3c-139">The RC version release in June 2012 has a few changes from the Beta version refresh that was released in March 2012.</span></span> <span data-ttu-id="0af3c-140">這些變更包括：</span><span class="sxs-lookup"><span data-stu-id="0af3c-140">These changes are:</span></span>

- <span data-ttu-id="0af3c-141">A`Validation.AddFormError`方法已加入至`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-141">A `Validation.AddFormError` method was added to the `Validation` helper.</span></span> <span data-ttu-id="0af3c-142">這非常有用，如果您以手動方式執行的驗證 （例如，您驗證傳遞查詢字串中的值） 和您想要新增錯誤訊息，可以顯示`Html.ValidationSummary`方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-142">This is useful if you perform validation manually (for example, you validate a value that is passed in the query string) and you want to add an error message that can be displayed by the `Html.ValidationSummary` method.</span></span> <span data-ttu-id="0af3c-143">如需詳細資訊，請參閱下節[驗證資料，不會直接從使用者](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users)中[ASP.NET Web Pages (Razor) 網站中驗證使用者輸入](https://go.microsoft.com/fwlink/?LinkId=253002)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-143">For more information, see the section [Validating Data That Doesn't Come Directly From Users](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) in [Validating User Input in ASP.NET Web Pages (Razor) Sites](https://go.microsoft.com/fwlink/?LinkId=253002).</span></span>
- <span data-ttu-id="0af3c-144">統合及縮製的功能已經移除了核心 ASP.NET Web Pages 2 組件。</span><span class="sxs-lookup"><span data-stu-id="0af3c-144">The functionality for bundling and minification has been removed from the core ASP.NET Web Pages 2 assemblies.</span></span> <span data-ttu-id="0af3c-145">因此，`Assets`不提供本文件稍後所列的協助程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-145">As a consequence, the `Assets` helper listed later in this document is not available.</span></span> <span data-ttu-id="0af3c-146">相反地，您必須安裝[ASP.NET 最佳化](http://nuget.org/packages/Microsoft.Web.Optimization/0.1)NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="0af3c-146">Instead, you must install the [ASP.NET Optimization](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) NuGet package.</span></span> <span data-ttu-id="0af3c-147">如需詳細資訊，請參閱[組合和 ASP.NET Web Pages (Razor) 網站中的 用資產](https://go.microsoft.com/fwlink/?LinkId=255373)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-147">For more information, see [Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkId=255373).</span></span>
- <span data-ttu-id="0af3c-148">已新增支援 ASP.NET Web Pages 2 的其他組件。</span><span class="sxs-lookup"><span data-stu-id="0af3c-148">Additional assemblies to support ASP.NET Web Pages 2 have been added.</span></span> <span data-ttu-id="0af3c-149">只有明顯的影響，這項變更是您可能會看到多個組件中的站台*bin*資料夾在建立站台，或將網站部署至主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="0af3c-149">The only noticeable effect of this change is that you might see more assemblies in a site's *bin* folder after you create a site or deploy a site to a hosting provider.</span></span>

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a><span data-ttu-id="0af3c-150">Beta 版中 (2 月版 2012) 的變更</span><span class="sxs-lookup"><span data-stu-id="0af3c-150">Changes for the Beta Version (February 2012)</span></span>

<span data-ttu-id="0af3c-151">在 2012 年 2 月發行的 Beta 版有只有少數變更從 2011 年 12 月發行的 Beta 版本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-151">The Beta version released in February 2012 has only a few changes from the Beta version that was released in December 2011.</span></span> <span data-ttu-id="0af3c-152">這些變更包括：</span><span class="sxs-lookup"><span data-stu-id="0af3c-152">These changes are:</span></span>

- <span data-ttu-id="0af3c-153">Razor 現在支援條件式屬性。</span><span class="sxs-lookup"><span data-stu-id="0af3c-153">Razor now supports conditional attributes.</span></span> <span data-ttu-id="0af3c-154">HTML 項目，如果您設定屬性的值，會在伺服器程式碼中解析`false`或`null`，ASP.NET 不會完全呈現的屬性。</span><span class="sxs-lookup"><span data-stu-id="0af3c-154">In an HTML element, if you set an attribute to a value that resolves in server code to `false` or `null`, ASP.NET does not render the attribute at all.</span></span> <span data-ttu-id="0af3c-155">例如，假設您有下列核取方塊標記：</span><span class="sxs-lookup"><span data-stu-id="0af3c-155">For example, imagine you have the following markup for a check box:</span></span>

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    <span data-ttu-id="0af3c-156">如果值`checked1`解析成`false`或`null`、`checked`屬性不會轉譯。</span><span class="sxs-lookup"><span data-stu-id="0af3c-156">If the value of `checked1` resolves to `false` or to `null`, the `checked` attribute is not rendered.</span></span> <span data-ttu-id="0af3c-157">這是一項重大變更。</span><span class="sxs-lookup"><span data-stu-id="0af3c-157">This is a breaking change.</span></span>
- <span data-ttu-id="0af3c-158">`Validation.GetHtml`方法重新命名為`Validation.For`。</span><span class="sxs-lookup"><span data-stu-id="0af3c-158">The `Validation.GetHtml` method has been renamed to `Validation.For`.</span></span> <span data-ttu-id="0af3c-159">這是一項重大變更。`Validation.GetHtml`在 Beta 版中無法運作。</span><span class="sxs-lookup"><span data-stu-id="0af3c-159">This is a breaking change; `Validation.GetHtml` will not work in the Beta release.</span></span>
- <span data-ttu-id="0af3c-160">您現在可以包含`~`運算子標記中的，而不使用參考網站根目錄`Href`函式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-160">You can now include the `~` operator in markup to reference the site root without using the `Href` function.</span></span> <span data-ttu-id="0af3c-161">(亦即，Razor 剖析器現在可以找出並解決`~`運算子，而不需要的明確方法呼叫`Href`。)`Href`方法仍能運作，因此這不是一項重大變更。</span><span class="sxs-lookup"><span data-stu-id="0af3c-161">(That is, the Razor parser can now find and resolve the `~` operator without requiring an explicit method call to `Href`.) The `Href` method still works, so this is not a breaking change.</span></span>

    <span data-ttu-id="0af3c-162">例如，如果您先前已標記如下：</span><span class="sxs-lookup"><span data-stu-id="0af3c-162">For example, if you previously had markup like this:</span></span>

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    <span data-ttu-id="0af3c-163">您現在可以使用標記如下：</span><span class="sxs-lookup"><span data-stu-id="0af3c-163">You can now use markup like this:</span></span>

    `<a href="~/Default.cshtml">Home</a>`
- <span data-ttu-id="0af3c-164">`Scripts`已取代為資產 （資源） 管理的協助程式`Assets`helper，有稍微不同的方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0af3c-164">The `Scripts` helper for assets (resource) management has been replaced with the `Assets` helper, which has slightly different methods, such as the following:</span></span>

    - <span data-ttu-id="0af3c-165">如`Scripts.Add`，使用`Assets.AddScript`</span><span class="sxs-lookup"><span data-stu-id="0af3c-165">For `Scripts.Add`, use `Assets.AddScript`</span></span>
    - <span data-ttu-id="0af3c-166">如`Scripts.GetScriptTags`，使用`Assets.GetScripts`</span><span class="sxs-lookup"><span data-stu-id="0af3c-166">For `Scripts.GetScriptTags`, use `Assets.GetScripts`</span></span>

    <span data-ttu-id="0af3c-167">這是一項重大變更。`Scripts`類別不是 Beta 版中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-167">This is a breaking change; the `Scripts` class is not available in the Beta release.</span></span> <span data-ttu-id="0af3c-168">這項變更已更新此文件的程式碼範例使用資產管理。</span><span class="sxs-lookup"><span data-stu-id="0af3c-168">The code examples in this document that use asset management have been updated with this change.</span></span>

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a><span data-ttu-id="0af3c-169">使用新的和更新的網站範本</span><span class="sxs-lookup"><span data-stu-id="0af3c-169">Using the New and Updated Site Templates</span></span>

<span data-ttu-id="0af3c-170">**入門網站**範本已更新以便在 Web Pages 2 上執行，預設值。</span><span class="sxs-lookup"><span data-stu-id="0af3c-170">The **Starter Site** template has been updated so that it runs on Web Pages 2 by default.</span></span> <span data-ttu-id="0af3c-171">它也包含下列新功能：</span><span class="sxs-lookup"><span data-stu-id="0af3c-171">It also includes the following new capabilities:</span></span>

- <span data-ttu-id="0af3c-172">行動設備友善網頁呈現。</span><span class="sxs-lookup"><span data-stu-id="0af3c-172">Mobile-friendly page rendering.</span></span> <span data-ttu-id="0af3c-173">透過使用 CSS 樣式和`@media`選取器**入門網站**提供改善的呈現頁面的較小的螢幕，包括行動裝置螢幕上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-173">Through the use of CSS styles and the `@media` selector, the **Starter Site** provides improved rendering of pages on smaller screens, including mobile device screens.</span></span>
- <span data-ttu-id="0af3c-174">改良的成員資格和驗證選項。</span><span class="sxs-lookup"><span data-stu-id="0af3c-174">Improved membership and authentication options.</span></span> <span data-ttu-id="0af3c-175">您可以讓使用者登入您從其他社交網路網站，例如 Twitter、 Facebook 和 Windows Live 使用其帳戶的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-175">You can let users log into your site using their accounts from other social networking sites, such as Twitter, Facebook, and Windows Live.</span></span> <span data-ttu-id="0af3c-176">如需詳細資訊，請參閱[啟用登入的 Facebook 和其他站台使用 OAuth 和 OpenID](#oauthsetup) > 一節。</span><span class="sxs-lookup"><span data-stu-id="0af3c-176">For more information, see the [Enabling Logins from Facebook and Other Sites using OAuth and OpenID](#oauthsetup) section.</span></span>
- <span data-ttu-id="0af3c-177">HTML5 項目。</span><span class="sxs-lookup"><span data-stu-id="0af3c-177">HTML5 elements.</span></span>

<span data-ttu-id="0af3c-178">新**個人網站**範本可讓您建立包含個人部落格、 相片頁面和 Twitter 網頁的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-178">The new **Personal Site** template lets you create a website that contains a personal blog, a photos page, and a Twitter page.</span></span> <span data-ttu-id="0af3c-179">您可以自訂為基礎的站台**個人網站**範本，方法如下：</span><span class="sxs-lookup"><span data-stu-id="0af3c-179">You can customize a site based on the **Personal Site** template by doing the following:</span></span>

- <span data-ttu-id="0af3c-180">藉由編輯配置檔案中變更網站的外觀 (*\_SiteLayout.cshtml*) 和樣式檔案 (*Site.css*)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-180">Change the look of the site by editing the layout file (*\_SiteLayout.cshtml*) and the styles file (*Site.css*).</span></span>
- <span data-ttu-id="0af3c-181">安裝 NuGet 封裝會將功能加入至您的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-181">Install NuGet packages that add functionality to your site.</span></span> <span data-ttu-id="0af3c-182">如需如何安裝封裝，包括 ASP.NET Web Helpers Library，請參閱本教學課程[安裝 helper](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-182">For information about how to install packages, including the ASP.NET Web Helpers Library, see the tutorial about [installing helpers](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).</span></span>

<span data-ttu-id="0af3c-183">若要存取**個人網站**範本中，選擇**範本**上 WebMatrix**快速入門**螢幕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-183">To access the **Personal Site** template, choose **Templates** on the WebMatrix **Quick Start** screen.</span></span>

<span data-ttu-id="0af3c-184">[![topseven personalsite 1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-184">[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)</span></span>

<span data-ttu-id="0af3c-185">在**範本**對話方塊方塊中，選擇**個人網站**範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-185">In the **Templates** dialog box, choose the **Personal Site** template.</span></span>

<span data-ttu-id="0af3c-186">[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-186">[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)</span></span>

<span data-ttu-id="0af3c-187">登陸頁面**個人網站**範本可讓您設定您的部落格連結 Twitter 網頁和相片頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-187">The landing page of the **Personal Site** template lets you follow links to set up your blog, Twitter page, and photos page.</span></span>

<span data-ttu-id="0af3c-188">[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-188">[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)</span></span>

<a id="validation"></a>
### <a name="validating-user-input"></a><span data-ttu-id="0af3c-189">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="0af3c-189">Validating User Input</span></span>

<span data-ttu-id="0af3c-190">在 Web Pages 1，來驗證使用者輸入，在提交表單中，您使用`System.Web.WebPages.Html.ModelState`類別。</span><span class="sxs-lookup"><span data-stu-id="0af3c-190">In Web Pages 1, to validate user input on submitted forms, you use the `System.Web.WebPages.Html.ModelState` class.</span></span> <span data-ttu-id="0af3c-191">(說明這種情形有幾個程式碼範例在 Web Pages 1 教學課程中標題為[使用資料](../data/5-working-with-data.md)。)您仍然可以使用這種方法，在 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="0af3c-191">(This is illustrated in several of the code samples in the Web Pages 1 tutorial titled [Working with Data](../data/5-working-with-data.md).) You can still use this approach in Web Pages 2.</span></span> <span data-ttu-id="0af3c-192">不過，Web Pages 2 也提供改良的工具，驗證使用者輸入：</span><span class="sxs-lookup"><span data-stu-id="0af3c-192">However, Web Pages 2 also offers improved tools for validating user input:</span></span>

- <span data-ttu-id="0af3c-193">新的驗證類別，包括`System.Web.WebPages.ValidationHelper`和`System.Web.WebPages.Validator`，它可讓您執行幾行程式碼的功能強大的驗證工作。</span><span class="sxs-lookup"><span data-stu-id="0af3c-193">New validation classes, including `System.Web.WebPages.ValidationHelper` and `System.Web.WebPages.Validator`, that let you do powerful validation tasks with a few lines of code.</span></span>
- <span data-ttu-id="0af3c-194">（選擇性） 用戶端驗證，而不需要往返伺服器使用者，若要檢查立即回應提供驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="0af3c-194">Optionally, client-side validation, which provides immediate feedback to the user instead of requiring a round trip to the server to check for validation errors.</span></span> <span data-ttu-id="0af3c-195">（基於安全性理由，驗證伺服器上執行則即使事先在用戶端已執行檢查。）</span><span class="sxs-lookup"><span data-stu-id="0af3c-195">(For security reasons, validation is performed on the server even if the checks have been performed in the client beforehand.)</span></span>

<span data-ttu-id="0af3c-196">若要使用新的驗證功能，執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="0af3c-196">To use the new validation features, do the following:</span></span>

<span data-ttu-id="0af3c-197">在頁面的程式碼，註冊項目以進行驗證所使用的方法`Validation`helper: `Validation.RequireField`， `Validation.RequireFields` （若要註冊為必要的多個項目），或`Validation.Add`。</span><span class="sxs-lookup"><span data-stu-id="0af3c-197">In the page's code, register an element to be validated by using methods of the `Validation` helper: `Validation.RequireField`, `Validation.RequireFields` (to register multiple elements to be required), or `Validation.Add`.</span></span> <span data-ttu-id="0af3c-198">`Add`方法可讓您指定其他類型的驗證檢查，例如資料型別檢查、 比較不同的欄位，字串長度檢查中的項目，以及模式 （使用規則運算式）。</span><span class="sxs-lookup"><span data-stu-id="0af3c-198">The `Add` method lets you specify other types of validation checks, like data-type checking, comparing entries in different fields, string-length checks, and patterns (using regular expressions).</span></span> <span data-ttu-id="0af3c-199">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="0af3c-199">Here are some examples:</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

<span data-ttu-id="0af3c-200">若要顯示特定欄位的錯誤，請呼叫`Html.ValidationMessage`中驗證每個項目的標記：</span><span class="sxs-lookup"><span data-stu-id="0af3c-200">To display a field-specific error, call `Html.ValidationMessage` in the markup for each element being validated:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

<span data-ttu-id="0af3c-201">若要顯示的摘要 (`<ul>`清單) 的頁面中的所有錯誤`Html.ValidationSummary`標記中：</span><span class="sxs-lookup"><span data-stu-id="0af3c-201">To display a summary (`<ul>` list) of all the errors in the page, `Html.ValidationSummary` in the markup:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

<span data-ttu-id="0af3c-202">這些步驟便足以實作伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="0af3c-202">These steps are enough to implement server-side validation.</span></span> <span data-ttu-id="0af3c-203">如果您想要新增用戶端驗證，請執行下列此外。</span><span class="sxs-lookup"><span data-stu-id="0af3c-203">If you want to add client-side validation, do the following in addition.</span></span>

<span data-ttu-id="0af3c-204">加入下列指令碼檔參考內部`<head>`web 頁面的區段。</span><span class="sxs-lookup"><span data-stu-id="0af3c-204">Add the following script file references inside the `<head>` section of a web page.</span></span> <span data-ttu-id="0af3c-205">前兩個指令碼參考會指向內容傳遞網路 (CDN) 伺服器上的遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-205">The first two script references point to remote files on a content delivery network (CDN) server.</span></span> <span data-ttu-id="0af3c-206">第三個參考會指向本機指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-206">The third reference points to a local script file.</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

<span data-ttu-id="0af3c-207">若要取得的本機副本最簡單的方式*jquery.validate.unobtrusive.min.js*程式庫是建立新的 Web Pages 站台中，根據其中一個網站範本 （例如入門網站）。</span><span class="sxs-lookup"><span data-stu-id="0af3c-207">The easiest way to get a local copy of the *jquery.validate.unobtrusive.min.js* library is to create a new Web Pages site based on one of the site templates (such as Starter Site).</span></span> <span data-ttu-id="0af3c-208">在範本所建立的站台包含*jquery.validate.unobtrusive.js*資料夾中檔案的指令碼，從中您可以將它複製到您的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-208">The site created by the template includes *jquery.validate.unobtrusive.js* file in its Scripts folder, from which you can copy it to your site.</span></span>

<span data-ttu-id="0af3c-209">如果您的網站使用*\_SiteLayout*來控制頁面版面配置頁面上，您可以包含這些指令碼參考在該頁面，以便驗證是否能夠使用所有的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-209">If your website uses a*\_SiteLayout* page to control the page layout, you can include these script references in that page so that validation is available to all content pages.</span></span> <span data-ttu-id="0af3c-210">如果您想要執行驗證，只在特定的頁面上，您可以使用的資產管理員註冊在只有這些頁面上的指令碼。</span><span class="sxs-lookup"><span data-stu-id="0af3c-210">If you want to perform validation only on particular pages, you can use the assets manager to register the scripts on only those pages.</span></span> <span data-ttu-id="0af3c-211">若要這樣做，請呼叫`Assets.AddScript(path)`在頁面中您想要驗證，並參考每個指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-211">To do this, call `Assets.AddScript(path)` in the page that you want to validate and reference each of the script files.</span></span> <span data-ttu-id="0af3c-212">然後將呼叫加入`Assets.GetScripts`中 *\_SiteLayout*頁面呈現註冊所`<script>`標記。</span><span class="sxs-lookup"><span data-stu-id="0af3c-212">Then add a call to `Assets.GetScripts` in the *\_SiteLayout* page in order to render the registered `<script>` tags.</span></span> <span data-ttu-id="0af3c-213">如需詳細資訊，請參閱下節[與資產管理員註冊的指令碼](#resmanagement)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-213">For more information, see the section [Registering Scripts with the Assets Manager](#resmanagement).</span></span>

<span data-ttu-id="0af3c-214">在標記中的個別項目，呼叫`Validation.For`方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-214">In the markup for an individual element, call the `Validation.For` method.</span></span> <span data-ttu-id="0af3c-215">這個方法會發出屬性該 jQuery 可以攔截以便提供用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="0af3c-215">This method emits attributes that jQuery can hook in order to provide client-side validation.</span></span> <span data-ttu-id="0af3c-216">例如: </span><span class="sxs-lookup"><span data-stu-id="0af3c-216">For example:</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

<span data-ttu-id="0af3c-217">下列範例會驗證使用者輸入，在表單上的頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-217">The following example shows a page that validates user input on a form.</span></span> <span data-ttu-id="0af3c-218">若要執行測試此驗證程式碼，請執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="0af3c-218">To run and test this validation code, do this:</span></span>

1. <span data-ttu-id="0af3c-219">建立新的網站使用其中一個 WebMatrix 2 的網站範本，其中包含*指令碼*資料夾，例如**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-219">Create a new web site using one of the WebMatrix 2 site templates that includes a *Scripts* folder, such as the **Starter Site** template.</span></span>
2. <span data-ttu-id="0af3c-220">在新的站台建立新*.cshtml*頁面，然後以下列程式碼取代頁面內容。</span><span class="sxs-lookup"><span data-stu-id="0af3c-220">In the new site, create a new *.cshtml* page, and replace the contents of the page with the following code.</span></span>
3. <span data-ttu-id="0af3c-221">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-221">Run the page in a browser.</span></span> <span data-ttu-id="0af3c-222">輸入有效和無效的值，以查看上驗證的效果。</span><span class="sxs-lookup"><span data-stu-id="0af3c-222">Enter valid and invalid values to see the effects on validation.</span></span> <span data-ttu-id="0af3c-223">例如，將必要的欄位保留空白，或輸入中的字母**信用額度**欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-223">For example, leave a required field blank or enter a letter in the **Credits** field.</span></span>


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

<span data-ttu-id="0af3c-224">當使用者提交有效的輸入，以下是頁面：</span><span class="sxs-lookup"><span data-stu-id="0af3c-224">Here is the page when a user submits valid input:</span></span>

<span data-ttu-id="0af3c-225">[![topSeven 有效 1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-225">[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)</span></span>

<span data-ttu-id="0af3c-226">當使用者提交它具有必要的欄位保留空白，以下是頁面：</span><span class="sxs-lookup"><span data-stu-id="0af3c-226">Here is the page when a user submits it with a required field left empty:</span></span>

<span data-ttu-id="0af3c-227">[![topSeven 有效 2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-227">[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)</span></span>

<span data-ttu-id="0af3c-228">當使用者將它提交使用中的整數以外的項目時，以下是頁面**信用額度**欄位：</span><span class="sxs-lookup"><span data-stu-id="0af3c-228">Here is the page when a user submits it with something other than an integer in the **Credits** field:</span></span>

<span data-ttu-id="0af3c-229">[![topSeven 有效 3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-229">[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)</span></span>

<span data-ttu-id="0af3c-230">如需詳細資訊，請參閱下列部落格文章：</span><span class="sxs-lookup"><span data-stu-id="0af3c-230">For more information, see the following blog posts:</span></span>

- <span data-ttu-id="0af3c-231">[更新網頁 v2 驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344)新增驗證使用的基本概念`Validation`helper （只有伺服器端）</span><span class="sxs-lookup"><span data-stu-id="0af3c-231">[Updated validation in Web Pages v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Basics of adding validation using the `Validation` helper (server-side only)</span></span>
- <span data-ttu-id="0af3c-232">[更新網頁 v2、 第 2 部分中的驗證](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347)加入用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="0af3c-232">[Updated validation in Web Pages v2, Part 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) Adding client-side validation.</span></span>
- <span data-ttu-id="0af3c-233">[更新網頁 v2、 第 3 中的驗證](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351)格式驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="0af3c-233">[Updated validation in Web Pages v2, Part 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) Formatting validation errors.</span></span>

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a><span data-ttu-id="0af3c-234">註冊指令碼使用的資產管理員</span><span class="sxs-lookup"><span data-stu-id="0af3c-234">Registering Scripts Using the Assets Manager</span></span>

<span data-ttu-id="0af3c-235">資產管理員是新的功能，您可以使用伺服端程式碼來註冊及轉譯用戶端指令碼。</span><span class="sxs-lookup"><span data-stu-id="0af3c-235">The assets manager is a new feature that you can use in server code to register and render client scripts.</span></span> <span data-ttu-id="0af3c-236">當您正在使用中 （例如版面配置頁、 內容頁面，協助程式 」 等） 會結合成單一頁面上，在執行階段的多個檔案的程式碼時，此功能很實用。</span><span class="sxs-lookup"><span data-stu-id="0af3c-236">This feature is helpful when you are working with code from multiple files (such as layout pages, content pages, helpers, etc.) that are combined into a single page at run time.</span></span> <span data-ttu-id="0af3c-237">資產管理員協調之來源檔案，請確定指令碼檔案的參考都正確並有效率地在轉譯頁面上，不論哪一個程式碼檔案從呼叫或所呼叫的次數。</span><span class="sxs-lookup"><span data-stu-id="0af3c-237">The assets manager coordinates the source files to make sure that script files are referenced correctly and efficiently on the rendered page, regardless of which code files they are called from or how many times they are called.</span></span> <span data-ttu-id="0af3c-238">資產管理員也會呈現`<script>`標記在正確的位置，以便快速 （而不需要下載指令碼時轉譯） 可以載入的頁面和避免錯誤，可能會發生，如果在呈現之前呼叫的指令碼已完成。</span><span class="sxs-lookup"><span data-stu-id="0af3c-238">The assets manager also renders `<script>` tags in the right place so that the page can load quickly (without downloading scripts while rendering) and to avoid errors that can occur if scripts are called before rendering is complete.</span></span>

<span data-ttu-id="0af3c-239">例如，假設您建立自訂的 helper 可呼叫的 JavaScript 檔案，並於三個不同的地方呼叫此 helper 內容頁面上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-239">For example, suppose that you create a custom helper that calls a JavaScript file, and you call this helper at three different places in your content page code.</span></span> <span data-ttu-id="0af3c-240">如果您不使用的資產管理員來註冊指令碼會呼叫中的協助程式，有三個不同`<script>`所有指向相同的指令碼檔案會都出現在轉譯頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="0af3c-240">If you don't use the assets manager to register the script calls in the helper, three different `<script>` tags that all point to the same script file will appear in your rendered page.</span></span> <span data-ttu-id="0af3c-241">此外，根據 where`<script>`標記會插入在呈現的頁面、 指令碼嘗試存取頁面完全載入之前的特定頁面項目可能會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="0af3c-241">Plus, depending on where the `<script>` tags are inserted in the rendered page, errors may occur if the script tries to access certain page elements before the page fully loads.</span></span> <span data-ttu-id="0af3c-242">如果您使用的資產管理員註冊的指令碼時，會避免這些問題。</span><span class="sxs-lookup"><span data-stu-id="0af3c-242">If you use the assets manager to register the script, you avoid these problems.</span></span>

<span data-ttu-id="0af3c-243">您可以執行此動作，與資產管理員註冊指令碼：</span><span class="sxs-lookup"><span data-stu-id="0af3c-243">You can register a script with the assets manager by doing this:</span></span>

- <span data-ttu-id="0af3c-244">需要參考指令碼的程式碼，在呼叫`Assets.AddScript`方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-244">In the code that needs to reference the script, call the `Assets.AddScript` method.</span></span>
- <span data-ttu-id="0af3c-245">在 *\_SiteLayout*頁面上，呼叫`Assets.GetScripts`方法以呈現`<script>`標記。</span><span class="sxs-lookup"><span data-stu-id="0af3c-245">In a *\_SiteLayout* page, call the `Assets.GetScripts` method to render the `<script>` tags.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0af3c-246">將呼叫`Assets.GetScripts`中的最後一個項目為`<body>`元素 *\_SiteLayout*頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-246">Put calls to `Assets.GetScripts` as the very last item in the `<body>` element of the *\_SiteLayout* page.</span></span> <span data-ttu-id="0af3c-247">這有助於頁面更快載入，而且可以協助避免指令碼錯誤。</span><span class="sxs-lookup"><span data-stu-id="0af3c-247">This helps the page load faster and can help avoid script errors.</span></span>

<span data-ttu-id="0af3c-248">下列範例會顯示資產管理員如何運作。</span><span class="sxs-lookup"><span data-stu-id="0af3c-248">The following example shows how the assets manager works.</span></span> <span data-ttu-id="0af3c-249">程式碼包含下列項目：</span><span class="sxs-lookup"><span data-stu-id="0af3c-249">The code contains the following items:</span></span>

- <span data-ttu-id="0af3c-250">名為的自訂 helper `MakeNote`。</span><span class="sxs-lookup"><span data-stu-id="0af3c-250">A custom helper named `MakeNote`.</span></span> <span data-ttu-id="0af3c-251">此 helper 來呈現的方塊內的字串換行`div`項目周圍的框線及新增具有樣式&quot;附註：&quot;它。</span><span class="sxs-lookup"><span data-stu-id="0af3c-251">This helper renders a string inside a box by wrapping a `div` element around it that's styled with a border and by adding &quot;Note:&quot; to it.</span></span> <span data-ttu-id="0af3c-252">Helper 也會呼叫的 JavaScript 檔案新增至便箋的執行階段行為。</span><span class="sxs-lookup"><span data-stu-id="0af3c-252">The helper also calls a JavaScript file that adds run-time behavior to the note.</span></span> <span data-ttu-id="0af3c-253">而不是使用指令碼的參考`<script>`標記，協助專家登錄的指令碼，藉由呼叫`Assets.AddScript`。</span><span class="sxs-lookup"><span data-stu-id="0af3c-253">Rather than reference the script with a `<script>` tag, the helper registers the script by calling `Assets.AddScript` .</span></span>
- <span data-ttu-id="0af3c-254">JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-254">A JavaScript file.</span></span> <span data-ttu-id="0af3c-255">這是檔案所呼叫的協助程式，而且會暫時增加附註項目時的字型大小`mouseover`事件。</span><span class="sxs-lookup"><span data-stu-id="0af3c-255">This is the file that's called by the helper, and it temporarily increases the font size of note items during a `mouseover` event.</span></span>
- <span data-ttu-id="0af3c-256">內容頁面上，即在參考*\_SiteLayout*  頁面上，呈現在主體中，某些內容，然後呼叫`MakeNote`協助程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-256">A content page, which references the*\_SiteLayout* page, renders some content in the body, and then calls the `MakeNote` helper.</span></span>
- <span data-ttu-id="0af3c-257">A  *\_SiteLayout*頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-257">A *\_SiteLayout* page.</span></span> <span data-ttu-id="0af3c-258">此頁面提供的通用標頭和頁面配置結構。</span><span class="sxs-lookup"><span data-stu-id="0af3c-258">This page provides a common header and a page layout structure.</span></span> <span data-ttu-id="0af3c-259">它也包含呼叫`Assets.GetScripts`，即資產管理員如何轉譯指令碼會呼叫在網頁中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-259">It also includes a call to `Assets.GetScripts`, which is how the assets manager renders script calls in a page.</span></span>

<span data-ttu-id="0af3c-260">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="0af3c-260">To run the sample:</span></span>

1. <span data-ttu-id="0af3c-261">建立空白 Web Pages 2 網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-261">Create an empty Web Pages 2 website.</span></span> <span data-ttu-id="0af3c-262">您可以使用 WebMatrix**空白網站**這個範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-262">You can use the WebMatrix **Empty Site** template for this.</span></span>
2. <span data-ttu-id="0af3c-263">建立名為的資料夾*指令碼*站台中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-263">Create a folder named *Scripts* in the site.</span></span>
3. <span data-ttu-id="0af3c-264">在*指令碼*資料夾中，建立名為*Test.js*，複製*Test.js*範例中，從內容到它，然後儲存檔案...</span><span class="sxs-lookup"><span data-stu-id="0af3c-264">In the *Scripts* folder, create a file named *Test.js*, copy the *Test.js* content into it from the example, and save the file..</span></span>
4. <span data-ttu-id="0af3c-265">建立名為的資料夾*應用程式\_程式碼*站台中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-265">Create a folder named *App\_Code* in the site.</span></span>
5. <span data-ttu-id="0af3c-266">在*應用程式\_程式碼*資料夾中，建立名為*Helpers.cshtml*、 將範例程式碼複製到其中，並將它儲存在名為的資料夾中*應用程式\_程式碼*根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-266">In the *App\_Code* folder, create a file named *Helpers.cshtml*, copy the example code into it, and save it in a folder named *App\_Code* in the root folder.</span></span>
6. <span data-ttu-id="0af3c-267">在站台的根資料夾中，建立名為 *\_SiteLayout.cshtml，*範例複製到它，然後儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-267">In the site's root folder, create a file named *\_SiteLayout.cshtml,* copy the example into it, and save the file.</span></span>
7. <span data-ttu-id="0af3c-268">在站台根目錄中，建立名為*ContentPage.cshtml*、 加入範例程式碼，並將它儲存。</span><span class="sxs-lookup"><span data-stu-id="0af3c-268">In the site root, create a file named *ContentPage.cshtml*, add the example code, and save it.</span></span>
8. <span data-ttu-id="0af3c-269">執行*ContentPage*瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="0af3c-269">Run *ContentPage* in a browser.</span></span> <span data-ttu-id="0af3c-270">您傳遞給字串`MakeNote`helper 會呈現為 boxed 的附註。</span><span class="sxs-lookup"><span data-stu-id="0af3c-270">The string you passed to the `MakeNote` helper is rendered as a boxed note.</span></span>
9. <span data-ttu-id="0af3c-271">滑鼠指標通過附註。</span><span class="sxs-lookup"><span data-stu-id="0af3c-271">Pass the mouse pointer over the note.</span></span> <span data-ttu-id="0af3c-272">指令碼會暫時增加便箋的字型大小。</span><span class="sxs-lookup"><span data-stu-id="0af3c-272">The script temporarily increases the font size of the note.</span></span>
10. <span data-ttu-id="0af3c-273">檢視所呈現頁面的來源。</span><span class="sxs-lookup"><span data-stu-id="0af3c-273">View the source of the rendered page.</span></span> <span data-ttu-id="0af3c-274">由於您放置呼叫`Assets.GetScripts`，呈現`<script>`呼叫的標記*Test.js*是網頁主體中的最後一個項目。</span><span class="sxs-lookup"><span data-stu-id="0af3c-274">Because of where you placed the call to `Assets.GetScripts`, the rendered `<script>` tag that calls *Test.js* is the very last item in the body of the page.</span></span>

<span data-ttu-id="0af3c-275">*Test.js*</span><span class="sxs-lookup"><span data-stu-id="0af3c-275">*Test.js*</span></span>

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

<span data-ttu-id="0af3c-276">*Helpers.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0af3c-276">*Helpers.cshtml*</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

<span data-ttu-id="0af3c-277">*\_SiteLayout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0af3c-277">*\_SiteLayout.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

<span data-ttu-id="0af3c-278">*ContentPage.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0af3c-278">*ContentPage.cshtml*</span></span>

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

<span data-ttu-id="0af3c-279">下列螢幕擷取畫面顯示*ContentPage.cshtml*當您透過便箋滑鼠指標在瀏覽器中：</span><span class="sxs-lookup"><span data-stu-id="0af3c-279">The following screenshot shows *ContentPage.cshtml* in a browser when you hold the mouse pointer over the note:</span></span>

<span data-ttu-id="0af3c-280">[![topSeven resmgr 1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-280">[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)</span></span>

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a><span data-ttu-id="0af3c-281">啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入</span><span class="sxs-lookup"><span data-stu-id="0af3c-281">Enabling Logins from Facebook and Other Sites Using OAuth and OpenID</span></span>

<span data-ttu-id="0af3c-282">Web Pages 2 提供增強的成員資格和驗證的選項。</span><span class="sxs-lookup"><span data-stu-id="0af3c-282">Web Pages 2 provides enhanced options for membership and authentication.</span></span> <span data-ttu-id="0af3c-283">主要增強功能是在新[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供者。</span><span class="sxs-lookup"><span data-stu-id="0af3c-283">The main enhancement is that there are new [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="0af3c-284">使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證從 Facebook、 Twitter、 Windows Live、 Google 和 Yahoo。</span><span class="sxs-lookup"><span data-stu-id="0af3c-284">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Windows Live, Google, and Yahoo.</span></span> <span data-ttu-id="0af3c-285">例如，使用 Facebook 帳戶進行登入，使用者就可以選擇一個 Facebook 圖示，重新導向至在其中輸入他們的使用者資訊的 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-285">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="0af3c-286">他們可將其網站上的帳戶產生關聯 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-286">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="0af3c-287">網頁的成員資格功能相關的增強功能是，使用者可將多個登入 （包括從社交網路網站的登入） 與您的網站上的單一帳戶。</span><span class="sxs-lookup"><span data-stu-id="0af3c-287">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="0af3c-288">下圖顯示登入頁面**入門網站**範本，其中使用者可以選擇要啟用外部的帳戶登入的 Facebook、 Twitter、 或 Windows Live 圖示：</span><span class="sxs-lookup"><span data-stu-id="0af3c-288">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, or Windows Live icon to enable logging in with an external account:</span></span>

<span data-ttu-id="0af3c-289">[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-289">[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)</span></span>

<span data-ttu-id="0af3c-290">您可以啟用 OAuth 和 OpenID 使用幾行程式碼的成員資格。</span><span class="sxs-lookup"><span data-stu-id="0af3c-290">You can enable OAuth and OpenID membership using just a few lines of code.</span></span> <span data-ttu-id="0af3c-291">您的方法和屬性用來處理 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。</span><span class="sxs-lookup"><span data-stu-id="0af3c-291">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span>

<span data-ttu-id="0af3c-292">不過，而不是使用程式碼來啟用登入從其他站台，若要開始使用新的提供者的建議的方式是使用新**入門網站**隨附 WebMatrix 2 beta 版的範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-292">However, instead of using code to enable logins from other sites, a recommended way to get started with the new providers is to use the new **Starter Site** template that is included with WebMatrix 2 Beta.</span></span> <span data-ttu-id="0af3c-293">**入門網站**範本包含完整的成員資格基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成與登入頁面、 成員資格資料庫和所有的程式碼.</span><span class="sxs-lookup"><span data-stu-id="0af3c-293">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a><span data-ttu-id="0af3c-294">如何啟用使用 OAuth 和 OpenID 提供者的登入</span><span class="sxs-lookup"><span data-stu-id="0af3c-294">How to Enable Logins using the OAuth and OpenID Providers</span></span>

<span data-ttu-id="0af3c-295">本節提供如何讓使用者從外部網站 （Facebook、 Twitter、 Windows Live、 Google 或 Yahoo） 登入的範例為基礎的站台**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-295">This section provides an example of how to let users log in from external sites (Facebook, Twitter, Windows Live, Google, or Yahoo) to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="0af3c-296">建立入門網站之後, 您這樣做，（詳細資料）：</span><span class="sxs-lookup"><span data-stu-id="0af3c-296">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="0af3c-297">針對使用 OAuth 提供者 （Facebook、 Twitter 和 Windows Live） 的網站，建立外部站台上的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-297">For the sites that use an OAuth provider (Facebook, Twitter, and Windows Live), create an application on the external site.</span></span> <span data-ttu-id="0af3c-298">這讓您將需要以叫用這些站台的登入功能的應用程式金鑰。</span><span class="sxs-lookup"><span data-stu-id="0af3c-298">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span> <span data-ttu-id="0af3c-299">使用 OpenID 提供者 （Google、 Yahoo） 的網站，您沒有建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-299">For sites that use an OpenID provider (Google, Yahoo), you do not have to create an application.</span></span> <span data-ttu-id="0af3c-300">針對所有這些站台，您必須有帳戶才能登入，並建立開發人員應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-300">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0af3c-301">Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-301">Windows Live applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="0af3c-302">若要指定適當的驗證提供者，以及提交的登入您想要使用的站台，請編輯網站中的幾個檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-302">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="0af3c-303">**若要啟用 Google 和 Yahoo 登入**:</span><span class="sxs-lookup"><span data-stu-id="0af3c-303">**To enable Google and Yahoo logins**:</span></span>

1. <span data-ttu-id="0af3c-304">在您的網站編輯 *\_AppStart.cshtml*頁面上，並在呼叫之後，新增 Razor 程式碼區塊中的下列兩行程式碼`WebSecurity.InitializeDatabaseConnection`方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-304">In your website, edit the *\_AppStart.cshtml* page and add the following two lines of code in the Razor code block after the call to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="0af3c-305">此程式碼可讓 Google 和 Yahoo OpenID 提供者。</span><span class="sxs-lookup"><span data-stu-id="0af3c-305">This code enables both the Google and Yahoo OpenID providers.</span></span> 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. <span data-ttu-id="0af3c-306">在*~/Account/Login.cshtml*頁面上，移除註解下列`<fieldset>`接近頁面的結束標記的區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-306">In the *~/Account/Login.cshtml* page, remove the comments from the following `<fieldset>` block of markup near the end of the page.</span></span> <span data-ttu-id="0af3c-307">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-307">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="0af3c-308">產生的程式碼區塊看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0af3c-308">The resulting code block looks like this:</span></span>

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. <span data-ttu-id="0af3c-309">新增`<input>`Google 或 Yahoo 提供者的項目`<fieldset>`群組*~/Account/Login.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-309">Add an `<input>` element for the Google or Yahoo provider to the `<fieldset>` group in the *~/Account/Login.cshtml* page.</span></span> <span data-ttu-id="0af3c-310">已更新`<fieldset>`群組與`<input>`元素 Google 和 Yahoo 看起來類似下列的範例：</span><span class="sxs-lookup"><span data-stu-id="0af3c-310">The updated `<fieldset>` group with `<input>` elements for both Google and Yahoo looks like the following example:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. <span data-ttu-id="0af3c-311">在*~/Account/AssociateServiceAccount.cshtml*頁面上，新增`<input>`Google 或 Yahoo 至的項目`<fieldset>`群組接近檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="0af3c-311">In the *~/Account/AssociateServiceAccount.cshtml* page, add `<input>` elements for Google or Yahoo to the `<fieldset>` group near the end of the file.</span></span> <span data-ttu-id="0af3c-312">您可以複製相同`<input>`剛加入的項目`<fieldset>`一節中*~/Account/Login.cshtml*頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-312">You can copy the same `<input>` elements that you just added to the `<fieldset>` section in the *~/Account/Login.cshtml* page.</span></span> 

    <span data-ttu-id="0af3c-313">*~/Account/AssociateServiceAccount.cshtml*入門網站範本中的頁面可以使用，如果您想要建立的使用者可以與您的網站上的單一帳戶聯想從其他站台的多個登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-313">The *~/Account/AssociateServiceAccount.cshtml* page in the Starter Site template can be used if you want to create a page on which users can associate multiple logins from other sites with a single account on your website.</span></span>

<span data-ttu-id="0af3c-314">現在您可以測試 Google 和 Yahoo 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-314">Now you can test Google and Yahoo logins.</span></span>

1. <span data-ttu-id="0af3c-315">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-315">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="0af3c-316">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇  **Google**或**Yahoo**送出按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-316">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="0af3c-317">這個範例會使用 Google 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-317">This example uses the Google login.</span></span> 

    <span data-ttu-id="0af3c-318">網頁上將要求重新導向至 Google 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-318">The web page redirects the request to the Google login page.</span></span>

    <span data-ttu-id="0af3c-319">[![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-319">[![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)</span></span>
3. <span data-ttu-id="0af3c-320">現有的 Google 帳戶輸入認證。</span><span class="sxs-lookup"><span data-stu-id="0af3c-320">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="0af3c-321">如果 Google 會詢問您是否要允許使用來自帳戶的資訊，請按一下 Localhost**允許**。</span><span class="sxs-lookup"><span data-stu-id="0af3c-321">If Google asks you whether you want to allow Localhost to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="0af3c-322">程式碼來驗證使用者使用 Google 語彙基元，並會返回此頁面，在您的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-322">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="0af3c-323">此頁面可讓使用者將其 Google 登入與現有的帳戶，在您的網站產生關聯，或他們可以註冊新的帳戶產生關聯的外部登入您的站台上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-323">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    <span data-ttu-id="0af3c-324">[![topSeven oauth 5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-324">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)</span></span>
5. <span data-ttu-id="0af3c-325">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-325">Choose the **Associate** button.</span></span> <span data-ttu-id="0af3c-326">瀏覽器會返回應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="0af3c-326">The browser returns to your application's home page.</span></span>

    <span data-ttu-id="0af3c-327">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-327">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)</span></span>

    <span data-ttu-id="0af3c-328">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-328">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)</span></span>

<span data-ttu-id="0af3c-329">**若要啟用 Facebook 登入**:</span><span class="sxs-lookup"><span data-stu-id="0af3c-329">**To enable Facebook logins**:</span></span>

1. <span data-ttu-id="0af3c-330">移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您未登入）。</span><span class="sxs-lookup"><span data-stu-id="0af3c-330">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="0af3c-331">選擇**建立新的應用程式**按鈕，然後再遵循提示來命名和建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-331">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="0af3c-332">一節中**選取與 Facebook 整合您的應用程式將會如何**，選擇**網站**> 一節。</span><span class="sxs-lookup"><span data-stu-id="0af3c-332">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="0af3c-333">填寫**網站 URL**欄位與您的網站 URL (例如， [ `http://www.example.com` ](http://www.example.com))。</span><span class="sxs-lookup"><span data-stu-id="0af3c-333">Fill in the **Site URL** field with the URL of your site (for example, [`http://www.example.com`](http://www.example.com)).</span></span> <span data-ttu-id="0af3c-334">**網域**欄位是選擇性的; 您可以使用此提供驗證整個網域 (例如*example.com*)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-334">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0af3c-335">如果您正在執行網站的 url 在本機電腦上要`http://localhost:12345`（其中數目是本機連接埠號碼），您可以將此值以**網站 URL**欄位以供測試您的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-335">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="0af3c-336">不過，任何時候您本機站台的變更的通訊埠編號，您將需要更新**網站 URL**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-336">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="0af3c-337">選擇**儲存變更** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-337">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="0af3c-338">選擇**應用程式**同樣地，索引標籤，然後再檢視應用程式的 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-338">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="0af3c-339">複製**應用程式識別碼**和**應用程式秘鑰**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="0af3c-339">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="0af3c-340">在網站上的程式碼中，您將會 Facebook 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="0af3c-340">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="0af3c-341">結束 Facebook 開發人員網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-341">Exit the Facebook developer site.</span></span>

<span data-ttu-id="0af3c-342">現在您變更兩個頁面中您的網站，讓使用者將能夠登入使用其 Facebook 帳戶的站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-342">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="0af3c-343">在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Facebook OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="0af3c-343">In your website, edit the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="0af3c-344">取消註解的程式碼區塊看起來如下：</span><span class="sxs-lookup"><span data-stu-id="0af3c-344">The uncommented code block looks like the following:</span></span> 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. <span data-ttu-id="0af3c-345">複製**應用程式識別碼**從 Facebook 應用程式做為值的值`consumerKey`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="0af3c-345">Copy the **App ID** value from the Facebook application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
3. <span data-ttu-id="0af3c-346">複製**應用程式秘鑰**從 Facebook 應用程式做為值`consumerSecret`參數值。</span><span class="sxs-lookup"><span data-stu-id="0af3c-346">Copy **App Secret** value from the Facebook application as the `consumerSecret` parameter value.</span></span>
4. <span data-ttu-id="0af3c-347">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-347">Save and close the file.</span></span>
5. <span data-ttu-id="0af3c-348">編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-348">Edit the *~/Account/Login.cshtml* page and remove the comments from the `<fieldset>` block near the end of the page.</span></span> <span data-ttu-id="0af3c-349">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-349">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="0af3c-350">使用註解的程式碼區塊中移除看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="0af3c-350">The code block with comments removed looks like the following:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. <span data-ttu-id="0af3c-351">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-351">Save and close the file.</span></span>

<span data-ttu-id="0af3c-352">現在您可以測試 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-352">Now you can test the Facebook login.</span></span>

1. <span data-ttu-id="0af3c-353">執行站台的*default.cshtml*頁面上，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-353">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="0af3c-354">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Facebook**圖示。</span><span class="sxs-lookup"><span data-stu-id="0af3c-354">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="0af3c-355">網頁上將要求重新導向至 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-355">The web page redirects the request to the Facebook login page.</span></span>

    <span data-ttu-id="0af3c-356">[![topSeven oauth 2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-356">[![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)</span></span>
3. <span data-ttu-id="0af3c-357">登入 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="0af3c-357">Log into a Facebook account.</span></span> 

    <span data-ttu-id="0af3c-358">程式碼來驗證您使用 Facebook 語彙基元，然後傳回頁面您可以在其中將與您的網站登入您的 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-358">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="0af3c-359">使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-359">Your user name or email address is filled into the **Email** field on the form.</span></span>

    <span data-ttu-id="0af3c-360">[![topSeven oauth 5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-360">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)</span></span>
4. <span data-ttu-id="0af3c-361">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-361">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="0af3c-362">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-362">The browser returns to the home page and you are logged in.</span></span>

    <span data-ttu-id="0af3c-363">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-363">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)</span></span>

<span data-ttu-id="0af3c-364">**若要啟用 Twitter 登入：**</span><span class="sxs-lookup"><span data-stu-id="0af3c-364">**To enable Twitter logins:**</span></span> 

1. <span data-ttu-id="0af3c-365">瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-365">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="0af3c-366">選擇**建立應用程式**連結，然後再登入網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-366">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="0af3c-367">在**建立應用程式**表單中，填寫**名稱**和**描述**欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-367">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="0af3c-368">在**網站**欄位中，輸入您網站的 URL (例如， [ `http://www.example.com` ](http://www.example.com))。</span><span class="sxs-lookup"><span data-stu-id="0af3c-368">In the **WebSite** field, enter the URL of your site (for example, [`http://www.example.com`](http://www.example.com)).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0af3c-369">如果您要測試您的網站，在本機 (使用的 URL，例如`http://localhost:12345`)，Twitter 可能無法接受 URL。</span><span class="sxs-lookup"><span data-stu-id="0af3c-369">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="0af3c-370">不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-370">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="0af3c-371">這樣可簡化測試您的應用程式在本機的程序。</span><span class="sxs-lookup"><span data-stu-id="0af3c-371">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="0af3c-372">不過，每次變更您的本機網站的通訊埠編號，您必須先更新**網站**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-372">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="0af3c-373">在**回呼 URL**欄位中，輸入您的網站，您要讓使用者在後返回記錄到 Twitter 中頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="0af3c-373">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="0af3c-374">例如，要傳送給使用者 （這會將其登入的狀態） 的入門網站的首頁，輸入您在輸入的相同 URL**網站**欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-374">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="0af3c-375">接受條款，然後選擇 [**建立應用程式 Twitter** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-375">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="0af3c-376">在**我的應用程式**登陸頁面上，選擇您所建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-376">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="0af3c-377">在**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-377">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="0af3c-378">上**詳細資料**索引標籤上，複製**取用者索引鍵**和**取用者秘密**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="0af3c-378">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="0af3c-379">在網站上的程式碼中，您將 Twitter 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="0af3c-379">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="0af3c-380">結束 Twitter 站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-380">Exit the Twitter site.</span></span>

<span data-ttu-id="0af3c-381">現在您變更兩個頁面中您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。</span><span class="sxs-lookup"><span data-stu-id="0af3c-381">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="0af3c-382">在您的網站編輯 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Twitter OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="0af3c-382">In your website, edit the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="0af3c-383">取消註解的程式碼區塊看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="0af3c-383">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. <span data-ttu-id="0af3c-384">複製**取用者索引鍵**從 Twitter 應用程式做為值的值`consumerKey`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="0af3c-384">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
3. <span data-ttu-id="0af3c-385">複製**取用者秘密**從 Twitter 應用程式做為值的值`consumerSecret`參數。</span><span class="sxs-lookup"><span data-stu-id="0af3c-385">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
4. <span data-ttu-id="0af3c-386">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-386">Save and close the file.</span></span>
5. <span data-ttu-id="0af3c-387">編輯*~/Account/Login.cshtml*頁面上，並移除從註解`<fieldset>`接近頁面結尾區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-387">Edit the *~/Account/Login.cshtml* page and remove the comments from the `<fieldset>` block near the end of the page.</span></span> <span data-ttu-id="0af3c-388">若要取消註解程式碼，移除`@*`字元在的`<fieldset>`區塊。</span><span class="sxs-lookup"><span data-stu-id="0af3c-388">To uncomment the code, remove the `@*` characters that precede and follow the `<fieldset>` block.</span></span> <span data-ttu-id="0af3c-389">使用註解的程式碼區塊中移除看起來如下所示：</span><span class="sxs-lookup"><span data-stu-id="0af3c-389">The code block with comments removed looks like the following:</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. <span data-ttu-id="0af3c-390">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-390">Save and close the file.</span></span>

<span data-ttu-id="0af3c-391">現在您可以測試 Twitter 登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-391">Now you can test the Twitter login.</span></span>

1. <span data-ttu-id="0af3c-392">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-392">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="0af3c-393">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Twitter**圖示。</span><span class="sxs-lookup"><span data-stu-id="0af3c-393">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="0af3c-394">網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-394">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    <span data-ttu-id="0af3c-395">[![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-395">[![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)</span></span>
3. <span data-ttu-id="0af3c-396">登入 Twitter 帳戶。</span><span class="sxs-lookup"><span data-stu-id="0af3c-396">Log into a Twitter account.</span></span>
4. <span data-ttu-id="0af3c-397">程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您的頁面您可在關聯與您網站的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-397">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="0af3c-398">您的名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-398">Your name or email address is filled into the **Email** field on the form.</span></span>

    <span data-ttu-id="0af3c-399">[![topSeven oauth 5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-399">[![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)</span></span>
5. <span data-ttu-id="0af3c-400">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-400">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="0af3c-401">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="0af3c-401">The browser returns to the home page and you are logged in.</span></span>

    <span data-ttu-id="0af3c-402">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-402">[![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)</span></span>

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a><span data-ttu-id="0af3c-403">新增對應使用對應 Helper</span><span class="sxs-lookup"><span data-stu-id="0af3c-403">Adding Maps Using the Maps Helper</span></span>

<span data-ttu-id="0af3c-404">Web Pages 2 包括附加的 ASP.NET Web Helpers Library，這個 Web Pages 站台中的增益集的封裝。</span><span class="sxs-lookup"><span data-stu-id="0af3c-404">Web Pages 2 includes additions to the ASP.NET Web Helpers Library, which is a package of add-ins for a Web Pages site.</span></span> <span data-ttu-id="0af3c-405">下列其中一種是所提供的對應元件`Microsoft.Web.Helpers.Maps`類別。</span><span class="sxs-lookup"><span data-stu-id="0af3c-405">One of these is a mapping component provided by the `Microsoft.Web.Helpers.Maps` class.</span></span> <span data-ttu-id="0af3c-406">您可以使用`Maps`類別來產生地圖根據位址或一組經度和緯度座標。</span><span class="sxs-lookup"><span data-stu-id="0af3c-406">You can use the `Maps` class to generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="0af3c-407">`Maps`類別可讓您直接呼叫包括 Bing、 Google、 MapQuest 和 Yahoo 熱門地圖引擎。</span><span class="sxs-lookup"><span data-stu-id="0af3c-407">The `Maps` class lets you call directly into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="0af3c-408">若要使用新`Maps`類別在您的網站，您必須先安裝了第 2 版 Web Helpers Library。</span><span class="sxs-lookup"><span data-stu-id="0af3c-408">To use the new `Maps` class in your website, you must first install the version 2 of the Web Helpers Library.</span></span> <span data-ttu-id="0af3c-409">若要這樣做，請移至安裝的目前發行的版本的指示[ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers)並安裝第 2 版。</span><span class="sxs-lookup"><span data-stu-id="0af3c-409">To do this, go to the instructions for installing the currently released version of the [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) and install version 2.</span></span>

<span data-ttu-id="0af3c-410">將對應新增至網頁的步驟都相同不論哪一對應引擎呼叫。</span><span class="sxs-lookup"><span data-stu-id="0af3c-410">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="0af3c-411">您只要加入您的對應頁面的 JavaScript 檔案參考，然後再新增呈現呼叫`<script>`標記放在您的頁面上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-411">You just add a JavaScript file reference to your mapping page and then add a call that renders the `<script>` tags on your page.</span></span> <span data-ttu-id="0af3c-412">然後在您的對應頁面上，呼叫您想要使用的對應引擎。</span><span class="sxs-lookup"><span data-stu-id="0af3c-412">Then on your mapping page, call the map engine you want to use.</span></span>

<span data-ttu-id="0af3c-413">下列範例會示範如何建立會根據位址，將模式轉譯的頁面及呈現地圖經度和緯度座標為基礎的另一個頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-413">The following example shows how to create a page that renders a map based on an address, and another page that renders a map based on longitude and latitude coordinates.</span></span> <span data-ttu-id="0af3c-414">位址對應範例會使用 Google 地圖和座標的對應範例會使用 Bing 地圖服務。</span><span class="sxs-lookup"><span data-stu-id="0af3c-414">The address mapping example uses Google Maps, and the coordinate mapping example uses Bing Maps.</span></span> <span data-ttu-id="0af3c-415">請注意程式碼中的下列元素：</span><span class="sxs-lookup"><span data-stu-id="0af3c-415">Note the following elements in the code:</span></span>

- <span data-ttu-id="0af3c-416">若要呼叫`Assets.AddScript`頂端的兩個對應頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-416">The call to `Assets.AddScript` at the top of the two mapping pages.</span></span> <span data-ttu-id="0af3c-417">這個方法會將參考加入*jquery 1.6.2.min.js*檔案所包含的**入門網站**範本，而且需要透過`Maps`類別。</span><span class="sxs-lookup"><span data-stu-id="0af3c-417">This method adds a reference to the *jquery-1.6.2.min.js* file that is included with the **Starter Site** template and that's required by the `Maps` class.</span></span>
- <span data-ttu-id="0af3c-418">若要呼叫`Assets.GetScripts`配置檔案中的方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-418">The call to the `Assets.GetScripts` method in the layout file.</span></span> <span data-ttu-id="0af3c-419">這個方法會呈現`<script>`標記的兩個對應頁面。</span><span class="sxs-lookup"><span data-stu-id="0af3c-419">This method renders the `<script>` tag on the two mapping pages.</span></span>
- <span data-ttu-id="0af3c-420">若要呼叫`@Maps.GetGoogleHtml`和`@Maps.GetBingHtml`中的對應頁面的方法。</span><span class="sxs-lookup"><span data-stu-id="0af3c-420">The call to the `@Maps.GetGoogleHtml` and the `@Maps.GetBingHtml` methods in the mapping pages.</span></span> <span data-ttu-id="0af3c-421">若要對應的位址，您必須傳遞位址字串。</span><span class="sxs-lookup"><span data-stu-id="0af3c-421">To map an address, you must pass an address string.</span></span> <span data-ttu-id="0af3c-422">若要在地圖座標中，您必須傳遞經度和緯度座標。</span><span class="sxs-lookup"><span data-stu-id="0af3c-422">To map coordinates, you must pass longitude and latitude coordinates.</span></span> <span data-ttu-id="0af3c-423">Bing 地圖服務引擎，您還必須傳遞的索引鍵 (在註冊免費取得[Bing 地圖服務開發人員網站](https://www.microsoft.com/maps/developers/web.aspx))。</span><span class="sxs-lookup"><span data-stu-id="0af3c-423">For the Bing Maps engine, you must also pass a key (which you get for free by signing up at the [Bing Maps Developers site](https://www.microsoft.com/maps/developers/web.aspx)).</span></span> <span data-ttu-id="0af3c-424">其他對應引擎方法的運作方式類似 (`@Maps.GetYahooHtml`， `@Maps.GetMapQuestHtml`)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-424">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>

<span data-ttu-id="0af3c-425">若要建立對應頁面：</span><span class="sxs-lookup"><span data-stu-id="0af3c-425">To create mapping pages:</span></span>

1. <span data-ttu-id="0af3c-426">建立網站，根據**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-426">Create a website based on the **Starter Site** template.</span></span>
2. <span data-ttu-id="0af3c-427">建立名為*MapAddress.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-427">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="0af3c-428">此頁面將會產生傳遞給它的位址為基礎的對應。</span><span class="sxs-lookup"><span data-stu-id="0af3c-428">This page will generate a map based on an address that you pass to it.</span></span>
3. <span data-ttu-id="0af3c-429">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="0af3c-429">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. <span data-ttu-id="0af3c-430">建立名為 *\_MapLayout.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-430">Create a file named *\_MapLayout.cshtml* in the root of the site.</span></span> <span data-ttu-id="0af3c-431">此頁面將兩個對應頁面的版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="0af3c-431">This page will be the layout page for the two mapping pages.</span></span>
5. <span data-ttu-id="0af3c-432">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="0af3c-432">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. <span data-ttu-id="0af3c-433">建立名為*MapCoordinates.cshtml*根目錄中的站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-433">Create a file named *MapCoordinates.cshtml* in the root of the site.</span></span> <span data-ttu-id="0af3c-434">此頁面將會產生一組您傳遞給它的座標為基礎的對應。</span><span class="sxs-lookup"><span data-stu-id="0af3c-434">This page will generate a map based on a set of coordinates that you pass to it.</span></span>
7. <span data-ttu-id="0af3c-435">將下列程式碼複製到檔案，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="0af3c-435">Copy the following code into the file, overwriting the existing content.</span></span> 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

<span data-ttu-id="0af3c-436">若要測試您的對應頁面：</span><span class="sxs-lookup"><span data-stu-id="0af3c-436">To test your mapping pages:</span></span>

1. <span data-ttu-id="0af3c-437">執行網頁*MapAddress.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="0af3c-437">Run the page *MapAddress.cshtml* file.</span></span>
2. <span data-ttu-id="0af3c-438">輸入街道地址、 狀態或市和郵遞區號，包括完整位址字串，然後選擇**Map It**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-438">Enter a full address string including a street address, state or province, and postal code, and then choose the **Map It** button.</span></span> <span data-ttu-id="0af3c-439">頁面會從 Google 地圖呈現對應：</span><span class="sxs-lookup"><span data-stu-id="0af3c-439">The page renders a map from Google Maps:</span></span> 

    <span data-ttu-id="0af3c-440">[![topseven maphelper 1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-440">[![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)</span></span>
3. <span data-ttu-id="0af3c-441">尋找特定位置的緯度和經度座標的集合。</span><span class="sxs-lookup"><span data-stu-id="0af3c-441">Find a set of latitude and longitude coordinates for a specific location.</span></span>
4. <span data-ttu-id="0af3c-442">執行網頁*MapCoordinates.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-442">Run the page *MapCoordinates.cshtml*.</span></span> <span data-ttu-id="0af3c-443">輸入座標，然後選擇**Map It**  按鈕。</span><span class="sxs-lookup"><span data-stu-id="0af3c-443">Enter the coordinates and then choose the **Map It** button.</span></span> <span data-ttu-id="0af3c-444">頁面會引導模式轉譯來自 Bing 地圖服務：</span><span class="sxs-lookup"><span data-stu-id="0af3c-444">The page renders a map from Bing Maps:</span></span> 

    <span data-ttu-id="0af3c-445">[![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-445">[![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)</span></span>

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a><span data-ttu-id="0af3c-446">執行網頁應用程式並排顯示</span><span class="sxs-lookup"><span data-stu-id="0af3c-446">Running Web Pages Applications Side by Side</span></span>

<span data-ttu-id="0af3c-447">Web Pages 2 還新增了可執行應用程式並存。</span><span class="sxs-lookup"><span data-stu-id="0af3c-447">Web Pages 2 adds the ability to run applications side by side.</span></span> <span data-ttu-id="0af3c-448">這可讓您繼續執行應用程式 Web Pages 1、 建立新的 Web Pages 2 應用程式，並在相同電腦上執行所有程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-448">This lets you continue to run your Web Pages 1 applications, build new Web Pages 2 applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="0af3c-449">以下是要記住，當您使用 WebMatrix 安裝 Web Pages 2 beta 版的一些事項：</span><span class="sxs-lookup"><span data-stu-id="0af3c-449">Here are some things to remember when you install the Web Pages 2 Beta with WebMatrix:</span></span>

- <span data-ttu-id="0af3c-450">根據預設，現有的 Web Pages 應用程式會在電腦上執行做為第 2 版應用程式。</span><span class="sxs-lookup"><span data-stu-id="0af3c-450">By default, existing Web Pages applications will run as version 2 applications on your computer.</span></span> <span data-ttu-id="0af3c-451">（第 2 版的組件安裝在 GAC 和會自動使用）。</span><span class="sxs-lookup"><span data-stu-id="0af3c-451">(The assemblies for version 2 are installed in the GAC and will be used automatically.)</span></span>
- <span data-ttu-id="0af3c-452">如果您想要執行站台使用網頁版本 1 （而不是預設值，如同先前的點），您可以設定站台執行此作業。</span><span class="sxs-lookup"><span data-stu-id="0af3c-452">If you want to run a site using Web Pages version 1 (instead of the default, as in the previous point), you can configure the site to do that.</span></span> <span data-ttu-id="0af3c-453">如果還沒有網站*web.config*檔根目錄中的站台、 建立一個新並將下列 XML 複製到其中，覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="0af3c-453">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="0af3c-454">如果網站已包含*web.config* file、 add`<appSettings>`元素類似下列的其中一個來`<configuration>`> 一節。</span><span class="sxs-lookup"><span data-stu-id="0af3c-454">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
<span data-ttu-id="0af3c-455">'-如果您未指定的版本中*web.config*檔案，網站會部署為第 2 版的站台。</span><span class="sxs-lookup"><span data-stu-id="0af3c-455">\`- If you do not specify a version in the *web.config* file, a site is deployed as a version 2 site.</span></span> <span data-ttu-id="0af3c-456">(第 2 版組件會複製到*bin*資料夾中已部署的站台。)</span><span class="sxs-lookup"><span data-stu-id="0af3c-456">(The version 2 assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="0af3c-457">您使用 Web Matrix 2 Beta 包含在網站的網頁版本 2 組件的版本中的網站範本所建立的新應用程式*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="0af3c-457">New applications that you create using the site templates in Web Matrix version 2 Beta include the Web Pages version 2 assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="0af3c-458">一般情況下，您一律可以控制哪個版本的網頁使用與您的網站使用 NuGet 將適當的組件安裝到站台的*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="0af3c-458">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="0af3c-459">若要尋找的封裝，請瀏覽[NuGet.org](http://NuGet.org)。</span><span class="sxs-lookup"><span data-stu-id="0af3c-459">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a><span data-ttu-id="0af3c-460">行動裝置的轉譯頁面</span><span class="sxs-lookup"><span data-stu-id="0af3c-460">Rendering Pages for Mobile Devices</span></span>

<span data-ttu-id="0af3c-461">Web Pages 2 可讓您建立來呈現內容的自訂顯示行動裝置或其他裝置上。</span><span class="sxs-lookup"><span data-stu-id="0af3c-461">Web Pages 2 lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="0af3c-462">`System.Web.WebPages`命名空間包含下列類別，可讓您使用的顯示模式： `DefaultDisplayMode`， `DisplayInfo`，和`DisplayModes`。</span><span class="sxs-lookup"><span data-stu-id="0af3c-462">The `System.Web.WebPages` namespace contains the following classes that let you work with display modes: `DefaultDisplayMode`, `DisplayInfo`, and `DisplayModes`.</span></span> <span data-ttu-id="0af3c-463">您可以直接使用這些類別，並撰寫呈現特定裝置的正確輸出的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0af3c-463">You can use these classes directly and write code that renders the right output for specific devices.</span></span>

<span data-ttu-id="0af3c-464">或者，您可以建立裝置的特定頁面所使用的檔案命名模式如下：*檔名。**行動**.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-464">Alternatively, you can create device-specific pages by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="0af3c-465">例如，您可以建立兩個版本的頁面上，一個名為*MyFile.cshtml* ，而另一個名為*MyFile.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-465">For example, you can create two versions of a page, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="0af3c-466">在執行的階段，當行動裝置的要求*MyFile.cshtml*，網頁呈現內容從*MyFile.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-466">At run time, when a mobile device requests *MyFile.cshtml*, Web Pages renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="0af3c-467">否則， *MyFile.cshtml*轉譯。</span><span class="sxs-lookup"><span data-stu-id="0af3c-467">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="0af3c-468">下列範例會示範如何藉由新增行動裝置的內容頁面中啟用行動裝置的轉譯。</span><span class="sxs-lookup"><span data-stu-id="0af3c-468">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="0af3c-469">*Page1.cshtml*包含內容加上導覽提要欄位。</span><span class="sxs-lookup"><span data-stu-id="0af3c-469">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="0af3c-470">*Page1.Mobile.cshtml*包含相同的內容，但會省略 [資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="0af3c-470">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

<span data-ttu-id="0af3c-471">若要建置並執行程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="0af3c-471">To build and run the code sample:</span></span>

1. <span data-ttu-id="0af3c-472">在網頁上，建立名為*Page1.cshtml*並複製*Page1.cshtml*到其中的內容範例。</span><span class="sxs-lookup"><span data-stu-id="0af3c-472">In a Web Pages site, create a file named *Page1.cshtml* and copy the *Page1.cshtml* content into it from the example.</span></span>
2. <span data-ttu-id="0af3c-473">建立名為*Page1.Mobile.cshtml*並複製*Page1.Mobile.cshtml*到其中的內容範例。</span><span class="sxs-lookup"><span data-stu-id="0af3c-473">Create a file named *Page1.Mobile.cshtml* and copy the *Page1.Mobile.cshtml* content into it from the example.</span></span> <span data-ttu-id="0af3c-474">請注意行動版的頁面會省略更好的呈現較小螢幕上的瀏覽區段。</span><span class="sxs-lookup"><span data-stu-id="0af3c-474">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>
3. <span data-ttu-id="0af3c-475">執行桌面瀏覽器並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-475">Run a desktop browser and browse to *Page1.cshtml*.</span></span>
4. <span data-ttu-id="0af3c-476">執行行動瀏覽器 （或行動裝置模擬器），並瀏覽至*Page1.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-476">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="0af3c-477">請注意，目前網頁呈現頁面的行動裝置的版本。</span><span class="sxs-lookup"><span data-stu-id="0af3c-477">Notice that this time Web Pages renders the mobile version of the page.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="0af3c-478">若要測試行動頁面，您可以使用行動裝置模擬器，桌面的電腦上執行。</span><span class="sxs-lookup"><span data-stu-id="0af3c-478">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="0af3c-479">此工具可讓您測試網頁，因為它們會在行動裝置上看起來 （也就是通常具有較小顯示區域）。</span><span class="sxs-lookup"><span data-stu-id="0af3c-479">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="0af3c-480">模擬器的其中一個範例是[使用者代理程式切換器附加元件](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/)Mozilla Firefox，這可讓您模擬 Firefox 桌面版本從各種行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0af3c-480">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<span data-ttu-id="0af3c-481">*Page1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0af3c-481">*Page1.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

<span data-ttu-id="0af3c-482">*Page1.Mobile.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0af3c-482">*Page1.Mobile.cshtml*</span></span>

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

<span data-ttu-id="0af3c-483">*Page1.cshtml*桌面瀏覽器中呈現：</span><span class="sxs-lookup"><span data-stu-id="0af3c-483">*Page1.cshtml* rendered in a desktop browser:</span></span>

<span data-ttu-id="0af3c-484">[![topseven displaymodes 1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-484">[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)</span></span>

<span data-ttu-id="0af3c-485">*Page1.Mobile.cshtml* Apple iPhone 模擬器檢視中 Firefox 瀏覽器中顯示。</span><span class="sxs-lookup"><span data-stu-id="0af3c-485">*Page1.Mobile.cshtml* displayed in an Apple iPhone simulator view in the Firefox browser.</span></span> <span data-ttu-id="0af3c-486">即使該要求是*Page1.cshtml*，應用程式呈現*Page1.Mobile.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="0af3c-486">Even though the request is to *Page1.cshtml*, the application renders *Page1.Mobile.cshtml*.</span></span>

<span data-ttu-id="0af3c-487">[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="0af3c-487">[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)</span></span>

<a id="resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0af3c-488">其他資源</span><span class="sxs-lookup"><span data-stu-id="0af3c-488">Additional Resources</span></span>

### <a name="aspnet-web-pages-1-resources"></a><span data-ttu-id="0af3c-489">ASP.NET Web Pages 1 資源</span><span class="sxs-lookup"><span data-stu-id="0af3c-489">ASP.NET Web Pages 1 Resources</span></span>

> [!NOTE]
> <span data-ttu-id="0af3c-490">大部分的網頁 1 程式設計和應用程式開發介面資源仍然會套用到 Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="0af3c-490">Most Web Pages 1 programming and API resources still apply to Web Pages 2.</span></span>

- [<span data-ttu-id="0af3c-491">ASP.NET Web Pages 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="0af3c-491">Introduction to ASP.NET Web Pages Programming</span></span>](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a><span data-ttu-id="0af3c-492">WebMatrix 資源</span><span class="sxs-lookup"><span data-stu-id="0af3c-492">WebMatrix Resources</span></span>

- [<span data-ttu-id="0af3c-493">WebMatrix 2 最新消息</span><span class="sxs-lookup"><span data-stu-id="0af3c-493">WebMatrix 2 What's New</span></span>](http://webmatrix.com/next)
- [<span data-ttu-id="0af3c-494">Microsoft WebMatrix 網站</span><span class="sxs-lookup"><span data-stu-id="0af3c-494">Microsoft WebMatrix Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=195076)
- <span data-ttu-id="0af3c-495">[從 Microsoft WebMatrix Web 程式開發](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)（包括完整的範例網頁應用程式）</span><span class="sxs-lookup"><span data-stu-id="0af3c-495">[Starting Web Development with Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(includes a full-length sample Web Pages application)</span></span>
