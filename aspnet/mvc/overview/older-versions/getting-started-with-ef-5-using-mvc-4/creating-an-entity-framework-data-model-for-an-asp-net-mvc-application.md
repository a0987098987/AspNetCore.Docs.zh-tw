---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型 |Microsoft 文件
author: tdykstra
description: 此教學課程系列的較新版本可用，Visual Studio 2013、 Entity Framework 6 和 MVC 5。 Contoso 大學範例 web 應用程式 de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877927"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a><span data-ttu-id="b047c-104">建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型</span><span class="sxs-lookup"><span data-stu-id="b047c-104">Creating an Entity Framework Data Model for an ASP.NET MVC Application (1 of 10)</span></span>
====================
<span data-ttu-id="b047c-105">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b047c-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b047c-106">下載完成的專案</span><span class="sxs-lookup"><span data-stu-id="b047c-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > <span data-ttu-id="b047c-107">A[此教學課程系列的新版](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)可用，Visual Studio 2013、 Entity Framework 6，和 MVC 5。</span><span class="sxs-lookup"><span data-stu-id="b047c-107">A [newer version of this tutorial series](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) is available, for Visual Studio 2013, Entity Framework 6, and MVC 5.</span></span>
> 
> 
> <span data-ttu-id="b047c-108">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 和 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="b047c-108">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 and Visual Studio 2012.</span></span> <span data-ttu-id="b047c-109">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="b047c-109">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="b047c-110">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="b047c-110">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="b047c-111">此教學課程說明如何建置 Contoso 大學範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="b047c-111">This tutorial series explains how to build the Contoso University sample application.</span></span> <span data-ttu-id="b047c-112">您可以[下載完成的應用程式](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)。</span><span class="sxs-lookup"><span data-stu-id="b047c-112">You can [download the completed application](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).</span></span>
> 
> ## <a name="code-first"></a><span data-ttu-id="b047c-113">Code First</span><span class="sxs-lookup"><span data-stu-id="b047c-113">Code First</span></span>
> 
> <span data-ttu-id="b047c-114">有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，和*Code First*。</span><span class="sxs-lookup"><span data-stu-id="b047c-114">There are three ways you can work with data in the Entity Framework: *Database First*, *Model First*, and *Code First*.</span></span> <span data-ttu-id="b047c-115">本教學課程適用於第一個程式碼。</span><span class="sxs-lookup"><span data-stu-id="b047c-115">This tutorial is for Code First.</span></span> <span data-ttu-id="b047c-116">如需如何選擇適合您案例的這些工作流程和指引之間差異的詳細資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。</span><span class="sxs-lookup"><span data-stu-id="b047c-116">For information about the differences between these workflows and guidance on how to choose the best one for your scenario, see [Entity Framework Development Workflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>
> 
> ## <a name="mvc"></a><span data-ttu-id="b047c-117">MVC</span><span class="sxs-lookup"><span data-stu-id="b047c-117">MVC</span></span>
> 
> <span data-ttu-id="b047c-118">範例應用程式根據[ASP.NET MVC](../../../index.md)。</span><span class="sxs-lookup"><span data-stu-id="b047c-118">The sample application is built on [ASP.NET MVC](../../../index.md).</span></span> <span data-ttu-id="b047c-119">如果您想要使用 ASP.NET Web Form 模型，請參閱[模型繫結和 Web Form](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md)教學課程系列和[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="b047c-119">If you prefer to work with the ASP.NET Web Forms model, see the [Model Binding and Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) tutorial series and [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="b047c-120">軟體版本</span><span class="sxs-lookup"><span data-stu-id="b047c-120">Software versions</span></span>
> 
> | <span data-ttu-id="b047c-121">**教學課程中所示**</span><span class="sxs-lookup"><span data-stu-id="b047c-121">**Shown in the tutorial**</span></span> | <span data-ttu-id="b047c-122">**也可以搭配**</span><span class="sxs-lookup"><span data-stu-id="b047c-122">**Also works with**</span></span> |
> | --- | --- |
> | <span data-ttu-id="b047c-123">Windows 8</span><span class="sxs-lookup"><span data-stu-id="b047c-123">Windows 8</span></span> | <span data-ttu-id="b047c-124">Windows 7</span><span class="sxs-lookup"><span data-stu-id="b047c-124">Windows 7</span></span> |
> | <span data-ttu-id="b047c-125">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="b047c-125">Visual Studio 2012</span></span> | <span data-ttu-id="b047c-126">Visual Studio 2012 Express for Web.</span><span class="sxs-lookup"><span data-stu-id="b047c-126">Visual Studio 2012 Express for Web.</span></span> <span data-ttu-id="b047c-127">這會自動安裝 Windows Azure SDK 如果您還沒有 VS 2012 或 VS 2012 Express for Web。</span><span class="sxs-lookup"><span data-stu-id="b047c-127">This is automatically installed by the Windows Azure SDK if you don't already have VS 2012 or VS 2012 Express for Web.</span></span> <span data-ttu-id="b047c-128">Visual Studio 2013 應該會運作，但本教學課程尚未經過測試，和某些功能表選取項目和對話方塊不同。</span><span class="sxs-lookup"><span data-stu-id="b047c-128">Visual Studio 2013 should work, but the tutorial has not been tested with it, and some menu selections and dialog boxes are different.</span></span> <span data-ttu-id="b047c-129">[VS 2013 版本的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510)需要 Windows Azure 部署。</span><span class="sxs-lookup"><span data-stu-id="b047c-129">The [VS 2013 version of the Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) is required for Windows Azure deployment.</span></span> |
> | <span data-ttu-id="b047c-130">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b047c-130">.NET 4.5</span></span> | <span data-ttu-id="b047c-131">在.NET 4 中運作的大多數功能顯示，但有些不會。</span><span class="sxs-lookup"><span data-stu-id="b047c-131">Most of the features shown will work in .NET 4, but some won't.</span></span> <span data-ttu-id="b047c-132">例如，在 EF 列舉支援需要.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="b047c-132">For example, enum support in EF requires .NET 4.5.</span></span> |
> | <span data-ttu-id="b047c-133">Entity Framework 5</span><span class="sxs-lookup"><span data-stu-id="b047c-133">Entity Framework 5</span></span> |  |
> | [<span data-ttu-id="b047c-134">Windows Azure SDK 2.1</span><span class="sxs-lookup"><span data-stu-id="b047c-134">Windows Azure SDK 2.1</span></span>](https://go.microsoft.com/fwlink/p/?linkid=323511) | <span data-ttu-id="b047c-135">如果您略過 Windows Azure 的部署步驟，您不需要 SDK。</span><span class="sxs-lookup"><span data-stu-id="b047c-135">If you skip the Windows Azure deployment steps, you don't need the SDK.</span></span> <span data-ttu-id="b047c-136">新的 sdk 版本發行時，連結將會安裝較新版本。</span><span class="sxs-lookup"><span data-stu-id="b047c-136">When a new version of the SDK is released, the link will install the newer version.</span></span> <span data-ttu-id="b047c-137">在此情況下，您可能必須調整一些新的 UI 和功能的指示。</span><span class="sxs-lookup"><span data-stu-id="b047c-137">In that case, you might have to adapt some of the instructions to new UI and features.</span></span> |
> 
> ## <a name="questions"></a><span data-ttu-id="b047c-138">問題</span><span class="sxs-lookup"><span data-stu-id="b047c-138">Questions</span></span>
> 
> <span data-ttu-id="b047c-139">如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)、 [Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。</span><span class="sxs-lookup"><span data-stu-id="b047c-139">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx), the [Entity Framework and LINQ to Entities forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), or [StackOverflow.com](http://stackoverflow.com/).</span></span>
> 
> ## <a name="acknowledgments"></a><span data-ttu-id="b047c-140">感謝</span><span class="sxs-lookup"><span data-stu-id="b047c-140">Acknowledgments</span></span>
> 
> <span data-ttu-id="b047c-141">請參閱中的序列的最後一個教學課程[通知和 VB 的附註](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments)。</span><span class="sxs-lookup"><span data-stu-id="b047c-141">See the last tutorial in the series for [acknowledgments and a note about VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).</span></span>
> 
> ## <a name="original-version-of-the-tutorial"></a><span data-ttu-id="b047c-142">原始版的教學課程</span><span class="sxs-lookup"><span data-stu-id="b047c-142">Original version of the tutorial</span></span>
> 
> <span data-ttu-id="b047c-143">本教學課程的原始版本位於[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)。</span><span class="sxs-lookup"><span data-stu-id="b047c-143">The original version of the tutorial is available in the [EF 4.1 / MVC 3 e-book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).</span></span>


## <a name="the-contoso-university-web-application"></a><span data-ttu-id="b047c-144">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b047c-144">The Contoso University Web Application</span></span>

<span data-ttu-id="b047c-145">您在這些教學課程中會建置的應用程式為一個簡單的大學網站。</span><span class="sxs-lookup"><span data-stu-id="b047c-145">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="b047c-146">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="b047c-146">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="b047c-147">以下是您會建立的幾個畫面。</span><span class="sxs-lookup"><span data-stu-id="b047c-147">Here are a few of the screens you'll create.</span></span>

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="b047c-149">此網站的 UI 樣式與內建範本產生的相當類似，以使此教學課程能聚焦於如何使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="b047c-149">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b047c-150">必要條件</span><span class="sxs-lookup"><span data-stu-id="b047c-150">Prerequisites</span></span>

<span data-ttu-id="b047c-151">方向和螢幕擷取畫面，在本教學課程假設您使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads)或[Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131)、 最新的 [更新] 和 Azure SDK for.NET 安裝為準，年 7 月，2013。</span><span class="sxs-lookup"><span data-stu-id="b047c-151">The directions and screen shots in this tutorial assume that you're using [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) or [Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131), with the latest update and Azure SDK for .NET installed as of July, 2013.</span></span> <span data-ttu-id="b047c-152">您可以取得這些全部都使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="b047c-152">You can get all of this with the following link:</span></span>

[<span data-ttu-id="b047c-153">Azure SDK for.NET (Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="b047c-153">Azure SDK for .NET (Visual Studio 2012)</span></span>](https://go.microsoft.com/fwlink/?LinkId=254364)

<span data-ttu-id="b047c-154">如果您有安裝 Visual Studio 時，上面的連結將會安裝任何遺漏的元件。</span><span class="sxs-lookup"><span data-stu-id="b047c-154">If you have Visual Studio installed, the link above will install any missing components.</span></span> <span data-ttu-id="b047c-155">如果您沒有 Visual Studio，連結將會安裝 Visual Studio 2012 Express for Web。</span><span class="sxs-lookup"><span data-stu-id="b047c-155">If you don't have Visual Studio, the link will install Visual Studio 2012 Express for Web.</span></span> <span data-ttu-id="b047c-156">您可以使用 Visual Studio 2013 中，但某些必要的程序和畫面會不同。</span><span class="sxs-lookup"><span data-stu-id="b047c-156">You can use Visual Studio 2013, but some of the required procedures and screens will differ.</span></span>

## <a name="create-an-mvc-web-application"></a><span data-ttu-id="b047c-157">建立 MVC Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b047c-157">Create an MVC Web Application</span></span>

<span data-ttu-id="b047c-158">開啟 Visual Studio 並建立新 C# 專案名為"ContosoUniversity 」 使用**ASP.NET MVC 4 Web 應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="b047c-158">Open Visual Studio and create a new C# project named "ContosoUniversity" using the **ASP.NET MVC 4 Web Application** template.</span></span> <span data-ttu-id="b047c-159">請確定您的目標 **.NET Framework 4.5** (您將使用[`enum`屬性](https://msdn.microsoft.com/data/hh859576.aspx)，而且這需要.NET 4.5)。</span><span class="sxs-lookup"><span data-stu-id="b047c-159">Make sure you target **.NET Framework 4.5** (you'll be using [`enum` properties](https://msdn.microsoft.com/data/hh859576.aspx), and that requires .NET 4.5).</span></span>

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="b047c-161">在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**範本。</span><span class="sxs-lookup"><span data-stu-id="b047c-161">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template.</span></span>

<span data-ttu-id="b047c-162">保留**Razor**檢視引擎選取，並留下**建立單元測試專案**清除核取方塊。</span><span class="sxs-lookup"><span data-stu-id="b047c-162">Leave the **Razor** view engine selected, and leave the **Create a unit test project** check box cleared.</span></span>

<span data-ttu-id="b047c-163">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="b047c-163">Click **OK**.</span></span>

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="b047c-165">設定站台樣式</span><span class="sxs-lookup"><span data-stu-id="b047c-165">Set Up the Site Style</span></span>

<span data-ttu-id="b047c-166">一些簡單的變更會設定網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="b047c-166">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="b047c-167">開啟 *_layout.cshtml\\_Layout.cshtml*，並以下列程式碼取代檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="b047c-167">Open *Views\Shared\\_Layout.cshtml*, and replace the contents of the file with the following code.</span></span> <span data-ttu-id="b047c-168">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="b047c-168">The changes are highlighted.</span></span>

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

<span data-ttu-id="b047c-169">此程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b047c-169">This code makes the following changes:</span></span>

- <span data-ttu-id="b047c-170">取代"Contoso 大學"「 My ASP.NET MVC 應用程式 」 和 「 您的標誌在此 「 的範本執行個體。</span><span class="sxs-lookup"><span data-stu-id="b047c-170">Replaces the template instances of "My ASP.NET MVC Application" and "your logo here" with "Contoso University".</span></span>
- <span data-ttu-id="b047c-171">新增將稍後在本教學課程中使用的數個動作連結。</span><span class="sxs-lookup"><span data-stu-id="b047c-171">Adds several action links that will be used later in the tutorial.</span></span>

<span data-ttu-id="b047c-172">在*Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼以消除有關 ASP.NET 和 MVC 範本段落：</span><span class="sxs-lookup"><span data-stu-id="b047c-172">In *Views\Home\Index.cshtml*, replace the contents of the file with the following code to eliminate the template paragraphs about ASP.NET and MVC:</span></span>

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

<span data-ttu-id="b047c-173">在*Controllers\HomeController.cs*，變更值`ViewBag.Message`中`Index`要 「 歡迎使用 Contoso 大學 ！"，如下列範例所示的動作方法：</span><span class="sxs-lookup"><span data-stu-id="b047c-173">In *Controllers\HomeController.cs*, change the value for `ViewBag.Message` in the `Index` Action method to "Welcome to Contoso University!", as shown in the following example:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

<span data-ttu-id="b047c-174">按 CTRL + F5 以執行站台。</span><span class="sxs-lookup"><span data-stu-id="b047c-174">Press CTRL+F5 to run the site.</span></span> <span data-ttu-id="b047c-175">您會看到與主功能表的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="b047c-175">You see the home page with the main menu.</span></span>

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a><span data-ttu-id="b047c-177">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="b047c-177">Create the Data Model</span></span>

<span data-ttu-id="b047c-178">接下來您會為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="b047c-178">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="b047c-179">會先處理下列三個實體：</span><span class="sxs-lookup"><span data-stu-id="b047c-179">You'll start with the following three entities:</span></span>

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="b047c-181">在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="b047c-181">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="b047c-182">換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="b047c-182">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="b047c-183">在下節中，您會為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="b047c-183">In the following sections you'll create a class for each one of these entities.</span></span>

> [!NOTE]
> <span data-ttu-id="b047c-184">如果您嘗試編譯專案，才能完成建立所有的這些實體類別時，就會發生編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="b047c-184">If you try to compile the project before you finish creating all of these entity classes, you'll get compiler errors.</span></span>


### <a name="the-student-entity"></a><span data-ttu-id="b047c-185">學生實體</span><span class="sxs-lookup"><span data-stu-id="b047c-185">The Student Entity</span></span>

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="b047c-187">在*模型*資料夾中，建立*Student.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b047c-187">In the *Models* folder, create *Student.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

<span data-ttu-id="b047c-188">`StudentID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="b047c-188">The `StudentID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="b047c-189">根據預設，Entity Framework 會解譯此屬性，名為`ID`或*classname* `ID`為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="b047c-189">By default, the Entity Framework interprets a property that's named `ID` or *classname* `ID` as the primary key.</span></span>

<span data-ttu-id="b047c-190">`Enrollments`屬性是*導覽屬性*。</span><span class="sxs-lookup"><span data-stu-id="b047c-190">The `Enrollments` property is a *navigation property*.</span></span> <span data-ttu-id="b047c-191">導覽屬性會保留與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="b047c-191">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="b047c-192">在此情況下，`Enrollments`屬性`Student`實體會保存所有`Enrollment`的實體有關的`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="b047c-192">In this case, the `Enrollments` property of a `Student` entity will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="b047c-193">換句話說，如果給定`Student`資料庫中的資料列都有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將會包含這兩者`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="b047c-193">In other words, if a given `Student` row in the database has two related `Enrollment` rows (rows that contain that student's primary key value in their `StudentID` foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="b047c-194">導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能例如*消極式載入*。</span><span class="sxs-lookup"><span data-stu-id="b047c-194">Navigation properties are typically defined as `virtual` so that they can take advantage of certain Entity Framework functionality such as *lazy loading*.</span></span> <span data-ttu-id="b047c-195">(將說明消極式載入稍後在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程稍後在本系列。</span><span class="sxs-lookup"><span data-stu-id="b047c-195">(Lazy loading will be explained later, in the [Reading Related Data](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial later in this series.</span></span>

<span data-ttu-id="b047c-196">若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection`。</span><span class="sxs-lookup"><span data-stu-id="b047c-196">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection`.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="b047c-197">註冊實體</span><span class="sxs-lookup"><span data-stu-id="b047c-197">The Enrollment Entity</span></span>

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="b047c-199">在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b047c-199">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

<span data-ttu-id="b047c-200">等級屬性是[列舉](https://msdn.microsoft.com/data/hh859576.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b047c-200">The Grade property is an [enum](https://msdn.microsoft.com/data/hh859576.aspx).</span></span> <span data-ttu-id="b047c-201">問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b047c-201">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://msdn.microsoft.com/library/2cf62fcy.aspx).</span></span> <span data-ttu-id="b047c-202">為 null 的等級是不同於零的等級，null 表示等級不未知或尚未被指派。</span><span class="sxs-lookup"><span data-stu-id="b047c-202">A grade that's null is different from a zero grade — null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="b047c-203">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="b047c-203">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="b047c-204">`Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。</span><span class="sxs-lookup"><span data-stu-id="b047c-204">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="b047c-205">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="b047c-205">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="b047c-206">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="b047c-206">An `Enrollment` entity is associated with one `Course` entity.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="b047c-207">課程實體</span><span class="sxs-lookup"><span data-stu-id="b047c-207">The Course Entity</span></span>

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

<span data-ttu-id="b047c-209">在*模型*資料夾中，建立*Course.cs*，以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b047c-209">In the *Models* folder, create *Course.cs*, replacing the existing code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="b047c-210">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b047c-210">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="b047c-211">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="b047c-211">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="b047c-212">我們將更多有關 [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)。無）] 下一個教學課程中的屬性。</span><span class="sxs-lookup"><span data-stu-id="b047c-212">We'll say more about the [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx).None)] attribute in the next tutorial.</span></span> <span data-ttu-id="b047c-213">基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。</span><span class="sxs-lookup"><span data-stu-id="b047c-213">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="b047c-214">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="b047c-214">Create the Database Context</span></span>

<span data-ttu-id="b047c-215">協調對給定的資料模型的 Entity Framework 功能的主要類別是*資料庫內容*類別。</span><span class="sxs-lookup"><span data-stu-id="b047c-215">The main class that coordinates Entity Framework functionality for a given data model is the *database context* class.</span></span> <span data-ttu-id="b047c-216">您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。</span><span class="sxs-lookup"><span data-stu-id="b047c-216">You create this class by deriving from the [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) class.</span></span> <span data-ttu-id="b047c-217">在您的程式碼中，您會指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="b047c-217">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="b047c-218">您也可以自訂某些 Entity Framework 行為。</span><span class="sxs-lookup"><span data-stu-id="b047c-218">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="b047c-219">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="b047c-219">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="b047c-220">建立名為的資料夾*DAL* （適用於資料存取層）。</span><span class="sxs-lookup"><span data-stu-id="b047c-220">Create a folder named *DAL* (for Data Access Layer).</span></span> <span data-ttu-id="b047c-221">該資料夾中建立新的類別檔案命名為*SchoolContext.cs*，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b047c-221">In that folder create a new class file named *SchoolContext.cs*, and replace the existing code with the following code:</span></span>

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="b047c-222">此程式碼建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。</span><span class="sxs-lookup"><span data-stu-id="b047c-222">This code creates a [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) property for each entity set.</span></span> <span data-ttu-id="b047c-223">在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，和*實體*對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="b047c-223">In Entity Framework terminology, an *entity set* typically corresponds to a database table, and an *entity* corresponds to a row in the table.</span></span>

<span data-ttu-id="b047c-224">`modelBuilder.Conventions.Remove`陳述式中的[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會從正在 pluralized 防止資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="b047c-224">The `modelBuilder.Conventions.Remove` statement in the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) method prevents table names from being pluralized.</span></span> <span data-ttu-id="b047c-225">如果您沒有這麼做，所產生之資料表就會命名為`Students`， `Courses`，和`Enrollments`。</span><span class="sxs-lookup"><span data-stu-id="b047c-225">If you didn't do this, the generated tables would be named `Students`, `Courses`, and `Enrollments`.</span></span> <span data-ttu-id="b047c-226">相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。</span><span class="sxs-lookup"><span data-stu-id="b047c-226">Instead, the table names will be `Student`, `Course`, and `Enrollment`.</span></span> <span data-ttu-id="b047c-227">針對是否要複數化資料表名稱，開發人員並沒有共識。</span><span class="sxs-lookup"><span data-stu-id="b047c-227">Developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="b047c-228">本教學課程使用的單數形式，但是很重要的一點是，您可以選取您想要包含或省略這行程式碼的任何表單。</span><span class="sxs-lookup"><span data-stu-id="b047c-228">This tutorial uses the singular form, but the important point is that you can select whichever form you prefer by including or omitting this line of code.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b047c-229">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b047c-229">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b047c-230">[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版 SQL Server Express Database Engine，視需要啟動並以使用者模式執行。</span><span class="sxs-lookup"><span data-stu-id="b047c-230">[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="b047c-231">LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您能夠使用資料庫，做為 *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-231">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="b047c-232">一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="b047c-232">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span> <span data-ttu-id="b047c-233">在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-233">The user instance feature in SQL Server Express also enables you to work with *.mdf* files, but the user instance feature is deprecated; therefore, LocalDB is recommended for working with *.mdf* files.</span></span>

<span data-ttu-id="b047c-234">一般 SQL Server Express 不用於生產環境 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b047c-234">Typically SQL Server Express is not used for production web applications.</span></span> <span data-ttu-id="b047c-235">LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是使用 IIS。</span><span class="sxs-lookup"><span data-stu-id="b047c-235">LocalDB in particular is not recommended for production use with a web application because it is not designed to work with IIS.</span></span>

<span data-ttu-id="b047c-236">在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。</span><span class="sxs-lookup"><span data-stu-id="b047c-236">In Visual Studio 2012 and later versions, LocalDB is installed by default with Visual Studio.</span></span> <span data-ttu-id="b047c-237">在 Visual Studio 2010 和舊版中，SQL Server Express （而不要使用 LocalDB) 預設會安裝 Visual studio;您必須手動安裝它，如果您使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="b047c-237">In Visual Studio 2010 and earlier versions, SQL Server Express (without LocalDB) is installed by default with Visual Studio; you have to install it manually if you're using Visual Studio 2010.</span></span>

<span data-ttu-id="b047c-238">在此教學課程中您將使用 LocalDB 資料庫可以儲存在*應用程式\_資料*資料夾 *.mdf*檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-238">In this tutorial you'll work with LocalDB so that the database can be stored in the *App\_Data* folder as an *.mdf* file.</span></span> <span data-ttu-id="b047c-239">開啟根*Web.config*檔案，然後加入新的連接字串至`connectionStrings`集合中，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="b047c-239">Open the root *Web.config* file and add a new connection string to the `connectionStrings` collection, as shown in the following example.</span></span> <span data-ttu-id="b047c-240">(請確定您更新*Web.config*根專案資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-240">(Make sure you update the *Web.config* file in the root project folder.</span></span> <span data-ttu-id="b047c-241">另外還有*Web.config*檔案位於*檢視*不需要更新的子資料夾。)</span><span class="sxs-lookup"><span data-stu-id="b047c-241">There's also a *Web.config* file is in the *Views* subfolder that you don't need to update.)</span></span>

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

<span data-ttu-id="b047c-242">根據預設，Entity Framework 會尋找名稱相同的連接字串`DbContext`類別 (`SchoolContext`這個專案)。</span><span class="sxs-lookup"><span data-stu-id="b047c-242">By default, the Entity Framework looks for a connection string named the same as the `DbContext` class (`SchoolContext` for this project).</span></span> <span data-ttu-id="b047c-243">已新增的連接字串會指定名為 LocalDB 資料庫*ContosoUniversity.mdf*位於*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b047c-243">The connection string you've added specifies a LocalDB database named *ContosoUniversity.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="b047c-244">如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b047c-244">For more information, see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="b047c-245">實際上，您不需要指定的連接字串。</span><span class="sxs-lookup"><span data-stu-id="b047c-245">You don't actually need to specify the connection string.</span></span> <span data-ttu-id="b047c-246">如果您並未提供的連接字串，Entity Framework 會建立一個。不過，資料庫可能不會在*應用程式\_資料*應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="b047c-246">If you don't supply a connection string, Entity Framework will create one for you; however, the database might not be in the *App\_data* folder of your app.</span></span> <span data-ttu-id="b047c-247">會建立此資料庫的資訊，請參閱[Code First 到新的資料庫](https://msdn.microsoft.com/data/jj193542)。</span><span class="sxs-lookup"><span data-stu-id="b047c-247">For information on where the database will be created, see [Code First to a New Database](https://msdn.microsoft.com/data/jj193542).</span></span>

<span data-ttu-id="b047c-248">`connectionStrings`集合也具有名為連接字串`DefaultConnection`用的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-248">The `connectionStrings` collection also has a connection string named `DefaultConnection` which is used for the membership database.</span></span> <span data-ttu-id="b047c-249">您將不會在本教學課程使用成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-249">You won't be using the membership database in this tutorial.</span></span> <span data-ttu-id="b047c-250">兩個連接字串之間唯一的差別是資料庫名稱和 name 屬性值。</span><span class="sxs-lookup"><span data-stu-id="b047c-250">The only difference between the two connection strings is the database name and the name attribute value.</span></span>

## <a name="set-up-and-execute-a-code-first-migration"></a><span data-ttu-id="b047c-251">設定及執行程式碼的第一次移轉</span><span class="sxs-lookup"><span data-stu-id="b047c-251">Set up and Execute a Code First Migration</span></span>

<span data-ttu-id="b047c-252">當您第一次開始開發應用程式時，您的資料模型變更常見問題，以及每次將模型變更，它會取得與資料庫同步處理。</span><span class="sxs-lookup"><span data-stu-id="b047c-252">When you first start to develop an application, your data model changes frequently, and each time the model changes it gets out of sync with the database.</span></span> <span data-ttu-id="b047c-253">您可以設定 Entity Framework 自動卸除並重新建立每次變更資料模型資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-253">You can configure the Entity Framework to automatically drop and re-create the database each time you change the data model.</span></span> <span data-ttu-id="b047c-254">因為測試資料是容易重新建立，但您已部署至實際執行環境之後，您通常想要更新資料庫結構描述，但不卸除資料庫，這是不及早在開發中的問題。</span><span class="sxs-lookup"><span data-stu-id="b047c-254">This is not a problem early in development because test data is easily re-created, but after you have deployed to production you usually want to update the database schema without dropping the database.</span></span> <span data-ttu-id="b047c-255">移轉功能可讓程式碼第一次更新資料庫卸除並重新建立它。</span><span class="sxs-lookup"><span data-stu-id="b047c-255">The Migrations feature enables Code First to update the database without dropping and re-creating it.</span></span> <span data-ttu-id="b047c-256">您可能要使用在新專案的開發週期的早期[DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx)卸除、 重新建立，並重新植入資料庫中，每次將模型變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-256">Early in the development cycle of a new project you might want to use [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) to drop, recreate and re-seed the database each time the model changes.</span></span> <span data-ttu-id="b047c-257">您做好準備以部署您的應用程式的其中一個，您可以將轉換的移轉方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-257">One you get ready to deploy your application, you can convert to the migrations approach.</span></span> <span data-ttu-id="b047c-258">此教學課程中只會使用移轉作業。</span><span class="sxs-lookup"><span data-stu-id="b047c-258">For this tutorial you'll only use migrations.</span></span> <span data-ttu-id="b047c-259">如需詳細資訊，請參閱[Code First 移轉](https://msdn.microsoft.com/data/jj591621)和[移轉錄影畫面數列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b047c-259">For more information, see [Code First Migrations](https://msdn.microsoft.com/data/jj591621) and [Migrations Screencast Series](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).</span></span>

### <a name="enable-code-first-migrations"></a><span data-ttu-id="b047c-260">啟用 Code First 移轉</span><span class="sxs-lookup"><span data-stu-id="b047c-260">Enable Code First Migrations</span></span>

1. <span data-ttu-id="b047c-261">從**工具**功能表上，按一下 **程式庫套件管理員**然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="b047c-261">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. <span data-ttu-id="b047c-263">在`PM>`提示字元輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b047c-263">At the `PM>` prompt enter the following command:</span></span>

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![enable-migrations 命令](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    <span data-ttu-id="b047c-265">此命令會建立*移轉*資料夾 ContosoUniversity 專案中，它會將該資料夾中放*configuration.cs 中*設定移轉，您可以編輯的檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-265">This command creates a *Migrations* folder in the ContosoUniversity project, and it puts in that folder a *Configuration.cs* file that you can edit to configure Migrations.</span></span>

    ![Migrations 資料夾](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    <span data-ttu-id="b047c-267">`Configuration`類別包含`Seed`建立資料庫時，每次更新之後的資料模型變更時呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-267">The `Configuration` class includes a `Seed` method that is called when the database is created and every time it is updated after a data model change.</span></span>

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    <span data-ttu-id="b047c-268">目的`Seed`方法是為了讓您將測試資料插入資料庫之後第一個程式碼就會建立或更新它。</span><span class="sxs-lookup"><span data-stu-id="b047c-268">The purpose of this `Seed` method is to enable you to insert test data into the database after Code First creates it or updates it.</span></span>

### <a name="set-up-the-seed-method"></a><span data-ttu-id="b047c-269">設定種子方法</span><span class="sxs-lookup"><span data-stu-id="b047c-269">Set up the Seed Method</span></span>

<span data-ttu-id="b047c-270">[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法執行 Code First 移轉建立資料庫時，以及每次它將資料庫更新為最新的移轉。</span><span class="sxs-lookup"><span data-stu-id="b047c-270">The [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs when Code First Migrations creates the database and every time it updates the database to the latest migration.</span></span> <span data-ttu-id="b047c-271">Seed 方法的目的是讓您能夠將資料插入資料表之前應用程式存取資料庫第一次。</span><span class="sxs-lookup"><span data-stu-id="b047c-271">The purpose of the Seed method is to enable you to insert data into your tables before the application accesses the database for the first time.</span></span>

<span data-ttu-id="b047c-272">在舊版的第一個程式碼中，發行移轉之前，它是很常見的`Seed`方法來插入測試資料，因為資料庫已完全刪除並從頭開始重新建立每一次在開發期間的模型變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-272">In earlier versions of Code First, before Migrations was released, it was common for `Seed` methods to insert test data, because with every model change during development the database had to be completely deleted and re-created from scratch.</span></span> <span data-ttu-id="b047c-273">Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法通常不是必要。</span><span class="sxs-lookup"><span data-stu-id="b047c-273">With Code First Migrations, test data is retained after database changes, so including test data in the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method is typically not necessary.</span></span> <span data-ttu-id="b047c-274">事實上，您不想`Seed`方法插入的測試資料，如果您將使用移轉將資料庫部署到生產環境，因為`Seed`方法會在生產環境中執行。</span><span class="sxs-lookup"><span data-stu-id="b047c-274">In fact, you don't want the `Seed` method to insert test data if you'll be using Migrations to deploy the database to production, because the `Seed` method will run in production.</span></span> <span data-ttu-id="b047c-275">在此情況下您想`Seed`方法，以您想要在生產環境中要插入的資料插入資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-275">In that case you want the `Seed` method to insert into the database only the data that you want to be inserted in production.</span></span> <span data-ttu-id="b047c-276">例如，您可能想要包括在實際的部門名稱的資料庫`Department`資料表在生產環境中可以使用應用程式時。</span><span class="sxs-lookup"><span data-stu-id="b047c-276">For example, you might want the database to include actual department names in the `Department` table when the application becomes available in production.</span></span>

<span data-ttu-id="b047c-277">此教學課程中，您將會使用移轉部署，但您`Seed`方法將還是插入測試資料，以便更輕鬆地查看應用程式的功能而不需要以手動方式插入大量資料的運作方式。</span><span class="sxs-lookup"><span data-stu-id="b047c-277">For this tutorial, you'll be using Migrations for deployment, but your `Seed` method will insert test data anyway in order to make it easier to see how application functionality works without having to manually insert a lot of data.</span></span>

1. <span data-ttu-id="b047c-278">取代內容*configuration.cs 中*以下列程式碼，將測試資料載入至新的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-278">Replace the contents of the *Configuration.cs* file with the following code, which will load test data into the new database.</span></span> 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    <span data-ttu-id="b047c-279">[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法會採用做為輸入參數，為資料庫物件和方法中的程式碼會使用該物件加入資料庫中的新實體。</span><span class="sxs-lookup"><span data-stu-id="b047c-279">The [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method takes the database context object as an input parameter, and the code in the method uses that object to add new entities to the database.</span></span> <span data-ttu-id="b047c-280">每個實體類型，程式碼會建立新實體的集合，將它們加入至適當[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)屬性，然後按一下 儲存至資料庫的變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-280">For each entity type, the code creates a collection of new entities, adds them to the appropriate [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) property, and then saves the changes to the database.</span></span> <span data-ttu-id="b047c-281">不需要呼叫[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法之後每個實體群組，做為執行這項，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b047c-281">It isn't necessary to call the [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) method after each group of entities, as is done here, but doing that helps you locate the source of a problem if an exception occurs while the code is writing to the database.</span></span>

    <span data-ttu-id="b047c-282">某些插入資料的陳述式使用[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，以執行 「 更新插入 」 作業。</span><span class="sxs-lookup"><span data-stu-id="b047c-282">Some of the statements that insert data use the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method to perform an "upsert" operation.</span></span> <span data-ttu-id="b047c-283">因為`Seed`方法執行每個移轉，因為您嘗試新增的資料列，就會有第一個移轉建立資料庫之後插入資料。</span><span class="sxs-lookup"><span data-stu-id="b047c-283">Because the `Seed` method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="b047c-284">「 更新插入 」 作業會防止您嘗試要插入的資料列已經存在，但它會發生的錯誤***會覆寫***資料您測試應用程式時所做的任何變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-284">The "upsert" operation prevents errors that would happen if you try to insert a row that already exists, but it ***overrides*** any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="b047c-285">測試資料表中的資料部分可能不會想才會發生： 在某些情況下測試時變更資料時要您的資料庫更新後要保持的變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-285">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="b047c-286">在此情況下您要執行條件式的 insert 作業： 插入資料列，只有當其不存在。</span><span class="sxs-lookup"><span data-stu-id="b047c-286">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span> <span data-ttu-id="b047c-287">Seed 方法會使用這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-287">The Seed method uses both approaches.</span></span>

    <span data-ttu-id="b047c-288">第一個參數傳遞至[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定用來檢查資料列是否已經存在的屬性。</span><span class="sxs-lookup"><span data-stu-id="b047c-288">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="b047c-289">您提供的測試學生資料`LastName`因為每個清單中的最後一個名稱是唯一的可以針對此用途使用屬性：</span><span class="sxs-lookup"><span data-stu-id="b047c-289">For the test student data that you are providing, the `LastName` property can be used for this purpose since each last name in the list is unique:</span></span>

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    <span data-ttu-id="b047c-290">此程式碼會假設這些姓氏是唯一。</span><span class="sxs-lookup"><span data-stu-id="b047c-290">This code assumes that last names are unique.</span></span> <span data-ttu-id="b047c-291">如果您手動新增一位學生重複最後一個名稱，您會得到下列例外狀況下一次執行移轉。</span><span class="sxs-lookup"><span data-stu-id="b047c-291">If you manually add a student with a duplicate last name, you'll get the following exception the next time you perform a migration.</span></span>

    <span data-ttu-id="b047c-292">序列包含一個以上的項目</span><span class="sxs-lookup"><span data-stu-id="b047c-292">Sequence contains more than one element</span></span>

    <span data-ttu-id="b047c-293">如需有關`AddOrUpdate`方法，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 部落格上。</span><span class="sxs-lookup"><span data-stu-id="b047c-293">For more information about the `AddOrUpdate` method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) on Julie Lerman's blog.</span></span>

    <span data-ttu-id="b047c-294">加入的程式碼`Enrollment`實體不會使用`AddOrUpdate`方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-294">The code that adds `Enrollment` entities doesn't use the `AddOrUpdate` method.</span></span> <span data-ttu-id="b047c-295">它會檢查是否實體已經存在，並將實體，如果不存在。</span><span class="sxs-lookup"><span data-stu-id="b047c-295">It checks if an entity already exists and inserts the entity if it doesn't exist.</span></span> <span data-ttu-id="b047c-296">這種方法將會保留您對註冊等級執行移轉時變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-296">This approach will preserve changes you make to an enrollment grade when migrations run.</span></span> <span data-ttu-id="b047c-297">此程式碼迴圈的每個成員`Enrollment`[清單](https://msdn.microsoft.com/library/6sh2ey19.aspx)，如果資料庫中找不到註冊，它會將註冊加入資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-297">The code loops through each member of the `Enrollment`[List](https://msdn.microsoft.com/library/6sh2ey19.aspx) and if the enrollment is not found in the database, it adds the enrollment to the database.</span></span> <span data-ttu-id="b047c-298">第一次更新資料庫，資料庫將是空的因此它會將加入每個註冊。</span><span class="sxs-lookup"><span data-stu-id="b047c-298">The first time you update the database, the database will be empty, so it will add each enrollment.</span></span>

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    <span data-ttu-id="b047c-299">如需有關如何偵錯資訊`Seed`方法以及如何處理重複的資料，例如兩個學生名為"Alexander Carson"，請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson 部落格上。</span><span class="sxs-lookup"><span data-stu-id="b047c-299">For information about how to debug the `Seed` method and how to handle redundant data such as two students named "Alexander Carson", see [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) on Rick Anderson's blog.</span></span>
2. <span data-ttu-id="b047c-300">建置專案。</span><span class="sxs-lookup"><span data-stu-id="b047c-300">Build the project.</span></span>

### <a name="create-and-execute-the-first-migration"></a><span data-ttu-id="b047c-301">建立和執行第一次移轉</span><span class="sxs-lookup"><span data-stu-id="b047c-301">Create and Execute the First Migration</span></span>

1. <span data-ttu-id="b047c-302">在 [封裝管理員主控台] 視窗中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="b047c-302">In the Package Manager Console window, enter the following commands:</span></span> 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    <span data-ttu-id="b047c-303">`add-migration`命令會新增至 Migrations 資料夾 *[DateStamp]\_InitialCreate.cs*包含程式碼會建立資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-303">The `add-migration` command adds to the Migrations folder a *[DateStamp]\_InitialCreate.cs* file that contains code which creates the database.</span></span> <span data-ttu-id="b047c-304">第一個參數 (`InitialCreate)`用於檔案名稱，而且可以是您所要的任何; 您通常會選擇的單字或片語來摘要列出所要完成移轉中的作業。</span><span class="sxs-lookup"><span data-stu-id="b047c-304">The first parameter (`InitialCreate)` is used for the file name and can be whatever you want; you typically choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="b047c-305">例如，您可能會命名稍後移轉&quot;AddDepartmentTable&quot;。</span><span class="sxs-lookup"><span data-stu-id="b047c-305">For example, you might name a later migration &quot;AddDepartmentTable&quot;.</span></span>

    ![初始移轉 migrations 資料夾](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    <span data-ttu-id="b047c-307">`Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表和`Down`方法會刪除它們。</span><span class="sxs-lookup"><span data-stu-id="b047c-307">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them.</span></span> <span data-ttu-id="b047c-308">Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="b047c-308">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="b047c-309">當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-309">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span> <span data-ttu-id="b047c-310">下列程式碼顯示的內容`InitialCreate`檔案：</span><span class="sxs-lookup"><span data-stu-id="b047c-310">The following code shows the contents of the `InitialCreate` file:</span></span>

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    <span data-ttu-id="b047c-311">`update-database`命令執行`Up`方法來建立資料庫然後執行`Seed`方法來擴展資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-311">The `update-database` command runs the `Up` method to create the database and then it runs the `Seed` method to populate the database.</span></span>

<span data-ttu-id="b047c-312">SQL Server 資料庫現在已建立資料模型。</span><span class="sxs-lookup"><span data-stu-id="b047c-312">A SQL Server database has now been created for your data model.</span></span> <span data-ttu-id="b047c-313">資料庫的名稱是*ContosoUniversity*，而 *.mdf*檔案位於您的專案*應用程式\_資料*資料夾因為這是您在中指定您連接字串。</span><span class="sxs-lookup"><span data-stu-id="b047c-313">The name of the database is *ContosoUniversity*, and the *.mdf* file is in your project's *App\_Data* folder because that's what you specified in your connection string.</span></span>

<span data-ttu-id="b047c-314">您可以使用**伺服器總管**或**SQL Server 物件總管**(SSOX) 在 Visual Studio 中檢視的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b047c-314">You can use either **Server Explorer** or **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span> <span data-ttu-id="b047c-315">此教學課程中，您將使用**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="b047c-315">For this tutorial you'll use **Server Explorer**.</span></span> <span data-ttu-id="b047c-316">在 Visual Studio Express 2012 for Web，**伺服器總管**稱為**資料庫總管**。</span><span class="sxs-lookup"><span data-stu-id="b047c-316">In Visual Studio Express 2012 for Web, **Server Explorer** is called **Database Explorer**.</span></span>

1. <span data-ttu-id="b047c-317">從**檢視**功能表上，按一下 **伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="b047c-317">From the **View** menu, click **Server Explorer**.</span></span>
2. <span data-ttu-id="b047c-318">按一下**加入連接**圖示。</span><span class="sxs-lookup"><span data-stu-id="b047c-318">Click the **Add Connection** icon.</span></span>

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. <span data-ttu-id="b047c-319">如果您收到提示**選擇資料來源** 對話方塊中，按一下  **Microsoft SQL Server**，然後按一下 **繼續**。</span><span class="sxs-lookup"><span data-stu-id="b047c-319">If you are prompted with the **Choose Data Source** dialog, click **Microsoft SQL Server**, and then click **Continue**.</span></span>  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. <span data-ttu-id="b047c-320">在**加入連接**對話方塊方塊中，輸入 **(localdb) \v11.0**如**伺服器名稱**。</span><span class="sxs-lookup"><span data-stu-id="b047c-320">In the **Add Connection** dialog box, enter **(localdb)\v11.0** for the **Server Name**.</span></span> <span data-ttu-id="b047c-321">在下**選取或輸入資料庫名稱**，選取**ContosoUniversity。**</span><span class="sxs-lookup"><span data-stu-id="b047c-321">Under **Select or enter a database name**, select **ContosoUniversity.**</span></span>  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. <span data-ttu-id="b047c-322">按一下 [確定] </span><span class="sxs-lookup"><span data-stu-id="b047c-322">Click **OK.**</span></span>
6. <span data-ttu-id="b047c-323">展開**SchoolContext** ，然後展開 **資料表**。</span><span class="sxs-lookup"><span data-stu-id="b047c-323">Expand **SchoolContext** and then expand **Tables**.</span></span>  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. <span data-ttu-id="b047c-324">以滑鼠右鍵按一下**學生**資料表，並按一下**顯示資料表資料**以查看所建立的資料行和資料列插入資料表。</span><span class="sxs-lookup"><span data-stu-id="b047c-324">Right-click the **Student** table and click **Show Table Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

    ![學生資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a><span data-ttu-id="b047c-326">建立學生控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="b047c-326">Creating a Student Controller and Views</span></span>

<span data-ttu-id="b047c-327">下一個步驟是在您可以使用其中一個資料表的應用程式中建立 ASP.NET MVC 控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="b047c-327">The next step is to create an ASP.NET MVC controller and views in your application that can work with one of these tables.</span></span>

1. <span data-ttu-id="b047c-328">若要建立`Student`控制站，以滑鼠右鍵按一下**控制站**資料夾中的**方案總管] 中**，選取**新增**，然後按一下 [**控制器**.</span><span class="sxs-lookup"><span data-stu-id="b047c-328">To create a `Student` controller, right-click the **Controllers** folder in **Solution Explorer**, select **Add**, and then click **Controller**.</span></span> <span data-ttu-id="b047c-329">在**加入控制器**對話方塊方塊中，請選取下列項目，然後按一下**新增**:</span><span class="sxs-lookup"><span data-stu-id="b047c-329">In the **Add Controller** dialog box, make the following selections and then click **Add**:</span></span> 

   - <span data-ttu-id="b047c-330">控制器名稱： **StudentController**。</span><span class="sxs-lookup"><span data-stu-id="b047c-330">Controller name: **StudentController**.</span></span>
   - <span data-ttu-id="b047c-331">範本：**具有讀取/寫入動作和檢視表、 使用 Entity Framework 的 MVC 控制器**。</span><span class="sxs-lookup"><span data-stu-id="b047c-331">Template: **MVC controller with read/write actions and views, using Entity Framework**.</span></span>
   - <span data-ttu-id="b047c-332">模型類別：**學生 (ContosoUniversity.Models)**。</span><span class="sxs-lookup"><span data-stu-id="b047c-332">Model class: **Student (ContosoUniversity.Models)**.</span></span> <span data-ttu-id="b047c-333">（如果您沒有看到下拉式清單中的這個選項，建置專案並再試一次）。</span><span class="sxs-lookup"><span data-stu-id="b047c-333">(If you don't see this option in the drop-down list, build the project and try again.)</span></span>
   - <span data-ttu-id="b047c-334">資料內容類別： **SchoolContext (ContosoUniversity.Models)**。</span><span class="sxs-lookup"><span data-stu-id="b047c-334">Data context class: **SchoolContext (ContosoUniversity.Models)**.</span></span>
   - <span data-ttu-id="b047c-335">檢視： **Razor (CSHTML)**。</span><span class="sxs-lookup"><span data-stu-id="b047c-335">Views: **Razor (CSHTML)**.</span></span> <span data-ttu-id="b047c-336">（預設值。）</span><span class="sxs-lookup"><span data-stu-id="b047c-336">(The default.)</span></span>

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. <span data-ttu-id="b047c-338">Visual Studio 隨即開啟*Controllers\StudentController.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="b047c-338">Visual Studio opens the *Controllers\StudentController.cs* file.</span></span> <span data-ttu-id="b047c-339">您會看到已建立的資料庫內容物件具現化類別變數：</span><span class="sxs-lookup"><span data-stu-id="b047c-339">You see a class variable has been created that instantiates a database context object:</span></span>

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     <span data-ttu-id="b047c-340">`Index`動作方法會取得一份學生*學生*實體集藉由讀取`Students`的資料庫內容執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="b047c-340">The `Index` action method gets a list of students from the *Students* entity set by reading the `Students` property of the database context instance:</span></span>

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     <span data-ttu-id="b047c-341">*Student\Index.cshtml*檢視會顯示在資料表中的這份清單：</span><span class="sxs-lookup"><span data-stu-id="b047c-341">The *Student\Index.cshtml* view displays this list in a table:</span></span>

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. <span data-ttu-id="b047c-342">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="b047c-342">Press CTRL+F5 to run the project.</span></span>

     <span data-ttu-id="b047c-343">按一下**學生**索引標籤，查看測試資料的`Seed`插入的方法。</span><span class="sxs-lookup"><span data-stu-id="b047c-343">Click the **Students** tab to see the test data that the `Seed` method inserted.</span></span>

     ![學生索引頁](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a><span data-ttu-id="b047c-345">慣例</span><span class="sxs-lookup"><span data-stu-id="b047c-345">Conventions</span></span>

<span data-ttu-id="b047c-346">您必須撰寫 Entity framework 可以為您建立完整的資料庫中的程式碼數量是因為使用了最小*慣例*，或讓 Entity Framework 的假設。</span><span class="sxs-lookup"><span data-stu-id="b047c-346">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of *conventions*, or assumptions that the Entity Framework makes.</span></span> <span data-ttu-id="b047c-347">其中部分已經被記：</span><span class="sxs-lookup"><span data-stu-id="b047c-347">Some of them have already been noted:</span></span>

- <span data-ttu-id="b047c-348">Pluralized 的形式的實體類別名稱做為資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="b047c-348">The pluralized forms of entity class names are used as table names.</span></span>
- <span data-ttu-id="b047c-349">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="b047c-349">Entity property names are used for column names.</span></span>
- <span data-ttu-id="b047c-350">實體屬性是名為`ID`或*classname* `ID`被當做主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="b047c-350">Entity properties that are named `ID` or *classname* `ID` are recognized as primary key properties.</span></span>

<span data-ttu-id="b047c-351">您已看過可覆寫慣例 （例如，您指定資料表名稱不應該 pluralized），您將學習更多關於慣例及如何覆寫在[建立多個複雜資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)教學課程稍後在本系列。</span><span class="sxs-lookup"><span data-stu-id="b047c-351">You've seen that conventions can be overridden (for example, you specified that table names shouldn't be pluralized), and you'll learn more about conventions and how to override them in the [Creating a More Complex Data Model](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial later in this series.</span></span> <span data-ttu-id="b047c-352">如需詳細資訊，請參閱[程式碼優先 」 慣例](https://msdn.microsoft.com/data/jj679962)。</span><span class="sxs-lookup"><span data-stu-id="b047c-352">For more information, see [Code First Conventions](https://msdn.microsoft.com/data/jj679962).</span></span>

## <a name="summary"></a><span data-ttu-id="b047c-353">總結</span><span class="sxs-lookup"><span data-stu-id="b047c-353">Summary</span></span>

<span data-ttu-id="b047c-354">現在您已建立簡單的應用程式使用 Entity Framework 和 SQL Server Express 來儲存和顯示資料。</span><span class="sxs-lookup"><span data-stu-id="b047c-354">You've now created a simple application that uses the Entity Framework and SQL Server Express to store and display data.</span></span> <span data-ttu-id="b047c-355">在下列的教學課程中，您將學習如何執行基本 CRUD （建立、 讀取、 更新、 刪除） 作業。</span><span class="sxs-lookup"><span data-stu-id="b047c-355">In the following tutorial you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span> <span data-ttu-id="b047c-356">您可以離開此頁面底部的意見反應。</span><span class="sxs-lookup"><span data-stu-id="b047c-356">You can leave feedback at the bottom of this page.</span></span> <span data-ttu-id="b047c-357">請讓我們知道您所喜歡的教學課程的這個部分的方式，以及我們如何改善它。</span><span class="sxs-lookup"><span data-stu-id="b047c-357">Please let us know how you liked this portion of the tutorial and how we could improve it.</span></span>

<span data-ttu-id="b047c-358">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="b047c-358">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b047c-359">下一步</span><span class="sxs-lookup"><span data-stu-id="b047c-359">Next</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
