---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 開始使用 Entity Framework 6 Code First 使用 MVC 5 |Microsoft Docs
author: tdykstra
description: 本教學課程系列的較新版本位於： 開始使用 ASP.NET Core 和 Entity Framework Core 使用 Visual Studio 2015。 Contoso Universi 中...
ms.author: riande
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 29004ec2271dbf77395f07e030533e23662b67c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834656"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a><span data-ttu-id="4661d-104">透過 MVC 5 開始使用 Entity Framework 6 Code First</span><span class="sxs-lookup"><span data-stu-id="4661d-104">Getting Started with Entity Framework 6 Code First using MVC 5</span></span>
====================
<span data-ttu-id="4661d-105">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4661d-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4661d-106">[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="4661d-106">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> > [!NOTE] 
> > 
> > <span data-ttu-id="4661d-107">本教學課程系列的較新版本位於：[開始使用 ASP.NET Core 和 Entity Framework Core 使用 Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html)。</span><span class="sxs-lookup"><span data-stu-id="4661d-107">A newer version of this tutorial series is available: [Get started with ASP.NET Core and Entity Framework Core using Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).</span></span>
> 
> 
> <span data-ttu-id="4661d-108">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4661d-108">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 and Visual Studio 2013.</span></span> <span data-ttu-id="4661d-109">本教學課程使用 Code First 的工作流程。</span><span class="sxs-lookup"><span data-stu-id="4661d-109">This tutorial uses the Code First workflow.</span></span> <span data-ttu-id="4661d-110">如需有關如何選擇 Code First、 Database First 或 Model First 的資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。</span><span class="sxs-lookup"><span data-stu-id="4661d-110">For information about how to choose between Code First, Database First, and Model First, see [Entity Framework Development Workflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>
> 
> <span data-ttu-id="4661d-111">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="4661d-111">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="4661d-112">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="4661d-112">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="4661d-113">本教學課程系列會說明如何建立 Contoso 大學範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="4661d-113">This tutorial series explains how to build the Contoso University sample application.</span></span> <span data-ttu-id="4661d-114">您可以[下載完成的應用程式](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)。</span><span class="sxs-lookup"><span data-stu-id="4661d-114">You can [download the completed application](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).</span></span>
> 
> <span data-ttu-id="4661d-115">由 Mike Brind 轉譯的 Visual Basic 版本位於： [EF 6，在 Visual Basic 中的 MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting 站台上。</span><span class="sxs-lookup"><span data-stu-id="4661d-115">A Visual Basic version translated by Mike Brind is available: [MVC 5 with EF 6 in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) on the Mikesdotnetting site.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4661d-116">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="4661d-116">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4661d-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4661d-117">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="4661d-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="4661d-118">.NET 4.5</span></span>
> - <span data-ttu-id="4661d-119">Entity Framework 6 （EntityFramework 6.1.0 NuGet 套件）</span><span class="sxs-lookup"><span data-stu-id="4661d-119">Entity Framework 6 (EntityFramework 6.1.0 NuGet package)</span></span>
> - <span data-ttu-id="4661d-120">[Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) （選擇性）</span><span class="sxs-lookup"><span data-stu-id="4661d-120">[Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (optional)</span></span>
>   
> 
> <span data-ttu-id="4661d-121">本教學課程也應該使用[Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="4661d-121">The tutorial should also work with [Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) or Visual Studio 2012.</span></span> <span data-ttu-id="4661d-122">[VS 2012 版的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) ，才能使用 Visual Studio 2012 的 Windows Azure 部署。</span><span class="sxs-lookup"><span data-stu-id="4661d-122">The [VS 2012 version of the Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) is required for Windows Azure deployment with Visual Studio 2012.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="4661d-123">教學課程的版本</span><span class="sxs-lookup"><span data-stu-id="4661d-123">Tutorial versions</span></span>
> 
> <span data-ttu-id="4661d-124">本教學課程的先前版本，請參閱[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)並[開始使用 MVC 4 的 EF 5](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="4661d-124">For previous versions of this tutorial, see [the EF 4.1 / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) and [Getting Started with EF 5 using MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4661d-125">提出問題或意見</span><span class="sxs-lookup"><span data-stu-id="4661d-125">Questions and comments</span></span>
> 
> <span data-ttu-id="4661d-126">您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="4661d-126">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4661d-127">如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)，則[Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="4661d-127">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx), the [Entity Framework and LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), or [StackOverflow.com](http://stackoverflow.com/).</span></span>
> 
> <span data-ttu-id="4661d-128">如果您遇到的問題，無法解析時，您通常可以藉由比較您的程式碼已完成的專案，您可以下載中找到問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="4661d-128">If you run into a problem you can't resolve, you can generally find the solution to the problem by comparing your code to the completed project that you can download.</span></span> <span data-ttu-id="4661d-129">一些常見錯誤及如何解決這些問題，請參閱[常見的錯誤和解決方案或因應措施它們。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="4661d-129">For some common errors and how to solve them, see [Common errors, and solutions or workarounds for them.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


## <a name="the-contoso-university-web-application"></a><span data-ttu-id="4661d-130">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4661d-130">The Contoso University Web Application</span></span>

<span data-ttu-id="4661d-131">您在這些教學課程中會建置的應用程式為一個簡單的大學網站。</span><span class="sxs-lookup"><span data-stu-id="4661d-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="4661d-132">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="4661d-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="4661d-133">以下是您會建立的幾個畫面。</span><span class="sxs-lookup"><span data-stu-id="4661d-133">Here are a few of the screens you'll create.</span></span>

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![編輯學生](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="4661d-136">此網站的 UI 樣式與內建範本產生的相當類似，以使此教學課程能聚焦於如何使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="4661d-136">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4661d-137">必要條件</span><span class="sxs-lookup"><span data-stu-id="4661d-137">Prerequisites</span></span>

<span data-ttu-id="4661d-138">請參閱**軟體版本**頁面的頂端。</span><span class="sxs-lookup"><span data-stu-id="4661d-138">See **Software Versions** at the top of the page.</span></span> <span data-ttu-id="4661d-139">Entity Framework 6 不是必要條件，因為您安裝 EF NuGet 套件做為本教學課程的一部分。</span><span class="sxs-lookup"><span data-stu-id="4661d-139">Entity Framework 6 is not a prerequisite because you install the EF NuGet package as part of the tutorial.</span></span>

## <a name="create-an-mvc-web-application"></a><span data-ttu-id="4661d-140">建立 MVC Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="4661d-140">Create an MVC Web Application</span></span>

<span data-ttu-id="4661d-141">開啟 Visual Studio 並建立新的 C# Web 專案，名為"ContosoUniversity"。</span><span class="sxs-lookup"><span data-stu-id="4661d-141">Open Visual Studio and create a new C# Web project named "ContosoUniversity".</span></span>

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="4661d-143">在 [**新的 ASP.NET 專案**] 對話方塊中選取**MVC**範本。</span><span class="sxs-lookup"><span data-stu-id="4661d-143">In the **New ASP.NET Project** dialog box select the **MVC** template.</span></span>

<span data-ttu-id="4661d-144">如果**在雲端託管**中的核取方塊**Microsoft Azure**區段會被選取，請清除它。</span><span class="sxs-lookup"><span data-stu-id="4661d-144">If the **Host in the cloud** check box in the **Microsoft Azure** section is selected, clear it.</span></span>

<span data-ttu-id="4661d-145">按一下 **變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="4661d-145">Click **Change Authentication**.</span></span>

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="4661d-147">在 **變更驗證**對話方塊中，選取**不需要驗證**，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="4661d-147">In the **Change Authentication** dialog box, select **No Authentication**, and then click **OK**.</span></span> <span data-ttu-id="4661d-148">本教學課程中您不會要求使用者登入或根據登入的使用者限制存取。</span><span class="sxs-lookup"><span data-stu-id="4661d-148">For this tutorial you won't be requiring users to log on or restricting access based on who's logged on.</span></span>

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="4661d-150">在 [新增 ASP.NET 專案] 對話方塊中，按一下**確定**建立專案。</span><span class="sxs-lookup"><span data-stu-id="4661d-150">Back in the New ASP.NET Project dialog box, click **OK** to create the project.</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="4661d-151">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="4661d-151">Set Up the Site Style</span></span>

<span data-ttu-id="4661d-152">一些簡單的變更會設定網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="4661d-152">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="4661d-153">開啟*Views\Shared\\_Layout.cshtml*，並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="4661d-153">Open *Views\Shared\\_Layout.cshtml*, and make the following changes:</span></span>

- <span data-ttu-id="4661d-154">將每個出現的"My ASP.NET Application"和"Application name"變更為"Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="4661d-154">Change each occurrence of "My ASP.NET Application" and "Application name" to "Contoso University".</span></span>
- <span data-ttu-id="4661d-155">新增功能表項目為學生、 課程、 講師和部門，並刪除連絡人項目。</span><span class="sxs-lookup"><span data-stu-id="4661d-155">Add menu entries for Students, Courses, Instructors, and Departments, and delete the Contact entry.</span></span>

<span data-ttu-id="4661d-156">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="4661d-156">The changes are highlighted.</span></span>

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

<span data-ttu-id="4661d-157">在  *Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼，以使用關於此應用程式的文字取代關於 ASP.NET 和 MVC 的文字：</span><span class="sxs-lookup"><span data-stu-id="4661d-157">In *Views\Home\Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

<span data-ttu-id="4661d-158">按下 CTRL + F5，執行站台。</span><span class="sxs-lookup"><span data-stu-id="4661d-158">Press CTRL+F5 to run the site.</span></span> <span data-ttu-id="4661d-159">您會看到與主功能表的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="4661d-159">You see the home page with the main menu.</span></span>

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a><span data-ttu-id="4661d-161">安裝 Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4661d-161">Install Entity Framework 6</span></span>

<span data-ttu-id="4661d-162">從**工具**功能表上，按一下**NuGet 套件管理員**，然後按一下  **Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="4661d-162">From the **Tools** menu click **NuGet Package Manager** and then click **Package Manager Console**.</span></span>

<span data-ttu-id="4661d-163">在  **Package Manager Console**視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="4661d-163">In the **Package Manager Console** window enter the following command:</span></span>

`Install-Package EntityFramework`

![安裝 EF](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="4661d-165">下圖顯示 6.0.0 安裝，但 NuGet 會安裝最新版的 Entity Framework （不含發行前版本），這本教學課程以最新的更新是 6.1.1。</span><span class="sxs-lookup"><span data-stu-id="4661d-165">The image shows 6.0.0 being installed, but NuGet will install the latest version of Entity Framework (excluding pre-release versions), which as of the most recent update to the tutorial is 6.1.1.</span></span>

<span data-ttu-id="4661d-166">這個步驟是，本教學課程有您手動執行，但它無法在完成自動由 ASP.NET MVC 樣板功能的幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="4661d-166">This step is one of a few steps that this tutorial has you do manually, but which could have been done automatically by the ASP.NET MVC scaffolding feature.</span></span> <span data-ttu-id="4661d-167">您的所作所為它們以手動方式，讓您可以看到使用 Entity Framework 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="4661d-167">You're doing them manually so that you can see the steps required to use the Entity Framework.</span></span> <span data-ttu-id="4661d-168">您將更新版本使用 scaffolding 建立 MVC 控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="4661d-168">You'll use scaffolding later to create the MVC controller and views.</span></span> <span data-ttu-id="4661d-169">替代方法是讓 scaffolding 自動安裝 EF NuGet 套件、 建立資料庫內容類別，以及建立連接字串。</span><span class="sxs-lookup"><span data-stu-id="4661d-169">An alternative is to let scaffolding automatically install the EF NuGet package, create the database context class, and create the connection string.</span></span> <span data-ttu-id="4661d-170">當您準備好這麼這樣一來時，您只需要為略過這些步驟，並在您建立實體類別之後，建立 MVC 控制器的結構。</span><span class="sxs-lookup"><span data-stu-id="4661d-170">When you're ready to do it that way, all you have to do is skip those steps and scaffold your MVC controller after you create your entity classes.</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="4661d-171">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="4661d-171">Create the Data Model</span></span>

<span data-ttu-id="4661d-172">接下來您會為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-172">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="4661d-173">您首先要使用下列三個實體：</span><span class="sxs-lookup"><span data-stu-id="4661d-173">You'll start with the following three entities:</span></span>

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="4661d-175">在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="4661d-175">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="4661d-176">換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="4661d-176">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="4661d-177">在下節中，您會為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-177">In the following sections you'll create a class for each one of these entities.</span></span>

> [!NOTE]
> <span data-ttu-id="4661d-178">如果您嘗試編譯專案，才能完成建立所有這些實體類別時，您會收到編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="4661d-178">If you try to compile the project before you finish creating all of these entity classes, you'll get compiler errors.</span></span>


### <a name="the-student-entity"></a><span data-ttu-id="4661d-179">Student 實體</span><span class="sxs-lookup"><span data-stu-id="4661d-179">The Student Entity</span></span>

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="4661d-181">在 *模型*資料夾中，建立名為的類別檔案*Student.cs*並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="4661d-181">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

<span data-ttu-id="4661d-182">`ID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-182">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="4661d-183">根據預設，Entity Framework 的屬性解譯，稱為`ID`或是*classname* `ID`為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="4661d-183">By default, the Entity Framework interprets a property that's named `ID` or *classname* `ID` as the primary key.</span></span>

<span data-ttu-id="4661d-184">`Enrollments`屬性是*導覽屬性*。</span><span class="sxs-lookup"><span data-stu-id="4661d-184">The `Enrollments` property is a *navigation property*.</span></span> <span data-ttu-id="4661d-185">導覽屬性會保留與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-185">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="4661d-186">在此情況下，`Enrollments`的屬性`Student`實體將保存的所有`Enrollment`相關的實體`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-186">In this case, the `Enrollments` property of a `Student` entity will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="4661d-187">換句話說，如果給定`Student`資料庫中的資料列有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將包含這兩個`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-187">In other words, if a given `Student` row in the database has two related `Enrollment` rows (rows that contain that student's primary key value in their `StudentID` foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="4661d-188">導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能這類*消極式載入*。</span><span class="sxs-lookup"><span data-stu-id="4661d-188">Navigation properties are typically defined as `virtual` so that they can take advantage of certain Entity Framework functionality such as *lazy loading*.</span></span> <span data-ttu-id="4661d-189">(將會說明消極式載入稍後，在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。)</span><span class="sxs-lookup"><span data-stu-id="4661d-189">(Lazy loading will be explained later, in the [Reading Related Data](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial later in this series.)</span></span>

<span data-ttu-id="4661d-190">若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection`。</span><span class="sxs-lookup"><span data-stu-id="4661d-190">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection`.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="4661d-191">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="4661d-191">The Enrollment Entity</span></span>

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

<span data-ttu-id="4661d-193">在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="4661d-193">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="4661d-194">`EnrollmentID`屬性會是主索引鍵，此實體會使用*classname* `ID`模式，而非`ID`本身為您在看到`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-194">The `EnrollmentID` property will be the primary key; this entity uses the *classname* `ID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="4661d-195">通常您會選擇一個模式，然後在您整個資料模型中使用此模式。</span><span class="sxs-lookup"><span data-stu-id="4661d-195">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="4661d-196">在這裡，此變化僅作為向您展示使用不同模式之用。</span><span class="sxs-lookup"><span data-stu-id="4661d-196">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="4661d-197">在稍後的教學課程中，您會看到如何使用`ID`而不需要`classname`更輕鬆地在資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="4661d-197">In a later tutorial, you'll see how using `ID` without `classname` makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="4661d-198">`Grade`屬性是[列舉](https://msdn.microsoft.com/data/hh859576.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4661d-198">The `Grade` property is an [enum](https://msdn.microsoft.com/data/hh859576.aspx).</span></span> <span data-ttu-id="4661d-199">問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4661d-199">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx).</span></span> <span data-ttu-id="4661d-200">為 null 的成績等級是不同於成績為零： null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="4661d-200">A grade that's null is different from a zero grade — null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="4661d-201">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="4661d-201">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="4661d-202">`Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。</span><span class="sxs-lookup"><span data-stu-id="4661d-202">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="4661d-203">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="4661d-203">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="4661d-204">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="4661d-204">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="4661d-205">Entity Framework 的屬性解譯為外部索引鍵屬性是否將它命名*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (例如`StudentID`for`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。</span><span class="sxs-lookup"><span data-stu-id="4661d-205">Entity Framework interprets a property as a foreign key property if it's named *&lt;navigation property name&gt;&lt;primary key property name&gt;* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="4661d-206">外部索引鍵屬性也可以命名相同只要*&lt;主索引鍵屬性名稱&gt;* (例如`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。</span><span class="sxs-lookup"><span data-stu-id="4661d-206">Foreign key properties can also be named the same simply *&lt;primary key property name&gt;* (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="4661d-207">Course 實體</span><span class="sxs-lookup"><span data-stu-id="4661d-207">The Course Entity</span></span>

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

<span data-ttu-id="4661d-209">在 *模型*資料夾中，建立*Course.cs*，以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="4661d-209">In the *Models* folder, create *Course.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="4661d-210">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="4661d-210">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="4661d-211">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="4661d-211">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="4661d-212">我們將進一步討論[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)在稍後的教學課程中，在這一系列的屬性。</span><span class="sxs-lookup"><span data-stu-id="4661d-212">We'll say more about the [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) attribute in a later tutorial in this series.</span></span> <span data-ttu-id="4661d-213">基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。</span><span class="sxs-lookup"><span data-stu-id="4661d-213">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="4661d-214">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="4661d-214">Create the Database Context</span></span>

<span data-ttu-id="4661d-215">協調 Entity Framework 功能，為指定的資料模型的主要類別是*資料庫內容*類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-215">The main class that coordinates Entity Framework functionality for a given data model is the *database context* class.</span></span> <span data-ttu-id="4661d-216">您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-216">You create this class by deriving from the [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) class.</span></span> <span data-ttu-id="4661d-217">在您的程式碼中，您會指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-217">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="4661d-218">您也可以自訂某些 Entity Framework 行為。</span><span class="sxs-lookup"><span data-stu-id="4661d-218">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="4661d-219">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="4661d-219">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="4661d-220">ContosoUniversity 專案中建立資料夾，請以滑鼠右鍵按一下專案中的**方案總管**，按一下 **新增**，然後按一下**新資料夾**。</span><span class="sxs-lookup"><span data-stu-id="4661d-220">To create a folder in the ContosoUniversity project, right-click the project in **Solution Explorer** and click **Add**, and then click **New Folder**.</span></span> <span data-ttu-id="4661d-221">將新資料夾命名*DAL* （適用於資料存取層）。</span><span class="sxs-lookup"><span data-stu-id="4661d-221">Name the new folder *DAL* (for Data Access Layer).</span></span> <span data-ttu-id="4661d-222">在該資料夾中建立新的類別檔案，名為*SchoolContext.cs*，並以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="4661d-222">In that folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a><span data-ttu-id="4661d-223">指定實體集</span><span class="sxs-lookup"><span data-stu-id="4661d-223">Specifying entity sets</span></span>

<span data-ttu-id="4661d-224">此程式碼會建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。</span><span class="sxs-lookup"><span data-stu-id="4661d-224">This code creates a [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) property for each entity set.</span></span> <span data-ttu-id="4661d-225">在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，以及*實體*對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="4661d-225">In Entity Framework terminology, an *entity set* typically corresponds to a database table, and an *entity* corresponds to a row in the table.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="4661d-226">您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式和它的運作。</span><span class="sxs-lookup"><span data-stu-id="4661d-226">You could have omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="4661d-227">Entity Framework 會隱含它們，因為 `Student` 實體參考了 `Enrollment` 實體；而 `Enrollment` 實體參考了 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="4661d-227">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>


### <a name="specifying-the-connection-string"></a><span data-ttu-id="4661d-228">指定連接字串</span><span class="sxs-lookup"><span data-stu-id="4661d-228">Specifying the connection string</span></span>

<span data-ttu-id="4661d-229">連接字串 （其中您稍後會加入至 Web.config 檔案） 的名稱會傳遞至建構函式。</span><span class="sxs-lookup"><span data-stu-id="4661d-229">The name of the connection string (which you'll add to the Web.config file later) is passed in to the constructor.</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

<span data-ttu-id="4661d-230">您也可以傳入連接字串而不是儲存在 Web.config 檔案中的其中一個的名稱。</span><span class="sxs-lookup"><span data-stu-id="4661d-230">You could also pass in the connection string itself instead of the name of one that is stored in the Web.config file.</span></span> <span data-ttu-id="4661d-231">如需使用指定的資料庫選項的詳細資訊，請參閱[Entity Framework-連線和模型](https://msdn.microsoft.com/data/jj592674)。</span><span class="sxs-lookup"><span data-stu-id="4661d-231">For more information about options for specifying the database to use, see [Entity Framework - Connections and Models](https://msdn.microsoft.com/data/jj592674).</span></span>

<span data-ttu-id="4661d-232">如果您未指定連接字串或其中一個名稱明確，Entity Framework 會假設此連接字串名稱是類別名稱相同。</span><span class="sxs-lookup"><span data-stu-id="4661d-232">If you don't specify a connection string or the name of one explicitly, Entity Framework assumes that the connection string name is the same as the class name.</span></span> <span data-ttu-id="4661d-233">在此範例中的預設連接字串名稱就得`SchoolContext`，等同於您要明確指定。</span><span class="sxs-lookup"><span data-stu-id="4661d-233">The default connection string name in this example would then be `SchoolContext`, the same as what you're specifying explicitly.</span></span>

### <a name="specifying-singular-table-names"></a><span data-ttu-id="4661d-234">指定單數資料表名稱</span><span class="sxs-lookup"><span data-stu-id="4661d-234">Specifying singular table names</span></span>

<span data-ttu-id="4661d-235">`modelBuilder.Conventions.Remove`中的陳述式[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會防止資料表名稱的複數化。</span><span class="sxs-lookup"><span data-stu-id="4661d-235">The `modelBuilder.Conventions.Remove` statement in the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) method prevents table names from being pluralized.</span></span> <span data-ttu-id="4661d-236">如果您沒有這麼做，在資料庫中產生的資料表就會命名為`Students`， `Courses`，和`Enrollments`。</span><span class="sxs-lookup"><span data-stu-id="4661d-236">If you didn't do this, the generated tables in the database would be named `Students`, `Courses`, and `Enrollments`.</span></span> <span data-ttu-id="4661d-237">相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。</span><span class="sxs-lookup"><span data-stu-id="4661d-237">Instead, the table names will be `Student`, `Course`, and `Enrollment`.</span></span> <span data-ttu-id="4661d-238">針對是否要複數化資料表名稱，開發人員並沒有共識。</span><span class="sxs-lookup"><span data-stu-id="4661d-238">Developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="4661d-239">本教學課程使用的單數形式，但很重要的一點是，您可以選取任何您想要包含或省略這行程式碼的表單。</span><span class="sxs-lookup"><span data-stu-id="4661d-239">This tutorial uses the singular form, but the important point is that you can select whichever form you prefer by including or omitting this line of code.</span></span>

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a><span data-ttu-id="4661d-240">初始化具有測試資料的資料庫設定 EF</span><span class="sxs-lookup"><span data-stu-id="4661d-240">Set up EF to initialize the database with test data</span></span>

<span data-ttu-id="4661d-241">Entity Framework 可以自動建立 （或卸除並重新建立） 為您的應用程式執行時的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-241">The Entity Framework can automatically create (or drop and re-create) a database for you when the application runs.</span></span> <span data-ttu-id="4661d-242">您可以指定每次執行應用程式，或此模型會與現有的資料庫同步處理時，才，應該完成這。</span><span class="sxs-lookup"><span data-stu-id="4661d-242">You can specify that this should be done every time your application runs or only when the model is out of sync with the existing database.</span></span> <span data-ttu-id="4661d-243">您也可以撰寫`Seed`Entity Framework 會自動以填入測試資料建立資料庫之後呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="4661d-243">You can also write a `Seed` method that the Entity Framework automatically calls after creating the database in order to populate it with test data.</span></span>

<span data-ttu-id="4661d-244">預設行為是建立資料庫，如果它不存在 （且擲回例外狀況，如果模型已變更，而且資料庫已經存在）。</span><span class="sxs-lookup"><span data-stu-id="4661d-244">The default behavior is to create a database only if it doesn't exist (and throw an exception if the model has changed and the database already exists).</span></span> <span data-ttu-id="4661d-245">在本節中，您會指定資料庫應該卸除並重新建立每次模型變更時。</span><span class="sxs-lookup"><span data-stu-id="4661d-245">In this section you'll specify that the database should be dropped and re-created whenever the model changes.</span></span> <span data-ttu-id="4661d-246">卸除資料庫時，會造成您的所有資料遺失。</span><span class="sxs-lookup"><span data-stu-id="4661d-246">Dropping the database causes the loss of all your data.</span></span> <span data-ttu-id="4661d-247">這通常是 [確定] 在開發期間，因為`Seed`資料庫重新建立，而且將會重新建立您的測試資料時，會執行方法。</span><span class="sxs-lookup"><span data-stu-id="4661d-247">This is generally OK during development, because the `Seed` method will run when the database is re-created and will re-create your test data.</span></span> <span data-ttu-id="4661d-248">但在生產環境中通常不想遺失所有資料，每當您需要變更資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="4661d-248">But in production you generally don't want to lose all your data every time you need to change the database schema.</span></span> <span data-ttu-id="4661d-249">稍後您會看到如何使用 Code First 移轉來變更資料庫結構描述，而不是卸除並重新建立資料庫來處理模型變更。</span><span class="sxs-lookup"><span data-stu-id="4661d-249">Later you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="4661d-250">在 DAL 資料夾中，建立新的類別檔案，名為*SchoolInitializer.cs*並取代範本程式碼</span><span class="sxs-lookup"><span data-stu-id="4661d-250">In the DAL folder, create a new class file named *SchoolInitializer.cs* and replace the template code with the</span></span>  
<span data-ttu-id="4661d-251">下列程式碼，使資料庫建立時所需，並將測試資料載入新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-251">following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="4661d-252">`Seed`方法會採用資料庫內容物件做為輸入參數，並在方法中的程式碼會使用該物件來將新實體新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-252">The `Seed` method takes the database context object as an input parameter, and the code in the method uses that object to add new entities to the database.</span></span> <span data-ttu-id="4661d-253">每個實體類型，程式碼會建立新實體的集合，將它們新增至適當`DbSet`屬性，然後按一下 儲存至資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="4661d-253">For each entity type, the code creates a collection of new  entities, adds them to the appropriate `DbSet` property, and then saves the changes to the database.</span></span> <span data-ttu-id="4661d-254">不需要呼叫`SaveChanges`實體的每一個群組之後的方法，做為在這裡，完成，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="4661d-254">It isn't necessary to call the `SaveChanges` method after each group of entities, as is done here, but doing that helps you locate the source of a problem if an exception occurs while the code is writing to the database.</span></span>

<span data-ttu-id="4661d-255">若要告訴 Entity Framework 使用初始設定式類別，將新增項目`entityFramework`應用程式中的項目*Web.config*檔的檔案 （在根專案資料夾中），如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="4661d-255">To tell Entity Framework to use your initializer class, add an element to the `entityFramework` element in the application *Web.config* file (the one in the root project folder), as shown in the following example:</span></span>

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

<span data-ttu-id="4661d-256">`context type`指定完整的內容類別名稱和組件，而`databaseinitializer type`指定初始設定式類別和組件中的完整的名稱。</span><span class="sxs-lookup"><span data-stu-id="4661d-256">The `context type` specifies the fully qualified context class name and the assembly it's in, and the `databaseinitializer type` specifies the fully qualified name of the initializer class and the assembly it's in.</span></span> <span data-ttu-id="4661d-257">(當您不想要使用初始設定式的 EF 時，您可以設定屬性`context`項目： `disableDatabaseInitialization="true"`。)如需詳細資訊，請參閱 < [Entity Framework-設定檔設定](https://msdn.microsoft.com/data/jj556606)。</span><span class="sxs-lookup"><span data-stu-id="4661d-257">(When you don't want EF to use the initializer, you can set an attribute on the `context` element: `disableDatabaseInitialization="true"`.) For more information, see [Entity Framework - Config File Settings](https://msdn.microsoft.com/data/jj556606).</span></span>

<span data-ttu-id="4661d-258">除了設定初始設定式*Web.config*檔案是要在程式碼中加上`Database.SetInitializer`陳述式來`Application_Start`中的方法*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-258">As an alternative to setting the initializer in the *Web.config* file is to do it in code by adding a `Database.SetInitializer` statement to the `Application_Start` method in the *Global.asax.cs* file.</span></span> <span data-ttu-id="4661d-259">如需詳細資訊，請參閱 <<c0> [ 了解在 Entity Framework Code First 資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)。</span><span class="sxs-lookup"><span data-stu-id="4661d-259">For more information, see [Understanding Database Initializers in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).</span></span>

<span data-ttu-id="4661d-260">應用程式現在已設定讓該特定執行的第一次存取資料庫時</span><span class="sxs-lookup"><span data-stu-id="4661d-260">The application is now set up so that when you access the database for the first time in a given run of the</span></span>  
<span data-ttu-id="4661d-261">應用程式中，Entity Framework 比較的模型資料庫 (您`SchoolContext`和實體類別)。</span><span class="sxs-lookup"><span data-stu-id="4661d-261">application, the Entity Framework compares the database to the model (your `SchoolContext` and entity classes).</span></span> <span data-ttu-id="4661d-262">如果沒有差異，應用程式會卸除並重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-262">If there's a difference, the application drops and re-creates the database.</span></span>

> [!NOTE]
> <span data-ttu-id="4661d-263">當您部署在生產網頁伺服器應用程式時，您必須移除或停用程式碼，會卸除並重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-263">When you deploy an application to a production web server, you must remove or disable code that drops and re-creates the database.</span></span> <span data-ttu-id="4661d-264">您將在稍後的教學課程中，在這一系列來這麼做。</span><span class="sxs-lookup"><span data-stu-id="4661d-264">You'll do that in a later tutorial in this series.</span></span>


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a><span data-ttu-id="4661d-265">設定為使用 SQL Server Express LocalDB 資料庫的 EF</span><span class="sxs-lookup"><span data-stu-id="4661d-265">Set up EF to use a SQL Server Express LocalDB database</span></span>

<span data-ttu-id="4661d-266">[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版的 SQL Server Express Database Engine。</span><span class="sxs-lookup"><span data-stu-id="4661d-266">[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) is a lightweight version of the SQL Server Express Database Engine.</span></span> <span data-ttu-id="4661d-267">輕鬆安裝和設定、 啟動隨選、 並以使用者模式執行。</span><span class="sxs-lookup"><span data-stu-id="4661d-267">It's easy to install and configure, starts on demand, and runs in user mode.</span></span> <span data-ttu-id="4661d-268">LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-268">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="4661d-269">您可以將 LocalDB 資料庫檔案放在*應用程式\_資料*web 專案，如果您想要能夠將複製的資料庫專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="4661d-269">You can put LocalDB database files in the *App\_Data* folder of a web project if you want to be able to copy the database with the project.</span></span> <span data-ttu-id="4661d-270">在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-270">The user instance feature in SQL Server Express also enables you to work with *.mdf* files, but the user instance feature is deprecated; therefore, LocalDB is recommended for working with *.mdf* files.</span></span> <span data-ttu-id="4661d-271">在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="4661d-271">In Visual Studio 2012 and later versions, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="4661d-272">SQL Server Express 不是常用的生產 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="4661d-272">Typically SQL Server Express is not used for production web applications.</span></span> <span data-ttu-id="4661d-273">LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是在搭配 IIS 運作。</span><span class="sxs-lookup"><span data-stu-id="4661d-273">LocalDB in particular is not recommended for production use with a web application because it is not designed to work with IIS.</span></span>

<span data-ttu-id="4661d-274">在本教學課程中您將使用 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="4661d-274">In this tutorial you'll work with LocalDB.</span></span> <span data-ttu-id="4661d-275">開啟應用程式*Web.config*檔案，並新增`connectionStrings`上述項目`appSettings`項目，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="4661d-275">Open the application *Web.config* file and add a `connectionStrings` element preceding the `appSettings` element, as shown in the following example.</span></span> <span data-ttu-id="4661d-276">(請確定您更新*Web.config*根專案資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-276">(Make sure you update the *Web.config* file in the root project folder.</span></span> <span data-ttu-id="4661d-277">另外還有*Web.config*檔案位於*檢視*您不需要更新的子資料夾。)</span><span class="sxs-lookup"><span data-stu-id="4661d-277">There's also a *Web.config* file is in the *Views* subfolder that you don't need to update.)</span></span>

<span data-ttu-id="4661d-278">如果您使用 Visual Studio 2015，將"v11.0"連接字串中與"MSSQLLocalDB"，因為預設的 SQL Server 執行個體名稱已變更。</span><span class="sxs-lookup"><span data-stu-id="4661d-278">If you are using Visual Studio 2015, replace "v11.0" in the connection string with "MSSQLLocalDB", as the default SQL Server instance name has changed.</span></span>

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

<span data-ttu-id="4661d-279">您已新增連接字串會指定使用 Entity Framework 會使用名為 LocalDB 資料庫*ContosoUniversity1.mdf*。</span><span class="sxs-lookup"><span data-stu-id="4661d-279">The connection string you've added specifies that Entity Framework will use a LocalDB database named *ContosoUniversity1.mdf*.</span></span> <span data-ttu-id="4661d-280">（資料庫還不存在;EF 會建立它。）如果您想要在建立資料庫您*應用程式\_資料*資料夾中，您可以新增`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`的連接字串。</span><span class="sxs-lookup"><span data-stu-id="4661d-280">(The database doesn't exist yet; EF will create it.) If you wanted the database to be created in your *App\_Data* folder, you could add `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` to the connection string.</span></span> <span data-ttu-id="4661d-281">如需有關連接字串的詳細資訊，請參閱 < [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。</span><span class="sxs-lookup"><span data-stu-id="4661d-281">For more information about connection strings, see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="4661d-282">您實際上不需要在連接字串*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-282">You don't actually have to have a connection string in the *Web.config* file.</span></span> <span data-ttu-id="4661d-283">如果您未提供的連接字串，Entity Framework 會使用一項是依據您的內容類別的預設值。</span><span class="sxs-lookup"><span data-stu-id="4661d-283">If you don't supply a connection string, Entity Framework will use a default one based on your context class.</span></span> <span data-ttu-id="4661d-284">如需詳細資訊，請參閱 < [Code First 至新的資料庫](https://msdn.microsoft.com/data/jj193542)。</span><span class="sxs-lookup"><span data-stu-id="4661d-284">For more information, see [Code First to a New Database](https://msdn.microsoft.com/data/jj193542).</span></span>

## <a name="creating-a-student-controller-and-views"></a><span data-ttu-id="4661d-285">建立學生控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="4661d-285">Creating a Student Controller and Views</span></span>

<span data-ttu-id="4661d-286">現在您將建立網頁上顯示資料，並要求資料的程序會自動觸發</span><span class="sxs-lookup"><span data-stu-id="4661d-286">Now you'll create a web page to display data, and the process of requesting the data will automatically trigger</span></span>  
<span data-ttu-id="4661d-287">建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-287">the creation of the database.</span></span> <span data-ttu-id="4661d-288">您一開始先建立新的控制器。</span><span class="sxs-lookup"><span data-stu-id="4661d-288">You'll begin by creating a new controller.</span></span> <span data-ttu-id="4661d-289">但您這麼做之前，建置專案，以供 MVC 控制器的 scaffolding 的模型和內容的類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-289">But before you do that, build the project to make the model and context classes available to MVC controller scaffolding.</span></span>

1. <span data-ttu-id="4661d-290">以滑鼠右鍵按一下**控制站**資料夾中的**方案總管**，選取**新增**，然後按一下**新增 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="4661d-290">Right-click the **Controllers** folder in **Solution Explorer**, select **Add**, and then click **New Scaffolded Item**.</span></span>
2. <span data-ttu-id="4661d-291">在 **新增 Scaffold**對話方塊中，選取**使用 MVC 5 控制器與檢視，Entity Framework**。</span><span class="sxs-lookup"><span data-stu-id="4661d-291">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework**.</span></span>

     ![新增 Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. <span data-ttu-id="4661d-293">在 新增控制器 對話方塊中，進行下列選擇，然後按一下 **新增**:</span><span class="sxs-lookup"><span data-stu-id="4661d-293">In the Add Controller dialog box, make the following selections and then click **Add**:</span></span>

   - <span data-ttu-id="4661d-294">模型類別：**學生 (ContosoUniversity.Models)**。</span><span class="sxs-lookup"><span data-stu-id="4661d-294">Model class: **Student (ContosoUniversity.Models)**.</span></span> <span data-ttu-id="4661d-295">（如果您沒有看到此選項，在下拉式清單中的，建置專案並再試一次）。</span><span class="sxs-lookup"><span data-stu-id="4661d-295">(If you don't see this option in the drop-down list, build the project and try again.)</span></span>
   - <span data-ttu-id="4661d-296">資料內容類別： **SchoolContext (ContosoUniversity.DAL)**。</span><span class="sxs-lookup"><span data-stu-id="4661d-296">Data context class: **SchoolContext (ContosoUniversity.DAL)**.</span></span>
   - <span data-ttu-id="4661d-297">控制器名稱： **StudentController** (不 StudentsController)。</span><span class="sxs-lookup"><span data-stu-id="4661d-297">Controller name: **StudentController** (not StudentsController).</span></span>
   - <span data-ttu-id="4661d-298">保留其他欄位的預設值。</span><span class="sxs-lookup"><span data-stu-id="4661d-298">Leave the default values for the other fields.</span></span>

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     <span data-ttu-id="4661d-300">當您按一下 **新增**，框架建立 StudentController.cs 檔案和一組可以使用該控制器的檢視 （.cshtml 檔案）。</span><span class="sxs-lookup"><span data-stu-id="4661d-300">When you click **Add**, the scaffolder creates a StudentController.cs file and a set of views (.cshtml files) that work with the controller.</span></span> <span data-ttu-id="4661d-301">在未來，當您建立使用 Entity Framework 的專案中您也可以利用的一些額外的功能的框架： 只要建立您的第一個模型類別，請勿建立連接字串，然後在**新增控制器**  方塊中指定新的內容類別。</span><span class="sxs-lookup"><span data-stu-id="4661d-301">In the future when you create projects that use Entity Framework you can also take advantage of some additional functionality of the scaffolder: just create your first model class, don't create a connection string, and then in the **Add Controller** box specify new context class.</span></span> <span data-ttu-id="4661d-302">框架會建立您`DbContext`類別和您的連線字串以及控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="4661d-302">The scaffolder will create your `DbContext` class and your connection string as well as the controller and views.</span></span>
4. <span data-ttu-id="4661d-303">Visual Studio 會開啟*Controllers\StudentController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="4661d-303">Visual Studio opens the *Controllers\StudentController.cs* file.</span></span> <span data-ttu-id="4661d-304">您會看到已建立的資料庫內容物件具現化類別變數：</span><span class="sxs-lookup"><span data-stu-id="4661d-304">You see a class variable has been created that instantiates a database context object:</span></span>

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     <span data-ttu-id="4661d-305">`Index`動作方法會取得一份的學生*學生*實體集，請閱讀`Students`的資料庫內容執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="4661d-305">The `Index` action method gets a list of students from the *Students* entity set by reading the `Students` property of the database context instance:</span></span>

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     <span data-ttu-id="4661d-306">*Student\Index.cshtml*檢視以表格顯示這份清單：</span><span class="sxs-lookup"><span data-stu-id="4661d-306">The *Student\Index.cshtml* view displays this list in a table:</span></span>

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. <span data-ttu-id="4661d-307">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="4661d-307">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="4661d-308">（如果您收到 「 無法建立陰影複製 」 的錯誤時，關閉瀏覽器並再試一次）。</span><span class="sxs-lookup"><span data-stu-id="4661d-308">(If you get a "Cannot create Shadow Copy" error, close the browser and try again.)</span></span>

     <span data-ttu-id="4661d-309">按一下 **學生**索引標籤來查看測試資料，`Seed`插入的方法。</span><span class="sxs-lookup"><span data-stu-id="4661d-309">Click the **Students** tab to see the test data that the `Seed` method inserted.</span></span> <span data-ttu-id="4661d-310">根據如何窄瀏覽器視窗是，您會看到的最上層的網址列中的學生 索引標籤連結，或您必須按一下右上角來查看連結。</span><span class="sxs-lookup"><span data-stu-id="4661d-310">Depending on how narrow your browser window is, you'll see the Student tab link in the top address bar or you'll have to click the upper right corner to see the link.</span></span>

     ![功能表按鈕](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![學生 [索引] 頁面](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a><span data-ttu-id="4661d-313">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="4661d-313">View the Database</span></span>

<span data-ttu-id="4661d-314">當您執行 Students 頁面並應用程式嘗試存取資料庫時，EF 會看到沒有任何資料庫，並讓它建立一個，然後執行 seed 方法，來填入資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-314">When you ran the Students page and the application tried to access the database, EF saw that there was no database and so it created one, then it ran the seed method to populate the database with data.</span></span>

<span data-ttu-id="4661d-315">您可以使用**伺服器總管**或是**SQL Server 物件總管**(SSOX)，以在 Visual Studio 中檢視的資料庫。</span><span class="sxs-lookup"><span data-stu-id="4661d-315">You can use either **Server Explorer** or **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span> <span data-ttu-id="4661d-316">您將使用本教學課程**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="4661d-316">For this tutorial you'll use **Server Explorer**.</span></span> <span data-ttu-id="4661d-317">(在 Visual Studio Express 版本早於 2013 年**伺服器總管**稱為**資料庫總管**。)</span><span class="sxs-lookup"><span data-stu-id="4661d-317">(In Visual Studio Express editions earlier than 2013, **Server Explorer** is called **Database Explorer**.)</span></span>

1. <span data-ttu-id="4661d-318">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="4661d-318">Close the browser.</span></span>
2. <span data-ttu-id="4661d-319">中**伺服器總管**，展開**資料連接**，展開**學校內容 (ContosoUniversity)**，然後展開**資料表**查看您新的資料庫中的資料表。</span><span class="sxs-lookup"><span data-stu-id="4661d-319">In **Server Explorer**, expand **Data Connections**, expand **School Context (ContosoUniversity)**, and then expand **Tables** to see the tables in your new database.</span></span>

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. <span data-ttu-id="4661d-320">以滑鼠右鍵按一下**學生**資料表，然後按一下**顯示資料表資料**以查看所建立的資料行和資料列插入至資料表。</span><span class="sxs-lookup"><span data-stu-id="4661d-320">Right-click the **Student** table and click **Show Table Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

    ![Student 資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. <span data-ttu-id="4661d-322">關閉**伺服器總管**連接。</span><span class="sxs-lookup"><span data-stu-id="4661d-322">Close the **Server Explorer** connection.</span></span>

<span data-ttu-id="4661d-323">*ContosoUniversity1.mdf*並 *.ldf*資料庫檔案位於`C:\Users\<yourusername>`資料夾。</span><span class="sxs-lookup"><span data-stu-id="4661d-323">The *ContosoUniversity1.mdf* and *.ldf* database files are in the `C:\Users\<yourusername>` folder.</span></span>

<span data-ttu-id="4661d-324">因為您使用`DropCreateDatabaseIfModelChanges`初始設定式，您可以現在進行的變更`Student`類別，請再次執行應用程式，以及資料庫會自動重新建立以符合您的變更。</span><span class="sxs-lookup"><span data-stu-id="4661d-324">Because you're using the `DropCreateDatabaseIfModelChanges` initializer, you could now make a change to the `Student` class, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="4661d-325">例如，如果您加入`EmailAddress`屬性，以`Student`類別，請再次執行 Students 頁面以及再查看資料表一次，您將會看到新`EmailAddress`資料行。</span><span class="sxs-lookup"><span data-stu-id="4661d-325">For example, if you add an `EmailAddress` property to the `Student` class, run the Students page again, and then look at the table again, you will see a new `EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="4661d-326">慣例</span><span class="sxs-lookup"><span data-stu-id="4661d-326">Conventions</span></span>

<span data-ttu-id="4661d-327">您必須讓 Entity Framework 能夠為您建立完整的資料庫撰寫的程式碼數量非常小，因為使用了*慣例*，可讓 Entity Framework 的假設。</span><span class="sxs-lookup"><span data-stu-id="4661d-327">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of *conventions*, or assumptions that the Entity Framework makes.</span></span> <span data-ttu-id="4661d-328">其中一些已註明，或用來而您知道它們的存在：</span><span class="sxs-lookup"><span data-stu-id="4661d-328">Some of them have already been noted or were used without your being aware of them:</span></span>

- <span data-ttu-id="4661d-329">複數化的形式的實體類別名稱會用於資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="4661d-329">The pluralized forms of entity class names are used as table names.</span></span>
- <span data-ttu-id="4661d-330">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="4661d-330">Entity property names are used for column names.</span></span>
- <span data-ttu-id="4661d-331">名為實體屬性`ID`或是*classname* `ID`被視為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="4661d-331">Entity properties that are named `ID` or *classname* `ID` are recognized as primary key properties.</span></span>
- <span data-ttu-id="4661d-332">是否將它命名，將會解譯為外部索引鍵屬性的屬性*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (比方說，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。</span><span class="sxs-lookup"><span data-stu-id="4661d-332">A property is interpreted as a foreign key property if it's named *&lt;navigation property name&gt;&lt;primary key property name&gt;* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="4661d-333">外部索引鍵屬性也可以命名相同只要&lt;主索引鍵屬性名稱&gt;(例如`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="4661d-333">Foreign key properties can also be named the same simply &lt;primary key property name&gt; (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="4661d-334">您已了解可以覆寫慣例。</span><span class="sxs-lookup"><span data-stu-id="4661d-334">You've seen that conventions can be overridden.</span></span> <span data-ttu-id="4661d-335">例如，指定資料表名稱不應該要複數化，，和更新版本，您會看到如何明確地標示為外部索引鍵屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="4661d-335">For example, you specified that table names shouldn't be pluralized, and you'll see later how to explicitly mark a property as a foreign key property.</span></span> <span data-ttu-id="4661d-336">您將進一步了解慣例和如何覆寫其[建立更多複雜的資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)稍後在本系列教學課程。</span><span class="sxs-lookup"><span data-stu-id="4661d-336">You'll learn more about conventions and how to override them in the [Creating a More Complex Data Model](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial later in this series.</span></span> <span data-ttu-id="4661d-337">如需慣例的詳細資訊，請參閱 <<c0> [ 程式碼的第一個慣例](https://msdn.microsoft.com/data/jj679962)。</span><span class="sxs-lookup"><span data-stu-id="4661d-337">For more information about conventions, see [Code First Conventions](https://msdn.microsoft.com/data/jj679962).</span></span>

## <a name="summary"></a><span data-ttu-id="4661d-338">總結</span><span class="sxs-lookup"><span data-stu-id="4661d-338">Summary</span></span>

<span data-ttu-id="4661d-339">您現在已建立簡單的應用程式使用 Entity Framework 和 SQL Server Express LocalDB 來儲存和顯示資料。</span><span class="sxs-lookup"><span data-stu-id="4661d-339">You've now created a simple application that uses the Entity Framework and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="4661d-340">在下列教學課程中，您將學習如何執行基本的 CRUD （建立、 讀取、 更新、 刪除） 作業。</span><span class="sxs-lookup"><span data-stu-id="4661d-340">In the following tutorial you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="4661d-341">您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="4661d-341">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="4661d-342">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="4661d-342">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="4661d-343">其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="4661d-343">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4661d-344">下一步</span><span class="sxs-lookup"><span data-stu-id="4661d-344">Next</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
