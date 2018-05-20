---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 中最新消息 |Microsoft 文件
author: rick-anderson
description: ASP.NET MVC 4 是用於建置使用信譽良好的設計模式與強大的 ASP.NET 的可擴充、 以標準為基礎的 web 應用程式的架構和...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 485d2ba7a1274bbb36cfbcbca9322cecc8c8d77c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/18/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="9fe7f-103">ASP.NET MVC 4 中最新消息</span><span class="sxs-lookup"><span data-stu-id="9fe7f-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="9fe7f-104">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9fe7f-105">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="9fe7f-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9fe7f-106">ASP.NET MVC 4 是用於建置使用信譽良好的設計模式與強大的 ASP.NET 和.NET framework 的可擴充、 以標準為基礎的 web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="9fe7f-107">這個新，第四個版本的 framework 著重於讓行動裝置 web 應用程式開發更容易。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="9fe7f-108">一開始，當您建立新的 ASP.NET MVC 4 專案沒有現在可用來建置針對行動裝置的獨立應用程式的行動應用程式專案範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="9fe7f-109">此外，與 jQuery Mobile jQuery.Mobile.MVC NuGet 封裝透過整合 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="9fe7f-110">jQuery Mobile 是以 HTML5 為基礎的架構，用於開發 web 應用程式所相容的所有熱門的行動裝置平台，包括 Windows Phone、 iPhone、 Android，依此類推。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="9fe7f-111">不過，如果您需要特製化，ASP.NET MVC 4 也可讓您提供不同裝置的不同檢視，並提供裝置特定的最佳化。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="9fe7f-112">在這個實際操作實驗室中，您將啟動 ASP.NET MVC 4&quot;網際網路應用程式&quot;建立相片圖庫的應用程式的專案範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="9fe7f-113">您將漸進式增強使用 jQuery Mobile 和 ASP.NET MVC 4 的新功能，才能讓它與不同的行動裝置和桌面的網頁瀏覽器相容的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="9fe7f-114">您也將了解程式碼產生和 ASP.NET MVC 4 如何讓您更輕鬆地撰寫非同步動作方法，支援工作的新程式碼訣竅&lt;ActionResult&gt;傳回型別。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe7f-115">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9fe7f-116">這個實驗室中的特定專案將會位於[What's New in ASP.NET 4.5 Web Form](https://github.com/Microsoft-Web/HOL-ASPNETWebForms)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9fe7f-117">目標</span><span class="sxs-lookup"><span data-stu-id="9fe7f-117">Objectives</span></span>

<span data-ttu-id="9fe7f-118">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="9fe7f-119">利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="9fe7f-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="9fe7f-120">若要改善在行動裝置上的顯示使用 HTML5 檢視區屬性和 CSS 的媒體查詢</span><span class="sxs-lookup"><span data-stu-id="9fe7f-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="9fe7f-121">使用 jQuery Mobile 漸進式增強功能，以及建置觸控最佳化 web UI</span><span class="sxs-lookup"><span data-stu-id="9fe7f-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="9fe7f-122">建立行動裝置的特定檢視</span><span class="sxs-lookup"><span data-stu-id="9fe7f-122">Create mobile-specific views</span></span>
- <span data-ttu-id="9fe7f-123">使用檢視切換程式元件，將行動和桌面應用程式中的檢視之間切換</span><span class="sxs-lookup"><span data-stu-id="9fe7f-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="9fe7f-124">建立使用工作支援非同步控制器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9fe7f-125">必要條件</span><span class="sxs-lookup"><span data-stu-id="9fe7f-125">Prerequisites</span></span>

<span data-ttu-id="9fe7f-126">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9fe7f-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 B](#AppendixB)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="9fe7f-128">[ASP.NET MVC 4](../../../mvc4.md) （隨附於 Microsoft Visual Studio 2012 安裝）</span><span class="sxs-lookup"><span data-stu-id="9fe7f-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="9fe7f-129">Windows Phone 模擬器 (包含在[Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="9fe7f-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="9fe7f-130">選用- [WebMatrix 2](https://www.microsoft.com/web/webmatrix/)與**Electric Plum iPhone 模擬器**延伸模組 （僅適用於 iPhone 模擬器與瀏覽 web 應用程式的練習 3)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9fe7f-131">安裝程式</span><span class="sxs-lookup"><span data-stu-id="9fe7f-131">Setup</span></span>

<span data-ttu-id="9fe7f-132">在實驗室文件，您會指示要插入的程式碼區塊。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="9fe7f-133">為了方便起見，大部分的程式碼是依現狀 Visual Studio 程式碼片段，也可以從 Visual Studio 內使用並不必以手動方式新增。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="9fe7f-134">若要安裝的程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-134">To install the code snippets:</span></span>

1. <span data-ttu-id="9fe7f-135">開啟 Windows 檔案總管 視窗並瀏覽至實驗室**Source\Setup**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="9fe7f-136">按兩下**Setup.cmd**安裝 Visual Studio 程式碼片段的這個資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="9fe7f-137">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 a： 使用程式碼片段](#AppendixA)&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9fe7f-138">練習</span><span class="sxs-lookup"><span data-stu-id="9fe7f-138">Exercises</span></span>

<span data-ttu-id="9fe7f-139">這個實際操作實驗室包含下列練習：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="9fe7f-140">新的 ASP.NET MVC 4 專案範本</span><span class="sxs-lookup"><span data-stu-id="9fe7f-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="9fe7f-141">建立相片圖庫的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9fe7f-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="9fe7f-142">新增行動裝置的支援</span><span class="sxs-lookup"><span data-stu-id="9fe7f-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="9fe7f-143">使用非同步控制器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="9fe7f-144">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9fe7f-145">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="9fe7f-146">完成本實驗室估計時間： **60 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="9fe7f-147">練習 1： 新的 ASP.NET MVC 4 專案範本</span><span class="sxs-lookup"><span data-stu-id="9fe7f-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="9fe7f-148">在此練習中，您會瀏覽 ASP.NET MVC 4 專案範本中的增強功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="9fe7f-149">除了網際網路應用程式範本，MVC 3 中已有此版本現在包含行動應用程式的另一個範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="9fe7f-150">首先，您會看到一些相關的每個範本功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="9fe7f-151">然後，您會在使用正確的方法來呈現您的頁面在不同的平台上正確作用。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="9fe7f-152">工作 1-瀏覽網際網路應用程式範本</span><span class="sxs-lookup"><span data-stu-id="9fe7f-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="9fe7f-153">開啟**Visual Studio**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="9fe7f-154">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9fe7f-155">在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9fe7f-156">將專案命名**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-157">您稍後將會自訂 PhotoGallery ASP.NET MVC 4 方案，您現在建立。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="9fe7f-158">![建立新的專案](whats-new-in-aspnet-mvc-4/_static/image1.png "建立新的專案")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="9fe7f-159">*建立新的專案*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-159">*Creating a new project*</span></span>
3. <span data-ttu-id="9fe7f-160">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="9fe7f-161">請確定您已選取 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="9fe7f-162">![建立新 ASP.NET MVC 4 網際網路應用程式](whats-new-in-aspnet-mvc-4/_static/image2.png "建立新 ASP.NET MVC 4 網際網路應用程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="9fe7f-163">*建立新 ASP.NET MVC 4 網際網路應用程式*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-164">ASP.NET MVC 3 中已經導入 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="9fe7f-165">其目的是字元和所需在檔案中，啟用快速和程式碼撰寫工作流程的流體按鍵數目降至最低。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="9fe7f-166">Razor 運用現有的 C# / VB （或其他） 的語言能力，並傳遞可讓實用的 HTML 建構工作流程範本標記語法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="9fe7f-167">按**F5**以執行此方案，並查看更新的範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="9fe7f-168">您可以查看下列功能：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-168">You can check out the following features:</span></span>

    <span data-ttu-id="9fe7f-169">**現代化樣式範本**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-169">**Modern-style templates**</span></span>

    <span data-ttu-id="9fe7f-170">已經更新的範本，提供更多現代尋找樣式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="9fe7f-171">![ASP.NET MVC 4 重新設定樣式範本](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 重新設定樣式，範本")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="9fe7f-172">*ASP.NET MVC 4 重新設定樣式範本*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="9fe7f-173">![新的連絡人 頁面](whats-new-in-aspnet-mvc-4/_static/image4.png "新連絡人頁面")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="9fe7f-174">*新的連絡人 頁面*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-174">*New Contact page*</span></span>

    <span data-ttu-id="9fe7f-175">**調整呈現**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="9fe7f-176">簽出調整瀏覽器視窗，並注意如何頁面配置動態適應新的視窗大小。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="9fe7f-177">這些範本會使用適應性呈現技術，在桌上型電腦和行動平台，而無需自訂正確轉譯。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="9fe7f-178">![ASP.NET MVC 4 專案範本中不同的瀏覽器大小](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 專案範本中不同的瀏覽器的大小")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="9fe7f-179">*ASP.NET MVC 4 專案範本中不同的瀏覽器的大小*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="9fe7f-180">**更豐富的 UI 使用 JavaScript**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="9fe7f-181">預設專案範本的其他增強功能，就是使用 JavaScript 來提供更具互動性的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="9fe7f-182">範本中使用的登入和註冊連結提供使用 jQuery 驗證來驗證來自用戶端的輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery 驗證](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="9fe7f-184">*jQuery 驗證*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-185">請注意這兩個登入中的第一個區段的區段，您可以記錄在使用 registerd 帳戶從站台，您可以 altenativelly 使用登入 google （預設為停用） 等其他的驗證服務的第二個區段中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="9fe7f-186">關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="9fe7f-187">開啟檔案**AuthConfig.cs**位於**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="9fe7f-188">從註冊 Google 用戶端的最後一行移除註解*OAuth*驗證。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="9fe7f-189">請注意，您可以輕鬆使用類似 Facebook、 Twitter、 Microsoft 等任何 OpenID 或 OAuth 服務進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="9fe7f-190">按**F5**執行方案，並瀏覽到登入頁面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="9fe7f-191">選取**Google**服務以登入。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-191">Select **Google** service to log in.</span></span>

    ![在服務中選取記錄檔](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="9fe7f-193">*在服務中選取記錄檔*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="9fe7f-194">使用 Google 帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="9fe7f-195">允許 Google 帳戶中擷取資訊的站台 (localhost)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="9fe7f-196">最後，您必須註冊在 Google 帳戶相關聯的站台。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![將您的 Google 帳戶建立關聯](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="9fe7f-198">*將您的 Google 帳戶產生關聯*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="9fe7f-199">關閉瀏覽器以停止偵錯工具，並返回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="9fe7f-200">現在您可以瀏覽簽出由 ASP.NET MVC 4 專案範本中導入了一些其他新功能的方案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="9fe7f-201">![ASP.NET MVC 4 網際網路應用程式專案範本](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 網際網路應用程式專案範本")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="9fe7f-202">*ASP.NET MVC 4 網際網路應用程式專案範本*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="9fe7f-203">**HTML 5 標記**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="9fe7f-204">瀏覽以找出新的佈景主題標記的樣板檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="9fe7f-205">![新的範本，使用 Razor 和 HTML5 標記 About.cshtml。](whats-new-in-aspnet-mvc-4/_static/image10.png "使用 Razor 和 HTML5 標記 About.cshtml 的新範本。")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="9fe7f-206">*新的範本，使用 Razor 和 HTML5 的標記 (About.cshtml)。*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="9fe7f-207">**更新的 JavaScript 程式庫**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="9fe7f-208">ASP.NET MVC 4 預設範本現在包含 KnockoutJS，可讓您建立豐富的 JavaScript MVVM 架構和高回應性的 web 應用程式使用 JavaScript 和 HTML。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="9fe7f-209">例如在 MVC3、 jQuery 和 jQuery UI 程式庫是也包含在 ASP.NET MVC 4。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="9fe7f-210">您可以取得有關 KnockOutJS 此連結中的程式庫的詳細資訊： [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="9fe7f-211">此外，您可以了解 jQuery 和 jQuery UI 中[ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="9fe7f-212">工作 2-瀏覽 行動應用程式範本</span><span class="sxs-lookup"><span data-stu-id="9fe7f-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="9fe7f-213">ASP.NET MVC 4 可加速開發行動應用程式的網站和平板電腦瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="9fe7f-214">此範本具有相同的應用程式結構，為網際網路應用程式範本 （請注意，控制器的程式碼是幾乎完全相同），但其樣式已修改為在觸控式行動裝置中正確轉譯。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="9fe7f-215">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9fe7f-216">在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9fe7f-217">為專案名稱， **PhotoGallery.Mobile**、 選取的位置 （或保留預設值），選取&quot;將加入至方案&quot;按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="9fe7f-218">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**行動應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="9fe7f-219">請確定您已選取 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="9fe7f-220">![建立新 ASP.NET MVC 4 行動應用程式](whats-new-in-aspnet-mvc-4/_static/image11.png "建立新 ASP.NET MVC 4 行動應用程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="9fe7f-221">*建立新 ASP.NET MVC 4 行動應用程式*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="9fe7f-222">現在您可以瀏覽方案，並查看一些行動裝置版的 ASP.NET MVC 4 方案範本所引進的新功能：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="9fe7f-223">**jQuery Mobile 的程式庫**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="9fe7f-224">行動應用程式專案範本包括 jQuery 行動程式庫，也就是行動瀏覽器相容性的開放原始碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="9fe7f-225">jQuery Mobile 適用於行動瀏覽器支援 CSS 和 JavaScript 的漸進式增強功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="9fe7f-226">漸進式增強功能可讓所有瀏覽器來顯示網頁的基本內容，而只可讓最強大的瀏覽器顯示豐富的內容。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="9fe7f-227">JavaScript 和 CSS 檔案，包含在 jQuery 行動樣式，協助以容納至畫面中的內容，而不進行任何變更，在網頁標記中的行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="9fe7f-229">*範本包含 jQuery 行動程式庫*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="9fe7f-230">**HTML5 基礎標記**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="9fe7f-232">*使用 HTML5 標記、 （Login.cshtml 和 index.cshtml） 的行動應用程式範本*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="9fe7f-233">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="9fe7f-234">開啟**Windows Phone 7 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="9fe7f-235">在 phone [開始] 畫面中，開啟 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="9fe7f-236">簽出的桌面應用程式的開始位置的 URL，並從手機瀏覽至該 URL (例如`http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="9fe7f-237">現在您可以輸入登入頁面，或查看有關頁面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="9fe7f-238">請注意該網站的樣式，根據行動應用程式的新 Metro 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="9fe7f-239">ASP.NET MVC 4 專案範本會正確顯示在行動裝置，並確認頁面的所有項目可見並已啟用。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="9fe7f-240">請注意，標頭上的連結不夠大，按一下或點選。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="9fe7f-241">![專案範本在行動裝置的網頁](whats-new-in-aspnet-mvc-4/_static/image14.png "專案範本頁面，在行動裝置")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="9fe7f-242">*專案範本頁面，在行動裝置*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="9fe7f-243">新的範本也會使用**檢視區 meta 標記**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="9fe7f-244">大部分行動瀏覽器定義虛擬瀏覽器視窗的寬度或&quot;檢視區&quot;，其大於行動裝置的實際寬度。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="9fe7f-245">這可讓行動瀏覽器顯示整個網頁內虛擬顯示器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="9fe7f-246">**檢視區 meta 標記**可讓 web 開發人員在行動裝置上設定的寬度、 高度和小數位數的瀏覽器區域 **。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="9fe7f-247">行動應用程式的 ASP.NET MVC 4 範本將檢視區設定為裝置寬度 (&quot;寬度 = 裝置寬度&quot;) 中的版面配置範本 (*_layout.cshtml\_Layout.cshtml*)，如此所有頁面會有其檢視區設定的裝置的螢幕寬度。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="9fe7f-248">請注意的檢視區 meta 標記，不會變更預設瀏覽器檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="9fe7f-249">開啟 **\_Layout.cshtml**，位於**檢視 |共用**資料夾和註解的檢視區 meta 標記。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="9fe7f-250">執行應用程式中，如果不是已開啟，且簽出差異。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="9fe7f-251">![站台後的檢視區 meta 標記，標記為註解](whats-new-in-aspnet-mvc-4/_static/image15.png "後的檢視區 meta 標記，標記為註解的站台")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="9fe7f-252">*站台後的檢視區 meta 標記，標記為註解*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="9fe7f-253">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="9fe7f-254">取消註解的檢視區 meta 標記。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="9fe7f-255">工作 3-使用適應性呈現</span><span class="sxs-lookup"><span data-stu-id="9fe7f-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="9fe7f-256">在這項工作，您將學習要在同一時間而無需自訂呈現在行動裝置及網頁瀏覽器上正確的網頁另一個方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="9fe7f-257">您已經有類似用途使用檢視區的中繼標籤。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="9fe7f-258">現在您將會符合另一個功能強大的方法：*適應性呈現*。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="9fe7f-259">調整呈現是一種技術使用**CSS3 媒體查詢**自訂套用至頁面的樣式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="9fe7f-260">媒體查詢定義在樣式表，分組在特定條件下的 CSS 樣式內的條件。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="9fe7f-261">只有當條件為 true，則會將樣式套用至宣告的物件。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="9fe7f-262">調整呈現技術所提供的彈性可讓不同的裝置上顯示站台的任何自訂。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="9fe7f-263">您可以定義多您要在單一樣式表上不需要撰寫邏輯程式碼選擇樣式的樣式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="9fe7f-264">因此，它是非常簡潔的方式調整頁面樣式，因為它會減少重複的程式碼和邏輯的轉譯用途數量。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="9fe7f-265">相反地，頻寬耗用量會增加，CSS 檔案的大小可能會稍微變。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="9fe7f-266">根據使用適應性呈現技術，將會是您的站台**正常地顯示，不論瀏覽器。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="9fe7f-267">不過，您應該考慮是否載入額外的頻寬是一項考量。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe7f-268">媒體查詢的基本格式為： @media \[範圍： 所有 | 掌上型 | 列印 | 投影 | 螢幕\]([屬性： 值] 和...[屬性： 值]）</span><span class="sxs-lookup"><span data-stu-id="9fe7f-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="9fe7f-269">媒體查詢的範例： &gt;  <strong>@media所有和 (最大寬度： 1000px) 和 (最小寬度： 700px) {}:</strong> 700px 和 1000px 之間所有解析的。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="9fe7f-270"><strong>@media 螢幕和 (最小寬度： 400px) 和 (最大寬度： 700px) {...}:</strong>只針對螢幕。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="9fe7f-271">解析度必須介於 400 到 700px。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="9fe7f-272"><strong>@media 掌上型和 (最小寬度： 20em)、 螢幕以及 (最小寬度： 20em) {...}:</strong>的掌上型 （行動裝置和裝置） 和畫面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="9fe7f-273">最小寬度必須大於 20em。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="9fe7f-274">您可以找到相關資訊上[W3C 網站](http://www.w3.org/TR/css3-mediaqueries/)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="9fe7f-275">您現在將探索調整呈現的運作方式、 提升可讀性的 ASP.NET MVC 4 預設的網站範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="9fe7f-276">開啟**PhotoGallery.sln**方案，您必須建立在工作 1，並選取**PhotoGallery**專案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="9fe7f-277">按**F5**執行解決方案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="9fe7f-278">調整瀏覽器的寬度，設定 windows，一半或少於原始大小的四分之一。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="9fe7f-279">請注意，標頭中的項目會發生什麼事： 某些項目不會出現在標頭的可見區域。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="9fe7f-280">開啟<strong>Site.css</strong>檔從 Visual Studio 方案總管 中，位於<strong>內容</strong>專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="9fe7f-281">按<strong>CTRL + F</strong>開啟 Visual Studio 整合式的搜尋，和寫入<strong>@media</strong>找出<strong>CSS 媒體查詢</strong>。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="9fe7f-282">此範本中定義的媒體查詢條件適用於以這種方式： 當瀏覽器視窗大小低於**850 px**，套用的 CSS 規則可用來定義在這個媒體區塊內。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="9fe7f-283">![尋找的媒體查詢](whats-new-in-aspnet-mvc-4/_static/image16.png "尋找的媒體查詢")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="9fe7f-284">*尋找的媒體查詢*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-284">*Locating the media query*</span></span>
4. <span data-ttu-id="9fe7f-285">取代 850 中設定的最大寬度屬性值與像素**10px**，以停用適應性呈現，然後按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="9fe7f-286">返回瀏覽器，然後按**CTRL + F5**重新整理頁面與您所做的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="9fe7f-287">調整視窗的寬度時，請注意這兩個頁面中的差異。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="9fe7f-288">![在左邊，將套用頁面@media省略樣式，請在右側，樣式](whats-new-in-aspnet-mvc-4/_static/image17.png "頁面套用在左邊，@media省略樣式，請在右側，樣式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="9fe7f-289"><em>在左邊，將套用頁面@media省略樣式，請在右側，樣式</em></span><span class="sxs-lookup"><span data-stu-id="9fe7f-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="9fe7f-290">現在，讓我們查看的行動裝置上發生的狀況：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="9fe7f-291">![在左邊，將套用頁面@media省略樣式，請在右側，樣式](whats-new-in-aspnet-mvc-4/_static/image18.png "頁面套用在左邊，@media省略樣式，請在右側，樣式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="9fe7f-292"><em>在左邊，將套用頁面@media省略樣式，請在右側，樣式</em></span><span class="sxs-lookup"><span data-stu-id="9fe7f-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="9fe7f-293">雖然您會注意到使用行動裝置時不非常重要的是，變更 Web 瀏覽器中呈現網頁時的差異變得更為明顯。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="9fe7f-294">在左邊的映像中，我們可以看到的自訂樣式改善可讀性。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="9fe7f-295">適應性呈現可用於許多案例，以便更容易套用條件式樣式至網站並解決一般樣式問題很棒的方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="9fe7f-296">檢視區 meta 標記和 CSS 的媒體查詢不特定的 ASP.NET MVC 4，因此您可在任何 web 應用程式中使用這些功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="9fe7f-297">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="9fe7f-298">練習 2： 建立相片圖庫的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9fe7f-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="9fe7f-299">在此練習中，您會在相片圖庫應用程式顯示相片作用。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="9fe7f-300">您會開始使用 ASP.NET MVC 4 專案範本，然後您會加入一項功能可以從服務擷取相片，並且顯示在首頁上。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="9fe7f-301">在下列練習中，您將會更新此解決方案，以強化行動裝置顯示的方式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="9fe7f-302">工作 1-建立模擬的相片服務</span><span class="sxs-lookup"><span data-stu-id="9fe7f-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="9fe7f-303">在這項工作，您將建立相片服務將會顯示在圖庫中的內容擷取來源的模擬。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="9fe7f-304">若要這樣做，您將加入新的控制站，只會傳回 JSON 檔案，含有每張相片的資料。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="9fe7f-305">開啟**Visual Studio**如果尚未開啟。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="9fe7f-306">選取**檔案 |新 |專案**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9fe7f-307">在**新專案**對話方塊中，選取**Visual C# |Web**在左窗格中的範本樹狀結構，然後選擇  **ASP.NET MVC 4 Web 應用程式。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9fe7f-308">將專案命名**PhotoGallery**、 選取的位置 （或保留預設值），按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="9fe7f-309">或者，您可以繼續使用您現有的 ASP.NET MVC 4**網際網路應用程式**方案從**練習 1**並略過下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="9fe7f-310">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**專案範本，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="9fe7f-311">請確定您具有 Razor 檢視引擎為選取。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="9fe7f-312">在**方案總管 中**，以滑鼠右鍵按一下**應用程式\_資料**您的專案，然後選取資料夾**新增 |現有項目**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="9fe7f-313">瀏覽至**Source\Assets\App\_資料**本實驗室的資料夾並加入**Photos.json**檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="9fe7f-314">建立新的控制站名稱**PhotoController**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="9fe7f-315">若要這樣做，以滑鼠右鍵按一下**控制器**資料夾中，移至**新增**選取**控制站。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="9fe7f-316">完成控制站名稱，請將保留**空白的 MVC 控制器**範本，然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="9fe7f-317">![加入 PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "加入 PhotoController")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="9fe7f-318">*加入 PhotoController*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="9fe7f-319">取代**索引**以下列方法**圖庫**動作，並傳回內容最近加入至專案的 JSON 檔案中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="9fe7f-320">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-圖庫動作*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="9fe7f-321">按**F5**執行方案，並瀏覽至下列 URL 以測試模擬的相片服務： `http://localhost:[port]/photo/gallery` （取決於應用程式的啟動位置目前的連接埠的 [連接埠] 值）。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="9fe7f-322">此 URL 的要求應該擷取內容的**Photos.json**檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="9fe7f-323">![測試模擬的相片服務](whats-new-in-aspnet-mvc-4/_static/image20.png "測試模擬的相片服務")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="9fe7f-324">*測試模擬的相片服務*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="9fe7f-325">您可以在實際的實作使用[ASP.NET Web API](../../../../web-api/index.md)實作相片圖庫服務。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="9fe7f-326">ASP.NET Web API 是一種架構，可讓您更輕鬆地建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="9fe7f-327">ASP.NET Web 應用程式開發介面是在 .NET Framework 上建置 RESTful 應用程式的理想平台。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="9fe7f-328">工作 2-顯示相片圖庫</span><span class="sxs-lookup"><span data-stu-id="9fe7f-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="9fe7f-329">在這項工作，您將更新以顯示相片圖庫會使用您在此練習中的第一個工作中建立的模擬的服務的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="9fe7f-330">您會加入模型檔案，以及更新組件庫檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="9fe7f-331">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="9fe7f-332">建立**相片**類別**模型**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="9fe7f-333">若要這樣做，以滑鼠右鍵按一下**模型**資料夾中，選取**新增**按一下**類別**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="9fe7f-334">然後，將名稱設定為**Photo.cs**按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="9fe7f-335">將下列成員加入**相片**類別。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="9fe7f-336">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片模型*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="9fe7f-337">從 **Controllers** 資料夾中，開啟 **HomeController.cs** 檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="9fe7f-338">使用陳述式加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-338">Add the following using statements.</span></span>

    <span data-ttu-id="9fe7f-339">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-HomeController Using*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="9fe7f-340">更新**索引**動作使用**HttpClient**圖庫及擷取資料，然後使用**JavaScriptSerializer**其還原序列化至檢視模型。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="9fe7f-341">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-索引動作*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="9fe7f-342">開啟**Index.cshtml**檔案位於**Views\Home**資料夾，然後取代為下列程式碼的所有內容。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="9fe7f-343">此程式碼執行迴圈，從服務擷取的所有相片，並顯示為未排序清單。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="9fe7f-344">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex02-相片清單*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="9fe7f-345">在**方案總管 中**，以滑鼠右鍵按一下**內容**您的專案，然後選取資料夾**新增 |現有項目**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="9fe7f-346">瀏覽至**Source\Assets\Content**本實驗室的資料夾並加入**Site.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="9fe7f-347">您必須確認其取代。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="9fe7f-348">如果您有**Site.css**檔案開啟，您必須確認也重新載入檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="9fe7f-349">開啟檔案總管，並複製整個**相片**資料夾位於下面**Source\Assets**本實驗室中 [方案總管] 中的專案根資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="9fe7f-350">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-350">Run the application.</span></span> <span data-ttu-id="9fe7f-351">您現在應該會看到顯示相片圖庫中的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="9fe7f-352">![相片圖庫](whats-new-in-aspnet-mvc-4/_static/image21.png "相片圖庫")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="9fe7f-353">*相片圖庫*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="9fe7f-354">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="9fe7f-355">練習 3： 新增行動裝置的支援</span><span class="sxs-lookup"><span data-stu-id="9fe7f-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="9fe7f-356">在 ASP.NET MVC 4 中的金鑰更新的其中一個是行動應用程式開發的支援。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="9fe7f-357">在這個練習中，您將藉由擴充 PhotoGallery 方案，您已經在上一個練習建立探索行動應用程式的 ASP.NET MVC 4 新功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="9fe7f-358">工作 1-安裝 ASP.NET MVC 4 應用程式中的 jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="9fe7f-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="9fe7f-359">開啟**開始**方案位於**來源/Ex3MobileSupport/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="9fe7f-360">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9fe7f-361">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9fe7f-362">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9fe7f-363">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9fe7f-364">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9fe7f-365">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9fe7f-366">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9fe7f-367">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9fe7f-368">開啟**Package Manager Console**按一下**工具** &gt; **程式庫套件管理員** &gt; **封裝管理員主控台**功能表選項。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-368">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="9fe7f-369">![開啟 NuGet 封裝管理員主控台](whats-new-in-aspnet-mvc-4/_static/image22.png "開啟 NuGet 封裝管理員主控台")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="9fe7f-370">*開啟 NuGet 封裝管理員主控台*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="9fe7f-371">Package Manager Console 中執行下列命令以安裝**jQuery.Mobile.MVC**封裝。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="9fe7f-372">jQuery Mobile 是開放原始碼程式庫建置觸控最佳化 web UI。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="9fe7f-373">JQuery.Mobile.MVC NuGet 套件包含 ASP.NET MVC 4 應用程式中使用 jQuery Mobile 的協助程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-374">藉由執行下列命令，您將 jQuery.Mobile.MVC 程式庫從下載 Nuget。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="9fe7f-375">PM</span><span class="sxs-lookup"><span data-stu-id="9fe7f-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="9fe7f-376">此命令會安裝 jQuery Mobile 和一些協助程式檔案，包括下列：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="9fe7f-377">**Views/Shared/\_Layout.Mobile.cshtml**： 為 jQuery Mobile 架構版面配置最佳化的較小的螢幕。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="9fe7f-378">當網站從行動裝置瀏覽器收到要求時，它會取代原始的版面配置 (\_Layout.cshtml) 換成這一個。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="9fe7f-379">檢視切換程式元件： 組成**Views/Shared/\_ViewSwitcher.cshtml**部分檢視和**ViewSwitcherController.cs**控制站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="9fe7f-380">此元件會顯示行動瀏覽器可讓使用者切換到桌面版本 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="9fe7f-381">![相片圖庫的行動支援專案](whats-new-in-aspnet-mvc-4/_static/image23.png "相片圖庫專案使用的行動支援")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="9fe7f-382">*相片圖庫專案使用的行動支援*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="9fe7f-383">註冊行動裝置的組合。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-383">Register the Mobile bundles.</span></span> <span data-ttu-id="9fe7f-384">若要這樣做，請開啟**Global.asax.cs**檔案，然後加入下列一行。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="9fe7f-385">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-註冊行動裝置組合*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="9fe7f-386">執行使用桌面的網頁瀏覽器的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="9fe7f-387">開啟**Windows Phone 7 模擬器，** 位於**開始功能表 |所有程式 |Windows Phone SDK 7.1 |Windows Phone 模擬器。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="9fe7f-388">在 phone [開始] 畫面中，開啟 Internet Explorer。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="9fe7f-389">簽出啟動應用程式的 URL，並瀏覽至該電話瀏覽器的 URL (例如`http://localhost:[PortNumber]/`)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="9fe7f-390">您會注意到您的應用程式會尋找在 Windows Phone 模擬器中，不同 jQuery.Mobile.MVC 已顯示針對行動裝置最佳化的檢視之專案中建立新的資產。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="9fe7f-391">請注意上方的電話，顯示 切換到桌面檢視連結的訊息。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="9fe7f-392">此外，  **\_Layout.Mobile.cshtml**由您已安裝的封裝所建立的配置包括應用程式中不同的版面配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-393">目前為止，沒有任何連結回到行動檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="9fe7f-394">它將會包含在較新版本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-394">It will be included in later versions.</span></span>

    <span data-ttu-id="9fe7f-395">![相片圖庫首頁的行動檢視](whats-new-in-aspnet-mvc-4/_static/image24.png "相片圖庫首頁的行動檢視")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="9fe7f-396">*相片圖庫首頁的行動檢視*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="9fe7f-397">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="9fe7f-398">工作 2-建立行動檢視</span><span class="sxs-lookup"><span data-stu-id="9fe7f-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="9fe7f-399">在這項工作，您將建立索引檢視的行動裝置版的行動裝置的較佳 appareance 調整的內容。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="9fe7f-400">複製**Views\Home\Index.cshtml**檢視，並將它貼到 建立複本，請重新命名新的檔案， **Index.Mobile.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="9fe7f-401">開啟新建立**Index.Mobile.cshtml**檢視，並取代現有&lt;ul&gt;標記這個程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="9fe7f-402">如此一來，您將會更新&lt;ul&gt;使用從 jQuery 行動的佈景主題的 jQuery 行動資料的註解的標記。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="9fe7f-403">請注意：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="9fe7f-404">**資料角色**屬性設為**listview**會呈現使用 listview 樣式的清單。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="9fe7f-405">**資料內凹**屬性設為 true 會顯示具有圓角的框線和邊界的清單。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="9fe7f-406">**資料篩選**屬性設為**true**會產生在搜尋方塊。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="9fe7f-407">您可以深入了解專案文件中的 jQuery 行動慣例： [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="9fe7f-408">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="9fe7f-409">切換至**Windows Phone 模擬器**和重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="9fe7f-410">請注意新的外觀與風格的組件庫 清單中，為新的搜尋方塊位於最上方。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="9fe7f-411">然後，在 [搜尋] 方塊中輸入文字 (例如， **Tulips**) 才會開始搜尋相片圖庫中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="9fe7f-412">![使用已篩選清單檢視樣式的組件庫](whats-new-in-aspnet-mvc-4/_static/image25.png "使用 listview 樣式已篩選的組件庫")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="9fe7f-413">*使用已篩選清單檢視樣式的組件庫*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="9fe7f-414">若要總括而言，您已使用的檢視 Mobilizer 配方來建立一份索引檢視與&quot;行動&quot;後置詞。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="9fe7f-415">這個後置詞表示 ASP.NET MVC 4 行動裝置所產生的每個要求會使用這份索引。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="9fe7f-416">此外，您已更新索引檢視，可以提升網站的外觀與風格，行動裝置的使用 jQuery Mobile 的行動裝置的版本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="9fe7f-417">返回 Visual Studio] 和 [開啟**Site.Mobile.css**位於**內容**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="9fe7f-418">修正相片標題，讓它顯示在右側的映像的位置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="9fe7f-419">若要這樣做，請將加入下列程式碼加入**Site.Mobile.css**檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="9fe7f-420">CSS</span><span class="sxs-lookup"><span data-stu-id="9fe7f-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="9fe7f-421">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="9fe7f-422">切換回**Windows Phone 模擬器**和重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="9fe7f-423">請注意現在正確置於相片標題。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="9fe7f-424">![標題位於影像右側](whats-new-in-aspnet-mvc-4/_static/image26.png "放置影像右側的標題")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="9fe7f-425">*標題位於右側的映像*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="9fe7f-426">工作 3-jQuery Mobile 佈景主題</span><span class="sxs-lookup"><span data-stu-id="9fe7f-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="9fe7f-427">每個配置和 jQuery Mobile 中的小工具會針對新物件導向 CSS 的架構，可讓完整統一的視覺化設計佈景主題套用至站台和應用程式而設計。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="9fe7f-428">jQuery Mobile 預設佈景主題包含 5 個樣本指定字母 (a、 b、 c、 d，e） 以供快速參考。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="9fe7f-429">在這項工作，您將更新要使用與預設值不同的佈景主題的行動配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="9fe7f-430">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="9fe7f-431">開啟 **\_Layout.Mobile.cshtml**檔案位於 **_layout.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="9fe7f-432">尋找 div 項目設定為資料角色&quot;頁面&quot;並更新**資料佈景主題**至&quot; **e**&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="9fe7f-433">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="9fe7f-434">重新整理中的站台**Windows Phone 模擬器**，並注意新的色彩配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="9fe7f-435">![行動裝置的版面配置與不同的色彩配置](whats-new-in-aspnet-mvc-4/_static/image27.png "行動的版面配置與不同的色彩配置")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="9fe7f-436">*行動裝置的版面配置與不同的色彩配置*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="9fe7f-437">工作 4-使用檢視切換程式元件和瀏覽器覆寫功能</span><span class="sxs-lookup"><span data-stu-id="9fe7f-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="9fe7f-438">行動裝置最佳化網頁的慣例是頁面的加入連結，其文字是頁面的像桌面檢視或完整的網站模式可讓使用者切換到桌面版本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="9fe7f-439">JQuery.Mobile.MVC 封裝包含的範例，**檢視切換程式**元件用於此用途 **\_Layout.Mobile.cshtml**檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="9fe7f-440">![連結切換到桌面檢視](whats-new-in-aspnet-mvc-4/_static/image28.png "連結切換到桌面檢視")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="9fe7f-441">*若要切換到桌面檢視連結*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="9fe7f-442">檢視切換程式會使用新功能，稱為**瀏覽器覆寫**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="9fe7f-443">這項功能可讓您的應用程式要求視為就像是來自它們實際上來自於不同瀏覽器 （使用者代理程式）。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="9fe7f-444">在這個工作中，您將探索加入 jQuery.Mobile.MVC 和新的瀏覽器覆寫功能的 ASP.NET MVC 4 檢視切換器的範例實作。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="9fe7f-445">切換回 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="9fe7f-446">開啟 **\_Layout.Mobile.cshtml**檢視位於 **_layout.cshtml**資料夾，請注意為部分檢視所參考的檢視切換程式元件。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="9fe7f-447">![使用檢視切換程式元件的版面配置行動](whats-new-in-aspnet-mvc-4/_static/image29.png "行動使用檢視切換程式元件的版面配置")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="9fe7f-448">*行動裝置使用檢視切換程式元件的版面配置*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="9fe7f-449">開啟 **\_ViewSwitcher.cshtml**部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="9fe7f-450">部分檢視會使用新的方法**ViewContext.HttpContext.GetOverriddenBrowser()** 判斷 web 要求的來源，並顯示對應的連結，以切換至 桌面 或 行動檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="9fe7f-451">**GetOverridenBrowser**方法會傳回**HttpBrowserCapabilitiesBase**對應於目前針對要求設定的使用者代理程式執行個體 （實際或覆寫）。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="9fe7f-452">您可以使用此值來取得屬性，例如**IsMobileDevice**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="9fe7f-453">![ViewSwitcher 部分檢視](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher 部分檢視")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="9fe7f-454">*ViewSwitcher 部分檢視*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="9fe7f-455">開啟**ViewSwitcherController.cs**類別位於**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="9fe7f-456">簽出該 SwitchView 動作稱為 ViewSwitcher 元件中的連結，並注意新的 HttpContext 方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="9fe7f-457">**HttpContext.ClearOverridenBrowser()** 方法會移除目前要求的任何覆寫的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="9fe7f-458">**HttpContext.SetOverridenBrowser()** 方法會覆寫使用指定的使用者代理程式要求的實際使用者代理程式值。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="9fe7f-459">![ViewSwitcher 控制器](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher 控制器")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="9fe7f-460">*ViewSwitcher 控制器*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="9fe7f-461">瀏覽器正在覆寫是核心功能的 ASP.NET MVC 4，這也會提供即使您沒有安裝 jQuery.Mobile.MVC 封裝。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="9fe7f-462">不過，這項功能會影響檢視、 配置和部分檢視，而且不會影響任何相依於 Request.Browser 物件的功能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="9fe7f-463">工作 5-在 [桌面] 檢視中加入檢視切換程式</span><span class="sxs-lookup"><span data-stu-id="9fe7f-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="9fe7f-464">在這項工作，您將更新以包含檢視切換程式桌面的配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="9fe7f-465">這可讓行動使用者返回行動檢視瀏覽桌面檢視時。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="9fe7f-466">重新整理中的站台**Windows Phone 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="9fe7f-467">按一下**桌面檢視**頂端的組件庫的連結。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="9fe7f-468">請注意在桌面的檢視，以讓您回到 [行動] 檢視中沒有任何檢視切換程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="9fe7f-469">返回 Visual Studio] 和 [開啟 **\_Layout.cshtml**檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="9fe7f-470">尋找登入區段，然後插入要呈現的呼叫 **\_ViewSwitcher**下方的部分檢視 **\_LogOnPartial**部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="9fe7f-471">然後按下**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="9fe7f-472">按**CTRL + S**儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="9fe7f-473">重新整理頁面，在 Windows Phone 模擬器中的，按兩下 放大螢幕。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="9fe7f-474">請注意，現在顯示在首頁上**行動檢視**從行動切換到桌面檢視的連結。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="9fe7f-475">![檢視在桌面檢視中呈現的切換器](whats-new-in-aspnet-mvc-4/_static/image32.png "在桌面檢視中呈現的檢視切換程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="9fe7f-476">*在桌面檢視中呈現的檢視切換程式*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="9fe7f-477">再次切換至行動裝置的檢視，並瀏覽至<strong>有關</strong>頁面 (http://localhost[port] 首頁/有關)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="9fe7f-478">請注意，即使您沒有建立 About.Mobile.cshtml 檢視，[關於] 頁面會顯示使用行動裝置的配置 (\_Layout.Mobile.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="9fe7f-479">![有關頁面](whats-new-in-aspnet-mvc-4/_static/image33.png "關於頁面")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="9fe7f-480">*有關頁面*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-480">*About page*</span></span>
8. <span data-ttu-id="9fe7f-481">最後，桌面的網頁瀏覽器中開啟網站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="9fe7f-482">請注意，沒有任何先前的更新已受到影響桌面檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="9fe7f-483">![PhotoGallery 桌面檢視](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery 桌面檢視")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="9fe7f-484">*PhotoGallery 桌面檢視*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="9fe7f-485">工作 6-建立新的顯示模式</span><span class="sxs-lookup"><span data-stu-id="9fe7f-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="9fe7f-486">新的顯示模式功能可讓應用程式選取依產生要求的瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="9fe7f-487">例如，如果桌面瀏覽器要求在首頁上，應用程式會傳回**Views\Home\Index.cshtml**範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="9fe7f-488">然後，如果行動瀏覽器要求在首頁上，應用程式會傳回**Views\Home\Index.mobile.cshtml**範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="9fe7f-489">在這項工作，您將建立的自訂版面配置的 iPhone 裝置，而且您必須以模擬來自 iPhone 裝置的要求。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="9fe7f-490">若要這樣做，您可以使用任一 iPhone 模擬器/模擬器 (像是[電動行動模擬器](http://www.electricplum.com/)) 或修改使用者代理程式的附加元件的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="9fe7f-491">模擬在 iPhone 上 Safari 瀏覽器中設定的使用者代理字串的方式指示，請參閱[如何讓 Safari 假裝其 IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison 部落格中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="9fe7f-492">**請注意這項工作是選擇性，而且您可以繼續在實驗室，而不加以執行。**</span><span class="sxs-lookup"><span data-stu-id="9fe7f-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="9fe7f-493">在 Visual Studio 中，按**SHIFT** + **F5**停止偵錯應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="9fe7f-494">開啟**Global.asax.cs**並加入下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="9fe7f-495">將下列反白顯示的程式碼加入至應用程式\_Start 方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="9fe7f-496">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex03-iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="9fe7f-497">您註冊新**DefaultDisplayMode 名為&quot;iPhone&quot;**，靜態內**DisplayModeProvider.Instance.Modes**靜態清單，會比對每個傳入要求。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="9fe7f-498">如果傳入的要求包含字串&quot;iPhone&quot;，ASP.NET MVC 會尋找其名稱包含檢視&quot;iPhone&quot;後置詞。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="9fe7f-499">0 的參數會指出特定的是新的模式;例如，此檢視會比一般更特定&quot;.mobile&quot;規則符合來自行動裝置的要求。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="9fe7f-500">執行此程式碼，當 iPhone 瀏覽器產生的要求之後，您的應用程式都會使用 **_layout.cshtml\\_Layout.iPhone.cshtml**版面配置，您將在下一個步驟中建立。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="9fe7f-501">這種測試要求 iPhone 已簡化為示範用途，並且可能會無法正常運作的每個 iPhone 使用者代理字串 （如需範例測試會區分大小寫） 的方式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="9fe7f-502">建立一份 **\_Layout.Mobile.cshtml**檔案 **_layout.cshtml**資料夾，並重新命名複製到&quot;  **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="9fe7f-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="9fe7f-503">開啟 **\_Layout.iPhone.csthml**您在上一個步驟中建立。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="9fe7f-504">尋找 div 項目資料角色屬性設定為**頁面**並變更**資料佈景主題**屬性&quot; &quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="9fe7f-505">現在您會在 ASP.NET MVC 4 應用程式中有 3 配置：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="9fe7f-506">**\_Layout.cshtml**： 的桌面瀏覽器所使用的預設配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="9fe7f-507">**\_Layout.mobile.cshtml**： 使用行動裝置的預設配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="9fe7f-508">**\_Layout.iPhone.cshtml**： 使用不同的色彩配置從區分的 iPhone 裝置的特定配置\_Layout.mobile.cshtml。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="9fe7f-509">按**F5**執行應用程式，然後瀏覽的網站中**Windows Phone 模擬器**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="9fe7f-510">開啟**iPhone 模擬器**(請參閱[旓紵 C](#AppendixC)如需有關如何安裝及設定 iPhone 模擬器指示)，並瀏覽至站台太。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="9fe7f-511">請注意，每個電話使用特定範本。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="9fe7f-513">*使用每個行動裝置的不同檢視*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="9fe7f-514">練習 4： 使用非同步控制器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="9fe7f-515">Microsoft.NET Framework 4.5 引進了新的語言功能，在 C# 和 Visual Basic.NET 程式設計中非同步提供新的基礎架構。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="9fe7f-516">此新基礎架構，會使非同步程式設計類似於-大約像是那樣-同步程式設計。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="9fe7f-517">您現在已可使用 ASP.NET MVC 4 撰寫非同步動作方法**AsyncController**類別。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="9fe7f-518">您可以使用非同步動作方法的長時間執行、 不受限於 CPU 的要求。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="9fe7f-519">如此可避免在處理要求時執行工作的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="9fe7f-520">AsyncController 類別通常用於長時間執行的 Web 服務呼叫。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="9fe7f-521">這個練習說明 ASP.NET MVC 4 中的非同步作業的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="9fe7f-522">如果您想深入探討，您可以查看下列文章： [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="9fe7f-523">工作 1-實作非同步控制器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="9fe7f-524">開啟**開始**方案位於**來源/Ex4 非同步處理/開始/** 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="9fe7f-525">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9fe7f-526">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9fe7f-527">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9fe7f-528">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9fe7f-529">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9fe7f-530">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9fe7f-531">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9fe7f-532">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9fe7f-533">開啟**HomeController.cs**類別從**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="9fe7f-534">加入下列 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="9fe7f-535">更新**HomeController**類別繼承自**AsyncController**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="9fe7f-536">衍生自 AsyncController 的控制站會啟用 ASP.NET 處理非同步要求，並仍然可以服務同步動作方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="9fe7f-537">新增**非同步**關鍵字**索引**方法，並將其傳回型別**工作&lt;ActionResult&gt;**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="9fe7f-538">**非同步**關鍵字是其中一個.NET Framework 4.5 提供新的關鍵字; 它會告訴編譯器，這個方法包含非同步程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="9fe7f-539">A**工作**物件都代表在某個時間點可能在未來完成的非同步作業。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="9fe7f-540">取代**用戶端。GetAsync()** 呼叫具有完整的非同步版本使用 await 關鍵字，如下所示。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="9fe7f-541">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="9fe7f-542">在先前版本中，您已使用**結果**屬性從**工作**封鎖執行緒，直到結果會傳回 （同步處理版本） 的物件。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="9fe7f-543">加入**await**關鍵字會指示編譯器將以非同步方式等候工作在方法呼叫傳回。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="9fe7f-544">這表示，程式碼的其餘部分則會執行當做回呼只有等候的方法完成之後。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="9fe7f-545">另一個要注意的一點是您不需要變更 try-catch 區塊，才能執行這項工作： 在背景或前景，就可能發生的例外狀況依然可以攔截而無需任何額外的工作，使用 framework 所提供的處理常式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="9fe7f-546">程式碼，以繼續新的程式碼行取代成如下所示的非同步實作</span><span class="sxs-lookup"><span data-stu-id="9fe7f-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="9fe7f-547">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="9fe7f-548">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-548">Run the application.</span></span> <span data-ttu-id="9fe7f-549">您會發現沒有重大變更，但您的程式碼不會封鎖執行緒的執行緒集區進行更好的使用方式的伺服器資源，並改善效能。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-550">您可以進一步了解在實驗室中的新非同步程式設計功能&quot;**以 C# 和 Visual Basic.NET 4.5 中進行非同步程式設計**&quot;包含在 Visual Studio 訓練套件。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="9fe7f-551">工作 2-處理逾時，使用取消語彙基元</span><span class="sxs-lookup"><span data-stu-id="9fe7f-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="9fe7f-552">傳回工作執行個體的非同步動作方法也可支援逾時。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="9fe7f-553">在這項工作，您將會更新索引方法的程式碼來處理逾時案例中使用取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="9fe7f-554">返回 Visual Studio 並再按**SHIFT + F5**停止偵錯。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="9fe7f-555">加入下列 using 陳述式來**HomeController.cs**檔案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="9fe7f-556">更新要接收的索引動作**CancellationToken**引數。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="9fe7f-557">更新**GetAsync**呼叫傳遞取消語彙基元。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="9fe7f-558">(程式碼片段- *CancellationToken 與 ASP.NET MVC 4 實驗室-Ex04-SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="9fe7f-559">裝飾*索引*方法**AsyncTimeout**屬性設為 500 毫秒和**HandleError**屬性設定為處理**TaskCanceledException**重新導向至**TimedOut**檢視。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="9fe7f-560">(程式碼片段- *ASP.NET MVC 4 實驗室-Ex04-屬性*)</span><span class="sxs-lookup"><span data-stu-id="9fe7f-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="9fe7f-561">開啟**PhotoController**類別和更新**圖庫**延遲執行 1000年毫秒 （1 秒），以模擬長時間執行工作的方法。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="9fe7f-562">開啟**Web.config**檔案，並新增下列項目，以啟用自訂錯誤。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="9fe7f-563">建立新的檢視中 **_layout.cshtml**名為**TimedOut**並使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="9fe7f-564">在 [方案總管] 中，以滑鼠右鍵按一下 **_layout.cshtml**資料夾，然後選取**新增 |檢視**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="9fe7f-565">![使用每個行動裝置的不同檢視](whats-new-in-aspnet-mvc-4/_static/image36.png "使用每個行動裝置的不同檢視")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="9fe7f-566">*使用每個行動裝置的不同檢視*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="9fe7f-567">更新**TimedOut**檢視內容，如下所示。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="9fe7f-568">執行應用程式，並瀏覽至根 URL。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="9fe7f-569">當您加入**Thread.Sleep** 1000年毫秒就會逾時錯誤，所產生**AsyncTimeout**屬性，並攔截的**HandleError**屬性。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="9fe7f-570">![逾時例外狀況處理](whats-new-in-aspnet-mvc-4/_static/image37.png "逾時例外狀況處理")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="9fe7f-571">*逾時例外狀況處理*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="9fe7f-572">此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixD)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9fe7f-573">總結</span><span class="sxs-lookup"><span data-stu-id="9fe7f-573">Summary</span></span>

<span data-ttu-id="9fe7f-574">在這個實際操作實驗室，您所觀察到的一些新功能在 ASP.NET MVC 4 中駐留。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="9fe7f-575">已經討論過下列概念：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="9fe7f-576">利用 ASP.NET MVC 專案範本包括新的行動應用程式專案範本的增強功能</span><span class="sxs-lookup"><span data-stu-id="9fe7f-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="9fe7f-577">若要改善在行動裝置上的顯示使用 HTML5 檢視區屬性和 CSS 的媒體查詢</span><span class="sxs-lookup"><span data-stu-id="9fe7f-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="9fe7f-578">使用 jQuery Mobile 漸進式增強功能，以及建置觸控最佳化 web UI</span><span class="sxs-lookup"><span data-stu-id="9fe7f-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="9fe7f-579">建立行動裝置的特定檢視</span><span class="sxs-lookup"><span data-stu-id="9fe7f-579">Create mobile-specific views</span></span>
- <span data-ttu-id="9fe7f-580">使用檢視切換程式元件，將行動和桌面應用程式中的檢視之間切換</span><span class="sxs-lookup"><span data-stu-id="9fe7f-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="9fe7f-581">建立使用工作支援非同步控制器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="9fe7f-582">附錄 a： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="9fe7f-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="9fe7f-583">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9fe7f-584">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9fe7f-585">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](whats-new-in-aspnet-mvc-4/_static/image38.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9fe7f-586">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9fe7f-587">***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="9fe7f-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9fe7f-588">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9fe7f-589">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9fe7f-590">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9fe7f-591">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9fe7f-592">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9fe7f-593">![開始輸入程式碼片段名稱](whats-new-in-aspnet-mvc-4/_static/image39.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9fe7f-594">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="9fe7f-595">![按 Tab 鍵選取反白顯示的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image40.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9fe7f-596">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9fe7f-597">![按 Tab 鍵一次程式碼片段就會擴展](whats-new-in-aspnet-mvc-4/_static/image41.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9fe7f-598">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9fe7f-599">***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）***</span><span class="sxs-lookup"><span data-stu-id="9fe7f-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="9fe7f-600">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="9fe7f-601">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="9fe7f-602">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9fe7f-603">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-aspnet-mvc-4/_static/image42.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9fe7f-604">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9fe7f-605">![透過在按一下挑選清單中，從相關的程式碼片段](whats-new-in-aspnet-mvc-4/_static/image43.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9fe7f-606">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9fe7f-607">附錄 b： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="9fe7f-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9fe7f-608">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9fe7f-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9fe7f-609">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9fe7f-610">移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9fe7f-611">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9fe7f-612">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-612">Click on **Install Now**.</span></span> <span data-ttu-id="9fe7f-613">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9fe7f-614">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9fe7f-615">![安裝 Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9fe7f-616">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9fe7f-617">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="9fe7f-619">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9fe7f-620">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-620">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="9fe7f-622">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-622">*Installation progress*</span></span>
6. <span data-ttu-id="9fe7f-623">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-623">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="9fe7f-625">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-625">*Installation completed*</span></span>
7. <span data-ttu-id="9fe7f-626">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9fe7f-627">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="9fe7f-629">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="9fe7f-630">附錄 c： 安裝 WebMatrix 2 和 iPhone 模擬器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="9fe7f-631">在模擬的 iPhone 裝置中執行您的網站中，您可以使用 WebMatrix 延伸&quot;電動適用於 iPhone 的行動模擬器&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="9fe7f-632">此外，您可以設定要從 Visual Studio 2012 中執行的模擬器相同的副檔名。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="9fe7f-633">工作 1-安裝 WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="9fe7f-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="9fe7f-634">移至[ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9fe7f-635">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>WebMatrix 2</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="9fe7f-636">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-636">Click on **Install Now**.</span></span> <span data-ttu-id="9fe7f-637">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9fe7f-638">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9fe7f-639">![安裝 WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "安裝 WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="9fe7f-640">*安裝 WebMatrix 2*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="9fe7f-641">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="9fe7f-642">![接受授權條款](whats-new-in-aspnet-mvc-4/_static/image50.png "接受授權條款")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="9fe7f-643">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9fe7f-644">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="9fe7f-645">![安裝進度](whats-new-in-aspnet-mvc-4/_static/image51.png "安裝進度")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="9fe7f-646">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-646">*Installation progress*</span></span>
6. <span data-ttu-id="9fe7f-647">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="9fe7f-648">![安裝已完成](whats-new-in-aspnet-mvc-4/_static/image52.png "安裝已完成")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="9fe7f-649">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-649">*Installation completed*</span></span>
7. <span data-ttu-id="9fe7f-650">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="9fe7f-651">工作 2-安裝在 iPhone 模擬器延伸模組</span><span class="sxs-lookup"><span data-stu-id="9fe7f-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="9fe7f-652">執行**WebMatrix**和開啟任何現有的網站或另外新建一個。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="9fe7f-653">按一下**執行**按鈕**首頁**功能區，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="9fe7f-654">![加入新的 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image53.png "加入新的 WebMatrix 延伸模組")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="9fe7f-655">*加入新的 WebMatrix 延伸模組*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="9fe7f-656">選取**iPhone 模擬器**按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="9fe7f-657">![瀏覽 WebMatrix 延伸](whats-new-in-aspnet-mvc-4/_static/image54.png "瀏覽 WebMatrix 延伸模組")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="9fe7f-658">*瀏覽 WebMatrix 延伸模組*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="9fe7f-659">封裝的詳細資訊，請按一下**安裝**繼續擴充功能安裝。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="9fe7f-660">![iPhone 模擬器延伸](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone 模擬器延伸模組")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="9fe7f-661">*iPhone 模擬器延伸模組*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="9fe7f-662">閱讀並接受使用者授權合約的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="9fe7f-663">![WebMatrix 延伸 EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix 延伸使用者授權合約")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="9fe7f-664">*WebMatrix 延伸使用者授權合約*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="9fe7f-665">現在，您可以執行您的網站從 WebMatrix 使用 iPhone 模擬器選項。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="9fe7f-666">![使用 iPhone 執行](whats-new-in-aspnet-mvc-4/_static/image57.png "執行使用 iPhone")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="9fe7f-667">*使用 iPhone 執行*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="9fe7f-668">工作 3-設定要執行 iPhone 模擬器的 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9fe7f-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="9fe7f-669">開啟**Visual Studio 2012**和開啟任何網站，或建立新的專案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="9fe7f-670">按一下向下的箭號，從 [執行] 按鈕，然後選取**瀏覽包含**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="9fe7f-671">![瀏覽包含](whats-new-in-aspnet-mvc-4/_static/image58.png "瀏覽方式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="9fe7f-672">*瀏覽方式*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-672">*Browse with*</span></span>
3. <span data-ttu-id="9fe7f-673">在&quot;瀏覽&quot;] 對話方塊中，按一下 [**新增**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="9fe7f-674">在&quot;新增程式&quot; 對話方塊中，使用下列值：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="9fe7f-675"><strong>程式</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * （據以更新路徑）。</em></span><span class="sxs-lookup"><span data-stu-id="9fe7f-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="9fe7f-676">**引數**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="9fe7f-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="9fe7f-677">**易記名稱**: iPhone 模擬器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="9fe7f-678">![加入程式](whats-new-in-aspnet-mvc-4/_static/image59.png "加入程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="9fe7f-679">*新增程式，瀏覽包含*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="9fe7f-680">按一下**確定**並關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="9fe7f-681">現在，您就能夠在 iPhone 模擬器中執行您 Web 應用程式，從 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="9fe7f-682">![瀏覽與 iPhone 模擬器](whats-new-in-aspnet-mvc-4/_static/image60.png "瀏覽與 iPhone 模擬器")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="9fe7f-683">*瀏覽與 iPhone 模擬器*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9fe7f-684">附錄 d： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="9fe7f-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9fe7f-685">此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="9fe7f-686">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="9fe7f-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="9fe7f-687">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-688">與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9fe7f-689">您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9fe7f-690">![登入 Windows Azure 入口網站](whats-new-in-aspnet-mvc-4/_static/image61.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9fe7f-691">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="9fe7f-692">按一下**新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9fe7f-693">![建立新的網站](whats-new-in-aspnet-mvc-4/_static/image62.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9fe7f-694">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9fe7f-695">按一下**計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9fe7f-696">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="9fe7f-697">新的網站上提供可用的 URL，然後按一下**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-698">Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9fe7f-699">快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="9fe7f-700">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9fe7f-701">![建立新的網站使用 快速建立](whats-new-in-aspnet-mvc-4/_static/image63.png "建立新的網站使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9fe7f-702">*建立新的網站使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9fe7f-703">等候新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9fe7f-704">建立網站之後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9fe7f-705">請檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9fe7f-706">![瀏覽至新的網站](whats-new-in-aspnet-mvc-4/_static/image64.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9fe7f-707">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9fe7f-708">![執行網站](whats-new-in-aspnet-mvc-4/_static/image65.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="9fe7f-709">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-709">*Web site running*</span></span>
6. <span data-ttu-id="9fe7f-710">返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9fe7f-711">![開啟網站管理頁面](whats-new-in-aspnet-mvc-4/_static/image66.png "開啟網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9fe7f-712">*開啟網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9fe7f-713">在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-714">*發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="9fe7f-715">發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9fe7f-716">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="9fe7f-717">![下載網站發行設定檔](whats-new-in-aspnet-mvc-4/_static/image67.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9fe7f-718">*下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9fe7f-719">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9fe7f-720">進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="9fe7f-721">![儲存發行設定檔](whats-new-in-aspnet-mvc-4/_static/image68.png "儲存發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9fe7f-722">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9fe7f-723">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="9fe7f-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9fe7f-724">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9fe7f-725">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9fe7f-726">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9fe7f-727">您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9fe7f-728">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9fe7f-729">請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9fe7f-730">並未建立資料庫，因為它會在後續階段中建立。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9fe7f-731">![SQL Database 伺服器儀表板](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9fe7f-732">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9fe7f-733">下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9fe7f-734">若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![加入用戶端 IP 位址](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="9fe7f-736">*加入用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9fe7f-737">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="9fe7f-739">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9fe7f-740">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="9fe7f-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9fe7f-741">返回至 ASP.NET MVC 4 方案。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9fe7f-742">在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9fe7f-743">![發行應用程式](whats-new-in-aspnet-mvc-4/_static/image73.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="9fe7f-744">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="9fe7f-745">匯入發行設定檔儲存在第一項工作中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9fe7f-746">![匯入發行設定檔](whats-new-in-aspnet-mvc-4/_static/image74.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9fe7f-747">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="9fe7f-748">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-748">Click **Validate Connection**.</span></span> <span data-ttu-id="9fe7f-749">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9fe7f-750">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9fe7f-751">![驗證連線](whats-new-in-aspnet-mvc-4/_static/image75.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="9fe7f-752">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-752">*Validating connection*</span></span>
4. <span data-ttu-id="9fe7f-753">在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9fe7f-754">![Web 部署設定](whats-new-in-aspnet-mvc-4/_static/image76.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9fe7f-755">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9fe7f-756">設定資料庫連接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9fe7f-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="9fe7f-757">在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="9fe7f-758">在**使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="9fe7f-759">在**密碼**輸入伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="9fe7f-760">輸入新的資料庫名稱，例如： *MVC4SampleDB*。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="9fe7f-761">![設定目的地連接字串](whats-new-in-aspnet-mvc-4/_static/image77.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="9fe7f-762">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9fe7f-763">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="9fe7f-763">Then click **OK**.</span></span> <span data-ttu-id="9fe7f-764">當系統提示您建立資料庫依序按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9fe7f-765">![建立資料庫](whats-new-in-aspnet-mvc-4/_static/image78.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="9fe7f-766">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-766">*Creating the database*</span></span>
7. <span data-ttu-id="9fe7f-767">您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9fe7f-768">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-768">Then click **Next**.</span></span>

    <span data-ttu-id="9fe7f-769">![連接字串指向 SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "連接字串指向 SQL 資料庫")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9fe7f-770">*連接字串指向 SQL 資料庫*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9fe7f-771">在**預覽**頁面上，按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9fe7f-772">![Web 應用程式發行](whats-new-in-aspnet-mvc-4/_static/image80.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="9fe7f-773">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="9fe7f-774">一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="9fe7f-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="9fe7f-775">![應用程式發行至 Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "應用程式發行至 Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="9fe7f-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="9fe7f-776">*應用程式發行至 Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="9fe7f-776">*Application published to Windows Azure*</span></span>
