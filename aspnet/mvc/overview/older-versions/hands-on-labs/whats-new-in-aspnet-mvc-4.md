---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 中最新消息 |Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 是用來建置可調整、 以標準為基礎的 web 應用程式，使用堅實的設計模式，以及使用 ASP.NET 的強大的架構和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8862c4da0d881a6f1084317e08697354c0ae6d48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374100"
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="f6078-103">ASP.NET MVC 4 中最新消息</span><span class="sxs-lookup"><span data-stu-id="f6078-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="f6078-104">藉由[Web Camp 小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f6078-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="f6078-105">下載 Web 研討會訓練套件</span><span class="sxs-lookup"><span data-stu-id="f6078-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="f6078-106">ASP.NET MVC 4 是建置可調整、 以標準為基礎的 web 應用程式使用完善的設計模式，以及 ASP.NET 和.NET framework 的強大功能的架構。</span><span class="sxs-lookup"><span data-stu-id="f6078-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="f6078-107">這個新、 第四個版本的 framework，著重於更輕鬆進行行動 web 應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="f6078-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="f6078-108">一開始，當您建立新的 ASP.NET MVC 4 專案目前沒有可用來建置一個獨立應用程式，專為行動裝置的行動應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="f6078-109">此外，ASP.NET MVC 4 與 jQuery Mobile jQuery.Mobile.MVC NuGet 套件透過整合。</span><span class="sxs-lookup"><span data-stu-id="f6078-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="f6078-110">jQuery Mobile 是以 HTML5 為基礎的架構開發 web 應用程式相容於所有熱門的行動裝置平台，包括 Windows Phone、 iPhone、 Android 等等。</span><span class="sxs-lookup"><span data-stu-id="f6078-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="f6078-111">不過，如果您需要特製化時，ASP.NET MVC 4 也可讓您提供不同裝置的不同檢視，並提供裝置特定的最佳化。</span><span class="sxs-lookup"><span data-stu-id="f6078-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="f6078-112">在這個實際操作實驗室中，您將開始使用 ASP.NET MVC 4&quot;網際網路應用程式&quot;建立相片圖庫的應用程式的專案範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="f6078-113">您會以漸進方式來加強使用 jQuery Mobile 和 ASP.NET MVC 4 的新功能，讓它與不同的行動裝置和桌面的網頁瀏覽器相容的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="f6078-114">您也將了解程式碼產生和 ASP.NET MVC 4 如何讓您更輕鬆地撰寫非同步動作方法所支援工作的新程式碼配方&lt;ActionResult&gt;傳回型別。</span><span class="sxs-lookup"><span data-stu-id="f6078-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="f6078-115">所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="f6078-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="f6078-116">這個實驗室中的特定專案將會位於[What's New in ASP.NET 4.5 Web Form](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)。</span><span class="sxs-lookup"><span data-stu-id="f6078-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f6078-117">目標</span><span class="sxs-lookup"><span data-stu-id="f6078-117">Objectives</span></span>

<span data-ttu-id="f6078-118">在這個實際操作實驗室中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="f6078-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f6078-119">利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="f6078-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="f6078-120">使用 HTML5 檢視區屬性和 CSS 媒體查詢，以改善在行動裝置上顯示</span><span class="sxs-lookup"><span data-stu-id="f6078-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="f6078-121">使用 jQuery Mobile，漸進式增強功能，以及建置觸控最佳化的 web UI</span><span class="sxs-lookup"><span data-stu-id="f6078-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="f6078-122">建立行動專用的檢視</span><span class="sxs-lookup"><span data-stu-id="f6078-122">Create mobile-specific views</span></span>
- <span data-ttu-id="f6078-123">行動和桌面應用程式中的檢視之間切換時，用以檢視切換器元件</span><span class="sxs-lookup"><span data-stu-id="f6078-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="f6078-124">建立使用工作支援非同步控制器</span><span class="sxs-lookup"><span data-stu-id="f6078-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f6078-125">必要條件</span><span class="sxs-lookup"><span data-stu-id="f6078-125">Prerequisites</span></span>

<span data-ttu-id="f6078-126">您必須具備下列項目，即可完成此實驗室：</span><span class="sxs-lookup"><span data-stu-id="f6078-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="f6078-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 B](#AppendixB)如需有關如何安裝它)。</span><span class="sxs-lookup"><span data-stu-id="f6078-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="f6078-128">[ASP.NET MVC 4](../../../mvc4.md) （隨附於 Microsoft Visual Studio 2012 安裝）</span><span class="sxs-lookup"><span data-stu-id="f6078-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="f6078-129">Windows Phone 模擬器 (納入[7.1.1 的 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="f6078-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="f6078-130">選用- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)具有**Electric Plum iPhone 模擬器**延伸模組 （僅適用於用來瀏覽 web 應用程式，與 iPhone 模擬器的練習 3)</span><span class="sxs-lookup"><span data-stu-id="f6078-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f6078-131">安裝程式</span><span class="sxs-lookup"><span data-stu-id="f6078-131">Setup</span></span>

<span data-ttu-id="f6078-132">整個實驗室文件中，您將會指示要插入程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="f6078-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="f6078-133">為了方便起見，大部分的程式碼提供 Visual Studio 程式碼片段，可以從 Visual Studio 內使用並避免以手動方式新增。</span><span class="sxs-lookup"><span data-stu-id="f6078-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="f6078-134">若要安裝的程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="f6078-134">To install the code snippets:</span></span>

1. <span data-ttu-id="f6078-135">開啟 Windows 檔案總管 視窗並瀏覽至實驗室**Source\Setup**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="f6078-136">按兩下**Setup.cmd**這個資料夾，以安裝 Visual Studio 程式碼片段中的檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="f6078-137">如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f6078-138">練習</span><span class="sxs-lookup"><span data-stu-id="f6078-138">Exercises</span></span>

<span data-ttu-id="f6078-139">這個實際操作實驗室還包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="f6078-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="f6078-140">新的 ASP.NET MVC 4 專案範本</span><span class="sxs-lookup"><span data-stu-id="f6078-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="f6078-141">建立相片圖庫 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f6078-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="f6078-142">新增行動裝置的支援</span><span class="sxs-lookup"><span data-stu-id="f6078-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="f6078-143">使用非同步控制器</span><span class="sxs-lookup"><span data-stu-id="f6078-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="f6078-144">每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f6078-145">如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。</span><span class="sxs-lookup"><span data-stu-id="f6078-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="f6078-146">完成這個實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="f6078-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="f6078-147">練習 1： 新的 ASP.NET MVC 4 專案範本</span><span class="sxs-lookup"><span data-stu-id="f6078-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="f6078-148">在此練習中，您將探索 ASP.NET MVC 4 專案範本中的增強功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="f6078-149">除了網際網路應用程式範本中，MVC 3 中已有此版本現在包含行動裝置應用程式的另一個範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="f6078-150">首先，您將查看每個範本的一些相關功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="f6078-151">然後，您將使用在使用正確的方式呈現您在不同的平台上正確的頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="f6078-152">工作 1-瀏覽網際網路應用程式範本</span><span class="sxs-lookup"><span data-stu-id="f6078-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="f6078-153">開啟**Visual Studio**。</span><span class="sxs-lookup"><span data-stu-id="f6078-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="f6078-154">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="f6078-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="f6078-155">在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="f6078-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="f6078-156">將專案命名為**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-157">您稍後將會自訂您要現在建立 PhotoGallery ASP.NET MVC 4 解決方案。</span><span class="sxs-lookup"><span data-stu-id="f6078-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="f6078-158">![建立新的專案](whats-new-in-aspnet-mvc-4/_static/image1.png "建立新的專案")</span><span class="sxs-lookup"><span data-stu-id="f6078-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="f6078-159">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="f6078-159">*Creating a new project*</span></span>
3. <span data-ttu-id="f6078-160">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="f6078-161">請確定您已選取 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="f6078-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="f6078-162">![建立新 ASP.NET MVC 4 網際網路應用程式](whats-new-in-aspnet-mvc-4/_static/image2.png "建立新 ASP.NET MVC 4 網際網路應用程式")</span><span class="sxs-lookup"><span data-stu-id="f6078-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="f6078-163">*建立新 ASP.NET MVC 4 網際網路應用程式*</span><span class="sxs-lookup"><span data-stu-id="f6078-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-164">ASP.NET MVC 3 中已引進 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="f6078-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="f6078-165">其目標是要減少字元和啟用快速和流暢的程式碼撰寫工作流程在檔案中，所需按鍵的數目。</span><span class="sxs-lookup"><span data-stu-id="f6078-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="f6078-166">Razor 會利用現有的 C# / VB （或其他） 的語言能力，並提供可讓一個很棒的 HTML 建構工作流程的範本標記語法。</span><span class="sxs-lookup"><span data-stu-id="f6078-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="f6078-167">按下**F5**以執行此方案，並查看更新的範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="f6078-168">您可以查看下列功能：</span><span class="sxs-lookup"><span data-stu-id="f6078-168">You can check out the following features:</span></span>

    <span data-ttu-id="f6078-169">**現代化樣式範本**</span><span class="sxs-lookup"><span data-stu-id="f6078-169">**Modern-style templates**</span></span>

    <span data-ttu-id="f6078-170">範本已經更新，提供更多的現代化外觀的樣式。</span><span class="sxs-lookup"><span data-stu-id="f6078-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="f6078-171">![重新設定樣式的 ASP.NET MVC 4 範本](whats-new-in-aspnet-mvc-4/_static/image3.png "抵擋範本的 MVC 4")</span><span class="sxs-lookup"><span data-stu-id="f6078-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="f6078-172">*重新設定樣式的 ASP.NET MVC 4 範本*</span><span class="sxs-lookup"><span data-stu-id="f6078-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="f6078-173">![新的 Contact 頁面](whats-new-in-aspnet-mvc-4/_static/image4.png "新連絡人 頁面")</span><span class="sxs-lookup"><span data-stu-id="f6078-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="f6078-174">*新的 [連絡人] 頁面*</span><span class="sxs-lookup"><span data-stu-id="f6078-174">*New Contact page*</span></span>

    <span data-ttu-id="f6078-175">**自適性轉譯**</span><span class="sxs-lookup"><span data-stu-id="f6078-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="f6078-176">請參閱調整瀏覽器視窗的大小，並注意如何頁面配置動態調整為新的視窗大小。</span><span class="sxs-lookup"><span data-stu-id="f6078-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="f6078-177">這些範本會使用適應性呈現技巧，來正確轉譯，而不需要任何自訂的桌上型電腦和行動裝置平台。</span><span class="sxs-lookup"><span data-stu-id="f6078-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="f6078-178">![ASP.NET MVC 4 專案範本，以不同的瀏覽器尺寸](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 專案範本，在不同的瀏覽器大小")</span><span class="sxs-lookup"><span data-stu-id="f6078-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="f6078-179">*ASP.NET MVC 4 專案範本，在不同的瀏覽器大小*</span><span class="sxs-lookup"><span data-stu-id="f6078-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="f6078-180">**使用 JavaScript 更豐富的 UI**</span><span class="sxs-lookup"><span data-stu-id="f6078-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="f6078-181">預設專案範本的其他增強功能是使用 JavaScript 來提供更具互動性的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="f6078-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="f6078-182">範本中使用的登入和註冊連結提供如何使用 jQuery 驗證來驗證用戶端的輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="f6078-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery 驗證](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="f6078-184">*jQuery 驗證*</span><span class="sxs-lookup"><span data-stu-id="f6078-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-185">請注意這兩個登入中的第一個區段的區段，您可以登入從站台的註冊帳戶並在第二個區段可以 altenativelly 登入使用另一個驗證服務，例如 google （預設為停用）。</span><span class="sxs-lookup"><span data-stu-id="f6078-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="f6078-186">關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f6078-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="f6078-187">開啟檔案**AuthConfig.cs**位於**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="f6078-188">移除註冊的 Google 用戶端的最後一行的註解*OAuth*驗證。</span><span class="sxs-lookup"><span data-stu-id="f6078-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="f6078-189">請注意，您可以輕鬆地啟用使用任何 OpenID 或 OAuth 的服務，例如 Facebook、 Twitter、 Microsoft 等的驗證。</span><span class="sxs-lookup"><span data-stu-id="f6078-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="f6078-190">按下**F5**執行方案，並瀏覽到 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="f6078-191">選取  **Google**服務以登入。</span><span class="sxs-lookup"><span data-stu-id="f6078-191">Select **Google** service to log in.</span></span>

    ![在服務中選取記錄檔](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="f6078-193">*在服務中選取記錄檔*</span><span class="sxs-lookup"><span data-stu-id="f6078-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="f6078-194">使用您的 Google 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="f6078-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="f6078-195">允許 Google 帳戶中擷取資訊的站台 (localhost)。</span><span class="sxs-lookup"><span data-stu-id="f6078-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="f6078-196">最後，您必須在站台建立關聯的 Google 帳戶中註冊。</span><span class="sxs-lookup"><span data-stu-id="f6078-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![將您的 Google 帳戶產生關聯](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="f6078-198">*將您的 Google 帳戶產生關聯*</span><span class="sxs-lookup"><span data-stu-id="f6078-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="f6078-199">關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f6078-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="f6078-200">立即探索方案，以查看由 ASP.NET MVC 4 專案範本中有些其他新功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="f6078-201">![ASP.NET MVC 4 的網際網路應用程式專案範本](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 的網際網路應用程式專案範本")</span><span class="sxs-lookup"><span data-stu-id="f6078-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="f6078-202">*ASP.NET MVC 4 的網際網路應用程式專案範本*</span><span class="sxs-lookup"><span data-stu-id="f6078-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="f6078-203">**HTML 5 標記**</span><span class="sxs-lookup"><span data-stu-id="f6078-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="f6078-204">瀏覽以找出新的佈景主題標記的範本檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="f6078-205">![新的範本，使用 Razor 和 HTML5 標記 About.cshtml。](whats-new-in-aspnet-mvc-4/_static/image10.png "使用 Razor 和 HTML5 標記 About.cshtml 的新範本。")</span><span class="sxs-lookup"><span data-stu-id="f6078-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="f6078-206">*新的範本，使用 Razor 和 HTML5 的標記 (About.cshtml)。*</span><span class="sxs-lookup"><span data-stu-id="f6078-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="f6078-207">**更新的 JavaScript 程式庫**</span><span class="sxs-lookup"><span data-stu-id="f6078-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="f6078-208">ASP.NET MVC 4 預設範本現在包含 KnockoutJS，可讓您建立豐富的 JavaScript MVVM 架構和靈敏的 web 應用程式使用 JavaScript 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="f6078-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="f6078-209">例如在 MVC3，jQuery 和 jQuery UI 程式庫也包含 ASP.NET MVC 4 中。</span><span class="sxs-lookup"><span data-stu-id="f6078-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f6078-210">您可以取得 KnockOutJS 程式庫，在下列連結的詳細資訊： [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="f6078-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="f6078-211">此外，您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。</span><span class="sxs-lookup"><span data-stu-id="f6078-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="f6078-212">工作 2-瀏覽行動裝置應用程式範本</span><span class="sxs-lookup"><span data-stu-id="f6078-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="f6078-213">ASP.NET MVC 4 可加速開發行動裝置的網站和平板電腦瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f6078-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="f6078-214">此範本有相同的應用程式結構，為網際網路應用程式範本 （請注意，控制器程式碼是幾乎完全相同），但其樣式已修改為觸控式行動裝置中正確轉譯。</span><span class="sxs-lookup"><span data-stu-id="f6078-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="f6078-215">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="f6078-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="f6078-216">在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="f6078-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="f6078-217">將專案命名為**PhotoGallery.Mobile**、 選取的位置 （或保留預設值），選取&quot;加入至方案&quot;，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="f6078-218">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**行動裝置應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="f6078-219">請確定您已選取 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="f6078-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="f6078-220">![建立新 ASP.NET MVC 4 行動應用程式](whats-new-in-aspnet-mvc-4/_static/image11.png "建立新 ASP.NET MVC 4 行動應用程式")</span><span class="sxs-lookup"><span data-stu-id="f6078-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="f6078-221">*建立新 ASP.NET MVC 4 行動應用程式*</span><span class="sxs-lookup"><span data-stu-id="f6078-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="f6078-222">現在，您便能夠探索解決方案，以及查看一些 ASP.NET MVC 4 的解決方案範本，適用於行動裝置所引進的新功能：</span><span class="sxs-lookup"><span data-stu-id="f6078-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="f6078-223">**jQuery Mobile 程式庫**</span><span class="sxs-lookup"><span data-stu-id="f6078-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="f6078-224">行動裝置應用程式專案範本包含 jQuery Mobile 文件庫，也就是行動瀏覽器相容性的開放原始碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="f6078-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="f6078-225">jQuery Mobile 適用於行動瀏覽器支援 CSS 和 JavaScript 的漸進式增強功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="f6078-226">漸進式增強功能可讓所有的瀏覽器，以顯示網頁的基本內容，而這只能啟用最強大的瀏覽器顯示豐富的內容。</span><span class="sxs-lookup"><span data-stu-id="f6078-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="f6078-227">JavaScript 和 CSS 檔案，納入 jQuery 行動樣式，說明要放入螢幕的內容，而不進行任何變更，在網頁標記中的行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f6078-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="f6078-229">*範本中所包含的 jQuery mobile 文件庫*</span><span class="sxs-lookup"><span data-stu-id="f6078-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="f6078-230">**HTML5 為基礎的標記**</span><span class="sxs-lookup"><span data-stu-id="f6078-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="f6078-232">*使用 HTML5 標記、 （Login.cshtml 和 index.cshtml） 的行動應用程式範本*</span><span class="sxs-lookup"><span data-stu-id="f6078-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="f6078-233">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="f6078-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="f6078-234">開啟**Windows Phone 7 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="f6078-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="f6078-235">在手機的開始畫面中，開啟 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="f6078-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="f6078-236">簽出的桌面應用程式的啟動位置的 URL，並從行動電話瀏覽至該 URL (例如`http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="f6078-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="f6078-237">現在您可以輸入的登入頁面，或請參閱 about 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="f6078-238">請注意，網站的樣式會根據新的 Metro 應用程式，適用於行動裝置。</span><span class="sxs-lookup"><span data-stu-id="f6078-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="f6078-239">ASP.NET MVC 4 專案範本會正確顯示行動裝置上，確定頁面上的所有項目會顯示並啟用。</span><span class="sxs-lookup"><span data-stu-id="f6078-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="f6078-240">請注意，標頭上的連結不夠大，按一下或點選。</span><span class="sxs-lookup"><span data-stu-id="f6078-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="f6078-241">![專案中的行動裝置的範本頁面](whats-new-in-aspnet-mvc-4/_static/image14.png "專案範本頁面，在行動裝置")</span><span class="sxs-lookup"><span data-stu-id="f6078-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="f6078-242">*行動裝置中的專案範本頁面*</span><span class="sxs-lookup"><span data-stu-id="f6078-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="f6078-243">也會使用新的範本**檢視區的中繼標籤**。</span><span class="sxs-lookup"><span data-stu-id="f6078-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="f6078-244">大部分的行動瀏覽器定義虛擬瀏覽器視窗的寬度或&quot;viewport&quot;，即大於行動裝置的實際寬度。</span><span class="sxs-lookup"><span data-stu-id="f6078-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="f6078-245">這可讓行動瀏覽器顯示整個 web 網頁內虛擬顯示。</span><span class="sxs-lookup"><span data-stu-id="f6078-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="f6078-246">**檢視區的中繼標籤**可讓 web 開發人員在行動裝置上設定的寬度、 高度和小數位數的瀏覽器區域 **。**</span><span class="sxs-lookup"><span data-stu-id="f6078-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="f6078-247">行動應用程式的 ASP.NET MVC 4 範本會將檢視區設定為裝置寬度 (&quot;寬度 = 裝置寬度&quot;) 中的版面配置範本 (*Views\Shared\_Layout.cshtml*)，因此所有頁面會有其設定為裝置螢幕寬度的檢視區。</span><span class="sxs-lookup"><span data-stu-id="f6078-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="f6078-248">請注意，將檢視區的中繼標籤不會變更預設瀏覽器檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="f6078-249">開啟 **\_Layout.cshtml**位於**檢視 |共用**資料夾，然後將檢視區的中繼標籤的註解。</span><span class="sxs-lookup"><span data-stu-id="f6078-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="f6078-250">執行應用程式中，如果不是已開啟，並查看差異。</span><span class="sxs-lookup"><span data-stu-id="f6078-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="f6078-251">![註解將檢視區的中繼標籤之後的站台](whats-new-in-aspnet-mvc-4/_static/image15.png "註化檢視區中繼標籤之後的站台")</span><span class="sxs-lookup"><span data-stu-id="f6078-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="f6078-252">*站台之後註解的檢視區的中繼標記*</span><span class="sxs-lookup"><span data-stu-id="f6078-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="f6078-253">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="f6078-254">取消註解將檢視區的中繼標籤。</span><span class="sxs-lookup"><span data-stu-id="f6078-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="f6078-255">工作 3-使用適應性呈現</span><span class="sxs-lookup"><span data-stu-id="f6078-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="f6078-256">在這個工作中，您將學習另一種方法，而不需要任何自訂，同時呈現在行動裝置和網頁瀏覽器上正確的網頁。</span><span class="sxs-lookup"><span data-stu-id="f6078-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="f6078-257">您已使用類似的目的與檢視區的中繼標籤。</span><span class="sxs-lookup"><span data-stu-id="f6078-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="f6078-258">現在您將會符合另一個功能強大的方法：*適應性呈現*。</span><span class="sxs-lookup"><span data-stu-id="f6078-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="f6078-259">適應性呈現是使用一種技術**CSS3 媒體查詢**自訂樣式套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="f6078-260">媒體查詢定義群組在特定條件下的 CSS 樣式的樣式表內的條件。</span><span class="sxs-lookup"><span data-stu-id="f6078-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="f6078-261">只有當條件為 true，則會將樣式套用至宣告的物件。</span><span class="sxs-lookup"><span data-stu-id="f6078-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="f6078-262">適應性呈現技術所提供的彈性可讓不同裝置上顯示站台的任何自訂。</span><span class="sxs-lookup"><span data-stu-id="f6078-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="f6078-263">您可以定義多個您要在單一樣式表上不需要撰寫邏輯程式碼選擇樣式的樣式。</span><span class="sxs-lookup"><span data-stu-id="f6078-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="f6078-264">因此，它是非常簡潔的方式，進行調整頁面的樣式，因為它會減少重複的程式碼和邏輯的轉譯用途的數量。</span><span class="sxs-lookup"><span data-stu-id="f6078-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="f6078-265">相反地，頻寬耗用量會增加，因為您的 CSS 檔案的大小可能會稍微變。</span><span class="sxs-lookup"><span data-stu-id="f6078-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="f6078-266">您的網站將會使用適應性呈現技術，**正常地顯示，不論瀏覽器。**</span><span class="sxs-lookup"><span data-stu-id="f6078-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="f6078-267">不過，您應該考慮是否有額外的頻寬載入是一項考量。</span><span class="sxs-lookup"><span data-stu-id="f6078-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="f6078-268">媒體查詢的基本格式為： @media \[範圍： 所有 | 掌上型 | 列印 | 投影 | 畫面\]([屬性： 值] 和...[屬性： value])</span><span class="sxs-lookup"><span data-stu-id="f6078-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="f6078-269">媒體查詢的範例： &gt;  <strong>@media所有和 (最大寬度： 1000px) 和 (最小寬度： 700px) {}:</strong> 700px 和 1000px 之間的所有解析度。</span><span class="sxs-lookup"><span data-stu-id="f6078-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="f6078-270"><strong>@media 螢幕和 (最小寬度： 400px) 和 (最大寬度： 700px) {...}:</strong>只針對螢幕。</span><span class="sxs-lookup"><span data-stu-id="f6078-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="f6078-271">解決方法必須是介於 400 到 700px。</span><span class="sxs-lookup"><span data-stu-id="f6078-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="f6078-272"><strong>@media 掌上型和 (最小寬度： 20em)，畫面和 (最小寬度： 20em) {...}:</strong>掌上型 （行動裝置和裝置） 和畫面。</span><span class="sxs-lookup"><span data-stu-id="f6078-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="f6078-273">最小寬度必須大於 20em。</span><span class="sxs-lookup"><span data-stu-id="f6078-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="f6078-274">您可以找到相關的詳細資訊，在[W3C 網站](http://www.w3.org/TR/css3-mediaqueries/)。</span><span class="sxs-lookup"><span data-stu-id="f6078-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="f6078-275">您現在將探索適應性呈現的運作方式，改善可讀性的 ASP.NET MVC 4 預設的網站範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="f6078-276">開啟**PhotoGallery.sln**方案，您已在工作 1 中建立，並選取**PhotoGallery**專案。</span><span class="sxs-lookup"><span data-stu-id="f6078-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="f6078-277">按下**F5**執行方案。</span><span class="sxs-lookup"><span data-stu-id="f6078-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="f6078-278">調整瀏覽器的寬度，設定 windows，一半或小於其原始大小的四分之一。</span><span class="sxs-lookup"><span data-stu-id="f6078-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="f6078-279">請注意，標頭中的項目會發生什麼事： 某些項目不會出現在標頭的可見區域。</span><span class="sxs-lookup"><span data-stu-id="f6078-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="f6078-280">開啟<strong>Site.css</strong>檔案，從 Visual Studio 方案總管 中，位於<strong>內容</strong>專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="f6078-281">按下<strong>CTRL + F</strong>以開啟 Visual Studio 整合式的搜尋，並撰寫<strong>@media</strong>找出<strong>CSS 媒體查詢</strong>。</span><span class="sxs-lookup"><span data-stu-id="f6078-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="f6078-282">此範本中定義的媒體查詢條件適用於這種方式： 當瀏覽器視窗大小低於**850 px**，套用的 CSS 規則是定義在這個媒體區塊中的項目。</span><span class="sxs-lookup"><span data-stu-id="f6078-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="f6078-283">![尋找媒體查詢](whats-new-in-aspnet-mvc-4/_static/image16.png "尋找媒體查詢")</span><span class="sxs-lookup"><span data-stu-id="f6078-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="f6078-284">*尋找媒體查詢*</span><span class="sxs-lookup"><span data-stu-id="f6078-284">*Locating the media query*</span></span>
4. <span data-ttu-id="f6078-285">850 中設定的最大寬度屬性值取代 px **10px**，以停用自動調整的轉譯，然後按**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="f6078-286">回到瀏覽器，並按下**CTRL + F5**重新整理該頁面與您所做的變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="f6078-287">請注意這兩個頁面中的差異，當調整視窗的寬度。</span><span class="sxs-lookup"><span data-stu-id="f6078-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="f6078-288">![套用頁面左側@media樣式，請在右邊的樣式省略](whats-new-in-aspnet-mvc-4/_static/image17.png "方，頁面會將套用@media樣式，請在右邊的樣式省略")</span><span class="sxs-lookup"><span data-stu-id="f6078-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="f6078-289"><em>在左邊，將套用頁面@media省略了樣式，請在右側，樣式</em></span><span class="sxs-lookup"><span data-stu-id="f6078-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="f6078-290">現在，讓我們查看行動裝置上發生的狀況：</span><span class="sxs-lookup"><span data-stu-id="f6078-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="f6078-291">![套用頁面左側@media樣式，請在右邊的樣式省略](whats-new-in-aspnet-mvc-4/_static/image18.png "方，頁面會將套用@media樣式，請在右邊的樣式省略")</span><span class="sxs-lookup"><span data-stu-id="f6078-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="f6078-292"><em>在左邊，將套用頁面@media省略了樣式，請在右側，樣式</em></span><span class="sxs-lookup"><span data-stu-id="f6078-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="f6078-293">雖然您會注意到，在網頁瀏覽器中呈現的頁面時，會變更時，不非常重要的是，使用行動裝置的差異變得更明顯。</span><span class="sxs-lookup"><span data-stu-id="f6078-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="f6078-294">左邊的映像，我們可以看到自訂樣式改善可讀性。</span><span class="sxs-lookup"><span data-stu-id="f6078-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="f6078-295">適應性呈現可以用於許多案例，讓您更輕鬆地套用條件式網站設定樣式和解決一般樣式問題很棒的方法。</span><span class="sxs-lookup"><span data-stu-id="f6078-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="f6078-296">檢視區 meta 標記和 CSS 媒體查詢特有不 ASP.NET MVC 4 中，因此您可以利用這些功能的任何 web 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="f6078-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="f6078-297">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="f6078-298">練習 2： 建立相片圖庫 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f6078-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="f6078-299">在此練習中，您將使用的相片圖庫應用程式，以顯示相片。</span><span class="sxs-lookup"><span data-stu-id="f6078-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="f6078-300">ASP.NET MVC 4 專案範本中，將會開始，然後您會在其中加入從服務擷取相片，並在 [首頁] 頁面中顯示的功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="f6078-301">在下列練習中，您將會更新此解決方案，強化行動裝置的裝置顯示的方式。</span><span class="sxs-lookup"><span data-stu-id="f6078-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="f6078-302">工作 1-建立模擬 （mock） 的相片服務</span><span class="sxs-lookup"><span data-stu-id="f6078-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="f6078-303">在這個工作中，您將建立 mock 相片要擷取的服務將會顯示在資源庫的內容。</span><span class="sxs-lookup"><span data-stu-id="f6078-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="f6078-304">若要這樣做，您將加入新的控制站，只會傳回具有資料的每張相片的 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="f6078-305">開啟**Visual Studio**如果尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="f6078-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="f6078-306">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="f6078-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="f6078-307">在 **新的專案**對話方塊中，選取**Visual C# |Web**範本，在左窗格中樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="f6078-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="f6078-308">將專案命名為**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="f6078-309">或者，您可以繼續使用您現有的 ASP.NET MVC 4**網際網路應用程式**解決方案**練習 1**並略過下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="f6078-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="f6078-310">在 **新的 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="f6078-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="f6078-311">請確定您具有 Razor 選為檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="f6078-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="f6078-312">在 [**方案總管] 中**，以滑鼠右鍵按一下**應用程式\_資料**資料夾，您的專案，然後選取**新增 |現有的項目**。</span><span class="sxs-lookup"><span data-stu-id="f6078-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="f6078-313">瀏覽至**Source\Assets\App\_資料**本實驗室的資料夾，並新增**Photos.json**檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="f6078-314">建立新的控制器名稱**PhotoController**。</span><span class="sxs-lookup"><span data-stu-id="f6078-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="f6078-315">若要這樣做，以滑鼠右鍵按一下**控制器**資料夾中，移至**新增**，然後選取**控制站。**</span><span class="sxs-lookup"><span data-stu-id="f6078-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="f6078-316">完成 控制器名稱，請將保留**空白 MVC 控制器**範本，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6078-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="f6078-317">![新增 PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "新增 PhotoController")</span><span class="sxs-lookup"><span data-stu-id="f6078-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="f6078-318">*新增 PhotoController*</span><span class="sxs-lookup"><span data-stu-id="f6078-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="f6078-319">取代**Index**方法取代為下列**資源庫**動作，以及最近加入至專案的 JSON 檔案中的傳回內容。</span><span class="sxs-lookup"><span data-stu-id="f6078-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="f6078-320">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-組件庫動作*)</span><span class="sxs-lookup"><span data-stu-id="f6078-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="f6078-321">按下**F5**執行方案，並然後若要測試的模擬 （mock） 的相片服務瀏覽至下列 URL: `http://localhost:[port]/photo/gallery` （取決於目前的連接埠已在其中啟動應用程式的 [連接埠] 值）。</span><span class="sxs-lookup"><span data-stu-id="f6078-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="f6078-322">此 URL 的要求時，應擷取的內容**Photos.json**檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="f6078-323">![測試模擬 （mock） 的相片服務](whats-new-in-aspnet-mvc-4/_static/image20.png "測試模擬 （mock） 的相片服務")</span><span class="sxs-lookup"><span data-stu-id="f6078-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="f6078-324">*測試模擬 （mock） 的相片服務*</span><span class="sxs-lookup"><span data-stu-id="f6078-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="f6078-325">在實際的實作，您可以使用[ASP.NET Web API](../../../../web-api/index.md)實作相片圖庫 服務。</span><span class="sxs-lookup"><span data-stu-id="f6078-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="f6078-326">ASP.NET Web API 是一種架構，可讓您更輕鬆建置 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="f6078-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="f6078-327">ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。</span><span class="sxs-lookup"><span data-stu-id="f6078-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="f6078-328">工作 2-顯示相片圖庫</span><span class="sxs-lookup"><span data-stu-id="f6078-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="f6078-329">在這個工作中，您將會更新以顯示相片圖庫，使用您在本練習的第一項工作中建立的模擬 （mock） 的服務的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="f6078-330">您會將模型檔案，並更新 [資源庫] 檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="f6078-331">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="f6078-332">建立**相片**類別中**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="f6078-333">若要這樣做，以滑鼠右鍵按一下**模型**資料夾中，選取**新增**然後按一下**類別**。</span><span class="sxs-lookup"><span data-stu-id="f6078-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="f6078-334">然後，將名稱設定為**Photo.cs**然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6078-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="f6078-335">新增下列成員來**相片**類別。</span><span class="sxs-lookup"><span data-stu-id="f6078-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="f6078-336">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片模型*)</span><span class="sxs-lookup"><span data-stu-id="f6078-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="f6078-337">從 **Controllers** 資料夾中，開啟 **HomeController.cs** 檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="f6078-338">使用陳述式加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-338">Add the following using statements.</span></span>

    <span data-ttu-id="f6078-339">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-HomeController Using*)</span><span class="sxs-lookup"><span data-stu-id="f6078-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="f6078-340">更新**的索引**若要使用的動作**HttpClient**組件庫及擷取資料，然後使用**JavaScriptSerializer**將它還原序列化至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="f6078-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="f6078-341">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-索引動作*)</span><span class="sxs-lookup"><span data-stu-id="f6078-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="f6078-342">開啟**Index.cshtml**下的檔案**Views\Home**資料夾，並將下列程式碼的所有內容。</span><span class="sxs-lookup"><span data-stu-id="f6078-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="f6078-343">此程式碼從服務擷取的所有相片中執行都迴圈，並顯示到的未排序清單中。</span><span class="sxs-lookup"><span data-stu-id="f6078-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="f6078-344">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片清單*)</span><span class="sxs-lookup"><span data-stu-id="f6078-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="f6078-345">在 [**方案總管] 中**，以滑鼠右鍵按一下**內容**資料夾，您的專案，然後選取**新增 |現有的項目**。</span><span class="sxs-lookup"><span data-stu-id="f6078-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="f6078-346">瀏覽至**Source\Assets\Content**這個實驗室的資料夾，並新增**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="f6078-347">您必須確認其取代。</span><span class="sxs-lookup"><span data-stu-id="f6078-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="f6078-348">如果您有**Site.css**檔案開啟，您必須確認也重新載入檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="f6078-349">開啟檔案總管，然後複製整個**相片**資料夾位於下面**Source\Assets**本實驗室的 方案總管 中的專案根資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="f6078-350">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-350">Run the application.</span></span> <span data-ttu-id="f6078-351">現在，您應該會看到顯示相片圖庫中的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="f6078-352">![相片圖庫](whats-new-in-aspnet-mvc-4/_static/image21.png "相片圖庫")</span><span class="sxs-lookup"><span data-stu-id="f6078-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="f6078-353">*相片圖庫*</span><span class="sxs-lookup"><span data-stu-id="f6078-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="f6078-354">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="f6078-355">練習 3： 新增行動裝置的支援</span><span class="sxs-lookup"><span data-stu-id="f6078-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="f6078-356">ASP.NET MVC 4 中的金鑰更新的其中一個是行動裝置開發的支援。</span><span class="sxs-lookup"><span data-stu-id="f6078-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="f6078-357">在此練習中，您將探索 ASP.NET MVC 4 行動裝置應用程式的新功能來擴充您在前一個練習中建立的 PhotoGallery 解決方案。</span><span class="sxs-lookup"><span data-stu-id="f6078-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="f6078-358">工作 1-安裝 ASP.NET MVC 4 應用程式中的 jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="f6078-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="f6078-359">開啟**開始**解決方案位於**來源/Ex3-MobileSupport/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="f6078-360">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="f6078-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f6078-361">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="f6078-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f6078-362">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="f6078-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f6078-363">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="f6078-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f6078-364">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="f6078-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f6078-365">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="f6078-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f6078-366">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="f6078-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f6078-367">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="f6078-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="f6078-368">開啟**Package Manager Console**依序按一下**工具** &gt; **程式庫套件管理員** &gt; **套件管理員主控台**功能表選項。</span><span class="sxs-lookup"><span data-stu-id="f6078-368">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="f6078-369">![開啟 NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "開啟 NuGet 套件管理員主控台")</span><span class="sxs-lookup"><span data-stu-id="f6078-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="f6078-370">*開啟 NuGet 套件管理員主控台*</span><span class="sxs-lookup"><span data-stu-id="f6078-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="f6078-371">在 Package Manager Console 執行下列命令以安裝**jQuery.Mobile.MVC**封裝。</span><span class="sxs-lookup"><span data-stu-id="f6078-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="f6078-372">jQuery Mobile 是開放原始碼程式庫建置觸控最佳化的 web UI。</span><span class="sxs-lookup"><span data-stu-id="f6078-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="f6078-373">JQuery.Mobile.MVC NuGet 套件包含 ASP.NET MVC 4 應用程式中使用 jQuery Mobile 的協助程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-374">藉由執行下列命令，您將下載 jQuery.Mobile.MVC 程式庫從 Nuget。</span><span class="sxs-lookup"><span data-stu-id="f6078-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="f6078-375">PM</span><span class="sxs-lookup"><span data-stu-id="f6078-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="f6078-376">此命令會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：</span><span class="sxs-lookup"><span data-stu-id="f6078-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="f6078-377">**Views/Shared/\_Layout.Mobile.cshtml**： 是 jQuery mobile 的版面配置適用於較小的螢幕。</span><span class="sxs-lookup"><span data-stu-id="f6078-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="f6078-378">當網站收到要求時從行動瀏覽器時，它會取代原始的版面配置 (\_Layout.cshtml) 換成這一個。</span><span class="sxs-lookup"><span data-stu-id="f6078-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="f6078-379">檢視切換器元件： 組成**Views/Shared/\_ViewSwitcher.cshtml**部分檢視和**ViewSwitcherController.cs**控制站。</span><span class="sxs-lookup"><span data-stu-id="f6078-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="f6078-380">此元件會顯示在行動瀏覽器，讓使用者可以切換到桌面版本的頁面上的連結。</span><span class="sxs-lookup"><span data-stu-id="f6078-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="f6078-381">![行動裝置支援的相片圖庫專案](whats-new-in-aspnet-mvc-4/_static/image23.png "相片圖庫 專案的行動支援")</span><span class="sxs-lookup"><span data-stu-id="f6078-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="f6078-382">*相片圖庫 專案的行動支援*</span><span class="sxs-lookup"><span data-stu-id="f6078-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="f6078-383">註冊行動裝置的組合。</span><span class="sxs-lookup"><span data-stu-id="f6078-383">Register the Mobile bundles.</span></span> <span data-ttu-id="f6078-384">若要這樣做，請開啟**Global.asax.cs**檔案，並新增下面這一行。</span><span class="sxs-lookup"><span data-stu-id="f6078-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="f6078-385">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-註冊行動裝置組合*)</span><span class="sxs-lookup"><span data-stu-id="f6078-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="f6078-386">執行桌面的網頁瀏覽器應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="f6078-387">開啟**Windows Phone 7 模擬器，模擬器**位於 **[開始] 功能表 |所有程式 |Windows Phone SDK 7.1 |Windows Phone 模擬器。**</span><span class="sxs-lookup"><span data-stu-id="f6078-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="f6078-388">在手機的開始畫面中，開啟 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="f6078-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="f6078-389">查看 啟動應用程式的 URL，並瀏覽至該電話瀏覽器的 URL (例如`http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="f6078-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="f6078-390">您會發現您的應用程式會尋找在 Windows Phone 模擬器中，不同 jQuery.Mobile.MVC 已顯示針對行動裝置最佳化的檢視之專案中建立新的資產。</span><span class="sxs-lookup"><span data-stu-id="f6078-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="f6078-391">請注意頂端的電話，顯示會切換成桌面檢視連結的訊息。</span><span class="sxs-lookup"><span data-stu-id="f6078-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="f6078-392">此外，  **\_Layout.Mobile.cshtml**由您已安裝的封裝所建立的版面配置在應用程式中，包括不同的版面配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-393">到目前為止，沒有任何連結回到行動檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="f6078-394">就會包含在較新版本。</span><span class="sxs-lookup"><span data-stu-id="f6078-394">It will be included in later versions.</span></span>

    <span data-ttu-id="f6078-395">![相片圖庫首頁上的行動裝置檢視](whats-new-in-aspnet-mvc-4/_static/image24.png "Photo Gallery 首頁上的行動檢視")</span><span class="sxs-lookup"><span data-stu-id="f6078-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="f6078-396">*相片圖庫首頁上的行動檢視*</span><span class="sxs-lookup"><span data-stu-id="f6078-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="f6078-397">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="f6078-398">工作 2-建立行動裝置檢視</span><span class="sxs-lookup"><span data-stu-id="f6078-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="f6078-399">在這個工作中，您將建立索引 檢視的行動裝置版與針對行動裝置中的較佳 appareance 調整的內容。</span><span class="sxs-lookup"><span data-stu-id="f6078-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="f6078-400">複製**Views\Home\Index.cshtml**檢視，並將它貼到 建立複本、 重新命名新的檔案以**Index.Mobile.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="f6078-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="f6078-401">開啟新建立**Index.Mobile.cshtml**檢視，並取代現有&lt;ul&gt;標記，以下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="f6078-402">如此一來，您將會更新&lt;ul&gt;使用來自 jQuery 行動的佈景主題的 jQuery 行動資料註解的標記。</span><span class="sxs-lookup"><span data-stu-id="f6078-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="f6078-403">請注意：</span><span class="sxs-lookup"><span data-stu-id="f6078-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="f6078-404">**資料角色**屬性設為**listview**會呈現使用 listview 樣式的清單。</span><span class="sxs-lookup"><span data-stu-id="f6078-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="f6078-405">**資料嵌入**屬性設為 true 將會顯示具有圓角的框線和邊界的清單。</span><span class="sxs-lookup"><span data-stu-id="f6078-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="f6078-406">**資料篩選**屬性設為 **，則為 true**會產生一個搜尋方塊。</span><span class="sxs-lookup"><span data-stu-id="f6078-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="f6078-407">您可以深入了解專案文件中的 jQuery Mobile 慣例： [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="f6078-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="f6078-408">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="f6078-409">若要切換**Windows Phone 模擬器**和重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="f6078-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="f6078-410">請注意新的外觀與風格的組件庫 清單中，為新的搜尋方塊位於頂端。</span><span class="sxs-lookup"><span data-stu-id="f6078-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="f6078-411">然後，在搜尋方塊中輸入文字 (例如**Tulips**) 才會在相片圖庫中開始搜尋。</span><span class="sxs-lookup"><span data-stu-id="f6078-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="f6078-412">![使用 listview 樣式已篩選的資源庫](whats-new-in-aspnet-mvc-4/_static/image25.png "使用 listview 樣式已篩選的組件庫")</span><span class="sxs-lookup"><span data-stu-id="f6078-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="f6078-413">*使用 listview 樣式已篩選的組件庫*</span><span class="sxs-lookup"><span data-stu-id="f6078-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="f6078-414">總結來說，您已用檢視 Mobilizer 配方來建立一份索引檢視，其中&quot;行動&quot;後置詞。</span><span class="sxs-lookup"><span data-stu-id="f6078-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="f6078-415">這個後置詞表示 ASP.NET MVC 4 從行動裝置產生的每個要求會使用這份索引。</span><span class="sxs-lookup"><span data-stu-id="f6078-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="f6078-416">此外，您已更新 [索引] 檢視，可以提升網站的外觀與風格，行動裝置中使用 jQuery Mobile 的行動裝置的版本。</span><span class="sxs-lookup"><span data-stu-id="f6078-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="f6078-417">返回 Visual Studio 並開啟**Site.Mobile.css**位於**內容**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="f6078-418">修正相片標題，讓它顯示在右側的映像的位置。</span><span class="sxs-lookup"><span data-stu-id="f6078-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="f6078-419">若要這樣做，請新增下列程式碼**Site.Mobile.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="f6078-420">CSS</span><span class="sxs-lookup"><span data-stu-id="f6078-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="f6078-421">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="f6078-422">切換回**Windows Phone 模擬器**和重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="f6078-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="f6078-423">請注意相片標題則適當定位現在。</span><span class="sxs-lookup"><span data-stu-id="f6078-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="f6078-424">![標題位於影像的右側](whats-new-in-aspnet-mvc-4/_static/image26.png "定位影像右側的標題")</span><span class="sxs-lookup"><span data-stu-id="f6078-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="f6078-425">*標題位於右側的映像*</span><span class="sxs-lookup"><span data-stu-id="f6078-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="f6078-426">工作 3-jQuery Mobile 佈景主題</span><span class="sxs-lookup"><span data-stu-id="f6078-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="f6078-427">每個配置和中的 jQuery Mobile 小工具的設計周圍的新物件導向 CSS 架構讓您能夠將完整整合的視覺化設計佈景主題套用至網站和應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="f6078-428">jQuery Mobile 的預設佈景主題包含 5 個樣本指定字母 (a、 b、 c、 d、 e） 的快速參考。</span><span class="sxs-lookup"><span data-stu-id="f6078-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="f6078-429">在這個工作中，您將會更新行動裝置的版面配置，以使用不同的佈景主題，比預設值。</span><span class="sxs-lookup"><span data-stu-id="f6078-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="f6078-430">切換回到 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f6078-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="f6078-431">開啟 **\_Layout.Mobile.cshtml**檔案位於**Views\Shared**。</span><span class="sxs-lookup"><span data-stu-id="f6078-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="f6078-432">尋找 div 項目設定為資料角色&quot;頁面&quot;，並更新**資料佈景主題**來&quot;**電子**&quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="f6078-433">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="f6078-434">重新整理中的站台**Windows Phone 模擬器**並注意新的色彩配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="f6078-435">![行動裝置使用不同的色彩配置的配置](whats-new-in-aspnet-mvc-4/_static/image27.png "行動配置使用不同的色彩配置")</span><span class="sxs-lookup"><span data-stu-id="f6078-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="f6078-436">*行動裝置使用不同的色彩配置的配置*</span><span class="sxs-lookup"><span data-stu-id="f6078-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="f6078-437">工作 4-使用檢視切換器元件，並覆寫功能的瀏覽器</span><span class="sxs-lookup"><span data-stu-id="f6078-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="f6078-438">最適合行動裝置的網頁通常會加入其文字是像桌面檢視] 或 [完整的網站模式可讓使用者切換到桌面版本的頁面連結。</span><span class="sxs-lookup"><span data-stu-id="f6078-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="f6078-439">JQuery.Mobile.MVC 套件包含範例**檢視切換器**元件用於此用途 **\_Layout.Mobile.cshtml**檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="f6078-440">![若要切換至 [桌面] 檢視的連結](whats-new-in-aspnet-mvc-4/_static/image28.png "連結切換到桌面檢視")</span><span class="sxs-lookup"><span data-stu-id="f6078-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="f6078-441">*若要切換到桌面檢視連結*</span><span class="sxs-lookup"><span data-stu-id="f6078-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="f6078-442">檢視切換程式會使用新功能，稱為**瀏覽器覆寫**。</span><span class="sxs-lookup"><span data-stu-id="f6078-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="f6078-443">這項功能可讓您要求視為就像是來自它們實際上來自於不同瀏覽器 （使用者代理程式） 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="f6078-444">在這個工作中，您將探索由 jQuery.Mobile.MVC 和新的瀏覽器覆寫 ASP.NET MVC 4 中的功能加入檢視切換器的範例實作。</span><span class="sxs-lookup"><span data-stu-id="f6078-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="f6078-445">切換回到 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f6078-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="f6078-446">開啟 **\_Layout.Mobile.cshtml**檢視位於**Views\Shared**資料夾，請注意與部分檢視所參考的檢視切換器元件。</span><span class="sxs-lookup"><span data-stu-id="f6078-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="f6078-447">![行動裝置使用檢視切換器元件的版面配置](whats-new-in-aspnet-mvc-4/_static/image29.png "行動配置使用檢視切換器元件")</span><span class="sxs-lookup"><span data-stu-id="f6078-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="f6078-448">*行動裝置使用檢視切換器元件的版面配置*</span><span class="sxs-lookup"><span data-stu-id="f6078-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="f6078-449">開啟 **\_ViewSwitcher.cshtml**部分檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="f6078-450">部分檢視會使用新的方法**ViewContext.HttpContext.GetOverriddenBrowser()** 判斷 web 要求的來源，並顯示對應的連結，以切換至 桌面 或 行動檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="f6078-451">**GetOverridenBrowser**方法會傳回**HttpBrowserCapabilitiesBase**對應至目前設定為要求的使用者代理程式的執行個體 （實際或覆寫）。</span><span class="sxs-lookup"><span data-stu-id="f6078-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="f6078-452">您可以使用此值，取得屬性，例如**IsMobileDevice**。</span><span class="sxs-lookup"><span data-stu-id="f6078-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="f6078-453">![ViewSwitcher 部分檢視](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分檢視")</span><span class="sxs-lookup"><span data-stu-id="f6078-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="f6078-454">*ViewSwitcher 部分檢視*</span><span class="sxs-lookup"><span data-stu-id="f6078-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="f6078-455">開啟**ViewSwitcherController.cs**類別位於**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="f6078-456">簽出該 SwitchView 動作會呼叫 ViewSwitcher 元件中的連結，並注意新的 HttpContext 方法。</span><span class="sxs-lookup"><span data-stu-id="f6078-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="f6078-457">**HttpContext.ClearOverridenBrowser()** 方法會移除目前要求的任何覆寫的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="f6078-458">**HttpContext.SetOverridenBrowser()** 方法會覆寫要求使用指定的使用者代理程式的實際使用者代理程式值。</span><span class="sxs-lookup"><span data-stu-id="f6078-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="f6078-459">![ViewSwitcher 控制器](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher 控制器")</span><span class="sxs-lookup"><span data-stu-id="f6078-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="f6078-460">*ViewSwitcher 控制器*</span><span class="sxs-lookup"><span data-stu-id="f6078-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="f6078-461">瀏覽器覆寫是核心功能的 ASP.NET MVC 4 中，即使您沒有安裝 jQuery.Mobile.MVC 封裝所也提供。</span><span class="sxs-lookup"><span data-stu-id="f6078-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="f6078-462">不過，這項功能會影響檢視、 配置和部分檢視，而不影響任何相依於 Request.Browser 物件的功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="f6078-463">工作 5-加入桌面檢視中的檢視切換器</span><span class="sxs-lookup"><span data-stu-id="f6078-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="f6078-464">在這個工作中，您將會更新桌面的版面配置，以包含檢視切換程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="f6078-465">這可讓行動使用者返回行動檢視瀏覽桌面檢視時。</span><span class="sxs-lookup"><span data-stu-id="f6078-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="f6078-466">重新整理中的站台**Windows Phone 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="f6078-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="f6078-467">按一下 **桌面檢視**頂端的 資源庫的連結。</span><span class="sxs-lookup"><span data-stu-id="f6078-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="f6078-468">請注意在桌面的檢視，讓您回到 [行動] 檢視中沒有任何檢視切換器。</span><span class="sxs-lookup"><span data-stu-id="f6078-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="f6078-469">返回 Visual Studio 並開啟 **\_Layout.cshtml**檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="f6078-470">尋找登入區段，並插入呼叫轉譯 **\_ViewSwitcher**下的部分檢視 **\_LogOnPartial**部分檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="f6078-471">然後按**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="f6078-472">按下**CTRL + S**以儲存變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="f6078-473">重新整理頁面，在 Windows Phone 模擬器中的，然後按兩下 [放大] 畫面。</span><span class="sxs-lookup"><span data-stu-id="f6078-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="f6078-474">請注意，在 [首頁] 頁面現在會顯示**行動裝置檢視**從行動裝置會切換成桌面檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="f6078-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="f6078-475">![檢視切換器在桌面檢視中呈現](whats-new-in-aspnet-mvc-4/_static/image32.png "在桌面檢視中呈現的檢視切換器")</span><span class="sxs-lookup"><span data-stu-id="f6078-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="f6078-476">*在桌面檢視中呈現的檢視切換器*</span><span class="sxs-lookup"><span data-stu-id="f6078-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="f6078-477">再次切換至 行動裝置檢視，然後瀏覽至<strong>關於</strong>網頁 (http://localhost[連接埠] / Home/有關)。</span><span class="sxs-lookup"><span data-stu-id="f6078-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="f6078-478">請注意，即使您沒有建立 About.Mobile.cshtml 檢視，[關於] 頁面會顯示使用行動配置 (\_Layout.Mobile.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="f6078-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="f6078-479">![有關頁面](whats-new-in-aspnet-mvc-4/_static/image33.png "關於頁面")</span><span class="sxs-lookup"><span data-stu-id="f6078-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="f6078-480">*關於頁面*</span><span class="sxs-lookup"><span data-stu-id="f6078-480">*About page*</span></span>
8. <span data-ttu-id="f6078-481">最後，在桌面的網頁瀏覽器中開啟網站。</span><span class="sxs-lookup"><span data-stu-id="f6078-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="f6078-482">請注意，沒有任何先前的更新已受到影響桌面檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="f6078-483">![PhotoGallery 桌面檢視](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery 桌面檢視")</span><span class="sxs-lookup"><span data-stu-id="f6078-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="f6078-484">*PhotoGallery 桌面檢視*</span><span class="sxs-lookup"><span data-stu-id="f6078-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="f6078-485">工作 6-建立新的顯示模式</span><span class="sxs-lookup"><span data-stu-id="f6078-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="f6078-486">新的顯示模式功能可讓應用程式選取視瀏覽器會將產生要求的檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="f6078-487">例如，如果桌面瀏覽器要求首頁上，應用程式會傳回**Views\Home\Index.cshtml**範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="f6078-488">然後，如果在行動瀏覽器要求首頁上，應用程式會傳回**Views\Home\Index.mobile.cshtml**範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="f6078-489">在這個工作中，您將建立自訂的版面配置，iPhone 裝置，您必須模擬要求從 iPhone 裝置。</span><span class="sxs-lookup"><span data-stu-id="f6078-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="f6078-490">若要這樣做，您可以使用任一 iPhone 模擬器/模擬器 (例如[Electric 所研發的行動電話模擬器](http://www.electricplum.com/)) 或修改使用者代理程式的附加元件的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f6078-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="f6078-491">如何在 Safari 瀏覽器中設定的使用者代理字串來模擬在 iPhone 的指示，請參閱[如何讓假裝是 IE 的 Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)給 David Alison 的部落格。</span><span class="sxs-lookup"><span data-stu-id="f6078-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="f6078-492">**請注意，這項工作是選擇性的您可以繼續到整個實驗室，而不加以執行。**</span><span class="sxs-lookup"><span data-stu-id="f6078-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="f6078-493">在 Visual Studio 中按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="f6078-494">開啟**Global.asax.cs**並新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="f6078-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="f6078-495">將下列反白顯示的程式碼新增至應用程式\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="f6078-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="f6078-496">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="f6078-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="f6078-497">您已註冊的新**DefaultDisplayMode 名為&quot;iPhone&quot;**，在靜態**DisplayModeProvider.Instance.Modes**靜態清單，會比對每個傳入的要求。</span><span class="sxs-lookup"><span data-stu-id="f6078-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="f6078-498">如果連入要求包含字串&quot;iPhone&quot;，ASP.NET MVC 會尋找其名稱包含檢視&quot;iPhone&quot;後置詞。</span><span class="sxs-lookup"><span data-stu-id="f6078-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="f6078-499">0 的參數會指出特定的是新的模式;比方說，此檢視會比一般更明確&quot;.mobile&quot;符合要求，從行動裝置的規則。</span><span class="sxs-lookup"><span data-stu-id="f6078-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="f6078-500">此程式碼執行，iPhone 瀏覽器就會產生要求之後，將使用您的應用程式**Views\Shared\\_Layout.iPhone.cshtml**您將在接下來的步驟建立的配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="f6078-501">這種測試 iPhone 已簡化，供示範之用，而且可能無法正常運作的每個 iPhone 使用者代理字串 （如範例測試會區分大小寫） 的要求方法。</span><span class="sxs-lookup"><span data-stu-id="f6078-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="f6078-502">建立一份 **\_Layout.Mobile.cshtml**中的檔案**Views\Shared**資料夾，並重新命名複製到&quot;  **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="f6078-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="f6078-503">開啟 **\_Layout.iPhone.csthml**您在上一個步驟中建立。</span><span class="sxs-lookup"><span data-stu-id="f6078-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="f6078-504">資料角色屬性設定為尋找 div 項目**網頁**並變更**資料佈景主題**屬性設定為&quot; &quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="f6078-505">現在您會在 ASP.NET MVC 4 應用程式中有 3 個版面配置：</span><span class="sxs-lookup"><span data-stu-id="f6078-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="f6078-506">**\_Layout.cshtml**： 用於桌面瀏覽器的預設版面配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="f6078-507">**\_Layout.mobile.cshtml**： 適用於行動裝置所使用的預設配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="f6078-508">**\_Layout.iPhone.cshtml**： 使用不同的色彩配置從區分的 iPhone 裝置特定配置\_Layout.mobile.cshtml。</span><span class="sxs-lookup"><span data-stu-id="f6078-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="f6078-509">按下**F5**以執行應用程式，並瀏覽中的站台**Windows Phone 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="f6078-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="f6078-510">開啟**iPhone 模擬器**(請參閱[旓紵 C](#AppendixC)如需有關如何安裝和設定 iPhone 模擬器)，而且也瀏覽至站台。</span><span class="sxs-lookup"><span data-stu-id="f6078-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="f6078-511">請注意，每個電話使用特定的範本。</span><span class="sxs-lookup"><span data-stu-id="f6078-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="f6078-513">*針對每個行動裝置使用不同的檢視*</span><span class="sxs-lookup"><span data-stu-id="f6078-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="f6078-514">練習 4： 使用非同步控制器</span><span class="sxs-lookup"><span data-stu-id="f6078-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="f6078-515">Microsoft.NET Framework 4.5 引進了新的語言功能，在 C# 和 Visual Basic.NET 程式設計中非同步提供新的基礎。</span><span class="sxs-lookup"><span data-stu-id="f6078-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="f6078-516">這個新的基礎架構可讓非同步程式設計，類似於-且大約一樣直接-同步程式設計。</span><span class="sxs-lookup"><span data-stu-id="f6078-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="f6078-517">現在您就可以使用 ASP.NET MVC 4 中撰寫非同步動作方法**AsyncController**類別。</span><span class="sxs-lookup"><span data-stu-id="f6078-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="f6078-518">您可以使用非同步動作方法的長時間執行，不受限於 CPU 的要求。</span><span class="sxs-lookup"><span data-stu-id="f6078-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="f6078-519">如此可避免在處理要求時執行工作的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6078-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="f6078-520">AsyncController 類別通常用於長時間執行的 Web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="f6078-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="f6078-521">這個練習說明 ASP.NET MVC 4 中的非同步作業的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f6078-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="f6078-522">如果您想更深入的了解，您可以查看下列文章： [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="f6078-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="f6078-523">工作 1-實作非同步控制器</span><span class="sxs-lookup"><span data-stu-id="f6078-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="f6078-524">開啟**開始**解決方案位於**來源/Ex4-非同步處理/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="f6078-525">否則，您可能會繼續使用**結束**方案取得完成前一個練習。</span><span class="sxs-lookup"><span data-stu-id="f6078-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f6078-526">如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。</span><span class="sxs-lookup"><span data-stu-id="f6078-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f6078-527">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="f6078-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f6078-528">在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。</span><span class="sxs-lookup"><span data-stu-id="f6078-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f6078-529">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="f6078-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f6078-530">使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。</span><span class="sxs-lookup"><span data-stu-id="f6078-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f6078-531">NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。</span><span class="sxs-lookup"><span data-stu-id="f6078-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f6078-532">這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="f6078-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="f6078-533">開啟**HomeController.cs**從類別**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="f6078-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="f6078-534">新增下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="f6078-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="f6078-535">更新**HomeController**類別繼承自**AsyncController**。</span><span class="sxs-lookup"><span data-stu-id="f6078-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="f6078-536">衍生自 AsyncController 的控制站可讓 ASP.NET 處理非同步要求，並仍然可以服務同步動作方法。</span><span class="sxs-lookup"><span data-stu-id="f6078-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="f6078-537">新增**非同步**關鍵字來**Index**方法，並使其傳回型別**工作&lt;ActionResult&gt;**。</span><span class="sxs-lookup"><span data-stu-id="f6078-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="f6078-538">**非同步**關鍵字是其中一個.NET Framework 4.5 提供新的關鍵字; 它會告訴編譯器設定，這個方法所包含的非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="f6078-539">A**任務**物件都代表可能在未來某個時間點在完成的非同步作業。</span><span class="sxs-lookup"><span data-stu-id="f6078-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="f6078-540">取代**用戶端。GetAsync()** 呼叫，但完整的非同步版本使用 await 關鍵字，如下所示。</span><span class="sxs-lookup"><span data-stu-id="f6078-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="f6078-541">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="f6078-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="f6078-542">在舊版中，您使用**結果**屬性從**工作**物件封鎖執行緒，直到傳回 （同步處理版本） 的結果。</span><span class="sxs-lookup"><span data-stu-id="f6078-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="f6078-543">新增**await**關鍵字會指示編譯器將以非同步方式等候工作傳回方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="f6078-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="f6078-544">這表示，其餘的程式碼會執行為回呼在等候的方法完成之後，才。</span><span class="sxs-lookup"><span data-stu-id="f6078-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="f6078-545">另外要注意的是您不需要變更您的 try / catch 區塊，若要這麼做： 在背景或前景中發生的例外狀況將仍可找出不需要任何額外的工作，使用 framework 所提供的處理常式。</span><span class="sxs-lookup"><span data-stu-id="f6078-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="f6078-546">變更繼續在非同步處理實作新的程式碼行取代成如下所示的程式碼</span><span class="sxs-lookup"><span data-stu-id="f6078-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="f6078-547">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="f6078-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="f6078-548">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-548">Run the application.</span></span> <span data-ttu-id="f6078-549">您會注意到沒有重大變更，但您的程式碼將不會封鎖執行緒，以從執行緒集區進行更好的伺服器資源使用量，並改善效能。</span><span class="sxs-lookup"><span data-stu-id="f6078-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-550">您可以深入了解在實驗室中的新非同步程式設計功能&quot;**在.NET 4.5 中以 C# 和 Visual Basic 進行非同步程式設計**&quot;包含在 Visual Studio 訓練套件。</span><span class="sxs-lookup"><span data-stu-id="f6078-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="f6078-551">工作 2-使用取消語彙基元的處理逾時</span><span class="sxs-lookup"><span data-stu-id="f6078-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="f6078-552">傳回工作的執行個體的非同步動作方法也可支援逾時。</span><span class="sxs-lookup"><span data-stu-id="f6078-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="f6078-553">在這個工作中，您將會更新索引方法程式碼來處理逾時的案例，使用取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="f6078-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="f6078-554">請返回 Visual Studio，然後按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="f6078-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="f6078-555">新增下列 using 陳述式**HomeController.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="f6078-556">更新索引動作，以接收**CancellationToken**引數。</span><span class="sxs-lookup"><span data-stu-id="f6078-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="f6078-557">更新**GetAsync**呼叫傳遞取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="f6078-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="f6078-558">(程式碼片段- *CancellationToken 與 ASP.NET MVC 4 實驗室-Ex04-SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="f6078-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="f6078-559">裝飾*Index*方法**AsyncTimeout**屬性設為 500 毫秒和**HandleError**屬性設定為處理**TaskCanceledException**重新導向至**TimedOut**檢視。</span><span class="sxs-lookup"><span data-stu-id="f6078-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="f6078-560">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-屬性*)</span><span class="sxs-lookup"><span data-stu-id="f6078-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="f6078-561">開啟**PhotoController**類別和更新**資源庫**方法來延遲執行 1000年毫秒 （1 秒），以模擬長時間執行的工作。</span><span class="sxs-lookup"><span data-stu-id="f6078-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="f6078-562">開啟**Web.config**檔案，並新增下列項目，以啟用自訂錯誤。</span><span class="sxs-lookup"><span data-stu-id="f6078-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="f6078-563">建立新的檢視，在**Views\Shared**名為**TimedOut** ，並使用預設的版面配置。</span><span class="sxs-lookup"><span data-stu-id="f6078-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="f6078-564">在 [方案總管] 中，以滑鼠右鍵按一下**Views\Shared**資料夾，然後選取**新增 |檢視**。</span><span class="sxs-lookup"><span data-stu-id="f6078-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="f6078-565">![針對每個行動裝置使用不同的檢視](whats-new-in-aspnet-mvc-4/_static/image36.png "使用針對每個行動裝置的不同檢視")</span><span class="sxs-lookup"><span data-stu-id="f6078-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="f6078-566">*針對每個行動裝置使用不同的檢視*</span><span class="sxs-lookup"><span data-stu-id="f6078-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="f6078-567">更新**TimedOut**檢視內容，如下所示。</span><span class="sxs-lookup"><span data-stu-id="f6078-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="f6078-568">執行應用程式，並瀏覽至根 URL。</span><span class="sxs-lookup"><span data-stu-id="f6078-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="f6078-569">因為您已新增**Thread.Sleep** 1000年毫秒，您會取得所產生的逾時錯誤**AsyncTimeout**屬性，並攔截**HandleError**屬性。</span><span class="sxs-lookup"><span data-stu-id="f6078-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="f6078-570">![逾時例外狀況處理](whats-new-in-aspnet-mvc-4/_static/image37.png "逾時例外狀況處理")</span><span class="sxs-lookup"><span data-stu-id="f6078-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="f6078-571">*逾時例外狀況處理*</span><span class="sxs-lookup"><span data-stu-id="f6078-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="f6078-572">此外，您可以在其中部署此應用程式以 Windows Azure 網站的下列[附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixD)。</span><span class="sxs-lookup"><span data-stu-id="f6078-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f6078-573">總結</span><span class="sxs-lookup"><span data-stu-id="f6078-573">Summary</span></span>

<span data-ttu-id="f6078-574">在這個實際操作實驗室，您觀察到的一些新功能駐留在 ASP.NET MVC 4 中。</span><span class="sxs-lookup"><span data-stu-id="f6078-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="f6078-575">已經討論過下列概念：</span><span class="sxs-lookup"><span data-stu-id="f6078-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="f6078-576">利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="f6078-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="f6078-577">使用 HTML5 檢視區屬性和 CSS 媒體查詢，以改善在行動裝置上顯示</span><span class="sxs-lookup"><span data-stu-id="f6078-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="f6078-578">使用 jQuery Mobile，漸進式增強功能，以及建置觸控最佳化的 web UI</span><span class="sxs-lookup"><span data-stu-id="f6078-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="f6078-579">建立行動專用的檢視</span><span class="sxs-lookup"><span data-stu-id="f6078-579">Create mobile-specific views</span></span>
- <span data-ttu-id="f6078-580">行動和桌面應用程式中的檢視之間切換時，用以檢視切換器元件</span><span class="sxs-lookup"><span data-stu-id="f6078-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="f6078-581">建立使用工作支援非同步控制器</span><span class="sxs-lookup"><span data-stu-id="f6078-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="f6078-582">附錄 a： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="f6078-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="f6078-583">使用程式碼片段，您會有您需要在隨手可得的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f6078-584">實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="f6078-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f6078-585">![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](whats-new-in-aspnet-mvc-4/_static/image38.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="f6078-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f6078-586">*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="f6078-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="f6078-587">***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="f6078-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="f6078-588">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f6078-589">開始輸入程式碼片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="f6078-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f6078-590">觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="f6078-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f6078-591">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="f6078-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f6078-592">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f6078-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="f6078-593">![開始鍵入程式碼片段名稱](whats-new-in-aspnet-mvc-4/_static/image39.png "開始輸入程式碼片段名稱")</span><span class="sxs-lookup"><span data-stu-id="f6078-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="f6078-594">*開始鍵入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="f6078-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="f6078-595">![按 Tab 鍵以選取反白顯示的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image40.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="f6078-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="f6078-596">*按 Tab 鍵以選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="f6078-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="f6078-597">![再次按 Tab 鍵和程式碼片段會依序展開](whats-new-in-aspnet-mvc-4/_static/image41.png "再次按 Tab 鍵和程式碼片段會展開")</span><span class="sxs-lookup"><span data-stu-id="f6078-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="f6078-598">*再次按 Tab 鍵和程式碼片段會展開*</span><span class="sxs-lookup"><span data-stu-id="f6078-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="f6078-599">***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）***</span><span class="sxs-lookup"><span data-stu-id="f6078-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="f6078-600">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="f6078-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="f6078-601">選取 **插入程式碼片段**後面**My Code Snippets**。</span><span class="sxs-lookup"><span data-stu-id="f6078-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="f6078-602">選擇相關的程式片段，從清單中，對它按一下。</span><span class="sxs-lookup"><span data-stu-id="f6078-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="f6078-603">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-aspnet-mvc-4/_static/image42.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="f6078-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="f6078-604">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="f6078-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="f6078-605">![對它按一下挑選清單中，相關程式碼片段](whats-new-in-aspnet-mvc-4/_static/image43.png "對它按一下挑選清單中，相關程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="f6078-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="f6078-606">*對它按一下挑選清單中，相關程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="f6078-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f6078-607">附錄 b： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="f6078-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f6078-608">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="f6078-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f6078-609">下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="f6078-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f6078-610">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="f6078-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f6078-611">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f6078-612">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="f6078-612">Click on **Install Now**.</span></span> <span data-ttu-id="f6078-613">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="f6078-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f6078-614">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f6078-615">![安裝 Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f6078-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f6078-616">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f6078-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f6078-617">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="f6078-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="f6078-619">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="f6078-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f6078-620">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="f6078-620">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="f6078-622">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="f6078-622">*Installation progress*</span></span>
6. <span data-ttu-id="f6078-623">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="f6078-623">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="f6078-625">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="f6078-625">*Installation completed*</span></span>
7. <span data-ttu-id="f6078-626">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="f6078-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f6078-627">若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。</span><span class="sxs-lookup"><span data-stu-id="f6078-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 圖格](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="f6078-629">*VS Express for Web 圖格*</span><span class="sxs-lookup"><span data-stu-id="f6078-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="f6078-630">附錄 c： 安裝 WebMatrix 2 和 iPhone 模擬器</span><span class="sxs-lookup"><span data-stu-id="f6078-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="f6078-631">若要模擬的 iPhone 裝置中執行您的網站中，您可以使用 WebMatrix 延伸&quot;Electric 所研發的行動裝置模擬器，在適用於 iPhone&quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="f6078-632">此外，您可以設定要執行模擬器，從 Visual Studio 2012 的相同擴充功能。</span><span class="sxs-lookup"><span data-stu-id="f6078-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="f6078-633">工作 1-安裝 WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="f6078-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="f6078-634">移至[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="f6078-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f6078-635">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>WebMatrix 2</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="f6078-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="f6078-636">按一下 **立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="f6078-636">Click on **Install Now**.</span></span> <span data-ttu-id="f6078-637">如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="f6078-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f6078-638">一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f6078-639">![安裝 WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "安裝 WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="f6078-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="f6078-640">*安裝 WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="f6078-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="f6078-641">閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。</span><span class="sxs-lookup"><span data-stu-id="f6078-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="f6078-642">![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image50.png "接受授權條款")</span><span class="sxs-lookup"><span data-stu-id="f6078-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="f6078-643">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="f6078-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f6078-644">等候完成的下載與安裝程序。</span><span class="sxs-lookup"><span data-stu-id="f6078-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="f6078-645">![Průběh instalace](whats-new-in-aspnet-mvc-4/_static/image51.png "安裝進度")</span><span class="sxs-lookup"><span data-stu-id="f6078-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="f6078-646">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="f6078-646">*Installation progress*</span></span>
6. <span data-ttu-id="f6078-647">安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="f6078-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="f6078-648">![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image52.png "安裝已完成")</span><span class="sxs-lookup"><span data-stu-id="f6078-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="f6078-649">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="f6078-649">*Installation completed*</span></span>
7. <span data-ttu-id="f6078-650">按一下 **結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="f6078-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="f6078-651">工作 2-安裝在 iPhone 模擬器延伸模組</span><span class="sxs-lookup"><span data-stu-id="f6078-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="f6078-652">執行**WebMatrix**並開啟任何現有的網站，或建立新的。</span><span class="sxs-lookup"><span data-stu-id="f6078-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="f6078-653">按一下 **執行**按鈕**Home**功能區，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6078-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="f6078-654">![加入新的 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image53.png "新增新的 WebMatrix 延伸模組")</span><span class="sxs-lookup"><span data-stu-id="f6078-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="f6078-655">*加入新的 WebMatrix 延伸模組*</span><span class="sxs-lookup"><span data-stu-id="f6078-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="f6078-656">選取  **iPhone 模擬器**然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="f6078-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="f6078-657">![瀏覽 WebMatrix 延伸模組](whats-new-in-aspnet-mvc-4/_static/image54.png "瀏覽 WebMatrix 延伸模組")</span><span class="sxs-lookup"><span data-stu-id="f6078-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="f6078-658">*瀏覽 WebMatrix 延伸模組*</span><span class="sxs-lookup"><span data-stu-id="f6078-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="f6078-659">封裝的詳細資訊，請按一下**安裝**來繼續執行延伸模組安裝。</span><span class="sxs-lookup"><span data-stu-id="f6078-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="f6078-660">![iPhone 模擬器延伸模組](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone 模擬器延伸模組")</span><span class="sxs-lookup"><span data-stu-id="f6078-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="f6078-661">*iPhone 模擬器延伸模組*</span><span class="sxs-lookup"><span data-stu-id="f6078-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="f6078-662">閱讀並接受使用者授權合約的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="f6078-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="f6078-663">![WebMatrix 延伸 EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 延伸使用者授權合約")</span><span class="sxs-lookup"><span data-stu-id="f6078-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="f6078-664">*WebMatrix 延伸使用者授權合約*</span><span class="sxs-lookup"><span data-stu-id="f6078-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="f6078-665">現在，您可以執行您的網站從 WebMatrix 使用 iPhone 模擬器選項。</span><span class="sxs-lookup"><span data-stu-id="f6078-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="f6078-666">![執行使用 iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "執行使用 iPhone")</span><span class="sxs-lookup"><span data-stu-id="f6078-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="f6078-667">*執行使用 iPhone*</span><span class="sxs-lookup"><span data-stu-id="f6078-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="f6078-668">工作 3-設定執行 iPhone 模擬器的 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6078-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="f6078-669">開啟**Visual Studio 2012**並開啟任何網站，或建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="f6078-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="f6078-670">按一下向下的箭號，從 [執行] 按鈕，然後選取**瀏覽包含**。</span><span class="sxs-lookup"><span data-stu-id="f6078-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="f6078-671">![瀏覽方式](whats-new-in-aspnet-mvc-4/_static/image58.png "瀏覽方式")</span><span class="sxs-lookup"><span data-stu-id="f6078-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="f6078-672">*瀏覽方式*</span><span class="sxs-lookup"><span data-stu-id="f6078-672">*Browse with*</span></span>
3. <span data-ttu-id="f6078-673">在 [&quot;瀏覽&quot;] 對話方塊中，按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="f6078-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="f6078-674">在 [&quot;新增程式&quot;] 對話方塊中，使用下列值：</span><span class="sxs-lookup"><span data-stu-id="f6078-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="f6078-675"><strong>計劃</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * （據以更新的路徑）</em></span><span class="sxs-lookup"><span data-stu-id="f6078-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="f6078-676">**引數**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="f6078-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="f6078-677">**易記名稱**: iPhone 模擬器</span><span class="sxs-lookup"><span data-stu-id="f6078-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="f6078-678">![新增程式](whats-new-in-aspnet-mvc-4/_static/image59.png "新增程式")</span><span class="sxs-lookup"><span data-stu-id="f6078-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="f6078-679">*新增程式，瀏覽包含*</span><span class="sxs-lookup"><span data-stu-id="f6078-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="f6078-680">按一下 **確定**並關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f6078-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="f6078-681">現在，您就能夠在 iPhone 模擬器中執行您的 Web 應用程式，從 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="f6078-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="f6078-682">![使用 iPhone 模擬器進行瀏覽](whats-new-in-aspnet-mvc-4/_static/image60.png "瀏覽與 iPhone 模擬器")</span><span class="sxs-lookup"><span data-stu-id="f6078-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="f6078-683">*使用 iPhone 模擬器進行瀏覽*</span><span class="sxs-lookup"><span data-stu-id="f6078-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f6078-684">附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f6078-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f6078-685">本附錄將說明如何從 Windows Azure 管理入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="f6078-686">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="f6078-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="f6078-687">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="f6078-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-688">使用 Windows Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。</span><span class="sxs-lookup"><span data-stu-id="f6078-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f6078-689">您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="f6078-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f6078-690">![登入 Windows Azure 入口網站](whats-new-in-aspnet-mvc-4/_static/image61.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="f6078-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f6078-691">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="f6078-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="f6078-692">按一下 **新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="f6078-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f6078-693">![建立新的 Web 站台](whats-new-in-aspnet-mvc-4/_static/image62.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="f6078-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f6078-694">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="f6078-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f6078-695">按一下 **計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="f6078-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f6078-696">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="f6078-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="f6078-697">新的網站提供可用的 URL，然後按**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="f6078-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-698">Windows Azure 網站時，您可以控制和管理雲端中執行的 web 應用程式的主應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f6078-699">[快速建立] 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="f6078-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="f6078-700">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="f6078-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f6078-701">![建立新的網站上，使用 快速建立](whats-new-in-aspnet-mvc-4/_static/image63.png "建立新的網站上，使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="f6078-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f6078-702">*建立新的網站上，使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="f6078-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f6078-703">等到新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="f6078-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f6078-704">建立網站後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="f6078-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f6078-705">檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="f6078-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f6078-706">![瀏覽至新的 web 站台](whats-new-in-aspnet-mvc-4/_static/image64.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="f6078-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f6078-707">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="f6078-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f6078-708">![執行的 web 站台](whats-new-in-aspnet-mvc-4/_static/image65.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="f6078-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="f6078-709">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="f6078-709">*Web site running*</span></span>
6. <span data-ttu-id="f6078-710">返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f6078-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f6078-711">![開啟 網站管理頁面](whats-new-in-aspnet-mvc-4/_static/image66.png "開啟的網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="f6078-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f6078-712">*開啟 網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="f6078-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f6078-713">在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="f6078-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-714">*發行設定檔*包含所有發行至 Windows Azure 網站的每個已啟用的發行方法的 web 應用程式所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="f6078-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="f6078-715">發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。</span><span class="sxs-lookup"><span data-stu-id="f6078-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f6078-716">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Windows Azure 網站的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f6078-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="f6078-717">![正在下載網站發行設定檔](whats-new-in-aspnet-mvc-4/_static/image67.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="f6078-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f6078-718">*正在下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="f6078-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f6078-719">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="f6078-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f6078-720">進一步在這個練習中，您會看到如何從 Visual Studio 的 web 應用程式至 Windows Azure 網站發行使用此檔案。</span><span class="sxs-lookup"><span data-stu-id="f6078-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="f6078-721">![儲存發行設定檔](whats-new-in-aspnet-mvc-4/_static/image68.png "儲存發佈設定檔")</span><span class="sxs-lookup"><span data-stu-id="f6078-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f6078-722">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="f6078-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f6078-723">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="f6078-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f6078-724">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="f6078-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f6078-725">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="f6078-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f6078-726">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="f6078-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f6078-727">您可以從您的訂用帳戶，在 Windows Azure Management 入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="f6078-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f6078-728">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f6078-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f6078-729">請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="f6078-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f6078-730">並未建立資料庫，因為它會建立在稍後的階段。</span><span class="sxs-lookup"><span data-stu-id="f6078-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f6078-731">![SQL Database 伺服器儀表板](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="f6078-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f6078-732">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="f6078-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f6078-733">下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="f6078-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f6078-734">若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f6078-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![新增用戶端 IP 位址](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="f6078-736">*新增用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="f6078-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f6078-737">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="f6078-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="f6078-739">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="f6078-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f6078-740">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="f6078-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f6078-741">返回至 ASP.NET MVC 4 的方案。</span><span class="sxs-lookup"><span data-stu-id="f6078-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f6078-742">在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="f6078-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f6078-743">![發行應用程式](whats-new-in-aspnet-mvc-4/_static/image73.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="f6078-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="f6078-744">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="f6078-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="f6078-745">匯入發行設定檔儲存在第一項工作。</span><span class="sxs-lookup"><span data-stu-id="f6078-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f6078-746">![匯入發行設定檔](whats-new-in-aspnet-mvc-4/_static/image74.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="f6078-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f6078-747">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="f6078-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="f6078-748">按一下 **驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="f6078-748">Click **Validate Connection**.</span></span> <span data-ttu-id="f6078-749">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="f6078-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6078-750">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="f6078-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f6078-751">![驗證連接](whats-new-in-aspnet-mvc-4/_static/image75.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="f6078-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="f6078-752">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="f6078-752">*Validating connection*</span></span>
4. <span data-ttu-id="f6078-753">在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="f6078-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f6078-754">![Web 部署組態](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="f6078-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f6078-755">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="f6078-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f6078-756">設定資料庫連線，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f6078-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f6078-757">在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="f6078-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f6078-758">在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="f6078-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f6078-759">在 **密碼**輸入您的伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="f6078-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f6078-760">輸入新的資料庫名稱，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="f6078-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f6078-761">![設定目的地連接字串](whats-new-in-aspnet-mvc-4/_static/image77.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="f6078-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f6078-762">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="f6078-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f6078-763">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="f6078-763">Then click **OK**.</span></span> <span data-ttu-id="f6078-764">當系統提示您建立資料庫時，按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="f6078-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f6078-765">![建立資料庫](whats-new-in-aspnet-mvc-4/_static/image78.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="f6078-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="f6078-766">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="f6078-766">*Creating the database*</span></span>
7. <span data-ttu-id="f6078-767">您將用來連接至 Windows Azure 中的 SQL 資料庫的連接字串會顯示在文字方塊中的預設連線。</span><span class="sxs-lookup"><span data-stu-id="f6078-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f6078-768">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="f6078-768">Then click **Next**.</span></span>

    <span data-ttu-id="f6078-769">![連接字串指向 SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "連接字串指向 SQL Database")</span><span class="sxs-lookup"><span data-stu-id="f6078-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f6078-770">*連接字串指向 SQL Database*</span><span class="sxs-lookup"><span data-stu-id="f6078-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f6078-771">在  **Preview**頁面上，按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="f6078-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f6078-772">![Web 應用程式發行](whats-new-in-aspnet-mvc-4/_static/image80.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="f6078-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="f6078-773">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="f6078-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="f6078-774">當發行程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="f6078-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f6078-775">![應用程式發行至 Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "應用程式發行至 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f6078-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f6078-776">*應用程式發佈至 Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="f6078-776">*Application published to Windows Azure*</span></span>
